# 📱 Panduan Build APK - Task Manager

## ✅ Capacitor Sudah Ter-Setup!

Capacitor project sudah dikonfigurasi dan siap untuk build APK.

---

## 🔧 3 Cara Build APK:

### **Opsi 1: Build di Local Machine (RECOMMENDED)**

1. **Clone/Download project** ke komputer Anda
2. **Install dependencies:**
   ```bash
   npm install
   ```
3. **⚠️ WAJIB - Sync Capacitor (Generate Android assets):**
   ```bash
   npx cap sync
   ```
   **PENTING:** Command ini wajib karena file di `android/app/src/main/assets/public/` tidak ada di GitHub dan harus di-generate!
4. **Install Android Studio** (https://developer.android.com/studio)
5. **Install Java JDK 17+** jika belum ada
6. **Build APK:**
   ```bash
   cd android
   ./gradlew assembleDebug
   ```
7. **APK akan ada di:** `android/app/build/outputs/apk/debug/app-debug.apk`

**Waktu build:** ~5-10 menit (pertama kali download dependencies)

---

### **Opsi 2: Build dengan Android Studio GUI**

1. **Clone/Download project** ke komputer
2. **Install dependencies & sync:**
   ```bash
   npm install
   npx cap sync  # ⚠️ WAJIB!
   ```
3. **Buka Android Studio**
4. **Open Project** → Pilih folder `android/`
5. **Klik Build > Build Bundle(s)/APK(s) > Build APK(s)**
6. **Tunggu proses selesai**
7. **APK siap di:** `android/app/build/outputs/apk/debug/`

**Kelebihan:** Lebih mudah, ada GUI, bisa langsung test di emulator

---

### **Opsi 3: Cloud Build Service (Tercepat)**

**Menggunakan GitHub Actions (Gratis):**

1. **Push project ke GitHub**
2. **Buat file** `.github/workflows/build-apk.yml`:
   ```yaml
   name: Build APK
   on: [push]
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - uses: actions/setup-java@v3
           with:
             distribution: 'temurin'
             java-version: '17'
         - name: Install dependencies
           run: npm install
         - name: Sync Capacitor
           run: npx cap sync
         - name: Build APK
           run: |
             cd android
             chmod +x ./gradlew
             ./gradlew assembleDebug
         - uses: actions/upload-artifact@v3
           with:
             name: app-debug.apk
             path: android/app/build/outputs/apk/debug/app-debug.apk
   ```
3. **Push ke GitHub** → GitHub Actions otomatis build
4. **Download APK** dari tab Actions

**Kelebihan:** Tidak perlu install apapun, otomatis, gratis

---

## 📦 Distribusi APK (Tanpa Play Store)

Setelah APK berhasil di-build:

### **1. Direct Download**
- Upload APK ke cloud storage (Google Drive, Dropbox, dll)
- Share link ke user
- User download & install manual

### **2. Website Download**
- Upload APK ke website
- Buat halaman download dengan tombol
- User klik download langsung dari browser

### **3. WhatsApp/Telegram**
- Kirim APK langsung via chat (file <100MB)
- User download & install

### **4. QR Code**
- Generate QR code untuk link download APK
- User scan → otomatis download

---

## ⚠️ Cara Install APK Manual (User Android)

1. **Download APK** dari link yang diberikan
2. **Buka file APK** di HP
3. **Izinkan "Install from Unknown Sources"** (jika diminta)
4. **Klik Install**
5. **Selesai!** App muncul di home screen

---

## 🔐 Signing APK (Untuk Production)

**Build Debug APK:** Untuk testing internal (sudah cukup)

**Build Release APK:** Untuk distribusi luas
```bash
cd android
./gradlew assembleRelease
```

**Generate Keystore (sekali saja):**
```bash
keytool -genkey -v -keystore taskmanager.keystore -alias taskmanager -keyalg RSA -keysize 2048 -validity 10000
```

**Sign APK dengan keystore** → Buat APK yang trusted

---

## 📊 Ukuran APK

- **Debug APK:** ~5-7 MB
- **Release APK (optimized):** ~3-5 MB
- **Dengan ProGuard/R8:** ~2-3 MB

---

## 🚀 Update APK (Cara Distribusi Update)

**Tidak perlu publish ulang ke Play Store!**

1. Update code di Replit
2. Build APK baru (versi number naik)
3. Share link download APK baru
4. User install APK baru (replace otomatis)

**Atau gunakan PWA** → Update otomatis tanpa install ulang!

---

## ✅ Status Setup

- ✅ Capacitor ter-install
- ✅ Android platform ditambahkan  
- ✅ Web assets sudah di-sync
- ✅ Project siap untuk build
- ✅ PWA sudah aktif (alternatif tanpa APK)

**Pilih cara yang paling cocok untuk Anda!**

---

## 💡 Rekomendasi

**Untuk Testing:** Gunakan PWA (install langsung dari browser)

**Untuk Distribusi Luas:** Build APK dengan GitHub Actions (gratis & otomatis)

**Untuk Kontrol Penuh:** Build di Android Studio lokal
