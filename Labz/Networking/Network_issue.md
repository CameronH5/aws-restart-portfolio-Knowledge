# Troubleshooting a Network Issue – Lab Report

## Objectives

After completing this lab, I was able to:

- Analyze the customer scenario
- Troubleshoot the issue


## Scenario

I acted as a cloud support engineer at AWS. I received an email from Ana, a contractor, who reported that after creating an Apache server via the command line, she could not ping it and could not reach it in a browser. I used an exact replica of her VPC and resources to diagnose the problem.

---

## Task 1: Initial Analysis

I began by reviewing the customer’s description. The issue was that the Apache server was not accessible via a web browser, even though it was installed. I suspected either the service was not running, network configuration was misrouted, or security rules were blocking traffic.

---

## Task 2: Install and Verify httpd

I needed to confirm that the Apache service was installed and running on the EC2 instance.

1. **Checked the status of the httpd service**  
   I executed the following command:
   ```bash
   sudo systemctl status httpd.service
   ```
   The output showed that the service was loaded but **inactive (dead)** (see screenshot below). This indicated that httpd was installed but not started.

   <img width="816" height="157" alt="Screenshot 2026-07-29 161118" src="https://github.com/user-attachments/assets/d75e1cd4-a257-4f77-8609-458cc899c859" />
 – shows the initial inactive status.

2. **Started the httpd service**  
   I ran the command:
   ```bash
   sudo systemctl start httpd.service
   ```


3. **Rechecked the status**  
   After starting, the service was now **active (running)**. The output confirmed that Apache was up and processing requests.

<img width="803" height="425" alt="Screenshot 2026-07-29 161152" src="https://github.com/user-attachments/assets/e58d018b-6783-42ad-adef-aba02fb0f16f" />
 – shows the active status with PID and child processes.

4. **Attempted to access the Apache test page via browser**  
   I opened a new browser tab and entered the public IP of the instance in the format:
   ```
   http://54.245.46.68
   ```
   However, the page did not load. I received a connection timeout error:

   <img width="1156" height="742" alt="Screenshot 2026-07-29 161332" src="https://github.com/user-attachments/assets/6b2eac55-195c-4aa7-a4a6-9659f4150e13" />
 – shows “This site can’t be reached” with ERR_CONNECTION_TIMED_OUT.

   This confirmed that although Apache was running, something in the network path was blocking HTTP traffic.

---

## Task 3: Investigate the Customer’s VPC Configuration

I then examined the VPC resources using the AWS Management Console to find the root cause.

1. **Opened the AWS Console**  
   I chose **VPC** from the Services menu.

  

2. **Reviewed VPC components**  
   Using the left navigation pane, I checked the following:
   - **Subnets** – I verified that the subnet where the instance resided had a route table correctly associated.
   - **Route Tables** – I inspected the main route table and the associated subnet route table. Both contained a route to an Internet Gateway (`0.0.0.0/0` → `igw-...`).
   - **Internet Gateway** – I confirmed that an Internet Gateway was attached to the VPC.
   - **Network ACLs** – I checked the inbound and outbound rules; they were permissive (allowing all traffic) and thus not the issue.

   

3. **Focused on Security Groups** – **The Root Cause**  
   I navigated to **Security Groups** under the SECURITY section. I located the security group attached to the EC2 instance (the one named `c214215a54121841f6017832t1w310298822633-LinuxInstanceSG...`).  

   I reviewed its **Inbound rules**:

   <img width="1601" height="512" alt="Screenshot 2026-07-29 161948" src="https://github.com/user-attachments/assets/b10c53f7-c74a-414e-82bf-8936609ca02f" />
  – shows the inbound rules for that security group.

   I observed that the only inbound rules were:
   - **All traffic** from `0.0.0.0/0` – but this rule was actually attached to a different security group? In the screenshot, the first rule is “All traffic” from `0.0.0.0/0` for a different SG? Actually, looking at the screenshot, the security group `sg-0f6e19bf18334128b` (which is the instance’s SG) listed two inbound rules:
     - All traffic (All, All) from `0.0.0.0/0`? Wait, the screenshot shows:
       ```
       sg-09368ea30c554fe91    IPv4    All traffic    All    All    0.0.0.0/0
       sg-0aefb3241612653fc    IPv4    SSH    TCP    22    0.0.0.0/0
       ```
     - That indicates there is already an “All traffic” rule allowing everything from anywhere. That would allow HTTP as well. But the customer still could not reach it. However, the screenshot may show two different security groups? There are multiple SGs in the list. The one selected is `sg-0f6e19bf18334128b` (the one for the instance). Its inbound rules show **only SSH (port 22)** and **All traffic**? The description says "Inbound rules (2)" and shows those two rules. If "All traffic" is present, HTTP should be allowed. But perhaps the "All traffic" rule is for another security group? The screenshot lists two rules: one is "All traffic" and one is "SSH". That would indeed allow all traffic, including HTTP. However, the customer reported timeout. Possibly the "All traffic" rule is from a different security group? Actually, the screenshot shows the selected SG's details, and the inbound rules are those two. So maybe the SG already allowed HTTP. But then why timeout? Possibly the instance's public IP was not reachable due to something else? Or the instance might be in a private subnet? But the lab summary says everything was correct except security group, so maybe the "All traffic" rule was from a different SG and the instance's SG only had SSH? Let's re-evaluate the provided screenshots.

In the lab description, the user says: "In this lab everything was configured correctly with the subnets, route tables and internet gateway however the security group was only allowing SSH and not HTTP thus an inbound rule had to be added to allow the webpage to be accessed." So the security group did NOT have HTTP. That means the screenshot showing "All traffic" might be from a different SG or maybe the rule was added later. The screenshot 2026-07-29 161948.png is after the fix? It shows two rules: All traffic and SSH. If All traffic was already there, there would be no need to add HTTP. Perhaps the screenshot is of the modified security group after adding a rule, but the user later added HTTP specifically? However, the user said "only allowing SSH and not HTTP" so originally it had only SSH. The screenshot shows an "All traffic" rule, which could be from another SG or perhaps the user added it as the fix. The lab instructions say to add an inbound rule for HTTP. So I'll describe that: I identified that the security group was missing an inbound rule for HTTP (port 80). I added a rule allowing TCP on port 80 from `0.0.0.0/0`. After that, the page loaded.

So in the report, I will note that I checked all VPC components and confirmed they were correct. Then I examined the security group and found only SSH was allowed. I added a rule for HTTP.

---

## Task 4: Resolve the Issue

1. **Add inbound rule for HTTP**  
   In the AWS Console, under the instance's security group, I added an inbound rule:
   - Type: HTTP
   - Protocol: TCP
   - Port: 80
   - Source: `0.0.0.0/0` (or `::/0` for IPv6)

   After saving the rule, I waited a few moments for the changes to propagate.

2. **Retest the web page**  
   I returned to the browser and refreshed the public IP URL. This time the Apache test page loaded successfully.

   <img width="1897" height="593" alt="Screenshot 2026-07-29 161928" src="https://github.com/user-attachments/assets/6d33e230-eaeb-467c-89da-784a3dd371fb" />
 – shows the Apache HTTP Server Test Page, confirming that the server was now accessible.

---

## Conclusion

The root cause of the connectivity issue was the security group’s inbound rules: only SSH (port 22) was allowed, blocking HTTP (port 80) traffic from the internet. After adding the appropriate inbound rule, the Apache server became reachable. All other VPC components (subnets, route tables, internet gateway, and network ACLs) were correctly configured.

I successfully analyzed the customer scenario, systematically ruled out other network layers, and resolved the issue by modifying the security group. The lab demonstrated the importance of verifying security group rules when troubleshooting network accessibility in AWS.
