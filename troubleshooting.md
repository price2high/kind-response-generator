# 🛠️ TROUBLESHOOTING – Kind AI Response Generator

This document outlines known issues and solutions encountered while deploying and running the Kind AI Response Generator. It covers frontend, API, and AWS infrastructure components.

---

## 🔍 Table of Contents

- [S3 Hosting Issues](#s3-hosting-issues)
- [403 and 404 Errors](#403-and-404-errors)
- [API Gateway or Lambda Not Responding](#api-gateway-or-lambda-not-responding)
- [CORS Errors](#cors-errors)
- [Frontend Not Showing AI Response](#frontend-not-showing-ai-response)
- [Chat UI Not Loading Properly](#chat-ui-not-loading-properly)

---

## 🪣 S3 Hosting Issues

### ❌ Problem: `NoSuchKey` or 404 Not Found

**Cause**: `index.html` is missing or incorrectly named in the bucket root.

**Fix**:
- Upload your `index.html` to the **root** of your S3 bucket.
- File name must be **exactly** `index.html` (case-sensitive).

---

## 🚫 403 Forbidden

### ❌ Problem: Access denied when visiting the S3 site

**Fix**:
1. Go to your bucket → **Permissions**
2. Click **Edit block public access**
3. Disable **“Block all public access”**
4. Add this bucket policy:

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
