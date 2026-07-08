
## Lab Analysis: Introduction to an Amazon Linux AMI

### **SSH Connection to EC2 Instance**
I connected to an Amazon Linux EC2 instance using Secure Shell (SSH). Depending on your operating system:

- **If Windows used**, I:
  - Selected the "Details" dropdown menu to access credentials
  - Downloaded the `labsuser.ppk` file (PPK format for PuTTY)
  - Noted the PublicIP address of the instance
  - Downloaded PuTTY SSH client (if not already installed)
  - Opened `putty.exe`
  - Configured your PuTTY session to connect to the Linux instance using the downloaded private key

- **If macOS or Linux used**, must follow different SSH connection instructions.

### **Explored the Linux Manual Pages**
Once connected to the EC2 instance, I:
- Opened a bash terminal in a PuTTY session
- Executed the command `man man` to access the Linux standard help system
- Learned about the manual pages (man pages) — the built-in help documentation for Linux commands
- Explored the following objectives:
  - Understood the purpose and function of the `man` command
  - Demonstrated the search feature within man pages
  - Examined man page headers and structure

### **Key Learning Outcomes**
Through this lab, I gained foundational knowledge in:
- **SSH connectivity** — securely accessing remote Linux instances on AWS
- **Linux help system** — navigating and using man pages for command documentation
- **CLI fundamentals** — working within a bash terminal environment on Amazon Linux

# Lab Exercise: Exploring the Linux Man Pages

## Overview
This document details the practical exercise completed in the AWS Restart Linux introduction lab, specifically focusing on exploring and understanding the Linux manual pages (man pages) system.

## Exercise Objective
To become proficient in navigating the Linux standard help system (man pages) and understanding how to locate command documentation and usage information.

## Tasks Completed

### Exercise - Explore the Linux Man Pages

#### Important Man Page Headers
I documented the following key man page headers (standard across Linux systems):

- **NAME** — The name of the command and a brief one-line description
- **SYNOPSIS** — The syntax for using the command, showing required and optional arguments
- **DESCRIPTION** — A detailed explanation of what the command does and how it works
- **OVERVIEW** — A general overview of the command's functionality
- **EXAMPLES** — Practical examples demonstrating how to use the command
- **FILES** — A list of files that the command uses or affects
- **OPTIONS** — Detailed descriptions of all available flags and options
- **SEE ALSO** — Cross-references to related commands and documentation

**Key Learning**: Understanding these headers allows you to quickly navigate man pages and find exactly what you need without reading the entire documentation.

There is particular attention to the **DESCRIPTION** header.

The DESCRIPTION section provides:
- An overview of what the command does
- How the command operates
- General usage context and behavior


#### Step 25: Exiting Man Pages
Lastly I learned the command to exit the man page viewer:

```bash
q

