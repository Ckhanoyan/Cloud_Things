# Launching a Google Cloud VM for a Scalable Strava-like Platform

## Step 1: Set Up Your Google Cloud Project
1. Log in to the [Google Cloud Console](https://console.cloud.google.com).
2. Create a new project or select an existing one.
3. Enable the **Compute Engine API**:
   - Go to the **APIs & Services** page.
   - Search for "Compute Engine API" and click **Enable**.

---

## Step 2: Launch a VM Instance
Run the following command in the Cloud Shell or your terminal with `gcloud` installed:

```bash
gcloud compute instances create strava-backend \
    --machine-type=n1-standard-1 \
    --image-family=ubuntu-2004-lts \
    --image-project=ubuntu-os-cloud \
    --zone=us-central1-a
```

### Breakdown of the Command:
- **`strava-backend`**: The name of your virtual machine.
- **`n1-standard-1`**: A machine type suitable for medium workloads.
- **`ubuntu-2004-lts`**: The operating system image for the VM.
- **`us-central1-a`**: The zone where the VM is created.

---

## Step 3: SSH into the VM and Set Up the Backend
1. SSH into the VM using the Google Cloud Console or CLI:
   ```bash
   gcloud compute ssh strava-backend --zone=us-central1-a
   ```
2. Update the system and install dependencies:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install python3-pip -y
   pip3 install flask
   ```
3. Deploy a simple Flask API for handling uploads and user activities:
   ```python
   from flask import Flask, request, jsonify
   app = Flask(__name__)

   @app.route('/upload', methods=['POST'])
   def upload_activity():
       data = request.json
       return jsonify({"message": "Activity uploaded successfully!"}), 200

   @app.route('/activities', methods=['GET'])
   def get_activities():
       activities = [{"user": "John", "distance": "25 km"}]
       return jsonify(activities), 200

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=80)
   ```
4. Save this script as `app.py` and run it:
   ```bash
   python3 app.py
   ```

---

## Step 4: Open the VM to Public Traffic
1. Modify the firewall rules to allow HTTP traffic:
   ```bash
   gcloud compute firewall-rules create allow-http \
       --allow tcp:80 \
       --target-tags=http-server \
       --description="Allow HTTP traffic" \
       --direction=INGRESS
   ```

2. Access your backend using the VM’s external IP address.

---

## Step 5: Implement Autoscaling and Load Balancing
1. Create a VM template for Managed Instance Groups:
   ```bash
   gcloud compute instance-templates create strava-template \
       --machine-type=n1-standard-1 \
       --image-family=ubuntu-2004-lts \
       --image-project=ubuntu-os-cloud
   ```

2. Set up a Managed Instance Group:
   ```bash
   gcloud compute instance-groups managed create strava-group \
       --base-instance-name strava-backend \
       --template=strava-template \
       --size=1 \
       --zone=us-central1-a
   ```

3. Enable autoscaling:
   ```bash
   gcloud compute instance-groups managed set-autoscaling strava-group \
       --zone=us-central1-a \
       --max-num-replicas=5 \
       --target-cpu-utilization=0.6
   ```

4. Add a load balancer to distribute traffic:
   ```bash
   gcloud compute forwarding-rules create strava-load-balancer \
       --load-balancing-scheme=EXTERNAL \
       --ports=80 \
       --target-pool=strava-group \
       --region=us-central1
   ```

---

## Step 6: Monitor and Optimize
1. Use **Google Cloud Monitoring** to track CPU, memory, and network usage.
2. Optimize your VM or MIG settings based on traffic patterns.

