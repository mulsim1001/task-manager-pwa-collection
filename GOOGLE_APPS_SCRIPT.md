# 📜 GOOGLE APPS SCRIPT - TASK MANAGER BACKEND

## 🎯 STEP-BY-STEP SETUP

### **STEP 1: Buat Google Spreadsheet**

1. Buka [Google Sheets](https://sheets.google.com)
2. Buat spreadsheet baru
3. Rename jadi: **"Task Manager Database"**

---

### **STEP 2: Setup Struktur Database**

#### **TAB 1: user**
Buat tab bernama `user` dengan struktur:

| A (username) | B (password) | C (role) |
|--------------|--------------|----------|
| mulia        | m1001        | admin    |
| arif         | a1001        | user     |
| ilham        | i1001        | user     |
| restu        | r1001        | user     |

#### **TAB 2: db01**
Buat tab bernama `db01` dengan header:

| A (id) | B (itemName) | C (pic) | D (status) | E (tsCreated) | F (tsCompleted) | G (completionNote) |
|--------|--------------|---------|------------|---------------|-----------------|---------------------|
|        |              |         |            |               |                 |                     |

*(Biarkan kosong, nanti isi via aplikasi)*

---

### **STEP 3: Buat Apps Script**

1. Di spreadsheet, klik **Extensions** → **Apps Script**
2. Hapus code default
3. Copy-paste code berikut:

```javascript
// ================================================================
// TASK MANAGER - GOOGLE APPS SCRIPT BACKEND
// File: Code.gs
// ================================================================

const SHEET_ID = SpreadsheetApp.getActiveSpreadsheet();

// -------------------- HELPERS --------------------
function getSheet(name) {
  return SHEET_ID.getSheetByName(name);
}

function success(data) {
  return ContentService.createTextOutput(JSON.stringify({
    success: true,
    data: data
  })).setMimeType(ContentService.MimeType.JSON);
}

function error(message) {
  return ContentService.createTextOutput(JSON.stringify({
    success: false,
    error: message
  })).setMimeType(ContentService.MimeType.JSON);
}

function generateId() {
  const num = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
  const char1 = String.fromCharCode(65 + Math.floor(Math.random() * 26));
  const char2 = String.fromCharCode(65 + Math.floor(Math.random() * 26));
  return num + char1 + char2;
}

// -------------------- AUTH --------------------
function validateUser(username, password) {
  const sheet = getSheet('user');
  if (!sheet) return false;
  
  const data = sheet.getDataRange().getValues();
  
  for (let i = 1; i < data.length; i++) {
    if (data[i][0] === username && data[i][1] === password) {
      return true;
    }
  }
  return false;
}

// -------------------- CRUD --------------------
function getTasks() {
  const sheet = getSheet('db01');
  if (!sheet) return [];
  
  const data = sheet.getRange('A2:G').getValues();
  
  return data
    .filter(row => row[0])
    .map(row => ({
      id: row[0],
      itemName: row[1],
      pic: row[2],
      status: row[3],
      tsCreated: row[4],
      tsCompleted: row[5] || null,
      completionNote: row[6] || null
    }));
}

function addTask(itemName, username) {
  const sheet = getSheet('db01');
  if (!sheet) throw new Error('Sheet db01 not found');
  
  const id = generateId();
  const now = new Date().toISOString();
  
  sheet.appendRow([id, itemName, username, 'onProgress', now, '', '']);
  
  return {
    id: id,
    itemName: itemName,
    pic: username,
    status: 'onProgress',
    tsCreated: now
  };
}

function completeTask(id, note, username) {
  const sheet = getSheet('db01');
  if (!sheet) return false;
  
  const data = sheet.getDataRange().getValues();
  
  for (let i = 1; i < data.length; i++) {
    if (data[i][0] === id) {
      sheet.getRange(i + 1, 4).setValue('done');
      sheet.getRange(i + 1, 6).setValue(new Date().toISOString());
      sheet.getRange(i + 1, 7).setValue(note || '');
      return true;
    }
  }
  return false;
}

function deleteTask(id) {
  const sheet = getSheet('db01');
  if (!sheet) return false;
  
  const data = sheet.getDataRange().getValues();
  
  for (let i = 1; i < data.length; i++) {
    if (data[i][0] === id) {
      sheet.deleteRow(i + 1);
      return true;
    }
  }
  return false;
}

function checkOverdue() {
  const sheet = getSheet('db01');
  if (!sheet) return;
  
  const data = sheet.getDataRange().getValues();
  const now = new Date();
  
  for (let i = 1; i < data.length; i++) {
    if (data[i][3] === 'onProgress') {
      const created = new Date(data[i][4]);
      const hoursDiff = (now - created) / (1000 * 60 * 60);
      
      if (hoursDiff > 24) {
        sheet.getRange(i + 1, 4).setValue('overdue');
      }
    }
  }
}

// -------------------- MAIN ENDPOINT --------------------
function doPost(e) {
  try {
    const params = JSON.parse(e.postData.contents);
    
    if (!validateUser(params.username, params.password)) {
      return error('Invalid credentials');
    }
    
    switch(params.action) {
      case 'login':
        return success({ message: 'Login success' });
        
      case 'getDashboard':
        checkOverdue();
        return success(getTasks());
        
      case 'addTask':
        return success(addTask(params.itemName, params.username));
        
      case 'completeTask':
        completeTask(params.id, params.note, params.username);
        return success({ message: 'Task completed' });
        
      case 'deleteTask':
        deleteTask(params.id);
        return success({ message: 'Task deleted' });
        
      default:
        return error('Unknown action');
    }
  } catch (err) {
    return error(err.toString());
  }
}

function doGet(e) {
  return ContentService.createTextOutput('Task Manager API is running! ✅');
}
```

4. **Save** (Ctrl+S atau icon disket)
5. Rename project jadi: **"Task Manager API"**

---

### **STEP 4: Deploy sebagai Web App**

1. Klik **Deploy** → **New deployment**
2. Klik ⚙️ (gear icon) → Pilih **Web app**
3. Settings:
   - **Description:** Task Manager API v1
   - **Execute as:** Me (your@email.com)
   - **Who has access:** Anyone
4. Klik **Deploy**
5. **COPY URL** yang muncul (penting!)
   - Format: `https://script.google.com/macros/s/AKfycby.../exec`

---

### **STEP 5: Update Frontend**

1. Buka file: **`public/index.html`**
2. Cari baris: `const API_URL = 'YOUR_APPS_SCRIPT_URL_HERE';`
3. Ganti dengan URL dari Step 4
4. Save

5. Buka file: **`public/dashboard.html`**
6. Cari baris: `const API_URL = 'YOUR_APPS_SCRIPT_URL_HERE';`
7. Ganti dengan URL yang sama
8. Save

---

### **STEP 6: Testing**

1. Buka `public/index.html` di browser (atau langsung buka `http://localhost:5000/`)
2. Login dengan:
   - Username: `mulia`
   - Password: `m1001`
3. Coba add task
4. Coba complete task (klik status)
5. Coba delete task

---

## ✅ CHECKLIST SETUP

- [ ] Spreadsheet dibuat
- [ ] Tab `user` ada dan terisi 4 user
- [ ] Tab `db01` ada dengan header
- [ ] Apps Script di-paste dan save
- [ ] Deploy sebagai Web App
- [ ] URL di-copy
- [ ] URL dipaste di `index.html` (baris 135)
- [ ] URL dipaste di `dashboard.html` (baris 261)
- [ ] Test login berhasil
- [ ] Test CRUD berhasil

---

## 🐛 TROUBLESHOOTING

**Error: "Invalid credentials"**
→ Cek data di tab `user`, pastikan username & password sesuai

**Error: "Sheet not found"**
→ Pastikan tab bernama persis `user` dan `db01`

**Error: "Failed to fetch"**
→ Pastikan URL Apps Script sudah benar di kedua file HTML

**Task tidak muncul**
→ Cek tab `db01`, pastikan ada data. Atau coba add task dulu.

**Status overdue tidak update**
→ Normal, hanya update saat load dashboard (setiap 30 detik auto refresh)

---

## 📊 QUOTA & LIMITS

- **Free tier:** 20,000 calls/day
- **Response time:** ~300-500ms
- **Max data:** Sheets support jutaan rows (cukup!)
- **Concurrent users:** No limit (stateless)

---

## 🚀 DONE!

Aplikasi Task Manager Anda sudah jalan!

**Minimal Files:**
- 2 HTML (login + dashboard)
- 1 Apps Script (backend)
- 1 Spreadsheet (database)

**Total: 4 files** ✅

Selamat! Anda baru saja bikin aplikasi tanpa jadi programmer! 🎉
