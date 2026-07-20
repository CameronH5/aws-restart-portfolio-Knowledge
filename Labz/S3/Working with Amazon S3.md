# Working with Amazon S3

## Lab Report

### Objectives
By the end of this lab, I was able to do the following:

- Use the `s3api` and `s3` AWS CLI commands to create and configure an S3 bucket
- Verify write permissions to a user on an S3 bucket
- Configure event notification on an S3 bucket

---

## Task 1: Connecting to the CLI Host EC2 instance and configuring the AWS CLI

### Task 1.1: Connecting to the CLI Host EC2 instance

I connected to the CLI Host EC2 instance using EC2 Instance Connect. From the AWS Management Console, I navigated to the EC2 service, selected the CLI Host instance from the list, chose **Connect**, and then selected the **EC2 Instance Connect** tab. This opened a new browser tab with the EC2 Instance Connect terminal window that I used to complete all the tasks throughout the lab.

### Task 1.2: Configuring the AWS CLI on the CLI Host instance

I configured the AWS CLI by running the `aws configure` command in the EC2 Instance Connect terminal. At the prompts, I entered:

- **AWS Access Key ID**: The AccessKey value
- **AWS Secret Access Key**: The SecretKey value
- **Default region name**: `us-west-2`
- **Default output format**: `json`

After completing these steps, I was ready to run AWS CLI commands to interact with AWS services.

---

## Task 2: Creating and initializing the S3 share bucket

### Creating the S3 Bucket

I created an S3 bucket using the following command:

```bash
aws s3 mb s3://cafe-hug0z --region 'us-west-2'
```

The output confirmed the bucket was created successfully:

```
make_bucket: cafe-hug0z
```

<img width="785" height="71" alt="Screenshot 2026-07-01 190015" src="https://github.com/user-attachments/assets/0e1ca751-aa53-42ef-8b22-7b67ace5c94d" />

### Uploading Images to the Bucket

I uploaded sample images to the bucket under the `/images` prefix using the sync command:

```bash
aws s3 sync ~/initial-images/ s3://cafe-hug0z/images
```

The command output showed the three image files being uploaded:
- Cup-of-Hot-Chocolate.jpg
- Donuts.jpg
- Strawberry-Tarts.jpg

<img width="1061" height="126" alt="Screenshot 2026-07-01 190109" src="https://github.com/user-attachments/assets/9e86f359-03e2-42e4-9eb4-957e1cc3d479" />


### Verifying the Upload

I verified the files were synced to the S3 bucket with the following command:

```bash
aws s3 ls s3://cafe-hug0z/images/ --human-readable --summarize
```

The output confirmed:
- **Total Objects**: 3
- **Total Size**: 1.1 MiB

<img width="977" height="163" alt="Screenshot 2026-07-01 190153" src="https://github.com/user-attachments/assets/9264582d-d743-4437-8b2a-03fab6af574d" />


---

## Task 3: Reviewing the IAM group and user permissions

### Task 3.1: Reviewing the mediaco IAM group

I navigated to the IAM Management Console and reviewed the permissions assigned to the **mediaco** IAM user group. The group had two policies attached:

**IAMUserChangePassword Policy**: This AWS managed policy permits users to change their own password. It allows the `iam:ChangePassword` action and the `iam:GetAccountPasswordPolicy` action.

<img width="1902" height="662" alt="Screenshot 2026-07-01 190518" src="https://github.com/user-attachments/assets/38477f10-a8fd-4061-ba57-d7c5ac4a0ab7" />


**mediaCoPolicy Policy**: This custom policy contained several statements:
- The first statement (`AllowGroupToSeeBucketListInTheConsole`) allowed users to view the list of S3 buckets in the account.
- The second statement (`AllowRootLevelListingOfTheBucket`) allowed users to view first-level objects in the cafe bucket.
- The third statement (`AllowUserSpecificActionsOnlyInTheSpecificPrefix`) specified actions users could perform on objects in the `cafe-*/images/*` folder, including `GetObject`, `PutObject`, and `DeleteObject`.

<img width="1458" height="593" alt="Screenshot 2026-07-01 190610" src="https://github.com/user-attachments/assets/44ba02b4-756d-4f3f-b9f5-03ff1503110b" />


<img width="1242" height="451" alt="Screenshot 2026-07-01 190632" src="https://github.com/user-attachments/assets/5e69645d-f184-44ad-ad9e-c78ec6eb94b6" />

<img width="1473" height="532" alt="Screenshot 2026-07-01 190641" src="https://github.com/user-attachments/assets/f56dd170-09e2-47d3-898a-8ba57c458163" />


### Task 3.2: Reviewing the mediacouser IAM user

I reviewed the properties of the **mediacouser** IAM user. On the **Permissions** tab, I confirmed the user inherited two policies from the mediaco group: **IAMUserChangePassword** and **mediaCoPolicy**. The **Groups** tab confirmed the user was a member of the mediaco group.

I then created access keys for the mediacouser user:

1. Navigated to the **Security credentials** tab
2. Chose **Create access key**
3. Selected **Command Line Interface (CLI)**
4. Created the access key
5. Downloaded the `.csv` file containing the credentials
6. Copied the **Console sign-in link**

<img width="1325" height="630" alt="Screenshot 2026-07-01 190938" src="https://github.com/user-attachments/assets/c21f317d-7730-4e80-a885-d366a36239ad" />


<img width="1447" height="437" alt="Screenshot 2026-07-01 190957" src="https://github.com/user-attachments/assets/0b5cf280-564e-4bd7-a113-ef65ec966f96" />


### Task 3.3: Testing the mediacouser permissions

I signed in as the mediacouser user in a new incognito browser window using:
- **IAM user name**: mediacouser
- **Password**: Training1!

#### Testing the View Use Case

I navigated to the S3 console and selected the bucket I created (cafe-hug0z). I opened the `images/` folder and selected **Donuts.jpg**, then chose **Open**. A new browser tab opened showing the image of various donuts.

<img width="1902" height="598" alt="Screenshot 2026-07-01 191615" src="https://github.com/user-attachments/assets/2e980d17-b94f-4383-9337-b39b3daf5ea2" />


<img width="1721" height="683" alt="Screenshot 2026-07-01 191642" src="https://github.com/user-attachments/assets/8e9fa158-b19f-4857-8457-75bcae7719e7" />


#### Testing the Upload Use Case

I tested the upload use case by:

1. Selecting **Upload**
2. Adding a picture from my local computer
3. Choosing **Upload**
4. The upload succeeded (1 file, 841.1 KB)

<img width="1911" height="666" alt="Screenshot 2026-07-01 191810" src="https://github.com/user-attachments/assets/1b56b778-f604-4d0a-ae14-37649b34f01b" />


I then opened the uploaded file to verify it was accessible.

<img width="1197" height="567" alt="Screenshot 2026-07-01 191819" src="https://github.com/user-attachments/assets/945536a1-0954-487b-82f3-aa0ff6aab7da" />


#### Testing the Delete Use Case

I tested the delete use case by:

1. Selecting the checkbox for **Cup-of-Hot-Chocolate.jpg**
2. Choosing **Delete**
3. Entering `delete` to confirm
4. Choosing **Delete objects**

The object was successfully deleted and no longer appeared in the image list.

<img width="1878" height="786" alt="Screenshot 2026-07-01 191912" src="https://github.com/user-attachments/assets/bfebfa8f-9e6a-430a-8469-f49cb8236a71" />


<img width="1041" height="502" alt="Screenshot 2026-07-01 191922" src="https://github.com/user-attachments/assets/c937f69c-239a-4224-888b-cdc214f963c8" />


<img width="612" height="532" alt="Screenshot 2026-07-01 191932" src="https://github.com/user-attachments/assets/3d2e6d0a-56ec-4d56-95fa-819c97edda92" />


#### Testing the Unauthorized Use Case

I tested the unauthorized use case by attempting to change the bucket's permissions. When I selected the **Permissions** tab, the following error was displayed:

> **Access denied**
> 
> User: arn:aws:iam::292395993294:user/mediacourier is not authorized to perform: s3:GetBucketPublicAccessBlock on resource: "arn:aws:s3::cafe-hug02"

This confirmed that mediacouser was prevented from changing the bucket permissions.

<img width="1837" height="682" alt="Screenshot 2026-07-01 192055" src="https://github.com/user-attachments/assets/3acaec20-3a3d-457c-b57f-2ecfd0e90208" />


I then signed out of the Amazon S3 console as mediacouser.

---

## Task 4: Configuring event notifications on the S3 share bucket

### Task 4.1: Creating and configuring the s3NotificationTopic SNS topic

I returned to the AWS Management Console as the `voclabs/user` and created an SNS topic:

1. Navigated to the Simple Notification Service console
2. Chose **Topics** from the navigation pane
3. Selected **Create topic**
4. Chose **Standard** as the topic type
5. Entered **Name**: `s3NotificationTopic`
6. Created the topic

<img width="1341" height="682" alt="Screenshot 2026-07-01 192221" src="https://github.com/user-attachments/assets/5df8c007-2990-4deb-a080-71b2e50f7af1" />


I copied the **ARN** value for the topic:
```
arn:aws:sns:us-west-2:292395993294:s3NotificationTopic
```

#### Configuring the Topic's Access Policy

I edited the topic's access policy by replacing the contents with a policy that grants the S3 bucket permission to publish messages to the topic:

```json
{
  "Version": "2008-10-17",
  "Id": "S3PublishPolicy",
  "Statement": [
    {
      "Sid": "AllowPublishFromS3",
      "Effect": "Allow",
      "Principal": {
        "Service": "s3.amazonaws.com"
      },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:us-west-2:292395993294:s3NotificationTopic",
      "Condition": {
        "ArnLike": {
          "aws:SourceArn": "arn:aws:s3:*:*:*:cafe-hug0z"
        }
      }
    }
  ]
}
```

<img width="1013" height="313" alt="Screenshot 2026-07-01 192603" src="https://github.com/user-attachments/assets/7e80155b-960a-4f3b-9a2f-2c75c7a0a0b8" />


#### Subscribing to the Topic

I created a subscription to the topic:

1. Selected the **Subscriptions** tab
2. Chose **Create subscription**
3. Selected the s3NotificationTopic
4. Chose **Email** as the protocol
5. Entered an email address
6. Created the subscription

<img width="1392" height="575" alt="Screenshot 2026-07-01 192716" src="https://github.com/user-attachments/assets/f795fc13-1638-452c-b242-bb7d03a00197" />


I received a confirmation email and clicked the **Confirm subscription** link.

<img width="1292" height="442" alt="Screenshot 2026-07-01 192740" src="https://github.com/user-attachments/assets/a958c11d-e981-4510-b59f-0007081a2ce4" />


<img width="731" height="311" alt="Screenshot 2026-07-01 192752" src="https://github.com/user-attachments/assets/6c48cc6b-8394-4748-b055-c23fff797b57" />


### Task 4.2: Adding an event notification configuration to the S3 bucket

I created an event notification configuration file named `s3EventNotification.json` using the `vi` editor:

```json
{
  "TopicConfigurations": [
    {
      "TopicArn": "arn:aws:sns:us-west-2:292395993294:s3NotificationTopic",
      "Events": ["s3:ObjectCreated:*", "s3:ObjectRemoved:*"],
      "Filter": {
        "Key": {
          "FilterRules": [
            {
              "Name": "prefix",
              "Value": "images/"
            }
          ]
        }
      }
    }
  ]
}
```

This configuration requests Amazon S3 to publish event notifications whenever `ObjectCreated` or `ObjectRemoved` events occur on objects in the `images/` folder.

<img width="677" height="53" alt="Screenshot 2026-07-01 193050" src="https://github.com/user-attachments/assets/671fea8f-44e3-40da-a2d7-d548b0255f81" />
<img width="812" height="496" alt="Screenshot 2026-07-01 193058" src="https://github.com/user-attachments/assets/e1fc6d7c-bb0d-4ef4-9947-8ce75738701a" />


I associated the event configuration file with the S3 bucket:

```bash
aws s3api put-bucket-notification-configuration --bucket cafe-hug0z --notification-configuration file://s3EventNotification.json
```

<img width="1622" height="51" alt="Screenshot 2026-07-01 193220" src="https://github.com/user-attachments/assets/529a43d3-91d4-4074-86b2-d8dce61e13fb" />


I received a test email notification with the event `s3:TestEvent`, confirming the configuration was successful.

<img width="1482" height="502" alt="Screenshot 2026-07-01 193340" src="https://github.com/user-attachments/assets/222c000b-d922-433c-a065-4b8736783e14" />


---

## Task 5: Testing the S3 share bucket event notifications

### Configuring CLI with mediacouser Credentials

I configured the AWS CLI to use the mediacouser credentials:

```bash
aws configure
```

I entered the Access Key ID and Secret Access Key from the `.csv` file downloaded in Task 3, set the default region to `us-west-2`, and the default output format to `json`.

![Screenshot of AWS configure with mediacouser credentials](Screenshot%202026-07-01%20193648.png)

### Testing the PUT Operation

I tested the put use case by uploading the Caramel-Delight.jpg image:

```bash
aws s3api put-object --bucket cafe-hug0z --key images/Caramel-Delight.jpg --body ~/new-images/Caramel-Delight.jpg
```

The command returned:
```json
{
    "ETag": "\"31ac30da619244b0ce786f106e4f3df7\"",
    "ServerSideEncryption": "AES256"
}
```

<img width="1645" height="257" alt="Screenshot 2026-07-01 193648" src="https://github.com/user-attachments/assets/67df40dd-0c45-482b-babb-c54144cdd4cb" />


I received an email notification with:
- **eventName**: `ObjectCreated:Put`
- **key**: `images/Caramel-Delight.jpg`

<img width="1502" height="377" alt="Screenshot 2026-07-01 193722" src="https://github.com/user-attachments/assets/0b6b3dbc-a435-44f2-81b1-3d3345b06ce4" />


### Testing the GET Operation

I tested the get use case by retrieving the Donuts.jpg object:

```bash
aws s3api get-object --bucket cafe-hug0z --key images/Donuts.jpg Donuts.jpg
```

The command successfully retrieved the object:
```json
{
    "AcceptRanges": "bytes",
    "ContentType": "image/jpeg",
    "LastModified": "Wed, 01 Jul 2026 17:01:05 GMT",
    "ContentLength": 380753,
    "ETag": "\"405b0bcc53cb5ab713c967dc1422b4f4\"",
    "ServerSideEncryption": "AES256",
    "Metadata": {}
}
```

**Note**: No email notification was generated for this operation, as expected, since notifications are configured only for object creation and deletion.

<img width="1530" height="327" alt="Screenshot 2026-07-01 194026" src="https://github.com/user-attachments/assets/63f48cb1-b5d9-48a2-91b6-5f6c3ae9881d" />


### Testing the DELETE Operation

I tested the delete use case by deleting the Strawberry-Tarts.jpg object:

```bash
aws s3api delete-object --bucket cafe-hug0z --key images/Strawberry-Tarts.jpg
```

<img width="1112" height="45" alt="Screenshot 2026-07-01 194119" src="https://github.com/user-attachments/assets/ac17b9f3-cccd-472a-8e91-329a67da5bce" />


I received an email notification with:
- **eventName**: `ObjectRemoved:Delete`
- **key**: `images/Strawberry-Tarts.jpg`

<img width="1492" height="351" alt="Screenshot 2026-07-01 194132" src="https://github.com/user-attachments/assets/dee9d9d9-6234-4217-ac06-01a0b330fb63" />


### Testing the Unauthorized Use Case

I attempted to change the permissions of the Donuts.jpg object to allow public read access:

```bash
aws s3api put-object-acl --bucket cafe-hug0z --key images/Donuts.jpg --acl public-read
```

The command failed as expected:

> **An error occurred (AccessDenied)** when calling the PutObjectACL operation: User: arn:aws:iam::292395993294:user/mediacouser is not authorized to perform: s3:PutObjectACL on resource: "arn:aws:s3:::cafe-hug0z/images/Donuts.jpg" because public ACLs are prevented by the BlockPublicACLs setting in S3 Block Public Access.

<img width="1896" height="115" alt="Screenshot 2026-07-01 194216" src="https://github.com/user-attachments/assets/8f961472-e6ce-449a-88d5-a5d5b5dbf445" />


This confirmed that the mediacouser user did not have permission to modify object permissions.

---

## Conclusion

In this lab, I successfully completed the following tasks:

1. **Connected to the CLI Host EC2 instance** using EC2 Instance Connect and configured the AWS CLI with the appropriate credentials.

2. **Created and initialized an S3 bucket** named `cafe-hug0z` using `s3 mb`, uploaded sample images with `s3 sync`, and verified the upload using `s3 ls`.

3. **Reviewed IAM permissions** for the mediaco group and mediacouser user, and tested:
   - **View** use case - successfully viewed Donuts.jpg
   - **Upload** use case - successfully uploaded a file
   - **Delete** use case - successfully deleted Cup-of-Hot-Chocolate.jpg
   - **Unauthorized** use case - prevented from changing bucket permissions

4. **Configured event notifications** by:
   - Creating an SNS topic named `s3NotificationTopic`
   - Applying an access policy allowing S3 to publish to the topic
   - Subscribing an email endpoint
   - Adding an event notification configuration to the S3 bucket with `put-bucket-notification-configuration`

5. **Tested event notifications** by:
   - Uploading an object (received `ObjectCreated:Put` notification)
   - Downloading an object (no notification generated)
   - Deleting an object (received `ObjectRemoved:Delete` notification)
   - Attempting an unauthorized operation (received `AccessDenied` error)

The lab demonstrated the successful implementation of a secure S3 bucket with event notifications, proper IAM permissions, and appropriate restrictions on unauthorized actions.
