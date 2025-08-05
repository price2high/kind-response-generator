# 🤖 AI Agent Build Guide – Kind AI Response Generator (AWS)

This guide walks you through building and deploying a polite-response AI agent using Amazon Bedrock, Lambda, and API Gateway.

---

## ✅ STEP 1: Define Your AI Agent's Role

**Purpose**:  
Help users respond nicely to difficult or emotionally charged messages.

**Input**:  
Plain text message from the user

**Output**:  
AI-generated polite and emotionally intelligent response

---

## ✅ STEP 2: Enable Access to Amazon Bedrock

1. Log in to the **AWS Console**
2. Go to **Amazon Bedrock**
3. In the sidebar, click **Build → Agents**
4. Click **Create agent**
5. Select a model and click **Request access**
   - You may need to apply for access to specific models
   - **Note**: If Claude is unavailable, use Amazon Titan (pre-approved)

> ⚠️ Access approval may take a few minutes

---

## ✅ STEP 3: Create the Backend (Lambda + API Gateway)

### 🔹 A. Create Lambda Function

- Go to **AWS Lambda → Create function**
- Choose:
  - Author from scratch
  - Name: `AIResponseGenerator`
  - Runtime: Python 3.12
  - Role: Create a new role with basic Lambda permissions
- Click **Create function**

---

### 🔹 B. Add Bedrock Permissions to Lambda

- Go to **IAM → Roles**
- Select your Lambda's execution role
- Click **Add permissions → Attach policies**
- Attach `AmazonBedrockFullAccess`

---

### 🔹 C. Paste Lambda Code

Paste this code into your Lambda function:

```python
import json
import boto3

bedrock_runtime = boto3.client("bedrock-runtime", region_name="us-east-1")

def lambda_handler(event, context):
    body = json.loads(event["body"])
    user_input = body.get("message", "")

    prompt = f"""Respond to this message in a kind and emotionally intelligent way:

Message: "{user_input}"

Response:"""

    response = bedrock_runtime.invoke_model(
        modelId="anthropic.claude-3-sonnet-20240229-v1:0",
        contentType="application/json",
        accept="application/json",
        body=json.dumps({
            "prompt": prompt,
            "max_tokens": 300,
            "temperature": 0.7
        })
    )

    result = json.loads(response["body"].read())
    reply = result.get("completion", "Sorry, something went wrong.")

    return {
        "statusCode": 200,
        "body": json.dumps({"response": reply}),
        "headers": {
            "Content-Type": "application/json"
        }
    }
```

> 🔄 Update the `modelId` if you're using Mistral or Titan instead of Claude.

---

## ✅ STEP 4: Connect Lambda to API Gateway

1. Go to **API Gateway → Create API**
2. Choose:
   - HTTP API
   - Integration: select your `AIResponseGenerator` Lambda
3. Click **Next → Create**
4. Go to **Routes → Add route**:
   - Method: `POST`
   - Path: `/generate-reply`
5. Deploy your API and copy the **Invoke URL**  
   (e.g., `https://xyz123.execute-api.us-east-1.amazonaws.com/generate-reply`)

---

## ✅ STEP 5: Test the Agent

Use `curl` or Postman:

```bash
curl -X POST https://xyz123.execute-api.us-east-1.amazonaws.com/generate-reply \
-H "Content-Type: application/json" \
-d '{"message": "My coworker keeps taking credit for my work."}'
```

**Example Response:**

```json
{
  "response": "It’s frustrating when others take credit for your efforts. A calm response could be: 'I’ve noticed my contributions haven’t been acknowledged in some recent discussions, and I’d appreciate being included when we talk about our shared work.'"
}
```

---

## ✅ STEP 6: Secure It

### 🔐 API Key Auth (Simple)

1. In API Gateway → Create an **API Key**
2. Attach a **usage plan** to your route
3. Require API key header in requests:

```http
x-api-key: YOUR_API_KEY
```

---

### 🔒 Advanced (Optional)

- Use **Amazon Cognito** for user auth
- Enable CloudWatch logs:
  - In Lambda settings
  - In API Gateway access logs

---

## ✅ STEP 7: Optional – Build a Chat UI

### 🧑‍💻 Static Web UI

1. Build an HTML or React frontend
2. Use `fetch()` or `axios.post()` to hit your API
3. Display the AI response on the page

### ☁️ Deploy the UI

- Upload to **S3** (enable static hosting)
- Optionally connect **CloudFront** for HTTPS + performance

---

🎉 Done! You now have a fully serverless AI-powered polite responder.
