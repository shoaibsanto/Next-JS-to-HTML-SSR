<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://github.com/user-attachments/assets/0aa67016-6eaf-458a-adb2-6e31a0763ed6" />
</div>

# Run and deploy your AI Studio app

This contains everything you need to run your app locally.

View your app in AI Studio: https://ai.studio/apps/a2425cef-b002-490e-ac1d-61546a785545

## Run Locally

**Prerequisites:**  Node.js


1. Install dependencies:
   `npm install`
2. Set the `GEMINI_API_KEY` in [.env.local](.env.local) to your Gemini API key
3. Run the app:
   `npm run dev`

---

## ⚡ Next.js ওয়েবসাইটটিকে Static (HTML/CSS/JS) হিসেবে Export করার নিয়ম

পরবর্তীতে যেকোনো সময় এই প্রজেক্টটিকে স্ট্যাটিক ওয়েবসাইট হিসেবে এক্সপোর্ট করতে চাইলে নিচের তিনটি সহজ ধাপ অনুসরণ করুন:

### ধাপ ১: `next.config.ts` কনফিগারেশন চেক
নিশ্চিত করুন যে প্রজেক্টের `next.config.ts` ফাইলে নিচের সেটিংস দুটি দেওয়া আছে:
- `output: 'export'` (স্ট্যাটিক ফাইল এক্সপোর্ট চালু করার জন্য)
- `images: { unoptimized: true, ... }` (Next.js এর সার্ভারভিত্তিক ইমেজ অপটিমাইজেশন বন্ধ করার জন্য)

### ধাপ ২: Build কমান্ড রান করা
টার্মিনাল বা কমান্ড প্রম্পট ওপেন করে নিচের কমান্ডটি রান করুন:
```bash
npm run build
```

### ধাপ ৩: এক্সপোর্ট করা ফাইলগুলো দেখা এবং হোস্টিং
- বিল্ড শেষ হলে প্রজেক্টে **`out`** নামের একটি নতুন ফোল্ডার তৈরি হবে। এই ফোল্ডারেই আপনার সব স্ট্যাটিক ফাইল (HTML, CSS, JS) জমা হবে।
- **গুরুত্বপূর্ণ:** সরাসরি ডাবল-ক্লিক করে `index.html` ওপেন করলে ডিজাইন ভেঙে যেতে পারে (Next.js এর স্ট্রাকচারের কারণে)। 
- আপনার পিসিতে (লোকালি) এটি ঠিকমতো কাজ করছে কিনা তা দেখতে টার্মিনালে কমান্ড দিন:
  ```bash
  npx serve out
  ```
- কমান্ডটি রান করার পর যে লিংকটি (যেমন `http://localhost:3000`) পাবেন, সেটি ব্রাউজারে প্রবেশ করালেই ওয়েবসাইটটি সুন্দরভাবে দেখতে পারবেন।
- ওয়েবসাইটটি লাইভ করতে চাইলে এই `out` ফোল্ডারটির ভেতরের সব ফাইল যেকোনো হোস্টিং (যেমন cPanel, GitHub Pages, Netlify বা Hostinger)-এ সরাসরি আপলোড করে দিন।

---

## 🐍 Django টেমপ্লেটে এই স্ট্যাটিক সাইট ইন্টিগ্রেট করার নিয়ম

স্ট্যাটিক এক্সপোর্ট করা (`out` ফোল্ডারের) ফাইলগুলো দিয়ে যদি আপনি জ্যাঙ্গো অ্যাপ বানাতে চান, তবে নিচের ধাপগুলো অনুসরণ করুন:

**ধাপ ১:** আপনার জ্যাঙ্গো প্রজেক্টের অ্যাপ ডিরেক্টরিতে `templates` এবং `static` ফোল্ডার তৈরি করুন।
**ধাপ ২:** `out` ফোল্ডারের ভেতরের সব `.html` ফাইল জ্যাঙ্গোর `templates` ফোল্ডারে (যেমন `myapp/templates/myapp/`) কপি করুন।
**ধাপ ৩:** `out` ফোল্ডারের ভেতরের `_next` ফোল্ডার এবং অন্যান্য এসেটগুলো জ্যাঙ্গোর `static` ফোল্ডারে (যেমন `myapp/static/myapp/`) কপি করুন।
**ধাপ ৪:** সবচেয়ে গুরুত্বপূর্ণ কাজ হলো HTML ফাইলগুলোতে জ্যাঙ্গোর স্ট্যাটিক ট্যাগ অ্যাড করা:
  - `index.html` এর একদম উপরে `{% load static %}` যোগ করুন।
  - এরপর ফাইলের ভেতরের সব `/_next/static/...` লিংকগুলোকে `{% static 'myapp/_next/static/...' %}` দিয়ে রিপ্লেস করে দিন।
     - *আগে:* `<link href="/_next/static/css/style.css">`
     - *পরে:* `<link href="{% static 'myapp/_next/static/css/style.css' %}">`
**ধাপ ৫:** সবশেষে `views.py` তে ফাংশন লিখে এবং `urls.py` তে রাউট কনফিগার করে পেজটি রেন্ডার করুন।
