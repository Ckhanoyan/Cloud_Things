### Google Cloud Storage Commands Using `gsutil`

#### Core Commands for Cloud Storage
1. **List Buckets**
   ```bash
   gsutil ls

2. **Create a bucket**
   ```bash
   gsutil mb gs://my-bucket

 **Use case**: set up a storage bucket for storing trackme.ai log files 

3. **Upload Files**
  ```bash
  gsutil cp local-file gs://my-bucket
```
**Use case**: upload user-generated content like workout data or images

4. **Download Files**
   ```bash
   gsutil cp gs://my-bucket/remote-file local-file
   ```
   **Use Case**: retrieve archived logs or leaderboard data

5. **Transfer Data Between Buckets**
   ```bash
   gsutil cp gs://source-bucket/file gs://destination-bucket/file
   ```
   **Use Case**: Migrate data between different storage locations during scaling.

6. **Enable Versioning**
   ```bash
   gsutil versioning set on gs://my-bucket
   ```
   **Use Case**: Preserve previous versions of leaderboard files for audit purposes.

7. **Set Object Lifecycle Rules**
   ```bash
   gsutil lifecycle set lifecycle.json gs://my-bucket
   ```
   **Use Case**: Automate deletion of outdated logs or archived files.


### Online Transfer for Trackme.ai 

1. **Resumable Uploads**
   ```bash
   gsutil -o 'GSUtil:resumable_threshold=150M' cp large-file gs://my-bucket
   ```
   **Scenario**: Uploading large video workout files securely and efficiently.

2. **Parallel Composite Uploads**
   ```bash
   gsutil -o 'GSUtil:parallel_composite_upload_threshold=150M' cp large-file gs://my-bucket
   ```
   **Scenario**: Accelerate the upload of large files to meet real-time processing needs.

3. **Sync Local Directory to Cloud Storage**
   ```bash
   gsutil rsync -r ./local-dir gs://my-bucket
   ```
   **Scenario**: Backup local datasets to GCP for disaster recovery.

4. **Transfer Files Using Signed URLs**
   ```bash
   gsutil signurl -d 10m my-key.json gs://my-bucket/file
   ```
   **Scenario**: Allow a temporary, secure upload of workout data from unauthenticated clients.

### Scenarios for Online Transfer in Trackme.ai App

1. Workout File Uploads
   - Scenario: Users upload workout videos via the mobile app. Use resumable uploads to handle intermittent connectivity.

2. Leaderboard Updates
   - Scenario: Weekly leaderboard updates require transferring pre-computed data to a public bucket. Use parallel composite uploads for efficiency.
  
3. Message Payloads
   - Scenario: Use signed URLs to temporarily store user messages for processing.
  
4. Data Migration
   - Scenario: Users upload workout videos via the mobile app. Use resumable uploads to handle intermittent connectivity.
  
### Related Files in Repository
- [Cloud Storage Best Practices](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/cloud_storage.md)
- [Migration Use Case](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/migration_usecase.md)
- [Online Transfer Documentation](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/online_transfer.md)
- [Compute Engine Setup](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/compute_engine.md)
- [High Availability Strategies](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/availability.md)
- [Learning Path Study Checklist](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/learning_path/study_checklist.md)
- [Live Migration Monitoring](https://github.com/Ckhanoyan/Cloud_Things/blob/main/GCP/basics/trackme.ai/live_migration.md)

