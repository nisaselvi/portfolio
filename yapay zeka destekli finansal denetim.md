# 🧾 AIFO — Yapay Zeka Destekli Finansal Denetim

> Excel/CSV finansal tablolarını okuyup kalemleri otomatik eşleştiren, KPI'ları deterministik formüllerle hesaplayan ve risk skorlaması yapan yerel-öncelikli finansal analiz uygulaması.

---

## 🧠 Projedeki Rolüm

Proje mevcut bir altyapı üzerine geliştirildi. v1.0 kapsamında benim katkılarım:

- **Hata loglama modülü** (`src/lib/monitoring.ts`) — sıfırdan yazdım
- **PDF export yeniden yazımı** — görüntü tabanlı export yerine tarayıcı print API'sine geçiş
- **GitHub Actions CI pipeline** — lint → test → build kalite kapısı
- **Vendor chunk optimizasyonu** — `vite.config.ts` ile bundle boyutu azaltımı
- **macOS artefact izolasyonu** — `__MACOSX/**` lint akışından dışlandı
- **README ve komut akışı standardizasyonu**
- **Dynamic/static import çakışması giderimi** — storage modülündeki kritik hata

---

## 📦 Geliştirdiğim: Monitoring Modülü

Uygulama genelinde kullanılan merkezi loglama sistemi.

**Tasarım kararları:**
- `domain` + `code` + `level` üçlüsüyle yapılandırılmış log formatı — log'ları filtrelenebilir kılar
- `navigator.sendBeacon` öncelikli gönderim — sayfa kapanırken bile log kaybolmaz
- `fetch` ile fallback — beacon desteklenmeyen ortamlar için
- Remote endpoint opsiyonel (`VITE_LOG_ENDPOINT`) — tanımlanmamışsa sadece console'a yazar
- Hata normalizasyonu — `Error`, `object`, `string` tiplerini tutarlı formata çevirir
- **Log gönderimi hiçbir zaman uygulama akışını bozmaz** — try/catch ile sessizce başarısız olur

```typescript
// Kullanım
logError({ domain: 'parser', code: 'PARSE_FAIL', message: '...', error })
logWarn({ domain: 'export', code: 'PDF_TIMEOUT', message: '...' })
```

---

## 📄 Geliştirdiğim: PDF Export Yeniden Yazımı

Önceki sürümde dashboard `html2canvas` gibi bir kütüphaneyle görüntüye dönüştürülüp PDF'e aktarılıyordu — bu yaklaşımda uzun sayfalarda içerik tekrarı yaşanıyordu.

**Yeni yaklaşım:** Tarayıcının kendi print API'si kullanılıyor.

```
CSS @media print → window.print() → Tarayıcı PDF'e aktar
```

- `aifo-print-mode` class'ı ile print-only stiller tetikleniyor
- Sayfa başlığı export sırasında güncelleniyor, sonra geri alınıyor
- `afterprint` event ile temizlik yapılıyor
- Harici kütüphane bağımlılığı sıfır

---

## ⚙️ Geliştirdiğim: GitHub Actions CI Pipeline

Her `push` ve `pull_request`'te otomatik çalışan kalite kapısı:

```
Checkout → Node 20 kurulum → npm ci → lint → test → build
```

```yaml
# Üç adım sıralı çalışır, biri başarısız olursa pipeline durur
- npm run lint    # ESLint
- npm run test    # Vitest
- npm run build   # TypeScript + Vite build
```

---

## 🗂 Bundle Optimizasyonu

`vite.config.ts`'e eklediğim `manualChunks` ile build çıktısı mantıksal parçalara ayrıldı:

| Chunk | İçerik |
|---|---|
| `react` | react, react-dom, react-router-dom |
| `charts` | recharts |
| `excel` | xlsx |
| `state` | zustand, idb |

Böylece kullanıcı her sayfada tüm bundle'ı indirmek zorunda kalmıyor — değişmeyen vendor kodları cache'de kalıyor.

---

## 🛠 Tech Stack

`React 19` `TypeScript` `Vite 7` `TailwindCSS 4` `Zustand` `IndexedDB (idb)` `Recharts` `xlsx` `Vitest` `GitHub Actions`

---

## 🏗 Uygulama Akışı

```
/yukle → /eslestirme → /dashboard → /gecmis
Excel/CSV   Kalem         KPI + Risk    Geçmiş
yükleme     eşleştirme    skoru         analizler
```

Tüm veriler IndexedDB'de yerel olarak saklanır — merkezi sunucu bağımlılığı yok.

---

<img width="1831" height="874" alt="image" src="https://github.com/user-attachments/assets/0f4f9b13-3620-4fd2-aeaa-65e390fbcf91" />

## 🔒 Not

Kaynak kodu şirket reposunda olup kamuya açık değildir.
