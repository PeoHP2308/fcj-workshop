---
title: "Event 4"
date: "2025-10-06"
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# EVENT REPORT: DEVOPS ON AWS

&emsp;**Date:** Monday, November 17, 2025

&emsp;**Time:** 8:30 AM – 5:00 PM

&emsp;**Location:** AWS Office

&emsp;**Event Objective:** To provide comprehensive knowledge on DevOps Culture, principles, performance metrics, and the suite of AWS DevOps services for building CI/CD pipelines, IaC, and Observability.

---

## MORNING SESSION (8:30 AM – 12:00 PM): CI/CD & INFRASTRUCTURE AS CODE

### 1. Welcome & DevOps Mindset (8:30 – 9:00 AM)
* The event began with a **quick recap** from the previous AI/ML session, setting the context for moving models/applications into production.
* The focus was on **DevOps culture and principles**, emphasizing collaboration between Development (Dev) and Operations (Ops).
* Introduction to **key performance metrics** such as **DORA** (Deployment Frequency, Lead Time for Changes, MTTR, Change Failure Rate) and **MTTR** (Mean Time To Recovery).

### 2. AWS DevOps Services – Building the CI/CD Pipeline (9:00 – 10:30 AM)
This section delved into the AWS CodeFamily suite of tools to automate **Continuous Integration (CI)** and **Continuous Deployment (CD)**:
* **Source Control:** Using **AWS CodeCommit** and discussing source code management strategies like **GitFlow** and **Trunk-based**.
* **Build & Test:** Configuring **CodeBuild** to automate compilation and running testing pipelines.
* **Deployment:** Analyzing advanced deployment strategies with **CodeDeploy**, including **Blue/Green, Canary, and Rolling updates**.
* **Orchestration:** Utilizing **CodePipeline** to automate the entire process from commit to production.
* **Demo:** A visual demonstration of a complete CI/CD pipeline on AWS.

### 3. Infrastructure as Code (IaC) (10:45 AM – 12:00 PM)
IaC is the foundation of modern DevOps, ensuring environment consistency and reproducibility:
* **AWS CloudFormation:** Learning about **Templates, Stacks,** and how to use **Drift Detection** to identify discrepancies between the actual state and the source code.
* **AWS CDK (Cloud Development Kit):** Introducing CDK as a more modern approach, using familiar programming languages (TypeScript, Python,...) to define infrastructure via **Constructs** and **reusable patterns**.
* **Demo & Discussion:** Demonstrating infrastructure deployment using both CloudFormation and CDK, along with a discussion on criteria for choosing the right IaC tool.

---

## AFTERNOON SESSION (1:00 PM – 5:00 PM): CONTAINER, OBSERVABILITY & BEST PRACTICES

### 4. Container Services on AWS (1:00 – 2:30 PM)
* **Docker Fundamentals:** Review of basic concepts related to **Microservices** and **Containerization**.
* **Amazon ECR (Elastic Container Registry):** The service for storing container images, including **image scanning** features and **lifecycle policies**.
* **Amazon ECS & EKS:** Comparing the two main container orchestration services: **ECS** (simpler, AWS integrated) and **EKS** (Kubernetes-based). Analysis of deployment and scaling strategies.
* **AWS App Runner:** A simplified solution for container deployment, focusing on code over infrastructure.
* **Demo & Case Study:** Illustrating microservices architecture deployment and comparing the services.

### 5. Monitoring & Observability (2:45 – 4:00 PM)
* **CloudWatch:** The core tool for collecting **metrics, logs, alarms, and dashboards** across the entire AWS ecosystem.
* **AWS X-Ray:** Provides **Distributed Tracing** to track request flow across microservices, helping to identify bottlenecks and performance issues.
* **Demo:** Setting up a comprehensive observability system (Full-stack observability).
* **Best Practices:** Best practices for **Alerting, dashboards,** and the **on-call** process.

### 6. DevOps Best Practices & Case Studies (4:00 – 4:45 PM)
* **Advanced Deployment Strategies:** Discussion on **Feature flags** and **A/B testing** within the deployment pipeline.
* **Automated testing** and deep integration with CI/CD.
* **Incident Management:** Managing incidents and the importance of **Postmortems** for learning and improvement.
* **Case Studies:** Lessons learned from startups and large enterprises that have undergone successful DevOps transformations.

### 7. Q&A & Wrap-up (4:45 – 5:00 PM)
* Addressing in-depth questions.
* Providing information on **DevOps career pathways** and relevant AWS certifications.

---

## GENERAL ASSESSMENT AND APPLICATION

**Assessment:**
The event covered the entire DevOps lifecycle, from culture, CI/CD process, infrastructure management as code, to container operations and observability. The knowledge delivered was clear, combining theory (DevOps Principles, DORA metrics) and practical tools (CodePipeline, CDK, X-Ray).

**Key Knowledge Acquired:**
* Mastery of the AWS CodeFamily tools for setting up automated CI/CD pipelines.
* Clear understanding of the pros and cons of CloudFormation and CDK for IaC.
* Ability to differentiate and apply container services (ECS, EKS).
* Setting up proactive monitoring systems using CloudWatch and X-Ray.

**Proposed Next Steps:**
1.  **Practice CI/CD:** Utilize **CodeCommit, CodeBuild, and CodePipeline** to automate deployment for the current project (if applicable) or a sample application.
2.  **Explore CDK:** Start using **AWS CDK** to define project infrastructure instead of the Console, focusing on creating simple **Constructs**.
3.  **Implement Observability:** Integrate **CloudWatch Logs** and experiment with **AWS X-Ray** to trace function calls (API calls) within the project to apply monitoring principles from the start.
## Event Photos.

![](/images/4-Events/Event4.1.jpg)
![](/images/4-Events/Event4.2.jpg)
![](/images/4-Events/Event4.3.jpg)
![](/images/4-Events/Event4.4.jpg)

