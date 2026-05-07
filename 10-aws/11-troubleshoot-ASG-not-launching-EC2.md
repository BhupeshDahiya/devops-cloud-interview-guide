# 🔹 Auto Scaling Group is Not Launching EC2 Instances — How Would You Troubleshoot?

If an Auto Scaling Group suddenly stops launching EC2 instances, I would approach it systematically by checking configuration, capacity, permissions, and scaling activity logs rather than assuming a single root cause.

---

## ⭐ Step 1: Check Auto Scaling Activity History

> “My first step would be checking the Auto Scaling activity history and events.”

**Tools:**  
- AWS Auto Scaling console  
- CloudTrail if required  

**Look for:**  
- Launch failures  
- Insufficient capacity  
- Invalid AMI  
- IAM permission failures  

---

## ⭐ Step 2: Verify Launch Template / Launch Configuration

> “Then I would validate the launch template attached to the Auto Scaling Group.”

**Check:**  
- AMI ID validity  
- Instance type validity  
- Key pair  
- Security groups  
- User data script  
- IAM instance profile  

> **Real-world tip:** AMIs may be deprecated or deleted if teams rotate golden images regularly.

---

## ⭐ Step 3: Check Subnets & Availability Zones

> “Next, I would verify subnet configuration and AZ health.”

**Check:**  
- Correct subnet association  
- Available IP addresses in subnet  
- AZ outages or insufficient capacity  

---

## ⭐ Step 4: Check EC2 Capacity / Quotas

> “I would verify whether the account is hitting EC2 service quotas or instance capacity limits.”

**Examples:**  
- vCPU quota reached  
- Specific instance family unavailable  
- Spot capacity exhaustion  

> Common with GPU instances, Spot fleets, or large burst scaling.

---

## ⭐ Step 5: Check IAM Permissions

> “I would verify whether the Auto Scaling service role and instance profile permissions are intact.”

**Key permissions to validate:**  
- `ec2:RunInstances`  
- `iam:PassRole`  

> Realistic scenario: Someone modifies IAM policies causing launch failures.

---

## ⭐ Step 6: Check User Data / Bootstrapping Failures

> “If instances launch and terminate immediately, I would inspect bootstrapping or user-data execution failures.”

**Check logs:**  
- `/var/log/cloud-init.log`  
- `/var/log/user-data.log`  

**Look for:**  
- Package installation failures  
- Failed application initialization  

> ASG may continuously replace unhealthy instances.

---

## ⭐ Step 7: Check Health Checks

> “I would verify whether instances are failing ALB or EC2 health checks.”

**Possible issues:**  
- Application not listening on expected port  
- Target group health check misconfiguration  

> Result: ASG launches → marks unhealthy → terminates repeatedly

---

## ⭐ Real Production Example

In one case, new instances launched successfully but immediately failed health checks because the latest AMI had an outdated startup script. The Auto Scaling Group kept terminating and recreating instances. We identified it through scaling activity logs and fixed the launch template.

---

## ⭐ Final Polished Answer

I would start by checking Auto Scaling activity history to identify launch failure reasons. Then I would validate the launch template, including AMI,
instance type, security groups, IAM roles, and user data scripts. After that, I would verify subnet capacity, AZ health, EC2 quotas,
and whether instances are failing health checks or bootstrapping during startup. In production environments, issues are often related to invalid AMIs,
permission changes, or failed initialization scripts rather than the Auto Scaling Group itself.

---


## 🚀 Tough Follow-Ups

**Q:** Instances launch but terminate immediately?  
**A:** Check health checks, user-data logs, cloud-init logs, and application startup failures.  

**Q:** What logs would you check?  
**A:** `/var/log/cloud-init.log` and `/var/log/user-data.log`  

**Q:** How do you debug launch template issues?  
**A:** Manually launch an EC2 instance using the same launch template to test.  

---

## 🚀 Killer Line

With Auto Scaling issues, the goal is to identify whether the failure is happening before launch, during bootstrapping, or during health validation.

---

## 🚀 Ultra Short Revision

1. Check ASG activity  
2. Validate launch template  
3. Check quotas/capacity  
4. Check IAM permissions  
5. Check user-data/cloud-init  
6. Check ALB/EC2 health checks
