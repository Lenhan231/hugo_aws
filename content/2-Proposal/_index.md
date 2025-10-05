---
title: "Proposal"

weight: 2
chapter: false
pre: " <b> 2. </b> "
---



# KenFi – AI Fitness Commerce & Membership Platform
## A Unified AWS Serverless Stack for Membership, E-Commerce, and AI Fitness Coaching

### 1. Executive Summary
KenFi is an AI-powered Fitness Commerce and Membership Platform developed as a prototype for a local gym facility, with a scalable architecture designed to support future expansion into a full multi-branch fitness chain. The platform consolidates three traditionally fragmented operations—product retail, membership registration, and customer assistance—into a single digital ecosystem.

Through an integrated e-commerce module, customers can purchase fitness products such as whey protein, supplements, apparel, and accessories directly from the platform. Membership plans—including monthly, quarterly, and annual subscriptions—can be acquired online, upon which the system automatically generates a unique Customer ID to be presented to gym reception staff for check-in and access control.

A key differentiator of KenFi is its AI-powered chatbot assistant, designed with a dual-tone personality: approachable and conversational during general interactions, yet precise and professional when providing fitness or nutrition guidance. Phase 1 introduces a calorie and nutrition calculator powered by the USDA FoodData Central API, enabling users to track dietary intake and estimate caloric expenditure based on physical activity levels.

The platform is hosted on AWS using a fully serverless architecture to minimize operational overhead and ensure long-term scalability. Secure online payment processing is supported through sandbox integrations with VNPay and PayPal, allowing seamless transition to production gateways upon deployment in a live environment.

KenFi aims to streamline internal gym operations, reduce manual registration processes, and deliver a modern, data-driven experience for both gym owners and members. While initially deployed for a single facility, the system is technically architected to scale into a centralized platform capable of serving multiple locations under a unified brand.

### 2. Problem Statement
### What’s the Problem?
Most local gyms in Vietnam still rely heavily on manual processes. Memberships are often tracked on paper or basic Excel files. Customers must come directly to the counter to register or renew, causing delays and crowding during peak hours. There is no self-service option for purchasing membership or supplements online. Customer identity is not standardized—each branch or staff member may use different formats, leading to confusion and inconsistent records. Additionally, nutrition and calorie consultation is usually informal, depending on whichever trainer is available, resulting in inconsistent advice.

### The Expected Digital Experience Customers Want:
Modern gym members expect the same level of convenience they receive from e-commerce or online banking. They want to subscribe to a training package directly on their phones, pay instantly using common payment gateways such as VNPay or PayPal, and receive a membership QR code or Customer ID immediately. They want clear guidance on calorie intake and supplement usage without needing to constantly ask the front desk. They also expect the system to recognize them instantly at check-in without repeating personal information.

### The Gap in the Market:
While large fitness chains in Vietnam such as California Fitness or CityGym have partial digital systems, there is no unified, affordable platform for small-to-medium gyms to operate with the same level of professionalism. Buying ready-made systems from foreign vendors is expensive and not optimized for Vietnamese workflows. Most “gym management” software products currently available focus only on attendance tracking, without integrating e-commerce, nutrition guidance, or AI-based support.

### KenFi as the Solution:
KenFi bridges this gap by offering an AWS-powered fitness commerce and membership platform tailored specifically for local gyms. It centralizes membership registration, digital payments, supplement sales, nutrition calculation, and customer identity into a single friendly ecosystem. Customers receive a unique Customer ID or QR Code immediately after purchasing a package, which can be shown to reception staff for instant recognition. A built-in AI chatbot acts as a friendly gym bro for casual questions, but switches to serious expert mode when discussing technical fitness topics. This platform enables small gyms to operate with the professionalism of major fitness chains without requiring heavy upfront infrastructure or maintenance.

### 3. Solution Architecture
The platform employs a serverless AWS architecture to manage data from 5 Raspberry Pi-based stations, scalable to 15. Data is ingested via AWS IoT Core, stored in an S3 data lake, and processed by AWS Glue Crawlers and ETL jobs to transform and load it into another S3 bucket for analysis. Lambda and API Gateway handle additional processing, while Amplify with Next.js hosts the dashboard, secured by Cognito. The architecture is detailed below:

![IoT Weather Station Architecture](/images/2-Proposal/edge_architecture.jpeg)

![IoT Weather Platform Architecture](/images/2-Proposal/platform_architecture.jpeg)

### AWS Services Used
- **AWS IoT Core**: Ingests MQTT data from 5 stations, scalable to 15.
- **AWS Lambda**: Processes data and triggers Glue jobs (two functions).
- **Amazon API Gateway**: Facilitates web app communication.
- **Amazon S3**: Stores raw data in a data lake and processed outputs (two buckets).
- **AWS Glue**: Crawlers catalog data, and ETL jobs transform and load it.
- **AWS Amplify**: Hosts the Next.js web interface.
- **Amazon Cognito**: Secures access for lab users.

### Component Design
- **Edge Devices**: Raspberry Pi collects and filters sensor data, sending it to IoT Core.
- **Data Ingestion**: AWS IoT Core receives MQTT messages from the edge devices.
- **Data Storage**: Raw data is stored in an S3 data lake; processed data is stored in another S3 bucket.
- **Data Processing**: AWS Glue Crawlers catalog the data, and ETL jobs transform it for analysis.
- **Web Interface**: AWS Amplify hosts a Next.js app for real-time dashboards and analytics.
- **User Management**: Amazon Cognito manages user access, allowing up to 5 active accounts.

### 4. Technical Implementation

**Development Phases**

The KenFi platform will be implemented in clearly defined stages to ensure rapid prototyping while maintaining long-term scalability.

Phase 1 – Architecture Planning and AWS Service Setup. Define the exact AWS resources required including Amplify, Cognito User Pool, API Gateway, Lambda functions, DynamoDB tables, and S3 buckets. Establish naming conventions, region selection, and IAM roles.

Phase 2 – Core Membership Workflow. Implement the end-to-end flow of registering an account, purchasing a training package, generating Customer ID and QR code, and storing membership validity in DynamoDB.

Phase 3 – Supplement Commerce Module. Build product catalog, add-to-cart flow, checkout via VNPay or PayPal sandbox, and order history tracking.

Phase 4 – Nutrition Calculator Integration. Connect Lambda functions to the USDA FoodData Central API, allowing users to input food items and receive calorie and macro breakdowns.

Phase 5 – AI Chatbot. Integrate a lightweight LLM model running on Lambda or optionally through Amazon Bedrock. Configure behavior switching between casual “gym bro” mode and serious expert mode.

**AWS Services and Key Responsibilities**

AWS Amplify hosts the Next.js frontend and automates CI/CD from GitHub or CodeCommit.

Amazon Cognito manages sign-up, login, password resets, and identity tokens.

Amazon API Gateway exposes secure HTTPS endpoints for all application actions.

AWS Lambda functions handle business logic such as membership purchase, QR code generation, and nutrition lookup.

Amazon DynamoDB stores customer profiles, membership tokens, transaction records, and product inventory.

Amazon S3 stores public assets such as QR images, banner images, and downloadable membership cards.

Optional Amazon SES can be used to send confirmation emails with QR code attachments.

**QR Code Generation Process**

When a purchase is confirmed, Lambda generates a unique membership string using a prefix such as KF-YYYYMM-XXXX. A QR code is rendered via a QR generation library, temporarily stored in memory, and saved as an image file in S3 under a path such as /qr-codes/customer-id.png. The URL is returned to the frontend and optionally emailed to the customer.


**Payment Gateway Connection**

VNPay and PayPal sandbox accounts are used during development. The frontend redirects the user to the appropriate payment URL. Upon completion, VNPay or PayPal returns a callback to API Gateway with transaction status. A dedicated Lambda function verifies the payment signature and marks the membership as active.

**Deployment Strategy**

The entire infrastructure can be provisioned manually through the AWS Console during the prototype stage. Once stable, infrastructure as code will be applied using AWS CDK or CloudFormation for reproducible deployments across environments. Versioning for Lambdas and rollback strategies will be enforced.

**Testing and Quality Control**

Unit testing is done on each Lambda function independently. Frontend integration is verified through Amplify preview builds. Full end-to-end testing is simulated with a dummy user registering, purchasing, receiving QR code, and checking in. Penetration testing focuses on access control and payment spoofing prevention.


### 5. Timeline & Milestones
**Project Timeline**
- Pre-Internship (Month 0): 1 month for planning and old station review.
- Internship (Months 1-3): 3 months.
    - Month 1: Study AWS and upgrade hardware.
    - Month 2: Design and adjust architecture.
    - Month 3: Implement, test, and launch.
- Post-Launch: Up to 1 year for research.

### 6. Budget Estimation
You can find the budget estimation on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01).  
Or you can download the [Budget Estimation File](../attachments/budget_estimation.pdf).

### Infrastructure Costs
- AWS Services:
    - AWS Amplify (Web Hosting and CI/CD Pipeline): approximately 0.30 to 0.50 USD per month under light traffic.
    - Amazon Cognito (User Authentication and Identity Management): free for up to 50,000 monthly active users.
    - Amazon API Gateway (HTTPS API Invocation Layer): typically 0.05 to 0.10 USD per month for under 10,000 calls.
    - AWS Lambda (Business Logic Execution): near zero cost due to generous free tier (1 million requests per month).
    - Amazon DynamoDB (Membership, Transaction, and Product Data Storage): approximately 0.03 to 0.05 USD per month for minimal read/write activity.
    - Amazon S3 (Static Asset Storage for QR Codes and Product Images): between 0.05 and 0.10 USD per month depending on image volume.

Total: ≈ $0.7/month, $8.40/12 months

- Hardware: $265 one-time (Raspberry Pi 5 and sensors).

### 7. Risk Assessment
#### Risk Matrix
- Network Outages: Medium impact, medium probability.
- Sensor Failures: High impact, low probability.
- Cost Overruns: Medium impact, low probability.

#### Mitigation Strategies
- Network: Local storage on Raspberry Pi with Docker.
- Sensors: Regular checks and spares.
- Cost: AWS budget alerts and optimization.

#### Contingency Plans
- Revert to manual methods if AWS fails.
- Use CloudFormation for cost-related rollbacks.

### 8. Expected Outcomes
#### Technical Improvements: 
Real-time data and analytics replace manual processes.  
Scalable to 10-15 Store.
#### Long-term Value
1-year data foundation for AI research.  
Reusable for future projects.