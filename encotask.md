# ✅ EncoTask — Proje Yönetim Uygulaması

> Takımlar için geliştirilmiş kapsamlı bir proje yönetim aracı. Kanban board, döküman editörü, mind map, kapasite planlaması ve rol tabanlı yetkilendirme içerir.

---

## 🧠 Projedeki Rolüm
Proje mevcut bir kurulum üzerine geliştirildi. Benim katkılarım:

Kanban board yapısı — liste ve kart mimarisi, @dnd-kit ile sürükle-bırak
Kapasite Planlaması modülü — sıfırdan tasarladım ve geliştirdim
Rol tabanlı yetkilendirme sistemi — middleware mimarisi, oda erişim kontrolü
Sidebar — navigasyon yapısı ve sayfa geçişleri
Dark mode — uygulama genelinde karanlık tema desteği
UI/UX yeniden tasarımı — genel arayüz iyileştirmeleri ve bileşen tasarımı
Dosya yükleme altyapısı — Multer ile güvenli upload middleware

---

## 📊 Geliştirdiğim: Kapasite Planlaması Modülü

Tüm personelin üzerindeki görev yükünü tek ekranda görselleştiren yönetici modülü.

**Nasıl çalışır:**
- `/capacity/workload` endpoint'i tüm kullanıcıların aktif görevlerini çeker
- `WorkloadOverview` — özet istatistik kartları
- `PersonnelList` — tüm çalışanlar ve iş yükleri
- `PersonnelDetail` — kişi bazlı görev dökümü (modal)
- `CapacitySettings` — yönetici ataması (sadece primary manager erişebilir)

**Yetki katmanı:** `isPrimary` kontrolü ile sadece yetkili yöneticiler ayarlar ekranına erişebilir.

---

## 🔐 Yetkilendirme Sistemi

Üç katmanlı middleware mimarisi kurdum:

| Middleware | Açıklama |
|---|---|
| `authenticateToken` | JWT doğrulama — her korumalı route'da çalışır |
| `isAdmin` | Role kontrolü — sadece `admin` rolüne izin verir |
| `checkRoomAccess` | Oda bazlı erişim — kullanıcı oda sahibi mi veya üye mi kontrol eder |

```
İstek → authenticateToken → isAdmin / checkRoomAccess → Route Handler
```

Oda erişim kontrolü DB'den dinamik sorguyla çalışır — hem `owner_id` hem `room_members` tablosu kontrol edilir.

---

## 🗂 Mimari

```
encotask/
├── client/          ← React 18 + Vite frontend
│   ├── components/capacity/   ← Benim yazdığım modül
│   ├── context/               ← CapacityContext
│   └── ...
├── server/          ← Express backend
│   ├── middleware/  ← auth.js, upload.js (benim yazdığım)
│   ├── routes/
│   └── db/
```

---

## 🛠 Tech Stack

**Frontend**
`React 18` `Vite` `TypeScript` `TailwindCSS` `React Router v6` `@dnd-kit` (drag & drop) `Tiptap` (rich text editor) `ReactFlow` (mind map) `Recharts` `lucide-react`

**Backend**
`Node.js` `Express` `MySQL2` `JWT` `Multer` `Helmet` `express-rate-limit` `Nodemailer`

**Export**
`jsPDF` `xlsx` `file-saver`

---

## ⚙️ Öne Çıkan Teknik Detaylar

- **@dnd-kit** ile sürükle-bırak Kanban board
- **Tiptap** ile zengin metin editörü (görev dökümanları)
- **ReactFlow** ile interaktif mind map görselleştirme
- **Rate limiting** — genel 500 istek/15dk, auth için 20/15dk
- **Multer** ile 10MB limit ve dosya tipi doğrulamalı güvenli upload
- **Parametreli SQL sorguları** ile injection koruması
- **PDF + Excel export** desteği

---

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d3d068cc-df36-45cd-ae7b-cf79cf51ea84" />

## 🔒 Not

Kaynak kodu şirket reposunda olup kamuya açık değildir.
