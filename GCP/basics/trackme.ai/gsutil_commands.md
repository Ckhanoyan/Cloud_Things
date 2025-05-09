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
