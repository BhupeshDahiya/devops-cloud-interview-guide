## terraformvars.tf vs variables.tf

#### 1. variables.tf (The Definition)
This file is for declarations. You are telling Terraform: "I plan to use a variable called instance_type, and it should be a string."

- Purpose: Defines the variable name, type, description, and (optional) default value.

- Version Control: Always commit this to Git.

- Logic: It establishes the "schema" of your infrastructure.

Example Syntax:
```bash
variable "instance_count" {
  type        = number
  description = "The number of EC2 instances to spin up"
  default     = 1  # If no value is provided elsewhere, use 1
}
```
#### 2. terraform.tfvars (The Assignment)
This file is for setting values. You are telling Terraform: "For this specific deployment, I want instance_count to be 5."

- Purpose: Assigns actual data to the variables you defined in variables.tf.

- Version Control: Usually ignored (added to .gitignore) if it contains sensitive data like API keys or environment-specific IP addresses.

- Logic: It allows you to use the same code (the .tf files) for different environments (Dev, Staging, Prod) just by swapping the .tfvars file.

Example Syntax:
```bash
instance_count = 5
```

#### Pro Tip: The Loading Order
Terraform is pretty smart about how it looks for values. If you have a variable defined, it looks for the value in this specific order of priority (last one wins):

- Environment variables (e.g., TF_VAR_instance_count)
- The terraform.tfvars file
- Any *.auto.tfvars files
- The -var or -var-file flags used in the command line (highest priority)

> In short: Use variables.tf to tell Terraform what variables exist, and use terraform.tfvars to tell Terraform what those variables should actually be for your current run.
