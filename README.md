# Load Balancer for a Single EC2 Instance

### ☁️ Built with Amazon Web Services (AWS)

![AWS](https://img.shields.io/badge/Built%20with-AWS-orange?style=flat&logo=amazonaws)
![Project Status](https://img.shields.io/badge/status-finished-green)


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

![image alt](https://github.com/Juniorklb/Create-a-Load-Balancer-for-a-single-EC2-instance/blob/d64548ec9f3da500d2e5e86fd5d4cc8ba2b77125/Images/ALBloadbalancer.PNG)

## 📂 Folder Structure

- `setup/`: shell script to install the web server
- `terraform/`: optional infrastructure-as-code
- `screenshots/`: images of test results

## Status
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
     
 ![img alt](https://github.com/Juniorklb/Create-a-Load-Balancer-for-a-single-EC2-instance/blob/8f683b6763c019ed4a8c173d0b541d21e1872905/Images/EC2-WEB-TG%201.PNG)

####  Register Targets

- Select your EC2 instance

- Click Include as pending below

- Click Create target group

## Step 3: Create the Application Load Balancer (ALB)

    This will distribute traffic (even if it’s just one instance for now) and allow you to scale in the future.

![image alt](https://github.com/Juniorklb/Create-a-Load-Balancer-for-a-single-EC2-instance/blob/a01ab23b4ee675a916cd475e79592f500c2f27be/Images/APPloadbalancer.PNG)

#### 🧭 How to Create an ALB in the Console

- Go to EC2 Dashboard

- In the left menu, click Load Balancers

- Click Create Load Balancer

- Choose Application Load Balancer (NOT Network Load Balancer)

#### ⚙️ Step-by-Step ALB Setup
##### Basic Configuration

- Name: WebApp-ALB

- Scheme: Internet-facing

- IP address type: IPv4

##### Network Mapping

- VPC: Same as your EC2

- Availability Zones:

   - Select at least one AZ with your instance in it

   - Tick the subnet of your EC2
 
##### Listeners and Routing
- Listener: HTTP : 80

- Default action: Choose Forward to → Select your Target Group (EC2-Web-TG)

####  Security Groups

- Choose an existing SG OR

- Create a new Security Group that allows inbound HTTP (port 80)
 
#### Review and Create

- Click Create load balancer

- Wait until the State becomes Active (refresh if needed)

#### Test the Load Balancer

- Go to Load Balancers → Click your ALB

- Copy the DNS name

- Paste in your browser:
  
      http://<your-load-balancer-DNS-name>

- You should see:
  
      Hello from EC2 behind a Load Balancer!
![image alt](https://github.com/Juniorklb/Create-a-Load-Balancer-for-a-single-EC2-instance/blob/d64548ec9f3da500d2e5e86fd5d4cc8ba2b77125/Images/ALBloadbalancer.PNG)
## Quick Troubleshooting Checklist

#### Is the Load Balancer in the "Active" state?
- Go to EC2 → Load Balancers

Check the State — it should say active.

#### Is your EC2 instance showing as “healthy” in the Target Group?
- Go to Target Groups → click your group (EC2-Web-TG)

Click the Targets tab

Look at the Health Status

- Should be healthy

    - ❌ If unhealthy, that’s the issue. Fix is in Step 4 below.

#### Is port 80 open in the EC2’s Security Group?
  
- Go to EC2 → click your instance → scroll down to Security groups

- Click the group → check Inbound rules

- You should see HTTP (port 80) allowed from 0.0.0.0/0
  
#### 4. Did the EC2 instance install the web server properly?
- Try connecting via SSH:
  
      ssh -i your-key.pem ec2-user@your-ec2-public-ip
- Then run:

      curl localhost
- If you don’t see the Hello from EC2... message, then the Apache server didn’t install or start.

- Run this to fix:

      sudo yum install -y httpd
      sudo systemctl start httpd
      sudo systemctl enable httpd
      echo "<h1>Hello from EC2 behind a Load Balancer!</h1>" | sudo tee /var/www/html/index.html
#### Health Check Path

- Go to Target Group → Health Checks

- Make sure the path is set to /, not something like /health unless you configured it

## 📌 Conclusion

- This setup demonstrates how to expose a web application using an AWS ALB even with a single backend instance. It's a solid base for future enhancements like:

- HTTPS with AWS Certificate Manager

- Auto Scaling groups

- CloudWatch monitoring and logging

<h2>👥 Connect with me:</h2>

<p align="left">
  <a href="https://www.linkedin.com/in/junior-kalomba-10002a18a/" target="_blank">
    <img src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="junior-kalomba-10002a18a" height="30" width="40"/>  
    
  </a>
  <a href="mailto:jrkalomba@gmail.com" target="_blank">
  <img  src="https://upload.wikimedia.org/wikipedia/commons/4/4e/Mail_%28iOS%29.svg" alt="Email" height="30" width="40"/>
</a>
</p>


[linkedin]: https://linkedin.com/in/Juniorkalomba

**🔗 Feel free to contribute or suggest improvements!**
