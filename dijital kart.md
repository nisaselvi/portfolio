> Şirket çalışanları için merkezi yönetilebilir dijital kartvizit sistemi. Bu projeyi sıfırdan tasarlayıp geliştirdim — mimari kararlar, veritabanı şeması, frontend ve backend dahil her katman bana ait.

**URL formatı:** `enco.com.tr/kart/ad-soyad`

---

## 🧠 Projedeki Rolüm

Bu proje tamamen benim tarafımdan sıfırdan geliştirildi:

- Sistem mimarisini tasarladım (monorepo, Next.js + Express ayrımı)
- Veritabanı şemasını ve Prisma modellerini yazdım
- Tüm backend API'yi geliştirdim (auth, employee, upload)
- Admin panelini ve çalışan profil sayfasını Next.js ile kodladım
- Cloudinary entegrasyonu ile görsel yükleme altyapısını kurdum
- QR kod üretim servisini yazdım
- Otomatik slug üretimi (`ahmet-yilmaz`) sistemi kurdum
- JWT tabanlı authentication middleware'i yazdım
- Güvenlik katmanlarını (helmet, rate limiting, dosya tipi doğrulama) ekledim

---

## 🏗 Mimari

```
┌──────────────────────────────────────────┐
│           Frontend (Next.js 14)          │
│  /admin/*          /kart/[slug]          │
│  Admin Paneli      Çalışan Profili       │
└──────────────┬───────────────────────────┘
               │ REST API
┌──────────────▼───────────────────────────┐
│         Backend (Express + TypeScript)   │
│  /api/auth   /api/employees  /api/upload │
└──────────────┬───────────────────────────┘
               │
┌──────────────▼───────────────────────────┐
│   PostgreSQL (Prisma ORM) + Cloudinary   │
└──────────────────────────────────────────┘
```

---

## ⚙️ Öne Çıkan Teknik Kararlar

**Public vs Protected route ayrımı:**
Çalışan profil sayfaları (`/kart/[slug]`) auth gerektirmez — herkes erişebilir. Admin işlemleri JWT middleware arkasında.

```typescript
router.get("/slug/:slug", getEmployeeBySlug);   // public
router.post("/", authenticate, createEmployee); // protected
```

**Cloudinary entegrasyonu:**
Dosyalar önce `/tmp`'ye yazılır, ardından Cloudinary'e yüklenir. Yerel disk bağımlılığı yok — production'da sorunsuz çalışır.

**Otomatik slug üretimi:**
Çalışan eklendiğinde `slugify` ile `Ahmet Yılmaz → ahmet-yilmaz` dönüşümü yapılır, benzersizlik kontrolü eklendi.

**QR kod servisi:**
Her çalışan için `qrcode` kütüphanesi ile profil URL'i QR koda dönüştürülür, admin panelinden indirilebilir.

**Boş alan gizleme:**
Sosyal medya linkleri gibi opsiyonel alanlar frontend'de koşullu render ile otomatik gizlenir.

---

## 🛠 Tech Stack

**Frontend**
`Next.js 14` `TypeScript` `TailwindCSS` `Zustand` `React Hook Form` `Zod` `QRCode.react` `react-dropzone` `Axios`

**Backend**
`Node.js` `Express` `TypeScript` `PostgreSQL` `Prisma ORM` `JWT` `Multer` `Sharp` `Cloudinary` `qrcode` `slugify` `Helmet` `express-rate-limit`

---

## 📡 API

| Method | Endpoint | Auth | Açıklama |
|---|---|---|---|
| POST | `/api/auth/login` | ❌ | Admin girişi |
| GET | `/api/auth/me` | ✅ | Mevcut kullanıcı |
| GET | `/api/employees` | ✅ | Tüm çalışanlar |
| POST | `/api/employees` | ✅ | Çalışan ekle |
| GET | `/api/employees/slug/:slug` | ❌ | Profil sayfası verisi |
| PUT | `/api/employees/:id` | ✅ | Çalışan güncelle |
| DELETE | `/api/employees/:id` | ✅ | Çalışan sil |
| GET | `/api/employees/:id/qr` | ✅ | QR kod |
| POST | `/api/upload/profile-photo` | ✅ | Profil fotoğrafı yükle |
| POST | `/api/upload/cover-photo` | ✅ | Kapak fotoğrafı yükle |

---

## 📁 Proje Yapısı

```
enco-digital-card/
├── frontend/
│   └── src/
│       ├── app/
│       │   ├── admin/        ← Admin paneli
│       │   └── kart/[slug]/  ← Dinamik profil sayfası
│       ├── components/
│       │   ├── admin/        ← EmployeeForm, EmployeeTable
│       │   ├── profile/      ← HeroSection, SocialLinks, QRSection
│       │   └── ui/           ← Button, Input, Modal, FileUpload
│       └── hooks/
│
└── backend/
    └── src/
        ├── controllers/
        ├── middleware/       ← auth.ts, validate.ts
        ├── routes/           ← auth, employees, upload
        ├── services/         ← employeeService, qrService
        └── prisma/           ← schema, seed
```

<img width="590" height="1060" alt="localhost_3000_kart_demo-calisan(iPhone 14 Pro Max) (1)" src="https://github.com/user-attachments/assets/9d88a9d3-42cd-446b-b438-422595c75fdd" />

---

## 🔒 Not

Kaynak kodu şirket reposunda olup kamuya açık değildir.
