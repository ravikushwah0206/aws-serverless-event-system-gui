# aws-serverless-event-system-gui

# AWS Serverless Event Announcement System (Step-by-Step GUI Guide)

📌 Project Overview
This repository document acts as a deployment guide for building a serverless, event-driven notification microservice directly inside the AWS Management Console using the Graphical User Interface (GUI). 

This project connects **Amazon API Gateway**, **AWS Lambda**, and **Amazon SNS** to build a public web link that automatically fires off alerts to subscribers. 

Building this system demonstrates a strong understanding of AWS service integration, cloud security (IAM) permissions, and infrastructure troubleshooting skills required for **Cloud Support** and Operations engineering.

---

🛠️ Step-by-Step AWS Console Walkthrough

📥 Step 1: Set Up the Amazon SNS Notification Broker
1. Log into the **AWS Management Console**.
2. Search for **SNS (Simple Notification Service)** in the top search bar and open it.
3. Click **Topics** on the left menu, then click the orange **Create topic** button.
4. Select **Standard** as the type.
5. Set the **Name** to: `event-announcements-topic`
6. Set the **Display name** to: `Event Alerts`
7. Scroll to the bottom and click **Create topic**.
8. **Copy the ARN string** (e.g., `arn:aws:sns:us-east-1:123456789012:event-announcements-topic`) to your notepad. You will need this later.

---

⚙️ Step 2: Provision the AWS Lambda Compute Backend
1. Search for **Lambda** in the top AWS search bar and click it.
2. Click the orange **Create function** button and choose **Author from scratch**.
3. Configure the following basic settings:
   - **Function name:** `PublishEventAnnouncement`
   - **Runtime:** `Python 3.11` (or the latest version)
4. Click **Create function**.
5. Inside the function screen, go to the **Configuration** tab -> click **Environment variables** on the left menu -> click **Edit**.
6. Click **Add environment variable**:
   - **Key:** `SNS_TOPIC_ARN`
   - **Value:** Paste the *SNS ARN* you saved during Step 1.
7. Click **Save**.

---

🔒 Step 3: Configure Security Access via AWS IAM
*By default, cloud services are locked down and cannot talk to each other. We must grant the Lambda function permission to push notifications to SNS.*

1. Inside your Lambda function screen, click the **Configuration** tab.
2. Click **Permissions** on the left menu.
3. Under **Execution role**, click on the blue **Role name** link. This will automatically open the IAM Security console in a new tab.
4. In the IAM screen, locate the **Add permissions** button dropdown and click **Attach policies**.
5. In the search box, type `AmazonSNSFullAccess`.
6. Check the box next to `AmazonSNSFullAccess` and click the blue **Add permissions** button at the bottom. 
7. Close this tab to return to your Lambda page.

---

🌐 Step 4: Create the Amazon API Gateway Public Web Entryway
1. Search for **API Gateway** in the top AWS search bar and open it.
2. Scroll down to **REST API** (ensure it does *not* say Private) and click **Build**.
3. Choose **New API** and set the **API name** to: `EventAnnouncementAPI`
4. Click **Create API**.
5. In the left panel, click **Resources**. Then click **Create resource**.
6. Set the **Resource name** to `announce` and click **Create resource**.
7. Click on your new `/announce` resource path on the screen, then click **Create method**.
8. Set **Method type** to `POST`.
9. Set **Integration type** to `Lambda function`.
10. Turn ON the toggle for **Lambda proxy integration**.
11. In the Lambda function search bar, select your function: `PublishEventAnnouncement`.
12. Click **Create method** (and click **OK** if AWS asks for permission to link them).

---

🚀 Step 5: Deploy the Microservice System
1. While still inside your API Gateway interface, click the orange **Deploy API** button at the top right.
2. Under **Stage**, select `*New Stage*`.
3. Set the **Stage name** to `prod` and click **Deploy**.
4. The system will display an **Invoke URL** at the top of the screen.

> 🎯 **Final Active URL:** Add your resource name to the end of the link to get your working endpoint:
> `https://your-api-id.execute-api.us-east-1.amazonaws.com/prod/announce`

---

🔒 Cloud Engineering Support Skills Highlighted
- **IAM Boundary Security:** Demonstrated implementation of cloud policies using standard managed rules (`AmazonSNSFullAccess`) to bridge disconnected platform layers safely.
- **Stateless Serverless Routing:** Leveraged AWS native managed proxies to process inbound HTTP traffic without maintaining or paying for physical server hardware.
- **Environment Variable Abstraction:** Kept architecture modular by decoupling platform resource strings (ARNs) away from basic logical components.
