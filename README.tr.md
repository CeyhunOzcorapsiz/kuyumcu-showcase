# Kuyumcu E-Ticaret ve Dinamik Altın Fiyatlama Platformu — Vitrin 💎⚡

[English](README.md) | **Türkçe**

**Kuyumculuk sektörüne özel e-ticaret platformu: canlı borsa has altın kuru entegrasyonu, dinamik işçilik marjları, özel matrah (KDV) hesaplama motoru ve 15 dakikalık fiyat sabitleme kilidi.**

> 🔒 **Bu Vitrin Hakkında:**  
> Bu depo teknik bir **ürün vitrini ve mimari portföyüdür**. Uygulamanın tam kaynak kodları (Next.js 16, React 19, Prisma modelleri, sanal POS ve e-fatura entegrasyonları) özel bir kurumsal depoda saklanmaktadır. Bu vitrin platformun mimarisini, finansal mantığını, ekran görüntülerini ve modül tasarımlarını belgeler.

---

## 📸 Ekran Görüntüleri ve Arayüz Turu

| Mağaza İçi Canlı Piyasa Ekranı (Digital Signage) | Ürün Detay & 15 Dakika Fiyat Sabitleme |
|:---:|:---:|
| ![Canlı Piyasa Ekranı](docs/screenshots/01-canli-piyasa.png) | ![Ürün Detayı ve Fiyat Kilidi](docs/screenshots/02-urun-detay-fiyat-kilidi.png) |
| *Borsa İstanbul ve serbest piyasa anlık altın alış/satış kurları* | *Canlı akan fiyat, 15 dakikalık sepet kilidi ve Özel Matrah KDV ayrımı* |

---

## 🎯 Kuyumcu Finansal Motorunun Temel İlkeleri

Standart e-ticaret altyapıları, altın ve mücevherat sektörünün dinamik piyasa ve yasal regülasyonlarını karşılayamaz. Platform şu temel sütunlar üzerine inşa edilmiştir:

1. **Fiyatlamada Tek Doğruluk Kaynağı (Single Source of Truth):** Vitrin, sepet, ödeme adımı ve e-fatura aynı `pricing` motorunu çalıştırır; tahsilat ile muhasebe arasında kuruş farkı oluşmaz.
2. **Korumalı Fiyat Yayını (Fail-Safe Publishing):** Canlı kur servisinde aşırı oynaklık (drift), gecikme veya anomali tespit edildiğinde, sistem maliyet altı satışı engellemek için fiyat güncellemelerini durdurur.
3. **Kayan Nokta (Floating-Point) Hatasızlığı:** Para birimleri ve gramajlar IEEE kayan nokta (`float/double`) yerine daima `Decimal(18,4)` hassasiyetinde işlenir.
4. **15 Dakikalık Fiyat Sabitleme Garantisi:** Borsa dalgalanmaları nedeniyle müşteri ödeme adımına geçtiğinde fiyat 15 dakika boyunca dondurulur.
5. **Özel Matrah (KDV) Ayrımı:** KDVK Md. 17/4-g uyarınca külçe altın bedeli KDV'den istisnadır; %20 KDV yalnızca işçilik ve kâr marjı üzerinden hesaplanır.
6. **Mevzuata Uyum (MASAK / KYC):** Belirli yasal limitleri aşan kıymetli maden alışverişlerinde kimlik doğrulama (`KycRecord`) zorunluluğu ve 8 yıl saklama süresi.

---

## 🏗️ Mimari Topoloji

```mermaid
flowchart TB
    subgraph UI["Next.js 16 + React 19 Mağaza"]
        VTR["Canlı Fiyatlı Katalog"]
        SPT["15 Dk Kilitli Sepet"]
        OD["Banka 3D Secure Ödeme"]
    end
    subgraph ENGINE["Finansal Mantık & Çekirdek Motor"]
        PRC["Dinamik Fiyatlama Motoru<br/>Ayar, Gram, Has Karşılığı"]
        VAT["Özel Matrah KDV Hesaplayıcı"]
        LCK["15 Dk Fiyat Kilidi Servisi"]
        SRC["Canlı Kur Beslemesi & Korumalar"]
    end
    subgraph EXT["Harici Servisler"]
        GOLD["Canlı Borsa / Kapalıçarşı API"]
        BANK["Banka Sanal POS Altyapısı"]
        INV["e-Fatura / e-Arşiv Entegrasyonu"]
    end

    SRC --> GOLD
    PRC --> SRC
    VAT --> PRC
    LCK --> PRC
    VTR --> LCK
    SPT --> LCK
    OD --> BANK
    OD --> INV
```

---

## 🛠 Teknoloji Yığını

| Katman | Teknoloji |
|---|---|
| **Ön Yüz & SSR** | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4 |
| **Veritabanı & ORM** | PostgreSQL, Prisma ORM, Decimal hassasiyetli veri depolama |
| **Alan Mantığı** | Ayar/Milyem çevirici, Özel Matrah KDV motoru, Fiyat kilidi yöneticisi |
| **Güvenlik & Ödeme** | Banka onaylı 3D Secure ödeme, MASAK KYC mevzuat uyumu |
| **Test & Doğrulama** | Vitest (26+ fiyatlama ve döviz çevrim birim testi) |
| **Detaylı Belgeler** | [docs/MIMARI.md](docs/MIMARI.md) · [docs/OZELLIK-PLANI.md](docs/OZELLIK-PLANI.md) |

---

## 🔒 Gizlilik Bildirimi
Telif hakkı içeren kaynak kodlar, banka API anahtarları ve üretim ortamı parametreleri gizli tutulmaktadır.
