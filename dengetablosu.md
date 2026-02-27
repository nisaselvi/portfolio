# 📊 Denge Tablosu — Finansal Yönetim Paneli

> Şirket içi finansal verileri merkezi bir arayüzde yönetmek ve yapay zeka destekli analiz sunmak amacıyla geliştirilmiş full-stack bir iç yönetim sistemidir.

---

## 🧠 Projedeki Rolüm

Bu proje ekip tarafından geliştirilmiştir. Ben projeye **AI destekli chatbot modülünü** entegre ettim:

- Mevcut backend mimarisini inceleyerek chat route'unu tasarladım
- OpenAI GPT-3.5 Turbo entegrasyonunu gerçekleştirdim
- Chatbot'u veritabanına bağladım — her konuşmada canlı finansal veriler çekilerek yanıt üretiliyor
- React ile sohbet arayüzü (ChatWidget) bileşenini geliştirdim
- Hata yönetimi ve API kota kontrollerini yazdım

---

## 🤖 Geliştirdiğim: AI Finansal Asistan

Sistem şu şekilde çalışıyor:

1. Kullanıcı chatbot'a soru sorar (örn. *"Bu ay ne kadar kâr ettik?"*)
2. Backend, MySQL veritabanından **anlık** gelir/gider/müşteri verilerini çeker
3. Bu veriler OpenAI'ye sistem prompt olarak iletilir
4. GPT-3.5, şirketin gerçek finansal durumuna göre Türkçe yanıt üretir
5. Cevap kullanıcıya iletilir

```
Kullanıcı → React ChatWidget → Express /api/chat → MySQL (canlı veri) → OpenAI GPT → Yanıt
```

---

## 🛠 Kullanılan Teknolojiler

**Frontend**
- React 18 · TypeScript · Vite
- TailwindCSS · shadcn/ui · Radix UI
- Recharts (grafik ve raporlar)
- lucide-react

**Backend**
- Node.js · Express
- MySQL2 · dotenv · helmet · cors · compression

**AI & Entegrasyon**
- OpenAI API (GPT-3.5 Turbo)
- Canlı veritabanı → prompt injection mimarisi

---

## 📁 Projenin Genel Yapısı

Projenin ana modülleri:

| Modül | Açıklama |
|---|---|
| Dashboard (One-Pager) | Genel finansal özet |
| Gelir / Gider Yönetimi | CRUD + tarih/kategori filtreleri |
| Müşteri & Firma Takibi | Aktif müşteri ve sözleşme yönetimi |
| İK & Personel | Şifreli erişimle maaş ve personel takibi |
| Yatırımcı Modülü | Yatırımcı kayıt ve takip sistemi |
| Raporlar & Dönem Analizi | PDF export destekli finansal raporlama |
| 🤖 AI Chatbot | **Benim geliştirdiğim modül** |

---

## ⚙️ API Mimarisinden Notlar

Projeyi incelerken öğrendiklerim:

- `exponential backoff` ile rate limit yönetimi (429 handling)
- `snake_case → camelCase` dönüşüm katmanı (DB ↔ Frontend uyumu)
- Şifre korumalı endpoint'ler (`x-ik-password`, `x-yatirimci-password` header'ları)
- ES Module yapısı (`"type": "module"`) ile Node.js backend
- Helmet + CORS + Compression middleware zinciri

---

<img width="502" height="307" alt="image" src="https://github.com/user-attachments/assets/5a38eb30-299c-479e-aa4e-5ebcf94174a3" />

## 🔒 Not

Bu proje şirket içi kullanım için geliştirilmiş olup kaynak kodu kamuya açık değildir.
