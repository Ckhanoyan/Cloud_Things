# GCP Study Guide

## Overview
This study guide is designed to enhance your understanding of Google Cloud Platform (GCP) and its application to real-world use cases, such as the Strava-like app documented in this repository. Each section includes actionable study tasks, links to official documentation, and additional resources to deepen your expertise.

---

## 1. Core Compute Services
- **[Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine/docs)**: Learn about cluster setup, workload management, and scaling.
  - [ ] Explore GKE Autopilot vs Standard GKE.
  - [ ] Study Kubernetes best practices for microservices.
  - Related File: [Microservices on GKE](./GCP/basics/stravalike_app/k8.md#L17-L33)

- **[Compute Engine](https://cloud.google.com/compute/docs)**: Understand VM instances, networking, and autoscaling.
  - [ ] Practice creating and managing VMs using both UI and CLI.
  - [ ] Experiment with setting up a Strava-like backend on Compute Engine.
  - Related File: [Compute Engine Setup](./GCP/basics/stravalike_app/compute_engine.md)

---

## 2. Storage Solutions
- **[Cloud Storage](https://cloud.google.com/storage/docs)**: Study object storage configurations (multi-region, dual-region).
  - [ ] Review best practices for storing large static assets like GPS files.
  - [ ] Experiment with lifecycle policies for cost optimization.

- **[Cloud SQL](https://cloud.google.com/sql/docs)** and **[Firestore](https://cloud.google.com/firestore/docs)**: Understand relational and NoSQL database options.
  - [ ] Compare use cases for Cloud SQL vs Firestore.
  - [ ] Set up a test database and practice querying data.

---

## 3. Networking
- **[Cloud Load Balancing](https://cloud.google.com/load-balancing/docs)**: Learn about traffic distribution and SSL termination.
  - [ ] Study global vs regional load balancing.
  - [ ] Configure a load balancer for a multi-region setup.

- **[VPC](https://cloud.google.com/vpc/docs)**: Explore networking fundamentals.
  - [ ] Practice creating custom VPCs with subnets and firewall rules.

---

## 4. Event-Driven Architecture
- **[Cloud Pub/Sub](https://cloud.google.com/pubsub/docs)**: Understand event-based messaging for distributed systems.
  - [ ] Study patterns for asynchronous task handling.
  - [ ] Implement a sample use case using Pub/Sub to trigger events.

---

## 5. CI/CD and Monitoring
- **[Cloud Build](https://cloud.google.com/build/docs)**: Learn continuous integration and deployment workflows.
  - [ ] Experiment with automating deployments to GKE.
  - Related File: [CI/CD Setup](./GCP/basics/stravalike_app/k8.md#L53-L70)

- **[Monitoring with Prometheus and Grafana](https://prometheus.io/docs/introduction/overview/)**:
  - [ ] Set up a monitoring stack in GCP.
  - [ ] Create dashboards for microservices performance.

---

## 6. Security and IAM
- **[IAM Roles](https://cloud.google.com/iam/docs)**: Study access control and permissions.
  - [ ] Practice assigning roles and managing service accounts.

- **[Secrets Manager](https://cloud.google.com/secret-manager/docs)**: Learn secure storage of API keys and credentials.
  - [ ] Create and manage secrets in a test project.

---

## 7. Advanced Topics
- **[BigQuery](https://cloud.google.com/bigquery/docs)**: Explore advanced analytics for large datasets.
  - [ ] Practice querying data and integrating with other services.

- **[OpenTelemetry](https://opentelemetry.io/)**: Learn about distributed tracing for observability.
  - [ ] Set up tracing for a sample application.

---

## 8. Migration and Best Practices
- **[AWS to GCP Migration](https://cloud.google.com/architecture/migrating-aws-to-gcp)**:
  - [ ] Study the differences in services like S3 vs Cloud Storage, and IAM policies.
  - Related File: [Migration Notes](./GCP/basics/stravalike_app/usecase.md)

- **[High Availability and Disaster Recovery](https://cloud.google.com/architecture)**:
  - [ ] Review strategies for multi-region deployments and backups.
  - Related File: [Availability Recommendations](./GCP/basics/stravalike_app/availability.md)

---

## Additional Resources
- **Official GCP Documentation**: [Google Cloud Docs](https://cloud.google.com/docs)
- **Interactive Labs**: [Qwiklabs](https://www.qwiklabs.com/)
- **Certifications**: [Prepare for GCP Certifications](https://cloud.google.com/certification)

---

## Action Plan
1. Complete the above study tasks section by section.
2. Document key takeaways and practical insights in this repository.
3. Share your progress and seek feedback from the community.

Good luck, and happy learning!
