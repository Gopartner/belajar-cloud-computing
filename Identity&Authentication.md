**Identity & Authentication**

---

# 🛡 Identity & Authentication

## 1. **IAM (Identity & Access Management)**

IAM adalah sistem untuk **mengatur siapa bisa melakukan apa pada resource mana** di cloud.
Biasanya terdiri dari:

- **Identity (Siapa)** → user, grup, service account.
- **Roles (Apa yang bisa dilakukan)** → permission yang diberikan.
- **Policies (Aturan)** → menghubungkan identity dengan roles pada resource tertentu.

🔑 Contoh:

- `User A` → bisa **read/write** ke database.
- `Service Account B` → hanya bisa **read** storage bucket.

---

## 2. **Authentication vs Authorization**

- **Authentication (AuthN)** → verifikasi identitas (apakah benar ini adalah User X?).
- **Authorization (AuthZ)** → verifikasi izin (bolehkah User X melakukan aksi Y di resource Z?).

---

## 3. **Metode Akses Umum**

### 🔹 API Key

- String unik (secret) yang diberikan untuk mengakses API.
- **Kelebihan**: mudah digunakan.
- **Kekurangan**: kurang aman (mudah dicuri kalau tidak disimpan dengan benar).

### 🔹 OAuth 2.0

- Protokol standar untuk login dengan pihak ketiga (Google, GitHub, Facebook, dsb).
- **Flow umum**:

  1. User klik “Login with Google”
  2. Redirect ke Google untuk verifikasi
  3. Jika sukses → dapatkan **Access Token**
  4. Token digunakan untuk akses API

👉 Biasanya dipakai untuk aplikasi web/mobile.

- [Integrasi OAuth 2.0 Google Login pada Web App dengan React + TypeScript + Firebase](Integrasi-OAuth-2.0-Google-Login.md)

### 🔹 Service Account

- Identitas khusus untuk **aplikasi atau server**, bukan user manusia.
- Biasanya digunakan di **backend, CI/CD, atau microservices**.
- Mendapatkan **JSON key file** untuk autentikasi otomatis.

---

## 4. Ilustrasi Sederhana

```text
[User/Service] --(Authentication)--> [IAM] --(Authorization)--> [Resource]
```

- User login (OAuth, API key, Service Account)
- IAM cek siapa + role apa yang dimiliki
- Jika valid → akses resource

---
