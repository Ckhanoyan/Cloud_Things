# Launching a Google Cloud VM for a Scalable Strava-like Platform

## Step 1: Set Up Your Google Cloud Project
1. Log in to the [Google Cloud Console](https://console.cloud.google.com).
2. Create a new project or select an existing one.
3. Enable the **Compute Engine API**:
   - **No-Code Option**: Use the Console UI to navigate to **APIs & Services**, search for "Compute Engine API," and click **Enable**.
   - **Code-Based Option**: Run the following in Cloud Shell or a terminal:
     ```bash
     gcloud services enable compute.googleapis.com
     ```

---

## Step 2: Launch a VM Instance
1. **No-Code Option**:
   - Navigate to the **Compute Engine** section in the Cloud Console and click **Create Instance**.
   - Fill in the details:
     - **Name**: `strava-backend`
     - **Machine Type**: `n1-standard-1`
     - **Image**: Ubuntu 20.04 LTS
     - **Zone**: `us-central1-a`
   - Click **Create**.

2. **Code-Based Option**:
   Run the following in Cloud Shell or a terminal:
   ```bash
   gcloud compute instances create strava-backend \
       --machine-type=n1-standard-1 \
       --image-family=ubuntu-2004-lts \
       --image-project=ubuntu-os-cloud \
       --zone=us-central1-a
   ```

---

## Step 3: SSH into the VM and Set Up the Backend
1. **No-Code Option**:
   - Use the **"SSH"** button next to the VM in the **Compute Engine Instances** list.
   - Once connected, run the following commands in the SSH terminal:
     ```bash
     sudo apt update && sudo apt upgrade -y
     sudo apt install apache2 -y
     ```

2. **Code-Based Option**:
   Run the following:
   ```bash
   gcloud compute ssh strava-backend --zone=us-central1-a
   sudo apt update && sudo apt upgrade -y
   sudo apt install apache2 -y
   ```

---

## Step 4: Serve Web Traffic with Apache
1. **No-Code Option**:
   - After installing Apache, navigate to the VM’s external IP address in your browser.
   - You should see the default Apache welcome page.

2. **Code-Based Option**:
   Deploy a custom application backend using Flask:
   ```bash
   sudo apt install python3-pip -y
   pip3 install flask
   ```

   Save the following Flask app as `app.py`:
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def home():
       return "Welcome to Strava-like Backend!"

   if __name__ == "__main__":
       app.run(host="0.0.0.0", port=80)
   ```

   Run the app:
   ```bash
   sudo python3 app.py
   ```

---

## Step 5: Open the VM to Public Traffic
1. **No-Code Option**:
   - In the Cloud Console, go to **VPC Network > Firewall Rules**.
   - Add a new rule allowing **HTTP (port 80)** inbound traffic to your VM.

2. **Code-Based Option**:
   Run the following:
   ```bash
   gcloud compute firewall-rules create allow-http \
       --allow tcp:80 \
       --target-tags=http-server \
       --description="Allow HTTP traffic" \
       --direction=INGRESS
   ```

---

## Step 6: Autoscaling and Load Balancing (Optional)
1. **No-Code Option**:
   - Use the Cloud Console to create a **Managed Instance Group** and configure autoscaling.
   - Set up a **Global HTTP Load Balancer** from the **Load Balancing** section.

2. **Code-Based Option**:
   Create a Managed Instance Group:
   ```bash
   gcloud compute instance-templates create strava-template \
       --machine-type=n1-standard-1 \
       --image-family=ubuntu-2004-lts \
       --image-project=ubuntu-os-cloud

   gcloud compute instance-groups managed create strava-group \
       --base-instance-name strava-backend \
       --template=strava-template \
       --size=1 \
       --zone=us-central1-a
   ```

   Enable autoscaling:
   ```bash
   gcloud compute instance-groups managed set-autoscaling strava-group \
       --zone=us-central1-a \
       --max-num-replicas=5 \
       --target-cpu-utilization=0.6
   ```

   Add a load balancer:
   ```bash
   gcloud compute forwarding-rules create strava-load-balancer \
       --load-balancing-scheme=EXTERNAL \
       --ports=80 \
       --target-pool=strava-group \
       --region=us-central1
   ```

