# Managing Storage Lab

## Introduction

During this lab, I successfully completed tasks related to managing storage on AWS. I created and maintained snapshots for Amazon EC2 instances, used Amazon S3 sync to copy files from an EBS volume to an S3 bucket, and utilized Amazon S3 versioning to retrieve deleted files. The lab involved working with EC2 instances, EBS volumes, and S3 buckets to understand storage management best practices.

## Objectives

By the end of this lab, I was able to do the following:

- Create and maintain snapshots for Amazon EC2 instances.
- Use Amazon S3 sync to copy files from an EBS volume to an S3 bucket.
- Use Amazon S3 versioning to retrieve deleted files.

---

## Task 1.1: Create an S3 Bucket

In this task, I created an S3 bucket to sync files from an EBS volume.

1. Navigated to the AWS Management Console and searched for S3 to open the S3 Management Console.
2. Clicked **Create bucket**.
3. Configured the bucket with the following:
   - **Bucket name**: `hlabz2`
   - **Region**: Left as default
4. Clicked **Create bucket**.

**Screenshot: S3 bucket created successfully**

<img width="1250" height="425" alt="Screenshot 2026-07-02 183757" src="https://github.com/user-attachments/assets/c9a6d65c-d656-4ab1-8575-bb1833f0c11d" />


---

## Task 1.2: Attach instance profile to Processor

In this task, I attached a pre-created IAM role as an instance profile to the EC2 instance "Processor," giving it permissions to interact with other AWS services such as EBS volumes and S3 buckets.

1. Navigated to the EC2 Management Console.
2. In the navigation pane, selected **Instances**.
3. Selected **Processor** from the list of EC2 instances.
4. Chose **Actions > Security > Modify IAM role**.
5. Selected the **S3BucketAccess** role in the IAM role dropdown list.
6. Clicked **Update IAM role**.

**Screenshot: IAM role attached to Processor instance**

<img width="930" height="328" alt="Screenshot 2026-07-02 184329" src="https://github.com/user-attachments/assets/cd810e13-724a-4ada-b273-4e742f98aa12" />


---

## Task 2.1: Connecting to the Command Host EC2 instance

In this task, I used EC2 Instance Connect to connect to the "Command Host" EC2 instance.

1. Navigated to the EC2 Management Console.
2. In the navigation pane, selected **Instances**.
3. From the list of instances, selected **Command Host**.
4. Clicked **Connect**.
5. On the EC2 Instance Connect tab, clicked **Connect**.
6. A new browser tab opened with the EC2 Instance Connect terminal window.

**Screenshot: EC2 Instances list showing both Processor and Command Host instances**

<img width="1886" height="356" alt="Screenshot 2026-07-02 183946" src="https://github.com/user-attachments/assets/7343b882-0e9d-4b7f-850c-d41534cf2453" />


---

## Task 2.2: Taking an initial snapshot

In this task, I identified the EBS volume attached to the "Processor" instance and took an initial snapshot using commands in the EC2 Instance Connect terminal window.

### Step 1: Display the EBS volume-id

**Command:**
```bash
aws ec2 describe-instances --filter 'Name=tag:Name,Values=Processor' --query 'Reservations[0].Instances[0].BlockDeviceMappings[0].Ebs.{VolumeId:VolumeId}'
```

**Output:**
```
{
    "VolumeId": "vol-02e3023d369006ace"
}
```

The volume ID `vol-02e3023d369006ace` was retrieved and used throughout the lab steps.

**Screenshot: Volume ID retrieved**

<img width="1866" height="88" alt="Screenshot 2026-07-02 184752" src="https://github.com/user-attachments/assets/4565e025-0bb7-46c3-a3f5-1a71aac5d1ff" />


### Step 2: Obtain the instance ID

**Command:**
```bash
aws ec2 describe-instances --filters 'Name=tag:Name,Values=Processor' --query 'Reservations[0].Instances[0].InstanceId'
```

**Output:**
```
"i-0b88e10145eab7ade"
```

**Screenshot: Instance ID retrieved**

<img width="1493" height="78" alt="Screenshot 2026-07-02 184949" src="https://github.com/user-attachments/assets/ed9cc711-b21c-4acc-a6c7-7ea1041e9f3a" />


### Step 3: Shut down the Processor instance

**Command:**
```bash
aws ec2 stop-instances --instance-ids i-0b88e10145eab7ade
```

**Output:**
```
"StoppingInstances": [
    {
        "InstanceId": "i-0b88e10145eab7ade",
        "CurrentState": {
            "Code": 64,
            "Name": "stopping"
        },
        "PreviousState": {
            "Code": 16,
            "Name": "running"
        }
    }
]
```

**Screenshot: Instance stopping**

<img width="1048" height="353" alt="Screenshot 2026-07-02 185903" src="https://github.com/user-attachments/assets/7b957ccc-68cd-4a6e-aa96-52a94edef6ce" />


### Step 4: Create the first snapshot

**Command:**
```bash
aws ec2 create-snapshot --volume-id vol-02e3023d369006ace
```

**Output:**
```
{
    "Tags": [],
    "SnapshotId": "snap-0cc7148d76cb2e658",
    "VolumeId": "vol-02e3023d369006ace",
    "State": "pending",
    "StartTime": "2026-07-02T17:02:37.286Z",
    "Progress": "",
    "OwnerId": "505147674991",
    "Description": "",
    "VolumeSize": 8,
    "Encrypted": false
}
```

**Screenshot: Snapshot creation started**

<img width="1107" height="312" alt="Screenshot 2026-07-02 190244" src="https://github.com/user-attachments/assets/3c02fd1b-8ab7-4be6-8764-455b09331131" />


### Step 5: Verify snapshot completion and restart instance

**Command to verify snapshot:**
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-02e3023d369006ace"
```

**Output showing snapshot completed:**
```
"Snapshots": [
    {
        "StorageTier": "standard",
        "TransferType": "standard",
        "CompletionTime": "2026-07-02T17:03:30.094Z",
        "FullSnapshotSizeInBytes": 2051014656,
        "SnapshotId": "snap-0cc7148d76cb2e658",
        "VolumeId": "vol-02e3023d369006ace",
        "State": "completed",
        "StartTime": "2026-07-02T17:02:37.286Z",
        "Progress": "100%",
        "OwnerId": "505147674991",
        "Description": "",
        "VolumeSize": 8,
        "Encrypted": false
    }
]
```

**Screenshot: Snapshot completed successfully**

<img width="1398" height="497" alt="Screenshot 2026-07-02 190728" src="https://github.com/user-attachments/assets/910c02cb-cd60-4a4f-b157-86e9e5b23224" />



### Step 6: Restart the Processor instance

**Command:**
```bash
aws ec2 start-instances --instance-ids i-0b88e10145eab7ade
```

**Output:**
```
"StartingInstances": [
    {
        "InstanceId": "i-0b88e10145eab7ade",
        "CurrentState": {
            "Code": 0,
            "Name": "pending"
        },
        "PreviousState": {
            "Code": 80,
            "Name": "stopped"
        }
    }
]
```

**Screenshot: Instance restarting**

<img width="1027" height="362" alt="Screenshot 2026-07-02 190416" src="https://github.com/user-attachments/assets/bc727780-104d-4fbc-91f6-fa1da520c506" />


---

## Task 2.3: Scheduling the creation of subsequent snapshots

In this task, I used the Linux scheduling system (cron) to set up a recurring snapshot process. I scheduled a snapshot creation every minute to verify the results.

### Step 1: Create and schedule a cron job

**Command:**
```bash
echo "***** aws ec2 create-snapshot --volume-id vol-02e3023d369006ace 2>&1 >> /tmp/cronlog" > cronjob
crontab cronjob
```

**Screenshot: Cron job creation**

<img width="1533" height="60" alt="Screenshot 2026-07-02 190738" src="https://github.com/user-attachments/assets/dbc5279b-75a8-485b-95f0-87433b6d3deb" />


### Step 2: Verify snapshots being created

**Command:**
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-02e3023d369006ace"
```

After waiting a few minutes, multiple snapshots were generated.

**Output showing multiple snapshots:**
```
"Snapshots": [
    {
        "StorageTier": "standard",
        "CompletionTime": "2026-07-02T17:03:30.094Z",
        "SnapshotId": "snap-0cc7148d76cb2e658",
        "State": "completed",
        ...
    },
    {
        "CompletionTime": "2026-07-02T17:10:37.501Z",
        "SnapshotId": "snap-0980ebae83aeb0227",
        "State": "completed",
        ...
    }
]
```

**Screenshot: Multiple snapshots created by cron job**

<img width="1207" height="747" alt="Screenshot 2026-07-02 191114" src="https://github.com/user-attachments/assets/943efcef-1d9d-4b03-9ee1-d848354f9004" />


---

## Task 2.4: Retaining the last two snapshots

In this task, I ran a Python script that maintains only the last two snapshots for any given EBS volume.

### Step 1: Stop the cron job

**Command:**
```bash
crontab -r
```

**Screenshot: Cron job removed**

<img width="668" height="73" alt="Screenshot 2026-07-02 191430" src="https://github.com/user-attachments/assets/6c4b1da7-250a-4836-a6f0-0f127fd48431" />


### Step 2: List current snapshots before running script

**Command:**
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-02e3023d369006ace" --query 'Snapshots[*].SnapshotId'
```

**Output:**
```
{
    "snap-0db017315902b0159",
    "snap-0bcadd2d157a5f131",
    "snap-0cc7148d76cb2e658",
    "snap-0980ebae83aeb0227",
    "snap-03c7e945817ef2272",
    "snap-01acf65dfdd143c3f"
}
```

**Screenshot: Snapshots before cleanup**

<img width="1648" height="227" alt="Screenshot 2026-07-02 191517" src="https://github.com/user-attachments/assets/f171290a-64da-4be9-8f69-a9974e9aa9b2" />


### Step 3: Run the snapshotter_v2.py script

**Command:**
```bash
python3.8 snapshotter_v2.py
```

**Output:**
```
Deleting snapshot snap-0cc7148d76cb2e658
Deleting snapshot snap-0980ebae83aeb0227
Deleting snapshot snap-01acf65dfdd143c3f
Deleting snapshot snap-0bcadd2d157a5f131
Deleting snapshot snap-03c7e945817ef2272
```

**Screenshot: Script execution and deletions**

<img width="635" height="172" alt="Screenshot 2026-07-02 191545" src="https://github.com/user-attachments/assets/fa9eda65-ce27-4f86-8e94-3c090b0debea" />


### Step 4: Verify only two snapshots remain

**Command:**
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-02e3023d369006ace" --query 'Snapshots[*].SnapshotId'
```

**Output:**
```
{
    "snap-0f4d80fe614d56221",
    "snap-0db017315902b0159"
}
```

**Screenshot: Only two snapshots remain**

<img width="1502" height="145" alt="Screenshot 2026-07-02 191607" src="https://github.com/user-attachments/assets/6343f1db-d21c-4891-b133-dcf1748cf177" />


---

## Task 3.1: Downloading and unzipping sample files

In this task, I downloaded and unzipped sample files on the Command Host instance.

### Step 1: Download sample files

**Command:**
```bash
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RSJAWS-3-124627/183-lab-JAWS-managing-storage/s3/files.zip
```

**Output:**
```
--2026-07-02 17:17:27--  https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RSJAWS-3-124627/183-lab-JAWS-managing-storage/s3/files.zip
Resolving aws-tc-largeobjects.s3.us-west-2.amazonaws.com... 52.92.144.162, 16.15.47.254, 52.92.147.114, ...
Connecting to aws-tc-largeobjects.s3.us-west-2.amazonaws.com|52.92.144.162|443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 72110 (70K) [application/zip]
Saving to: 'files.zip'

100%[==================>] 72,110 --.-K/s  in 0.004s

2026-07-02 17:17:27 (19.5 MB/s) - 'files.zip' saved [72110/72110]
```

**Screenshot: Files downloaded**

<img width="1882" height="313" alt="Screenshot 2026-07-02 191734" src="https://github.com/user-attachments/assets/95b06b33-c795-47c1-bfea-8f533b004b7e" />


### Step 2: Unzip the files

**Command:**
```bash
unzip files.zip
```

**Output:**
```
Archive:  files.zip
    inflating: files/file1.txt
    inflating: files/file2.txt
    inflating: files/file3.txt
```

**Screenshot: Files unzipped**

<img width="617" height="185" alt="Screenshot 2026-07-02 191750" src="https://github.com/user-attachments/assets/12e996e5-c910-44fb-99dc-6eb6782f1a54" />


---

## Task 3.2: Syncing files

In this task, I activated versioning on the S3 bucket, synced files, and managed file deletion and recovery.

### Step 1: Activate versioning on the bucket

**Command:**
```bash
aws s3api put-bucket-versioning --bucket hlabz2 --versioning-configuration Status=Enabled
```

**Screenshot: Versioning enabled**

<img width="1355" height="62" alt="Screenshot 2026-07-02 191829" src="https://github.com/user-attachments/assets/84875a64-680b-45f2-bbc7-018d50323402" />


### Step 2: Sync files to S3 bucket

**Command:**
```bash
aws s3 sync files s3://hlabz2/files/
```

**Output:**
```
upload: files/file1.txt to s3://hlabz2/files/file1.txt
upload: files/file2.txt to s3://hlabz2/files/file2.txt
upload: files/file3.txt to s3://hlabz2/files/file3.txt
```

**Screenshot: Files synced to S3**

<img width="671" height="116" alt="Screenshot 2026-07-02 191901" src="https://github.com/user-attachments/assets/12faef62-27e8-4c3d-ad37-827857547905" />


### Step 3: Verify files in S3 bucket

**Command:**
```bash
aws s3 ls s3://hlabz2/files/
```

**Output:**
```
2026-07-02 17:18:56    30318 file1.txt
2026-07-02 17:18:57    43784 file2.txt
2026-07-02 17:18:57    96675 file3.txt
```

**Screenshot: Files verified in S3**

<img width="665" height="167" alt="Screenshot 2026-07-02 191936" src="https://github.com/user-attachments/assets/d1cfd2b8-f97f-43cd-91b5-3b20d3f98d81" />


### Step 4: Delete a local file and sync with --delete

**Command to delete local file:**
```bash
rm files/file1.txt
```

**Command to sync with delete option:**
```bash
aws s3 sync files s3://hlabz2/files/ --delete
```

**Output:**
```
delete: s3://hlabz2/files/file1.txt
```

**Screenshot: File deleted with --delete option**

<img width="780" height="172" alt="Screenshot 2026-07-02 192038" src="https://github.com/user-attachments/assets/ef1d8933-15b8-4880-bd12-5972db6a819b" />


### Step 5: Verify file deletion in S3

**Command:**
```bash
aws s3 ls s3://hlabz2/files/
```

**Output:**
```
2026-07-02 17:18:57    43784 file2.txt
2026-07-02 17:18:57    96675 file3.txt
```

**Screenshot: Only two files remain in S3**

<img width="780" height="172" alt="Screenshot 2026-07-02 192038" src="https://github.com/user-attachments/assets/9eb7f0df-4a96-49cc-9379-9c5a25a26f79" />


### Step 6: View object versions

**Command:**
```bash
aws s3api list-object-versions --bucket hlabz2 --prefix files/file1.txt
```

**Output:**
```
"Versions": [
    {
        "ETag": "\"b76b2b775023e60be16bc332496f8409\"",
        "Size": 30318,
        "StorageClass": "STANDARD",
        "Key": "files/file1.txt",
        "VersionId": ".ejMsiNer13JAgQhtoNPmtFt7JbyQfdG",
        "IsLatest": false,
        "LastModified": "2026-07-02T17:18:56.000Z"
    }
],
"DeleteMarkers": [
    {
        "Key": "files/file1.txt",
        "VersionId": "Cec.pWzZVqDrgw0VspxLY.DKB7zGy452",
        "IsLatest": true,
        "LastModified": "2026-07-02T17:20:19.000Z"
    }
]
```

**Screenshot: Versioning details for deleted file**

<img width="1037" height="712" alt="Screenshot 2026-07-02 192112" src="https://github.com/user-attachments/assets/8b9abc2f-552a-4c7a-8702-827d5c5714d7" />


### Step 7: Download the previous version of the file

**Command:**
```bash
aws s3api get-object --bucket hlabz2 --key files/file1.txt --version-id .ejMsiNer13JAgQhtoNPmtFt7JbyQfdG files/file1.txt
```

**Output:**
```
"AcceptRanges": "bytes",
"LastModified": "Thu, 02 Jul 2026 17:18:56 GMT",
"ContentLength": 30318,
"ETag": "\"b76b2b775023e60be16bc32496f8409\"",
"ChecksumCRC32": "qqrPtQ==",
"ChecksumType": "FULL OBJECT",
"VersionId": ".ejMsiNer13JAgQhtoNfmtFt7JbyQfdG",
"ContentType": "text/plain",
"ServerSideEncryption": "AES256",
"Metadata": {}
```

**Screenshot: Previous version downloaded**

<img width="1565" height="360" alt="Screenshot 2026-07-02 192407" src="https://github.com/user-attachments/assets/aabb059a-59c6-40a8-b035-005b0f58bd17" />


### Step 8: Verify file restored locally

**Command:**
```bash
ls files
```

**Output:**
```
file1.txt  file2.txt  file3.txt
```

**Screenshot: All three files restored locally**

<img width="377" height="77" alt="Screenshot 2026-07-02 192420" src="https://github.com/user-attachments/assets/43cbae69-f55b-4234-b70a-906b1696f406" />


### Step 9: Re-sync the contents to S3

**Command:**
```bash
aws s3 sync files s3://hlabz2/files/
```

**Output:**
```
upload: files/file1.txt to s3://hlabz2/files/file1.txt
```

**Screenshot: File re-synced to S3**

<img width="697" height="160" alt="Screenshot 2026-07-02 192515" src="https://github.com/user-attachments/assets/9238085c-eec7-4452-ad7e-d56ea1d4162b" />

### Step 10: Verify file in S3 bucket

**Command:**
```bash
aws s3 ls s3://hlabz2/files/
```

**Output:**
```
2026-07-02 17:24:48    30318 file1.txt
2026-07-02 17:18:57    43784 file2.txt
2026-07-02 17:18:57    96675 file3.txt
```

**Screenshot: All three files restored in S3**

<img width="697" height="160" alt="Screenshot 2026-07-02 192515" src="https://github.com/user-attachments/assets/122c7824-0831-431f-a10b-ed72ee5baeff" />


---

## Conclusion

I successfully completed the following tasks:

- Created and maintained snapshots for Amazon EC2 instances, including scheduling automatic snapshots and retaining only the two most recent snapshots.
- Used Amazon S3 sync to copy files from an EBS volume to an S3 bucket.
- Used Amazon S3 versioning to retrieve deleted files by listing object versions, downloading a previous version, and re-syncing to S3.

Throughout this lab, I gained practical experience with AWS storage management, including EBS snapshots, S3 bucket operations, versioning, and CLI commands for managing AWS resources efficiently.
