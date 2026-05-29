# AWS IAM Role Chaining

## Project Overview

In large AWS environments, engineers often require temporary elevated access to perform operational or troubleshooting tasks. Granting permanent access to sensitive resources increases security risks and violates the principle of least privilege.

This project demonstrates how AWS IAM Role Chaining can be used to provide secure, temporary, and auditable access to resources through multiple IAM roles.

The implementation simulates a real-world cloud operations workflow where an engineer assumes an operational role and then assumes a monitoring role to gain additional access required for a specific task.

---

## Business Scenario

A Cloud Operations Engineer normally works with limited permissions.

During an infrastructure incident, the engineer may need access to operational resources and monitoring tools to investigate the issue.

Instead of assigning all permissions directly to the user, AWS allows temporary access through IAM roles.

Workflow:

chain-user
↓
CloudOpsRole
↓
MonitoringRole

This approach improves security by limiting permanent permissions and ensuring all privileged access is temporary and auditable.

---

## Why Role Chaining?

Role chaining allows one IAM role to assume another IAM role.

Benefits:

* Reduces permanent access permissions
* Supports least privilege access
* Improves security governance
* Enables temporary privilege elevation
  

AWS additionally limits chained role sessions to one hour, reducing the risk of long-lived privileged sessions.

---

## Architecture

chain-user
↓
AssumeRole
↓
CloudOpsRole
↓
AssumeRole
↓
MonitoringRole

---

## Services Used

* AWS IAM
* AWS STS (Security Token Service)
* AWS CloudTrail

---

## Implementation Steps

### Step 1 – Create IAM User

Created an IAM user named:

* chain-user

The user was intentionally created with minimal permissions.

---

### Step 2 – Create CloudOpsRole

Created an IAM role named:

* CloudOpsRole

Permissions:

* AmazonEC2ReadOnlyAccess

Purpose:

Allows engineers to view and investigate EC2 infrastructure resources.

---

### Step 3 – Create MonitoringRole

Created an IAM role named:

* MonitoringRole

Permissions:

* CloudWatchReadOnlyAccess

Purpose:

Allows engineers to access monitoring and operational metrics.

---

### Step 4 – Configure Trust Relationships

Configured MonitoringRole to trust CloudOpsRole.

This ensures that MonitoringRole can only be assumed through CloudOpsRole and not directly by the IAM user.

---

### Step 5 – Configure AssumeRole Permissions

Granted:

chain-user
↓
Assume CloudOpsRole

and

CloudOpsRole
↓
Assume MonitoringRole

This created the complete role chain.

---

### Step 6 – Validate Role Chaining

Successfully switched roles in the AWS Console:

chain-user
↓
CloudOpsRole
↓
MonitoringRole

This validated the trust relationships and AssumeRole permissions.

---

## Security Concepts Demonstrated

### Least Privilege

Users receive only the permissions required for their responsibilities.

### Temporary Credentials

Access is granted through temporary STS credentials rather than permanent permissions.

### Trust Relationships

IAM roles explicitly define who is allowed to assume them.

### Role Chaining

One role can assume another role to gain additional permissions when required.

---

## Key Learnings

* How IAM Roles differ from IAM Users
* How AssumeRole works
* How Trust Policies control access
* How AWS STS provides temporary credentials
* How Role Chaining enables secure privilege elevation
* Why AWS enforces a one-hour limit on chained role sessions


---

## Outcome

Successfully implemented and validated AWS IAM Role Chaining using IAM users, IAM roles, trust policies, and AssumeRole permissions.

The project demonstrates a real-world access delegation model used in enterprise AWS environments to provide secure, temporary, and auditable access while adhering to the principle of least privilege.
