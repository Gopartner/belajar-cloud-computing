**“Integrasi OAuth 2.0 Google Login pada Web App dengan React + TypeScript + Firebase”**

# 🔑 Integrasi OAuth 2.0 Google Login pada Web App dengan React + TypeScript + Firebase

---

## **1. Setup Project Vite + React + TypeScript** skip step ini jika project sudah ready.

```bash
# Buat project baru
npm create vite@latest my-app --template react-ts
cd my-app
npm install
# Install Firebase dan axios untuk HTTP request (optional)
npm install firebase axios
```

---

## **2. Buat Project di Firebase**

1. Buka [Firebase Console](https://console.firebase.google.com/) → klik **Add Project**.
2. Aktifkan **Authentication → Sign-in Method → Google**.
3. Aktifkan **Realtime Database** dan **Firestore**.
4. Catat **Web Client ID** dan informasi konfigurasi Firebase (API Key, Auth Domain, dsb).

---

## **3. Buat File .env**

Tambahkan di root project:

```env
VITE_GOOGLE_CLIENT_ID=your_google_client_id.apps.googleusercontent.com
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_DATABASE_URL=https://your_project.firebaseio.com
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
```

> ⚠️ Vite hanya bisa membaca variabel yang diawali `VITE_`.

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

## **5. Buat Komponen Login**

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

## **6. Gunakan Komponen di App**

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

## **7. Deploy ke Firebase Hosting**

```bash
# Install Firebase CLI
npm install -g firebase-tools
firebase login
firebase init
# Pilih Hosting → gunakan build output Vite: dist
npm run build
firebase deploy
```

---

## **8. Integrasi Realtime Database / Firestore**

Simpan data user setelah login:

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

---

## **9. Flow Lengkap**

1. User klik **Login with Google**
2. Firebase membuka popup Google Login
3. User login → Firebase mengembalikan user info
4. Simpan data user ke Realtime Database / Firestore
5. User sudah authenticated → bisa akses konten aplikasi
