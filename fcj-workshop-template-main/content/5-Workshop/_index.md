---
title: "Workshop"
date: "2025-10-06"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Implementing Detailed Security and Access Control for Hybrid S3 Access

#### Overview.

**Extended Lab Idea:**  
This lab focuses on applying **Defense-in-Depth** security layers to ensure that only authenticated and authorized
sources can access S3 resources through private connections **(PrivateLink/VPC Endpoint)**.

**Lab Objectives:**  
After completing this lab, you will be able to:

- Configure an **Interface VPC Endpoint** to extend private S3 connectivity to an on-premises (simulated) environment.
- Implement an **Endpoint Policy** to allow access to a specific S3 bucket only.
- Apply an **S3 Bucket Policy** to restrict access based on the source IP address of the on-premises data center.
- Demonstrate that **S3** access through endpoints is secured and strictly limited following the **Least Privilege** principle.

---

#### Contents

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Accessing S3 from VPC](5.3-S3-vpc/)
4. [Accessing S3 from On-premises Datacenter](5.4-S3-onprem/)
5. [VPC Endpoint Policies (optional)](5.5-Policy/)
6. [Resource Cleanup](5.6-Cleanup/)  
