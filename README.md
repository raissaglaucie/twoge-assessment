# Twoge Deployment on AWS

**Twoge** is a social media platform where users can exclusively share their thoughts about **Doge**. This project was successfully deployed using various AWS services to ensure scalability, security, and efficiency.

---

## Project Overview

As part of the Code Platoon DevOps team, the objective was to deploy Twoge on AWS, utilizing the following services:

- **Elastic Compute Cloud (EC2):** To host the application.
- **Simple Storage Service (S3):** For storing static files (e.g., images).
- **Identity and Access Management (IAM):** To manage permissions for AWS resources.
- **Virtual Private Cloud (VPC):** To create a secure and isolated network environment.
- **Application Load Balancer (ALB):** To distribute traffic across multiple EC2 instances.
- **Auto Scaling Group (ASG):** To automatically scale EC2 instances based on traffic.
- **Simple Notification Service (SNS):** For notifications related to ASG scaling events.
- **RDS (PostgreSQL):** A managed database service for Twoge.

---

## Milestones

1. **Amazon VPC with Public Subnets**
   - Created a VPC with two public subnets across different availability zones.

2. **Hosting Static Files on S3**
   - Created an S3 bucket for static files.
   - Configured a bucket policy to allow public read access.

3. **IAM Role for S3 Access**
   - Created an IAM role to allow EC2 instances to access the S3 bucket.

4. **EC2 Instance Deployment**
   - Launched an EC2 instance using Ubuntu Linux.
   - Installed and configured the Twoge application.

5. **Application Load Balancer (ALB)**
   - Configured an ALB to forward traffic to the EC2 instances.
   - Added listener rules to ensure traffic is routed correctly.

6. **Auto Scaling Group (ASG)**
   - Created an ASG to scale the EC2 instances based on traffic.
   - Integrated the ASG with the ALB.

7. **Simple Notification Service (SNS)**
   - Configured an SNS topic for email notifications about ASG scaling events.

8. **Server Shutdown Notifications**
   - Demonstrated SNS email notifications upon server shutdown.

9. **Stress Test with ASG Scaling**
   - Simulated high traffic using a Python-based stress test.
   - Demonstrated ASG functionality by scaling instances up and down.


![Terminal Screenshot](images/terminal.png)



![CPU Utilization Screenshot](images/CPU.png)



---

## Architecture Diagram

![Twoge Screenshot](images/Screenshot%202024-12-26%20at%207.54.28%E2%80%AFPM.png)


The architecture includes:

- **VPC:** A secure VPC with public subnets in 2 availability zones.
- **IAM:** Policies for service access and permission management.
- **S3:** Hosting static files.
- **EC2 Instances:** Managed by an ASG and connected to the ALB.
- **RDS:** A managed PostgreSQL database for application data.
- **SNS:** Notifications for scaling events.
- **ALB:** Routes incoming traffic to EC2 instances.

---

## Deployment Process

### Terminal Commands

```bash
# Update and install dependencies
sudo apt update -y && sudo apt upgrade -y
sudo apt install git python3 python3-pip python3-venv -y

# Clone Twoge repository
git clone https://github.com/codeplatoon-devops/twoge.git
cd twoge

# Set up Python environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Configure environment variables
echo "SQLALCHEMY_DATABASE_URI=postgresql://raissa:yourpassword@raissa-twoge-database.aws-url/twoge" >> .env

# Start application
nohup venv/bin/gunicorn --bind 0.0.0.0:8080 app:app > app.log 2>&1 &
```



## S3 Bucket Policy


```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::raissa-twoge-static-files/*"
    }
  ]
}
```

## IAM Role Policy

```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:*", "s3-object-lambda:*"],
      "Resource": "*"
    }
  ]
}

```

## Demonstration

### Architecture Explanation
Detailed explanation of the VPC, subnets, ALB, ASG, and RDS setup.
### Application Demo
Live demonstration of the Twoge application running on AWS.
### Stress Test
Simulate high traffic to trigger scaling and demonstrate the ASG functionality.


### Conclusion

The Twoge application was successfully deployed on AWS, leveraging best practices for scalability and reliability. The system ensures a smooth user experience and handles traffic efficiently.

### Repository

[GitHub Repository Link](https://github.com/codeplatoon-devops/twoge)

### Author

[Raissa Barbosa](https://github.com/raissaglaucie)






