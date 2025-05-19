# GCP IoT Analytics Pipeline: Wahoo Bike Computer Power Data

Prepare for the Google Cloud Professional Cloud Architect exam by reviewing how to architect a solution for ingesting, processing, and analyzing cycling power data from Wahoo bike computers. This scenario demonstrates practical use of core GCP services and how they interact in a real-world IoT analytics pipeline.

---

## GCP Services Checklist for Wahoo Power Data

### Compute & Processing

- [ ] **Cloud Functions / Cloud Run**
  - *Purpose*: Serverless, event-driven processing of device data.
  - *Use Case*: Triggered by Pub/Sub messages to parse, validate, and enrich power readings.
  - *Example*: A function processes each cycling power data point and writes results to BigQuery.
  - [Cloud Functions Docs](https://cloud.google.com/functions)

---

### Data Ingestion & Messaging

- [ ] **Pub/Sub**
  - *Purpose*: Scalable, decoupled messaging between producers (apps/devices) and consumers (data processors).
  - *Use Case*: Mobile app or gateway publishes power readings; backend services subscribe for real-time processing.
  - [Pub/Sub Example in Your Repo](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/pub_sub.md)
  - [Pub/Sub Docs](https://cloud.google.com/pubsub/docs)

---

### Storage

- [ ] **BigQuery**
  - *Purpose*: Serverless, scalable data warehouse for analytics.
  - *Use Case*: Store all device readings; run SQL analytics for trends, reports, and dashboards.
  - *Scenario*: Calculate average power, peak intervals, and visualize user progress over time.
  - [BigQuery Docs](https://cloud.google.com/bigquery/docs)

- [ ] **Cloud Storage**
  - *Purpose*: Raw file and batch data storage.
  - *Use Case*: Archive original ride files (e.g., FIT, TCX, GPX) for compliance or later batch processing.
  - [Cloud Storage Docs](https://cloud.google.com/storage/docs)

---

### Identity & Security

- [ ] **IAM (Identity & Access Management)**
  - *Purpose*: Secure access, principle of least privilege, and auditability.
  - *Use Case*: Restrict data ingestion, processing, and viewing to only authorized users, apps, and service accounts.
  - [IAM Docs](https://cloud.google.com/iam/docs)

---

## (Retired) IoT Device Management

- [ ] **IoT Core** *(sunset, but exam-relevant)*
  - *Purpose*: Secure device registration, authentication, and telemetry streaming.
  - *Use Case*: If available, Wahoo devices would authenticate and send data to IoT Core, which then forwards to Pub/Sub.
  - [GCP IoT Solutions](https://cloud.google.com/solutions/iot)


