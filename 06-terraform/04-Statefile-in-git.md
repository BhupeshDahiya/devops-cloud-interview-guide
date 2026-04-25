# Question
Have you considered storing statefile in Git instead of AWS S3 or Azure Blob ?

# Answer
No, we never store statefiles in Git, state belongs in a Remote Backend with Locking capabilities, like AWS S3 with DynamoDB or Terraform Cloud.

### Technical Justification

- **A. Security & Sensitive Data (The Biggest Risk)**
Terraform statefiles often contain plain-text secrets. If your code creates a database or a service principal, the initial passwords and keys are stored in the JSON statefile. Storing that in Git—even a private repo—means those secrets are now in the version history forever. If a developer's machine is compromised or the repo is leaked, your entire infrastructure is exposed.

- **B. The Lack of State Locking** 
Git is designed for merging code, not for real-time state management. If two DevOps engineers run terraform apply at the same time, Git has no mechanism to lock the state. This leads to race conditions where one person’s changes overwrite the other’s, or worse, the statefile becomes corrupted, leaving the infrastructure in an inconsistent state.

- **C. State Corruption & Merge Conflicts** 
Statefiles are large, complex JSON blobs. If you store them in Git, you will eventually hit a merge conflict. Manually resolving a merge conflict in a statefile is extremely dangerous; one wrong bracket and Terraform can no longer track your resources, potentially leading to accidental deletion of production assets.

> I configure a Remote Backend. For example, in AWS, I use an S3 bucket with Versioning enabled (to recover from accidental state loss) and a DynamoDB table for state locking. This ensures that only one person or CI/CD pipeline can modify the infrastructure at a time, keeping the 'Source of Truth' secure and synchronized.

### Two DevOps Engineers attempts to update statefile at once. What happens ?

**With state locking**
#### 1. Engineer A runs apply
- Acquires the state lock
- Applies the change
- Updates the state
- Releases the lock
#### 2. Engineer B runs apply (at the same time)
- Cannot acquire the lock
- Either:
  - Waits (default behavior), or
  - Fails immediately (if configured)
#### 3.After Engineer A finishes
- Engineer B’s run proceeds
- Terraform refreshes state
Sees that the change is already applied and gets Output:

`“No changes. Infrastructure is up-to-date.”`

> With locking, the first apply acquires the lock and performs the change. The second apply waits, then runs after the lock is released, refreshes the state, and sees no changes are needed. Without locking, both runs may attempt the same operation, which can lead to errors like duplicate resource creation or inconsistent state, even if the intended change is identical.

### Handling "Stuck" Locks
Sometimes, if your internet crashes or your CI/CD pipeline is killed mid-run, the lock stays in DynamoDB even though no one is actually working. Terraform will refuse to run until the lock is cleared.

The Fix:
First, get the Lock ID from the error message, then run:

```Bash
terraform force-unlock <LOCK_ID>
```
