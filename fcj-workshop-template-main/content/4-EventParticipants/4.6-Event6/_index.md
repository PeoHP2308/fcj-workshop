---
title: "Event 6"
date: "2025-10-06"
weight: 4
chapter: false
pre: " <b> 4.6. </b> "
---

# EVENT REPORT: AWS FOR SAP USING GENERATIVE AI
&emsp;**Time:** 1:00 PM - 5:00 PM.

&emsp;**Speaker:** Nonthakorn Junthapol.

&emsp;**Topic:** AWS for SAP Using Generative AI & SAP ABAP capabilities and Amazon Q Developer.

&emsp;**Event Objective:** To explore how Generative Artificial Intelligence (GenAI) can optimize and modernize the SAP environment, focusing on enhancing ABAP development productivity through Amazon Q Developer.

---

## CONTEXT AND GOALS OF GENAI IN SAP

### 1. Modernizing SAP on AWS
* The event established the foundation by affirming **AWS's role as the preferred cloud platform** for SAP S/4HANA systems (including the **RISE with SAP** model).
* Discussion covered traditional challenges in the SAP environment: the complexity of **Legacy ABAP** code, the need for modernization (e.g., migrating to **S/4HANA**), and slow development speed.

### 2. The Role of Generative AI
* GenAI was introduced as a strategic tool to overcome these challenges, especially in **code automation, refactoring, and data interaction**.
* Services like **Amazon Bedrock** (or custom models on SageMaker) can be used to analyze unstructured SAP data (e.g., PO documents, Invoices) after extraction via AWS.

---

## DEEP DIVE: AMAZON Q DEVELOPER AND ABAP CAPABILITIES

### 3. Introducing Amazon Q Developer
* **Amazon Q Developer** is an AI assistant designed to accelerate software development, customized to understand the context of AWS infrastructure and APIs.
* A key highlight is its ability to **interact using natural language** to solve technical issues and generate code.

### 4. Boosting Productivity with SAP ABAP
The core focus of the event delved into how Amazon Q directly supports ABAP developers:

| Amazon Q Capability | Application in ABAP | Value Proposition |
| :--- | :--- | :--- |
| **Code Generation** | Quickly creating ABAP code snippets for BAPIs, Function Modules, or standard ALV reports. | Increases initial coding speed. |
| **Code Explanation** | Analyzing and explaining complex or sparsely used legacy ABAP code segments. | Reduces maintenance time and code learning curve. |
| **Refactoring & Modernization** | Supporting the conversion of old ABAP code to more modern object-oriented (OOP) syntax and principles (e.g., preparing for S/4HANA). | Lowers the risk and cost of system migration/upgrade. |
| **Troubleshooting** | Suggesting solutions and debugging guidance based on error messages in the SAP environment. | Minimizes Mean Time To Recovery (MTTR). |


### 5. Integration Mechanism and Security
* Amazon Q functions as an abstraction layer, accessing ABAP source code through authorized connections (potentially via AWS Connector tools or integration with the development environment).
* **Data Security:** It was emphasized that ABAP source code data is processed securely and is **not used to train the underlying Amazon Q model**, ensuring the privacy and security of enterprise intellectual property.

---

## GENERAL ASSESSMENT AND NEXT STEPS

**Assessment:**
The event outlined a clear path for the future of SAP ABAP development, shifting from a manual and time-consuming process to one supported by AI. Amazon Q Developer serves as a disruptive tool, helping SAP enterprises fully leverage the power of the AWS Cloud, not just at the infrastructure level but also in application and development layers.

**Key Knowledge Acquired:** Understanding the direct, practical applications of Amazon Q Developer in accelerating ABAP modernization and standard development tasks.

**Proposed Next Steps:**
1.  **Pilot Amazon Q Developer:** SAP development teams should pilot Amazon Q Developer in a sandbox environment to test its capabilities in generating, explaining, and refactoring standard ABAP code snippets.
2.  **Refactoring Planning:** Utilize Amazon Q to assess the complexity and risk level of legacy ABAP code, thereby creating a detailed plan for modernization or migration projects to S/4HANA (Greenfield/Brownfield).
3.  **Integrate GenAI into Data Flow:** Investigate how to use other AWS GenAI services (beyond Q Developer) to process SAP business data streams (e.g., automated summarization of large sales orders, classification of transaction errors).

## Event Photos.

![](/images/4-Events/Event6.1.jpg)  
![](/images/4-Events/Event6.2.jpg)  


