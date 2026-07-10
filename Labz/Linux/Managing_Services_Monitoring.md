# Managing Services - Monitoring Lab

## Overview

In this lab, I learned how to manage and monitor services on an Amazon Linux 2 EC2 instance. The focus was on checking the status of the Apache HTTP Server (`httpd`) service and monitoring the instance with Linux tools and AWS CloudWatch.

## Objectives

By completing this lab, I was able to:

1. Check the status of the httpd service to ensure it is running
2. Verify that you can make an HTTP connection to the local host IP address
3. Monitor an Amazon Linux 2 EC2 instance using the Linux `top` command
4. Monitor an EC2 instance using AWS CloudWatch

---

## Task 2: Check the Status of the httpd Service

### Objective

Check the status of the httpd service, start it if necessary, verify it's working, and then stop it.

**Note**: You may need to use `sudo` to complete this exercise if you are not root.

### Step 21: Initial Status Check

Check the status of the httpd service using the `systemctl` command:

```bash
sudo systemctl status httpd.service
```

**Expected Output:**

The initial status check revealed the following:

```
httpd.service - The Apache HTTP Server
  Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset is disabled)
  Active: active (running) since Fri 2026-06-19 13:41:32 UTC; 19s ago
  
  Main PID: 2609 (httpd)
  Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes served/sec: 0 B/sec"
  CGroup: /system.slice/httpd.service
    ├─2609 /usr/bin/httpd -DFOREGROUND
    ├─2610 /usr/bin/httpd -DFOREGROUND
    ├─2611 /usr/bin/httpd -DFOREGROUND
    ├─2613 /usr/bin/httpd -DFOREGROUND
    ├─2631 /usr/bin/httpd -DFOREGROUND
    └─2635 /usr/bin/httpd -DFOREGROUND
    
Jun 19 13:41:32 ip-10-0-10-17.us-west-2.compute.internal systemd[1]: Starting...
Jun 19 13:41:32 ip-10-0-10-17.us-west-2.compute.internal systemd[1]: Started ...
Hint: Some lines were ellipsized, use -l to show in full.
```

<img width="811" height="413" alt="Screenshot 2026-06-19 154201" src="https://github.com/user-attachments/assets/73cc6079-8c20-417a-b99f-0b673413c510" />

**Analysis:**

The output showed that the httpd service was **loaded** and **active (running)** since 13:41:32 UTC. The service displayed multiple httpd processes running in the background with the `-DFOREGROUND` option.

### Step 22: Start the httpd Service

Start the httpd service using the `systemctl` command:

```bash
sudo systemctl start httpd.service
```

### Step 23: Verify the Service is Running

Check the status of the httpd service again to confirm it is now running:

```bash
sudo systemctl status httpd.service
```

<img width="718" height="107" alt="Screenshot 2026-06-19 154224" src="https://github.com/user-attachments/assets/8f324686-de59-416d-8cb1-5d6a3819b205" />

**Expected Output:**

After starting the service, the status command confirmed:

```
httpd.service - The Apache HTTP Server
  Active: active (running) since Fri 2026-06-19 13:41:32 UTC; 19s ago
```

The service was now confirmed to be in the **active (running)** state.

### Step 24: Test HTTP Connectivity

Now that httpd is running, verify it works correctly by opening a new tab in your browser and navigating to `http://<public-ip>`. Replace `<public-ip>` with the public IP address of your EC2 instance.

<img width="1901" height="551" alt="Screenshot 2026-06-19 154357" src="https://github.com/user-attachments/assets/c006b29c-c6b9-47dd-8a34-e403f054663e" />

When you navigated to `http://34.220.47.196`, the browser displayed the **Apache HTTP Server Test Page**.

### Step 25: Stop the httpd Service

Stop the httpd service using the `systemctl` command:

```bash
sudo systemctl stop httpd.service
```

<img width="625" height="57" alt="Screenshot 2026-06-19 154552" src="https://github.com/user-attachments/assets/0a23c962-5eb1-461f-b68d-8d448dccf422" />

---

## Task 3: Monitoring a Linux EC2 Instance

### Objective

Monitor your Amazon Linux 2 EC2 instance using both Linux command-line tools and AWS CloudWatch to understand system performance metrics.

**Note**: You may need to use `sudo` to complete this exercise if you are not root.

### Step 26: Display Running Processes with `top`

Display the list of running processes by entering the `top` command:

```bash
top
```

<img width="787" height="412" alt="Screenshot 2026-06-19 154649" src="https://github.com/user-attachments/assets/93ad3eb3-7f13-4a17-a0d6-599b6b25a57a" />

**Expected Output:**

The `top` command displayed the current running processes and resource usage (example output shown in the lab).

### Step 27: Run the Stress Script with `top`

Run the stress script that simulates a heavy workload on the EC2 instance:

```bash
./stress.sh & top
```

<img width="291" height="53" alt="Screenshot 2026-06-19 154638" src="https://github.com/user-attachments/assets/39e0b04f-53da-4eb9-8181-d3a69e61e4ad" />

When running the stress test with `top`, you could observe increased CPU usage and multiple stress processes listed in `top`.

### Step 28: List Running Processes with `top`

Display the list of running processes by entering the `top` command:

```bash
top
```

### Step 29: Access AWS Management Console

On the top right of your screen, select the AWS button. This displays the AWS Management Console in a new tab.

### Step 30: Search for CloudWatch

In the Search bar on the top of the screen, enter CloudWatch and press ENTER.

<img width="1622" height="817" alt="Screenshot 2026-06-19 154839" src="https://github.com/user-attachments/assets/5deac3ba-2767-41c2-b9e3-92ab29de321a" />

### Step 31: Access the EC2 Dashboard

On the left section of the navigation pane, select **Dashboard**, then select **Automatic dashboards**. In the **Automatic dashboards** list, select **EC2**.

<img width="1895" height="840" alt="Screenshot 2026-06-19 154903" src="https://github.com/user-attachments/assets/2984f8fb-d3bf-4be5-9f4a-d26ca6dab3a9" />

You could see a spike in CPU utilization that matched the timing of the stress script.

### Step 32: Monitor CPU Utilization Drop

Wait 5 minutes and go back to the AWS CloudWatch dashboard. You should see that the CPU utilization dropped back toward baseline after the stress test completed.

<img width="1873" height="712" alt="Screenshot 2026-06-19 155251" src="https://github.com/user-attachments/assets/e7af50f8-6b9b-4dc5-b340-7678bad194d1" />

---

## Summary

In this lab, I successfully:

1. **Managed the httpd Service**: Checked the status, started, and verified the Apache HTTP Server was running by accessing the test page through a web browser.
2. **Monitored with Linux Tools**: Used the `top` command to view real-time process and resource usage on the EC2 instance, including CPU utilization and memory consumption.
3. **Simulated System Load**: Ran a stress test script that created measurable CPU load on the instance, demonstrating how system resources respond to workloads.
4. **Monitored with AWS CloudWatch**: Accessed the EC2 CloudWatch dashboard to visualize instance metrics over time, including CPU utilization, network, and disk metrics.
5. **Correlated Events**: Connected the stress script execution visible in Linux tools (`top`) with the corresponding spike in AWS CloudWatch metrics.

---

