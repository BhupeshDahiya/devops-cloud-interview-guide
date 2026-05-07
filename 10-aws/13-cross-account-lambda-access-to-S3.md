# 🔹 How Can an AWS Lambda Function in One AWS Account Access an S3 Bucket in Another AWS Account?

Cross-account access between Lambda and S3 is mainly achieved using IAM roles and S3 bucket policies. The S3 bucket policy must explicitly trust the IAM role from the other AWS account.

---

## ⭐ Architecture

**Example:**  
- Account A: AWS Lambda function  
- Account B: Amazon S3 bucket  

**Goal:** Lambda in Account A should read/write objects in S3 bucket in Account B

---

## ⭐ Step 1: Configure Lambda Execution Role (Account A)

- Grant Lambda execution role permissions to access the target S3 bucket  
- Example permissions: s3:GetObject, s3:PutObject  
- Resource: target bucket ARN  

**Important:** Even though the bucket is in another account, permissions are defined normally in IAM.

---

## ⭐ Step 2: Update S3 Bucket Policy (Most Important)

- In Account B, update the S3 bucket policy to trust the Lambda execution role from Account A  
- The bucket policy must explicitly allow the IAM role ARN from Account A

---

## ⭐ Why Both Sides Matter

- IAM role must allow access  
- Bucket policy must trust that role  

Cross-account access works only when both sides allow it.

---

## ⭐ Security Best Practices

- Grant access only to specific actions and prefixes instead of full bucket access  
- Example prefixes: /uploads/*, /reports/*

---

## ⭐ Common Troubleshooting Steps

If access fails, verify:  
- IAM role permissions  
- Bucket policy  
- Object encryption (KMS)  
- Region configuration  
- Explicit deny policies

---

## ⭐ Advanced Point

- If the bucket uses KMS encryption, the Lambda role also needs permission to use the KMS key

---

## ⭐ Final Polished Answer

1. Attach required S3 permissions to the Lambda execution role in Account A  
2. Update S3 bucket policy in Account B to explicitly trust the IAM role ARN from Account A  
3. Ensure both IAM role permissions and bucket policy allow the operation  
4. If KMS encryption is used, grant Lambda role access to the KMS key

---

## ⚠️ Important Clarification

- Do not grant access directly to the Lambda function ARN  
- Grant access to the Lambda execution IAM role ARN instead

---

## 🚀 Tough Follow-Ups

- **Why isn’t IAM policy alone enough?**  
  S3 bucket policies are resource-based and must explicitly trust cross-account principals  

- **Can bucket ACLs be used?**  
  Possible, but bucket policies are preferred  

- **What if access is denied?**  
  Check explicit deny policies, SCPs, KMS permissions, object ownership  

- **Can Lambda assume a role in another account instead?**  
  Yes, using STS AssumeRole for controlled cross-account access

---

## 🚀 Ultra Short Revision

- Lambda role gets S3 permissions  
- Bucket policy trusts role ARN  
- Both sides must allow  
- Check KMS if encrypted  
- Troubleshoot denies/SCPs/KMS
