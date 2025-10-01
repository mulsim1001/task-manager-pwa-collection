# 📋 eTM v1.0 - Task Manager (Tanpa PWA)

## 🎯 Deskripsi

Versi basic dari Task Manager - aplikasi web murni **tanpa fitur PWA**. Aplikasi ini hanya menggunakan HTML, CSS, dan JavaScript vanilla tanpa service worker atau manifest.

**Karakteristik:**
- ❌ Tidak bisa diinstall sebagai app
- ❌ Tidak ada offline support
- ❌ Tidak ada icon di home screen
- ✅ Aplikasi web standar yang berjalan di browser

---

## 📁 File yang Ada

```
eTMv1/
├── index.html         # Halaman login (tanpa service worker)
├── dashboard.html     # Dashboard task management
├── icon-192.png       # Icon untuk referensi (tidak digunakan sebagai app icon)
├── icon-512.png       # Icon untuk referensi (tidak digunakan sebagai app icon)
└── README.md          # Dokumentasi ini
```

**Catatan:** Tidak ada `manifest.json` dan `sw.js` karena versi ini tidak support PWA.

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

### **3. Jalankan Aplikasi**

#### **Opsi A: HTTP Server (Node.js)**
```bash
npx http-server . -p 8080
```
Buka: `http://localhost:8080/`

#### **Opsi B: Python Server**
```bash
python -m http.server 8080
```
Buka: `http://localhost:8080/`

#### **Opsi C: PHP Server**
```bash
php -S localhost:8080
```
Buka: `http://localhost:8080/`

#### **Opsi D: Live Server (VS Code)**
Install extension "Live Server" dan klik "Go Live"

---

## 🔑 Login

**Default Users:**
- Username: `mulia` | Password: `m1001`
- Username: `arif` | Password: `a1001`
- Username: `ilham` | Password: `i1001`
- Username: `restu` | Password: `r1001`

---

## ⚡ Fitur

1. **Login System** - Autentikasi via Google Sheets
2. **Add Task** - Tambah task baru dengan PIC otomatis
3. **View Tasks** - Lihat semua task dengan sorting
4. **Complete Task** - Tandai task selesai dengan catatan
5. **Delete Task** - Hapus task dengan konfirmasi
6. **Auto Overdue Check** - Task > 24 jam otomatis status "overdue"
7. **Auto Refresh** - Dashboard refresh setiap 30 detik
8. **Responsive Design** - Desktop (tabel) & Mobile (card layout)

---

## 💻 Teknologi

- **Pure HTML5** - Struktur halaman
- **Pure CSS3** - Styling dan layout
- **Vanilla JavaScript** - Logic dan interaksi
- **Google Apps Script** - Backend API
- **Google Sheets** - Database
- **SessionStorage** - State management client-side

**Zero Dependencies** - Tidak butuh npm, webpack, atau framework apapun!

---

## 📊 Arsitektur

```
┌─────────────────────┐
│   Browser (Client)  │
│   • index.html      │
│   • dashboard.html  │
└──────────┬──────────┘
           │ HTTPS
           ▼
┌─────────────────────┐
│ Google Apps Script  │
│ (Backend API)       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Google Sheets      │
│  (Database)         │
└─────────────────────┘
```

---

## 🔧 Kelebihan vs Kekurangan

### **Kelebihan:**
- ✅ Simple dan mudah dipahami
- ✅ No build process needed
- ✅ Tidak butuh dependencies
- ✅ Hosting lebih mudah (bisa static hosting)
- ✅ Loading cepat (file kecil)

### **Kekurangan:**
- ❌ Tidak bisa install sebagai app mobile
- ❌ Tidak ada offline support
- ❌ Tidak ada caching otomatis
- ❌ Harus buka browser setiap kali
- ❌ Tidak muncul di app drawer/home screen

---

## 📱 Mobile Usage

Untuk menggunakan di mobile:
1. Buka URL di browser mobile (Chrome/Safari)
2. Bookmark halaman untuk akses cepat
3. Gunakan langsung dari browser

**Tips:** Jika ingin experience seperti app, gunakan **eTMv2** (dengan PWA support)

---

## 🌐 Deployment

### **Static Hosting (Gratis):**
1. **Netlify**
   - Drag & drop folder ini ke Netlify
   - URL otomatis tersedia

2. **Vercel**
   - Import folder via Vercel CLI atau Web
   - Deploy otomatis

3. **GitHub Pages**
   - Push ke GitHub repo
   - Enable Pages di Settings
   - Akses via `username.github.io/repo`

4. **Firebase Hosting**
   ```bash
   firebase init hosting
   firebase deploy
   ```

### **Shared Hosting (cPanel):**
- Upload semua file via FTP ke `public_html`
- Akses via domain Anda

---

## 🔄 Update & Maintenance

### **Tambah User Baru:**
1. Buka Google Sheets
2. Tab `user` → Add row
3. Isi: username | password | role

### **Edit Task Manual:**
1. Buka Google Sheets
2. Tab `db01` → Edit langsung di cell
3. Refresh dashboard

### **Update Code:**
1. Edit file HTML langsung
2. Refresh browser (Ctrl+F5 / Cmd+Shift+R)
3. No build process needed!

---

## 🐛 Troubleshooting

**Login gagal:**
→ Cek API_URL sudah diupdate dengan benar
→ Cek Google Sheets dan Apps Script sudah deploy

**Task tidak muncul:**
→ Cek console browser (F12) untuk error
→ Cek API endpoint di Network tab

**Error "Failed to fetch":**
→ Pastikan Apps Script deployed sebagai "Web App"
→ Pastikan permission "Anyone" untuk akses

---

## 📈 Kapan Pakai Versi Ini?

**Cocok untuk:**
- ✅ Internal tools yang selalu online
- ✅ Desktop-first application
- ✅ Quick prototype atau MVP
- ✅ Learning basic web development
- ✅ Deployment ke shared hosting

**Tidak cocok untuk:**
- ❌ Mobile app experience
- ❌ Offline-first application
- ❌ Need installable app
- ❌ Production app untuk client eksternal

---

## 🎓 Learning Path

**Dari versi ini, Anda bisa upgrade ke:**

1. **eTMv2** (PWA) - Jika butuh:
   - Install ke home screen
   - Offline support
   - App-like experience

2. **eTMv3** (PWA2APK) - Jika butuh:
   - Native Android APK
   - Distribusi via APK file
   - Wrap existing URL

---

## 📞 Support

**Referensi:**
- Google Apps Script: Lihat `GOOGLE_APPS_SCRIPT.md`
- Build APK: Lihat `BUILD_APK.md`
- PWA Version: Lihat `eTMv2/README.md`

---

**Built with ❤️**
- No framework
- No build tools
- Just pure HTML/CSS/JS
- Google Apps Script + Sheets

**"Simplicity is the ultimate sophistication."** - Leonardo da Vinci
