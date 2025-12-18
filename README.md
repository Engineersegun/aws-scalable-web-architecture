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

🛠️ Technical Challenges & Solutions
1. Automating the Web Server Configuration
Challenge: Ensuring that every new instance launched by the Auto Scaling Group was identical and ready to serve traffic without manual intervention.

Solution: Leveraged EC2 User Data scripts to automate the yum update process and the installation of the Apache (httpd) web server during the initial boot sequence.

2. Achieving High Availability Across Zones
Challenge: Preventing a single point of failure if an entire AWS Data Center (Availability Zone) went offline.

Solution: Configured the Auto Scaling Group to distribute instances across three distinct subnets (us-east-1a, us-east-1d, and us-east-1e), ensuring the app remains online even during a regional outage.

3. Traffic Management and Health Checks
Challenge: Ensuring the Load Balancer did not send users to a "broken" or "booting" server.

Solution: Implemented ALB Health Checks on the Target Group. The ALB only routes traffic to instances that return a 200 OK status on the /index.html path.

4. Cost vs. Performance Optimization
Challenge: Balancing the need for performance during spikes with the need to save money when traffic is low.

Solution: Established a Target Tracking Scaling Policy. I set a CPU utilization threshold of 60%, allowing the system to scale out to 4 instances only when needed and scale back to 2 during quiet hours to minimize costs.
