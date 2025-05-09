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
