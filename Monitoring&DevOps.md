# 📊 Monitoring & DevOps pada Cloud Projects

---

## **1. Apa itu DevOps dan Monitoring?**

- **DevOps** → budaya + praktik untuk **menyatukan development dan operations**, supaya pengembangan software lebih cepat, stabil, dan bisa di-deploy otomatis.
- **Monitoring** → proses **mengamati performa, ketersediaan, dan kesehatan aplikasi atau infrastruktur**, agar masalah bisa dideteksi cepat.

---

## **2. Komponen Penting DevOps**

1. **CI/CD (Continuous Integration / Continuous Deployment)**

   - CI → otomatisasi build & testing setiap ada perubahan kode.
   - CD → otomatisasi deployment ke staging / production.
   - Tools populer: GitHub Actions, GitLab CI, Jenkins, CircleCI.

2. **Infrastructure as Code (IaC)**

   - Mengelola infrastruktur dengan **kode**, bukan manual.
   - Tools: Terraform, Pulumi, Ansible.

3. **Version Control**

   - Semua kode & konfigurasi disimpan di repository (Git).
   - Memudahkan rollback, branch, dan kolaborasi.

4. **Containerization & Orchestration**

   - Docker → membuat environment konsisten.
   - Kubernetes → mengatur container agar scalable & resilient.

---

## **3. Monitoring**

Monitoring bertujuan **mendeteksi dan mencegah masalah sebelum berdampak besar**.

### **3.1 Jenis Monitoring**

1. **Infrastructure Monitoring**

   - Pantau CPU, memory, disk, network.
   - Tools: Prometheus + Grafana, Cloud Monitoring (GCP), CloudWatch (AWS).

2. **Application Performance Monitoring (APM)**

   - Pantau performa aplikasi: response time, error rate, throughput.
   - Tools: New Relic, Datadog, Elastic APM.

3. **Logging & Alerting**

   - Kumpulkan log dari server/app → analisis → buat alert jika ada anomali.
   - Tools: ELK Stack (Elasticsearch, Logstash, Kibana), Loki, Firebase Crashlytics (untuk mobile/web).

---

## **4. Step-by-Step Implementasi Monitoring & DevOps (Contoh Cloud Web App)**

### **Step 1: Setup Repository & CI/CD**

1. Buat repository GitHub → push code.
2. Buat workflow GitHub Actions:

```yaml
name: CI/CD

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: "18"
      - name: Install dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy to Firebase
        uses: w9jds/firebase-action@v2.2.0
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

---

### **Step 2: Infrastruktur dengan IaC**

- Contoh: Terraform untuk provisioning Firebase Hosting / Database

```hcl
provider "google" {
  project = "my-firebase-project"
  region  = "us-central1"
}

resource "google_firebase_project" "app" {
  project = "my-firebase-project"
}
```

---

### **Step 3: Containerization (Opsional)**

- Gunakan Docker untuk environment konsisten:

```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 5173
CMD ["npm","run","preview"]
```

---

### **Step 4: Monitoring & Logging**

1. Tambahkan **performance monitoring** (misal Firebase Performance Monitoring atau Grafana).
2. Tambahkan **error logging** (misal Sentry, Firebase Crashlytics).
3. Buat **alert** untuk CPU, memory, atau error rate tinggi.

---

### **Step 5: Dashboard Monitoring**

- Buat dashboard untuk pantau:

  - Traffic user
  - Response time endpoint
  - Error rate
  - Database read/write

- Tools populer: Grafana, Kibana, Firebase Console, Datadog.

---

## **5. Flow DevOps & Monitoring**

```text
[Developer] --> Push Code --> [CI/CD Pipeline] --> Build/Test --> Deploy --> [App & Infrastructure]
                                                            |
                                                            v
                                                   [Monitoring & Logging] --> Alerts
```

- Setiap push ke repository → pipeline otomatis test → build → deploy.
- Monitoring menampilkan health & performance → alert jika ada masalah.
