# 📱 eTM v2.0 - Task Manager PWA (Progressive Web App)

## 🎯 Deskripsi

Versi PWA (Progressive Web App) dari Task Manager - aplikasi web yang **bisa diinstall seperti app native** di mobile dan desktop.

**Karakteristik:**
- ✅ Bisa diinstall sebagai app
- ✅ Offline support (cache HTML)
- ✅ Icon di home screen
- ✅ Fullscreen (no browser bar)
- ✅ Auto update
- ✅ Push notification ready

---

## 📁 File yang Ada

```
eTMv2/
├── index.html         # Halaman login (dengan PWA support)
├── dashboard.html     # Dashboard task management
├── manifest.json      # PWA manifest (app config)
├── sw.js              # Service worker (offline & cache)
├── icon-192.png       # App icon 192x192
├── icon-512.png       # App icon 512x512
└── README.md          # Dokumentasi ini
```

---

## 🚀 Cara Menggunakan

### **1. Setup Backend (Google Apps Script)**
Ikuti panduan di file `GOOGLE_APPS_SCRIPT.md` di root project untuk:
- Setup Google Sheets dengan tab `user` dan `db01`
- Deploy Google Apps Script
- Dapatkan URL endpoint API

### **2. Update API URL**
Edit kedua file HTML dan update URL API:

**File: `index.html` (baris ~137)**
```javascript
const API_URL = 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec';
```

**File: `dashboard.html` (baris ~261)**
```javascript
const API_URL = 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec';
```

### **3. Jalankan dengan HTTPS (Wajib untuk PWA!)**

PWA **harus** running di HTTPS (kecuali localhost).

#### **Opsi A: Local Testing (HTTP OK)**
```bash
npx http-server . -p 8080
```
Buka: `http://localhost:8080/`

#### **Opsi B: Deploy ke Hosting (HTTPS)**
Deploy ke Netlify, Vercel, atau Firebase Hosting (otomatis HTTPS)

#### **Opsi C: ngrok (HTTPS Tunnel)**
```bash
npx http-server . -p 8080
ngrok http 8080
```
Gunakan URL HTTPS dari ngrok

---

## 📱 Cara Install PWA

### **Android (Chrome):**
1. Buka website di Chrome
2. Tap menu (⋮) → **"Add to Home screen"** atau **"Install app"**
3. Tap **"Install"**
4. App muncul di home screen dan app drawer

### **iOS (Safari):**
1. Buka website di Safari
2. Tap Share button (kotak dengan panah ke atas)
3. Tap **"Add to Home Screen"**
4. Tap **"Add"**
5. App muncul di home screen

### **Desktop (Chrome/Edge):**
1. Buka website di Chrome/Edge
2. Klik icon Install (➕) di address bar
3. Atau: Menu (⋮) → **"Install [App Name]"**
4. App muncul sebagai standalone window

---

## 🔑 Login

**Default Users:**
- Username: `mulia` | Password: `m1001`
- Username: `arif` | Password: `a1001`
- Username: `ilham` | Password: `i1001`
- Username: `restu` | Password: `r1001`

---

## ⚡ Fitur

### **Core Features:**
1. **Login System** - Autentikasi via Google Sheets
2. **Add Task** - Tambah task baru dengan PIC otomatis
3. **View Tasks** - Lihat semua task dengan sorting
4. **Complete Task** - Tandai task selesai dengan catatan
5. **Delete Task** - Hapus task dengan konfirmasi
6. **Auto Overdue Check** - Task > 24 jam otomatis status "overdue"
7. **Auto Refresh** - Dashboard refresh setiap 30 detik
8. **Responsive Design** - Desktop (tabel) & Mobile (card layout)

### **PWA Features:**
- ✅ **Installable** - Install ke home screen
- ✅ **Offline Cache** - HTML & assets cached
- ✅ **Service Worker** - Background sync & updates
- ✅ **App Icon** - Custom icon di home screen
- ✅ **Splash Screen** - Loading screen saat buka app
- ✅ **Fullscreen Mode** - No browser UI
- ✅ **Auto Update** - Service worker auto update cache

---

## 📊 Arsitektur PWA

```
┌─────────────────────────┐
│   Browser/PWA Shell     │
│   • index.html          │
│   • dashboard.html      │
│   • manifest.json       │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│   Service Worker        │
│   • Cache Management    │
│   • Offline Support     │
│   • Background Sync     │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  Google Apps Script     │
│  (Backend API)          │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  Google Sheets          │
│  (Database)             │
└─────────────────────────┘
```

---

## 🔧 Konfigurasi PWA

### **manifest.json**
```json
{
  "name": "Task Manager - Minimalis",
  "short_name": "Task Manager",
  "description": "Aplikasi Task Manager minimalis",
  "start_url": "/index.html",
  "display": "standalone",
  "background_color": "#667eea",
  "theme_color": "#667eea",
  "orientation": "portrait-primary",
  "icons": [...]
}
```

**Customization:**
- `name`: Nama lengkap app
- `short_name`: Nama pendek (di bawah icon)
- `theme_color`: Warna status bar
- `background_color`: Warna splash screen
- `display`: `standalone` (fullscreen) atau `browser`

### **Service Worker (sw.js)**

**Cache Strategy:** Cache First → Network Fallback

```javascript
const CACHE_NAME = 'task-manager-v1';
const urlsToCache = [
  '/index.html',
  '/dashboard.html'
];
```

**Update Cache:** Ubah `CACHE_NAME` untuk force update cache.

---

## 💻 Teknologi

- **HTML5** - Struktur & semantic markup
- **CSS3** - Styling & responsive layout
- **Vanilla JavaScript** - Logic & PWA APIs
- **Service Worker API** - Offline & cache management
- **Web App Manifest** - PWA configuration
- **Google Apps Script** - Backend API
- **Google Sheets** - Database

---

## 🌐 Deployment (Harus HTTPS!)

### **1. Netlify (Recommended)**
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod
```

Atau drag & drop folder ke https://app.netlify.com/drop

### **2. Vercel**
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod
```

### **3. Firebase Hosting**
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login & init
firebase login
firebase init hosting

# Deploy
firebase deploy
```

### **4. GitHub Pages**
```bash
# Push ke GitHub
git init
git add .
git commit -m "Deploy PWA"
git branch -M main
git remote add origin https://github.com/username/repo.git
git push -u origin main

# Enable Pages di Settings → Pages
```

**Note:** GitHub Pages otomatis HTTPS dengan custom domain.

---

## 🔄 Update PWA

### **Update Content:**
1. Edit file HTML/CSS/JS
2. Update `CACHE_NAME` di `sw.js` (misal: `v1` → `v2`)
3. Deploy ke hosting
4. User otomatis dapat update saat buka app

### **Force Update:**
```javascript
// Tambah di sw.js
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((names) => {
      return Promise.all(
        names.map((name) => {
          if (name !== CACHE_NAME) {
            return caches.delete(name);
          }
        })
      );
    })
  );
});
```

---

## 📱 Testing PWA

### **Chrome DevTools:**
1. Buka DevTools (F12)
2. Tab **Application**
3. Check:
   - Manifest (lihat icon, name, colors)
   - Service Workers (status: activated)
   - Cache Storage (files cached)

### **Lighthouse (PWA Audit):**
1. DevTools → Tab **Lighthouse**
2. Select **Progressive Web App**
3. Click **Generate report**
4. Target score: **90+/100**

### **Test Install:**
1. Buka di mobile Chrome
2. Check ada prompt "Add to Home screen"
3. Install dan test functionality

---

## 🐛 Troubleshooting

**Service Worker tidak register:**
→ Pastikan running di HTTPS (atau localhost)
→ Check console untuk error messages
→ Clear cache dan reload (Ctrl+Shift+R)

**Offline tidak work:**
→ Check file URLs di `urlsToCache` benar
→ Test dengan airplane mode
→ Check Network tab → Offline checkbox

**Update tidak muncul:**
→ Update `CACHE_NAME` di sw.js
→ Unregister old service worker
→ Hard refresh (Ctrl+Shift+R)

**Install prompt tidak muncul:**
→ Check manifest.json valid (DevTools → Application)
→ Pastikan icon PNG (bukan JPG)
→ Check `display: "standalone"` di manifest
→ Service worker harus registered

---

## 🎨 Customization

### **Change App Name:**
Edit `manifest.json`:
```json
{
  "name": "Your App Name",
  "short_name": "YourApp"
}
```

### **Change Icon:**
1. Generate icon set (192x192 dan 512x512) di:
   - https://icon.kitchen/
   - https://www.appicon.co/
2. Replace `icon-192.png` dan `icon-512.png`

### **Change Theme Color:**
Edit `manifest.json` dan HTML:
```json
{
  "theme_color": "#667eea",
  "background_color": "#667eea"
}
```

```html
<meta name="theme-color" content="#667eea">
```

---

## 📈 Kapan Pakai Versi Ini?

**Cocok untuk:**
- ✅ Mobile-first application
- ✅ Need offline functionality
- ✅ Want app-like experience
- ✅ Quick distribution (no app store)
- ✅ Progressive enhancement
- ✅ Production-ready web app

**Tidak cocok untuk:**
- ❌ Need APK file (gunakan eTMv3)
- ❌ Complex native features
- ❌ Play Store distribution

---

## 🎓 Upgrade Path

**Dari eTMv1 → eTMv2:**
1. Tambah `manifest.json`
2. Tambah `sw.js`
3. Tambah service worker registration di HTML
4. Deploy ke HTTPS hosting

**Dari eTMv2 → eTMv3 (APK):**
1. Setup Capacitor
2. Wrap PWA dengan WebView
3. Build APK untuk distribusi offline

---

## 📞 Support & Resources

**Documentation:**
- PWA Basics: https://web.dev/progressive-web-apps/
- Service Worker: https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- Web Manifest: https://web.dev/add-manifest/

**Tools:**
- Lighthouse: PWA audit tool
- Workbox: Advanced service worker library
- PWA Builder: https://www.pwabuilder.com/

---

**Built with ❤️**
- Progressive Web App
- Service Worker magic
- Google Apps Script backend
- Zero-cost hosting

**"The best app is the one you don't have to install."** 🚀
