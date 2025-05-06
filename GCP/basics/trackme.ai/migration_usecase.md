# Migration Use Case: AWS to GCP for `trackme.ai`

## Core Services and Migration Changes

| **AWS Service**                  | **GCP Equivalent**                                    | **Migration Changes**                                                                                   | **Bonus Links**                                                                                           |
|-----------------------------------|-------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **EC2 (Elastic Compute Cloud)**   | **Compute Engine**                                    | Migrate compute instances; choose appropriate machine types, preemptible VMs for cost savings.          | [Compute Engine Documentation](https://cloud.google.com/compute/docs)                                    |
| **S3 (Simple Storage Service)**   | **Cloud Storage**                                     | Transfer object storage; configure storage classes and lifecycle rules for cost optimization.           | [Cloud Storage Documentation](https://cloud.google.com/storage/docs)                                     |
| **RDS (Relational Database Service)** | **Cloud SQL** or **Cloud Spanner**                   | Migrate databases; evaluate between Cloud SQL (managed relational DB) and Cloud Spanner (horizontal scalability). | [Cloud SQL Documentation](https://cloud.google.com/sql/docs) \| [Cloud Spanner Documentation](https://cloud.google.com/spanner/docs) |
| **DynamoDB**                      | **Firestore** or **Bigtable**                        | Transition NoSQL workloads; Firestore for app-centric development, Bigtable for analytical workloads.   | [Firestore Documentation](https://cloud.google.com/firestore/docs) \| [Bigtable Documentation](https://cloud.google.com/bigtable/docs) |
| **Elastic Load Balancer (ELB)**   | **Cloud Load Balancing**                              | Reconfigure load balancers; ensure global load balancing and autoscaling are enabled.                   | [Cloud Load Balancing Documentation](https://cloud.google.com/load-balancing/docs)                       |
| **VPC (Virtual Private Cloud)**   | **VPC (Virtual Private Cloud)**                       | Recreate network topology; configure subnets, routes, and firewall rules.                               | [VPC Documentation](https://cloud.google.com/vpc/docs)                                                   |
| **IAM (Identity and Access Management)** | **IAM (Identity and Access Management)**             | Align user roles, permissions, and service accounts with GCP's IAM model.                               | [IAM Documentation](https://cloud.google.com/iam/docs)                                                   |
| **CloudWatch**                    | **Cloud Monitoring** and **Cloud Logging**           | Redefine monitoring and logging; configure dashboards, alerts, and log sinks.                          | [Cloud Monitoring Documentation](https://cloud.google.com/monitoring/docs) \| [Cloud Logging Documentation](https://cloud.google.com/logging/docs) |
| **SNS/SQS (Messaging Services)**  | **Pub/Sub**                                           | Migrate messaging queues; configure topics, subscriptions, and dead-letter queues.                      | [Pub/Sub Documentation](https://cloud.google.com/pubsub/docs)                                            |
| **AWS Lambda**                    | **Cloud Functions**                                   | Rewrite serverless functions; ensure compatibility with GCP triggers and runtime environments.          | [Cloud Functions Documentation](https://cloud.google.com/functions/docs)                                 |
| **Elastic Kubernetes Service (EKS)** | **Google Kubernetes Engine (GKE)**                   | Reconfigure Kubernetes clusters; leverage GKE's autopilot mode for managed operations.                  | [GKE Documentation](https://cloud.google.com/kubernetes-engine/docs)                                     |

## Additional Recommendations

1. **Billing and Cost Management**
   - Use **Cloud Billing** to monitor and optimize costs.
   - Set up budgets and alerts to track spending.
   - [Cloud Billing Documentation](https://cloud.google.com/billing/docs)

2. **Data Transfer**
   - Leverage **Storage Transfer Service** for bulk data migrations.
   - Use **Transfer Appliance** for petabyte-scale data transfer.
   - [Storage Transfer Service Documentation](https://cloud.google.com/storage-transfer/docs)

3. **Hybrid and Multi-Cloud Solutions**
   - Utilize **Anthos** for hybrid deployments and consistent policy enforcement.
   - [Anthos Documentation](https://cloud.google.com/anthos/docs)

4. **Security**
   - Implement **Cloud Armor** for DDoS protection.
   - Configure **Security Command Center** for threat detection and compliance.
   - [Cloud Security Documentation](https://cloud.google.com/security/docs)

---

## Related Files in the Repository

- [migration_usecase.md](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/migration_usecase.md): This file contains detailed migration steps.
- [GCP/VPC/network_config.md](#): Placeholder for network configurations (add link if file exists).
- [GCP/IAM/roles_and_permissions.md](#): Placeholder for IAM setup details (add link if file exists).

---

### Notes for the Exam
- Understand the pros and cons of each GCP service.
- Focus on **cost optimization**, **scalability**, and **high availability** when planning migrations.
- Practice with Google Cloud documentation and hands-on labs.

Good luck with your migration and exam prep!
