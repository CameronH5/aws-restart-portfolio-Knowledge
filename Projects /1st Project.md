
# Project 1 - Praesignis AWS re/Start: Salt & Pepper Restaurant
### Section 3.2: AWS Services Presentation & Migration Analysis

This document outlines the business profile, operational challenges, technical solution, and strategic benefits of migrating **Salt & Pepper** to the AWS cloud.

---

### 3.2.1. Company Overview (Detailed Profile)

**Business Name:** Salt & Pepper Restaurant
**Location:** 9 Yusuf Arafat Crescent, Khayelitsha, Cape Town, 7784
**Established:** 2016
**Core Offering:** A beloved local eatery serving hearty, affordable meals. Their menu blends traditional South African favorites (like hearty stews, pap, and grilled meats) with modern casual dining options like burgers, wraps, and freshly prepared salads. 
**Customer Base:** A loyal, tight-knit community of local residents, families, nearby retail workers, and students from the surrounding area. 

**Operational Status & Volume:**
Salt & Pepper is a staple in the Khayelitsha community. It is exceptionally busy during weekday lunch hours, after work (4 PM – 9PM), and particularly on weekends when families come out to eat. 

**The Current Manual System:**
Currently, Salt & Pepper runs on a fragile manual workflow:
1.  **Booking**: The only way to secure a table is via phone call or a WhatsApp message, which is written down on a shared paper diary by whichever staff member is closest to the phone.
2.  **Takeaway Orders**: Customers call in or send a WhatsApp. Staff frantically scribble these onto paper slips, which are then handed to the kitchen. 
3.  **Inventory & Payments**: There is no central digital record connecting a phone order to the kitchen's cooking progress or to the payment terminal at the front counter.

---

### 3.2.2. Restaurant Challenges (Deep Dive Analysis)

While the restaurant is loved by the community, the lack of digital infrastructure is creating severe friction, threatening customer satisfaction and staff morale. Here is a deep-dive into their current struggles:

**Challenge 1: Constant Order Mix-ups & Customer Frustration**
- **The Problem**: With over a lot of takeaway orders arriving via phone and WhatsApp daily, handwriting errors are incredibly common. A customer ordering a "Chicken Stew" might receive a "Beef Stew" because the paper slip was illegible. 
- **Community Impact**: In a close-knit neighborhood like Khayelitsha, word of mouth is everything. Repeated order mistakes cause frustrated customers to take their business to competitors, and the owner cannot afford to lose regulars.

**Challenge 2: The "Paper-Diary" Booking Nightmare**
- **The Problem**: The restaurant has limited seating, and peak weekends are heavily oversubscribed. The current paper diary frequently has pages ripped out, erased, or overlapping entries. This leads to embarrassing double-bookings where two families arrive at the exact same time for the only remaining table.
- **Staff Distraction**: Staff are constantly answering phone calls mid-service, neglecting the paying customers sitting right in front of them, leading to slow service and a chaotic atmosphere.

**Challenge 3: Zero Visibility into Customer Data and Trends**
- **The Problem**: Because every transaction is a paper slip, the owner has absolutely no data to work with. They don't know which dish is their best-seller, which days of the week are their absolute busiest, or who their repeat customers are. 
- **Lost Revenue**: When a customer calls to say, *"I brought my family here last Saturday and ordered the large platter—can I get the same one again this weekend?"*, the staff have no way to check the record. They cannot build loyalty programs, send birthday discounts, or track dietary preferences.

**Challenge 4: Inability to Scale for Community Events**
- **The Problem**: Khayelitsha hosts numerous community events, celebrations, and soccer match days. During these surges, the phone could ring off the hook. To handle the load, the owner faces an impossible choice: either refuse orders and lose money, or spend a huge amount of money hiring additional temporary staff just to answer phones—which they can't afford on a small business budget.

---

### 3.2.3. Recommended AWS Services (Proposed Technical Solution)

To solve these challenges without breaking the bank, we will deploy a **Serverless AWS Architecture**. This allows Salt & Pepper to pay *only pennies per order*, and requires zero technical maintenance from the owner.

**1. Front-End Hosting & Content Distribution**
- **Amazon S3**: Hosts the fully static website (HTML/CSS/JS). This will display a clean, user-friendly menu, a Table Booking form, and a Takeaway Order form.
- **Amazon CloudFront**: Speeds up website loading for all customers in Khayelitsha and the broader Cape Town area. It also provides a free SSL certificate, ensuring customers' personal information (like phone numbers) is sent securely.

**2. Back-End Logic & Processing**
- **Amazon API Gateway**: Provides a secure, public URL that receives the data submitted from the website's forms.
- **AWS Lambda**: Serverless code that processes the incoming form data. The Lambda function validates the booking time (ensuring no double-bookings), formats the data, and securely writes it into the database.

**3. Data Storage & Management**
- **Amazon DynamoDB**: A high-performance NoSQL database. We use this instead of a traditional SQL database because it scales automatically and costs nothing when it isn't being used. It will store three distinct tables:
  - `Bookings` (Date, Time, Party Size, Contact Number)
  - `Orders` (Customer name, Dish selected, Pickup time, Status - "Cooking/Ready")
  - `CustomerProfiles` (Phone number, address, favorite dishes)

**4. User Authentication**
- **Amazon Cognito**: Allows repeat customers to create a simple password-protected profile. When they log in, their phone number and address are automatically populated into the order form. This saves time and improves the user experience.

**5. Automated Notifications & Communications**
- **Amazon Simple Notification Service (SNS)**: Triggers an **instant SMS message** to the restaurant owner's and kitchen staff's phones whenever a new takeaway order or booking is submitted online. No more checking a computer screen—they are alerted immediately.
- **Amazon Simple Email Service (SES)**: Automatically sends a polite confirmation email (or SMS via SNS if they don't have email) to the customer, letting them know their order/booking was received and confirmed.

---

### 3.2.4. Benefits: Why Salt & Pepper Should Choose AWS

**Benefit 1: Eradicating Manual Errors and Double-Bookings**
- Digital forms capture every order perfectly without handwriting errors. DynamoDB automatically checks the calendar database to ensure a table isn't booked twice. The chaos of paper slips is gone, and customers get exactly what they ordered.

**Benefit 2: Extremely Low Cost (AWS Free Tier)**
- This is crucial for a small local business in Khayelitsha. The proposed serverless architecture costs roughly **$15–$20 per month** to run. 
- Even better: The AWS Free Tier covers **1 million Lambda requests, 25GB of S3 storage, and 50,000 monthly active Cognito users** for the first 12 months. Salt & 
- AWS provides different limits than the ones quoted:Lambda: You get 1 million free requests per month. You also get 400,000 GB-seconds of compute time per month. These limits never expire.
- S3: You get 5 GB of standard storage, not 25GB. You also get 20,000 Get requests and 2,000 Put requests per month. This also never expires.
- Cognito: The always-free tier provides 10,000 Monthly Active Users (MAUs) (under the Essentials plan), not 50,000
  Pepper's actual monthly bill could be **less than $5** for the entire first year.

**Benefit 3: Automatic Scalability for Peak Times**
- When a massive community event happens in Khayelitsha and orders flood in, AWS Lambda and DynamoDB handle the traffic automatically. They can process 500 orders simultaneously without needing any new staff to answer phones or any expensive new hardware. The owner focuses on cooking, not answering calls.

**Benefit 4: Data-Driven Menu Optimization**
- By storing every order in DynamoDB, the owner can finally see exactly what sells best. They can remove slow-moving dishes to reduce food waste, stock up on their best-sellers, and identify their top 10 loyal customers to give them special discounts—cultivating a community-driven loyalty program.

**Benefit 5: Massive Staff Relief and Efficiency**
- By automating the order-taking and notification process, the staff no longer have to be glued to the phone during lunch rush. They can focus entirely on serving the dine-in guests, cleaning tables, and delivering amazing hospitality, which improves both customer satisfaction and staff happiness.

---

