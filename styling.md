# 🎨 Styling & Frontend Implementation

The UI is built with Tailwind CSS using CDN, styled with soft purples and a polished, clean layout.

---

## 🧱 Components

- **Chat Box** with scrollable message history
- **User/Bot Chat Bubbles** aligned with `self-end` and `self-start`
- **Textarea** input with ring focus
- **Buttons**:
  - Get Kind Response
  - Download Chat 💾
  - Toggle 🌙/☀️ mode

---

## 🌗 Dark Mode

Dark mode is toggled by adding/removing `dark` class on `<html>`, and stored in `localStorage`.

```js
const toggleTheme = document.getElementById('toggleTheme');
toggleTheme.addEventListener('click', () => {
  document.documentElement.classList.toggle('dark');
  localStorage.setItem('kindTheme', document.documentElement.classList.contains('dark') ? 'dark' : 'light');
});
