# 🤖 ChatbotEnco — AI Chatbot Platform

> Şirketlerin kendi web sitelerine kolayca entegre edebildiği, bilgi tabanı yönetimli, canlı destek destekli kurumsal chatbot platformu.

**Production:** [encobot.lojistikportal.com](https://encobot.lojistikportal.com)

---

## 🧠 Projedeki Rolüm

Bu proje iki versiyondan oluşuyor. **v2.0.0'ı tamamen ben geliştirdim.**

V2 kapsamında sıfırdan yazdıklarım:

- **Semantik arama motoru** — pgvector + Google Gemini embedding entegrasyonu, hibrit arama mimarisi
- **Canlı destek sistemi** — AI → insan temsilci geçişi, durum makinesi tasarımı
- **WebSocket altyapısı** — Socket.io ile gerçek zamanlı iletişim (polling'den migrate)
- **Tarayıcı push bildirimleri** — Web Notifications API entegrasyonu
- **Konuşma yönetimi** — Arama, filtreleme, arşivleme, transcript modal
- **Tekrar eden içerik önleme** — Duplicate dosya/URL kontrolü
- **Hazır yanıtlar** — Admin için quick reply sistemi
- **"Yazıyor..." göstergesi** — WebSocket üzerinden anlık iletişim

---

## ⚙️ Mimari

```
Kullanıcı (Widget) ←→ Socket.io ←→ NestJS Backend ←→ PostgreSQL + pgvector
                                           ↕
                                    Google Gemini AI
                                           ↕
                               Admin Panel (Next.js 14)
```

**Monorepo yapısı:**
```
chatbot-monorepo/
├── backend/    ← NestJS API + WebSocket Gateway
├── admin/      ← Next.js 14 Admin Paneli
└── package.json ← Concurrently ile ortak script'ler
```

---

## 🧠 Semantik Arama — Nasıl Çalışır?

V2'nin teknik açıdan en kritik özelliği. Klasik anahtar kelime aramasının yetersiz kaldığı durumlarda anlamsal yakınlığa göre arama yapıyor.

1. Belge yüklendiğinde her chunk için Gemini `text-embedding-004` modeline istek atılır → 768 boyutlu vektör üretilir
2. Vektör PostgreSQL'deki `document_chunks.embedding` kolonuna kaydedilir
3. Kullanıcı soru sorduğunda soru da vektöre çevrilir
4. **Hibrit skor** hesaplanır:

| Yöntem | Ağırlık | Teknoloji |
|---|---|---|
| Anahtar kelime eşleşmesi | %40 | PostgreSQL `tsvector` / `ts_rank` |
| Semantik benzerlik | %60 | pgvector `<=>` kosinüs operatörü |

5. En yüksek skorlu chunk'lar Gemini'ye bağlam olarak iletilir

---

## 🎧 Canlı Destek — Durum Makinesi

Her konuşma şu durumlardan birinde bulunur:

```
active → live_requested → live_agent → closed
  ↑____________________________________________|  (yeni konuşma)
```

| Durum | Açıklama |
|---|---|
| `active` | AI yanıtlıyor |
| `live_requested` | Kullanıcı temsilci talep etti, admin'e bildirim gitti |
| `live_agent` | Admin devraldı, AI devre dışı |
| `closed` | Konuşma kapatıldı |

---

## ⚡ WebSocket Events

| Event | Yön | Açıklama |
|---|---|---|
| `join_session` | Client → Server | Konuşma odasına katıl |
| `typing` | Client → Server | Yazıyor bildirimi |
| `new_message` | Server → Client | Yeni mesaj |
| `session_update` | Server → Client | Durum değişikliği |
| `live_request` | Server → Client | Yeni canlı destek talebi |

---

## 🛠 Tech Stack

**Backend**
`NestJS` `TypeORM` `PostgreSQL` `pgvector` `Socket.io` `Google Gemini AI` `Google Drive API` `Cheerio`

**Frontend (Admin)**
`Next.js 14` `TypeScript` `TailwindCSS` `Socket.io-client` `Web Notifications API`

**DevOps**
`PM2` `Nginx` `Monorepo` `concurrently`

---

## 📊 API — Ana Endpoint'ler

| Grup | Endpoint | Açıklama |
|---|---|---|
| Bots | `GET /bots` | Bot listesi |
| Knowledge | `POST /knowledge/upload/:botId` | Dosya yükle |
| Knowledge | `POST /knowledge/discover` | Website link keşfi |
| Knowledge | `POST /knowledge/scrape-batch/:botId` | Toplu scraping |
| Chat | `POST /chat/session` | Yeni session |
| Chat | `POST /chat/message` | Mesaj gönder |
| Widget | `GET /widget.js` | Embed script |

---
<img width="1919" height="747" alt="Image" src="https://github.com/user-attachments/assets/753828fa-19d5-483b-9909-6289bc78f556" />

## 🔒 Not

Kaynak kodu şirket reposunda olup kamuya açık değildir.
