# AWS Route 53 High Availability Web Application

## Project Overview

This project demonstrates the deployment of a highly available web application on AWS using EC2, Application Load Balancer (ALB), and Route 53. The infrastructure is designed to distribute traffic across multiple Availability Zones and provide DNS-based routing capabilities such as Failover Routing, Latency-Based Routing, and Geolocation Routing.

## Architecture

```text
User
   │
   ▼
Route 53
   │
   ▼
Application Load Balancer
   │
   ├── EC2 Instance 1 (us-east-1a)
   │      └── Nginx Web Server
   │
   └── EC2 Instance 2 (us-east-1b)
          └── Nginx Web Server
```

## Technologies Used

* Amazon EC2
* Amazon VPC
* Public Subnets
* Application Load Balancer (ALB)
* Route 53
* Nginx
* Security Groups
* Target Groups
* Health Checks

## Features

* Multi-AZ deployment for High Availability
* Application Load Balancer for traffic distribution
* Route 53 DNS management
* Health monitoring using Target Groups
* Failover Routing
* Latency-Based Routing
* Geolocation Routing
* Custom Domain Integration

## AWS Services Used

### Amazon EC2

Two EC2 instances are launched in separate Availability Zones to host the web application.

### Application Load Balancer

The ALB distributes incoming HTTP requests across healthy EC2 instances.

### Target Group

The Target Group performs health checks and forwards traffic only to healthy instances.

### Route 53

Route 53 manages DNS records and provides advanced routing policies.

## Infrastructure Setup

### 1. Create VPC

* Create a custom VPC
* CIDR Block: 10.0.0.0/16

### 2. Create Public Subnets

| Subnet          | Availability Zone |
| --------------- | ----------------- |
| Public-Subnet-A | us-east-1a        |
| Public-Subnet-B | us-east-1b        |

### 3. Configure Internet Connectivity

* Create Internet Gateway
* Attach Internet Gateway to VPC
* Update Route Tables

### 4. Launch EC2 Instances

Create two Amazon Linux 2023 EC2 instances:

* public_server1
* public_server2

### 5. Install Nginx

```bash
sudo dnf update -y
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 6. Configure Web Pages

Server 1:

```html
<h1>Welcome From Server1</h1>
```

Server 2:

```html
<h1>Welcome From Server2</h1>
```

### 7. Create Target Group

Configuration:

* Target Type: Instances
* Protocol: HTTP
* Port: 80

Register both EC2 instances.

### 8. Create Application Load Balancer

Configuration:

* Internet Facing
* IPv4
* Multiple Availability Zones
* Attach Target Group

### 9. Configure Route 53

Create:

* Public Hosted Zone
* Alias A Record
* Route traffic to ALB

### 10. Configure Routing Policies

#### Simple Routing

Routes traffic to a single endpoint.

#### Failover Routing

Automatically redirects traffic to a secondary endpoint when the primary endpoint becomes unavailable.

#### Latency-Based Routing

Routes users to the AWS Region with the lowest latency.

#### Geolocation Routing

Routes users based on their geographic location.

## Health Checks

Target Group health checks are configured using:

* Protocol: HTTP
* Port: Traffic Port
* Path: /
* Success Code: 200

Only healthy instances receive traffic from the Load Balancer.

## Security Configuration

Inbound Rules:

| Type | Port |
| ---- | ---- |
| SSH  | 22   |
| HTTP | 80   |

Outbound Rules:

* Allow All Traffic

## Project Workflow

1. User accesses application domain.
2. Route 53 resolves DNS request.
3. Route 53 forwards request to ALB.
4. ALB checks healthy targets.
5. Traffic is distributed to healthy EC2 instances.
6. Nginx serves the web application.

## Results

* Successfully deployed a highly available web application on AWS.
* Implemented load balancing across multiple Availability Zones.
* Configured Route 53 DNS routing policies.
* Enabled health monitoring and traffic distribution.
* Improved application availability and fault tolerance.

## Learning Outcomes

* AWS Networking Fundamentals
* EC2 Instance Management
* Application Load Balancer Configuration
* Route 53 DNS Management
* Health Checks and Monitoring
* High Availability Architecture Design
* Cloud Infrastructure Deployment

## Author

Kedarling Ashok Kanade

## License

This project is created for educational and academic purposes.
