# AWS IAM Project – Users, Policies, Roles, and CLI Access

## 📌 Overview
This project demonstrates setting up and testing **AWS Identity and Access Management (IAM)**.  
I created IAM users, groups, policies, and roles, then validated their permissions through the AWS CLI, CloudShell, and an EC2 instance with an attached IAM role.

---

## 🛠️ Steps Followed
1. Created **StudentUser** and added it to the **Developers** group.
2. Attached a custom **S3 read-only policy** to the group, and an additional custom policy for specific bucket permissions where needed.
3. Configured AWS CLI locally with StudentUser’s credentials using `aws configure --profile student`.
4. Verified StudentUser permissions:
   - `aws s3 ls` ✅ worked
   - `aws s3 mb ...` ❌ denied (AccessDenied)
   - `aws ec2 describe-instances` ✅ worked
5. Created an **IAM Role (EC2-S3ReadWriteRole)** and attached it to an EC2 instance.
6. Verified that the EC2 instance could upload files directly to S3 without static keys using `aws s3 cp /etc/hostname s3://studentuser-assignment-bucket/`.
7. Ran CLI commands in **CloudShell** to test identity and permissions (`aws sts get-caller-identity`, `aws s3 ls`).

---

## 🖼️ Screenshots Included
- `iam-student-user.png` – IAM console screenshot showing StudentUser.
- `iam-developers-group.png` – Developers group with attached policies.
- `custom-policy.png` – Custom JSON policy used for S3 permissions.
- `aws-sts-identity.png` – Output of `aws sts get-caller-identity`.
- `aws-s3-ls.png` – Output of `aws s3 ls` for the student profile.
- `cloudshell-cli.png` – CloudShell running an AWS CLI command.
- `ec2-upload-s3.png` – EC2 instance SSH session uploading `/etc/hostname` to S3.

---

## ⚡ Challenges and Solutions
- **ConfigParseError** when running AWS CLI due to malformed `~/.aws/config`. Solution: fixed the INI formatting and ensured profiles are correctly defined.
- **AccessDenied** for bucket creation while testing StudentUser. Solution: Verified policies were intentionally read-only; used IAM Role for EC2 write access instead.
- **Avoid storing long-lived keys on EC2**: used an IAM role attached to the instance to provide temporary credentials.

---

## 🔐 Shared Responsibility Model (IAM)
AWS secures the **infrastructure of the cloud** (physical hosts, network, and the IAM service).  
The customer secures **in the cloud**: managing IAM users, groups, roles, permissions, credential rotation, and enforcing least privilege for identities and workloads.

---

## ✅ How to reproduce tests (summary)
1. Configure student profile: `aws configure --profile student`
2. List buckets: `aws s3 ls --profile student`
3. Attempt to create a bucket: `aws s3 mb s3://studentuser-assignment-bucket --profile student` (expected: AccessDenied if read-only)
4. Verify EC2 role write access by running inside EC2: `aws s3 cp /etc/hostname s3://studentuser-assignment-bucket/`
5. Get caller identity: `aws sts get-caller-identity --profile student`

---

**Author**: Josh (Student)  
**Date**: 2025-09-16

