````markdown
# Auto Scaling Lab with Apache Web Server

This project demonstrates how to build a **highly available and scalable web infrastructure** on AWS using **CloudFormation**.  
The stack provisions networking resources, security, an Application Load Balancer (ALB), an Auto Scaling Group (ASG), and Apache web servers running a sample web app.

---

## 📌 Features

- **VPC Setup**

  - Custom VPC with CIDR `10.0.0.0/16`
  - 2 Public Subnets (for ALB & NAT Gateways)
  - 2 Private Subnets (for EC2 instances in Auto Scaling Group)
  - Internet Gateway & NAT Gateways for outbound access

- **Routing**

  - Public Route Table (routes internet traffic through IGW)
  - Private Route Tables (routes private traffic via NAT Gateways)

- **Security**

  - Security Group allowing inbound HTTP (port 80) from anywhere
  - EC2 IAM Role with **SSM Managed Instance Core** for Systems Manager access (no key pairs required)

- **Compute**

  - Launch Template using the latest Amazon Linux 2 AMI
  - User Data script installs Apache (`httpd`) and serves a custom **web page with CGI scripts**
  - CGI endpoints:
    - `/instance-info` → Shows instance ID and IP
    - `/stress-cpu` → Runs a CPU stress test for 5 minutes

- **Load Balancing & Auto Scaling**
  - Application Load Balancer (ALB) across public subnets
  - Target Group attached to Auto Scaling Group
  - Auto Scaling Group in private subnets
  - Scaling Policy: scales out/in based on **average CPU utilization > 30%**

---

## 🚀 Deployment

1. Save the CloudFormation template as `autoscaling-lab.yaml`.
2. Deploy using AWS CLI:
   ```bash
   aws cloudformation create-stack \
     --stack-name AutoScalingLab \
     --template-body file://autoscaling-lab.yaml \
     --capabilities CAPABILITY_NAMED_IAM
   ```
````

3. Wait for the stack to finish creating:

   ```bash
   aws cloudformation wait stack-create-complete --stack-name AutoScalingLab
   ```

---

## 🌍 Accessing the Application

After deployment, the stack exports the **Load Balancer DNS name**.
Find it in the **Outputs** section of the CloudFormation console or run:

```bash
aws cloudformation describe-stacks \
  --stack-name AutoScalingLab \
  --query "Stacks[0].Outputs"
```

Then open in browser:

```
http://<LoadBalancerDNS>
```

---

## 📖 Learning Objectives

- Understand **VPC design with public & private subnets**
- Deploy **highly available NAT Gateways**
- Configure **routing for public/private traffic**
- Automate server setup with **User Data scripts**
- Implement **Application Load Balancer** for traffic distribution
- Enable **Auto Scaling** with CPU-based scaling policies
- Use **AWS Systems Manager** instead of SSH key pairs for secure management

---

## ✅ Cleanup

To avoid ongoing costs, delete the stack after use:

```bash
aws cloudformation delete-stack --stack-name AutoScalingLab
```

---

## 🛠 Tools & Services Used

- **AWS CloudFormation**
- **Amazon VPC**
- **Amazon EC2**
- **AWS Systems Manager (SSM)**
- **Elastic Load Balancer (ALB)**
- **Auto Scaling**
- **IAM**

```

```
