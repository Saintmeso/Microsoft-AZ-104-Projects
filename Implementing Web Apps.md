# AZ-104 - Implementing Web Apps

This Microsoft azure lab is focused on deploying, managing, and scaling web applications while using Azure App Service. Deployment slots, auto scaling, and azure load testing.

This lab is meant to show how applications are able to be deployed from Git repo's, tested in a staging environment, promoted to production and then tested under simulated traffic via azure load testing.

---
## Scenario 

An Organization needs to deploy a web application that remains accessible as user traffic increases.
Deploying new application versions directly into production can introduce downtime or application errors. Similarly, insufficient compute capacity can affect application performance during traffic surges.

To address these challenges, the organization uses Azure App Service to host the application and deployment slots to separate staging from production.

The lab focuses on four primary tasks:

1. Deploying a web application using Azure App Service
2. Configuring staging and production deployment slots
3. Configuring automatic scaling and testing application performance
4. Understanding availability, troubleshooting, and cost management

<img width="1284" height="706" alt="image" src="https://github.com/user-attachments/assets/2c3a3932-92fc-41d3-98b7-977b5a980e57" />

---

# Task 1 - Creating an Azure Web App

## 1. Configure the Web App

| Setting | Configuration |
|---|---|
| Resource Group | `practice-rg` |
| Operating System | Linux |
| Runtime Stack | PHP 8.2 |
| App Service Plan | Premium V3 |
| SKU | P1V3 |
| Region | East US |

The Web App was configured to host a PHP application using Azure's managed application hosting platform.

### Why Azure App Service?

Azure App Service provides a managed hosting platform, reducing the need to maintain operating systems and web server infrastructure and letting azure handle it for you.

An App Service Plan defines the compute tier and capacity available to the application.

### Web App Configuration 

<img width="980" height="868" alt="Creating a web app picture " src="https://github.com/user-attachments/assets/f7efcbfb-4673-4954-9d37-603a0abafe00" />

# Task 2 - Configure a Staging Deployment Slot

## 1. Create the Staging slot

Created a deployment slot named:

**`staging`**

This slot provides a separate environment other than production for deploying and validating application versions before deploying them into production.

### Why Deployment Slots?

Deployment slots help reduce the risk associated with releasing new application versions directly to production.

Instead of replacing the live application immediately, Administrators can deploy the new version to staging and verify that it works before performing a slot swap.

### Staging Slot Configuration

<img width="961" height="857" alt="Staging deployment slots pic" src="https://github.com/user-attachments/assets/4fe52f57-85b9-4cc1-b88c-452a422b303b" />

# Task 3 - Configure External Git Deployment

## 1. Enable SCM Basic Auth Publishing Credentials

Enabled **SCM Basic Auth Publishing Credentials** in the App Service configuration.

This setting allows supported publishing methods to auth to the App service deployment endpoint using publishing credentials.

## Purpose

Authentication helps ensure that only authorized users or deployment tools can perform operations that require publishing credentials.

Enabling this setting does not establish the Github connection by itself. The deployment source must still be configured separately.

## 2. Configure Deployment Center

Configured Azure Deployment Center to retrieve the PHP application from an external Git repository.

| Setting | Configuration |
|---|---|
| Deployment Source | External Git |
| Repository | `https://github.com/Azure-Samples/php-docs-hello-world` |
| Branch | `master` |
| Target Slot | Staging |

The application code was deployed to the staging slot through the configured deployment workflow.

### Purpose

External Git deployment allows Azure App Service to retrieve application source code from a repository and deploy it to the web application.

This separates source code management from the hosting environment.

### Git Deployment Configuration



<img width="1088" height="857" alt="Deployment Center pic" src="https://github.com/user-attachments/assets/55f7244f-61c4-4fdb-bf18-a7f819636b60" />

---

## 3. Verify the Deployment

After the deployment completed, the sample PHP application was accessed through the staging URL.

The application displayed the expected Hello World page.

### Purpose

Verifying the deployed application confirms that the deployment workflow completed and that the application can respond to requests.

### Staging Application Verification



<img width="1087" height="243" alt="Hello world Web App Pic" src="https://github.com/user-attachments/assets/3c21283a-37e0-4507-8e60-385c41994471" />

---

# Task 4 – Swap Staging and Production

## 1. Promote the Staging Application

Performed a slot swap to promote the application version from staging to production.

The production slot then served the deployed Hello World application.

### Purpose

A slot swap simplifies the release process by promoting a tested application version to production without requiring administrators to manually copy the application code into the production environment.

Slot swaps can reduce deployment downtime, although they do not guarantee zero downtime or eliminate every deployment risk.

### Production Deployment Verification



<img width="1090" height="484" alt="Deployment Swaps confirmation" src="https://github.com/user-attachments/assets/9124971d-b7ce-453f-9cde-78bb41ec377a" />
<img width="1092" height="174" alt="Staging deployment Hello world swap" src="https://github.com/user-attachments/assets/b73eb3f7-1eb6-4e25-a058-b60305ef0738" />

---

# Task 5 – Configure Automatic Scaling

## 1. Configure the Scaling Settings

Configured automatic scaling for the production App Service Plan using the settings specified in the lab.

| Setting | Configuration |
|---|---|
| Scaling Mode | Automatic |
| Minimum Instances | 1 |
| Maximum Burst | 2 |

The configuration establishes the minimum instance count and the maximum burst capacity for the lab's scaling setup.

### Purpose

Scaling allows an application to accommodate changes in demand by adjusting its available compute capacity.

Automatic scaling reduces the need for an administrator to manually adjust capacity whenever workload demand changes.

### Scaling Configuration



<img width="1087" height="791" alt="Scaling out Web App" src="https://github.com/user-attachments/assets/88284063-2648-477b-af43-ff6796d6fe0c" />

---

# Task 6 – Perform Load Testing

## 1. Configure Azure Load Testing

Created an Azure Load Testing resource and configured an HTTP request targeting the deployed web application.

The test generated simulated traffic to evaluate application performance under load.

### Purpose

Load testing helps administrators understand how an application behaves when multiple simulated users send requests.

It can help identify potential performance bottlenecks and provide evidence for scaling decisions.

---

## 2. Review Load Testing Metrics

Reviewed the performance metrics reported by Azure Load Testing.

| Metric | Purpose |
|---|---|
| Virtual Users | Indicates the number of simulated users generating load |
| Response Time | Measures how long requests take to receive a response |
| Requests per Second | Indicates the rate of requests being processed or sent, depending on the reported metric |

### Purpose

These metrics help evaluate application responsiveness and throughput under simulated demand.

Slow response times or increasing errors may indicate a performance bottleneck that requires further investigation.

### Load Testing Results



<img width="1090" height="855" alt="Load Testing metrics" src="https://github.com/user-attachments/assets/2f8569a1-6a02-45a3-a958-dfe6d4ef6853" />

---

# Key Technical Decisions

## Managed Hosting with Azure App Service

App Service was used instead of manually provisioning a virtual machine and installing the web server environment.

This reduces infrastructure administration and allows the focus to remain on application deployment, configuration, and scaling.

## Staging Before Production

A separate staging slot provides an environment for validating application versions before promotion.

This reduces release risk and simplifies application releases.

## Bounded Automatic Scaling

The scaling configuration establishes minimum and maximum instance limits.

This provides a defined scaling range and helps control resource allocation and potential costs.

## Load Testing Before Production Workloads Increase

Azure Load Testing provides evidence about application behavior under simulated demand.

The results can help identify bottlenecks and guide decisions about scaling or application optimization.

---

# Security, Availability & Cost Considerations

- **Deployment Authentication:** Publishing credentials should be protected and limited to the deployment workflows that require them.
- **Availability:** Zone redundancy can improve resilience against Availability Zone failures when supported and appropriately configured.
- **Configuration Management:** Slot-specific settings help keep environment-dependent configuration associated with the correct slot.
- **Performance:** Load testing provides evidence for scaling and troubleshooting decisions.
- **Cost Management:** App Service Plans can continue to incur charges even when application traffic is low.
- **Resource Cleanup:** Unused resources should be reviewed and removed to avoid unnecessary charges.

These are considerations explored through the project and do not imply that every listed security or availability control was implemented.

---

# Lessons Learned

This project reinforced the importance of understanding how Azure application hosting, deployment, scaling, and monitoring work together.

Key takeaways include:

- Azure App Service provides managed hosting for web applications.
- App Service Plans determine the compute tier and capacity available to hosted applications.
- Deployment slots provide a safer release workflow.
- SCM Basic Auth Publishing Credentials enable a specific authentication method for supported publishing workflows.
- Scaling out increases the number of instances, while scaling up increases the available compute tier or resources.
- Load testing helps identify potential performance bottlenecks.
- Zone redundancy and scaling solve different problems.
- Azure resource groups can simplify cleanup when they contain only resources dedicated to a project.

---

# Troubleshooting

## App Service Plan Quota Error

During deployment, Azure returned a quota error indicating that the subscription had a quota limit of zero for the required Premium V3 VM family.

The deployment could not proceed until the quota issue was resolved.

### Lesson Learned

Azure subscriptions can have resource quotas even when using a Pay-As-You-Go billing model.

Administrators must verify subscription quotas and regional availability when provisioning Azure resources.

