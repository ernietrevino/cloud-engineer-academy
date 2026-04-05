# Week 2 — AWS CLI, IAM, S3, and Cloud Architecture

## Week 2: Mastering the AWS CLI and designing cloud infrastructure

Setting up the AWS CLI, managing IAM users programmatically, 
storing files in S3, and designing a production-grade web 
application architecture.

---

Week 2 covered the AWS CLI, S3 storage, IAM management via 
terminal, and cloud architecture design. Here's what I built 
and what I learned.

## AWS CLI installation and configuration

Installed the AWS CLI on macOS using Homebrew with a single 
command. Configured it using `aws configure` with IAM user 
credentials, default region `us-east-1`, and JSON output format.

Verified the connection using:

    aws sts get-caller-identity

This confirmed the CLI was authenticated as my IAM user Lucky 
with full account access.

## S3 bucket operations

Created a new S3 bucket from the terminal:

    aws s3 mb s3://ernie-cloud-bootcamp-2026

Listed all buckets to verify:

    aws s3 ls

Uploaded a local file directly to the bucket:

    aws s3 cp test.txt s3://ernie-cloud-bootcamp-2026/

Verified the upload by listing bucket contents:

    aws s3 ls s3://ernie-cloud-bootcamp-2026/

Key insight: S3 buckets are globally unique. No two buckets 
anywhere in the world can share the same name.

## IAM user management via CLI

Created a new IAM user programmatically:

    aws iam create-user --user-name CloudStudent

Attached a read-only S3 policy using the policy ARN:

    aws iam attach-user-policy --user-name CloudStudent \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

Verified the policy was attached:

    aws iam list-attached-user-policies --user-name CloudStudent

Created programmatic access keys for the user:

    aws iam create-access-key --user-