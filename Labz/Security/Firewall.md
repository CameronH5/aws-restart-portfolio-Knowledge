# Malware Protection Using an AWS Network Firewall - Lab Report

## Lab Overview
In this lab, I took on the role of a new security engineer for *AnyCompany*. Given recent reports of users accidentally downloading malware from specific websites, my objective was to harden the company's security perimeter using an AWS Network Firewall. I created firewall rules to block access to malicious actor files, and verified that the solution was effective.

## Objectives Achieved
After completing this lab, I successfully:
*   Updated a network firewall's default stateless action.
*   Created a stateful firewall rule group with Suricata rules.
*   Verified and tested that access to malicious sites was successfully blocked.

---

## Task 1: Confirmed Reachability to Malicious URLs
I started by logging into the `TestInstance` EC2 instance that was pre-configured during the lab setup. To simulate an end-user inadvertently downloading malware from a browser, I used the `wget` command-line tool.

First, I navigated to the home directory and confirmed my current working path using the following commands:

**Commands executed:**
```bash
cd ~
pwd
```
<img width="162" height="110" alt="Screenshot 2026-08-03 193840" src="https://github.com/user-attachments/assets/2e1bcb2f-089a-46fc-9906-0b1af22470fb" />


Next, I downloaded the specified malicious test files hosted by the malware testing environment. I executed the provided `wget` commands:

**Commands executed:**
```bash
wget http://malware.wicar.org/data/js_crypto_miner.html
wget http://malware.wicar.org/data/java_jre17_exec.html
```
<img width="1871" height="783" alt="Screenshot 2026-08-03 194214" src="https://github.com/user-attachments/assets/5290c5a7-fb57-4bd9-8913-2744ff5d38a1" />


To verify the downloads were successful, I listed the contents of my current directory using the `ls` command.

**Command executed:**
```bash
ls
```
<img width="670" height="57" alt="Screenshot 2026-08-03 194231" src="https://github.com/user-attachments/assets/39be6554-09ad-4d53-a8cb-91db7f7e52bc" />


---

## Task 2: Inspected and Updated the Network Firewall
Moving to the AWS Management Console, I navigated to the **VPC** service dashboard and located the **Network Firewall** section. 
I selected the pre-configured `LabFirewall` to inspect its current configuration. Under the **Firewall policy settings**, I needed to update the stateless default action to ensure traffic is forwarded to a stateful rule engine for deeper inspection. 

I clicked **Edit** and configured the settings as follows:
*   **Fragmented packets:** Choose the same actions for all packets.
*   **Rule action:** Forward to stateful rule groups.

<img width="1502" height="653" alt="Screenshot 2026-08-03 194409" src="https://github.com/user-attachments/assets/668205c8-c8e6-483f-a500-59055c10cedd" />

<img width="1543" height="165" alt="Screenshot 2026-08-03 194503" src="https://github.com/user-attachments/assets/b530cf58-16dc-41dd-a3c9-cd8b4c21b171" />

<img width="1577" height="656" alt="Screenshot 2026-08-03 194631" src="https://github.com/user-attachments/assets/dc055f13-85fb-44d2-86cc-bb838c24aea4" />

---

## Task 3: Created a Firewall Rule Group
Next, I created a new network firewall rule group to block the specific URLs known to host malware. In the **Network Firewall rule groups** menu, I chose **Create Network Firewall rule group**.

I configured the rule group with the following settings:
*   **Rule group type:** Stateful rule group.
*   **Rule group format:** Suricata compatible rule string.
*   **Rule evaluation order:** Action order.
<img width="1867" height="675" alt="Screenshot 2026-08-03 194919" src="https://github.com/user-attachments/assets/34613a70-dda7-4d6e-b743-bb6ff32cead4" />


In the details section, I named the rule group `StatefulRuleGroup` and set its capacity to `100`.
<img width="1407" height="656" alt="Screenshot 2026-08-03 194949" src="https://github.com/user-attachments/assets/7eb32c0f-737f-4cf7-8d5d-9dfb4bbcd565" />


Under the Suricata rules configuration section, I pasted the provided Suricata rules. These rules instruct the firewall to `drop` (`block`) any outbound HTTP traffic from the internal home network attempting to access the specific paths hosting the malware files.
<img width="1532" height="242" alt="Screenshot 2026-08-03 195112" src="https://github.com/user-attachments/assets/4a1b3a54-6af6-4dfc-8e12-d62354ab0439" />


---

## Task 4: Attached the Rule Group to the Network Firewall
After creating the rule group, I navigated back to the `LabFirewall` main dashboard. Under the **Associated firewall policy**, I selected the `LabFirewallPolicy`. From the **Stateful rule groups** dropdown menu, I chose **Add unmanaged stateful rule groups**, selected the `StatefulRuleGroup` I just created, and clicked **Add stateful rule group**.

A green banner appeared at the top of the screen confirming the successful update.
<img width="1832" height="702" alt="Screenshot 2026-08-03 195638" src="https://github.com/user-attachments/assets/c36a3973-bf4e-46b1-95d2-873091c8cae4" />


---

## Task 5: Validated the Solution
To validate my security update, I returned to the `TestInstance` EC2 terminal using AWS Systems Manager Session Manager. I re-established my working directory, then attempted to download the same malware files again.

**Commands executed:**
```bash
cd ~
pwd
```
**[Screenshot: Terminal showing the `pwd` command]**

I re-ran the two `wget` commands used in Task 1:
```bash
wget http://malware.wicar.org/data/js_crypto_miner.html
wget http://malware.wicar.org/data/java_jre17_exec.html
```
<img width="880" height="258" alt="Screenshot 2026-08-03 195811" src="https://github.com/user-attachments/assets/942dc84f-dad9-4bb0-9db1-64ad0223a270" />


The traffic was now correctly blocked by the network firewall. The commands did not complete; instead, they hung on `HTTP request sent, awaiting response...` until I canceled them. This confirmed that the stateful firewall rules were active and properly blocking access to the malicious actor files.

Finally, I cleaned up the previously downloaded malware files from the server to conclude the test.

**Commands executed:**
```bash
rm java_jre17_exec.html js_crypto_miner.html
ls
```
<img width="615" height="85" alt="Screenshot 2026-08-03 195902" src="https://github.com/user-attachments/assets/ff0ded4c-cd03-4f48-b59b-733d7b846230" />


I removed the remaining duplicated file to completely clear the directory:

**Commands executed:**
```bash
rm java_jre17_exec.html.1
ls
```
<img width="450" height="72" alt="Screenshot 2026-08-03 195948" src="https://github.com/user-attachments/assets/a13ab2b2-0718-48b3-9e10-385ed994abab" />


## Conclusion
In conclusion, I successfully acted as a security engineer for *AnyCompany*. I inspected the existing firewall, created a custom stateful rule group utilizing Suricata IPS rules to target specific malware URLs, attached the rule group to the firewall policy, and validated the solution. The validation confirmed that legitimate users were now effectively blocked from accessing the malicious actor files, thereby reducing the organization's security risk.
```
