# AWS SimuLearn: Computing Solutions 

## Overview

**AWS SimuLearn: Computing Solutions** was a scenario-based learning assignment within the **AWS SimuLearn: Cloud Practitioner** learning path on AWS Skill Builder. It formed one of 12 hands-on modules in the Cloud Practitioner curriculum.

AWS SimuLearn itself was an online learning experience launched by Amazon Web Services that paired **generative AI-powered simulations** with hands-on practice in a live AWS Console environment. The platform used AI, powered by Amazon Bedrock, to enable life-like conversations with virtual customers. Learners engaged in dynamic AI-powered virtual meetings to understand business needs and translate them into technical solutions. The experience then transitioned into a guided lab where learners built and validated the proposed solution using real AWS tools.

## The Computing Solutions Assignment

### Simulated Business Scenario

The "Computing Solutions" assignment placed learners in a **simulated business scenario** involving a **school server** that ran a class scheduling solution. The server was experiencing performance issues and **needed more memory**. In some variations of the scenario, the EC2 instance was consuming excessive CPU and memory, causing slowdowns across classroom and study room bookings.

### The Learning Process

The assignment followed a structured workflow:

1. **AI-Powered Customer Conversation**: Learners first interacted with a simulated AI customer through an interactive chat interface. They assessed workload complexity, understood future needs (such as online student course registration), and identified blockers without over-engineering the solution.

2. **Architecture Design**: Learners drafted an architecture proposal and received constructive feedback. Their responses to the AI were evaluated to develop both technical and soft skills like communication and problem-solving.

3. **Hands-On Lab in Live AWS Console**: After completing the design phase, learners moved to a **live AWS Console environment** to build and validate the proposed solution. The lab was designated as **Lab #13** within the AWS Skill Builder platform.

### Technical Activities Performed

Throughout the hands-on lab, learners:

- **Explored and filtered Amazon EC2 instance types** based on compute and memory attributes
- **Learned about different EC2 instance families and types**, including `t3.large`, `c5.large`, and `r5.large`
- **Compared various methods for connecting to EC2 instances**, including EC2 Instance Connect, AWS Systems Manager Session Manager, and SSH clients
- **Practiced operational fundamentals**: starting, stopping, and resizing EC2 instances via the console
- **Retrieved instance metadata** via public IP and the Instance Metadata Service (IMDS)
- **Successfully resized an instance**—for example, from `t3.micro` to `m4.large` to improve performance

### Key Concepts Covered

The assignment reinforced core cloud computing concepts, including:

- **Vertical scaling** (scaling up) as the primary solution approach
- **Horizontal scaling** (scaling out) as a complementary concept
- **Right-sizing**—selecting EC2 instance types that better matched the workload

## Outcome and Significance

The "Computing Solutions" assignment was **one of the most practical ways** for learners to build muscle memory with AWS services rather than just reading about them. It demonstrated that **effective cloud solutions started with good conversations, not just configurations**. The key lesson emphasized that effective cloud architecture was not always about complexity—it was about **right-sizing, security, and aligning solutions to actual business needs**.


<img width="1165" height="815" alt="image" src="https://github.com/user-attachments/assets/c2373008-17d5-4573-b431-03eef27db99a" />
