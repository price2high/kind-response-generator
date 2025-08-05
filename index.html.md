# 💻 Kind AI Response Generator – `index.html` Source Code

This is the full HTML + JavaScript file for the frontend of the Kind AI project. It contains the Tailwind-styled UI, API integration logic, chat history handling, dark mode toggle, and export functionality.

---

## 🔗 Hosted on AWS S3 as a static website  
## 🌐 Connected to AWS API Gateway and Lambda

---

## 🧾 Full Code: `index.html`

```html
<!DOCTYPE html>
<html lang="en" class="">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Kind AI Reply 💜</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
    }
    .chat-bubble {
      border-radius: 20px;
      padding: 12px 16px;
      max-width: 75%;
      margin: 10px 0;
      word-wrap: break-word;
    }
  </style>
</head>
<body class="flex items-center justify-center min-h-screen p-6 transition-colors duration-300 bg-[#f5ebfa] dark:bg-gray-900" id="mainBody">
  <div class="w-full max-w-xl bg-white dark:bg-gray-900 rounded-3xl shadow-xl p-6 border border-purple-200 dark:border-gray-700 text-gray-900 dark:text-gray-100">
    <h1 class="text-3xl font-bold text-purple-700 dark:text-purple-300 text-center mb-6">💌 Kind AI Response Generator</h1>

    <div id="chatBox" class="h-80 overflow-y-auto mb-4 p-4 bg-purple-50 dark:bg-gray-800 rounded-lg border border-purple-200 dark:border-gray-700">
      <!-- Chat history will load here -->
    </div>

    <form id="chatForm" class="flex flex-col gap-3">
      <textarea id="userInput" class="resize-none border border-purple-300 dark:border-gray-600 dark:bg-gray-700 dark:text-white rounded-xl p-3 focus:outline-none focus:ring-2 focus:ring-purple-400" rows="3" placeholder="Type your difficult message here..."></textarea>
      <div class="flex justify-between items-center gap-3 flex-wrap">
        <button type="submit" class="bg-purple-500 hover:bg-purple-600 text-white font-semibold py-2 px-4 rounded-xl transition w-full sm:w-auto">
          Get Kind Response 🌷
        </button>
        <button type="button" id="downloadBtn" class="w-32 bg-white dark:bg-gray-700 text-purple-600 dark:text-purple-300 border border-purple-300 dark:border-purple-500 hover:bg-purple-50 dark:hover:bg-gray-600 font-semibold py-2 px-4 rounded-xl transition">
          Download 💾
        </button>
        <button type="button" id="toggleTheme" class="w-32 bg-purple-100 dark:bg-purple-900 text-purple-700 dark:text-purple-300 border border-purple-300 dark:border-purple-600 font-semibold py-2 px-4 rounded-xl transition">
          Toggle 🌙/☀️
        </button>
      </div>
    </form>
  </div>

  <script>
    const chatBox = document.getElementById('chatBox');
    const chatForm = document.getElementById('chatForm');
    const userInput = document.getElementById('userInput');
    const API_URL = "https://wi9rlg9ba5.execute-api.us-east-1.amazonaws.com/prod/generate-reply";

    function appendMessage(text, sender) {
      const div = document.createElement('div');
      div.className = `chat-bubble ${sender === 'user' ? 'bg-purple-200 dark:bg-purple-700 self-end text-right ml-auto' : 'bg-purple-100 dark:bg-gray-600 text-left mr-auto'} text-purple-900 dark:text-white`;
      div.textContent = text;
      chatBox.appendChild(div);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function saveToHistory(message, sender) {
      const history = JSON.parse(localStorage.getItem("chatHistory") || "[]");
      history.push({ message, sender });
      localStorage.setItem("chatHistory", JSON.stringify(history));
    }

    function loadHistory() {
      const history = JSON.parse(localStorage.getItem("chatHistory") || "[]");
      history.forEach(entry => {
        appendMessage(entry.message, entry.sender);
      });
    }

    chatForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const message = userInput.value.trim();
      if (!message) return;

      appendMessage(message, 'user');
      saveToHistory(message, 'user');
      userInput.value = '';

      try {
        const response = await fetch(API_URL, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ message })
        });

        const data = await response.json();
        const reply = data.response || "Oops! No response received.";
        appendMessage(reply, 'bot');
        saveToHistory(reply, 'bot');

      } catch (error) {
        console.error('Error:', error);
        const fallback = "Something went wrong. Try again soon.";
        appendMessage(fallback, 'bot');
        saveToHistory(fallback, 'bot');
      }
    });

    // Download chat history
    document.getElementById('downloadBtn').addEventListener('click', () => {
      const history = JSON.parse(localStorage.getItem("chatHistory") || "[]");
      if (history.length === 0) {
        alert("No chat history to export.");
        return;
      }

      const content = history.map(entry => `${entry.sender.toUpperCase()}: ${entry.message}`).join('\n\n');
      const blob = new Blob([content], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.download = 'KindAI_Chat_History.txt';
      link.click();
      URL.revokeObjectURL(url);
    });

    // Theme toggle logic
    const toggleTheme = document.getElementById('toggleTheme');
    const savedTheme = localStorage.getItem('kindTheme');

    if (savedTheme === 'dark') {
      document.documentElement.classList.add('dark');
    }

    toggleTheme.addEventListener('click', () => {
      document.documentElement.classList.toggle('dark');
      const newTheme = document.documentElement.classList.contains('dark') ? 'dark' : 'light';
      localStorage.setItem('kindTheme', newTheme);
    });

    window.onload = loadHistory;
  </script>
</body>
</html>
