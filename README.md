Scalable Web App using EC2,ALB,and Auto Scling for high availabity
<img width="1024" height="881" alt="AWS Auto scaling" src="https://github.com/user-attachments/assets/b69d0066-b290-450d-b210-308edb73160b" />
<img width="1366" height="768" alt="scaling test-png" src="https://github.com/user-attachments/assets/49a25185-8efa-4498-9c17-ad93f8194e41" />

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
<img width="1366" height="768" alt="scaling test hp" src="https://github.com/user-attachments/assets/44086d0c-f334-4fae-be9b-f20d39c3ac88" />
<img width="1366" height="768" alt="scaling pngg" src="https://github.com/user-attachments/assets/926b86db-4ae2-4b4d-9343-d1376ac7e01b" />
<img width="1366" height="768" alt="scaling (2)" src="https://github.com/user-attachments/assets/fc649214-9e04-4aaa-9144-711073198c1a" />
<img width="1366" height="768" alt="scaling-test" src="https://github.com/user-attachments/assets/6b1ac1e1-c6bf-489a-b9a7-8c4c3cc462ab" />
<img width="1366" height="768" alt="scaling" src="https://github.com/user-attachments/assets/84029203-53e7-4e32-aad2-635af18d18cb" />
<img width="1366" height="768" alt="scaling test" src="https://github.com/user-attachments/assets/8a140e55-a177-4a2a-a9b9-7dc11dc965c1" />
<img width="1366" height="768" alt="scaling test " src="https://github.com/user-attachments/assets/3c3ef89f-99bb-4f82-ae7e-e26f5cafb160" />
<img width="1366" height="768" alt="scaling-test png" src="https://github.com/user-attachments/assets/128622e9-bf21-4846-8c16-ded5110b1360" />



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
