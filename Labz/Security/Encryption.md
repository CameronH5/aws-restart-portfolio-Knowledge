<img width="1873" height="757" alt="Screenshot 2026-08-03 175640" src="https://github.com/user-attachments/assets/072f5299-9c82-4549-a5b6-bcebc17f878c" /># Data Protection Using Encryption - Lab Report

## Lab Overview
In this lab, I learned the foundational concepts of cryptography and encryption. The primary objective was to secure data using encryption, moving it from a readable form (plaintext) into an unreadable form (ciphertext) using cryptographic keys, and then transforming it back into its original state. To accomplish this, I performed the following tasks within an AWS environment: creating an AWS KMS key, installing the AWS Encryption CLI, encrypting a plaintext file, and decrypting the resulting ciphertext.

## Objectives Achieved
After completing the lab, I successfully achieved the following:
*   Created an AWS KMS encryption key.
*   Installed the AWS Encryption CLI.
*   Encrypted plaintext data.
*   Decrypted the resulting ciphertext.

---

## Task 1: Created an AWS KMS Key
I started by navigating to the AWS Key Management Service (KMS) console. My goal here was to create a symmetric encryption key. 
*   I initiated the key creation process and selected **Symmetric** for the Key type.
*   I added a label with the alias `MyKMSKey` and provided a description: *Key used to encrypt and decrypt data files*.
*   On the administrative permissions page, I selected the `voclabs` IAM role to act as the key administrator.
*   Similarly, on the usage permissions page, I gave the `voclabs` role permission to use the key for encryption and decryption activities.
*   After reviewing my settings, I successfully created the key. I then navigated to the newly created key's details page and copied the **ARN** (Amazon Resource Name), saving it to a text editor for use later in the lab.

<img width="1873" height="757" alt="Screenshot 2026-08-03 175640" src="https://github.com/user-attachments/assets/b6a7f5bb-2ac1-4d8a-93d7-55983b8724d1" />

<img width="1822" height="595" alt="Screenshot 2026-08-03 175708" src="https://github.com/user-attachments/assets/83b4d8c3-cabd-4ecf-aa21-011082734e9b" />

<img width="1748" height="522" alt="Screenshot 2026-08-03 175746" src="https://github.com/user-attachments/assets/b30c6af0-9b61-4ffc-99d4-c44df5554142" />

---

## Task 2: Configured the File Server Instance and AWS CLI
Next, I configured the File Server EC2 instance to communicate with the AWS KMS key I just created. I also installed the AWS Encryption CLI on this server.

### Configured AWS Credentials
I connected to the EC2 instance using AWS Session Manager. Upon accessing the terminal, I configured the AWS Command Line Interface (CLI) by running the `aws configure` command. I initially used `1` and `1` as placeholders for the keys.

**Command executed:**
```bash
aws configure
```

<img width="542" height="257" alt="Screenshot 2026-08-03 180652" src="https://github.com/user-attachments/assets/32b9c81e-8bde-4f53-aa83-e048880899c1" />

I then returned to the Vocareum console, where I retrieved the actual AWS credentials (Access Key ID, Secret Access Key, and Session Token) from the "AWS CLI" section. I opened the AWS credentials file using the `vi` editor:

**Command executed:**
```bash
vi ~/.aws/credentials
```
Inside `vi`, I removed the placeholder content using `dd` and pasted the credentials I copied from the Vocareum dashboard. I saved and exited the file using `:wq`.
To verify the credentials were correctly updated, I ran the following command:


<img width="1281" height="632" alt="Screenshot 2026-08-03 180522" src="https://github.com/user-attachments/assets/52847381-fd29-4d71-8b25-13d8d9cdcdd2" />
<img width="1917" height="356" alt="Screenshot 2026-08-03 180605" src="https://github.com/user-attachments/assets/3da7eaaf-d5a2-45b7-9d5a-eba060fbee8d" />

**Command executed:**
```bash
cat ~/.aws/credentials
```
<img width="1907" height="272" alt="Screenshot 2026-08-03 180712" src="https://github.com/user-attachments/assets/a6fc6355-0891-42e7-a03b-3f4fb2474f98" />


### Installed the AWS Encryption CLI
After configuring the credentials, I installed the AWS Encryption CLI. This tool allows users to securely encrypt and decrypt data from the command line. I ran the following command to install the CLI using `pip3`:

**Command executed:**
```bash
pip3 install aws-encryption-sdk-cli
```
<img width="1380" height="702" alt="Screenshot 2026-08-03 180805" src="https://github.com/user-attachments/assets/69afb48b-8d4e-427a-97a5-6a1612dabaf6" />


To ensure the CLI commands could be executed from any directory, I added the `local/bin` path to my user environment variables:

**Command executed:**
```bash
export PATH=$PATH:/home/ssm-user/.local/bin
```

---

## Task 3: Encrypted and Decrypted Data
For this final task, I created a mock sensitive text file, used the AWS Encryption CLI to encrypt it, verified the ciphertext, and successfully decrypted it to reveal the original content.

### Created the Plaintext File
I started by creating three plaintext files. I inserted a secret message into `secret1.txt`:

**Command executed:**
```bash
touch secret1.txt secret2.txt secret3.txt
echo 'TOP SECRET 1!!!' > secret1.txt
```
<img width="557" height="92" alt="Screenshot 2026-08-03 180843" src="https://github.com/user-attachments/assets/f107af61-a5c8-413a-85a5-98c58f8152c6" />


I verified the content of `secret1.txt` to confirm it contained my plaintext data:

**Command executed:**
```bash
cat secret1.txt
```
<img width="300" height="70" alt="Screenshot 2026-08-03 180858" src="https://github.com/user-attachments/assets/7255993b-d229-4c98-b9db-a5f7aac395a7" />


### Encrypted the File
Before encrypting, I created an output directory to store my encrypted file:

**Command executed:**
```bash
mkdir output
```

I then defined a bash variable named `keyArn` and assigned it the ARN value I copied during Task 1. This variable was used to tell the CLI which AWS KMS key to use for encryption.

**Command executed:**
```bash
keyArn=arn:aws:kms:us-west-2:47466334157:key/2f536286-dc00-4a5d-9e43-12bab65c5c31
```


Next, I ran the `aws-encryption-cli` using the `--encrypt` parameter. I specified the input file, wrapped the key using the `keyArn`, set a commitment policy, and directed the output to the `~/output/` directory.

**Command executed:**
```bash
aws-encryption-cli --encrypt \
    --input secret1.txt \
    --wrapping-keys key=$keyArn \
    --metadata-output ~/metadata \
    --encryption-context purpose=test \
    --commitment-policy require-encrypt-require-decrypt \
    --output ~/output/
```
<img width="1902" height="164" alt="Screenshot 2026-08-03 181021" src="https://github.com/user-attachments/assets/339c6e07-c05f-4571-a185-349ecf5a11f3" />


To confirm the encryption command completed successfully, I checked the exit status code; the output of `0` indicated success:

**Command executed:**
```bash
echo $?
```


### Verified the Ciphertext
I moved into the `output` directory to verify that the encrypted file was created:

**Command executed:**
```bash
ls output
```
<img width="743" height="201" alt="Screenshot 2026-08-03 184252" src="https://github.com/user-attachments/assets/a5f2d4ee-49f1-4ed8-80be-1fcac6afe246" />


Using the `cat` command, I viewed the contents of the newly generated `secret1.txt.encrypted`. It consisted of binary garbled data instead of the readable text, confirming that the encryption process successfully transformed the data into ciphertext.

**Command executed:**
```bash
cd output
cat secret1.txt.encrypted
```
<img width="1902" height="145" alt="Screenshot 2026-08-03 184628" src="https://github.com/user-attachments/assets/660a058a-5837-4985-9e82-6261dfc1d65b" />


### Decrypted the File
Finally, I utilized the same CLI tool to decrypt the ciphertext back to plaintext using the `--decrypt` parameter. I pointed the tool to the `secret1.txt.encrypted` file and the correct AWS KMS key ARN:

**Command executed:**
```bash
aws-encryption-cli --decrypt \
    --input secret1.txt.encrypted \
    --wrapping-keys key=$keyArn \
    --commitment-policy require-encrypt-require-decrypt \
    --encryption-context purpose=test \
    --metadata-output ~/metadata \
    --max-encrypted-data-keys 1 \
    --buffer \
    --output .
```
<img width="1901" height="330" alt="Screenshot 2026-08-03 184702" src="https://github.com/user-attachments/assets/472dde08-605e-4e13-9274-2a8d184eb646" />


I verified that the decryption process generated a new file named `secret1.txt.encrypted.decrypted`:

**Command executed:**
```bash
ls
```
<img width="1901" height="164" alt="Screenshot 2026-08-03 184702" src="https://github.com/user-attachments/assets/c673edd7-c5a6-4eda-910b-4201eb0a3fcd" />


I then checked its content to ensure the original text was restored:

**Command executed:**
```bash
cat secret1.txt.encrypted.decrypted
```
<img width="542" height="77" alt="Screenshot 2026-08-03 184729" src="https://github.com/user-attachments/assets/16c8aca3-b439-44dd-a732-82d653a47952" />


## Conclusion
In conclusion, I successfully completed the lab's objectives. I created a secure encryption key in AWS KMS, configured the CLI on an EC2 instance, and demonstrated the complete lifecycle of protecting data: transforming a readable plaintext document (`secret1.txt`) into an unreadable encrypted ciphertext (`secret1.txt.encrypted`), and accurately decrypting it back into its original readable state.
```
