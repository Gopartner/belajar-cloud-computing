Oke! Berikut versi **README lengkap step-by-step** untuk membuat **app baru dalam project Firebase yang sudah ada**, menggunakan **React + TypeScript + OAuth 2.0 Google Login + Firebase Hosting**. Bisa langsung kamu pakai di GitHub dan untuk presentasi.

---

# 📌 Integrasi OAuth 2.0 Google Login pada App Baru di Project Firebase yang Sudah Ada (React + TypeScript + Firebase)

---

## **1. Setup Project React + Vite + TypeScript**

```bash
# Buat project baru
npm create vite@latest my-web-app --template react-ts
cd my-web-app
npm install
# Install Firebase dan axios
npm install firebase axios
```

---

## **2. Tambahkan App Baru di Project Firebase yang Sudah Ada**

1. Masuk ke [Firebase Console](https://console.firebase.google.com/).
2. Pilih project yang sudah ada.
3. Klik **Add App → Web App**
4. Masukkan nama app baru (misal `my-web-app`)
5. Firebase akan memberikan **config snippet**:

```javascript
const firebaseConfig = {
  apiKey: "API_KEY",
  authDomain: "PROJECT_ID.firebaseapp.com",
  projectId: "PROJECT_ID",
  storageBucket: "PROJECT_ID.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID",
};
```

6. Salin config ini untuk langkah berikutnya.

---

## **3. Buat File .env**

Tambahkan di root project:

```env
VITE_GOOGLE_CLIENT_ID=your_google_client_id.apps.googleusercontent.com
VITE_FIREBASE_API_KEY=API_KEY
VITE_FIREBASE_AUTH_DOMAIN=PROJECT_ID.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=PROJECT_ID
VITE_FIREBASE_DATABASE_URL=https://PROJECT_ID.firebaseio.com
VITE_FIREBASE_STORAGE_BUCKET=PROJECT_ID.appspot.com
```

> ⚠️ Variabel environment di Vite harus diawali `VITE_`.

---

## **4. Konfigurasi Firebase di React**

`src/firebase.ts`:

```ts
import { initializeApp } from "firebase/app";
import { getAuth, GoogleAuthProvider } from "firebase/auth";
import { getDatabase } from "firebase/database";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  databaseURL: import.meta.env.VITE_FIREBASE_DATABASE_URL,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
};

const app = initializeApp(firebaseConfig);

export const auth = getAuth(app);
export const provider = new GoogleAuthProvider();
export const realtimeDB = getDatabase(app);
export const firestoreDB = getFirestore(app);
```

---

## **5. Buat Komponen Login dengan Google**

`src/components/Login.tsx`:

```tsx
import React from "react";
import { auth, provider } from "../firebase";
import { signInWithPopup } from "firebase/auth";

const Login: React.FC = () => {
  const handleLogin = async () => {
    try {
      const result = await signInWithPopup(auth, provider);
      const user = result.user;
      console.log("User Info:", user);
      alert(`Hello, ${user.displayName}`);
    } catch (err) {
      console.error(err);
      alert("Login gagal!");
    }
  };

  return (
    <button onClick={handleLogin} className="btn">
      Login with Google
    </button>
  );
};

export default Login;
```

---

## **6. Gunakan Komponen Login di App**

`src/App.tsx`:

```tsx
import React from "react";
import Login from "./components/Login";

function App() {
  return (
    <div className="App">
      <h1>OAuth 2.0 Login Google</h1>
      <Login />
    </div>
  );
}

export default App;
```

---

## **7. Integrasi Realtime Database / Firestore**

Setelah login, simpan data user ke database yang sudah ada:

**Realtime Database:**

```ts
import { ref, set } from "firebase/database";

const userRef = ref(realtimeDB, "users/" + user.uid);
set(userRef, { name: user.displayName, email: user.email });
```

**Firestore Database:**

```ts
import { doc, setDoc } from "firebase/firestore";

await setDoc(doc(firestoreDB, "users", user.uid), {
  name: user.displayName,
  email: user.email,
});
```

> Dengan cara ini, semua app dalam project yang sama bisa berbagi data user jika diperlukan.

---

## **8. Deploy App Baru ke Firebase Hosting**

1. Install Firebase CLI:

```bash
npm install -g firebase-tools
firebase login
```

2. Init Hosting untuk app baru:

```bash
firebase init hosting
# pilih project yang sudah ada
# tentukan folder build output (Vite: dist)
```

3. Build project:

```bash
npm run build
```

4. Deploy:

```bash
firebase deploy
```

---

## **9. Flow Lengkap OAuth 2.0**

1. User klik **Login with Google**
2. Firebase popup login → Google Auth
3. User login → Firebase mengembalikan **user info**
4. Simpan data user ke Realtime Database / Firestore
5. User authenticated → bisa akses konten aplikasi
