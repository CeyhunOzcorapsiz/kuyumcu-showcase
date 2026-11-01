# Mimari ve İnşa Sırası

**Stack:** Next.js 16 (App Router) · React 19 · TypeScript · Tailwind 4 · PostgreSQL + Prisma · Vitest

**Tasarım ilkeleri**
1. **Fiyat mantığı tek yerde.** Vitrin, sepet, sipariş ve fatura aynı `lib/gold` motorunu kullanır. İkinci bir hesap yazılmaz — fatura ile tahsilatın tutmamasının tek sebebi budur.
2. **Yanlış fiyatla satmaktansa satmamak.** Kur şüpheliyse sistem fiyat yayınlamaz.
3. **Çok dilli/çok para birimli veri modeli baştan**, arayüz şimdilik tek dil (TR/TRY).
4. **Para asla `number` değil.** Her yerde `Decimal`, depolamada `Decimal(18,4)`.

---

## Durum

### ✅ Tamamlandı — çekirdek fiyat motoru
| Dosya | İş |
|---|---|
| `src/lib/gold/karat.ts` | Ayar → milyem; damga milyemi override'ı |
| `src/lib/gold/pricing.ts` | Has karşılığı, işçilik (3 yöntem), kâr, **özel matrah KDV**, külçe fiyatı |
| `src/lib/gold/quote.ts` | 15 dk fiyat kilidi, ödeme öncesi geçerlilik kontrolü |
| `src/lib/gold/rate-source.ts` | Çoklu kur kaynağı, bayatlık + sapma + birim hatası koruması |
| `src/lib/gold/pricing.test.ts` | 26 test |

### 🔜 Sıradaki adımlar

**1. Veri modeli (Prisma)** — bir sonraki adım
`Product` (ayar, gramaj, milyem, taş, sertifika, i18n alanları) · `PriceQuote` · `Order` ·
`OrderItem` (fiyat kırılımı snapshot'ı) · `Customer` · `KycRecord` · `RateSnapshot`

> Kritik: `OrderItem` sipariş anındaki **tüm fiyat kırılımını** saklar. Sonradan yeniden
> hesaplanmaz — kur değişir, fatura tutmaz.

**2. Kur sağlayıcı adaptörleri** — `RateSource` arayüzünü uygulayan 2 gerçek kaynak + cron
ile `RateSnapshot` yazımı (pozisyon kapama raporu buradan çıkar).

**3. Vitrin + SEO iskeleti** — kategori sayfaları, canlı fiyat sayfaları (ISR),
`Product` schema.org, sitemap, liste toplama formu.

**4. Sepet + fiyat kilidi UI** — geri sayım sayacı, süre dolunca yenileme akışı.

**5. Ödeme — banka sanal POS** *(3D Secure zorunlu)*
> ⚠️ Kart verisi **hiçbir zaman** bizim sunucumuza düşmez. Banka hosted 3DS sayfası kullanılır.

**6. e-Fatura entegratörü** — özel matrah kırılımı fatura satırlarına birebir taşınır.

**7. Kargo** — sigortalı gönderi, etiket üretimi, takip.

**8. MASAK akışı** — eşik üstü siparişte kimlik tespiti, `KycRecord`, 8 yıl saklama.
> Eşik tutarını mali müşavir ile netleştirin; koda sabit yazılmaz, **yapılandırma** olur.

**9. Pazaryeri senkronizasyonu** — Trendyol/Hepsiburada ürün + stok + sipariş.

**10. WhatsApp / e-posta otomasyonu** — günlük fiyat kanalı, sepet terk, fiyat alarmı.

---

## Sizin tarafınızda gereken (kod dışı, paralel yürüsün)

| # | İş | Neden bloklar |
|---|---|---|
| 1 | **Neon/Supabase Postgres** aç, `DATABASE_URL` ver | Veri modeli buna bağlanacak |
| 2 | **Kur veri sağlayıcısı** — 2 adet, API dokümanı | Adaptörler bunlara yazılacak |
| 3 | **Banka sanal POS** — entegrasyon dokümanı + test ortamı | POS entegrasyonu |
| 4 | **e-Fatura entegratörü** — test hesabı | Fatura modülü |
| 5 | **Mali müşavir teyidi** — özel matrah uygulaması + MASAK eşiği | Vergi ve yasal doğruluk |

> 3, 4 ve 5 uzun sürer. **Bugün başlatın**; biz bu arada 1–4. adımları bitiririz.

---

## Komutlar

```bash
npm run dev
```
```bash
npm test
```
```bash
npm run typecheck
```
