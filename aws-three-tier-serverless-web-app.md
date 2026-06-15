# Building My Serverless Three-Tier Web App on AWS

![AWS Three-Tier Web App Architecture](https://github.com/user-attachments/assets/19124971-0ad0-4e0c-ae5b-452db36a1968)

---

## Architecture Overview

In this project, I designed and deployed a fully functional cloud-native web application by separating the system into three distinct, independently scalable tiers. This decoupled approach taught me how to build systems where each layer can evolve and scale without impacting the others:

1. **Presentation Tier (Frontend):** I built a static website using HTML, CSS, and JavaScript, then hosted it securely on **Amazon S3**. To ensure my users experience low-latency access regardless of their geographic location, I placed **Amazon CloudFront** in front of it as a global Content Delivery Network (CDN).

2. **Logic Tier (Backend API):** Rather than managing any servers myself, I implemented my backend logic as serverless microservices using **AWS Lambda**, written in Python. I then exposed these functions to the web through clean REST endpoints using **Amazon API Gateway**.

3. **Data Tier (Database):** I chose **Amazon DynamoDB** as my persistence layer — a fully managed, serverless NoSQL database that stores and retrieves my application data as flexible JSON-like items.

---

## AWS Services I Used

| Service | How I Used It |
|---|---|
| **Amazon S3** | Hosted my static web assets: `index.html`, `style.css`, and `script.js` |
| **Amazon CloudFront** | Configured as a CDN to cache and serve my frontend globally from edge locations |
| **Origin Access Control (OAC)** | Secured my S3 bucket so it is completely private and only reachable via CloudFront |
| **Amazon API Gateway** | Routed incoming HTTP requests from the browser to my Lambda backend |
| **AWS Lambda** | Executed my backend Python logic on-demand without any server management |
| **Amazon DynamoDB** | Stored my application entities as schema-less JSON-like items |
| **AWS IAM** | Enforced fine-grained, least-privilege permissions between every tier |

---

## My Setup & Deployment Process

### 1. Presentation Tier — Hosting the Frontend

I started by creating a **private S3 bucket** and uploading my three frontend files: `index.html`, `style.css`, and `script.js`. The key decision here was to keep the bucket entirely private — no public access whatsoever.

Next, I set up an **Amazon CloudFront distribution** pointing to my S3 bucket as its origin. To enforce strict access control, I implemented **Origin Access Control (OAC)** and updated the S3 bucket policy to grant read permissions *exclusively* to my CloudFront distribution. This means users can only reach my files through CloudFront — never by hitting S3 directly.

### 2. Data Tier — Setting Up DynamoDB

For the data layer, I created a **DynamoDB table** and defined a `Partition Key` (using the attribute `id`) to uniquely identify each item. Once the table was provisioned, I seeded it with sample records so I had realistic data to query when testing my backend logic.

### 3. Logic Tier — Building the Serverless Backend

With the data in place, I wrote a **Lambda function in Python** to handle querying the DynamoDB table. I was careful to create and attach a dedicated **IAM execution role** to this function, following the principle of least privilege — the role policy only allowed the Lambda to perform `Scan` and `GetItem` operations on my specific DynamoDB table, nothing more.

After the compute layer was ready, I moved to **API Gateway**:

- I created a new **REST API** and defined a resource endpoint.
- I added a `GET` method and integrated it with my Lambda function.
- I enabled **CORS** on the resource — a critical step to allow my CloudFront-hosted frontend to make cross-origin requests to the API without being blocked by the browser.
- Finally, I deployed the API to a **`prod` stage**, which gave me the public **Invoke URL** I needed to connect the frontend to the backend.

### 4. Integration — Wiring Everything Together

With the backend deployed, I updated my local `script.js` file by replacing the placeholder `const API_URL` with my actual API Gateway Invoke URL. I then re-uploaded the modified `script.js` to my S3 bucket.

---

## The Problem I Ran Into: CloudFront Caching Stale Files

### What Happened

After re-uploading my updated files, I accessed the application through my **CloudFront domain name** and immediately opened the browser Developer Tools (`F12`). The console was showing multiple `Cannot read properties of null` errors. The frontend and backend were not communicating at all.

At first, I suspected a JavaScript bug — maybe a DOM element wasn't found, or IDs were mismatched between my HTML and JS. But as I dug deeper, I realized something more fundamental was wrong: **the browser was executing an old version of my `script.js`** that didn't match my current `index.html`.

The reason this happened was subtle: I had previously used the same S3 bucket for a different project, and I had uploaded the new files **without changing their filenames**. CloudFront had no way of knowing the files had changed — it simply continued to serve the old cached versions it had already stored at its global edge locations.

### Why This Happens

CloudFront is designed to cache content for performance. When a user requests a file, CloudFront serves it from the nearest **edge location** rather than fetching it from the S3 origin every time. By default, CloudFront can cache files for 24 hours or longer. Updating a file in S3 doesn't automatically invalidate what CloudFront has already cached — those edge copies remain until they naturally expire.

### How I Fixed It — CloudFront Invalidation

To solve this immediately, I ran a **CloudFront Invalidation**:

1. I opened the **CloudFront console** in AWS and navigated to my distribution.
2. I clicked on the **Invalidations** tab and selected **Create invalidation**.
3. I entered `/*` as the object path to invalidate all cached files at once.
4. I submitted the request and waited for it to complete.

As soon as the invalidation finished, CloudFront purged its cached copies and fetched the fresh files directly from my S3 bucket. The errors disappeared and the web app worked perfectly — the frontend successfully fetched data from DynamoDB through API Gateway and Lambda.

### Long-Term Best Practice: File Versioning

I also learned that the proper long-term solution is **content versioning** — instead of overwriting `script.js` with each update, I should rename it (e.g., `script.v2.js` or `script.a1b2c3d4.js`) and reference the new name in `index.html`. Because CloudFront sees a brand-new filename, it treats it as a fresh file and fetches it from S3 immediately — no invalidation required. This approach also allows me to set very long cache durations on versioned assets, making my site faster for end users.

---

## Testing My Application

Once everything was wired up and the cache was cleared, I validated the full stack:

1. I accessed the app via my **CloudFront Domain Name** (`https://dxxxxxxxxxx.cloudfront.net`).
2. I opened **Developer Tools (`F12`)** and confirmed there were no CORS errors, no mixed-content warnings, and no null reference exceptions in the console.
3. I clicked the interactive buttons on the frontend and watched the app dynamically fetch real data from DynamoDB — passing through CloudFront → S3 (for the frontend) and API Gateway → Lambda → DynamoDB (for the data fetch).

---

## Key Learnings

**Decoupled Architecture**
By separating the presentation, logic, and data tiers, I gained hands-on experience building systems that are independently deployable and scalable. A change in the frontend doesn't require touching the backend, and vice versa.

**Serverless Operations**
I deployed a complete, production-like backend entirely without provisioning or managing any virtual machines. Lambda and DynamoDB handled all the scaling concerns automatically.

**Cloud Security**
I configured IAM policies with least-privilege access, enabled CORS policies at the API Gateway level, and secured my S3 origin using CloudFront OAC. These layers of security work together to ensure no resource is unnecessarily exposed to the public internet.

**CloudFront Caching Behavior**
Perhaps my most practical takeaway: understanding *how* CDN caching works and how to manage it. I now know the difference between waiting for a cache to naturally expire versus proactively invalidating it, and when to use file versioning as a more sustainable deployment strategy.

---

> **Stack:** Amazon S3 · CloudFront · API Gateway · AWS Lambda (Python) · DynamoDB · IAM · OAC
