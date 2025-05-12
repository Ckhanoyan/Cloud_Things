### Database Design for trackme.ai App

This checklist will guide you in selecting the appropriate database technologies for your global app, **trackme.ai**. Each section includes recommendations based on your requirements, linked to relevant GCP services and documentation.

---

#### **Global User Base and High Availability**
- [ ] **Multi-Regional Deployment**
  - **Recommendation**: [Cloud Spanner](https://cloud.google.com/spanner)
  - **Use Case**: Cloud Spanner ensures low-latency reads and high availability for users accessing the app from different parts of the world.
  - [Related File: databases.md](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/databases.md)

- [ ] **Read Replicas**
  - **Recommendation**: [Cloud SQL](https://cloud.google.com/sql) with read replicas
  - **Use Case**: Optimize for read-heavy workloads like leaderboard queries without impacting the primary database.

---

#### **Workouts and Leaderboard (Structured Data)**
- [ ] **Relational Database for Structured Data**
  - **Recommendation**: [Cloud SQL](https://cloud.google.com/sql) (PostgreSQL or MySQL)
  - **Use Case**: Leaderboard rankings require consistent and accurate aggregation of scores.
  - [Related File: databases.md](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/databases.md)

- [ ] **Global Consistency**
  - **Recommendation**: [Cloud Spanner](https://cloud.google.com/spanner)
  - **Use Case**: Users posting workouts and leaderboard updates should be consistent across regions.

---

#### **Messaging (Semi-Structured or High-Volume Data)**
- [ ] **NoSQL Database for Messaging**
  - **Recommendation**: [Firestore](https://cloud.google.com/firestore) for real-time messaging
  - **Use Case**: Firestore supports real-time data synchronization for chat applications.

- [ ] **High Write Throughput**
  - **Recommendation**: [Bigtable](https://cloud.google.com/bigtable)
  - **Use Case**: Handle high write throughput for messaging or notifications.

---

#### **Analytics and Reporting**
- [ ] **Data Warehouse for Analytics**
  - **Recommendation**: [BigQuery](https://cloud.google.com/bigquery)
  - **Use Case**: Analyze workout trends, user engagement, or leaderboard statistics.

- [ ] **Batch Ingestion**
  - **Recommendation**: [Pub/Sub](https://cloud.google.com/pubsub) to stream app events into BigQuery
  - **Use Case**: Aggregate and analyze user activity logs for trends or reporting.

---

#### **Disaster Recovery (RTO & RPO)**
- [ ] **Low RTO and RPO**
  - **Recommendation**: [Cloud Spanner](https://cloud.google.com/spanner)
  - **Use Case**: Built-in high availability and multi-region failover ensure minimal downtime.

- [ ] **Backup and Restore**
  - **Recommendation**: [Cloud SQL](https://cloud.google.com/sql) with automated backups and PITR (Point-In-Time Recovery)
  - **Use Case**: Protect against accidental deletion or corruption of data.

---

#### **Scalability**
- [ ] **Horizontal Scalability**
  - **Recommendation**: [Bigtable](https://cloud.google.com/bigtable)
  - **Use Case**: Store time-series data (e.g., workout records, logs, or events).

- [ ] **Vertical Scalability**
  - **Recommendation**: [Cloud SQL](https://cloud.google.com/sql)
  - **Use Case**: Scale up instances for relational database needs.

---

### **Key Decision Points**
#### **SQL vs. NoSQL**
1. **SQL**:
   - **Options**: Cloud SQL, Cloud Spanner
   - **Use Case**: Structured data like workouts, leaderboard rankings, and transactions where consistency is crucial.
   - **Pro**: Strong ACID compliance, relationships, and schema.
   - **Con**: Scalability can be limited for extremely high write throughput compared to NoSQL.

2. **NoSQL**:
   - **Options**: Firestore, Bigtable
   - **Use Case**: Semi-structured/unstructured data like chat messages, user preferences, and high write throughput workloads.
   - **Pro**: Scalability and flexibility for unstructured data.
   - **Con**: Limited transactional capabilities compared to SQL.

#### **RTO & RPO**
- For a **global app**, prioritize **Cloud Spanner** for its automatic multi-region replication and near-zero RTO/RPO.

#### **Read Replicas**
- Use **Cloud SQL** if you need relational database capabilities but want to optimize for read-heavy workloads like leaderboard queries.

---

### **Recommendations**
1. **Primary Database**:
   - Use **Cloud Spanner** for workouts, leaderboard, and global consistency.
   - Justification: Cloud Spanner provides the scalability, consistency, and multi-regional replication needed for your app.

2. **Messaging**:
   - Use **Firestore** for real-time message synchronization.
   - Justification: Firestore is optimized for semi-structured data and real-time use cases like chat.

3. **Analytics**:
   - Use **BigQuery** for analytics and reporting.
   - Justification: BigQuery excels at analyzing large datasets and providing insights.

4. **Backup and Disaster Recovery**:
   - Ensure **Cloud SQL** or **Cloud Spanner** has automated backups and multi-region failover enabled.
