# 📱 PWA2APK - URL WRAPPER

## 🎯 DESKRIPSI

Aplikasi ini adalah **APK wrapper** sederhana untuk membungkus website/PWA menjadi APK Android menggunakan Capacitor.

**Target URL:** https://mulsim1001.id/tm/

---

## 📁 STRUKTUR PROJECT

```
project/
├── public/
│   ├── index.html         # Redirect page ke URL target
│   ├── manifest.json      # PWA manifest
│   ├── sw.js              # Service worker
│   └── icon-*.png         # App icons
├── android/               # Capacitor Android project
├── capacitor.config.json  # Capacitor configuration
└── replit.md              # Dokumentasi
```

---

## 🏗️ CARA KERJA

### **Web Version:**
- `index.html` melakukan redirect otomatis ke URL target
- Meta refresh tag + JavaScript redirect
- Service worker di-unregister untuk menghindari cache

### **APK Version:**
- Capacitor config `server.url` langsung mengarah ke URL target
- APK membuka URL dalam WebView native
- Tidak perlu hosting, langsung load dari internet

---

## ⚙️ KONFIGURASI

### **Capacitor Config** (`capacitor.config.json`):
```json
{
  "appId": "id.mulsim1001.tm",
  "appName": "TM Mulsim1001",
  "webDir": "public",
  "server": {
    "url": "https://mulsim1001.id/tm/",
    "cleartext": true
  }
}
```

**Penjelasan:**
- `appId`: Package name untuk APK (format: com.domain.app)
- `appName`: Nama aplikasi yang muncul di Android
- `server.url`: URL yang akan dibuka di APK
- `cleartext`: Allow HTTP (set false untuk production)

---

## 🚀 CARA BUILD APK

### **Prasyarat:**
- Java JDK 17+
- Android Studio (atau Android SDK + Gradle)

### **Langkah Build:**

#### **Opsi 1: Local Machine**
```bash
# 1. Sync web assets ke Android project
npx cap sync

# 2. Build APK
cd android
./gradlew assembleDebug

# 3. APK ada di:
# android/app/build/outputs/apk/debug/app-debug.apk
```

#### **Opsi 2: Android Studio**
1. Buka Android Studio
2. Open Project → Pilih folder `android/`
3. Build > Build Bundle(s)/APK(s) > Build APK(s)
4. APK ada di: `android/app/build/outputs/apk/debug/`

#### **Opsi 3: GitHub Actions (Cloud Build)**
1. Push project ke GitHub
2. Setup workflow untuk auto-build APK
3. Download APK dari Actions tab

**Panduan lengkap:** Lihat file `BUILD_APK.md`

---

## 📦 DISTRIBUSI APK

### **Cara 1: Direct Download**
- Upload APK ke Google Drive / Dropbox
- Share link ke user
- User download & install manual (Enable "Install from Unknown Sources")

### **Cara 2: Website Download**
- Upload APK ke hosting
- Buat halaman download dengan tombol
- User download langsung dari browser

### **Cara 3: WhatsApp/Telegram**
- Kirim file APK langsung ke user
- User buka file untuk install

---

## 🔧 UBAH URL TARGET

Untuk mengubah URL yang dibungkus, edit 2 file:

### **1. `capacitor.config.json`**
```json
"server": {
  "url": "https://your-url-here.com",
  "cleartext": true
}
```

### **2. `public/index.html`**
- Update meta refresh tag: `<meta http-equiv="refresh" content="0; url=https://your-url-here.com">`
- Update JavaScript redirect: `window.location.replace('https://your-url-here.com');`
- Update manual link: `<a href="https://your-url-here.com">...</a>`

### **3. Sync ulang:**
```bash
npx cap sync
```

---

## 🎨 CUSTOMIZATION

### **App Name & Package:**
Edit `capacitor.config.json`:
```json
{
  "appId": "com.yourcompany.appname",
  "appName": "Your App Name"
}
```

### **App Icon:**
Replace files:
- `public/icon-192.png` (192x192)
- `public/icon-512.png` (512x512)

Atau gunakan tool online untuk generate icon set:
- https://icon.kitchen/
- https://www.appicon.co/

### **Splash Screen:**
Edit Android resources di `android/app/src/main/res/`

---

## 🌐 WEB VERSION (Testing)

**Server sudah running di port 5000:**
```
http://localhost:5000/
```

**Workflow:** Web Server (http-server)

**Note:** Web version akan redirect ke URL target. Gunakan untuk testing bahwa redirect bekerja dengan baik.

---

## ✅ STATUS

- ✅ Capacitor configured
- ✅ Android project setup
- ✅ Web assets synced
- ✅ URL redirect configured
- ✅ Ready to build APK

---

## 🔄 UPDATE WORKFLOW

Jika URL target berubah:

1. **Edit config** (capacitor.config.json + index.html)
2. **Sync:** `npx cap sync`
3. **Build APK baru:** `cd android && ./gradlew assembleDebug`
4. **Distribute:** Upload APK baru ke channel distribusi

---

## 📱 HASIL AKHIR

**APK yang di-build akan:**
- Buka URL target di WebView native Android
- Tampil seperti app native (fullscreen, dengan icon)
- Tidak perlu browser (standalone app)
- Dapat install seperti app biasa dari .apk file

**Ukuran APK:** ~5-7 MB (Capacitor + WebView wrapper)

---

## 💡 USE CASES

**PWA2APK wrapper cocok untuk:**
- Wrap website yang sudah ada menjadi APK
- Distribusi internal (tanpa Play Store)
- Testing PWA sebagai native app
- Quick mobile app dari web app

**Tidak cocok untuk:**
- App yang butuh fitur native kompleks
- App yang butuh performa tinggi
- App dengan asset besar (better embed in app)

---

## 🔐 PRODUCTION BUILD

Untuk distribusi production:

### **1. Update config untuk HTTPS:**
```json
"server": {
  "url": "https://mulsim1001.id/tm/",
  "cleartext": false
}
```

### **2. Build Release APK:**
```bash
cd android
./gradlew assembleRelease
```

### **3. Sign APK:**
- Generate keystore
- Sign dengan jarsigner
- Align dengan zipalign

**Panduan signing:** https://developer.android.com/studio/publish/app-signing

---

## 📞 TROUBLESHOOTING

**APK blank screen:**
→ Pastikan URL target accessible dari internet
→ Check console: `adb logcat`

**URL tidak load:**
→ Check internet permission di AndroidManifest.xml
→ Pastikan cleartext: true untuk HTTP

**Build error:**
→ Check Java version: `java -version` (need 17+)
→ Clean build: `./gradlew clean`

---

**Built with ❤️ using:**
- Capacitor (Native wrapper)
- Zero server dependency
- Simple URL redirect
- Ready for APK build

**"Simple APK Wrapper"** 🚀
