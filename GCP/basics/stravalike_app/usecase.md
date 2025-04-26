# Migrating Strava from AWS to Google Cloud Platform (GCP)

## Core Principles for Cloud Migration (AWS → GCP)

| **Topic** | **AWS** | **GCP Equivalent** | **What Changes?** |
|:---|:---|:---|:---|
| Compute | EC2 (VMs), ECS, EKS (Kubernetes) | GKE (Google Kubernetes Engine) | Move workloads to GKE clusters |
| Networking | VPC, ELB, Route 53 | VPC, Cloud Load Balancing, Cloud DNS | Map networking rules and services carefully |
| Storage | S3 (object storage) | GCS (Google Cloud Storage) | Migrate large datasets carefully |
| IAM/Security | IAM, KMS, Secrets Manager | IAM, KMS, Secret Manager | Rebuild roles, permissions, secrets |
| Database | RDS, DynamoDB | Cloud SQL, Firestore, Bigtable | Map data models and services to GCP databases |
| Monitoring | CloudWatch, X-Ray | Cloud Monitoring, Cloud Trace, Cloud Logging | Update observability stack |
| Serverless | Lambda | Cloud Functions, Cloud Run | Optional serverless migration |
| Messaging | SNS/SQS | Pub/Sub | Different service characteristics |

---

## Kubernetes-Specific Best Practices (GKE)

| **Topic** | **Best Practice on AWS** | **GCP Migration Notes** |
|:---|:---|:---|
| Kubernetes | EKS, manually scaling clusters | Use GKE Autopilot for easy scaling or Standard GKE for full control |
| Service Mesh | Istio/Linkerd (optional) | Anthos Service Mesh (optional, integrated into GCP) |
| Ingress Controller | AWS ALB Ingress Controller | Use GKE-native HTTP(S) Load Balancer with GKE Ingress |
| Persistent Volumes | EBS volumes | Switch to GCP Persistent Disks (dynamic provisioning) |
| Secrets Management | AWS Secrets Manager / Kubernetes Secrets | Use GCP Secret Manager integrated with GKE |
| Monitoring | Prometheus/Grafana + CloudWatch | Use GCP Managed Prometheus + Cloud Logging/Monitoring |
| CI/CD | GitHub Actions, AWS CodePipeline | Use Cloud Build, Cloud Deploy or GitHub pipelines |
| Data Storage | DynamoDB, RDS | Cloud SQL, Firestore, or Bigtable based on workload |

---

## Microservices Migration for Strava

| **Service** | **Considerations** | **Changes on GCP** |
|:---|:---|:---|
| Leaderboard | - High-concurrency read<br>- Consistency critical | - Cache hot data with MemoryStore (Redis)<br>- Use Cloud SQL (Postgres) for transactions |
| Kudos | - High volume fanout writes<br>- Event-driven architecture | - Use Pub/Sub for async fanout<br>- Batch writes into databases |
| Messaging | - Real-time message delivery<br>- High reliability | - Use Pub/Sub queues<br>- Cloud Run for real-time API endpoints<br>- Store messages in Firestore or Cloud SQL |

---

## Key Challenges to Solve

- Identity Federation: map IAM roles across AWS to GCP IAM policies
- Secrets Management: migrate and rotate sensitive credentials securely
- Data Transfer: careful planning if large-scale data migration (S3 → GCS)
- Kubernetes Upgrades: ensure microservices are forward-compatible on GKE
- Observability: rebuild monitoring dashboards (GKE-native or Prometheus)

---

## High-Level Migration Strategy

1. **Lift-and-shift Kubernetes apps** first (low-risk microservices)
2. **Refactor data layers** carefully — treat databases as critical
3. **Rebuild monitoring and alerting** using GCP Cloud Monitoring and Logging
4. **Optimize cost models** post-migration (GKE Autopilot, Committed Use Discounts)
5. **Integrate AI/ML personalization** using Vertex AI for new features

---

## Bonus (Why GCP fits Strava)

- **GKE Autopilot** = serverless Kubernetes clusters without ops overhead
- **Pub/Sub** = highly scalable event-driven architecture
- **Vertex AI** = future-ready platform to personalize Strava feeds and recommendations
- **Global Load Balancers** = world-class latency, ideal for global Strava users

---

# Architecture Diagram

```mermaid
flowchart TD
    User --> GlobalLoadBalancer
    GlobalLoadBalancer --> GKEIngress
    GKEIngress --> GKECluster
    GKECluster -->|Services| Microservices[Leaderboard, Kudos, Messaging]
    Microservices -->|DB Reads/Writes| CloudSQL[(Cloud SQL / Firestore)]
    Microservices --> PubSub[(Pub/Sub)]
    PubSub --> CloudFunctions[(Cloud Functions for Async Tasks)]
    Monitoring -->|Metrics| CloudMonitoring[(Cloud Monitoring)]
    Logging -->|Logs| CloudLogging[(Cloud Logging)]
