---
title : "Introduction"
date :  "2025-10-06" 
weight : 5
chapter : false
pre : " <b> 5.1. </b> "
---
### Introduction to VPC Endpoints.

- **VPC Endpoint** is a **virtual device** within an Amazon VPC that enables compute resources 
(such as EC2, Lambda, ECS,...) to **securely communicate** with AWS services **without traversing the public Internet**.
- These Endpoints are designed to be **horizontally scalable, highly redundant,** and 
**ensure high availability,** eliminating the risk of connection loss due to reliance on an Internet Gateway or NAT Gateway.
- Compute resources running within a **VPC** can access **Amazon S3** via a **Gateway Endpoint**, while an 
**Interface Endpoint (AWS PrivateLink)** can be used for resources **within the VPC or in an On-Premises data 
center** to access AWS services privately.

### Workshop Overview.

In this workshop, we will:

- Learn how to **create, configure, and test** two types of **VPC Endpoints** (Gateway and Interface).
- Set up a **Hybrid Access** model, where workloads in the VPC and On-Premises systems can both access 
**Amazon S3 securely, privately, and without going over the Internet.**
- Applying the following **security policy layers (Security Layers)**:
  + **Endpoint Policy**: Controls access permissions at the Endpoint level.
  + **Bucket Policy**: Restricts access based on the source IP and the allowed network range.
- Perform **access testing (Positive & Negative Testing)** to verify the operation of each security layer and 
- ensure that only valid sources can access data in S3.

### Implementation Steps.

#### Phase 1: Environment Preparation:

| Step  | Action                                                                                                                                                             | Role              |
|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| 1.1   | Create a **VPC** (Like: `Hybrid-VPC`) and 2 **Subnet** (1 Public, 1 Private).                                                                                      | Cloud Environment |
| 1.2   | Create a **S3 Bucket** (Like: `s3://hybrid-secure-data-prod`).                                                                                                     | Data Storage      |
| 1.3   | Create a **EC2 Instance** in **Subnet Private** (simulating the VPC Workload).                                                                                     | Test Subject 1    |
| 1.4   | Create a **EC2 Instance** in **Subnet Public** (simulating the On-Premise System) and assign a specific source IP (assuming this IP comes via Direct Connect/VPN). | Test Subject 2    |

#### Phase 2: Configure Interface Endpoint (For Hybrid Access):

| Step | Action                                                                                                                              | Purpose                                                                  |
|------|-------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
| 2.1  | Create an **Interface VPC Endpoint** for S3. Select the `Service Name` as `com.amazonaws.region.s3.`                                | Establish a private connection for On-Premise.                           |
| 2.2  | Configure **Private DNS** in the VPC so traffic from On-Premise resolves the S3 name to the Endpoint's private IP.                  | Ensure traffic does not go out to the Internet.                          |
| 2.3  | Apply the following **Endpoint Policy** to the Interface Endpoint, only allowing access to `arn:aws:s3:::hybrid-secure-data-prod*.` | **Security Layer 1:** Restrict the Buckets accessible via this Endpoint. |

#### JSON Policy:

```json
{
  "Statement": [
    {
      "Sid": "AllowAccessToSpecificBucket",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::hybrid-secure-data-prod",
        "arn:aws:s3:::hybrid-secure-data-prod/*"
      ]
    },
    {
      "Sid": "DenyAllOtherBuckets",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::*"
    }
  ]
}
```

#### Phase 3: Configure Gateway Endpoint (For In-VPC Access).

| Step | Action                                                                                                                     | Purpose                                                                     |
|------|----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| 3.1  | Create a **Gateway VPC Endpoint** for S3.                                                                                  | Ensure S3 access within the VPC without needing to go through the Internet. |
| 3.2  | **Update Route Table:** Associate the Endpoint with the Route Table of the Private Subnet.                                 | Direct S3 traffic within the VPC to the Gateway Endpoint.                   |
| 3.3  | Apply **Endpoint Policy**: Apply a simple policy that allows access, but only for specific IAM Roles/Users within the VPC. | **Security Layer 2**: Additional access control mechanism.                  |

---

#### Phase 4: Enforce S3 Bucket Policy (Source Control).

| Step | Active                                                                                                                                                                          | Purpose                                                                                   |
|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| 4.1  | Configure the **S3 Bucket Policy** for `s3://hybrid-secure-data-prod.`                                                                                                          | **Security Layer 3**: The final control layer, ensuring only trusted sources are allowed. |
| 4.2  | Use the **aws:SourceIp** condition to only allow access from the **On-Premise CIDR Block (IP of the simulated EC2 in 1.4)** and the **Private Subnet CIDR Block (EC2 in VPC)**. | Limit access to S3 from outside the allowed Hybrid sources.                         |

#### JSON Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RestrictAccessToHybridSources",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::hybrid-secure-data-prod",
        "arn:aws:s3:::hybrid-secure-data-prod/*"
      ],
      "Condition": {
        "NotIpAddress": {
          "aws:SourceIp": [
            "SIMULATED_ON_PREM_IP/32",
            "CIDR_CỦA_SUBNET_PRIVATE"
          ]
        }
      }
    }
  ]
}
```

#### Phase 5: Testing and Negative Testing.

The goal of this phase is to verify the effectiveness of the security layers applied in the VPC Endpoint configuration and S3 Bucket Policy, ensuring that only valid sources (identified via IP and Endpoint Policy) have access permission.

| **Step** | **Access Source**               | **Testing Goal**                                      | **Expected Result**                                                 |
|----------|---------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|
| **5.1**  | EC2 On-Premise*(Allowed IP)*    | Access`s3://hybrid-secure-data-prod`                  | **SUCCESS**: Allowed because the source IP and Bucket Policy match. |
| **5.2**  | EC2 On-Premise*(Allowed IP)*    | Access a **different** S3 bucket                      | **FAILURE**: Denied by the Interface Endpoint Policy.               |
| **5.3**  | EC2 trong VPC*(Subnet Private)* | Access`s3://hybrid-secure-data-prod`                  | **SUCCESS**: Allowed because the source IP and Bucket Policy match. |
| **5.4**  | EC2 bất kỳ*(có Public IP)*      | Access`s3://hybrid-secure-data-prod` via the Internet | **FAILURE**: Denied by the S3 Bucket Policy (`NotIpAddress`).         |

---

### Analysis of Test Results.

- **Valid Access (5.1 & 5.3):**
  Sources within the allowed IP list or from the Private Subnet were able to access normally through the secure Endpoints.
- **Denied Access (5.2 & 5.4):**
  Accesses outside the allowed IP or bucket scope were successfully denied by the **Endpoint Policy** or **Bucket Policy layer**, enforcing the **Zero Trust Access** principle.

---

### Conclusion.

The testing confirmed that:

- The **Gateway and Interface Endpoints** function correctly, routing internal traffic without traversing the Internet.

- The **multi-layered security policies** (VPC Endpoint Policy + S3 Bucket Policy) are effective.

- The **Hybrid Access model** is secured, with controlled access sources, adhering to AWS internal security principles.