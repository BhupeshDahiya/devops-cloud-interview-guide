# Question 

Where do you keep secrets for a Jenkins pipline/Github actions pipeline?

# For Jenkins 

### 1. Jenkins Credentials Provider (Built-in)
This is the most common method. You store secrets globally or within a specific folder in the Jenkins UI.

- How it works: You go to `Manage Jenkins > Credentials`, add your secret, and give it a unique ID.

- Pipeline Usage: You use the `withCredentials` wrapper or the `credentials()` helper. Jenkins will automatically "mask" the output in the console logs (e.g., it will show **** instead of your password).

### 2. External Secret Managers (Best Practice)
For high-security or large-scale environments, teams avoid storing secrets in Jenkins itself and instead fetch them from dedicated vaults. This centralizes security.

- `HashiCorp Vault`: The industry standard. You use the Jenkins Vault plugin to authenticate and pull secrets just-in-time.

- `AWS Secrets Manager / Azure Key Vault`: If your infrastructure is cloud-native, the pipeline can authenticate via IAM roles to grab secrets.

## To Inject the keys

### The AWS Secrets Manager / Parameter Store Plugin (Recommended)

This is the cleanest approach. You store your secret in AWS, and Jenkins fetches it as needed during the build.

- Store the Secret: In AWS, create a secret in Secrets Manager. You will select a KMS Key to encrypt this secret.

- IAM Role: Ensure the Jenkins server (or the node running the job) has an IAM Role with `kms:Decrypt` and `secretsmanager:GetSecretValue` permissions.
  > IAM role shouldn't just have kms:Decrypt, but should be restricted to the Resource ARN of that specific key)

- Jenkins Plugin: Install the AWS Secrets Manager Credentials Provider plugin.

- Usage: The plugin allows Jenkins to see AWS secrets as if they were local Jenkins credentials.

# For Github Actions

### 1. GitHub Actions Secrets (Encrypted)
GitHub provides a built-in secret store that encrypts your sensitive data at rest. Once a secret is saved, it cannot be viewed or edited—it can only be updated or deleted.

- Repository Secrets: Available to all workflows in a specific repository.

- Environment Secrets: (Recommended for Production) These are tied to a specific "Environment" (e.g., prod, staging). You can add protection rules, like requiring an approval from a lead before the secret can be accessed by the runner.

- Organization Secrets: Shared across multiple repositories within an organization.

How to add them:
`Go to Settings > Secrets and variables > Actions > New repository secret.`

### 2. The Modern Way: OIDC + IAM Roles (Best Practice)
Instead of putting AWS Access Keys into GitHub Secrets, you create a "Trust Relationship" in AWS. GitHub identifies itself to AWS, and AWS gives the runner a temporary, short-lived permission to use your KMS key.

- Setup: Create an OIDC provider in AWS IAM for GitHub.

- Workflow: Use the `aws-actions/configure-aws-credentials` action.

- KMS Usage: Once authenticated, you can run aws kms decrypt or use the KMS key to fetch secrets from AWS Secrets Manager.

### To Inject the keys

## OICD way

Step 1: Establish the OIDC Trust (AWS Side)
- Before the pipeline runs, AWS must be configured to trust GitHub.

- Identity Provider: Create an OIDC provider in IAM for `token.actions.githubusercontent.com`.

- IAM Role: Create a role with a Trust Policy that allows your specific GitHub repo to assume it.

# Notes

- Using AWS Secrets Manager or Vault allows for automatic rotation of credentials without needing to update the Jenkinsfile or GitHub repository settings.
- Always mention Least Privilege


- KMS Policy: Ensure this IAM role has `kms:Decrypt` (and `kms:GenerateDataKey` if needed) permissions in the KMS Key Policy.

Step 2: The GitHub Actions Workflow
In your `.github/workflows/deploy.yml`, you will use the official AWS actions to authenticate and then interact with KMS.
