# 🚀 Panduan Upload Project ke GitHub

## 📋 Daftar Isi

1. [Persiapan](#persiapan)
2. [Buat Repository di GitHub](#buat-repository-di-github)
3. [Upload dari Replit](#upload-dari-replit)
4. [Verifikasi & Testing](#verifikasi--testing)
5. [Update & Maintenance](#update--maintenance)

---

## 📦 Isi Project

Project ini berisi:

### **📱 3 Versi Task Manager:**
- **eTMv1/** - Web App Basic (tanpa PWA)
- **eTMv2/** - Progressive Web App
- **eTMv3/** - PWA2APK (URL Wrapper untuk Android)

### **📚 Ebook (Folder buku/):**
- BUKU_LENGKAP.md & .html
- DRAFT_BUKU.md & .html
- BUKU_LENGKAP.docx
- DRAFT_BUKU.docx

### **📖 Dokumentasi:**
- README.md - Dokumentasi utama
- BUILD_APK.md - Panduan build APK
- GOOGLE_APPS_SCRIPT.md - Setup backend
- replit.md - Project memory

### **🔧 Config Files:**
- capacitor.config.json
- package.json
- .gitignore

### **📂 Folder Android:**
- android/ - Capacitor Android project (untuk build APK)

---

## ⚙️ Persiapan

### **1. Cek Git Tersedia**

Di Replit Shell, jalankan:
```bash
git --version
```

Jika git sudah ada, lanjut ke langkah berikutnya.

### **2. Bersihkan File Temporary**

Pastikan `.gitignore` sudah setup dengan benar (sudah ada di project).

File yang akan **TIDAK** diupload ke GitHub:
- `node_modules/` (akan di-download ulang via `npm install`)
- `android/app/build/` (build output)
- `.cache/`, `.local/`, `.upm/` (Replit cache)
- `*.apk`, `*.aab` (APK files - size besar)
- `*.log`, `.env` (sensitive files)

---

## 🌐 Buat Repository di GitHub

### **Opsi A: Via Website GitHub (Recommended)**

1. **Login ke GitHub:** https://github.com/
2. **Klik tombol "+" di pojok kanan atas** → **"New repository"**
3. **Isi form:**
   - **Repository name:** `task-manager-pwa-collection` (atau nama yang Anda mau)
   - **Description:** `Collection of Task Manager: Web App, PWA, and PWA2APK wrapper with ebook`
   - **Visibility:** 
     - **Public** - Jika mau dibuka untuk umum
     - **Private** - Jika hanya untuk pribadi/tim
   - **Jangan centang** "Initialize with README" (karena kita sudah punya)
4. **Klik "Create repository"**
5. **Copy URL repository** yang muncul, contoh:
   ```
   https://github.com/username/task-manager-pwa-collection.git
   ```

### **Opsi B: Via GitHub CLI (Advanced)**

```bash
# Install GitHub CLI (jika belum)
gh auth login

# Buat repo
gh repo create task-manager-pwa-collection --public --source=. --remote=origin
```

---

## 📤 Upload dari Replit

### **Langkah-langkah:**

#### **1. Buka Shell di Replit**

Klik tab "Shell" di Replit IDE.

#### **2. Initialize Git (Jika belum)**

```bash
git init
```

Output yang diharapkan:
```
Initialized empty Git repository in /home/runner/...
```

#### **3. Add Remote Repository**

Ganti `USERNAME` dan `REPO_NAME` dengan GitHub Anda:

```bash
git remote add origin https://github.com/USERNAME/REPO_NAME.git
```

Contoh:
```bash
git remote add origin https://github.com/johndoe/task-manager-pwa-collection.git
```

**Verifikasi:**
```bash
git remote -v
```

Output:
```
origin  https://github.com/USERNAME/REPO_NAME.git (fetch)
origin  https://github.com/USERNAME/REPO_NAME.git (push)
```

#### **4. Add All Files**

```bash
git add .
```

Perintah ini akan add semua file **kecuali** yang ada di `.gitignore`.

**Cek file yang akan diupload:**
```bash
git status
```

Output akan menunjukkan:
- File hijau = akan dicommit
- File merah = belum di-add (atau di-ignore)

#### **5. Commit Changes**

```bash
git commit -m "Initial commit: Task Manager 3 versions + ebook + documentation"
```

Output:
```
[main (root-commit) abc1234] Initial commit: ...
 XX files changed, YYYY insertions(+)
```

#### **6. Push ke GitHub**

**Untuk pertama kali:**
```bash
git branch -M main
git push -u origin main
```

**Jika diminta login:**
- Username: `your-github-username`
- Password: **Gunakan Personal Access Token** (bukan password biasa)

**Cara buat Personal Access Token:**
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token → Classic
3. Set expiration & check `repo` scope
4. Copy token (hanya muncul sekali!)
5. Paste sebagai password saat `git push`

**Alternatif (Recommended): SSH Key**

Setup SSH key untuk tidak perlu token setiap kali:
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Copy public key
cat ~/.ssh/id_ed25519.pub
```

Paste ke: GitHub → Settings → SSH and GPG keys → New SSH key

Lalu ubah remote:
```bash
git remote set-url origin git@github.com:USERNAME/REPO_NAME.git
```

---

## ✅ Verifikasi & Testing

### **1. Cek Repository di GitHub**

Buka: `https://github.com/USERNAME/REPO_NAME`

Pastikan semua file sudah terupload:
- ✅ Folder: `eTMv1/`, `eTMv2/`, `eTMv3/`, `buku/`, `android/`
- ✅ File: `README.md`, `BUILD_APK.md`, `GOOGLE_APPS_SCRIPT.md`
- ✅ Config: `capacitor.config.json`, `package.json`
- ❌ **TIDAK ADA**: `node_modules/`, `build/`, `.cache/`, `*.apk`

### **2. Test Clone di Tempat Lain**

Untuk memastikan project bisa diclone orang lain:

```bash
# Di komputer/folder berbeda
git clone https://github.com/USERNAME/REPO_NAME.git
cd REPO_NAME

# Install dependencies
npm install

# ⚠️ WAJIB: Sync Capacitor (regenerate Android assets)
# File di android/app/src/main/assets/public/ tidak di-commit ke GitHub
# Harus di-generate ulang dengan command ini:
npx cap sync
```

**PENTING:** `npx cap sync` **WAJIB** dijalankan setelah clone karena:
- Android web assets (`android/app/src/main/assets/public/`) tidak ada di GitHub
- Files tersebut di-generate dari folder `eTMv1/`, `eTMv2/`, atau `eTMv3/`
- Tanpa ini, build APK akan error!

### **3. Cek README Tampil dengan Baik**

GitHub otomatis render `README.md` di halaman utama repo.
Pastikan:
- ✅ Formatting rapi
- ✅ Link antar dokumentasi bekerja
- ✅ Comparison table tampil benar

---

## 🔄 Update & Maintenance

### **Setelah Edit File di Replit:**

```bash
# 1. Cek perubahan
git status

# 2. Add file yang berubah
git add .

# 3. Commit dengan message deskriptif
git commit -m "Update: [deskripsi perubahan]"

# 4. Push ke GitHub
git push origin main
```

### **Contoh Update Messages:**

```bash
git commit -m "Fix: Perbaiki offline support documentation di eTMv3"
git commit -m "Add: Tambah section troubleshooting di README"
git commit -m "Update: Improve API URL configuration guide"
git commit -m "Docs: Update ebook dengan chapter baru"
```

### **Pull Changes dari GitHub (Jika Edit di Web):**

```bash
git pull origin main
```

---

## 📊 GitHub Features yang Berguna

### **1. GitHub Pages (Hosting Gratis)**

Untuk hosting eTMv1 atau eTMv2:

1. Settings → Pages
2. Source: Deploy from branch `main`
3. Folder: `/eTMv2` (pilih folder yang mau dihost)
4. Save
5. URL akan tersedia: `https://username.github.io/repo-name/`

**Note:** Hanya untuk eTMv1 & eTMv2. eTMv3 perlu build APK.

### **2. Releases (Distribusi APK)**

Untuk distribusi APK tanpa hosting sendiri:

1. Build APK:
   ```bash
   npx cap sync
   cd android
   ./gradlew assembleDebug
   ```

2. GitHub repo → **Releases** → **Create new release**
3. Tag version: `v1.0.0`
4. Title: `Task Manager APK v1.0.0`
5. Upload file: `app-debug.apk`
6. Publish release
7. Share link release ke user untuk download APK

### **3. README Badges (Optional)**

Tambahkan badges di `README.md` untuk tampilan lebih professional:

```markdown
![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Web%20%7C%20Android-orange)
```

### **4. GitHub Actions (Auto Build APK)**

Setup CI/CD untuk auto-build APK setiap push:

Buat file `.github/workflows/build-apk.yml`:
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
      - run: npm install
      - run: npx cap sync
      - run: cd android && chmod +x ./gradlew && ./gradlew assembleDebug
      - uses: actions/upload-artifact@v3
        with:
          name: app-debug.apk
          path: android/app/build/outputs/apk/debug/app-debug.apk
```

---

## 🔒 Security Best Practices

### **Jangan Upload ke GitHub:**

❌ **API Keys** - Jangan hardcode di code
❌ **Password** - Jangan simpan di file
❌ **Private Keys** - .keystore, .jks
❌ **Environment files** - .env dengan credentials
❌ **Large binary files** - APK > 100MB

### **Gunakan `.gitignore`** untuk exclude files sensitif.

### **Gunakan GitHub Secrets** untuk CI/CD:

Repository → Settings → Secrets → Actions → New secret

---

## 🐛 Troubleshooting

### **Error: "Permission denied (publickey)"**
→ Setup SSH key (lihat bagian SSH di atas)

### **Error: "remote: Repository not found"**
→ Cek URL repository benar
→ Cek akses ke repository (public/private)

### **Error: "Updates were rejected"**
→ Pull dulu: `git pull origin main`
→ Atau force push (hati-hati!): `git push -f origin main`

### **File besar tidak bisa diupload**
→ GitHub limit 100MB per file
→ Gunakan Git LFS untuk file > 50MB
→ Atau upload ke release (max 2GB)

### **node_modules terupload**
→ Pastikan ada `node_modules/` di `.gitignore`
→ Remove dari git:
```bash
git rm -r --cached node_modules
git commit -m "Remove node_modules"
git push origin main
```

---

## 📚 Resources

**Git Basics:**
- https://git-scm.com/docs
- https://training.github.com/

**GitHub Docs:**
- https://docs.github.com/

**Capacitor & APK:**
- https://capacitorjs.com/docs/android

---

## ✅ Checklist

Sebelum push ke GitHub, pastikan:

- [ ] `.gitignore` sudah benar
- [ ] Tidak ada credentials di code
- [ ] README.md informatif
- [ ] Semua dokumentasi lengkap
- [ ] File temporary sudah dibersihkan
- [ ] Test clone di tempat lain berhasil

---

## 🎉 Selamat!

Project Anda sekarang ada di GitHub dan bisa:
- ✅ Diakses dari mana saja
- ✅ Diclone orang lain
- ✅ Tracking version history
- ✅ Collaboration dengan tim
- ✅ Hosting via GitHub Pages
- ✅ Distribusi APK via Releases

**Next Steps:**
1. Share link repository ke tim/client
2. Setup GitHub Pages untuk hosting
3. Create releases untuk distribusi APK
4. Add collaborators jika project tim

---

**Built with ❤️**  
Task Manager Collection - Web App, PWA, & PWA2APK

**"Your code is now immortal on GitHub!"** 🚀
