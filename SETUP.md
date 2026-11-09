# Pixel Topup — Complete Project Setup Guide (সম্পূর্ণ প্রজেক্ট সেটআপ গাইড)

> **DEVELOP BY https://t.me/Infinity_Codex**

এই প্রজেক্টটি **Pixel Topup** ওয়েবসাইটের সম্পূর্ণ ইউজার প্যানেল (`user.html`), এডমিন প্যানেল (`admin.html`), ডাটাবেস (`database.json`), এবং ফায়ারবেস সিকিউরিটি রুলস (`rules.json`) নিয়ে গঠিত। নিচে ধাপে ধাপে সম্পূর্ণ সেটআপ করার নিয়ম দেওয়া হলো।

## ১. প্রজেক্টের ফাইলসমূহ (Project Files)

| ফাইলের নাম | কাজ ও বিবরণ |
| :--- | :--- |
| `user.html` | মেইন ইউজার ওয়েবসাইট (Storefront, Topup Order, Add Money, Order History, Account ও Built-in Admin Workspace) |
| `admin.html` | স্ট্যান্ডঅ্যালোন এডমিন কন্ট্রোল প্যানেল (Dashboard, Orders, Deposits, Categories, Games/Services, Packages, Stock/Voucher Codes, Users, Banners, Settings) |
| `database.json` | ফায়ারবেস রিয়েলটাইম ডাটাবেসের রেডিমেড ডাটা (গেমস, ডায়মন্ড প্যাকেজ, ক্যাটাগরি, ব্যানার ও সেটিংস) |
| `rules.json` | ফায়ারবেস রিয়েলটাইম ডাটাবেসের সিকিউরিটি রুলস |
| `SETUP.md` | প্রজেক্ট সেটআপ ও কনফিগারেশন গাইড |

## ২. Firebase প্রজেক্ট তৈরি ও কনফিগারেশন (Firebase Setup)

### ধাপ ১: Firebase Project তৈরি করুন
1. [Firebase Console](https://console.firebase.google.com/)-এ যান এবং **Create a project** / **Add project**-এ ক্লিক করুন।
2. প্রজেক্টের নাম দিন (যেমন: `Pixel Topup`) এবং প্রজেক্ট তৈরি সম্পন্ন করুন।
3. প্রজেক্ট ড্যাশবোর্ডে গিয়ে **Web (`</>`)** আইকনে ক্লিক করে একটি ওয়েব অ্যাপ রেজিস্টার করুন এবং আপনার `firebaseConfig` কোডটি কপি করে রাখুন।

### ধাপ ২: Authentication চালু করুন
1. বাম পাশের মেনু থেকে **Build → Authentication → Get started**-এ যান।
2. **Sign-in method** ট্যাবে গিয়ে নিচের দুটি মেথড **Enable** করে **Save** করুন:
   - **Email/Password** (ইউজার এবং এডমিন লগইনের জন্য আবশ্যক)
   - **Google** (গুগল ওয়ান-ক্লিক লগইনের জন্য আবশ্যক)
3. **Authentication → Settings → Authorized domains**-এ গিয়ে আপনার ওয়েবসাইটের ডোমেইন (যেখানে সাইট হোস্ট করবেন) যোগ করুন।

### ধাপ ৩: Realtime Database ও `database.json` ইমপোর্ট করুন
1. বাম পাশের মেনু থেকে **Build → Realtime Database → Create Database**-এ ক্লিক করুন।
2. যেকোনো লোকেশন সিলেক্ট করে **Start in test mode** দিয়ে ডাটাবেস চালু করুন।
3. ডাটাবেস তৈরি হলে **Data** ট্যাবে ডান পাশের তিন ডট (`⋮`) মেনুতে ক্লিক করে **Import JSON** সিলেক্ট করুন।
4. এই প্রজেক্টের ফোল্ডারে থাকা **`database.json`** ফাইলটি আপলোড করে **Import** করুন।
   - *বিকল্প পদ্ধতি:* আপনি চাইলে এডমিন প্যানেলে লগইন করে সেটিংস থেকে **Import Default Seed Data** বাটনে ক্লিক করেও এক ক্লিকে ডিফল্ট ডাটা ইমপোর্ট করতে পারবেন।

### ধাপ ৪: Database Rules (`rules.json`) সেটআপ করুন
1. **Realtime Database → Rules** ট্যাবে যান।
2. সেখানে নিচের রুলসটি (`rules.json` ফাইলের কোড) পেস্ট করে **Publish** বাটনে ক্লিক করুন:

```json
{
  "rules": {
    ".read": true,
    ".write": "auth != null"
  }
}
```

## ৩. HTML ফাইলে Firebase Config বসানোর নিয়ম

`user.html` এবং `admin.html` — উভয় ফাইলের একদম শুরুতে `<head>` সেকশনের ভেতরে `window.PIXEL_FIREBASE_CONFIG` খুঁজে বের করুন এবং আপনার নিজের Firebase Web App-এর তথ্য বসিয়ে দিন:

```javascript
window.PIXEL_FIREBASE_CONFIG = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.firebasestorage.app",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID",
    measurementId: "YOUR_MEASUREMENT_ID"
};
```

## ৪. এডমিন প্যানেল লগইন ও পাসওয়ার্ড পরিবর্তন (Admin Access)

`database.json` ইমপোর্ট করার পর ডিফল্ট এডমিন লগইন তথ্য নিচে দেওয়া হলো:

- **Default Admin Email:** `admin@gmail.com`
- **Default Admin Password:** `admin@123`

### কীভাবে লগইন করবেন:
1. সরাসরি `admin.html` ওপেন করে উপরের ইমেইল ও পাসওয়ার্ড দিয়ে লগইন করতে পারবেন।
2. অথবা `user.html`-এর লগইন পেজে এডমিন ইমেইল ও পাসওয়ার্ড দিলে স্বয়ংক্রিয়ভাবে **Pixel Admin** প্যানেলে নিয়ে যাবে।
3. লগইন করার পর **Settings → Admin Security & Password Change** কার্ড থেকে আপনার নিজের ইমেইল এবং নতুন পাসওয়ার্ড সেট করে **Update Admin Login Credentials**-এ ক্লিক করুন।

## ৫. পেমেন্ট নাম্বার ও API সেটিংস (Payment & API Setup)

এডমিন প্যানেলে লগইন করে **Settings** মেনু থেকে আপনি সবকিছু রিয়েল-টাইমে কন্ট্রোল করতে পারবেন:
- **General Info:** সাইটের নাম (`Pixel Topup`), লোগো লিংক, স্ক্রলিং নোটিশ, এবং অ্যাপ ডাউনলোড লিংক।
- **Social Links:** WhatsApp নাম্বার, Telegram সাপোর্ট লিংক, Facebook পেজ এবং YouTube চ্যানেল লিংক।
- **Payment Numbers:** আপনার bKash, Nagad এবং Rocket নাম্বার পরিবর্তন করুন।
- **Advanced Config:**
  - **Player Name Check API URL:** Free Fire UID দিলে স্বয়ংক্রিয়ভাবে প্লেয়ারের নাম চেক করার API লিংক।
  - **FF Auto Like API URL:** FF Like অর্ডারের অটোমেটিক ডেলিভারি API লিংক।

## ৬. হোস্টিং এবং ডিপ্লয়মেন্ট (Hosting & Deployment)

এই প্রজেক্টটি সম্পূর্ণ স্ট্যাটিক HTML/JS হওয়ায় যেকোনো হোস্টিংয়ে খুব সহজেই রান করবে:
- **cPanel / Shared Hosting:** `user.html` ফাইলটির নাম পরিবর্তন করে `index.html` রাখতে পারেন এবং `admin.html` একই ফোল্ডারে (`public_html`) আপলোড করুন।
- **Firebase Hosting / Netlify / Vercel / GitHub Pages:** প্রজেক্ট ফোল্ডারটি সরাসরি ডিপ্লয় করুন (`user.html`-কে মেইন পেজ বা `index.html` হিসেবে ব্যবহার করতে পারেন)।

**DEVELOP BY https://t.me/Infinity_Codex**
