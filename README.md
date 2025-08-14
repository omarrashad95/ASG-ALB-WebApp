# High Availability Web Application on AWS

## 📜 Overview
This project describes a **high-availability web application** hosted on AWS, leveraging **Auto Scaling**, **Application Load Balancer (ALB)**, **IAM role-based access**, **CloudWatch monitoring**, and **SNS notifications**.  
The architecture ensures scalability, fault tolerance, and secure resource access for EC2 instances.

---

## 🏗 Architecture Components

### 1. **Application Load Balancer (ALB)**
- Distributes incoming HTTP/HTTPS traffic across multiple EC2 instances in different Availability Zones.
- Enhances fault tolerance by routing traffic only to healthy targets.

### 2. **Auto Scaling Group (ASG)**
- Automatically adjusts the number of EC2 instances based on demand.
- Ensures that the right number of instances are running to handle the load.
- Launches instances from a **Launch Template** configured with an **IAM Role**.

### 3. **Amazon EC2 Instances**
- Hosts the web application.
- Each instance inherits permissions via an **IAM role** for secure interaction with AWS services (e.g., CloudWatch, SNS).

### 4. **IAM Role for EC2**
- Provides **temporary credentials** to EC2 instances without storing keys locally.
- Grants least-privilege access to:
  - Publish metrics/logs to **CloudWatch**
  - Send notifications to **SNS**

### 5. **Amazon CloudWatch**
- Collects and monitors metrics (CPU usage, request counts, latency, etc.).
- Triggers alarms when predefined thresholds are breached.
- Integrates with SNS to send alerts to administrators.

### 6. **Amazon SNS (Simple Notification Service)**
- Delivers alerts from CloudWatch to email, SMS, or other endpoints.
- Ensures quick awareness and response to application or infrastructure issues.

---

## 🔄 Workflow

1. **Traffic Ingress**:  
   Users send requests to the **Application Load Balancer (ALB)**.

2. **Traffic Distribution**:  
   ALB routes requests to healthy EC2 instances in the **Auto Scaling Group**.

3. **Application Execution**:  
   EC2 instances process requests.  
   Instances have an **IAM Role** attached via the Launch Template for secure AWS access.

4. **Monitoring & Scaling**:  
   - **CloudWatch** gathers metrics from EC2 instances.
   - Based on scaling policies, the **ASG** adds/removes instances automatically.
   - **CloudWatch Alarms** detect abnormal behavior and trigger **SNS notifications**.

5. **Alerting**:  
   **SNS** sends notifications to predefined subscribers for immediate action.

---

## 📊 Architecture Diagram

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d039928a-ec21-4905-8707-85e6ec2988ed" />


---

## 🔐 Security Considerations
- **Least privilege** IAM policy for EC2 role.
- No hardcoded AWS credentials; all access via IAM roles.
- ALB configured for HTTPS using AWS Certificate Manager.
- Security groups restrict inbound traffic to required ports only.

---

## ⚙ Deployment Steps

1. **Create IAM Role**  
   - Service: EC2  
   - Permissions: CloudWatch access, SNS publish, custom application permissions.

2. **Create Launch Template**  
   - AMI with application pre-installed.  
   - Select the IAM instance profile.  
   - Configure security groups and key pair.

3. **Create Auto Scaling Group**  
   - Link to the Launch Template.  
   - Choose multiple Availability Zones.  
   - Set desired, min, and max capacity.

4. **Set Up Application Load Balancer**  
   - Configure listeners and target group (linked to ASG).

5. **Set Up CloudWatch Alarms**  
   - Create alarms for CPU, request count, and latency.  
   - Link alarms to SNS topics.

6. **Create SNS Topic**  
   - Subscribe via email/SMS for alerts.

---

## 📬 Notifications
When CloudWatch detects a threshold breach:
- **CloudWatch Alarm → SNS → Email/SMS**  
Admins can take action based on the alert.

---

## ✅ Benefits of This Architecture
- **High Availability**: Instances distributed across multiple AZs.
- **Scalability**: ASG scales instances up/down automatically.
- **Security**: IAM roles for EC2, least privilege access.
- **Automation**: Monitoring, scaling, and notifications are automated.

---
