# 🔹 Explain NACL vs Security Group. Which One Do You Use?

---

## 🎯 Best Way to Start

Both Security Groups and Network ACLs are used to control traffic in AWS, but they operate at different layers and behave differently.

Strong opening. Straight to the point.

---

## ✅ Core Difference (MOST IMPORTANT)

| Feature       | Security Group             | Network ACL (NACL)          |
|---------------|---------------------------|-----------------------------|
| Applied At    | Instance level            | Subnet level               |
| Nature        | Stateful                  | Stateless                  |
| Rules         | Allow only                | Allow + Deny               |
| Scope         | Fine-grained              | Broad subnet control       |

---

## 🔥 Explain Security Groups First (Easier)

### 🔸 Security Group

Security Groups act as a virtual firewall at the instance level.

**Key Points:**

- Applied to EC2 instances / ENIs  
- Stateful: if inbound traffic is allowed, response traffic is automatically allowed  
- Supports **allow rules only**

**🧠 Real Example:**

In our organization, different applications inside the same subnet required different access rules. So we used Security Groups to control traffic at instance level.

- App A → allow port 80  
- App B → allow only port 443  

---

## 🔥 Explain NACL Properly

### 🔸 Network ACL (NACL)

Network ACLs operate at subnet level and provide an additional layer of security.

**Key Points:**

- Applied to subnets  
- Stateless: inbound and outbound traffic must be explicitly allowed  
- Supports both **allow** and **deny** rules

**🧠 Real Example:**

We used NACLs to restrict traffic to sensitive subnets like database subnets.

- Allow only DB traffic from app subnet  
- Deny all other access  

**⚠️ Important Correction:**  

- Default NACL → allows all traffic  
- Custom NACL → deny by default  
- Correct phrasing:  
NACLs follow an explicit rule-based model, and anything not allowed is denied.

---

## 🔥 Stateful vs Stateless (MUST Explain Well)

- **Security Group = Stateful**  
  If inbound traffic is allowed, return traffic is automatically permitted.
  Example: Allow inbound port 80 → response traffic allowed automatically

- **NACL = Stateless**  
  Inbound and outbound rules must both be configured explicitly. 
  Example: Allow inbound port 80 → must also allow outbound ephemeral ports

> Mentioning ephemeral ports is very strong if you can remember it.

---

## 🔥 Best Real-World Usage Answer

In practice, we mainly use Security Groups for application-level access control because they’re easier to manage and more granular. NACLs are used as an additional subnet-level security layer for sensitive environments.

---

## 🔥 Final Polished Answer (Say This)

Security Groups and NACLs are both used for traffic control in AWS. Security Groups operate at instance level and are stateful, meaning return traffic is automatically allowed. They support only allow rules and are commonly used for application-level access control.  
 Network ACLs operate at subnet level and are stateless, so inbound and outbound rules must both be configured. They support both allow and deny rules and are typically used as an additional security layer for subnet-level protection.


---

## 🔥 Tough Follow-ups (VERY COMMON)

- **Which is evaluated first?**  
  Traffic passes through NACLs at subnet level, then Security Groups at instance level.

- **Which one is more commonly used?**  
  Security Groups are used more frequently because they provide granular instance-level control.

- **Can Security Group deny traffic?**  
  No, Security Groups only support allow rules.

- **Why use NACL if SG already exists?**  
  NACL provides subnet-level protection and explicit deny capability.


## 🚀 Killer Line (Use This)

Security Groups provide fine-grained instance security, while NACLs provide coarse-grained subnet security.


---

## 🚀 Ultra Short Revision

- SG → instance level, stateful, allow only  
- NACL → subnet level, stateless, allow + deny  
- SG used more often  
- NACL for extra subnet protection
