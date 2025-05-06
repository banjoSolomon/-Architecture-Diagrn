Architecture Diagram Readme: Multi-Tier Application on AWS with Hybrid Connectivity
This document provides a high-level overview of the architecture depicted in the diagram. It outlines the different components, their relationships, and the overall flow of traffic and data within the system.

I. Overview

The diagram illustrates a multi-tier application hosted on Amazon Web Services (AWS) with a hybrid connectivity to an on-premises environment. The application appears to be composed of web, application, and data layers, leveraging various AWS services for scalability, security, and management. It also establishes a secure connection to resources residing in an on-premises data center.

II. Key Components and Layers

A. Public Facing Layer (within the Primary VPC)

User: An external user initiates a request to the application.
AWS WAF (Web Application Firewall): Acts as the first line of defense, protecting the application from common web exploits and bot traffic.
Amazon Route 53 (Implied): While not explicitly shown connecting to WAF, it's likely used to resolve the application's domain name to the WAF endpoint, providing DNS resolution.
Application Load Balancer (ALB): Distributes incoming HTTP/HTTPS traffic across multiple application instances for high availability and scalability. It likely handles SSL/TLS termination.
EC2 Instances (Amplify, Gatekeeper): These represent the web and potentially API gateway layers of the application.
Amplify: Suggests a component related to frontend hosting or mobile backend services.
Gatekeeper: Likely an API gateway or authorization service controlling access to backend services.
Security Group (Public Subnet): Controls inbound and outbound traffic at the instance level for resources in the public subnet.
Public Subnet: Contains the internet-facing components of the application.
Amazon S3: Used for storing static assets, user uploads, or other data.
B. Application Layer (within the Primary VPC)

Private Subnet: Contains the backend application logic and services, isolated from direct internet access.
Security Group (Private Subnet): Controls inbound and outbound traffic at the instance level for resources in the private subnet.
EC2 Instances (IDM, Wallet, Payment, Terminal, Notification): These represent various microservices or components of the application logic.
IDM (Identity Management): Handles user authentication and authorization.
Wallet: Manages user funds or credits.
Payment: Processes financial transactions.
Terminal: Potentially provides an interface for specific operations.
Notification: Handles sending alerts or updates to users.
AWS Secrets Manager: Securely stores and manages sensitive information like database credentials and API keys used by the application components.
C. Data Layer (within the Primary VPC)

Private Subnet (Separate): Contains the data stores, further isolating them for security.
EC2 Instance (ECR - Amazon Elastic Container Registry): A container registry used to store and manage Docker container images for the application components.
EC2 Instance (Keycloak): An open-source identity and access management solution, potentially used for more complex authentication and authorization scenarios.
MongoDB: A NoSQL database used by the application.
PostgreSQL: A relational database used by the application.
D. Secondary VPC (Potentially for specific zones or environments)

Private Subnet: Contains resources isolated within this secondary VPC.
Security Group (Private Subnet): Controls traffic for resources in this secondary VPC.
EC2 Instances (Zone, UP, ISW, Hattari): These represent application components or services within this secondary VPC. The naming suggests they might correspond to different geographical zones or specific functionalities.
Amazon EBS (Elastic Block Store): Provides persistent block storage volumes for the EC2 instances in this VPC.
E. Hybrid Connectivity (to On-Premise)

AWS VPN Connection: Establishes a secure, encrypted tunnel over the public internet between the AWS environment (specifically the Secondary VPC) and the on-premises data center.
On-Premise Environment: Represents the company's physical infrastructure.
Servers (Zone, ISW, UP, Hattari): These likely correspond to the EC2 instances with the same names in the Secondary VPC, indicating a hybrid deployment where some parts of the application run on-premises.
III. Traffic and Data Flow

External users access the application through a domain name resolved by Route 53 (implied) to the AWS WAF.
WAF inspects the incoming traffic and allows legitimate requests to pass to the ALB.
The ALB distributes traffic to the web/API gateway layer (Amplify, Gatekeeper) in the public subnet.
The web/API gateway interacts with backend microservices (IDM, Wallet, Payment, etc.) in the private subnet.
These microservices retrieve configuration and secrets from AWS Secrets Manager and interact with the data stores (MongoDB, PostgreSQL) in their dedicated private subnet.
Static assets and other data might be stored and retrieved from Amazon S3.
Container images for the application are pulled from Amazon ECR.
Components in the Secondary VPC communicate with the On-Premise environment via the secure AWS VPN connection. This suggests a hybrid deployment where certain functionalities or data reside on-premises.
IV. Security Considerations

WAF: Protects against web-based attacks.
Security Groups: Control network access at the instance level, enforcing network segmentation.
Private Subnets: Isolate backend services and data stores from direct internet exposure.
AWS Secrets Manager: Securely manages sensitive credentials.
VPN Connection: Encrypts communication between AWS and the on-premises environment.
IAM (Implied): AWS Identity and Access Management (IAM) is crucial for controlling access to all AWS resources based on the principle of least privilege.
V. Scalability and Availability

ALB: Provides load balancing and distribution for the web layer.
Auto Scaling (Implied): While not explicitly shown, the use of an ALB strongly suggests that the EC2 instances are part of Auto Scaling groups to handle varying load and ensure high availability.
Multi-AZ Deployment (Implied): The presence of multiple subnets within a "Region" suggests a deployment across multiple Availability Zones (AZs) for increased resilience.
Managed Services (ALB, S3, Secrets Manager): Leverage the scalability and availability inherent in AWS managed services.
VI. Conclusion

This architecture diagram illustrates a robust and scalable multi-tier application on AWS with secure hybrid connectivity. It leverages various AWS services for security, management, and data storage. The separation of layers into public and private subnets, along with the use of security groups and Secrets Manager, highlights a focus on security. The ALB and implied Auto Scaling indicate a design for high availability and scalability. The VPN connection enables seamless and secure integration with the company's on-premises infrastructure.

This readme provides a general understanding of the architecture. Further details about specific configurations, instance types, network routing, and application logic would require a more detailed review of the underlying infrastructure and application code
