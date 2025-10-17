# AWS-ELB-ASG-PROJECT
This project demonstrates how to deploy a highly available and scalable web application on AWS using Elastic Load Balancing (ELB) and Auto Scaling Groups (ASG). It highlights how these two services work together to ensure fault tolerance, resilience, and cost efficiency in cloud environments.

# **Elastic Load Balancer (ELB) & Auto Scaling Group (ASG) Project**

## **1. Notes on Load Balancer Types**

AWS provides three main types of **Elastic Load Balancers (ELB)**, each designed for specific use cases and traffic handling needs.

### **Application Load Balancer (ALB)**
- **Layer:** Application Layer (Layer 7)
- **Description:** The ALB is designed for **HTTP and HTTPS traffic**. It provides advanced routing features such as **path-based** and **host-based routing**, **SSL termination**, and seamless integration with **containerized or microservice architectures**.
- **Example:** Used by **streaming platforms like Netflix** to route user requests intelligently between different microservices — for example, directing `/login` traffic to the authentication service and `/movies` traffic to the content service.

### **Network Load Balancer (NLB)**
- **Layer:** Transport Layer (Layer 4)
- **Description:** The NLB is optimized for **extreme performance** and **low latency**. It can handle **TCP, UDP, and TLS traffic**, managing **millions of requests per second** while supporting **static IP addresses**.
- **Example:** Used by **stock trading platforms like Robinhood** to handle millions of real-time trading requests with ultra-low latency and high throughput.

### **Gateway Load Balancer (GLB)**
- **Layer:** Network Layer (Layer 3)
- **Description:** The GLB enables the deployment, scaling, and management of **third-party virtual appliances** such as firewalls, intrusion detection systems (IDS), and deep packet inspection tools. It uses the **GENEVE protocol** for seamless traffic integration and enhances **security and compliance**.
- **Example:** Used by **financial institutions like J.P. Morgan Chase** to deploy and manage fleets of virtual firewalls and intrusion detection systems that inspect and filter all incoming traffic for security and compliance.

---

## **2. Steps Followed to Configure ALB and ASG**

### **Step 1: Launch Two EC2 Instances (Instance A & Instance B)**
- Launched two **Amazon Linux t3.micro** instances in different **Availability Zones** within the same **VPC**.  
- Installed **Apache** on both instances and created custom web pages:
  - **Instance A:** “Hello from Server A”  
  - **Instance B:** “Hello from Server B”

### **Step 2: Create an Application Load Balancer (ALB)**
- Navigated to **EC2 → Load Balancers → Create Load Balancer → Application Load Balancer**.  
- Selected **HTTP (port 80)** and enabled **two subnets** across different Availability Zones for **high availability**.  
- Created a **Target Group (HTTP:80)** and **registered both EC2 instances**.  
- Accessed the **ALB DNS name** and confirmed that traffic alternated between **Instance A** and **Instance B** after refreshing multiple times.

### **Step 3: Create a Launch Template**
- Created a **Launch Template** using **Amazon Linux AMI** and **t3.micro** instance type.  
- Configured the **security group** to allow inbound traffic on ports **22 (SSH)** and **80 (HTTP)**.

### **Step 4: Configure the Auto Scaling Group (ASG)**
- Used the **Launch Template** to create an **Auto Scaling Group** with:
  - **Minimum:** 1  
  - **Desired:** 1  
  - **Maximum:** 3  
- Attached the **ASG** to the **ALB Target Group** for automatic registration of new instances.

### **Step 5: Test Auto Scaling Behavior**
- Stopped one of the EC2 instances to simulate a **failure**.  
- The **ASG** detected the issue and automatically launched a **replacement instance** to maintain the desired capacity.  
- The new instance was registered under the **ALB Target Group**, restoring full availability.

---

## **3. Shared Responsibility Model for ELB and ASG**

The **AWS Shared Responsibility Model** defines the division of security and management duties between **AWS** and the **customer**.

### **AWS Responsibilities**
AWS is responsible for securing the **underlying infrastructure**, which includes:
- Physical servers, networking, and storage.
- The software that powers services like **EC2**, **ELB**, and **ASG**.
- Maintaining the **availability**, **scalability**, and **physical security** of data centers and networking components.

### **Customer Responsibilities**
As a customer, I am responsible for securing and configuring resources within my AWS environment, including:
- Setting up proper **security groups**, **firewall rules**, and **IAM permissions**.  
- Managing **patches**, **software updates**, and **application-level security**.  
- Ensuring **scaling policies** and **health checks** are correctly configured.  
- Protecting data through **encryption**, **monitoring**, and **access management**.

---

** Reflection **  
- Through this project, I gained hands-on experience with load balancing strategies and how ALB, NLB, and GLB serve different use cases. Implementing the Auto Scaling Group taught me how AWS maintains availability and fault tolerance automatically.I also developed a practical understanding of the Shared Responsibility Model, appreciating the clear boundaries between AWS-managed infrastructure and customer-managed resources.
