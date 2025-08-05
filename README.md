# 🤖 Kind AI Response Generator

Kind AI is a serverless web app that helps rewrite difficult messages into kind and emotionally intelligent responses using Amazon Bedrock. It features a clean Tailwind-powered UI, is hosted on AWS S3, and leverages API Gateway and Lambda for backend logic.

---

## 💡 Features

- 💬 Kind response generation via Amazon Bedrock Titan
- 🌗 Dark mode toggle + responsive Tailwind styling
- 💾 Chat history stored locally in browser (with download option)
- ☁️ Frontend hosted on AWS S3 with static website hosting
- 🔁 Backend powered by Lambda + API Gateway

---

## 🛠 Tech Stack

| Layer       | Technology                   |
|-------------|-------------------------------|
| Frontend    | HTML + Tailwind CSS + JS      |
| Backend     | AWS Lambda (Python)           |
| AI Model    | Amazon Bedrock (Titan Lite)   |
| API Gateway | REST API with POST endpoint   |
| Hosting     | AWS S3 (Static Website)       |
