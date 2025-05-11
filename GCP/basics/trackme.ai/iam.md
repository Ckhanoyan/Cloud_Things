### IAM Best Practices for `trackme.ai`

#### Checklist:
- [ ] **Principle of Least Privilege**  
  Assign roles that provide only the permissions necessary to perform a task. Avoid granting overly permissive roles like `Owner` or `Editor` at the project level.  
  Example: Grant the `Storage Object Viewer` role for read access to a specific GCS bucket instead of `Storage Admin`.

- [ ] **Use Predefined Roles Instead of Basic Roles**  
  Use GCP's predefined roles to ensure that permissions are scoped appropriately. Avoid using basic roles (`Owner`, `Editor`, `Viewer`) except for special cases.  
  Example: Assign `Cloud SQL Viewer` for database monitoring.

- [ ] **Service Accounts with Minimal Permissions**  
  Create and manage service accounts specifically for your app components, ensuring each has minimal permissions.  
  Example: The `trackme.ai` app might use a service account with the `Pub/Sub Publisher` role for publishing application logs.

- [ ] **Enable Auditing for Sensitive Roles**  
  Set up logging for any sensitive permission modifications or access to critical resources. Use Cloud Audit Logs to monitor IAM policy changes.  
  Example: Track changes to roles like `Project Owner` or access to BigQuery datasets.

- [ ] **Avoid Using Personal Accounts**  
  Use group-based accounts and service accounts instead of personal Gmail or GCP accounts for production workflows.  
  Example: Use `team-trackme-admins@yourdomain.com` for administrative access.

- [ ] **Use Organization Policies**  
  Define and enforce organization-wide policies to restrict certain actions.  
  Example: Restrict the creation of service accounts to administrators only.

- [ ] **Regularly Review Permissions**  
  Periodically audit permissions to ensure compliance with security policies. Use the IAM Recommender to identify and remove unused permissions.  
  Example: Identify if the `trackme.ai` app service account has permissions that are no longer needed.

- [ ] **Enable Two-Factor Authentication (2FA)**  
  Require 2FA for all user accounts with access to GCP resources.

- [ ] **Use Conditional Role Bindings**  
  Apply conditions to role bindings to restrict access based on attributes like resource type, IP address, or date.  
  Example: Allow access only during business hours or from specific IP ranges.

---

#### Real-World Scenario for `trackme.ai`
**Use Case: Data Ingestion and Storage**
1. **Service Accounts**: Assign a service account with `Pub/Sub Publisher` for publishing logs and `Storage Object Admin` for uploading telemetry data to a GCS bucket.
2. **Access Restrictions**: Limit access to the GCS bucket to the `trackme.ai` service accounts and specific team members with `Storage Object Viewer`.
3. **Audit Logs**: Enable Cloud Audit Logs to monitor read/write access to the GCS bucket.

---

#### Related Files in Repo:
[TrackMe.ai IAM Document](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/iam.md)
