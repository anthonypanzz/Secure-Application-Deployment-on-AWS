# Secure-Application-Deployment-on-AWS

<img width="911" height="767" alt="image" src="https://github.com/user-attachments/assets/cd30f203-5b82-4724-ad5d-92104cba419a" />


Web applications can be launched with a focus on speed rather than security.

The outcome of this is leaving key cloud resources exposed directly to the public internet.

Web apps face significant risks, including unauthorized access, data breaches, and compliance issues. 

Utilizing the Security Pillar from the AWS Well-Architected Framework:

As an AWS Solutions Architect, it is crucial to work with customers and to take a vulnerable cloud environment and redesign it into a secure, production-ready architecture using AWS-native services and best practices.

A vulnerable infrastructure example includes:

- EC2 instance in public subnet with unrestricted SSH and HTTP access, SSH access from anywhere, and no IAM role-based access controls.
- Relational Database Service (RDS) is exposed directly to the internet, has weak authentication, no encryption, and overly permissive access.
- S3 bucket configured for public access, no encryption for data at rest or in transit, and publicly accessible file storage.

---

# Redesigning the Architecture with Security Best Practices (Security Pillar):

Network Security Architecture:
* Public tier: Only hosts components that face the internet, such as load balancers and bastion hosts.
* Private tier: Separates application servers and databases from direct access to the internet.
* Multi-AZ deployment: Ensures consistent availability and resilience against failures.
* Controlled internet access: Public subnets can access the internet directly through an Internet Gateway.
* Secure outbound connectivity: Private subnets can reach the internet via a NAT Gateway for updates.
* No inbound internet access: Private resources cannot be accessed directly via the internet.
<img width="1892" height="554" alt="image" src="https://github.com/user-attachments/assets/08047869-a599-4fe1-9ab0-a3ff42416217" />
<img width="1899" height="473" alt="image" src="https://github.com/user-attachments/assets/6870f26c-2641-4c72-9fa7-7f00b26a8a1e" />


---

Private EC2 + ALB + Bastion Access:

* To securely run the application backend, the EC2 instance is inside a private subnet.
* Since EC2 won’t have a public IP, use a Bastion Host in the public subnet for administrative access. 
- Bastion Host allows you to securely access that instance from a public subnet. 
* Application traffic will be routed securely through an Application Load Balancer (ALB) hosted in the public subnet. 
- Application Load Balancer is in a public subnet, and it securely forwards traffic to EC2 inside the private subnet.
<img width="1511" height="380" alt="image" src="https://github.com/user-attachments/assets/40505f90-b611-4082-bb0b-31ff41336ee3" />
<img width="1911" height="444" alt="image" src="https://github.com/user-attachments/assets/05e0130b-4290-4639-a67e-c9cfa1e7564f" />
<img width="1385" height="552" alt="image" src="https://github.com/user-attachments/assets/9eda3f62-c947-4bec-b34d-69b09c527100" />

---


Private RDS and S3:

* This design focuses on securing the data layer by deploying a private RDS MySQL database and setting up a private S3 bucket for static assets. 
* Both resources follow the principle of least privilege. 
* RDS is only accessible from private EC2, and S3 assets are not publicly accessible.
<img width="1889" height="782" alt="image" src="https://github.com/user-attachments/assets/cab43612-98c7-4d15-8ecc-04fdb4a3e993" />
<img width="1213" height="429" alt="image" src="https://github.com/user-attachments/assets/8a17c9de-5588-4753-8393-2533da555e84" />
<img width="1887" height="827" alt="image" src="https://github.com/user-attachments/assets/13c4fd96-58ea-47e0-a659-672a6a1be609" />

---


AWS Web Application Firewall:

* This design strengthens application-layer security.
* A WAF identifies and prevents harmful HTTP requests from reaching the application, including threats like SQL injection, cross-site scripting attacks, and automated bot traffic.
<img width="1171" height="752" alt="image" src="https://github.com/user-attachments/assets/c69c3b07-54cc-4a2d-ad93-c1ae529199a3" />
<img width="1767" height="640" alt="image" src="https://github.com/user-attachments/assets/dad24a66-dceb-4da4-8d2a-11ff5dfdc96e" />


