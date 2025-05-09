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








   ;
   Online Transfer for trackme.ai
Resumable Uploads

bash
gsutil -o 'GSUtil:resumable_threshold=150M' cp large-file gs://my-bucket
Scenario: Uploading large video workout files securely and efficiently.
Parallel Composite Uploads

bash
gsutil -o 'GSUtil:parallel_composite_upload_threshold=150M' cp large-file gs://my-bucket
Scenario: Accelerate the upload of large files to meet real-time processing needs.
Sync Local Directory to Cloud Storage

bash
gsutil rsync -r ./local-dir gs://my-bucket
Scenario: Backup local datasets to GCP for disaster recovery.
Transfer Files Using Signed URLs

Generate a signed URL:
bash
gsutil signurl -d 10m my-key.json gs://my-bucket/file
Scenario: Allow a temporary, secure upload of workout data from unauthenticated clients.
Scenarios for Online Transfer in trackme.ai
Workout File Uploads

Scenario: Users upload workout videos via the mobile app. Use resumable uploads to handle intermittent connectivity.
Leaderboard Updates

Scenario: Weekly leaderboard updates require transferring pre-computed data to a public bucket. Use parallel composite uploads for efficiency.
Message Payloads

Scenario: Use signed URLs to temporarily store user messages for processing.
Data Migration

Scenario: Migrate logs from AWS S3 to GCP buckets using gsutil cp or Storage Transfer Service for large-scale operations.
Related Files in Repository
Cloud Storage Best Practices
Migration Use Case
