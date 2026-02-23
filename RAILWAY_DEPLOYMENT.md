# 🚀 Deployment Website Gereja ke Railway

Railway adalah platform cloud deployment yang support full-stack Next.js dengan built-in database.

## ✅ Mengapa Railway?

- ✅ **Support full-stack Next.js** (API routes, database, dll)
- ✅ **Built-in database options** (PostgreSQL, MySQL)
- ✅ **Free $5 credit** untuk mencoba
- ✅ **GitHub integration** - auto deploy
- ✅ **SSL otomatis** (HTTPS)
- ✅ **Preview deployments**
- ✅ **Metrics & monitoring**

---

## ⚠️ Catatan Penting

Biaya Railway:
- **$5 free credit** (sekali, untuk testing)
- Setelah itu: **$5/bulan** untuk web service
- Database: $5/bulan (PostgreSQL) atau $7/bulan (MySQL)

Total untuk production: **~$10-12/bulan** (~Rp 150.000-180.000)

---

## 📋 Persyaratan Sebelum Deploy

### 1. Akun Railway
1. Buka https://railway.app/
2. Sign up dengan GitHub
3. Dapatkan $5 free credit

### 2. Akun GitHub
1. Buka https://github.com/signup
2. Daftar gratis

### 3. Project sudah di GitHub (untuk auto-deploy)

---

## 🚀 Cara Deploy ke Railway (2 Pilihan)

### Metode 1: Via Railway Dashboard (Paling Mudah)

#### Step 1: Create New Project
1. Buka https://railway.app/new
2. Klik "Deploy from GitHub repo"
3. Authorize GitHub (jika belum)
4. Pilih repository `gmih-baitel-idamgamlamo`
5. Klik "Add Project"

Railway akan otomatis:
- Detect Next.js framework
- Setup build command
- Start deployment

#### Step 2: Add Database
1. Di project Railway, klik "New Service"
2. Pilih "Database"
3. Pilih database type:
   - **PostgreSQL** ($5/bulan) - RECOMMENDED
   - **MySQL** ($7/bulan)
4. Klik "Create Database"

#### Step 3: Get Database Connection String
1. Klik database service yang baru dibuat
2. Klik tab "Variables"
3. Copy `DATABASE_URL`

Format:
```
# PostgreSQL
postgresql://postgres:password@host:port/railway

# MySQL
mysql://root:password@host:port/railway
```

#### Step 4: Setup Environment Variables
1. Klik web service (Next.js app)
2. Klik tab "Variables"
3. Add variables:

```env
DATABASE_URL=postgresql://postgres:password@host:port/railway
JWT_SECRET=random-secret-key-minimal-32-karakter
```

**Generate JWT Secret yang aman:**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

#### Step 5: Redeploy
1. Klik tab "Deployments"
2. Klik "Redeploy"
3. Tunggu 2-3 menit

#### Step 6: Access Website
1. Klik tab "Domains"
2. Copy domain URL (contoh: `your-project.railway.app`)
3. Buka di browser

Website sudah live!

---

### Metode 2: Via Railway CLI

#### Step 1: Install Railway CLI
```bash
# Menggunakan npm
npm i -g @railway/cli

# Menggunakan bun
bun install -g @railway/cli
```

#### Step 2: Login
```bash
railway login
```
Ini akan buka browser untuk login

#### Step 3: Initialize Project
```bash
cd /home/z/my-project
railway init
```

#### Step 4: Create Database
```bash
railway add postgresql
# atau
railway add mysql
```

#### Step 5: Deploy
```bash
railway up
```

Railway akan:
- Upload code
- Build project
- Deploy to cloud
- Generate domain URL

---

## 🔧 Konfigurasi Database

### Update Prisma Schema

Edit `prisma/schema.prisma`:

```prisma
datasource db {
  provider = "postgresql"  // atau "mysql"
  url      = env("DATABASE_URL")
}

// Model tetap sama...
```

### Migration Database

Setelah deploy, jalankan migration:

```bash
# Connect ke Railway database
railway run prisma db push

# Atau jika sudah ada migration file
railway run prisma migrate deploy
```

---

## 📁 File `railway.json` (Opsional)

Buat file `railway.json` di root project:

```json
{
  "build": {
    "builder": "NIXPACKS"
  },
  "deploy": {
    "startCommand": "bun start",
    "healthcheckPath": "/"
  }
}
```

---

## 🔒 Custom Domain

### Domain Gratis (DuckDNS)
1. Buka https://www.duckdns.org
2. Buat subdomain
3. Di Railway: Project → Domains → Add Domain
4. Masukkan domain (contoh: `gmih-baitel.duckdns.org`)
5. Update DNS di DuckDNS dengan CNAME dari Railway

### Domain Berbayar
1. Beli domain (Namecheap, GoDaddy, dll)
2. Di Railway: Project → Domains → Add Domain
3. Masukkan domain
4. Update nameservers atau CNAME records

---

## 🔄 Update Website

### Via GitHub (RECOMMENDED)
```bash
git add .
git commit -m "Update content"
git push
```
Railway akan otomatis redeploy!

### Via Railway CLI
```bash
railway up
```

---

## 🐛 Troubleshooting

### Error 1: "Database connection failed"
**Solusi:**
- Pastikan `DATABASE_URL` di Railway Variables sudah benar
- Cek apakah database service sudah running
- Test connection string di database service tab "Connect"

### Error 2: "Build failed"
**Solusi:**
- Cek build log di Railway dashboard
- Pastikan `package.json` punya script `build`
- Jalankan `bun run build` lokal untuk test

### Error 3: "Port not found"
**Solusi:**
- Pastikan app listen di port yang di-set Railway (via env var `PORT`)
- Update code di `src/app/page.tsx`:

```typescript
const port = process.env.PORT || 3000
// App harus listen di port ini
```

### Error 4: "Application not responding"
**Solusi:**
- Cek healthcheck path di `railway.json`
- Pastikan route `/` exists
- Cek logs di Railway dashboard

---

## 📊 Monitoring & Logs

1. Buka Railway dashboard
2. Pilih project
3. Klik web service
4. Klik tab "Deployments" - lihat deployment log
5. Klik tab "Logs" - lihat runtime log
6. Klik tab "Metrics" - lihat CPU, memory, usage

---

## 💰 Biaya Railway

### Free Tier
- $5 free credit (sekali)
- Cukup untuk testing 1-2 bulan

### Production Tier
- **Web Service**: $5/bulan
- **PostgreSQL**: $5/bulan
- **MySQL**: $7/bulan
- **Custom Domain**: Gratis

Total: **$10-12/bulan** (~Rp 150.000-180.000)

### Additional Costs
- Disk Storage: $0.25/GB/bulan (setelah 1GB gratis)
- Outbound Transfer: $0.10/GB (setelah 100GB gratis)

---

## 🎯 Checklist Sebelum Deploy Production

- [ ] Akun Railway sudah dibuat
- [ ] Repository sudah di GitHub
- [ ] Database sudah di-add (PostgreSQL/MySQL)
- [ ] `DATABASE_URL` sudah diset di Railway Variables
- [ ] `JWT_SECRET` sudah diset di Railway Variables
- [ ] Run `bun run build` lokal - tidak ada error
- [ ] Test database migration
- [ ] Setup custom domain (opsional)
- [ ] Test semua features setelah deploy:
  - [ ] Homepage
  - [ ] Admin login
  - [ ] API routes
  - [ ] Database CRUD operations

---

## 📞 Support

Jika mengalami masalah:
1. Cek Railway Documentation: https://docs.railway.app
2. Cek Deployment Logs di dashboard
3. Test environment variables
4. Pastikan database accessible

---

## 🎉 Deploy Selesai!

Website Anda sekarang live di:
- **URL**: `https://your-project-name.railway.app`
- **Domain**: `https://gmih-baitel.duckdns.org` (domain gratis)
- **Domain**: `https://www.gmih-baitel.com` (domain berbayar)

**Selamat! Website GMIH Bait'el Idamgamlamo sudah online di Railway! 🎊**
