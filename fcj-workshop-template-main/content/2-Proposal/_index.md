---
title: "Proposal"
date: "2025-10-06"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# AWS Cloud Health Dashboard

## Comprehensive AWS Infrastructure Monitoring and Optimization Solution

### 1. Executive Summary.

**AWS Cloud Health Dashboard** is a monitoring and optimization platform designed to help businesses and individuals 
effectively manage the **costs**, **security**, and **performance** of their AWS infrastructure.  
The platform leverages a lightweight architecture built on **EC2** and **DynamoDB**, integrating key AWS services such 
as **CloudWatch**, **Cost Explorer**, and **Security Hub** to deliver **real-time monitoring**, **data visualization**, 
and **cost analytics**.

**Key Highlights:**

   - **Operational Cost:** $12–18/month (leveraging AWS Free Tier).
   - **CloudWatch Integration:** Real-time metrics tracking and alerting.
   - **Cost Analysis:** Insights and optimization recommendations via **AWS APIs**.
   - **Real-Time Dashboard:** Efficient data caching for fast performance.
   - **Scalable & Maintainable:** Simple architecture for easy maintaining and scaling.

### 2. Problem Statement.

**Current Challenges:**

Many organizations and individual AWS users face several common issues:

1. **Uncontrolled Costs:**  
   Lack of centralized monitoring tools often leads to unexpected AWS billing spikes.

2. **Limited Security Visibility:**  
   No unified dashboard to aggregate and visualize security findings.

3. **Scattered Metrics:**  
   Users must navigate across multiple AWS consoles to view performance and cost data.

4. **Lack of Historical Data:**  
   CloudWatch retains metrics for only 15 days under the Free Tier.

5. **No Custom Alerting:**  
   Complex or multi-condition alert configurations are difficult to implement.

**Proposed Solution:**

**Cloud Health Dashboard** delivers a centralized monitoring and optimization platform with the following key features:

- **Centralized Monitoring:**  
  A single dashboard integrating **CloudWatch**, **Cost Explorer**, and **Security Hub** data.

- **Data Persistence with DynamoDB:**
    - Four dedicated tables: `CloudHealthMetrics`, `CloudHealthCosts`, `SecurityFindings`, and `Recommendations`.
    - Automatic **TTL policies** for efficient data lifecycle management:
        + Metrics: 30 days.
        + Costs: 365 days.
        + Security: 90 days.
        + Recommendations: 180 days.
    - **On-Demand pricing** to minimize cost.
    - **Optimized query patterns** using GSIs for efficient access across data types.

- **Cost analysis**:
    + Historical cost trends.
    + Service breakdown.
    + AWS Cost Explorer recommendations integration.
    + Budget alerts.

- **Security monitoring**:
    + Security Hub findings aggregation.
    + GuardDuty threat detection display.
    + Compliance status tracking.
    + Severity-based filtering.

- **Intelligent recommendations**:
    + Cost optimization suggestions.
    + Performance improvements.
    + Security enhancements.
    + Impact-based prioritization.

- **Performance Enhancements:**
    + **Redis Caching** to minimize AWS API calls and improve response time.
    + **Pre-Collected Data** stored in DynamoDB for quick retrieval.
    + **WebSocket Integration** for real-time data updates on the dashboard.

### Benefits and ROI.

1. **Cost Savings**:
    - Full visibility into **underutilized or idle resources**.
    - Leverages **AWS native cost recommendations** (Cost Explorer, Trusted Advisor).
    - Platform operating cost: **only $12–18/month**.
    - Potential cost reduction: **15–25%** by identifying and eliminating waste.

2. **Enhanced Visibility**:
    - Unified **single dashboard** replacing 5+ separate AWS consoles.
    - **Historical data retention** beyond CloudWatch limits.
    - **Custom alerts** and **real-time notifications** for faster response.
    - Comprehensive view of performance, cost, and security metrics.

3. **Improved Productivity**:
    - Reduces **daily monitoring time** from 30 minutes to just 5 minutes.
    - Automated data collection and report generation.
    - **Proactive alerts** prevent downtime and overspending.
    - Actionable recommendations streamline operations and decision-making.


### 3. Solution Architecture.

**Architecture Overview:**

The platform leverages a **Single EC2 Instance + DynamoDB** architecture to **optimize cost and simplify deployment**.  
All application components (backend, API, and dashboard) run on a single EC2 instance, while **DynamoDB** is used for 
persistent and scalable data storage.

![Architecture Diagram](/images/2-Proposal/AM.png)

Raw Design Draft:

![None Diagram](/images/2-Proposal/AR.png)

**AWS Services Used:**

1. **Amazon EC2** (Compute):
    - t3.micro instance (750h/month Free Tier).
    - Single public subnet (no NAT Gateway needed).
    - Components: Nginx, FastAPI, Redis, React, CloudWatch Agent.
    - Security: Restrictive security groups, Systems Manager Session Manager.

2. **Amazon DynamoDB** (Storage):
    - 4 specialized tables: Metrics, Costs, Security, Recommendations.
    - On-demand pricing mode.
    - TTL enabled for automatic cleanup.
    - Global Secondary Indexes for query optimization.
    - Point-in-time recovery for backups.

3. **Amazon CloudWatch**:
    - Metrics collection with EC2, RDS, S3, Lambda, etc.
    - Custom metrics for application monitoring.
    - Logs aggregation.
    - Alarms và notifications.

4. **AWS Cost Explorer**:
    - Cost and usage data via API.
    - Rightsizing recommendations (AWS native).
    - Cost forecast (AWS native).
    - Service breakdown.

5. **AWS Security Hub**:
    - Security findings aggregation.
    - Compliance checking.
    - Integration with GuardDuty.

6. **Amazon GuardDuty** (Optional):
    - Threat detection.
    - Anomaly monitoring.

7. **Amazon S3** (Backup):
    - DynamoDB backup exports.
    - CloudWatch logs archive.

**Single Public Subnet Design:**

The **EC2 instance** is deployed in a **public subnet** with the following **security measures:**

1. **Security Group Configuration:**
    - **Inbound:** Port 80 and 443 open to `0.0.0.0/0`.
    - **Inbound:** Port 22 allowed only from trusted IPs (or fully disabled).
    - **Outbound:** Unrestricted (to allow AWS API calls).

2. **Access Management:**
    - **AWS Systems Manager Session Manager** used instead of SSH.
    - **IAM Roles** designed with the **least privilege principle**.
    - **No hardcoded credentials** in the source code.

3. **Monitoring & Logging:**
    - **VPC Flow Logs** enabled for traffic visibility.
    - **CloudWatch Alarms** configured for security events.
    - **GuardDuty** enabled for continuous threat detection.

**Architecture Decision:**

A **private subnet architecture** was considered but **not implemented** due to:

   - **Increased cost:** Additional $33/month for NAT Gateway or ~$14/month for VPC Endpoints.
   - **Not suitable for a learning/portfolio project scope**.
   - **Public subnet with proper security best practices** is sufficient for this use case.
   - Demonstrates **cost-aware decision making**.

This architecture is **appropriate for development or demo environments**.  
For **production deployments**, private subnets with NAT Gateways or VPC Endpoints should be used to enhance network 
isolation and security.

**Components trong EC2 Instance:**

1. **Nginx (Port 80/443)**
    - Reverse proxy và web server.
    - Serve React static files.
    - Proxy API requests to FastAPI.
    - SSL/TLS termination.
    - Gzip compression.

2. **FastAPI (Port 8000)**
    - RESTful API backend.
    - AWS SDK integration (boto3).
    - Business logic processing.
    - Authentication/Authorization.
    - Background task scheduling.

3. **Redis (Port 6379)**
    - Cache AWS API responses.
    - Cache DynamoDB query results.
    - Session storage.
    - Rate limiting.
    - Typical cache hit rate: 60-80%.

4. **React Frontend**
    - Single Page Application (SPA).
    - Dashboard visualization.
    - API client.
    - Real-time updates via polling/WebSocket.
    - Responsive design.

5. **CloudWatch Agent**
    - EC2 metrics collection.
    - Application logs shipping.
    - Custom metrics publishing.

**DynamoDB Table Design (4 Tables):**

To optimize **performance**, **data scalability**, and **separation of concerns**, the system uses **4 specialized DynamoDB tables**:

**Table 1: CloudHealthMetrics**.

```python
{
    "TableName": "CloudHealthMetrics",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # service#metric_name
        {"AttributeName": "sk", "KeyType": "RANGE"}   # ISO timestamp
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"},
        {"AttributeName": "gsi1_pk", "AttributeType": "S"}
    ],
    "GlobalSecondaryIndexes": [
        {
            "IndexName": "MetricNameIndex",
            "KeySchema": [
                {"AttributeName": "gsi1_pk", "KeyType": "HASH"},  # metric_name
                {"AttributeName": "sk", "KeyType": "RANGE"}        # timestamp
            ],
            "Projection": {"ProjectionType": "ALL"}
        }
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 30 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "EC2#CPUUtilization",
    "sk": "2025-09-22T14:30:00Z",
    "gsi1_pk": "CPUUtilization",  # For cross-service queries
    "value": 75.5,
    "unit": "Percent",
    "service": "EC2",
    "metric_name": "CPUUtilization",
    "dimensions": {"InstanceId": "i-1234567890abcdef0"},
    "ttl": 1669824000  # Unix timestamp
}
```

**Table 2: CloudHealthCosts**.

```python
{
    "TableName": "CloudHealthCosts",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # service
        {"AttributeName": "sk", "KeyType": "RANGE"}   # date#granularity
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"}
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 365 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "EC2",
    "sk": "2025-09-22#DAILY",
    "date": "2025-09-22",
    "granularity": "DAILY",
    "cost": 12.45,
    "usage_quantity": 24.0,
    "usage_unit": "Hrs",
    "currency": "USD",
    "tags": {"Environment": "Production"},
    "ttl": 1704067200  # Unix timestamp (1 year)
}
```

**Table 3: SecurityFindings**.

```python
{
    "TableName": "SecurityFindings",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # finding_type
        {"AttributeName": "sk", "KeyType": "RANGE"}   # finding_id
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"},
        {"AttributeName": "gsi1_pk", "AttributeType": "S"},
        {"AttributeName": "gsi1_sk", "AttributeType": "S"}
    ],
    "GlobalSecondaryIndexes": [
        {
            "IndexName": "SeverityIndex",
            "KeySchema": [
                {"AttributeName": "gsi1_pk", "KeyType": "HASH"},  # severity
                {"AttributeName": "gsi1_sk", "KeyType": "RANGE"}  # created_at
            ],
            "Projection": {"ProjectionType": "ALL"}
        }
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 90 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "GuardDuty",
    "sk": "finding-abc123def456",
    "gsi1_pk": "HIGH",  # For severity-based queries
    "gsi1_sk": "2025-09-22T14:30:00Z",
    "finding_id": "finding-abc123def456",
    "finding_type": "GuardDuty",
    "severity": "HIGH",
    "status": "ACTIVE",
    "title": "Suspicious network traffic detected",
    "description": "Unusual outbound traffic from EC2 instance to known malicious IP",
    "service": "EC2",
    "resource_id": "i-1234567890abcdef0",
    "resource_type": "Instance",
    "recommendation": "Review security group rules and investigate instance activity",
    "created_at": "2025-09-22T14:30:00Z",
    "updated_at": "2025-09-22T14:30:00Z",
    "ttl": 1677484800  # Unix timestamp (90 days)
}
```

**Table 4: Recommendations**.

```python
{
    "TableName": "Recommendations",
    "KeySchema": [
        {"AttributeName": "pk", "KeyType": "HASH"},   # type
        {"AttributeName": "sk", "KeyType": "RANGE"}   # created_at#rec_id
    ],
    "AttributeDefinitions": [
        {"AttributeName": "pk", "AttributeType": "S"},
        {"AttributeName": "sk", "AttributeType": "S"},
        {"AttributeName": "gsi1_pk", "AttributeType": "S"},
        {"AttributeName": "gsi1_sk", "AttributeType": "N"}
    ],
    "GlobalSecondaryIndexes": [
        {
            "IndexName": "ImpactIndex",
            "KeySchema": [
                {"AttributeName": "gsi1_pk", "KeyType": "HASH"},  # impact#service
                {"AttributeName": "gsi1_sk", "KeyType": "RANGE"}  # estimated_savings
            ],
            "Projection": {"ProjectionType": "ALL"}
        }
    ],
    "TimeToLiveSpecification": {
        "AttributeName": "ttl",
        "Enabled": true  # Auto-delete after 180 days
    },
    "BillingMode": "PAY_PER_REQUEST"
}

# Example item:
{
    "pk": "cost",
    "sk": "2025-09-22T14:30:00Z#rec-789xyz",
    "gsi1_pk": "HIGH#EC2",  # For impact-based queries
    "gsi1_sk": 50.00,  # estimated_savings for sorting
    "rec_id": "rec-789xyz",
    "type": "cost",
    "title": "Rightsize EC2 instance i-1234567890abcdef0",
    "description": "Instance runs at <10% CPU utilization. Consider downsizing from t3.large to t3.small",
    "impact": "HIGH",
    "effort": "LOW",
    "confidence": 0.92,
    "estimated_savings": 50.00,
    "estimated_savings_period": "monthly",
    "service": "EC2",
    "resource_id": "i-1234567890abcdef0",
    "resource_type": "Instance",
    "current_config": "t3.large",
    "recommended_config": "t3.small",
    "action_steps": [
        "1. Create AMI backup",
        "2. Stop instance",
        "3. Change instance type",
        "4. Restart and monitor"
    ],
    "implemented": false,
    "created_at": "2025-09-22T14:30:00Z",
    "updated_at": "2025-09-22T14:30:00Z",
    "ttl": 1685894400  # Unix timestamp (180 days)
}

```

### **Design Rationale**.

The decision to use **4 specialized tables** instead of 2 aggregated tables is based on clear architectural and operational benefits:

#### **1. Query Performance**

Each table is optimized for a **specific access pattern**:
   - **Metrics** → Time-series queries with high write throughput.
   - **Costs** → Aggregation queries by service and date range.
   - **Security** → Real-time filtering by severity level.
   - **Recommendations** → Impact-based sorting and status tracking.

This design minimizes query latency and avoids unnecessary data scans.

#### **2. Separation of Concerns**

   - Clear **data ownership** and **lifecycle management**.
   - Different **TTL (Time to Live)** policies for each data type.
   - Easier to maintain, debug, and evolve over time.

#### **3. Scalability**

   - Each table scales **independently** as workloads grow.
   - Avoids **hot partitions** caused by data spikes in one category.
   - GSIs (Global Secondary Indexes) tailored to specific query needs.

#### **4. Cost Management**

   - Enables **granular cost tracking** per data category.
   - Flexible TTLs reduce storage cost for short-lived data.
   - Uses **on-demand billing**, ideal for variable workloads.

#### **Trade-offs**

   - Slightly higher cost (~$2–3/month) compared to 2-table setup.
   - Slightly more complexity in deployment and maintenance.
   - However, gains include **better performance**, **cleaner architecture**, and **simpler scalability**.

### 4. Technical Implementation.

**Technology Stack**.

**Backend**.
   - Python 3.9+ with **FastAPI**.
   - **boto3** for AWS SDK.
   - **Redis** for caching.
   - **uvicorn** ASGI server.
   - **asyncio** for asynchronous operations.

**Frontend**.
   - **React 18** with Vite.
   - **TanStack Query (React Query)** for data fetching.
   - **Recharts** for data visualization.
   - **Tailwind CSS** for styling.
   - **Axios** as HTTP client.

**Database**.
   - **DynamoDB** as the primary database (4 specialized tables).
   - **Redis** as in-memory cache.

**DevOps**.
   - **Nginx** as reverse proxy.
   - **systemd** for service management.
   - **CloudWatch Agent** for monitoring.
   - **Bash scripts** for automation.

**Core Features Implementation**.

1. **Metrics Collection Service**.
    - Background job runs every **5 minutes**.
    - Collects metrics from **CloudWatch API**.
    - Persists data to the **CloudHealthMetrics** table.
    - Caches recent metrics in **Redis** (TTL = 5 minutes).

2. **Cost Analysis Service**.
    - Daily job collects cost data from **Cost Explorer API**.
    - Stores historical cost records in **CloudHealthCosts** table.
    - Generates charts and trend data for the dashboard.
    - Surfaces **AWS native recommendations** (Cost Explorer, Trusted Advisor).

3. **Security Monitoring**.
    - Polls **Security Hub** and **GuardDuty** findings.
    - Persists findings into the **SecurityFindings** table.
    - Displays active findings with **severity filtering**.
    - Triggers alerts for **high/critical** severity items.
    - Read-only integration to avoid altering customer security state.

4. **Recommendations Engine**.
    - Analyzes metrics, cost, and security datasets.
    - Generates **actionable recommendations**.
    - Stores suggestions in the **Recommendations** table.
    - Prioritizes recommendations by **impact** and **confidence**.
    - Tracks implementation status.

5. **API Endpoints**.
    - `GET /api/v1/metrics` - Retrieve metrics from DynamoDB.
    - `GET /api/v1/costs` - Cost data and trends.
    - `GET /api/v1/security` - Security findings.
    - `GET /api/v1/recommendations` - Optimization recommendations.
    - `GET /api/v1/health` - Health check.
    - `WS /ws` - WebSocket cho real-time updates (optional).

6. **Caching Strategy**.
    - Redis cache for frequent queries.
    - **5-minute TTL** for metrics data.
    - **1-hour TTL** for cost data.
    - **30-minute TTL** for security findings.
    - **2-hour TTL** for recommendations.
    - Cache invalidation on data refresh.

7. **Security Measures**
    - **HTTPS** only with Let's Encrypt.
    - **IAM role**s with least privilege.
    - **Security groups** restrictive rules.
    - No **hardcoded credentials**.
    - **VPC Flow Logs** enabled.

### 5. Roadmap & Implementation Milestones.

**MVP Features (3 months, 4-member team):**

```
MONTH 1: Core Infrastructure & Basic Features.
├─ WEEK 1: Infrastructure Setup (1 DevOps + 3 support).
│  ├─ EC2 instance launch and configuration.
│  ├─ DynamoDB tables creation (4 tables with GSIs).
│  ├─ Security groups, IAM roles.
│  ├─ Development environment setup.
│  └─ Deliverable: Working infrastructure with 4 DynamoDB tables.
│
├─ WEEK 2: Backend Foundation (2 Backend + 2 Frontend start).
│  ├─ FastAPI skeleton with basic endpoints.
│  ├─ DynamoDB connection and basic CRUD for 4 tables.
│  ├─ CloudWatch integration (read metrics).
│  ├─ React app initialization.
│  └─ Deliverable: API responding, Frontend routing.
│
├─ WEEK 3: Core Monitoring (2 Backend + 2 Frontend).
│  ├─ Metrics collection background job.
│  ├─ Store metrics in CloudHealthMetrics table.
│  ├─ Basic dashboard page với charts.
│  ├─ Display real CloudWatch data.
│  └─ Deliverable: Metrics monitoring working.
│
└─ WEEK 4: Cost Integration (2 Backend + 2 Frontend).
   ├─ Cost Explorer API integration.
   ├─ Cost data storage in CloudHealthCosts table.
   ├─ Cost dashboard page.
   ├─ Charts and trends visualization.
   └─ Deliverable: Month 1 MVP - Metrics + Costs monitoring.

MONTH 2: Additional Features & Polish.
├─ WEEK 5: Redis Caching (1 Backend + 1 DevOps + 2 Frontend).
│  ├─ Redis setup and integration.
│  ├─ Cache layer implementation for 4 tables.
│  ├─ Performance optimization.
│  ├─ Frontend improvements.
│  └─ Deliverable: Improved performance.
│
├─ WEEK 6: Security Monitoring (2 Backend + 2 Frontend).
│  ├─ Security Hub and GuardDuty integration.
│  ├─ Store findings in SecurityFindings table.
│  ├─ Security dashboard page and severity filtering.
│  ├─ Alert system basic.
│  └─ Deliverable: Security visibility.
│
├─ WEEK 7: Recommendations Engine (2 Backend + 2 Frontend).
│  ├─ AWS Cost Explorer recommendations integration.
│  ├─ Rule-based cost optimization logic.
│  ├─ Store in Recommendations table.
│  ├─ Recommendations UI with impact sorting.
│  ├─ Testing and bug fixes.
│  └─ Deliverable: Optimization suggestions.
│
└─ WEEK 8: Integration & Testing (All 4).
   ├─ End-to-end testing across 4 tables.
   ├─ Bug fixes.
   ├─ Performance tuning.
   ├─ Documentation.
   └─ Deliverable: Stable application.

MONTH 3: Production Ready & Deployment.
├─ WEEK 9: Deployment Automation (1 DevOps + 3 support).
│  ├─ SSL certificates setup.
│  ├─ Nginx configuration.
│  ├─ Systemd services.
│  ├─ Monitoring setup.
│  └─ Deliverable: Production deployment.
│
├─ WEEK 10: Documentation & Polish (All 4).
│  ├─ User documentation.
│  ├─ API documentation.
│  ├─ DynamoDB schema documentation.
│  ├─ Deployment guide.
│  ├─ UI/UX improvements.
│  └─ Deliverable: Production-ready system.
│
├─ WEEK 11: Testing & Bug Fixes (All 4).
│  ├─ Load testing.
│  ├─ Security testing.
│  ├─ DynamoDB performance testing.
│  ├─ Bug fixing.
│  ├─ Performance optimization.
│  └─ Deliverable: Stable production.
│
└─ WEEK 12: Demo Preparation (All 4).
   ├─ Demo environment setup.
   ├─ Presentation materials.
   ├─ Final testing.
   ├─ Knowledge transfer.
   └─ Deliverable: Project handover.

Post-deployment: Maintenance & Enhancements.
└─ Monitoring, bug fixes, feature requests.
```

**Phase 2 (Future Enhancements - 3-6 months after):**

   - **Machine Learning** cost prediction models.
   - Multi-account support.
   - Advanced analytics.
   - **Well-Architected Framework** assessment.
   - **GuardDuty** deep integration.
   - **AWS Config** compliance tracking.
   - Custom alerting workflows.
   - **Mobile** responsive improvements.
   - Advanced recommendation algorithms.

### 6. Budget Estimation.

**AWS Infrastructure Cost (Monthly):**

| Service               | Description                | Month 1  | Month 2   | Month 3    |
|-----------------------|----------------------------|----------|-----------|------------|
| **EC2 t3.micro**      | 750h Free Tier             | $0       | $0        | $0         |
| **DynamoDB**          | On-demand, 4 tables        | $3-4     | $4-6      | $6-8       |
| **CloudWatch**        | Metrics + Logs (Free Tier) | $0-1     | $1-2      | $2-3       |
| **Data Transfer**     | **15GB Free Tier**         | $0       | $0-1      | $1         |
| **S3**                | Backup storage (~5GB)      | $0       | $0-1      | $1         |
| **Security Services** | GuardDuty (optional)       | $0       | $1        | $1-2       |
| **TOTAL**             |                            | **$3-5** | **$6-11** | **$11-18** |

**Average 12-Month Cost:** $12–18/month.

**DynamoDB Cost Breakdown (4 Tables):**

- **Storage:** ~5GB total = $1.25/month.
    + CloudHealthMetrics: 2GB ($0.50).
    + CloudHealthCosts: 1GB ($0.25).
    + SecurityFindings: 1GB ($0.25).
    + Recommendations: 1GB ($0.25).

- **Writes:** ~800K requests/month = $1.00/month.
    + Metrics: 500K writes ($0.625).
    + Costs: 100K writes ($0.125).
    + Security: 100K writes ($0.125).
    + Recommendations: 100K writes ($0.125).

- **Reads:** ~4M requests/month = $2.00/month.
    + Evenly distributed across 4 tables.

- **GSI:** Included in on-demand pricing.
- **Backup:** Free (Point-in-time recovery).
- **Total DynamoDB Cost:** $4–7/month.

**Note:** Costs may increase if:
   - Higher metric collection frequency.
   - More AWS services being monitored.
   - Longer data retention periods.
   - GuardDuty enabled (+$4–6/month).
   - Increased security findings and recommendations.

**Cost Optimization Strategies:**
   - Fully utilize **AWS Free Tier** (first 12 months).
   - Use **on-demand mode** instead of provisioned capacity.
   - Apply **TTL** for automatic data expiration per table.
   - Use **Redis caching** to reduce DynamoDB reads.
   - Set **CloudWatch log retention** to 7 days.
   - Disable monitoring for unused AWS services.
   - Perform **batch writes** where possible.
   - Design **efficient query patterns** with GSIs.

### 7. Risk Assessment.

**Risk Matrix:**

| Risk                        | Impact Level | Probability | Overall Risk |
|-----------------------------|--------------|-------------|--------------|
| **Budget** overrun          | Medium       | Medium      | Medium       |
| **EC2** downtime            | High         | Low         | Medium       |
| **Scope** creep             | Medium       | High        | High         |
| **Technical** complexity    | Medium       | Medium      | Medium       |
| **AWS API** rate limits     | Low          | Medium      | Low          |
| **DynamoDB** hot partitions | Low          | Low         | Low          |
| Team coordination issues    | Medium       | Medium      | Medium       |

**Mitigation Strategies:**

1. **Cost Management**
    - AWS Budget alerts set at **$15 and $20** thresholds.
    - Daily cost monitoring in AWS Cost Explorer.
    - DynamoDB on-demand mode (avoids unexpected bills).
    - Monitor DynamoDB cost per table.
    - Weekly cost review meetings.
    - Kill switch to disable data collection if budget exceeds limits.

2. **EC2 Availability**
    - CloudWatch alarms for health checks.
    - Systemd configured for automatic service restarts.
    - Health check endpoint for uptime validation.
    - Backup and recovery procedures documented.
    - Target uptime: **98–99%** (realistic for single-instance deployment).

3. **Scope Creep**
    - Strict MVP definition.
    - Feature freeze after **Week 8**.
    - Maintain a “Nice-to-Have” list for Phase 2.
    - Weekly standups to track progress.
    - Prioritize must-have features.
    - Document 4-table design clearly for alignment.

4. **Technical Complexity**
    - Begin with the simplest viable solutions.
    - Use AWS documentation extensively.
    - Establish a code review process.
    - Conduct technical spikes for unknown areas.
    - Run DynamoDB data modeling workshops.
    - Consult mentors when encountering blockers.

5. **API Rate Limits**
    - Implement **exponential backoff** on API retries.
    - Aggressive Redis caching to reduce API calls.
    - Monitor API usage metrics.
    - Stay within AWS Free Tier limits.
    - Use batch operations where possible.

6. **DynamoDB Hot Partitions**
    - Proper partition key design.
    - Write sharding for high-traffic tables.
    - Monitor CloudWatch metrics for throttling.
    - Efficient use of **Global Secondary Indexes (GSI)**.

7. **Team Coordination**
    - Daily 15-minute standup meetings.
    - Clear task ownership and accountability.
    - Git workflow with feature branches and pull requests.
    - Continuous documentation from Day 1.
    - Shared knowledge base for all members.
    - Maintain up-to-date DynamoDB schema documentation.

### Contingency Plan

1. **Service Failure**
    - Systemd auto-restart configuration.
    - Manual recovery steps documented.

2. **Data Loss**
    - Daily backups to **Amazon S3**.
    - **DynamoDB Point-in-Time Recovery (PITR)** enabled.

3. **Cost Spike**
    - Immediate budget alert notification.
    - Manual review and throttle data collection.

4. **Schedule Delays**
    - **De-scope** Phase 2 features.
    - Focus resources solely on **MVP** delivery.

5. **DynamoDB Performance Issues**
    - Fallback to direct **AWS API** calls.
    - Reduce write frequency temporarily.

### 8. Expected Outcomes.

**Technical Deliverables:**

1. **Working Dashboard:**
    - 4 main pages: **Metrics, Costs, Security, Recommendations**.
    - Real data from **AWS services**.
    - Responsive design.
    - Basic authentication.

2. **Data Collection System:**
    - Background jobs collecting metrics per 5 minutes.
    - Cost data updated daily.
    - Security findings updated hourly.
    - Recommendations generated daily.
    - Data stored in **4 specialized DynamoDB tables**.

3. **Performance:**
    - **Cached response time**: < 300ms (60-80% requests).
    - **Uncached response time**: < 2 seconds.
    - **DynamoDB query time**: 10-25ms (typical).
    - **GSI query time**: 15-30ms.
    - **Target uptime**: 98-99% (single instance).

4. **Cost Analysis:**
    - Historical cost trends.
    - Service breakdown charts.
    - **AWS Cost Explorer** recommendations display.
    - **Budget alerts**.
    - **Cost forecasting**.

5. **Security Visibility:**
    - **Security Hub** findings dashboard.
    - **GuardDuty** threat detection.
    - **Severity-based** filtering using **GSI**.
    - Real-time alerts cho critical findings.
    - Compliance status overview.

6. **Recommendations System:**
    - Cost optimization suggestions.
    - Performance improvement recommendations.
    - Security enhancement suggestions.
    - Impact-based prioritization using **GSI**.
    - Implementation tracking.

**Learning Outcomes:**

1. **AWS Services:**
    - Hands-on experience with **EC2, DynamoDB (advanced), CloudWatch**.
    - **Cost Explorer API integration**.
    - **Security Hub and GuardDuty** understanding.
    - **IAM roles and policies**.
    - **VPC** networking basics.
    - **DynamoDB** data modeling best practices.

2. **Full-Stack Development:**
    - **FastAPI** backend development.
    - React frontend development.
    - **RESTful API** design.
    - NoSQL database design patterns.
    - DynamoDB access patterns và **GSI** optimization.
    - Caching strategies.

3. **DevOps:**
    - **Linux server administration**.
    - **Nginx configuration**.
    - **Service management với system**.
    - **SSL/TLS** setup.
    - Monitoring và logging.
    - Database backup strategies.

4. **Best Practices:**
    - Security-first design.
    - Cost optimization.
    - Code organization.
    - Documentation.
    - Testing strategies.
    - **NoSQL** data modeling.

**Portfolio Value:**

Project này demonstrate:

   - Real **AWS production experience**.
   - Advanced DynamoDB data modeling **(4 tables, GSI)**.
   - Full-stack development skills.
   - Cost-aware architecture decisions.
   - Security consciousness.
   - Realistic project scoping.
   - Team collaboration.
   - Professional documentation.

**Limitations & Trade-offs:**

Documented limitations:

   - Single instance **(no high availability)**.
   - Public subnet **(no private network isolation)**.
   - Rule-based recommendations **(no ML-powered initially)**.
   - Manual **AWS service** additions.
   - Limited to **AWS (no multi-cloud)**.

Documented trade-offs:

   - Using **4 tables** increases complexity and cost **($2–3/month)** but improves performance and maintainability.
   - Hosting the **EC2 instance** in a public subnet reduces security but saves approximately **$33/month**.
   - Operating with a single instance lowers availability but aligns with the project's budget constraints.

These limitations and trade-offs are **conscious decisions** made for **cost efficiency** and **timeline feasibility**, demonstrating **real-world engineering judgment** and prioritization under resource constraints.

**A. Links:**
- [GitHub Repository](https://github.com/Unvianpetronas/Cloud_health_dashboard)

**B. Contact:**
   - **Project Leader:** Truong Quoc Tuan .
   - **Email:** unviantruong26@gmail.com .
   - **WhatsApp:** 0798806545 .
```

---


