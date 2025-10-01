# 📦 eTM v3.0 - PWA2APK (URL Wrapper)

## 🎯 Deskripsi

Versi **APK wrapper** untuk membungkus website/PWA menjadi aplikasi Android native menggunakan Capacitor. Versi ini tidak hosting aplikasi sendiri, melainkan **wrap URL eksternal** ke dalam APK.

**Target URL:** https://mulsim1001.id/tm/

**Karakteristik:**
- ✅ Native Android APK
- ✅ Distribusi via file APK (tanpa Play Store)
- ✅ Load URL dalam WebView
- ✅ Icon & nama app custom
- ✅ Fullscreen native experience
- ❌ Tidak perlu hosting sendiri (APK)
- ⚠️ **Butuh koneksi internet untuk akses website**

---

## ⚠️ Penting: Network Requirements

**APK wrapper ini membutuhkan koneksi internet aktif untuk bekerja.**

### **Mengapa?**
- APK tidak menyimpan konten website secara lokal
- Setiap kali dibuka, APK load URL dari server: `https://mulsim1001.id/tm/`
- WebView Android hanya render halaman yang didownload dari internet

### **Offline Support?**
- **APK itu sendiri:** ❌ Tidak ada offline support
- **Website target:** Tergantung apakah website target (mulsim1001.id) implement PWA dengan offline caching
- **Best practice:** Jika butuh offline, website target harus punya service worker sendiri

### **Konsekuensi:**
- ✅ Update website otomatis reflect tanpa rebuild APK
- ✅ Hemat storage device (no cached content)
- ❌ Tidak bisa buka app tanpa internet
- ❌ Loading time tergantung koneksi internet

---

## 📁 File yang Ada

```
eTMv3/
├── index.html         # Redirect page (fallback)
├── manifest.json      # PWA manifest
├── sw.js              # Service worker
├── icon-192.png       # App icon 192x192
├── icon-512.png       # App icon 512x512
└── README.md          # Dokumentasi ini
```

**Note:** Folder `android/` ada di root project (shared untuk semua versi APK build).

---

## ⚙️ Cara Kerja

### **Web Version:**
- File `index.html` melakukan redirect ke URL target
- Meta refresh + JavaScript redirect
- Service worker di-unregister untuk avoid cache

### **APK Version:**
- Capacitor config `server.url` pointing ke URL target
- APK load URL langsung dalam WebView Android
- User experience seperti app native

---

## 🚀 Cara Build APK

### **Prasyarat:**
- Node.js & npm installed
- Java JDK 17+ installed
- Android Studio (atau Gradle standalone)

### **Langkah Build:**

#### **1. Install Dependencies (jika belum)**
```bash
# Di root project
npm install
```

#### **2. Update URL Target (jika perlu)**

Edit `capacitor.config.json` di **root project**:
```json
{
  "appId": "id.mulsim1001.tm",
  "appName": "TM Mulsim1001",
  "webDir": "eTMv3",
  "server": {
    "url": "https://mulsim1001.id/tm/",
    "cleartext": true
  }
}
```

**Parameter Penting:**
- `appId`: Package name Android (format: com.domain.app)
- `appName`: Nama app yang tampil di Android
- `webDir`: Folder source (gunakan "eTMv3")
- `server.url`: URL yang akan di-wrap

#### **3. Sync Web Assets ⚠️ WAJIB**
```bash
# Di root project
npx cap sync
```

**⚠️ PENTING:** Command ini WAJIB dijalankan karena:
- Generate Android assets di `android/app/src/main/assets/public/`
- Files tersebut TIDAK ada di GitHub repository
- Tanpa ini, build APK akan ERROR!

#### **4. Build APK**

**Opsi A: Terminal (Recommended)**
```bash
cd android
./gradlew assembleDebug
```

APK tersedia di:
```
android/app/build/outputs/apk/debug/app-debug.apk
```

**Opsi B: Android Studio**
1. Buka Android Studio
2. Open Project → Pilih folder `android/`
3. Build > Build Bundle(s)/APK(s) > Build APK(s)
4. APK ada di: `android/app/build/outputs/apk/debug/`

**Opsi C: Release APK (Production)**
```bash
cd android
./gradlew assembleRelease
```

Release APK harus di-sign sebelum distribusi.

---

## 📱 Install APK di Android

### **Enable Unknown Sources:**
1. Settings → Security
2. Enable **"Install from Unknown Sources"** atau **"Allow from this source"**

### **Install APK:**
1. Transfer APK ke HP (via USB, WhatsApp, email, dll)
2. Buka file APK di File Manager
3. Tap **"Install"**
4. App muncul di app drawer

---

## 📦 Distribusi APK

### **1. Direct Download**
Upload APK ke:
- Google Drive (share link)
- Dropbox
- WeTransfer
- File hosting lainnya

Share link ke user untuk download.

### **2. Website Download**
```html
<a href="/downloads/app-debug.apk" download>
  Download APK
</a>
```

### **3. WhatsApp/Telegram**
- Kirim file APK langsung ke user/group
- User download dan install

### **4. QR Code**
Generate QR code untuk URL download APK:
- https://www.qr-code-generator.com/

User scan → download → install.

---

## 🔄 Update URL Target

Untuk wrap URL berbeda:

### **1. Edit Capacitor Config**
Edit `capacitor.config.json`:
```json
{
  "server": {
    "url": "https://your-new-url.com",
    "cleartext": true
  }
}
```

### **2. Update Redirect Fallback**
Edit `index.html`:
```html
<meta http-equiv="refresh" content="0; url=https://your-new-url.com">
```

Dan di JavaScript:
```javascript
window.location.replace('https://your-new-url.com');
```

### **3. Sync & Rebuild**
```bash
npx cap sync
cd android
./gradlew assembleDebug
```

---

## 🎨 Customization

### **App Name & Package ID**
Edit `capacitor.config.json`:
```json
{
  "appId": "com.yourcompany.appname",
  "appName": "Your App Name"
}
```

Setelah edit, run:
```bash
npx cap sync
```

### **App Icon**

**Generate Icon Set:**
1. Gunakan tool online:
   - https://icon.kitchen/
   - https://www.appicon.co/
   - https://easyappicon.com/

2. Download semua ukuran icon Android

3. Replace files di:
   ```
   android/app/src/main/res/
   ├── mipmap-hdpi/ic_launcher.png
   ├── mipmap-mdpi/ic_launcher.png
   ├── mipmap-xhdpi/ic_launcher.png
   ├── mipmap-xxhdpi/ic_launcher.png
   └── mipmap-xxxhdpi/ic_launcher.png
   ```

### **Splash Screen**
Edit resources di:
```
android/app/src/main/res/
├── drawable/splash.png
└── values/colors.xml
```

---

## 🔧 Advanced Config

### **Permissions (AndroidManifest.xml)**

Location: `android/app/src/main/AndroidManifest.xml`

Default permissions:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Add jika perlu:
```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### **WebView Settings**

Edit `MainActivity.java`:
```java
WebSettings settings = webView.getSettings();
settings.setJavaScriptEnabled(true);
settings.setDomStorageEnabled(true);
```

---

## 🔐 Production Build & Signing

### **1. Generate Keystore**
```bash
keytool -genkey -v -keystore my-release-key.keystore \
  -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

### **2. Configure Gradle**
Edit `android/app/build.gradle`:
```gradle
android {
    signingConfigs {
        release {
            storeFile file("my-release-key.keystore")
            storePassword "password"
            keyAlias "my-key-alias"
            keyPassword "password"
        }
    }
    buildTypes {
        release {
            signingConfig signingConfigs.release
        }
    }
}
```

### **3. Build Release APK**
```bash
cd android
./gradlew assembleRelease
```

Release APK di:
```
android/app/build/outputs/apk/release/app-release.apk
```

---

## 💻 Teknologi

- **Capacitor** - Native wrapper framework
- **Android WebView** - Render web content
- **Gradle** - Build system
- **Java/Kotlin** - Android native layer

**No Server Needed** - APK load URL directly from internet!

---

## 📊 Arsitektur

```
┌──────────────────────┐
│   Android APK        │
│   (Capacitor Shell)  │
└──────────┬───────────┘
           │ WebView
           ▼
┌──────────────────────┐
│   Target Website     │
│ https://mulsim1001   │
│        .id/tm/       │
└──────────────────────┘
```

**Flow:**
1. User buka app
2. APK load URL dalam WebView
3. Website render di native Android window
4. User interact seperti app native

---

## 🐛 Troubleshooting

**APK blank screen:**
→ Pastikan URL accessible dari internet
→ Check Android logs: `adb logcat`
→ Check `cleartext: true` untuk HTTP

**Build error:**
→ Check Java version: `java -version` (need 17+)
→ Clean build: `cd android && ./gradlew clean`
→ Delete `android/build` dan rebuild

**APK tidak install:**
→ Enable "Unknown Sources" di Settings
→ Check file tidak corrupt (re-download)
→ Uninstall old version dulu

**URL tidak load:**
→ Check internet permission di AndroidManifest
→ Test URL di browser mobile dulu
→ Check CORS settings di server

---

## 📈 Kapan Pakai Versi Ini?

**Cocok untuk:**
- ✅ Wrap existing website jadi APK
- ✅ Distribusi internal (tanpa Play Store)
- ✅ Quick mobile app dari web app
- ✅ Testing PWA sebagai native app
- ✅ Client minta "file APK"

**Tidak cocok untuk:**
- ❌ Complex native features (camera, GPS, etc)
- ❌ High-performance apps (games, video editing)
- ❌ Large assets better embedded in app
- ❌ Play Store distribution (prefer native)

---

## 🎓 Upgrade Path

**eTMv3 → Native App:**
1. Embed HTML/CSS/JS dalam APK (ubah `webDir`)
2. Add native plugins (Camera, Geolocation, etc)
3. Use Capacitor plugins untuk native features

**eTMv3 → Play Store:**
1. Build release APK dengan signing
2. Create Google Play Console account
3. Upload APK/AAB
4. Submit for review

---

## 📦 File Sizes

**APK Size:**
- Debug APK: ~5-7 MB
- Release APK: ~3-5 MB (after ProGuard)

**Download Size** (untuk end user):
- APK download: 3-7 MB
- First time use: Load website dari internet
- Subsequent uses: Cached by WebView

---

## 🔄 Update Strategy

### **Update URL Content:**
- Edit website, APK otomatis load versi terbaru
- No need rebuild APK

### **Update APK (icon, name, config):**
1. Edit config
2. Rebuild APK
3. Distribute new APK file
4. User install over old version (update)

---

## 📞 Support & Resources

**Capacitor Docs:**
- Official: https://capacitorjs.com/
- Android Guide: https://capacitorjs.com/docs/android

**Android Development:**
- Developer Guide: https://developer.android.com/
- WebView: https://developer.android.com/reference/android/webkit/WebView

**Build Tools:**
- Gradle: https://gradle.org/
- Android Studio: https://developer.android.com/studio

---

## 📝 Checklist Pre-Distribution

Sebelum distribusi APK ke user:

- [ ] Test URL accessible dari mobile
- [ ] Test APK di real device (bukan emulator)
- [ ] Check app name dan icon sudah benar
- [ ] Test install/uninstall flow
- [ ] Check permission requests
- [ ] Test offline behavior
- [ ] Create documentation untuk user
- [ ] Prepare download/distribution channel

---

**Built with ❤️**
- Capacitor wrapper
- Android WebView
- Simple URL redirect
- No server hosting needed

**"The simplest APK wrapper ever!"** 📦🚀
