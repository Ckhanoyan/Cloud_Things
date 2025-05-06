# Live Migration and GPU Setup for Trackme.ai App

This guide explains how to enable live migration and add GPU support to your virtual machine (VM) using the **Google Cloud Console**. These steps ensure uninterrupted service and enhanced performance for AI-based athlete performance prediction.

---

## 1. Enable Live Migration

Live migration allows your VM to move to new hardware during maintenance without stopping or restarting.

1. Go to the **Google Cloud Console**.
2. Navigate to **Compute Engine > VM instances**.
3. Select the VM you want to configure.
4. Click on **Edit**.
5. In the **Scheduling** section:
   - Set **On host maintenance** to **"Migrate VM instance"**.
6. Click **Save**.

---

## 2. Add GPU Support

To accelerate AI model training and inference, you need to attach a GPU to your VM.

1. **Stop the VM**:
   - In the **VM instances** page, select the instance and click **Stop**.
2. **Edit the VM**:
   - Click **Edit** for the VM.
   - Scroll to the **Graphics processing unit (GPU)** section.
   - Click **Add GPU** and select:
     - GPU type (e.g., NVIDIA Tesla T4).
     - Number of GPUs.
3. **Restart the VM**:
   - Save the changes and click **Start** to restart the instance.

---

## 3. Install GPU Drivers

If not automatically installed, ensure the NVIDIA drivers are installed on the VM.

1. SSH into the VM instance.
2. Run the following commands to install NVIDIA drivers:
   ```bash
   sudo apt-get update
   sudo apt-get install -y nvidia-driver-470

3. Verify the GPU is recognized: nvidia-smi

## 4. Monitor Live Migration

1. Go to Operations > Logs Explorer in the Google Cloud Console.
2. Use the following filter to monitor live migration events:
  ```code
  resource.type="gce_instance"
  textPayload: "live migration"
