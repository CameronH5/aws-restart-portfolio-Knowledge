# Lab Report: Systems Hardening with Patch Manager via AWS Systems Manager

## Lab Overview

In this lab, I used **Patch Manager**, a capability of AWS Systems Manager, to create a patch baseline and apply patches to both Linux and Windows EC2 instances. I first patched Linux instances using the default patch baseline, then created a custom patch baseline for Windows, associated it with a patch group, tagged the Windows instances, and finally patched them. I verified compliance to ensure all instances were up‑to‑date.

---

## Objectives

After completing this lab, I was able to:

- **Patch Linux instances using default baseline**
- **Create custom patch baseline**
- **Use patch groups to patch Windows instances using custom patch baseline**
- **Verify patch compliance**

---

## Task 1: Patch Linux Instances Using Default Baselines

In this task, I patched three Linux EC2 instances using the predefined default baseline for Amazon Linux 2.

### Steps Performed

**Step 6:** In the AWS Management Console, I entered **Systems Manager** in the search box and selected it to open the Systems Manager console.

**Step 7:** In the left navigation pane, under **Node Management**, I chose **Fleet Manager**.  

<img width="1790" height="787" alt="Screenshot 2026-08-02 145729" src="https://github.com/user-attachments/assets/3b593f0d-2d93-4c5c-bea8-20bd87e7c5b3" />
<img width="1571" height="767" alt="Screenshot 2026-08-02 145850" src="https://github.com/user-attachments/assets/f31276c7-edb7-4600-86c9-281599a17923" />


*Screenshot showing the Fleet Manager list of Linux instances*

**Step 8:** I selected the check box next to **Linux‑1**, then chose **Node actions** → **View details**.  
I reviewed details such as **Platform type**, **Node type**, **OS name**, and the IAM role that allows Systems Manager to manage the instance.

**Step 9:** At the top of the page, I clicked **AWS Systems Manager** to return to the Systems Manager homepage.

**Step 10:** In the left navigation pane, under **Node Management**, I chose **Patch Manager**.

**Step 11:** I chose **Start with an overview** (if it appeared; otherwise I proceeded).

**Step 12:** I chose **Patch now** to patch the Linux instances using the **AWS‑AmazonLinux2DefaultPatchBaseline**.

**Step 13:** Under **Basic configuration**, I set the following:
- **Patching operation**: *Scan and install*
- **Reboot option**: *Reboot if needed*
- **Instances to patch**: *Patch only the target instances I specify*
- **Target selection**: *Specify instance tags*
  - **Tag key**: `Patch Group`
  - **Tag value**: `LinuxProd`
- I clicked **Add**.

<img width="1183" height="817" alt="Screenshot 2026-08-02 145934" src="https://github.com/user-attachments/assets/14ad11a0-e58a-449c-9ef5-c315db5a4f4e" />


*Screenshot showing the Patch now configuration with tag LinuxProd*

**Step 14:** I clicked **Patch now**.

**Step 15:** I observed the status of the patching operation.  
A new page displayed the **AWS‑PatchNowAssociation** panel with a **Status** field showing that three instances were affected and the progress. I monitored until the operation completed.

<img width="1446" height="432" alt="Screenshot 2026-08-02 150054" src="https://github.com/user-attachments/assets/25b3f06b-9ecf-4825-bd24-f290a293295d" />


*Screenshot showing the execution summary with status Pending and targets tag:Patch Group:LinuxProd*

---

## Task 2: Create a Custom Patch Baseline for Windows Instances

In this task, I created a custom patch baseline specifically for Windows Server 2019, focusing on security updates with critical and important severity levels.

### Steps Performed

**Step 16:** I returned to the Systems Manager console by searching for **Systems Manager** again.

**Step 17:** In the left navigation pane, under **Node Management**, I chose **Patch Manager**.

**Step 18:** I chose **Start with an overview** (or proceeded if not present).

**Step 19:** I selected the **Patch baselines** tab.

**Step 20:** I clicked the **Create patch baseline** button.

**Step 21:** For **Patch baseline details**, I configured:
- **Name**: `WindowsServerSecurityUpdates`
- **Description**: `Windows security baseline patch`
- **Operating system**: `Windows`
- Left the **Default patch baseline** checkbox **unselected**.

**Step 22:** In the **Approval rules for operating systems** section, I added the first rule:
- **Products**: `WindowsServer2019` (deselected *All*)
- **Severity**: `Critical`
- **Classification**: `SecurityUpdates`
- **Auto‑approval**: `3 days`
- **Compliance reporting**: `Critical`

<img width="1555" height="778" alt="Screenshot 2026-08-02 150335" src="https://github.com/user-attachments/assets/d07f2bd7-1de7-4f75-bcdb-7e25a840b0e7" />


*Screenshot showing the first rule with Critical severity and SecurityUpdates classification*

**Step 23:** I clicked **Add rule** to create a second rule:
- **Products**: `WindowsServer2019` (deselected *All*)
- **Severity**: `Important`
- **Classification**: `SecurityUpdates`
- **Auto‑approval**: `3 days`
- **Compliance reporting**: `High`

<img width="1192" height="573" alt="Screenshot 2026-08-02 150427" src="https://github.com/user-attachments/assets/88bcf98f-a235-47af-a4ba-778a5780a3b1" />


*Screenshot showing the second rule with Important severity and High compliance reporting*

**Step 24:** I clicked **Create patch baseline**.

**Step 25:** In the **Patch baselines** list, I located the newly created `WindowsServerSecurityUpdates` baseline and selected its radio button.

**Step 26:** I chose the **Actions** dropdown and selected **Modify patch groups**.

**Step 27:** In the **Modify patch groups** section, under **Patch groups**, I entered `WindowsProd`.

**Step 28:** I clicked **Add** and then **Close**.

<img width="1272" height="552" alt="Screenshot 2026-08-02 150543" src="https://github.com/user-attachments/assets/c4567062-7cfa-4b53-95d7-78a0fcb331df" />


*Screenshot showing the patch group WindowsProd attached to the baseline*

---

## Task 3: Patching the Windows Instances

In this task, I first tagged the Windows instances with the same patch group (`WindowsProd`), then used the **Patch now** feature to patch them using the custom baseline.

### Task 3.1: Tagging Windows Instances

**Step 29:** In the AWS Management Console, I searched for **EC2** and selected it.

**Step 30:** I chose **Instances**, selected the check box next to **Windows‑1**, and then chose the **Tags** tab.

**Step 31:** I clicked **Manage tags**, then **Add new tag**, and set:
- **Key**: `Patch Group`
- **Value**: `WindowsProd`

**Step 32:** I clicked **Save**.

**Step 33:** I repeated these steps for **Windows‑2** and **Windows‑3** with the same tag.

<img width="1591" height="450" alt="Screenshot 2026-08-02 150828" src="https://github.com/user-attachments/assets/70819b06-863b-46db-a755-ad8929239ebc" />


*Screenshot showing the Manage tags dialog with Patch Group:WindowsProd added*

### Task 3.2: Patching Windows Instances

**Step 34:** I returned to the Systems Manager console by searching for **Systems Manager**.

**Step 35:** I patched the Windows instances by:
- Choosing **Patch Manager**
- Choosing **Start with an overview** (if needed)
- Clicking **Patch now**
- Setting:
  - **Patching operation**: *Scan and install*
  - **Reboot option**: *Reboot if needed*
  - **Instances to patch**: *Patch only the target instances I specify*
  - **Target selection**: *Specify instance tags*
  - **Tag key**: `Patch Group`, **Tag value**: `WindowsProd`
- Clicking **Add** and then **Patch now**.

<img width="1341" height="800" alt="Screenshot 2026-08-02 151056" src="https://github.com/user-attachments/assets/c95fe543-3280-40e9-aa37-f4f4849b2339" />


*Screenshot showing the Patch now configuration for WindowsProd*

**Step 36:** After the operation started, I clicked the link to the **Execution ID** when it became available. This opened a page in State Manager.

**Step 37:** I chose the **Output** link for one of the managed instances that showed `InProgress` status. This opened a Run Command page.

**Step 38:** I expanded the **Output** panel to observe the details.  
Behind the scenes, Patch Manager used Run Command to run the `PatchBaselineOperations`. In the output, I could see the `PatchGroup: WindowsProd` details.

<img width="1856" height="712" alt="Screenshot 2026-08-02 151334" src="https://github.com/user-attachments/assets/09016945-445b-4b3d-a7c7-05e081cb3b45" />


*Screenshot showing the execution ID and instance statuses (one InProgress)*

<img width="1861" height="782" alt="Screenshot 2026-08-02 151434" src="https://github.com/user-attachments/assets/8f226923-07e3-4764-87fa-189ace37c352" />


*Screenshot showing the output with steps PatchWindows and PatchLinux, and success status*

---

## Task 4: Verifying Compliance

In this task, I confirmed that all six instances (three Linux and three Windows) were patched and compliant.

### Steps Performed

**Step 39:** In the left navigation pane, under **Node Management**, I chose **Patch Manager**.

**Step 40:** I selected the **Dashboard** tab. Under **Compliance summary**, I saw **Compliant: 6**, verifying that all Windows and Linux instances were compliant.

**Step 41:** I chose the **Compliance reporting** tab.  
- This tab provided an overview of all running instances with SSM. I verified that the **Compliance status** of all Linux and Windows instances was **✔ Compliant**.
- All six instances (three Linux and three Windows) showed as compliant.
- I scrolled to the right in the **Node patching details** panel to view for each instance: Critical noncompliant count, Security noncompliant count, Other noncompliant count, Last operation date, and Baseline ID.
- I clicked the **Node ID** for one of the Windows nodes.
- On the Node ID page, I chose the **Patch** tab and scrolled down to observe which patches were applied and their installed times.

<img width="1512" height="791" alt="Screenshot 2026-08-02 151537" src="https://github.com/user-attachments/assets/72e161a4-9651-4169-98d4-511429d44cd9" />


*Screenshot showing the Compliance reporting tab with all six nodes marked Compliant*

---

## Conclusion

Congratulations! I successfully completed the lab and demonstrated the ability to:

- ✅ **Patch Linux instances using default baseline** – Used the default Amazon Linux 2 baseline with Patch now and tag‑based targeting.
- ✅ **Create custom patch baseline** – Built a custom Windows baseline with two approval rules for Critical and Important security updates.
- ✅ **Use patch groups to patch Windows instances using custom patch baseline** – Tagged Windows instances with `Patch Group:WindowsProd`, associated the custom baseline with that group, and ran a patch operation.
- ✅ **Verify patch compliance** – Confirmed that all six instances were compliant via the Dashboard and Compliance reporting tabs.

This lab reinforced how AWS Systems Manager Patch Manager can automate and centralise OS patching across large fleets, ensuring security and compliance with minimal manual effort.
