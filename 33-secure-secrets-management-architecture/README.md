# Secure Secrets Management Architecture

## System Design Question

Design a secure secrets-management architecture on AWS.

Applications should be able to retrieve sensitive information such as:

- Database passwords
- API keys
- Third-party credentials
- Application secrets

without storing those secrets directly inside:

- Source code
- `.env` files
- Container images
- EC2 user data
- Application configuration files

The architecture should support centralized secret storage, encryption, least-privilege access, automatic rotation, auditing, monitoring, and security alerts.

---

## Architecture

![Secure Secrets Management Architecture](./secure-secrets-management-architecture.png)

---

## Architecture Overview

This architecture uses AWS Secrets Manager as the central location for storing application secrets.

Applications running on:

- Amazon EC2
- AWS Lambda
- Amazon EKS

use IAM roles to securely request only the secrets they are authorized to access.

AWS KMS protects secrets with encryption.

AWS Secrets Manager can use a Rotation Lambda to rotate credentials automatically.

For database credentials, the Rotation Lambda updates both Secrets Manager and Amazon RDS.

AWS CloudTrail records secrets-related API activity.

Amazon CloudWatch provides monitoring.

Amazon EventBridge and Amazon SNS notify the Security Team when important failures or security events occur.

---

## Main AWS Services

- AWS Secrets Manager
- AWS IAM
- AWS KMS
- AWS Lambda
- Amazon EC2
- Amazon EKS
- Amazon RDS
- AWS CloudTrail
- Amazon CloudWatch
- Amazon EventBridge
- Amazon SNS

---

## Main Access Pattern

Applications should never contain permanent secrets directly.

Instead:

```text
Application
    ↓
IAM Role
    ↓
AWS Secrets Manager
    ↓
Secret Retrieved at Runtime
```

Examples:

```text
EC2 Application
      ↓
EC2 IAM Role
      ↓
Secrets Manager
```

```text
Lambda Function
      ↓
Lambda Execution Role
      ↓
Secrets Manager
```

```text
EKS Workload
      ↓
Pod IAM Role
      ↓
Secrets Manager
```

---

## Centralized Secret Storage

AWS Secrets Manager stores sensitive values in one controlled location.

Examples include:

```text
Database Password
API Key
Service Token
Third-Party Credential
Application Secret
```

Centralized storage reduces the risk of secrets being duplicated across applications.

---

## Runtime Secret Retrieval

Applications retrieve secrets only when required.

```text
Application Starts
      ↓
IAM Authentication
      ↓
Secrets Manager
      ↓
Secret Returned
      ↓
Application Uses Secret
```

This avoids keeping secrets permanently inside code or configuration files.

---

## IAM-Based Access

IAM controls which application can access which secret.

Example:

```text
Order Service
      ↓
Order IAM Role
      ↓
Order DB Secret
```

Another service may use a different role:

```text
Payment Service
      ↓
Payment IAM Role
      ↓
Payment API Secret
```

The Order Service should not automatically have permission to read the Payment Service secret.

This follows the principle of least privilege.

---

## Least-Privilege Access

A good policy should allow only the required action.

For example:

```text
Application Role
      ↓
secretsmanager:GetSecretValue
      ↓
Specific Secret Only
```

Avoid giving broad permissions such as:

```text
secretsmanager:*
```

across all secrets unless absolutely necessary.

---

## Encryption with AWS KMS

Secrets stored in Secrets Manager are protected using encryption.

```text
AWS KMS
   ↓
AWS Secrets Manager
```

KMS helps protect secret data at rest.

Access to the KMS key should also follow least privilege.

---

## Database Secret Example

A common use case is storing an RDS database password.

```text
Application
    ↓
IAM Role
    ↓
Secrets Manager
    ↓
Database Credentials
    ↓
Amazon RDS
```

The application does not need to permanently store the database username or password.

---

## Automatic Secret Rotation

Secrets should not remain unchanged forever.

AWS Secrets Manager can use a Lambda function to rotate credentials automatically.

```text
Secrets Manager
      ↓
Rotation Lambda
      ↓
Amazon RDS
```

A rotation process can:

1. Generate a new password
2. Update the target database
3. Update Secrets Manager
4. Test the new credential
5. Mark the new version as active

---

## Rotation Flow

```text
Current Secret
     ↓
Secrets Manager
     ↓
Rotation Lambda
     ↓
Generate New Credential
     ↓
Update Amazon RDS
     ↓
Update Secret
```

Applications can continue retrieving the current valid credential through Secrets Manager.

---

## Rotation Schedule

A rotation policy could be configured for:

```text
Every 30 Days
Every 60 Days
Every 90 Days
```

The exact rotation period depends on security and compliance requirements.

The important principle is:

```text
Secrets should rotate automatically instead of staying permanent.
```

---

## Rotation Failure Handling

Credential rotation can fail because of:

- Database connectivity problems
- Permission errors
- Lambda execution errors
- Incorrect secret configuration
- Target-service failures

A failure should generate an alert.

```text
Rotation Lambda
      ↓
Failure Event
      ↓
EventBridge
      ↓
SNS
      ↓
Security Team
```

This prevents broken rotations from going unnoticed.

---

## Amazon RDS Integration

The Rotation Lambda can update database credentials inside Amazon RDS.

```text
Secrets Manager
      ↓
Rotation Lambda
      ↓
Amazon RDS
```

After rotation, applications continue retrieving the latest valid secret from Secrets Manager.

---

## Audit Logging with CloudTrail

AWS CloudTrail records API activity involving Secrets Manager.

```text
Secrets Manager
      ↓
AWS CloudTrail
```

CloudTrail can help answer:

- Who accessed a secret?
- When was the secret accessed?
- Which IAM identity made the request?
- Which API operation was used?
- Who changed secret configuration?

An important API call to monitor is:

```text
GetSecretValue
```

---

## Monitoring with CloudWatch

Amazon CloudWatch provides operational visibility.

```text
Secrets Manager
Rotation Lambda
Amazon RDS
      ↓
CloudWatch
```

CloudWatch can monitor:

- Rotation Lambda errors
- Lambda duration
- Failed rotations
- Application failures
- Database connection failures
- Unusual operational activity

---

## Security Alerting

Important security or operational events can be routed through EventBridge.

```text
Security Event
     ↓
EventBridge
     ↓
SNS
     ↓
Security Team
```

Examples include:

- Rotation failure
- Unexpected Lambda failure
- Secret configuration change
- Operational incident

---

## No Hard-Coded Credentials

Applications should avoid patterns like:

```text
DB_PASSWORD="mypassword123"
```

inside:

- Application source code
- Git repositories
- Docker images
- Kubernetes manifests
- EC2 user data
- CI/CD scripts

Instead:

```text
Application
    ↓
IAM Role
    ↓
Secrets Manager
```

This greatly reduces credential exposure.

---

## EC2 Secret Access

An EC2 application can use an instance role.

```text
EC2 Application
      ↓
EC2 Instance Role
      ↓
Secrets Manager
```

No permanent AWS access keys need to be stored on the EC2 instance.

---

## Lambda Secret Access

AWS Lambda uses its execution role.

```text
Lambda Function
      ↓
Execution Role
      ↓
Secrets Manager
```

The role should only allow access to the specific secret required by that function.

---

## EKS Secret Access

An EKS workload should use workload-specific AWS identity.

Conceptually:

```text
EKS Pod
   ↓
Pod IAM Role
   ↓
Secrets Manager
```

This prevents all Pods from inheriting overly broad worker-node permissions.

---

## Application Isolation

Different workloads should have different permissions.

For example:

```text
EC2 App
→ Secret A
```

```text
Lambda App
→ Secret B
```

```text
EKS App
→ Secret C
```

This limits the impact if one application is compromised.

---

## Main Architecture Flow

```text
EC2 Application ─────┐
Lambda Function ─────┤
EKS Application ─────┤
                     ↓
                 IAM Roles
                     ↓
             AWS Secrets Manager
```

Encryption:

```text
AWS KMS
   ↓
Secrets Manager
```

Rotation:

```text
Secrets Manager
      ↓
Rotation Lambda
      ↓
Amazon RDS
```

Auditing:

```text
Secrets Manager
      ↓
CloudTrail
```

Monitoring:

```text
Secrets Manager
Rotation Lambda
      ↓
CloudWatch
```

Alerting:

```text
Rotation Failure
      ↓
EventBridge
      ↓
SNS
      ↓
Security Team
```

---

## Security Principles

### Centralize Secrets

Store secrets in one controlled service instead of spreading them across applications.

### Least Privilege

Each application should access only the secrets it requires.

### Encrypt Secrets

Protect secret values using AWS KMS.

### Rotate Credentials

Automatically replace long-lived credentials.

### Retrieve at Runtime

Applications should request secrets only when needed.

### Audit Access

Use CloudTrail to record API activity.

### Monitor Failures

Use CloudWatch to detect rotation and application problems.

### Alert Security Teams

Use EventBridge and SNS for important failures and security events.

---

## Key Design Decisions

| Component | Purpose |
|---|---|
| AWS Secrets Manager | Central secret storage |
| IAM Roles | Control application access |
| AWS KMS | Encrypt secret data |
| Rotation Lambda | Automatically rotates credentials |
| Amazon RDS | Example protected database |
| CloudTrail | Audits API activity |
| CloudWatch | Monitors operational health |
| EventBridge | Routes important events |
| SNS | Sends security alerts |
| Security Team | Responds to failures and incidents |

---

## Benefits

### Reduced Credential Exposure

Secrets are removed from source code and configuration files.

### Centralized Management

Secrets can be managed from one service.

### Least-Privilege Access

Different applications receive different permissions.

### Automatic Rotation

Credentials can change without manually updating every application.

### Stronger Auditing

CloudTrail records access and configuration activity.

### Better Monitoring

CloudWatch provides visibility into failures.

### Faster Security Response

EventBridge and SNS can alert the Security Team automatically.

---

## Tradeoffs

This architecture also introduces some operational considerations:

- Secrets Manager cost
- Rotation Lambda maintenance
- IAM policy complexity
- KMS key management
- Application caching strategy
- Rotation testing
- Dependency on Secrets Manager availability

Applications should also avoid requesting the same secret excessively when secure caching is appropriate.

---

## Key Takeaway

The core architecture is:

```text
Application
   ↓
IAM Role
   ↓
AWS Secrets Manager
```

Security is strengthened through:

```text
AWS KMS
+
Least-Privilege IAM
+
Runtime Retrieval
+
Automatic Rotation
```

Rotation follows:

```text
Secrets Manager
→ Rotation Lambda
→ Amazon RDS
```

Visibility is provided through:

```text
CloudTrail
+
CloudWatch
+
EventBridge
+
SNS
```

This design removes hard-coded credentials and creates a centralized, encrypted, auditable, and automatically rotating secrets-management system on AWS.