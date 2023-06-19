---
title: Azure Solutions Architect Expert
tags: [Microsoft, Azure, Cheat Sheet]
style: fill
color: primary
description: My short notes when studying for the Azure Solutions Expert
---

## Introduction <!-- omit from toc --> 

Hello. Thanks for checking this out. These are my personal notes made when studying for the Azure Solutions Architect Expert certification. 
Over time I will append links to official documentation. 
If something does not look right, [please reach out](mailto:brian@waithaka.me).

---

The exam is divided into the following topics. Find up to date info [here](https://learn.microsoft.com/en-us/certifications/azure-solutions-architect/)

---

## Table of Contents <!-- omit from toc --> 


- [Design identity, governance, and monitoring solutions (25–30%)](#design-identity-governance-and-monitoring-solutions-2530)
  - [Design a solution for logging and monitoring](#design-a-solution-for-logging-and-monitoring)
    - [Log Analytics workspace](#log-analytics-workspace)
    - [Azure Monitor Agent (AMA)](#azure-monitor-agent-ama)
  - [Design authentication and authorization solutions](#design-authentication-and-authorization-solutions)
      - [Azure Policy Effect](#azure-policy-effect)
      - [Azure AD object and RBAC roles](#azure-ad-object-and-rbac-roles)
  - [Design governance](#design-governance)
  - [Design identities and access for applications](#design-identities-and-access-for-applications)
- [Design data storage solutions (25–30%)](#design-data-storage-solutions-2530)
  - [Design a data storage solution for relational data](#design-a-data-storage-solution-for-relational-data)
      - [General Purpose v2 (GPv2) storage account](#general-purpose-v2-gpv2-storage-account)
      - [Azure SQL Managed Instance](#azure-sql-managed-instance)
      - [Azure SQL Database Hyperscale](#azure-sql-database-hyperscale)
  - [Design data integration](#design-data-integration)
      - [Azure Synapse Link](#azure-synapse-link)
      - [Azure Data Lake Storage Gen2](#azure-data-lake-storage-gen2)
      - [Azure Data Factory](#azure-data-factory)
  - [Recommend a data storage solution](#recommend-a-data-storage-solution)
  - [Design a data storage solution for non-relational data](#design-a-data-storage-solution-for-non-relational-data)
      - [Dynamic Data Masking (DDM)](#dynamic-data-masking-ddm)
- [Design business continuity solutions (10–15%)](#design-business-continuity-solutions-1015)
  - [Design a solution for backup and disaster recovery](#design-a-solution-for-backup-and-disaster-recovery)
      - [Azure Site Recovery](#azure-site-recovery)
  - [Design for high availability](#design-for-high-availability)
      - [Azure Traffic Manager](#azure-traffic-manager)
      - [Azure Front Door](#azure-front-door)
- [Design infrastructure solutions (25–30%)](#design-infrastructure-solutions-2530)
  - [Design a compute solution](#design-a-compute-solution)
      - [Azure Logic Apps](#azure-logic-apps)
      - [Azure Functions](#azure-functions)
  - [Design an application architecture](#design-an-application-architecture)
      - [Authorization workflow](#authorization-workflow)
  - [Design migrations](#design-migrations)
  - [Design network solutions](#design-network-solutions)


---

## Design identity, governance, and monitoring solutions (25–30%) 

<br>

### Design a solution for logging and monitoring
- Design a log routing solution 
- Recommend an appropriate level of logging 
- Recommend monitoring tools for a solution 

You need to analyze the network traffic to identify whether packets are being allowed or denied to the virtual machines. – Use Azure Network Watcher IP Flow Verify, which allows you to detect traffic filtering issues at a VM level.

Azure Monitor Log Tables to query 
- Windows Event Logs – Event ,
-  Linux – Syslog

Activity logs are kept for 90 days. You can query for any range of dates, as long as the starting date isn't more than 90 days in the past. – Create log for all ARM deployments in RG/subscription

Use the Azure Monitor agent if you need to: Collect guest logs and metrics from any machine in Azure, in other clouds, or on-premises.
Use the Dependency agent if you need to: Use the Map feature VM insights or the Service Map solution.

Design an architecture to capture the creation of users and the assignment of roles , data stored in Cosmos DB – Azure AD Audit Log -> Azure Event Hubs (you export AD logs to an event hub where you can even selectively pick)-> Azure Functions (serverless function to read events from the hub and store in database) ->Cosmos DB

Resources to create to monitor all warning events in the System logs of 300 virtual machines.- Azure Log Analytics Workspace & Storage Account, Configuration on VMs -> AZ Monitor Agent

#### Log Analytics workspace 
Is a unique environment for log data from Azure Monitor and other Azure services, such as Microsoft Sentinel and Microsoft Defender for Cloud. Each workspace has its own data repository and configuration but might combine data from multiple services.

#### Azure Monitor Agent (AMA) 
Collects monitoring data from the guest operating system of Azure and hybrid virtual machines and delivers it to Azure Monitor for use by features, insights, and other services, such as Microsoft Sentinel and Microsoft Defender for Cloud.

A single diagnostic setting can define no more than one of each of the destinations. If you want to send data to more than one of a particular destination type (for example, two different Log Analytics workspaces), then create multiple settings.

Each resource can have up to 5 diagnostic settings.

Note: This diagnostic telemetry can be streamed to one of the following Azure resources for analysis. 
- Log Analytics workspace 
- Azure Event Hubs 
- Azure Storage

<br>

### Design authentication and authorization solutions 
- Recommend a solution for securing resources with role-based access control 
- Recommend an identity management solution 
- Recommend a solution for securing identities 

Users to only connect without authentication prompt – Azure AD App Registration since app will use Azure AD authentication 
Users to only access from company owned computers – Conditional access policy 

Azure App proxy for connecting without vpn and Enterprise App for SSO
Shared Access Signatures (SAS) allows for limited-time fine grained access control to resources. So you can generate URL, specify duration (for month of April) and disseminate URL to 10 team members. On May 1, the SAS token is automatically invalidated, denying team members continued access.

Azure API Management - "Requests to the logic apps from the developers must be limited to lower rates than the requests from the users at Contoso." - This requirement can be achieved using the 'Limit call rate by key' feature of the API management., also supports OAuth2 authentication. 

Management groups can't span AAD tenant, so you need 2 management groups. Blueprints definition can be saved within management group which, in turn, means you need 2 blueprint definitions. Blueprint assignments are at subscription level, therefore you need 4.

##### Azure Policy Effect 
Modify is used to add, update, or remove properties or tags on a subscription or resource during creation or update. A common example is updating tags on resources such as costCenter. Existing non-compliant resources can be remediated with a remediation task. A single Modify rule can have any number of operations. Policy assignments with effect set as Modify require a managed identity to do remediation.

The following effects are deprecated: EnforceOPAConstraint EnforceRegoPolicy Append is used to add additional fields to the requested resource during creation or update.

##### Azure AD object and RBAC roles 
Is a managed identity with contributor role	
System-assigned managed identities provide a way for **Azure Functions** to authenticate to other Azure services, such as Activity Logs, without the need for storing or managing secrets. This approach minimizes administrative effort because the identity is tied directly to the Azure Functions service and is automatically managed by Azure.

The User Administrator role provides permissions to manage user accounts, including creating new users. The Password Administrator and Helpdesk Administrator roles provide permissions to reset user passwords. 

<br>

### Design governance 
- Recommend an organizational and hierarchical structure for Azure resources 
- Recommend a solution for enforcing and auditing compliance 

Azure Active Directory (Azure AD) **access reviews** enable organizations to efficiently manage group memberships, access to enterprise applications, and role assignments. User's access can be reviewed on a regular basis to make sure only the right people have continued access.

Use Azure Policy and tags to generate compliance reports for the subscription and group them by department. 

You apply tags to your Azure resources, resource groups, and subscriptions to logically organize them into a taxonomy. Each tag consists of a name and a value pair. By creating a policy, you avoid the scenario of resources being deployed to your subscription that don't have the expected tags for your organization. 
Instead of manually applying tags or searching for resources that aren't compliant, you create a policy that automatically applies the needed tags during deployment.

<br>

### Design identities and access for applications 
- Recommend solutions to allow applications to access Azure resources 
- Recommend a solution that securely stores passwords and secrets 
- Recommend a solution for integrating applications into Microsoft Azure Active Directory (Azure AD), part of Microsoft Entra 
- Recommend a user consent solution for applications 

---
## Design data storage solutions (25–30%) 

<br>

### Design a data storage solution for relational data 
-	Recommend database service tier sizing 
-	Recommend a solution for database scalability 
-	Recommend a solution for encrypting data at rest, data in transmission, and data in use 

##### General Purpose v2 (GPv2) storage account 
Can store blobs, files, queues, and tables, making it a versatile option for a wide range of applications. 
It supports customer-managed keys for encryption, allowing you to maintain control over the encryption keys. 
To encrypt each user's data with a separate key, you can use Azure Blob Storage Service Encryption with customer-managed keys, storing each user's data in separate containers, and then configuring separate encryption keys for each container.


##### Azure SQL Managed Instance
Is a fully managed SQL Server instance hosted in Azure that supports most of the SQL Server features. It provides easier migration from on-premises SQL Server with minimal database changes, while also minimizing management overhead. 
It allows existing SQL Server customers to lift and shift their on-premises applications to the cloud with minimal application and database changes. 
At the same time, SQL Managed Instance preserves all PaaS capabilities (*automatic patching and version updates, automated backups, high availability)* that drastically reduce management overhead and TCO. It supports features like cross-database queries and transactions

**Elastic pools** in Azure SQL Database are designed to handle multiple databases with varying usage patterns within a shared resource pool. Azure SQL Database provides an SLA of **99.99%** uptime, ensuring high availability for your databases.

##### Azure SQL Database Hyperscale
-	Support scaling up and down: The Hyperscale service tier supports scaling compute resources up and down based on your workload requirements.
-	Support geo-redundant backups: It offers automatic backups with the ability to enable geo-redundant backups to ensure data durability in case of regional disasters.
-	Support a database of up to 75 TB: Hyperscale supports databases up to 100 TB in size, which meets the requirement of 75 TB.
-	Be optimized for online transaction processing (OLTP): Azure SQL Database Hyperscale is designed to handle OLTP workloads with high performance and low latency.

SQL Database options that support Geo Redundancy, vCore model, General Purpose (at additional cost), Business Critical, DTU model, Premium. SQL MI does not support geo-redundancy at all.

<br>

### Design data integration 
-	Recommend a solution for data integration 
-	Recommend a solution for data analysis 

##### Azure Synapse Link
For Azure Cosmos DB, it creates a tight integration between Azure Cosmos DB and Azure Synapse Analytics. It enables customers to run near real-time analytics over their operational data with full performance isolation from their transactional workloads and without an ETL pipeline

*Case study. Service to store and query the data. 50000 IoT devices*

1. Azure Table Storage: You can't query data. Storage limited to 20000/s. 
1. Azure Event Grid: You can't store or query data. 
1. Azure Cosmos DB SQL API: You can store and query data.
4. Azure Time Series Insights: You can store and query data.

**Azure Cosmos DB SQL API**  - With Cosmos DB's novel multi-region (multi-master) writes replication protocol, every region supports both writes and reads. The multi-region writes capability also enables: Unlimited elastic write and read scalability. 99.999% read and write availability all around the world. Guaranteed reads and writes served in less than 10 milliseconds at the 99th percentile.

##### Azure Data Lake Storage Gen2 
Is designed for big data analytics workloads and supports organizing data in directories by date and time, as well as hierarchical namespace. It also allows stored data to be queried directly and is well-integrated with Azure Event Hubs. Supports HDFS protocol.

##### Azure Data Factory
Is a cloud-based data integration service that allows you to create, schedule, and manage data pipelines.

**Dedicated SQL pool**: It's best for big and complex tasks.

**Serverless Apache Spark pool**: It's best for big data analysis and machine learning tasks using Spark SQL and Spark DataFrames. 

**Serverless SQL pool**: This is a service that automatically adjusts the amount of resources you use based on your needs. You only pay for what you use. It's best for small to medium-sized tasks and tasks that change often.

<br>

### Recommend a data storage solution 
-	Recommend a solution for storing relational data 
-	Recommend a solution for storing semi-structured data 
-	Recommend a solution for storing non-relational data 

BlockBlobStorage - is a premium storage account type for block blobs and append blobs. Recommended for scenarios with high transactions rates, or scenarios that use smaller objects or require consistently low storage latency.

Blob -The Archive tier is an offline tier for storing blob data that is rarely accessed. The Archive tier offers the lowest storage costs, but higher data retrieval costs and latency compared to the online tiers (Hot and Cool). Data must remain in the Archive tier for at least 180 days or be subject to an early deletion charge.

Data stored in a premium block blob storage account cannot be tiered to hot, cool, or archive using Set Blob Tier or using Azure Blob Storage lifecycle management. Also, it can be used in conjunction with Azure Web Application Firewall (WAF) on Azure Front Door.

While a blob is in the Archive tier, it can't be read or modified. To read or download a blob in the Archive tier, you must first rehydrate it to an online tier, either Hot or Cool. Data in the Archive tier can take up to 15 hours to rehydrate, depending on the priority you specify for the rehydration operation.

Azure File shares requires Premium accounts. Only Storage1 and storage4 are premium.

TO WRITE INTO STORAGE - MUST BE IN SAME REGION
TO WRITE IN LOG ANALYTICS SPACE - CAN BE IN DIFFERENT REGION

The Data Migration Assistant (DMA) helps you upgrade to a modern data platform by detecting compatibility issues that can impact database functionality in your new version of SQL Server or Azure SQL Database.

<br>

### Design a data storage solution for non-relational data 
-	Recommend access control solutions to data storage 
-	Recommend a data storage solution to balance features, performance, and cost 
-	Design a data solution for protection and durability 

##### Dynamic Data Masking (DDM)
Is a feature in Azure SQL Database that helps you protect sensitive data by obfuscating it from non-privileged users. DDM allows you to define masking rules on specific columns, so that the data in those columns is automatically replaced with a masked value when queried by users without the appropriate permissions.

This ensures that only privileged users can view the actual Personally Identifiable Information (PII), while other users will see the masked data.

Encryption algorithm and key length  - RSA 3072 provides a higher level of encryption strength compared to RSA 2048. While RSA 4096 offers even stronger encryption, it is not supported by Azure SQL Database and Azure SQL Managed Instance for TDE protectors.

Apache Cassandra uses Cassandra Query Language (CQL). NoSQL stores data in document format - MongoDB stores data in a document structure (BSON format)


---
## Design business continuity solutions (10–15%) 

<br>

### Design a solution for backup and disaster recovery 
- Recommend a recovery solution for Azure, hybrid, and on-premises workloads that meets recovery objectives (Recovery Time Objective [RTO], Recovery Level Objective [RLO], Recovery Point Objective [RPO]) 
-	Understand the recovery solutions for containers 
-	Recommend a backup and recovery solution for compute 
-	Recommend a backup and recovery solution for databases 
-	Recommend a backup and recovery solution for unstructured data 

##### Azure Site Recovery 
Is a disaster recovery service that allows you to protect your Azure virtual machines by orchestrating replication, failover, and recovery. It helps you meet the RTO and RPO requirements, automates recovery, and provides protection against regional outages. While it may not be the lowest cost solution, it meets all the other requirements, including automated recovery and support for the specified RTO and RPO.

**Azure MySql server** - flexible only has zonal redundancy by default. YOu must enable geo-redundancy by yourself if you plan to use it for pair-region disaster recovery

**General Purpose v2 (tiered storage)** provides access to the latest Azure storage features, including Cool and Archive storage, with pricing optimized for the lowest GB storage prices. These accounts provide access to Block Blobs, Page Blobs, Files, and Queues. Recommended for most scenarios using Azure Storage.

The contents of your key vault are replicated within the region and to a secondary region at least 150 miles away. During failover, your key vault is in read-only mode.

Zone-redundant configuration is not available in SQL Managed Instance. To prevent Data Loss, Premium/Business Critical is required

**Always Encrypted** is a feature designed to protect sensitive data stored in specific database columns from access (for example, credit card numbers, national identification numbers, or data on a need to know basis). This includes database administrators or other privileged users who are authorized to access the database to perform management tasks, but have no business need to access the particular data in the encrypted columns

<br>

### Design for high availability 
-	Identify the availability requirements of Azure resources 
-	Recommend a high availability solution for compute 
-	Recommend a high availability solution for non-relational data storage 
-	Recommend a high availability solution for relational data storage 

##### Azure Traffic Manager
Is a DNS-based traffic load balancer that enables you to distribute traffic optimally to services across global Azure regions, while providing high availability and responsiveness.

##### Azure Front Door
Is an application delivery network that provides global load balancing and site acceleration service for web applications. It offers Layer 7 capabilities for your application like SSL offload, path-based routing, fast failover, caching, etc. to improve performance and high-availability of your applications. 

**Azure Application Gateway** will balance the traffic between VMs deployed in the same region. Create an Azure Traffic Manager profile instead (fits global requirements)

**Premium Azure file shares** only support LRS and ZRS. Premium file shares are backed by solid-state drives (SSDs) and provide consistent high performance and low latency, within single-digit milliseconds for most IO operations, for IO-intensive workloads. Hot file share is general use and is backed by HDD. 

For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted. Disks are encrypted by using cryptographic keys that are secured in an Azure Key Vault. You control these cryptographic keys and can audit their use.

**Data Box** is a rugged device that allows organizations to have 100 TB of capacity on which to copy their data and then send it to be transferred to Azure. It is an extremely powerful solution that helps customers get their data to the Azure public cloud in a cost-effective, secure, and efficient manner with powerful Azure and machine learning at play. 

Automate data movement using Azure Data Factory, then load data into Azure Data Lake Storage, transform and clean it using Azure Databricks, and make it available for analytics using Azure Synapse Analytics. Modernize your data warehouse in the cloud.

If your requirements include encrypting all of the above and end-to-end encryption, use **Azure Disk Encryption**. 

If your requirements include encrypting only data at rest with customer-managed key, then use Server-side encryption with customer-managed keys. You **cannot** encrypt a disk with both Azure Disk Encryption and Storage server-side encryption with customer managed keys

A self-hosted integration runtime can run copy activities between a cloud data store and a data store in a private network. It also can dispatch transform activities against compute resources in an on-premises network or an Azure virtual network. The installation of a self-hosted integration runtime needs an on-premises machine or a virtual machine inside a private network.


---

## Design infrastructure solutions (25–30%) 

<br>

### Design a compute solution 
- Recommend a virtual machine–based compute solution 
- Recommend an appropriately sized compute solution based on workload requirements 
- Recommend a container-based compute solution 
- Recommend a serverless-based compute solution 
  
##### Azure Logic Apps
Is a serverless solution that enables you to create and run workflows that integrate with various services and systems. You can use Azure Logic Apps to create a workflow that runs the PowerShell script once an hour using a time-based trigger, sends an email notification to the operations manager for approval, and processes the email response.

##### Azure Functions
Is a serverless compute service that allows you to run event-driven code without having to manage infrastructure explicitly. You can use Azure Functions to host the PowerShell script, which can be triggered by the Logic App when the operations manager approves the deletion.

<br>

### Design an application architecture 
- Recommend a caching solution for applications 
- Recommend a messaging architecture 
- Recommend an event-driven architecture 
- Recommend an automated deployment solution for your applications 
- Recommend an application configuration management solution 
- Recommend a solution for API integration 

##### Authorization workflow
A user or application acquires a token from Azure AD with permissions that grant access to the backend-app. The token is added in the Authorization header of API requests to API Management.

**API Management** validates the token by using the validate-jwt policy. If a request doesn't have a valid token, API Management blocks it. If a request is accompanied by a valid token, the gateway can forward the request to the API.

<br>

### Design migrations 
- Evaluate a migration solution that leverages the Cloud Adoption Framework for Azure 
- Assess and interpret on-premises servers, data, and applications for migration 
- Recommend a solution for migrating applications and virtual machines 
- Recommend a solution for migrating databases 
- Recommend a solution for migrating unstructured data 
  
You can use Azure AD Domain Services and sync identities needed from Azure AD to Azure AD DS to use legacy protocols like LDAP. Kerberos and NTLM

<br>

### Design network solutions 
- Recommend a network architecture solution based on workload requirements 
- Recommend a connectivity solution that connects Azure resources to the internet 
- Recommend a connectivity solution that connects Azure resources to on-premises networks 
- Optimize network performance for applications 
- Recommend a solution to optimize network security 
- Recommend a load balancing and routing solution
 
A basic Azure virtual WAN does not support express route. You have to upgrade to standard.

Azure File Sync enables you to centralize your organization's file shares in Azure Files while maintaining local access to the data. In this scenario, you can use Azure File Sync to synchronize the shared files on VM1 to an Azure file share.

An on-premises data gateway allows you to securely access on-premises data and resources from Azure Logic Apps. In this scenario, deploying an on-premises data gateway on Server1 or another server in the datacenter will enable LogicApp1 to access the SQL Server 2016 database on Server1

A connection gateway resource will communicate with the on-premises data gateway to provide LogicApp1 with the ability to access the SQL Server 2016 database on Server1 securely.

Azure WAF v2 has latest OWASP rules (3.2) in preview and requires App Gateway with required /24 subnet to deploy.

Azure API Management Premium tier supports virtual network integration, which allows you to restrict ingress access to the microservices to a single private IP address within the virtual network. This tier also supports mutual TLS authentication, rate-limiting policies, and provides a solution for exposing the microservices to the consumer apps while minimizing costs.

***THE END, Thanks for getting this far !***