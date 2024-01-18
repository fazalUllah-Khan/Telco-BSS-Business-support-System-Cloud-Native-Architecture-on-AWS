# Telco-BSS-Business-support-System-Cloud-Native-Architecture-on-AWS
In this repo, I have describe Telco BSS AWS base Cloud Native architecture with help of references

Business Support Systems (BSS) are a set of software components and functions interconnected together to grant the monetization of the communication service providers (CSPs), or simply the operators. BSS are the backbone of a service provider’s customer-facing strategy. BSS encompasses the spectrum from marketing, shopping, ordering, charging, taxation, invoicing, payments collection, dunning, and ultimately financial reporting. There are four primary domains: product management, order management, revenue management, and customer management.

The telco network is mainly divided into two main big block the OSS (Operation support system) and BSS(Business support system). In this article we are focusing on BSS and it infra structure. Below snap will show the end to end flow diagram of CRM and BSS system and how is communication.

![image](https://github.com/fazalUllah-Khan/Telco-BSS-Business-support-System-Cloud-Native-Architecture-on-AWS/assets/148821704/8cf58a0a-6201-4b30-a591-834bb55ff78d)

CRM system processes are very important to CSPs to deliver consistently superior customer experience. They also need to come up with a unified product catalog for the customer regardless of geography and market segment, and maintain a unified master database.

In this article for BSS infrastrucre implemenation on AWS I am taking refence architecture of Amdocs. Amdocs CES21 is a 5G native integrated BSS operations support system (OSS) suite. It is a cloud-native, open, and modular suite that supports many of the world’s top CSPs on their digital and 5G journeys.

Although there are multiple options for deploying the Digital Brand Experience Suite into an AWS environment, the Archtural diagrams reference in this repo primarily focus on deploying into a multi-tenant SaaS architecture. 

# Technical architecture 

# Common deployment architecture
  This diagram depicts the main resources deployed for the Digital Brand Experience Suite. The application is using the same AWS services, regardless of the nature of the cloud deployment (for example, SaaS vs. non-SaaS).

  ![image](https://github.com/fazalUllah-Khan/Telco-BSS-Business-support-System-Cloud-Native-Architecture-on-AWS/assets/148821704/66a4edd1-1027-4355-860d-ef9a681d4bc4)

In this archtitecture Amazon Virtual Private Cloud (VPC) used which is divided into three subnets, organizing the access, compute, and storage resources needed for the Digital Brand Experience Suite. All of these subnets are private—access is handled by a demilitarized zone (DMZ) such as the inbound services VPC of the SaaS offering.

customers subnet provides access and load balancing capabilities into the VPC and is the entry point from the DMZ (for example, inbound services VPC through AWS PrivateLink for customers interface). While BSS subnet holds the primary computing resources, comprising different Auto Scaling groups which is managed by Amazon Elastic Kubernetes Service (Amazon EKS).

More details can be found on : *https://docs.aws.amazon.com/whitepapers/latest/amdocs-digital-brand-experience-platform/digital-brand-experience-suite-deployment-architecture.html*  

- Bill Processing (BP) batch nodes using AWS Fargate, which comprises of scheduled and task-based applications like the billing processor and invoice generator. While also Amazon Elastic File System (Amazon EFS) instance is deployed within this subnet, that is used by the various processes of the billing application (for example, usage files which are shared between the different usage file rating processes).
-  Several processes used here serverless computing resources. For example, the payment gateway and web UI backend are implemented through AWS Lambda functions for event-based handling. Serverless computing on AWS—such as AWS Lambda—includes automatic scaling, built-in high availability, and a pay-for-value billing model.
-  The BIL(Business Integration Layer) database and BSS database use Amazon Aurora databases for commerce (shopping cart) and billing, respectively.
-  For high availibility Suite can be deployed in multiple Availability Zones (AZs) configuration below iagram for reference,.

  ![image](https://github.com/fazalUllah-Khan/Telco-BSS-Business-support-System-Cloud-Native-Architecture-on-AWS/assets/148821704/2220e54b-e144-4612-9b03-ef117d0f95b2)

-  For scalability ,  Auto Scaling groups of with various container types an be deployed. Auto Scaling groups can be configured with different scaling models, either scaling up or down based on events, system measurements, or a preset. schedule.
- For security, *access management* with IAM roles and based on who needs access to what. As a best practice, customers could assign permissions at IAM group role level to access applications in the specific VPCs, and never grant privileges beyond the minimum required for a user or group to fulfill their job requirements. *Data at rest* will be encrypted on the storage volume level (using AWS built-in capabilities) as well as on the database level (on configurable PII fields). Further , The web UIs access will be encrypted with SSL encryption (HTTPS). The solution API layer access will be encrypted with SSL encryption (HTTPS) to ensure *secure date at transit*

Reference:
- https://docs.aws.amazon.com/whitepapers/latest/amdocs-digital-brand-experience-platform/digital-brand-experience-suite-deployment-architecture.html
- https://docs.aws.amazon.com/whitepapers/latest/amdocs-digital-brand-experience-platform/aws-well-architected-framework.html
- https://docs.aws.amazon.com/whitepapers/latest/amdocs-digital-brand-experience-platform/amdocs-digital-brand-experience-suite-overview.html
- https://docs.aws.amazon.com/whitepapers/latest/amdocs-digital-brand-experience-platform/further-reading.html
