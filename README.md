# Pro Portfolio Manager

Personal portfolio tracking application - PWA supported, mobile responsive.

## 🚀 Setup

1. Go to the Settings page
2. Enter the following information:

| Field | Description |
|-------|-------------|
| **Supabase URL** | `https://YOUR_PROJECT.supabase.co` |
| **Supabase Key** | Anon/Public key (Dashboard > Settings > API) |
| **OpenAI API Key** | (Optional) For AI analysis |

3. Click "Save & Refresh"

**Quick Setup via URL:**
You can auto-configure by adding parameters to the URL:
```
https://yourdomain.com/?sburl=YOUR_URL&sbkey=YOUR_KEY&aikey=YOUR_AI_KEY
```

## 📱 Features

- ✅ PWA - Add to home screen
- ✅ Mobile responsive design
- ✅ iPhone notch/safe-area support
- ✅ Swipe gesture navigation
- ✅ Live price tracking (Supabase)
- ✅ AI portfolio analysis (OpenAI)
- ✅ Excel/CSV export

## 🔒 Security

This repository contains **NO hardcoded API keys**.  
All API credentials are entered by the user in Settings and stored in the browser's localStorage.

## 📁 File Structure

```
portfolio/
├── index.html      # Main application (single file)
├── manifest.json   # PWA manifest
├── sw.js           # Service Worker
├── icon-192.png    # PWA icon
├── icon-512.png    # PWA icon
└── README.md       # This file
```

## 📝 License

Private - All rights reserved.
