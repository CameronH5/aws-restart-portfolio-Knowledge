# Challenge Lab: Amazon S3

## Objectives
By the end of this challenge, I was able to do the following:

- Create an S3 bucket
- Upload an object into the bucket
- Access the object using a web browser
- List the contents of the S3 bucket using the AWS Command Line Interface (AWS CLI)

---

## Step-by-Step Implementation

### 1. Connecting to the CLI Host EC2 Instance

I connected to the **CLI Host** EC2 instance using EC2 Instance Connect. The instance was running in the `us-west-2a` Availability Zone with a public IPv4 address.

<img width="1887" height="232" alt="Screenshot 2026-07-02 193635" src="https://github.com/user-attachments/assets/0d72fb50-9c1a-4729-acce-0b785ae3ab22" />


---

### 2. Creating the S3 Bucket

I created a new S3 bucket using the `aws s3 mb` command. The bucket name I chose was `1ab184c`, but later I used the bucket `lab184c` for the object upload (the bucket `lab184c` was already available). The command I executed was:

```bash
aws s3 mb s3://1ab184c --region 'us-west-2'
```

The output confirmed successful bucket creation:

```
make_bucket: 1ab184c
```

<img width="773" height="102" alt="Screenshot 2026-07-02 194350" src="https://github.com/user-attachments/assets/df630a74-7d74-4cb8-a0d4-da6e9efc5346" />


> **Note:** In the subsequent steps, I used the bucket named `lab184c` for uploading and managing the object. This bucket existed prior to the lab.

---

### 3. Uploading an Object to the Bucket

I created a simple text file with the content `"Hello from my public object"` and uploaded it to the S3 bucket using the `aws s3 cp` command.

First, I created the file:

```bash
echo "Hello from my public object" > hello.txt
```

Then I uploaded it to the bucket `lab184c`:

```bash
aws s3 cp hello.txt s3://lab184c/hello.txt
```

The command output showed the upload was successful:

```
upload: ./hello.txt to s3://lab184c/hello.txt
```

<img width="805" height="115" alt="Screenshot 2026-07-02 194445" src="https://github.com/user-attachments/assets/99e5c0da-47c3-42ae-b6e6-7e35cb84f4d4" />


I verified the object details in the S3 console. The object `hello.txt` had a size of 28.0 bytes, and its Object URL was `https://lab184c.s3.us-west-2.amazonaws.com/hello.txt`.

<img width="1277" height="633" alt="Screenshot 2026-07-02 194534" src="https://github.com/user-attachments/assets/1d89342d-c016-4e51-99fc-13adb83e4d80" />

---

### 4. Making the Object Publicly Accessible

Initially, when I attempted to access the object via a web browser using the Object URL, I received an **Access Denied** error because the bucket had **Block all public access** enabled.

<img width="1073" height="227" alt="Screenshot 2026-07-02 194609" src="https://github.com/user-attachments/assets/b2418e7c-5cdd-4cf9-ae64-22316958c181" />


To enable public access, I modified the bucket's **Block Public Access** settings:

1. Navigated to the bucket **Permissions** tab.
2. Turned **off** the **Block all public access** setting.

<img width="1535" height="258" alt="Screenshot 2026-07-02 194749" src="https://github.com/user-attachments/assets/9a36a5cc-3af9-4be4-845e-6e3e5a905ce0" />

*Initially, Block all public access was turned on.*

<img width="947" height="277" alt="Screenshot 2026-07-02 194806" src="https://github.com/user-attachments/assets/f5bdd5a0-5d95-4baf-9497-b797056cd9a0" />


*After turning off Block all public access.*

Next, I needed to make the specific object public. I used the **Make public using ACL** action from the object's context menu.
<img width="1862" height="765" alt="Screenshot 2026-07-02 201647" src="https://github.com/user-attachments/assets/4b008e7c-71d0-49d0-b1b4-84bf5c1da906" />

 *Actually this is the same screenshot, but later ones show the "Make public" dialog.*

The **Make public** dialog confirmed that I was about to enable public read access for the object.

<img width="1872" height="507" alt="Screenshot 2026-07-02 201726" src="https://github.com/user-attachments/assets/ba09bbe2-faf1-4375-8db6-754d808f424d" />


After confirming, I received a success message.

<img width="485" height="95" alt="Screenshot 2026-07-02 201736" src="https://github.com/user-attachments/assets/105aa6be-aed5-453e-9032-5f756fe199ff" />


I also verified that the object now had public read access by checking the object's permissions.

<img width="1716" height="626" alt="Screenshot 2026-07-02 201748" src="https://github.com/user-attachments/assets/624e82b6-6802-4186-a8d8-7d702093ba62" />


---

### 5. Accessing the Object Using a Web Browser

With public access enabled, I opened the Object URL in a web browser:

`https://lab184c.s3.us-west-2.amazonaws.com/hello.txt`

The browser displayed the content of the file:

```
Hello from my public object
```

<img width="660" height="161" alt="Screenshot 2026-07-02 201834" src="https://github.com/user-attachments/assets/49866bd2-7837-4a76-a3b6-fa21cd5a5f14" />


This confirmed that the object was successfully made public and accessible from anywhere.

---

### 6. Listing the Bucket Contents Using the AWS CLI

Finally, I used the `aws s3 ls` command to list the contents of the S3 bucket:

```bash
aws s3 ls s3://lab184c/
```

The output showed the `hello.txt` file with its size and last modified date:

```
2026-07-02 17:44:40    28 hello.txt
```

<img width="605" height="83" alt="Screenshot 2026-07-02 202155" src="https://github.com/user-attachments/assets/48c381dd-83d3-405d-9ed2-7be0ac1c4d49" />


---

## Conclusion

In this challenge lab, I successfully:

- Created an S3 bucket using the `aws s3 mb` command.
- Uploaded a text object into the bucket using `aws s3 cp`.
- Made the object publicly readable by disabling Block Public Access and applying a public ACL.
- Accessed the object through a web browser using its Object URL.
- Listed the bucket contents using the AWS CLI with `aws s3 ls`.

This exercise demonstrated the fundamental steps of working with Amazon S3, including bucket creation, object upload, permission management, and verification via multiple access methods.
