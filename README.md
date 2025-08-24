# Belajar Cloud Computing

Diagram arsitektur:

![Diagram Cloud Computing](https://raw.githubusercontent.com/Gopartner/belajar-cloud-computing/refs/heads/Master/images/diagram.png)

---

## **1. Model Layanan Cloud (Service Models)**

1. **IaaS – Infrastructure as a Service**

   - **Fungsi:** Menyediakan infrastruktur virtual (server, storage, jaringan) yang bisa dikontrol penuh.
   - **Contoh:** GCP Compute Engine, AWS EC2, Azure Virtual Machines
   - **Implementasi Fullstack:**

     - Deploy backend Node.js / Python di VPS virtual
     - Pasang database MySQL/PostgreSQL di server virtual

2. **PaaS – Platform as a Service**

   - **Fungsi:** Platform untuk deploy aplikasi tanpa harus mengatur server. Fokus ke aplikasi dan kode saja.
   - **Contoh:** GCP App Engine, Cloud Run, AWS Elastic Beanstalk
   - **Implementasi Fullstack:**

     - Deploy aplikasi React + Node.js tanpa setting OS atau server
     - Auto scaling sesuai trafik

3. **SaaS – Software as a Service**

   - **Fungsi:** Software siap pakai via internet, pengguna tidak mengelola server atau aplikasi.
   - **Contoh:** Gmail, Google Docs, Slack
   - **Implementasi Fullstack:**

     - Integrasi aplikasi dengan layanan SaaS, misal login Google OAuth, Google Calendar API

---

## **2. Deployment & Compute Resources**

- **Virtual Machines / VPS** → server virtual untuk deploy aplikasi
- **Containers** → Docker container, dijalankan di Cloud Run / Kubernetes
- **Serverless Functions** → fungsi kecil yang berjalan hanya saat dipanggil, contohnya Cloud Functions

---

## **3. Storage & Database**

- **File Storage** → Cloud Storage (GCP), S3 (AWS)
- **Relational Database** → Cloud SQL, Aurora, PostgreSQL/MySQL managed
- **NoSQL Database** → Firestore, DynamoDB, MongoDB Atlas
- **Data Warehouse** → BigQuery, Redshift

---

## **4. Networking & Security**

- **Virtual Private Cloud (VPC)** → jaringan virtual di cloud
- **Firewall & Security Rules** → atur akses server dan aplikasi
- **Load Balancer** → distribusi trafik aplikasi agar stabil
- **DNS & CDN** → Cloud DNS, Cloud CDN untuk performa dan akses global

---

## **5. Identity & Authentication**

- **IAM (Identity & Access Management)** → mengatur siapa dapat akses resource
- **API Key / OAuth / Service Account** → akses aman dari aplikasi atau server
- [Lanjut: Identity & Authentication](Identity%26Authentication.md)

---

## **6. Monitoring & DevOps**

- **Monitoring** → Cloud Monitoring, Cloud Logging
- **CI/CD Pipelines** → Cloud Build, GitHub Actions integration
- **Error Reporting / Alerts** → notif kalau ada error di aplikasi
- [Lanjut: Monitoring & DevOps](Monitoring%26DevOps.md)

---

## **7. Advanced & Specialized Services**

- **Messaging & Event-driven** → Pub/Sub, EventBridge
- **AI/ML Services** → Vertex AI, TensorFlow di Cloud
- **Big Data & Analytics** → BigQuery, Dataflow

---

Kalau disederhanakan, **Cloud Computing itu terdiri dari:**

1. **Service Models** → IaaS, PaaS, SaaS
2. **Compute Resources** → VM, container, serverless
3. **Storage & Database**
4. **Networking & Security**
5. **Identity & Authentication**
6. **Monitoring & DevOps**
7. **Advanced Services** → AI, Analytics, Messaging

---
