# 📋 Task Manager - 3 Versi Berbeda

## 🎯 Overview

Repository ini berisi **3 versi berbeda** dari Task Manager application dengan level enhancement berbeda:

1. **eTMv1** - Web App Basic (tanpa PWA)
2. **eTMv2** - Progressive Web App (PWA)
3. **eTMv3** - PWA2APK (URL Wrapper untuk Android)

Setiap versi memiliki use case dan karakteristik yang berbeda.

---

## 📁 Struktur Project

```
project/
├── eTMv1/              # Versi 1: Web App Basic
│   ├── index.html
│   ├── dashboard.html
│   ├── icon-*.png
│   └── README.md
│
├── eTMv2/              # Versi 2: PWA
│   ├── index.html
│   ├── dashboard.html
│   ├── manifest.json
│   ├── sw.js
│   ├── icon-*.png
│   └── README.md
│
├── eTMv3/              # Versi 3: PWA2APK (URL Wrapper)
│   ├── index.html
│   ├── manifest.json
│   ├── sw.js
│   ├── icon-*.png
│   └── README.md
│
├── android/            # Capacitor Android project (shared)
├── buku/               # Book drafts & documentation
├── BUILD_APK.md        # Panduan build APK lengkap
├── GOOGLE_APPS_SCRIPT.md  # Setup backend
└── README.md           # Dokumentasi ini
```

---

## 📱 Perbandingan 3 Versi

| Feature | eTMv1 | eTMv2 | eTMv3 |
|---------|-------|-------|-------|
| **Tipe** | Web App | PWA | APK Wrapper |
| **Install ke Home Screen** | ❌ | ✅ | ✅ |
| **Offline Support** | ❌ | ✅ | ⚠️ Tergantung URL* |
| **Service Worker** | ❌ | ✅ | N/A (Load URL) |
| **Native APK** | ❌ | ❌ | ✅ |
| **Distribusi** | URL | URL + Install | APK File |
| **Hosting Required** | ✅ | ✅ | ❌ (APK) ✅ (URL) |
| **Update Method** | Reload | Auto Update | Website Update |
| **Fullscreen** | ❌ | ✅ | ✅ |
| **App Drawer** | ❌ | ✅ (PWA) | ✅ (Native) |
| **Butuh Koneksi Internet** | ✅ | ⚠️ Partial | ✅ Always |

*eTMv3 membutuhkan koneksi internet karena load URL eksternal. Offline support tergantung apakah website target support offline.

---

## 🚀 Kapan Pakai Versi Mana?

### **✨ eTMv1 - Web App Basic**

**Cocok untuk:**
- ✅ Internal tools yang selalu online
- ✅ Desktop-first application
- ✅ Quick prototype atau MVP
- ✅ Shared hosting deployment
- ✅ Learning web development

**Kelebihan:**
- Simple & mudah dipahami
- No build process
- Hosting lebih mudah
- No dependencies

**Kekurangan:**
- Tidak bisa install sebagai app
- Tidak ada offline support
- Harus buka dari browser

**📖 Dokumentasi:** [eTMv1/README.md](eTMv1/README.md)

---

### **📱 eTMv2 - Progressive Web App**

**Cocok untuk:**
- ✅ Mobile-first application
- ✅ Butuh offline functionality
- ✅ Want app-like experience
- ✅ Quick distribution (no app store)
- ✅ Production-ready web app

**Kelebihan:**
- Install ke home screen
- Offline support dengan cache
- Fullscreen (no browser bar)
- Auto update mechanism
- No APK file needed

**Kekurangan:**
- Harus HTTPS untuk production
- Limited device features
- Need modern browser support

**📖 Dokumentasi:** [eTMv2/README.md](eTMv2/README.md)

---

### **📦 eTMv3 - PWA2APK (URL Wrapper)**

**Cocok untuk:**
- ✅ Wrap existing website jadi APK
- ✅ Distribusi internal tanpa Play Store
- ✅ Client minta "file APK"
- ✅ Testing PWA sebagai native app

**Kelebihan:**
- Native Android APK
- Distribusi via file (WhatsApp, email, dll)
- No hosting needed untuk APK
- Fullscreen native experience
- Update website otomatis reflect di APK

**Kekurangan:**
- **Butuh koneksi internet** (load URL dari server)
- Tidak ada offline support (kecuali website target support)
- Butuh rebuild APK untuk ganti icon/name
- APK size ~5-7 MB

**📖 Dokumentasi:** [eTMv3/README.md](eTMv3/README.md)

---

## 🏗️ Arsitektur Keseluruhan

### **Backend (Shared untuk semua versi):**
```
┌─────────────────────┐
│ Google Apps Script  │
│ • validateUser()    │
│ • getTasks()        │
│ • addTask()         │
│ • completeTask()    │
│ • deleteTask()      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Google Sheets      │
│  • Tab: user        │
│  • Tab: db01        │
└─────────────────────┘
```

### **Frontend (3 Varian):**

**eTMv1:** Browser → HTML/CSS/JS → API

**eTMv2:** Browser/PWA → Service Worker → HTML/CSS/JS → API

**eTMv3:** APK → WebView → URL Target (https://mulsim1001.id/tm/)

---

## 🔧 Setup Backend (Untuk eTMv1 & eTMv2)

**Untuk versi yang butuh backend setup:**

### **1. Google Sheets Setup**
Lihat panduan lengkap di: **[GOOGLE_APPS_SCRIPT.md](GOOGLE_APPS_SCRIPT.md)**

### **2. Update API URL**
Edit file HTML:
```javascript
const API_URL = 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec';
```

### **3. Deploy**
- eTMv1: Upload ke hosting HTTP/HTTPS
- eTMv2: Deploy ke HTTPS hosting (Netlify, Vercel, Firebase)
- eTMv3: Build APK (no hosting needed)

---

## 📦 Build APK (eTMv3)

**Quick Guide:**

```bash
# 1. Update target URL di capacitor.config.json
# 2. Sync assets
npx cap sync

# 3. Build APK
cd android
./gradlew assembleDebug

# APK ada di: android/app/build/outputs/apk/debug/app-debug.apk
```

**Panduan lengkap:** [BUILD_APK.md](BUILD_APK.md)

---

## 🔑 Login Credentials

**Default Users (untuk eTMv1 & eTMv2):**
- Username: `mulia` | Password: `m1001`
- Username: `arif` | Password: `a1001`
- Username: `ilham` | Password: `i1001`
- Username: `restu` | Password: `r1001`

**Note:** eTMv3 menggunakan credentials dari website target.

---

## 🎯 Decision Tree

**Mau buat aplikasi web? Pilih versi yang tepat:**

```
START
  │
  ├─ Butuh APK file?
  │   ├─ Ya → eTMv3 (PWA2APK)
  │   └─ Tidak ↓
  │
  ├─ Butuh install ke home screen?
  │   ├─ Ya → eTMv2 (PWA)
  │   └─ Tidak ↓
  │
  └─ Simple web app saja?
      └─ Ya → eTMv1 (Basic)
```

---

## 📚 Dokumentasi Lengkap

- **[eTMv1/README.md](eTMv1/README.md)** - Web App Basic
- **[eTMv2/README.md](eTMv2/README.md)** - Progressive Web App
- **[eTMv3/README.md](eTMv3/README.md)** - PWA2APK Wrapper
- **[BUILD_APK.md](BUILD_APK.md)** - Panduan Build APK
- **[GOOGLE_APPS_SCRIPT.md](GOOGLE_APPS_SCRIPT.md)** - Setup Backend

---

## 🎓 Learning Path

**Recommended progression:**

1. **Mulai dari eTMv1** - Pahami basic web app
   - HTML/CSS/JS fundamentals
   - API integration
   - Session management

2. **Upgrade ke eTMv2** - Tambah PWA features
   - Service worker
   - Cache strategies
   - Web app manifest
   - Install prompt

3. **Build eTMv3** - Wrap ke native APK
   - Capacitor setup
   - Android build process
   - APK distribution

---

## 💻 Tech Stack

### **Frontend:**
- HTML5 - Structure
- CSS3 - Styling & responsive
- Vanilla JavaScript - Logic & interaction

### **Backend:**
- Google Apps Script - Serverless API
- Google Sheets - Database

### **PWA (eTMv2):**
- Service Worker API
- Web App Manifest
- Cache API

### **Native (eTMv3):**
- Capacitor - Native wrapper
- Android WebView
- Gradle - Build system

---

## 🔄 Migration Antar Versi

### **eTMv1 → eTMv2:**
1. Tambah `manifest.json`
2. Tambah `sw.js`
3. Tambah service worker registration
4. Deploy ke HTTPS

### **eTMv2 → eTMv3:**
1. Setup Capacitor
2. Update `capacitor.config.json`
3. Sync dengan `npx cap sync`
4. Build APK

### **eTMv3 → Native App:**
1. Embed HTML dalam app
2. Add native plugins
3. Optimize for mobile

---

## 🐛 Troubleshooting Umum

**Login gagal (eTMv1 & eTMv2):**
→ Check API_URL sudah benar
→ Check Google Apps Script sudah deploy

**Service Worker error (eTMv2):**
→ Harus running di HTTPS (kecuali localhost)
→ Clear cache dan hard refresh

**APK build error (eTMv3):**
→ Check Java version (need 17+)
→ Run `./gradlew clean` di folder android

---

## 📈 Project Status

- ✅ **eTMv1** - Ready to use
- ✅ **eTMv2** - Ready to deploy (need HTTPS)
- ✅ **eTMv3** - Ready to build APK

---

## 🎨 Customization

Semua versi support customization:

### **App Name & Branding:**
- eTMv1: Edit `<title>` di HTML
- eTMv2: Edit `manifest.json` + HTML title
- eTMv3: Edit `capacitor.config.json`

### **Colors & Theme:**
- eTMv1 & eTMv2: Edit CSS variables
- eTMv3: Edit manifest theme_color

### **Icons:**
- Generate di: https://icon.kitchen/
- Replace icon-192.png dan icon-512.png
- eTMv3: Ganti juga di `android/app/src/main/res/`

---

## 📞 Support

**Issues & Questions:**
1. Check dokumentasi masing-masing versi
2. Check troubleshooting section
3. Review console/logs untuk error details

**Resources:**
- PWA: https://web.dev/progressive-web-apps/
- Capacitor: https://capacitorjs.com/
- Google Apps Script: https://developers.google.com/apps-script

---

## 🏆 Best Practices

1. **Start Simple** - Mulai dari eTMv1, lalu upgrade
2. **Test Early** - Test di real device, bukan hanya browser
3. **Security First** - Jangan hardcode credentials
4. **User Experience** - Design for mobile-first
5. **Performance** - Optimize images & assets

---

## 📝 Checklist Development

**Sebelum Deploy:**

- [ ] Test login flow
- [ ] Test CRUD operations
- [ ] Test responsive design (mobile & desktop)
- [ ] Test di different browsers
- [ ] Check console untuk errors
- [ ] Update API_URL dengan production URL
- [ ] Test offline behavior (untuk PWA)
- [ ] Prepare user documentation

**Sebelum Distribusi APK:**

- [ ] Test APK di real Android device
- [ ] Check icon dan app name
- [ ] Test install/uninstall flow
- [ ] Prepare APK distribution channel
- [ ] Create user guide

---

**Built with ❤️**
- 3 Versions, 1 Codebase Philosophy
- Progressive Enhancement Strategy
- Zero to Native in 3 Steps

**"Choose Your Adventure: Web → PWA → Native"** 🚀
