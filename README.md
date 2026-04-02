AWS Cloud Infrastructure: Secure EC2 to S3 Integration
Project Overview
This project demonstrates the deployment of a secure, custom AWS environment. It features a manual build of a Virtual Private Cloud (VPC) and a Linux-based EC2 instance configured to interact with S3 storage using IAM Roles for enhanced security.

Infrastructure Components
VPC: gomyvpc (10.0.0.0/16)

Subnet: PublicSubnet1 (10.0.1.0/24)

Security Group: GoMyEC2SecurityGroup (SSH Port 22 allowed)

IAM Role: EC2-S3-Access-Role (S3 Full Access)

Storage: gomys3bucket (S3 Bucket)

Implementation Details
1. Network Setup
I created the gomyvpc network to isolate resources. I attached an Internet Gateway and configured the Route Table for PublicSubnet1 to allow outbound internet access, transforming it into a functional public subnet.

2. Security & Identity
Instead of using risky static credentials, I implemented an IAM Role named EC2-S3-Access-Role. This role was attached to the EC2 instance, allowing it to generate temporary security tokens to interact with AWS services.

The GoMyEC2SecurityGroup was configured with an inbound rule to restrict SSH access to my specific local IP address, preventing unauthorized remote access.

3. S3 Interaction (CLI Proof)
After connecting to the instance via SSH, I performed the following operations to verify the IAM Role integration:

Bash

# Verify the assigned IAM identity
aws sts get-caller-identity

# Create a local test file
echo "AWS Infrastructure Project Complete" > mytestfile.txt

# Upload file to the bucket
aws s3 cp mytestfile.txt s3://gomys3bucket/

# List bucket contents to verify upload
aws s3 ls s3://gomys3bucket/
