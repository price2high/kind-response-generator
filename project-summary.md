# 🧾 Project Summary – Kind AI Response Generator

The **Kind AI Response Generator** is a serverless web application that uses artificial intelligence to help users rewrite emotionally difficult or harsh messages in a kinder, more emotionally intelligent tone.

This project blends a beautiful frontend experience with a fully serverless AWS backend leveraging modern cloud and AI tools.

---

## 🎯 Objective

To assist users in rephrasing sensitive or challenging messages in a way that’s:
- Emotionally intelligent
- Respectful and professional
- Easy to use and accessible

---

## 🌐 Live Demo

Hosted on:  
Amazon AWS
## 🧠 Key Features

- 💬 AI-powered message rewriting (Amazon Bedrock – Titan Model)
- ✨ Tailwind CSS UI with dark mode toggle
- 💾 Local chat history + export feature
- ☁️ Serverless deployment using AWS S3, API Gateway, and Lambda
- 🔐 CORS-enabled, public API with JSON POST support
- 🧪 Built-in error handling and debugging tools

---

## 🧱 Architecture Overview

Frontend (HTML + JS + Tailwind)  
↓  
API Gateway (POST /generate-reply)  
↓  
Lambda Function (Python)  
↓  
Amazon Bedrock (Titan model)

---
```
Architecture:
  Frontend:
    - Technology: HTML + JS + Tailwind CSS
    - Function: Collect user input
  API Gateway:
    - Method: POST /generate-reply
    - Role: Trigger Lambda function
  Lambda:
    - Language: Python
    - Role: Process input and call Bedrock
  Bedrock:
    - Model: amazon.titan-text-lite-v1
    - Role: Return kind rephrased message
```

---

## 🧱 Tech Stack

| Component     | Technology                          |
|---------------|--------------------------------------|
| Frontend      | HTML + Tailwind CSS + Vanilla JS     |
| Backend       | Python (AWS Lambda)                  |
| AI Model      | Amazon Bedrock Titan Lite            |
| API Gateway   | REST API                             |
| Hosting       | AWS S3 Static Website Hosting        |
| DevOps        | GitHub Actions (S3 Deploy Workflow)  |

---

## 📁 Key Files

| File                 | Purpose                                     |
|----------------------|---------------------------------------------|
| `index.html`         | Full frontend with UI + logic               |
| `lambda_function.py` | AI message rewriter using Bedrock           |
| `README.md`          | Project overview                            |
| `HOSTING.md`         | Step-by-step AWS hosting instructions       |
| `STYLING.md`         | Frontend design + dark mode implementation  |
| `TROUBLESHOOTING.md` | Known issues + fixes                        |
| `PROJECT_SUMMARY.md` | This summary file                           |

---

## 🏁 Status

✅ MVP completed  
✅ Hosted on S3  
✅ Functional API integration  
🛠️ Optional: Add custom domain + CloudFront

---

## 🙌 Contributors

- **Tip Price** – Product Owner / Developer / Cloud Architect

---

## 📄 License

MIT License  
© 2025 KindReply & Tip Price
