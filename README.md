#!/bin/bash
# Update and install Apache
sudo yum update -y
sudo yum install -y httpd
# Start the service
sudo systemctl start httpd
sudo systemctl enable httpd
# Create a test page (This is what the Health Check looks for)
echo "<h1>Success! Server is Healthy</h1>" > /var/www/html/index.html

## 📊 Project Validation & Evidence

### 1. Application Load Balancer Routing
Successfully accessed the web application via the ALB DNS name, confirming traffic is being distributed to healthy instances.
![ALB Routing Proof](scaling%20test%20hp.png)

### 2. Multi-AZ Deployment Architecture
Verified that instances are running across different Availability Zones (1a, 1d, 1e) to ensure fault tolerance and high availability.
![Architecture Deployment](scaling.png)

### 3. Auto Scaling & Monitoring
Configured CloudWatch Alarms to monitor CPU utilization and manage the Auto Scaling Group lifecycle (Scale-out/Scale-in).
![Auto Scaling Monitoring](scaling%20(2).png)
