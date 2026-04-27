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


### Why DynamoDB is Preferred for state locks instead of all ther db's(rds,aurora)?

#### DynamoDB is preferred because it gives you simple, atomic locking with high availability and zero maintenance, while RDS/Aurora are heavier, more complex, and not optimized for this kind of lightweight coordination task. Earlier we needed DynamoDB because S3 couldn’t handle locking, but now Terraform added S3 native locking using .tflock files.So for new projects, we can skip DynamoDB — just make sure everyone is on Terraform 1.10+.

#### S3 locking
Terraform added native S3 locking using a lock file:

- Enable with: use_lockfile = true
- Terraform creates a .tflock file in S3
- Uses atomic/conditional writes to ensure only one writer

#### Benefits of DynamoDB locking
- Highly available and durable
- Serverless and Zero Maintenance: Unlike RDS or Aurora, which require managing instances, clusters, or complex scaling policies, DynamoDB is fully serverless. It requires no infrastructure provisioning, making it ideal for the simple key-value task of "locking".
- Cost Efficiency: DynamoDB offers a Free Tier and an on-demand pricing model where you only pay for the exact number of requests made. For state locking—which involves infrequent read/write operations—this is significantly cheaper than maintaining a running RDS instance.
- High Performance at Scale: DynamoDB provides single-digit millisecond latency for the simple "get" and "put" operations needed to acquire or release a lock, regardless of how many state files you manage.
- Unified Security: Security for DynamoDB is managed entirely through AWS IAM. RDS often requires managing separate database-level credentials (SQL GRANTS), which adds unnecessary complexity to automated CI/CD pipelines. 