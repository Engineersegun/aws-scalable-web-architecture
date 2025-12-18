#!/bin/bash
# Update and install Apache
sudo yum update -y
sudo yum install -y httpd
# Start the service
sudo systemctl start httpd
sudo systemctl enable httpd
# Create a test page (This is what the Health Check looks for)
echo "<h1>Success! Server is Healthy</h1>" > /var/www/html/index.html
