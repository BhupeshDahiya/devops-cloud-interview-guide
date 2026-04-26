## terraformvars.tf vs variables.tf

#### 1. variables.tf (The Definition)
TThis is where you declare variables.

- Defines what variables exist
- Can include type, default values, descriptions
- Acts like a schema

Example Syntax:
```bash
variable "instance_count" {
  type        = number
  description = "The number of EC2 instances to spin up"
  default     = 1  # If no value is provided elsewhere, use 1
}
```
#### 2. terraform.tfvars (The Assignment)
TThis is where you assign values to variables.

- Provides actual values
- Environment-specific (dev, staging, prod)
- No variable blocks—just key-value pairs

Example Syntax:
```bash
instance_count = 5
```

#### How they work together 

- variables.tf defines inputs
- tfvars supplies values
- Terraform combines them during execution

#### Pro Tip: The Loading Order
Terraform is pretty smart about how it looks for values. If you have a variable defined, it looks for the value in this specific order of priority (last one wins):

- Environment variables (e.g., TF_VAR_instance_count)
- The terraform.tfvars file
- Any *.auto.tfvars files
- The -var or -var-file flags used in the command line (highest priority)

> In short: Use variables.tf to tell Terraform what variables exist, and use terraform.tfvars to tell Terraform what those variables should actually be for your current run.

## Resource vs Data source

#### 1. Resource (resource)
A resource is something Terraform creates, updates, or deletes.

- Represents real infrastructure (VMs, buckets, networks, etc.)
- Managed by Terraform state
- Declared with resource block

Example: Creating a brand new S3 bucket or a Virtual Machine.

```bash
resource "aws_s3_bucket" "my_new_bucket" {
  bucket = "my-unique-app-data-2026"
}
```
#### 2. Data Source (data)
A data source is used to fetch or look up information about something that already exists.

- Read-only (Terraform does NOT manage it)
- Useful for referencing external or pre-existing resources
- Declared with data block

Example: Looking up the the latest Amazon Linux AMI ID, but dont create anything.

```bash
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["amazon"]
}
```

#### 🧠 How they work together

You often combine them:
```bash
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["amazon"]
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.latest.id
  instance_type = "t2.micro"
}
```
> I use Data Sources to gather 'context' about the environment I'm deploying into. For example, I'll use a data source to fetch the ID of a shared Corporate VPC. Then, I'll use a Resource block to create a new EC2 instance inside that VPC. This keeps my code flexible—I don't have to hard-code IDs; I let Terraform look them up dynamically.
