# AWS-3-Tier-Application-Architecture-Implementation
Description:
This project demonstrates the design and implementation of a robust, secure, and scalable 3-tier application architecture on AWS. The architecture is structured into three distinct layers: web (presentation), application (logic), and database (data) layers. Each tier is isolated using AWS services and best practices to ensure security, scalability, and high availability.

Key Steps and Architecture Components:
1.VPC Creation:
Created a Virtual Private Cloud (VPC) with a 10.0.0.0/16 CIDR block to segment the network for the application.
The network is subdivided into six subnets across two availability zones for redundancy:
Public Subnets for internet-facing resources like the load balancer:
Public Subnet 1 – 10.0.1.0/24 (AZ-a)
Public Subnet 2 – 10.0.2.0/24 (AZ-b)
Private Subnets for application servers:
Private Subnet 1 – 10.0.3.0/24 (AZ-a)
Private Subnet 2 – 10.0.4.0/24 (AZ-b)
Database Subnets for the RDS (Relational Database Service) instances:
DB Subnet 1 – 10.0.5.0/24 (AZ-a)
DB Subnet 2 – 10.0.6.0/24 (AZ-b)
Routing & Internet Access:

Configured a public route table for the public subnets and attached an Internet Gateway (IGW) to allow internet traffic.
Set up a NAT Gateway in one of the public subnets to provide internet access to resources in private subnets without exposing them to direct internet traffic.
Defined private route tables for the application and database subnets, ensuring traffic can flow securely through the NAT Gateway.
Security Groups:

Configured security groups to control the traffic between the different layers of the architecture:
LoadBalancer-SG: Allows inbound HTTP (80) and HTTPS (443) traffic from any source (0.0.0.0/0).
App-Server-SG: Allows inbound HTTP (80) traffic only from the load balancer’s security group (LoadBalancer-SG).
DB-SG: Allows inbound MySQL traffic (port 3306) from the application server’s security group (App-Server-SG).
EC2 Instances & IAM Roles:

Launched EC2 instances in private subnets as application servers, associating them with the appropriate IAM role (EC2SSMAgent) for secure management via AWS Systems Manager.
Installed the HTTPD web server and necessary components (PHP, MySQL client) to serve the application content.
MariaDB server was also installed and configured to connect to the database hosted in RDS.
RDS & Database Configuration:

Created an RDS MySQL database with subnets spanning across two availability zones for high availability and failover support.
Configured the database with appropriate parameters and connected it to the application tier.
Used the MySQL client to create and manage the necessary databases.
Application Setup (WordPress):

Installed WordPress in the application server’s root directory.
Configured the wp-config.php file with the database credentials, including the database name, username, password, and endpoint to ensure proper integration.
Verified the application functionality by restarting the web service and ensuring connectivity to the database.
Load Balancer and Auto Scaling:

Set up an Application Load Balancer (ALB) in the public subnets to distribute traffic evenly across the application servers in private subnets.
Created a Target Group to register application servers and attached it to the load balancer.
Implemented Auto Scaling to automatically adjust the number of application instances based on demand, ensuring optimal resource utilization.
SSL Certificate & DNS Configuration:

Configured Route 53 as the DNS service and linked it to the application’s domain.
Requested an SSL certificate from AWS Certificate Manager and updated the load balancer listener to handle secure HTTPS traffic.
Monitoring & Notifications:

Integrated CloudWatch for monitoring key metrics and setting up alarms for critical thresholds.
Configured SNS (Simple Notification Service) for real-time alerts in case of performance issues or failures.
