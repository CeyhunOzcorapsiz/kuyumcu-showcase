# Jewelry E-Commerce & Dynamic Gold Pricing Platform — Showcase 💎⚡

**English** | [Türkçe](README.tr.md)

**A specialized enterprise e-commerce platform for the jewelry industry: live spot gold rate integration, dynamic craftsmanship margins, special tax base (VAT) calculation engine, 15-minute price hold locks, and MASAK KYC regulatory compliance.**

> 🔒 **About This Showcase:**  
> This repository is a **product showcase and architecture portfolio**. The full application source code (Next.js 16, React 19, Prisma models, virtual POS, hedging calculators, and e-invoicing pipelines) is maintained in a private repository. This repository highlights the architecture, financial principles, visual screens, and module specifications.

---

## 📸 Visual Interface Tour

### 1. Storefront & Customer Journey

| Live Gold Market Display (In-Store Digital Signage) | Product Detail & 15-Minute Price Hold Guarantee |
|:---:|:---:|
| ![Live Gold Market Display](docs/screenshots/01-canli-piyasa.png) | ![Product Detail & Price Hold](docs/screenshots/02-urun-detay-fiyat-kilidi.png) |
| *Real-time spot gold prices & change % ticker board* | *Streaming live price, 15-min freeze lock & special VAT breakdown* |

| Cart & 15-Minute Price Hold Checkout |
|:---:|
| ![Cart & Price Hold](docs/screenshots/03-sepet-fiyat-sabitleme.png) |
| *Locked exchange rate countdown timer, special tax base (VAT) calculation, and 3D Secure bank checkout* |

### 2. Treasury, Hedging & Regulatory Admin Operations

| Daily Gold Position & Hedging Exposure | MASAK KYC Regulatory Compliance Audit Queue |
|:---:|:---:|
| ![Daily Gold Position](docs/screenshots/04-has-altin-pozisyonu.png) | ![MASAK KYC Compliance](docs/screenshots/05-masak-kyc-denetim.png) |
| *Turnover tracked in raw fine gold grams, open vs hedged exposure* | *Mandatory identity verification queue for transactions above threshold* |

---

## 🎯 Core Principles of the Jewelry Financial Engine

Standard e-commerce frameworks cannot accommodate the unique regulatory and real-time market constraints of gold and precious jewelry. This platform is engineered around these fundamentals:

1. **Single Source of Truth for Pricing:** Storefront, cart, checkout, and invoicing utilize the exact same `pricing` engine, eliminating discrepancies between payment settlement and accounting.
2. **Fail-Safe Price Publishing:** If live market feeds display excessive drift, staleness, or anomalies, price publishing automatically pauses to prevent sub-cost sales.
3. **Zero Floating-Point Inaccuracies:** Currency and weights strictly utilize `Decimal(18,4)` representations instead of standard IEEE floating-point numbers.
4. **15-Minute Price Hold Guarantee:** Due to real-time market volatility, customers receive a 15-minute price freeze upon checkout initiation.
5. **Special Tax Base (VAT) Isolation:** Compliant with jewelry tax regulations: VAT applies strictly to labor and margin, while raw gold value remains tax-exempt.
6. **Regulatory Compliance (MASAK / KYC):** Identity verification records (`KycRecord`) for high-value purchases exceeding regulatory thresholds with 8-year record retention.
7. **Fine Gold Gram Hedging:** Sales create fine gold liabilities (`Has Altın Borcu`); treasury dashboard tracks open positions in grams to hedge with suppliers at daily market close.

---

## 🏗️ Architecture Topology

```mermaid
flowchart TB
    subgraph UI["Next.js 16 + React 19 Storefront & Signage"]
        VTR["Live Priced Catalog"]
        SPT["15-Min Locked Cart"]
        OD["Bank 3D Secure Checkout"]
        DISP["In-Store Digital Signage Ticker"]
    end
    subgraph ENGINE["Financial Domain & Business Logic"]
        PRC["Dynamic Pricing Engine<br/>Karat, Grams, Fine Gold Value"]
        VAT["Special Tax Base VAT Calculator"]
        LCK["15-Minute Price Quote Service"]
        SRC["Live Rate Feeds & Anomaly Guardrails"]
        HEDGE["Daily Gram Position & Hedging Tracker"]
    end
    subgraph EXT["External Services"]
        GOLD["Live Spot Gold / FX API"]
        BANK["Bank Virtual POS Gateway"]
        INV["e-Invoice Integration"]
        KYC["MASAK Encrypted Identity Vault"]
    end

    SRC --> GOLD
    PRC --> SRC
    VAT --> PRC
    LCK --> PRC
    VTR --> LCK
    SPT --> LCK
    OD --> BANK
    OD --> INV
    HEDGE --> PRC
    OD --> KYC
```

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend & SSR** | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4 |
| **Persistence & ORM** | PostgreSQL, Prisma ORM, Decimal precision storage |
| **Domain Logic** | Karat/Millesimal converter, Special VAT calculator, Price lock manager, Hedging engine |
| **Security & Payments** | Bank-hosted 3D Secure checkout, MASAK KYC compliance |
| **Testing & Verification**| Vitest (26+ pricing & currency conversion unit tests) |
| **Specifications** | [docs/MIMARI.md](docs/MIMARI.md) · [docs/OZELLIK-PLANI.md](docs/OZELLIK-PLANI.md) |

---

## 🔒 Confidentiality Notice
Proprietary source code, banking credentials, and production configurations remain private.
