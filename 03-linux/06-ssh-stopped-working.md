## SSH to an instance stopped working. How will you troubleshoot the issue?

### 📝 Short Explanation  
This question checks how well you can debug access issues to a Linux server, particularly EC2 instances or cloud VMs. SSH failures could be due to network, OS-level, key-based, or IAM misconfigurations.

## ✅ Answer  
I would follow a step-by-step process to identify whether the issue is with networking (e.g., security group), the instance itself (e.g., crashed SSH service), or the credentials (e.g., PEM file or key mismatch). Based on the findings, I would take corrective actions accordingly.

---

### 📘 Detailed Explanation  

---

### ✅ Step 1: Confirm the Error Message  
From your local machine:
```bash
ssh -i my-key.pem ec2-user@<instance-public-ip>
```
Make sure ip address is correct, .pem file is correct and has correct permissions (400)

Typical errors:
- `Permission denied (publickey)`
- `Connection refused`
- `Operation timed out`

The error gives the first clue.

---

### ✅ Step 2: Check Instance Health & Reachability
- Is the instance **running**?
- Is it **reachable**?
  
Use AWS Console or CLI:
```bash
aws ec2 describe-instance-status --instance-id <id>
ping <public-ip>
```

If instance is unreachable → investigate VPC/subnet/NACL routing issues.

---

### ✅ Step 3: Verify Security Group Rules  
Make sure port 22 is open **from your IP**:
```text
Inbound rule:
Type: SSH
Port: 22
Source: your IP (e.g., 203.0.113.0/32)
```

If using a **bastion host**, check its connectivity as well.

---

### ✅ Step 4: Check Network ACLs & Route Tables  
Ensure NACLs are not blocking traffic and public subnet has a route to internet gateway.

---

### ✅ Step 5: Confirm Public IP or Elastic IP  
Check if the instance has a **public IP** or **Elastic IP** attached.  
Elastic IPs don’t change, but public IPs do if the instance is stopped and started.

---

### ✅ Step 6: Validate PEM File & User  
Make sure:
- The PEM file is correct (`chmod 400`)
- You're using the right username:
  - `ec2-user` for Amazon Linux
  - `ubuntu` for Ubuntu
  - `centos` for CentOS

---

### ✅ Step 7: Try EC2 Instance Connect (Amazon Linux only)  
If the PEM is lost or SSH doesn't work:
- Use **EC2 Instance Connect** via AWS Console
- Once inside, you can check:
  ```bash
  sudo systemctl status sshd
  tail -n 50 /var/log/auth.log  # or secure/log
  ```

---

### ✅ Step 8: Rescue Mode (Advanced)  
If all else fails:
- Stop the instance
- Detach the root volume
- Attach it to another instance
- Mount it and edit `~/.ssh/authorized_keys` or repair config
- Reattach and start the original instance

---

### 🧠 Real-Life Causes I’ve Faced:
- Team used a wrong key pair name
- Security group updated accidentally
- User tried SSH with wrong username (e.g., root)
- Instance rebooted with new IP and old DNS cached
- `/etc/ssh/sshd_config` edited incorrectly

> Summary:  
> Identify error → check network reachability → inspect key/user mismatch → use EC2 Connect if possible → rescue via EBS if needed.

---
