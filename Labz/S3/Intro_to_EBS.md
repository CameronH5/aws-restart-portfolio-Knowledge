# Working with Amazon EBS

In this lab, I completed a series of tasks to manage Amazon Elastic Block Store (EBS) volumes. The main objectives were to create a new EBS volume, attach and mount it to an EC2 Linux instance, create a snapshot of the volume, simulate data loss, and finally restore the data by creating a new volume from the snapshot.

## Task 1: Creating a New EBS Volume
I started by navigating to the EC2 management console and inspecting the currently running EC2 instance, which was named `Lab` and located in the `us-west-2a` Availability Zone. 
Next, I went to the **Elastic Block Store > Volumes** section and created a new volume. I configured the following parameters:
*   **Volume type:** General Purpose SSD (gp2)
*   **Size (GiB):** 1
*   **Availability Zone:** us-west-2a
*   **Tags:** I added a tag with Key `Name` and Value `My Volume`.


<img width="868" height="303" alt="Screenshot 2026-06-30 212847" src="https://github.com/user-attachments/assets/3a61c6be-1043-4632-aeff-4ff5cbb78f96" />

After choosing **Create volume**, the new 1 GiB volume appeared in the volume list and its state changed to `Available`. 
<img width="581" height="402" alt="Screenshot 2026-06-30 212702" src="https://github.com/user-attachments/assets/9d4259db-c8ae-473c-86c2-f4753d656ee8" />


<img width="631" height="150" alt="Screenshot 2026-06-30 212916" src="https://github.com/user-attachments/assets/b9b98510-2075-45d1-9621-2ace9e028ace" />
<img width="847" height="751" alt="Screenshot 2026-06-30 212838" src="https://github.com/user-attachments/assets/112e42a5-2926-469d-afff-d69283eab269" />

## Task 2: Attaching the Volume to an EC2 Instance
With the new volume created, I selected it and went to **Actions > Attach volume**. In the configuration menu, I chose the `Lab` instance (since it was in the same Availability Zone) and set the device name to `/dev/sdb`. 
I clicked **Attach volume**, and the EBS volume state immediately changed to `In-use`.


<img width="535" height="347" alt="Screenshot 2026-06-30 212938" src="https://github.com/user-attachments/assets/e223aa00-20b5-4adf-b6ba-e007ce765ad4" />

<img width="930" height="731" alt="Screenshot 2026-06-30 213005" src="https://github.com/user-attachments/assets/4753ab31-a13d-4638-abdf-c020c5eecc2b" />


## Task 3: Connecting to the EC2 Instance
I returned to the Instances list, selected the `Lab` instance, and clicked **Connect**. Using **EC2 Instance Connect**, I opened a browser-based terminal session to manage the instance.
<img width="757" height="495" alt="Screenshot 2026-06-30 213134" src="https://github.com/user-attachments/assets/230cd22a-bf69-4001-a14a-9e9980c437ca" />


## Task 4: Creating and Configuring the File System
Inside the terminal, I first ran `df -h` to check the available storage. As expected, the new 1 GiB volume was not yet visible. 
To prepare the new storage, I formatted it with the ext3 file system using the command:
```bash
sudo mkfs -t ext3 /dev/sdb
```
Next, I created a mount point directory and mounted the new volume:
```bash
sudo mkdir /mnt/data-store
sudo mount /dev/sdb /mnt/data-store
```
To ensure the volume would mount automatically after a reboot, I added its entry to the `/etc/fstab` file using a one-liner that appends to the file:
```bash
echo "/dev/sdb /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
```
I verified the configuration by running `cat /etc/fstab`. Finally, I ran `df -h` once more to confirm that `/dev/nvme1n1` (the device representation of the new EBS volume) was correctly mounted at `/mnt/data-store` with approximately 975 MB of available space.

<img width="681" height="443" alt="Screenshot 2026-06-30 213213" src="https://github.com/user-attachments/assets/d63995f5-14fe-4e97-855b-c583cac52d3e" />
<img width="943" height="453" alt="Screenshot 2026-06-30 213343" src="https://github.com/user-attachments/assets/8eedd4b8-b6cd-4a4e-843b-e3687adb1cfd" />


## Task 5: Creating a File and Taking a Snapshot
To test the storage, I created a text file with some content on the mounted volume:
```bash
sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
```
I verified the content by running `cat /mnt/data-store/file.txt`.
<img width="947" height="203" alt="Screenshot 2026-06-30 213411" src="https://github.com/user-attachments/assets/98008129-1fce-47f7-8c58-6382f5bbaa5d" />


Next, I returned to the AWS Management Console, navigated to **Volumes**, selected `My Volume`, and chose **Actions > Create snapshot**. I added a tag to the snapshot with Key `Name` and Value `My Snapshot`, then clicked **Create snapshot**.
I navigated to the **Snapshots** menu and confirmed that a new snapshot (`snap-02d442541e6659181`) was created successfully with a status of `Completed` and a size of 53 MiB.
<img width="617" height="471" alt="Screenshot 2026-06-30 213442" src="https://github.com/user-attachments/assets/9873a225-e0b9-4527-b8a0-e6b9a23d43a4" />
<img width="653" height="347" alt="Screenshot 2026-06-30 213527" src="https://github.com/user-attachments/assets/b2c14dc6-ea4e-4513-b8a2-15deb0f5926e" />


## Task 6: Simulating Data Loss
Back in the EC2 Instance Connect terminal, I simulated a data loss scenario by deleting the file I had created:
```bash
sudo rm /mnt/data-store/file.txt
```
To confirm it was gone, I ran the `ls` command, which returned the expected "No such file or directory" error.
<img width="802" height="181" alt="Screenshot 2026-06-30 213613" src="https://github.com/user-attachments/assets/aaabee47-418b-4bd1-971f-c602c36df8f4" />


## Task 7: Restoring the Amazon EBS Snapshot
To recover the data, I went back to the **Snapshots** page in the AWS console. I selected `My Snapshot`, clicked **Actions**, and chose **Create volume from snapshot**. I ensured the Availability Zone was set to `us-west-2a`, gave it a tag with Key `Name` and Value `Restored Volume`, and created it. Interestingly, the new volume was automatically created as a **gp3** volume type. 
Once created, I saw the new `Restored Volume` (1 GiB, gp3) appear in my volume list next to the original 8 GiB volume and my original 1 GiB volume.
<img width="907" height="727" alt="Screenshot 2026-06-30 213654" src="https://github.com/user-attachments/assets/2c53aec9-da25-4b72-85a6-a3191cef1b69" />
<img width="640" height="370" alt="Screenshot 2026-06-30 213746" src="https://github.com/user-attachments/assets/02f855ec-a1b5-4c3c-ae38-9a0463b714ed" />


## Task 8: Attaching and Mounting the Restored Volume
I selected the `Restored Volume` and attached it to the `Lab` instance, specifying the device name `/dev/sdc`. 
Back in the terminal, I created a new mount point directory and mounted the restored volume:
```bash
sudo mkdir /mnt/data-store2
sudo mount /dev/sdc /mnt/data-store2
```
Finally, I listed the contents of this new mount point to confirm the data had been recovered:
```bash
ls /mnt/data-store2/file.txt
```
The command successfully output the filename `file.txt`, proving that the file I had previously created and deleted was successfully restored from the snapshot.
<img width="726" height="177" alt="Screenshot 2026-06-30 213907" src="https://github.com/user-attachments/assets/03ab8209-77c5-4977-b1e8-ccb25e559c93" />


## Conclusion
By performing this lab, I successfully demonstrated the end-to-end lifecycle of an Amazon EBS volume. I created and attached a block storage device to a live EC2 instance, formatted it with a file system, established persistent mounting, and effectively used EBS snapshots to backup and restore data, ensuring durability and disaster recovery capabilities within AWS.
