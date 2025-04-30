# Load Balancer for a Single EC2 Instance

### ☁️ Built with Amazon Web Services (AWS)

![AWS](https://img.shields.io/badge/Built%20with-AWS-orange?style=flat&logo=amazonaws)
![Project Status](https://img.shields.io/badge/status-in--progress-yellow)


This project demonstrates how to set up an AWS Application Load Balancer (ALB) in front of a single EC2 instance, serving a basic web page.

## 🔧 Tech Stack
- AWS EC2
- AWS ALB (Application Load Balancer)
- Apache Web Server
- (Optional) Terraform

## 🧭 Steps Overview

1. Launch EC2 instance and install web server
2. Create a target group
3. Register EC2 instance
4. Create an Application Load Balancer
5. Test the endpoint via the Load Balancer DNS

## 📸 Output

![ALB Test](screenshots/alb-test.png)

## 📂 Folder Structure

- `setup/`: shell script to install the web server
- `terraform/`: optional infrastructure-as-code
- `screenshots/`: images of test results

## ✅ Status
- [x] EC2 Setup
- [x] ALB Configured
- [ ] Terraform version (optional)

## Step 1: Launch an EC2 Instance (Web Server)
####  Go to EC2 Dashboard
- Open the AWS Management Console

- Search for EC2

- Click Launch Instance

![image alt](https://github.com/Juniorklb/Create-a-Load-Balancer-for-a-single-EC2-instance/blob/eb2bae4e8162ef64ac666fa1d84421168c64040e/Images/EC2.PNG)
  
#### Configure Instance
