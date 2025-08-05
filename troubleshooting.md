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
```

> Replace `your-bucket-name` with your actual S3 bucket name.

---

## 🛑 API Gateway or Lambda Not Responding

### ❌ Problem: Frontend returns `"Something went wrong. Try again soon."`

**Cause**: API Gateway route isn't deployed, or Lambda is throwing an error.

**Fix**:
- Ensure your API Gateway has a `POST /generate-reply` route.
- Confirm the route is connected to your Lambda function.
- Deploy the API: **Actions → Deploy API**.
- Use API Gateway's **Test** tab to simulate a POST request.
- Check **CloudWatch Logs** for Lambda errors.

---

## 🚫 CORS Errors

### ❌ Problem: API response is blocked in browser

**Cause**: API Gateway or Lambda isn't returning `Access-Control-Allow-Origin`.

**Fix**:
- In Lambda, add this header to all responses:

```python
"headers": {
  "Content-Type": "application/json",
  "Access-Control-Allow-Origin": "*"
}
```

- In API Gateway:
  - Enable CORS for the route
  - Add `Access-Control-Allow-Origin` in Integration Response headers
  - Redeploy your API

---

## 🤖 Frontend Not Showing AI Response

### ❌ Problem: Response is blank or shows default error

**Cause**: Bedrock model response not parsed correctly.

**Fix**:
- In Lambda, decode and parse the Bedrock response:

```python
result = json.loads(response['body'].read().decode('utf-8'))
reply = result.get("results", [{}])[0].get("outputText", "")
```

- Make sure your Lambda returns:

```json
{ "response": "Your kind message here" }
```

---

## 🎨 Chat UI Not Loading Properly

### ❌ Problem: Tailwind styles not appearing

**Fix**:
- Confirm Tailwind is loaded via CDN in `index.html`:

```html
<script src="https://cdn.tailwindcss.com"></script>
```

- Clear browser cache or try Incognito mode
- Make sure file extension is `.html`, not `.txt`

---

## ✅ Deployment Recap

| Component         | Status |
|------------------|--------|
| `index.html` uploaded to S3 bucket root | ✅ |
| Static website hosting enabled | ✅ |
| Public read policy on S3 | ✅ |
| API Gateway POST method exists | ✅ |
| Lambda returns JSON with `response` | ✅ |
| Lambda has CORS headers | ✅ |
| API is deployed to a stage | ✅ |
| HTML uses full `API_URL` | ✅ |

---

🧪 Use browser DevTools → Network tab to verify request/response.

🎉 Once resolved, your app will be fully live and functional.
