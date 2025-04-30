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
####  Configure Instance
Name: WebServer-Instance

- AMI: Amazon Linux 2

- Instance Type: t2.micro (free tier eligible)

- Key pair: Create new or choose existing (you’ll need this to SSH)

- Network settings:
    - Allow SSH (port 22) and HTTP (port 80)

    - Optional: You can also allow HTTPS (port 443)


#### User Data (Advanced Details)      

Scroll to Advanced Details and paste this script into User data:

    #!/bin/bash
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello from EC2 behind a Load Balancer!</h1>" > /var/www/html/index.html

 This installs Apache and creates a basic web page.   

#### Launch
- Click Launch Instance

- Wait for status: Running

#### Test the Web Server

- Go to the EC2 Instance details

- Copy the Public IPv4 address

- Paste it in your browser:
  
          http://<your-ec2-ip>

- You should see:
  
      Hello from EC2 behind a Load Balancer!

  ![image alt](https://github.com/Juniorklb/Create-a-Load-Balancer-for-a-single-EC2-instance/blob/eefdad2dac97488ba1e8a6c5e2e98a07d308ef52/Images/ALBhello.PNG)

## Step 2: Create a Target Group

#### 🔧 How to Create a Target Group in the Console

<img align="right" alt="Coding" width="400" src="https://github.com/Juniorklb/Juniorklb/blob/662692f737cc8f550da799d48190446b55a68900/Working%20hard.jpeg">
 
- Go to the EC2 Dashboard

- In the left menu, scroll down and click Target Groups

- Click Create target group

####  🔧 Target Group Settings

- Target type: Instances

- Name: EC2-Web-TG

- Protocol: HTTP

- Port: 80

- VPC: Select the one your EC2 is in

- Health checks:

- Protocol: HTTP

- Path: /

   - Click Next
 
####  Register Targets

- Select your EC2 instance

- Click Include as pending below

- Click Create target group
