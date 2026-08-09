# Lab Report: Monitor an EC2 Instance

## Overview
In this lab, I implemented logging and monitoring techniques to ensure system performance baselines and security guidelines were met. I created an Amazon CloudWatch alarm triggered by high CPU utilization on an EC2 instance, simulated a malicious actor or performance issue by stress-testing the CPU, and set up a notification system through Amazon Simple Notification Service (SNS) to alert me of the violation. Finally, I created a CloudWatch dashboard for visual monitoring.

## Objectives Achieved

### 1. Create an Amazon SNS Notification
I started by navigating to the Amazon SNS console and creating a new SNS topic. 
* I selected a **Standard** topic type and named it `MyCwAlarm`.
* Next, I created a subscription for this topic. I set the protocol to **Email** and entered the endpoint email address `chugowork@gmail.com`.
* Upon creation, I received an AWS Notification subscription confirmation email at that address. I opened the email and clicked the **Confirm subscription** link. Returning to the SNS console, I verified that the subscription status had changed to **Confirmed**, enabling the topic to send alerts.

<img width="1341" height="780" alt="Screenshot 2026-08-09 142939" src="https://github.com/user-attachments/assets/d6905353-7236-4ca2-b8cc-d61c350ec8e4" />
<img width="1530" height="545" alt="Screenshot 2026-08-09 144057" src="https://github.com/user-attachments/assets/4f0f53b9-1dff-41a5-b874-c2900fd69866" />
<img width="827" height="322" alt="Screenshot 2026-08-09 144120" src="https://github.com/user-attachments/assets/de1633df-3da0-44be-a9f3-5bb18509d66d" />


### 2. Configure a CloudWatch Alarm
After configuring the notification system, I moved to the Amazon CloudWatch console to set up the alarm.
* I navigated to **Metrics**, selected **EC2**, and then **Per-Instance Metrics**.
* I located the `Stress Test` EC2 instance and selected the metric named `CPUUtilization`.
* I then clicked **Create alarm** to configure the threshold. 
* On the **Specify metric and conditions** page, I defined a **Static** threshold: `CPUUtilization` must be **Greater** than **60**, evaluated over a **1 minute** period.
* On the **Configure actions** page, I set the **Alarm state trigger** to **In alarm** and configured it to send a notification to the existing SNS topic I had created earlier (`MyCwAlarm`).
* I named the alarm `LabCPUUtilizationAlarm` and added an optional description: `CloudWatch alarm for Stress Test EC2 instance CPUUtilization`.
* After reviewing the configuration, I created the alarm.


<img width="1901" height="787" alt="Screenshot 2026-08-09 144335" src="https://github.com/user-attachments/assets/3565bf01-4fab-4cae-b8c3-23af839426fe" />
<img width="1842" height="761" alt="Screenshot 2026-08-09 144452" src="https://github.com/user-attachments/assets/a66bb73a-a471-474b-ba6c-2c5c3b1a9438" />
<img width="1568" height="640" alt="Screenshot 2026-08-09 144542" src="https://github.com/user-attachments/assets/1dcf5d93-21b4-4611-a6a1-bc8992c0ddc6" />

<img width="1502" height="558" alt="Screenshot 2026-08-09 144606" src="https://github.com/user-attachments/assets/56af22ec-c8ff-4b6a-8d8d-a7c32210ddbf" />
<img width="1522" height="610" alt="Screenshot 2026-08-09 144637" src="https://github.com/user-attachments/assets/f331f01e-ad75-4d2b-9291-bf082e5675d3" />
<img width="1200" height="517" alt="Screenshot 2026-08-09 144710" src="https://github.com/user-attachments/assets/2d457f07-e082-44a7-ba36-3aacad333b72" />



### 3. Stress Test an EC2 Instance
To manually trigger the alarm, I logged into the `Stress Test` EC2 instance using the provided URL in the Vocareum console. 
I ran a command to artificially stress the CPU load to 100 percent for 400 seconds. This is the command I executed:

```bash
sudo stress --cpu 10 -v --timeout 400s
```

<img width="1125" height="765" alt="Screenshot 2026-08-09 144819" src="https://github.com/user-attachments/assets/c3a2dccd-c182-4205-b4d2-72abcb3e7fdc" />


While the stress test was running, I opened a second terminal session to the same instance using another browser tab. I ran the `top` command to monitor the live CPU usage.

```bash
top
```

<img width="936" height="522" alt="Screenshot 2026-08-09 144857" src="https://github.com/user-attachments/assets/0e98971d-3c3a-4db9-8309-af10bf227a2c" />


### 4. Confirm that an Amazon SNS Email Was Sent
After running the stress test, I returned to the AWS CloudWatch console and opened the **Alarms** page. 
* I selected the `LabCPUUtilizationAlarm` and monitored the graph. 
* I clicked the **refresh** button every minute until the alarm state changed to `In alarm`. As expected, the graph showed CPUUtilization spiking well above the 60 percent threshold.
* I then navigated to my email inbox and verified that I received a new email notification from AWS notifying me that the EC2 instance had entered the alarm state.

<img width="1437" height="642" alt="Screenshot 2026-08-09 150753" src="https://github.com/user-attachments/assets/748bfe1d-94e7-4c28-b9e2-346a35e5521d" />


### 5. Create a CloudWatch Dashboard
To finalize the lab, I created a custom CloudWatch dashboard to easily visualize the metric I had been monitoring.
* In the CloudWatch console, I navigated to **Dashboards** and clicked **Create dashboard**.
* I named the dashboard `LabEC2Dashboard`.
* I selected a **Line** widget, chose the **EC2** namespace, and selected **Per-Instance Metrics**.
* I checked the box corresponding to the `Stress Test` instance and `CPUUtilization` metric.
* Once the widget configuration was complete, I clicked **Create widget** and then **Save dashboard**. This created a quick-access shortcut to view the `CPUUtilization` metric for the `Stress Test` instance in the future.


<img width="1911" height="527" alt="Screenshot 2026-08-09 145316" src="https://github.com/user-attachments/assets/0adb8b08-d8f9-468b-943f-c13dbcd51f93" />



