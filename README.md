# AWS IAM User, Group & EC2 Role-Based Access Control

## Project Overview

Implemented an AWS IAM access-control environment using two IAM users, two IAM groups, IAM policies, and EC2 instance roles to demonstrate role-based access control and least-privilege permissions across EC2 and S3.

---

## Objective

To understand how IAM users, groups, policies, and EC2 IAM roles control access to AWS resources and validate permissions through practical EC2 and S3 operations.

---

## IAM Structure

```text
                         AWS IAM
                           │
              ┌────────────┴────────────┐
              │                         │
       Support Admin              Storage Admin
          Group                       Group
              │                         │
            User 1                   User 2
              │                         │
       ┌──────┴──────┐          ┌──────┴──────┐
       │             │          │             │
  EC2 Full      S3 Read     S3 Full       EC2 Read
   Access         Only       Access          Only
```

### Support Admin Group

- **EC2 Full Access**
- **S3 Read Only Access**
- **User 1** added to the group

### Storage Admin Group

- **S3 Full Access**
- **EC2 Read Only Access**
- **User 2** added to the group

---

## IAM User Permission Validation

### User 1 — Support Admin

User 1 was tested against the permissions assigned through the **Support Admin** group.

**Allowed Operations**

- Create/launch EC2 instances
- List and view S3 buckets

**Restricted Operation**

- Create an S3 bucket

The failed bucket-creation operation demonstrated the restriction imposed by the **S3 Read Only Access** policy.

### User 2 — Storage Admin

User 2 was tested against the permissions assigned through the **Storage Admin** group.

**Allowed Operations**

- Create/manage S3 buckets
- View EC2 instances

**Restricted Operation**

- Launch/create an EC2 instance

The failed EC2 creation operation demonstrated the restriction imposed by the **EC2 Read Only Access** policy.

---

## EC2 Instance IAM Roles

User 1 launched two EC2 instances for testing IAM roles and S3 access.

### Instance 1 — `EC2-S3_Read-Only`

An IAM role was created and attached to the instance with permissions providing **S3 read-only access**.

The EC2 instance was accessed through SSH using MobaXterm.

The AWS CLI was then used to test S3 access:

```bash
aws s3 ls
```

The S3 bucket list was successfully displayed.

An attempt was then made to create an S3 bucket, but the operation was denied according to the read-only policy.

---

### Instance 2 — `EC2-S3_Full-Access`

A separate IAM role was created and attached to the instance to provide the required **S3 full-access permissions**.

The instance was accessed through SSH using MobaXterm.

The S3 bucket list was checked:

```bash
aws s3 ls
```

The bucket list was successfully displayed.

### Create S3 Bucket

```bash
aws s3 mb s3://nkaz1704 --region eu-north-1
```

The bucket was successfully created and verified in the AWS S3 console.

### Delete Empty S3 Bucket

```bash
aws s3 rb s3://nkaz1704
```

This command removes the bucket when it is empty.

### Force Delete S3 Bucket

```bash
aws s3 rb s3://nkaz1704 --force
```

This removes the non-versioned objects first and then attempts to delete the bucket.

The bucket was successfully created, verified in the S3 console, and deleted during the permission test.

---

## SSH & AWS CLI Validation

Both EC2 instances were accessed using their **Public IPv4 addresses** through MobaXterm.

```text
MobaXterm
     │
     ▼
    SSH
     │
     ▼
EC2 Instance
     │
     ▼
 AWS CLI
     │
     ▼
    S3
     │
     ▼
IAM Role Permissions
```

Initially, S3 commands were tested before assigning the IAM roles to the instances. After the appropriate roles were attached through **Actions → Security → Modify IAM role**, the AWS CLI operations reflected the permissions associated with each role.

---

## IAM Permission Model

The project demonstrates two IAM access-control models.

### IAM User Access

```text
IAM User
    ↓
IAM Group
    ↓
IAM Policy
    ↓
AWS Resource
```

### EC2 Instance Access

```text
EC2 Instance
    ↓
IAM Role
    ↓
IAM Policy
    ↓
AWS CLI
    ↓
AWS S3
```

EC2 instance roles allow applications and commands running on an instance to access AWS services through temporary role credentials rather than requiring long-term AWS access keys to be stored on the server.

---

## Key Learning Outcomes

- IAM users and groups
- IAM policies
- EC2 permissions
- S3 permissions
- Role-based access control
- Least-privilege access
- IAM roles for EC2
- AWS CLI authentication through IAM roles
- S3 access from EC2
- Permission testing
- Access-denied validation
- SSH access using MobaXterm

---

## Project Documentation

All implementation screenshots and validation evidence are included directly in the project documentation. The screenshots are not required as a separate repository folder.

### Documentation Files

- `AWS_IAM_Role_Based_Access_Control_Project_Documentation.docx` — Editable Microsoft Word project documentation with the complete architecture, project overview, implementation steps, and embedded screenshots.
- `AWS_IAM_Role_Based_Access_Control_Project_Documentation.pdf` — PDF version of the same complete project documentation for sharing and review.

> **Note:** All 33 screenshots are embedded in the documentation and arranged according to the project implementation flow.


## Final Result

Successfully implemented and validated an AWS IAM permission model using users, groups, policies, and EC2 IAM roles.

The project verified both **allowed and denied operations** across EC2 and S3, demonstrating how IAM policies control access to AWS resources at both the user and instance level.

