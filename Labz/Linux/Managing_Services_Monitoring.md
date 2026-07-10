# Managing Services - Monitoring Lab

## Overview

In this lab, I learned how to manage and monitor services on an Amazon Linux 2 EC2 instance. The focus was on checking the status of the Apache HTTP Server (`httpd`) service and monitoring the instance's performance using both Linux command-line tools and AWS CloudWatch.

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

The output showed that the httpd service was **loaded** and **active (running)** since 13:41:32 UTC. The service displayed multiple httpd processes running in the background with the `-DFOREGROUND` flag, indicating that Apache had spawned several worker processes to handle incoming requests. The status message showed "Total requests: 0; Idle/Busy workers 100/0" meaning the server was ready but hadn't received any traffic yet.

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

**Expected Output:**

<img width="1901" height="551" alt="Screenshot 2026-06-19 154357" src="https://github.com/user-attachments/assets/c006b29c-c6b9-47dd-8a34-e403f054663e" />


When you navigated to `http://34.220.47.196`, the browser displayed the **Apache HTTP Server Test Page**:

```
Test Page

This page is used to test the proper operation of the Apache HTTP server after it has been installed. 
If you can read this page, it means that the Apache HTTP server installed at this site is working properly.

If you are a member of the general public:
The fact that you are seeing this page indicates that the website you just visited is either experiencing 
problems, or is undergoing routine maintenance.

If you are the website administrator:
You may now add content to the directory /var/www/html/. Note that until you do so, people visiting your 
website will see this page, and not your content.
```

The page also displayed the "Powered by APACHE" logo, confirming that the httpd service was successfully running and able to serve HTTP requests. This verified you could make a successful HTTP connection to the local host IP address.

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

The `top` command displayed the current running processes and resource usage:

```
top - 13:46:44 up 11 min, 1 user, load average: 3.97, 0.92, 0.30
Tasks: 103 total, 15 running, 49 sleeping, 0 stopped, 0 zombie
%Cpu(s): 61.7 us, 38.3 sy, 0.0 ni, 0.0 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
KiB Mem : 962708 total, 288120 free, 272404 used, 402184 buff/cache
KiB Swap: 0 total, 0 free, 0 used, 540784 avail Mem

PID USER    PR NI VIRT   RES    SHR S %CPU %MEM TIME+ COMMAND
2683 ec2-user 20 0 138656 96732  276 R 14.7 10.0 0:02.58 stress
2684 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2685 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.57 stress
2686 ec2-user 20 0 138656 96732  276 R 10.0 10.0 0:02.58 stress
2687 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2690 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2691 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2693 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2694 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2695 ec2-user 20 0 7580   100    0 R 14.3 0.0 0:02.58 stress
2697 ec2-user 20 0 7580   100    0 R 14.0 0.0 0:02.58 stress
2698 ec2-user 20 0 7580   100    0 R 14.0 0.0 0:02.56 stress
2692 ec2-user 20 0 7580   100    0 R 14.0 0.0 0:02.58 stress
```

**Analysis:**

The `top` output showed:
- **System Load**: `load average: 3.97, 0.92, 0.30` — indicating significant CPU activity
- **Tasks**: 103 total, 15 running, 49 sleeping, 0 stopped, 0 zombie processes
- **CPU Usage**: 61.7% user space, 38.3% system, with minimal idle time
- **Memory**: 962708 total, 288120 free, 272404 used, 402184 buff/cache

The `top` command displays the processes currently running as well as the resource usage like CPU usage and memory usage. Press "q" to exit from the `top` command and return to the shell.

### Step 27: Run the Stress Script with `top`

Run the stress script that simulates a heavy workload on the EC2 instance:

```bash
./stress.sh & top
```

<img width="291" height="53" alt="Screenshot 2026-06-19 154638" src="https://github.com/user-attachments/assets/39e0b04f-53da-4eb9-8181-d3a69e61e4ad" />

**Expected Output:**

When running the stress test with `top`, the output showed:

```
top - 13:46:44 up 11 min, 1 user, load average: 3.97, 0.92, 0.30
Tasks: 103 total, 15 running, 49 sleeping, 0 stopped, 0 zombie
%Cpu(s): 61.7 us, 38.3 sy, 0.0 ni, 0.0 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
KiB Mem : 962708 total, 288120 free, 272404 used, 402184 buff/cache
KiB Swap: 0 total, 0 free, 0 used, 540784 avail Mem

PID USER    PR NI VIRT   RES SHR S %CPU %MEM TIME+ COMMAND
2683 ec2-user 20 0 138656 96732 276 R 14.7 10.0 0:02.58 stress
2684 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2685 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.57 stress
2686 ec2-user 20 0 138656 96732 276 R 10.0 10.0 0:02.58 stress
2687 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2690 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2691 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2693 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2694 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2695 ec2-user 20 0 7580   100   0 R 14.3 0.0 0:02.58 stress
2697 ec2-user 20 0 7580   100   0 R 14.0 0.0 0:02.58 stress
2698 ec2-user 20 0 7580   100   0 R 14.0 0.0 0:02.56 stress
2692 ec2-user 20 0 7580   100   0 R 14.0 0.0 0:02.58 stress
```

**Figure:** The command prompt shows a high CPU usage after running a script. It shows the user as ec2-user the percentage of CPU of 14-14.3 and the command used was stress.

**Analysis:**

You can see that the process you just ran has a high CPU usage. The script is designed to run for 6 minutes before stopping. 

In the next steps, you will open the **AWS Management Console** and start the **AWS CloudWatch** application that will give you better insight into your EC2 instance.

### Step 28: List Running Processes with `top`

Display the list of running processes by entering the `top` command:

```bash
top
```

**Expected Output:**

The `top` command displayed the current running processes showing the stress test workload.

### Step 29: Access AWS Management Console

On the top right of your screen, select the AWS button. This displays the AWS Management Console in a new tab.

### Step 30: Search for CloudWatch

In the Search bar on the top of the screen, enter CloudWatch and press ENTER.

**Expected Output:**

The search bar on the AWS console with the word CloudWatch entered.

The AWS console includes a search bar that you can use to search for services.

<img width="1622" height="817" alt="Screenshot 2026-06-19 154839" src="https://github.com/user-attachments/assets/5deac3ba-2767-41c2-b9e3-92ab29de321a" />


### Step 31: Access the EC2 Dashboard

On the left section of the navigation pane, select **Dashboard**, then select **Automatic dashboards**. In the **Automatic dashboards** list, select **EC2**.

**Expected Output:**

The CloudWatch dashboards display CloudWatch Events, CloudWatch Logs, and EC2 as the top three dashboards.

**Figure:** The graphs shown are CPU Utilization, DiskReadBytes, DiskReadOps, DiskWriteBytes, DiskWriteOps, and NetworkIn for the account's EC2 instances.

This opens up the EC2 dashboard created for you by AWS.

**Expected Output:**

The EC2 dashboard displays several metrics such as CPU utilization, disk reads and writes, and so on.

You can see that by default the **EC2 CloudWatch dashboard** displays several metrics such as the CPU utilization, disk reads and writes, and so on...

You can see a spike in the CPU utilization that matches the time when you started the stress script earlier.

<img width="1895" height="840" alt="Screenshot 2026-06-19 154903" src="https://github.com/user-attachments/assets/2984f8fb-d3bf-4be5-9f4a-d26ca6dab3a9" />


> **NOTE**
> - Dashboards are customizable so you can add or remove widgets
> - Reorganize them, customize colors, and more features such as alarms or events triggers that you will discover
> - Later that makes it a key **AWS Service** to monitor your applications
> - In real time: update the 5 minutes average to 1 second to second review
> - updates more quickly

### Step 32: Monitor CPU Utilization Drop

Wait 5 minutes and go back to the AWS CloudWatch dashboard. You see that the CPU utilization dropped significantly.

<img width="1873" height="712" alt="Screenshot 2026-06-19 155251" src="https://github.com/user-attachments/assets/e7af50f8-6b9b-4dc5-b340-7678bad194d1" />

**Expected Output:**

```
CPU Utilization: Average
Percent: 9.77%
```

The graph shown is the CPU Utilization Average. In this average, the highest percent it reaches is 62.9 percent of around 8 minutes and hovers around 33 percent then drops back down right after the 3 minute mark.

**Figure:** The graph is shown in the CPU Utilization Average. In this average, the highest percent it reaches is 62.9 percent of around 8 minutes and hovers around 33 percent then drops back down right after the 3 minute mark.

---

## Summary

In this lab, I successfully:

1. **Managed the httpd Service**: Checked the status, started, and verified the Apache HTTP Server was running by accessing the test page through a web browser.

2. **Monitored with Linux Tools**: Used the `top` command to view real-time process and resource usage on your EC2 instance, including CPU utilization and memory consumption.

3. **Simulated System Load**: Ran a stress test script that created measurable CPU load on the instance, demonstrating how system resources respond to workloads.

4. **Monitored with AWS CloudWatch**: Accessed the EC2 CloudWatch dashboard to visualize instance metrics over time, including:
   - **CPU Utilization**: Observed a spike to 62.9% during the stress test, dropping back to baseline after completion
   - **Network In**: Tracked network traffic during the monitoring period
   - Other metrics like Disk Read/Write operations (though not applicable to this CPU-focused workload)

5. **Correlated Events**: Connected the stress script execution visible in Linux tools (`top`) with the corresponding spike in AWS CloudWatch metrics, understanding how local system activity appears in cloud monitoring dashboards.


