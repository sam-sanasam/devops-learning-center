**Scenario-1:** <br>
### **Use Case Scenario: Deploying a Secure, Scalable Web Application in AWS**

#### **Background**
An organization is deploying a customer-facing web application on AWS. This application has two main components:
1. **Frontend Web Server**: Hosted on EC2 instances to serve static content and handle user requests.
2. **Backend Database**: An RDS instance for storing and managing user data with high availability.

To ensure security, scalability, and compliance, the infrastructure needs to follow best practices and adhere to tagging standards for resource management and cost tracking.

---

### **Infrastructure Requirements**
1. **Networking:**
   - A **VPC** to isolate resources and provide better security.
   - **Public Subnets** for EC2 instances to allow user access over the internet.
   - **Private Subnets** for the RDS instance, inaccessible directly from the internet.
   - Multi-AZ configuration for RDS to ensure high availability and disaster recovery.

2. **Compute Resources:**
   - **EC2 Instances** in the public subnet, behind a load balancer, to handle incoming traffic.

3. **Database:**
   - **Amazon RDS** (MySQL) in the private subnet for secure, reliable, and scalable database services.

4. **Security Groups:**
   - Restrict traffic to EC2 (allow SSH and HTTP/HTTPS).
   - Allow only EC2 instances to connect to the RDS database.

---

### **Compliance Requirements**
1. **Resource Tagging:**
   - All resources must have the following tags:
     - **Environment**: E.g., `Production`, `Staging`.
     - **Owner**: E.g., `DevOps Team`.

2. **Monitoring:**
   - Use **AWS Config Rules** to ensure tagging compliance. Resources missing tags or with incorrect tags are flagged as non-compliant.
   - AWS Config evaluates compliance for the following resource types:
     - EC2 Instances
     - RDS Instances
     - VPC Subnets

---

### **Detailed Workflow**

1. **Network Setup:**
   - A **VPC** is created with CIDR block `10.0.0.0/16`.
   - **Public Subnets** in two availability zones (AZs) for EC2 instances.
   - **Private Subnets** in the same AZs for RDS.

2. **Compute Layer:**
   - Launch **EC2 instances** in the public subnets with an application stack (e.g., NGINX or Apache) to serve web requests.
   - Attach an **Elastic Load Balancer (ELB)** in front of the EC2 instances to distribute incoming traffic.

3. **Database Layer:**
   - Deploy an **Amazon RDS instance** in private subnets with Multi-AZ for high availability.
   - Use a **security group** to allow only traffic from the EC2 instances to connect to the RDS instance on port `3306`.

4. **Security:**
   - Use **security groups** to:
     - Allow SSH access to EC2 from trusted IPs.
     - Restrict database access to EC2 instances only.
   - Apply **IAM roles** to EC2 instances for secure access to other AWS services (e.g., S3 for storing logs).

5. **Compliance:**
   - Tag all resources with:
     - `Environment = Production`
     - `Owner = DevOps Team`
   - Enable **AWS Config** with a rule (`REQUIRED_TAGS`) to monitor resource tagging compliance.

---

### **Scenario Walkthrough**

#### **1. Web Application Deployment**
- A customer visits the website, and their request is routed through the ELB.
- The ELB forwards the request to one of the EC2 instances in the public subnet.
- The EC2 instance processes the request and interacts with the RDS database in the private subnet to fetch or update data.

#### **2. High Availability and Disaster Recovery**
- The EC2 instances are deployed in multiple availability zones for fault tolerance.
- The RDS database is configured with Multi-AZ, ensuring an automatic failover to a secondary instance in case of an outage.

#### **3. Compliance Enforcement**
- AWS Config monitors the resources for proper tagging (e.g., `Environment`, `Owner`).
- If an untagged resource is detected (e.g., a new EC2 instance), AWS Config flags it as **non-compliant**.
- Notifications are sent via Amazon SNS, alerting the DevOps team.

---

### **Challenges Addressed**
1. **Security:**
   - Private subnets prevent direct access to the RDS database from the internet.
   - Security groups ensure only necessary traffic flows between EC2 and RDS.

2. **Scalability:**
   - Public subnets and ELB enable horizontal scaling of EC2 instances.
   - RDS handles high availability and disaster recovery with Multi-AZ deployment.

3. **Compliance:**
   - AWS Config ensures all resources are tagged for easy identification, cost management, and auditing.

---

### **Outcome**
- A secure, scalable, and compliant infrastructure for hosting a web application.
- Proactive monitoring of resource tagging ensures operational excellence and reduces governance risks.
