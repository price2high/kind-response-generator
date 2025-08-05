# ☁️ Hosting the Kind AI App on AWS

This document explains how to deploy the **Kind AI Response Generator** frontend as a static website using **Amazon S3**, and optionally use a **custom domain** via **Route 53** + **CloudFront**.

---

## 🪄 What We're Hosting

We're deploying a single `index.html` file that:
- Contains a Tailwind-styled UI
- Sends POST requests to an API Gateway `/generate-reply` endpoint
- Shows AI responses from a Lambda → Bedrock integration

---

## 🗂 Step 1: Upload to S3 Bucket

1. Go to the **S3 Console**
2. Click **Create bucket**
   - Name it: `kindreply.com` (or similar)
   - Region: `us-east-1` (recommended for Bedrock)
   - **Uncheck** “Block all public access”
3. After creating, go to the bucket → **Properties**
   - Enable **Static website hosting**
   - Index document: `index.html`
4. Upload your `index.html` file

---

## 🔐 Step 2: Make Site Public

Go to the **Permissions** tab of your bucket and do the following:

### ✅ A. Disable “Block all public access”
- Click Edit → Uncheck the setting → Confirm

### ✅ B. Add this Bucket Policy

> Replace `your-bucket-name` with your actual S3 bucket name.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
