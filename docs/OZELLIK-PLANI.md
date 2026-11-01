# Özellik Planı — Tam Kapsam

> Global araştırmada (`global-ozellikler-ve-teknoloji.md`) çıkan **her özellik** bu listededir.
> Sıra, öncelik değil **bağımlılık** sırasıdır: her paket bir öncekinin üstüne kurulur.
>
> `[x]` tamam · `[ ]` yapılacak · `⏳` dış bağımlılık bekliyor

**Durum (17.08.2026):** 180 tamam · 25 açık.

*Satış tarafında* açık maddelerin tamamı bir dış hesaba/sözleşmeye (sanal POS,
e-Fatura entegratörü, Sentry, AR sağlayıcısı, pazaryeri), bir mevzuat teyidine
(taksit/BNPL) veya yayın sonrası veriye (alan performans ölçümü) bağlı.

*Dükkân tarafında* (Paket 13) yazılacak iş var ve tasarımı sizin çalışma
şeklinizi bilmeyi gerektiriyor — cari hesapta vade ve kapatma nasıl işliyor,
müşteriye veresiye veriyor musunuz gibi. Tahmin edip sonra sökmektense sorup
yazmak doğru.

Denetim kapsamı: 103 sayfa/API rota dosyası; üretim derlemesinde 142 statik
sayfa girdisi başarıyla üretildi. 725 test, tip denetimi, eslint ve üretim
derlemesi temiz. Parlegold karşılaştırması: `PARLEGOLD-KARSILASTIRMA.md`.

---

## ✅ Paket 0 — Çekirdek (tamamlandı)

- [x] Ayar/milyem modeli, damga milyemi override
- [x] Fiyat motoru: has karşılığı, 3 işçilik yöntemi, kâr, **özel matrah KDV**
- [x] Yatırımlık ürün fiyatı (makas)
- [x] Fiyat kilidi (15 dk) + ödeme öncesi geçerlilik kontrolü
- [x] Kur kaynağı zinciri: **manuel (tedarikçi) → Truncgil → yedek**
- [x] Bayatlık / sapma / birim hatası koruması + kaynak bazlı pencere
- [x] Türkçe sayı ayrıştırma (bin katı hata koruması)
- [x] Yapılandırma doğrulama (bozuk config ile uygulama başlamaz)
- [x] Veri modeli (21 model, i18n + çok para birimi hazır)
- [x] **Stok rezervasyonu — aşırı satış koruması** (atomik, gerçek DB'de kanıtlandı)
- [x] Ürün → teklif servisi (tek kullanımlık, atomik tüketim)
- [x] Manuel kur girme aracı (`npm run rate:set`)

## ✅ Paket 1 — Ticaret omurgası (tamamlandı)

- [x] Sipariş oluşturma (idempotent, atomik, snapshot'lı)
- [x] Sipariş numarası (ardışık değil — iş hacmi sızdırmaz)
- [x] MASAK eşik kontrolü (sipariş anında işaretleme)
- [x] Denetim kaydı (audit log)
- [x] Ödeme akışı: başlatma, 3DS yönlendirme, sonuçlandırma
- [x] Webhook: **imza + replay + tutar** doğrulaması
- [x] Başarıda stok düşümü / başarısızlıkta serbest bırakma
- [x] Sandbox ödeme sağlayıcı (gerçek HMAC)
- [x] Sepet (çerezde ürün+adet, fiyat sunucuda) + fiyat kilidi sayacı
- [x] Sipariş özeti sayfası

## ✅ Paket 2 — Vitrin temeli (tamamlandı)

- [x] Ürün sayfası (gramaj/ayar/milyem/sertifika/stok + metadata)
- [x] **Fiyat kırılımı şeffaflığı** — TR'de fark yaratan özellik
- [x] Statik kabuk + akan canlı fiyat (Partial Prerender)
- [x] Kategori sayfaları + ana sayfa
- [x] **Canlı altın fiyat sayfası** (12 kalem) + eğitim içeriği
- [x] schema.org `Product` + `Offer`, sitemap, robots

---

## 🔨 Paket 3 — Tasarım, görsel ve güven

### Tasarım sistemi
- [x] Tasarım tokenları (renk, tipografi, aralık) — premium/lüks hissiyat
- [x] Site kabuğu: header, gezinme, footer
- [x] Sepet rozeti (adet göstergesi)
- [x] Karanlık/aydınlık tema tutarlılığı
- [x] Klavye odağı görünürlüğü + "içeriğe atla" (WCAG 2.4.1 / 2.4.7)
- [x] `prefers-reduced-motion` desteği (WCAG 2.3.3)
- [x] **Mobil açılır gezinme menüsü** (`<details>` ile, JavaScript'siz)
      · Mobil arama artık sürekli görünür; hesap, istek listesi, canlı fiyat,
        koleksiyon, hediye bulucu ve video danışmanlık menüde erişilebilir
      · 390 px gerçek tarayıcı testinde yatay taşma yok, hedefler 44×44 px

### Ürün görselleştirme
- [x] `next/image` geçişi (AVIF/WebP, responsive, LCP önceliği)
- [x] **Ürün galerisi** — küçük görseller + büyük görünüm
- [x] **HD zoom / makro inceleme** (James Allen modeli)
- [x] Görsel CDN yapılandırması (alan adı seçilince devreye girer)
- [x] **360° döndürülebilir görünüm** (sürükle + klavye, kareler önyüklemeli)
- [x] Ürün videosu desteği (kapak görseli + metin alternatifi)
- [x] Model üzerinde çekim + **ölçek referansı** — ayrı medya türü, rozetli
- [x] Medya türü modeli (PHOTO / SPIN / MODEL / SCALE / VIDEO)
- [x] Demo medya üreteci (`npm run demo:media`) — gerçek fotoğraf gelene kadar
- [x] **Görsel yükleme arayüzü** — Cloudinary, tarayıcıdan doğrudan imzalı yükleme
- [x] Medya yönetimi: tür seçimi, alt metin zorunluluğu, kapak, silme
- [x] Cloudinary görsel yükleyicisi (f_auto/q_auto/c_limit, çifte optimizasyon yok)

### Güven ve satın alma sonrası
- [x] **Yüzük ölçü aracı** (iki yöntem + ölçü tablosu + ipuçları)
- [x] Ürün bazlı yüzük ölçüsü seçimi + sepet/sipariş/atölye snapshot'ı
- [x] Güven rozetleri (sigortalı kargo, sertifika, iade, 3D Secure)
- [x] Sertifika bilgisi ürün sayfasında
- [x] Zincir boyu görselleştirici (7 boy, siluet üzerinde, `chainLengthCm`)
- [x] Sertifika görüntüleyici (GIA / IGI belgesi + kurumun doğrulama sayfasına bağlantı)
- [x] Fotoğraflı müşteri yorumları + toplama otomasyonu
      (teslimattan 5 gün sonra tek kullanımlık davet · moderasyon · AggregateRating)
- [x] Ekspertiz / değerleme belgesi (sipariş snapshot'ından, yazdırılabilir)

## 🔨 Paket 4 — Ticari mekanikler

- [x] **Fiyat alarmı / bülten** (çift onaylı) → liste toplama
- [x] KVKK açık rıza kaydı (metin sürümü + IP + zaman damgası)
- [x] **Stokta yok bildirimi** (ürün bazlı)
- [x] **İstek listesi** (çerez tabanlı, üyelik gerektirmez) + liste sayfası
- [x] **Hediye paketi + kişisel not + "bu bir hediye"** (fiyatsız paket)
- [x] Onay e-postası altyapısı — transaction outbox + şablon + sağlayıcı adaptörü
      · Canlı gönderim için e-posta hesabı/DNS doğrulaması bekliyor
- [x] "Birine gönder" (siparişi veren müşteri ile kimlikle teslim alacak alıcı ayrı)
- [x] **Hediye çeki** — kod üretimi/bakiye takibi, yönetimden çıkarma, sipariş
      sayfasında (TR ve EN) kısmi ödeme olarak kullanım
- [x] **Genel ürün seçeneği** — rakam/harf/yüzey/renk/motif gibi seçenekler
      ürün panelinden tanımlanır; sepette ayrı satır, siparişte değişmez snapshot
      · Sipariş servisi seçeneği güncel ürün listesine karşı tekrar doğrular
- [x] Mağazadan teslim / randevu (kargosuz toplam, hazır bildirimi, kimlikli teslim audit'i)
- [x] **Video danışmanlık randevusu** (açık rıza, hız sınırı, yönetim kuyruğu)
- [ ] Taksit / BNPL ⚠️ *TR'de kıymetli maden kart taksiti kısıtlı — mevzuat teyidi*

## 🔨 Paket 5 — Konfigüratörler (en yüksek marj)

- [x] **Kazıma / gravür konfigüratörü** — canlı önizleme, 3 yazı tipi, 2 konum
- [x] Kazıma ücretinin fiyat motoruna entegrasyonu (marja tabi değil, KDV'ye tabi)
- [x] Kazınamayan karakter doğrulaması (emoji/çok baytlı reddedilir)
- [x] Cayma hakkı uyarısı + onay zaman damgası (ispat yükü satıcıda)
- [x] Sepette ve sipariş özetinde kazıma snapshot'ı
- [x] Doğum taşı seçimi (`/yuzuk-tasarla`, `/en/design-a-ring`, charm kurgulayıcı)
- [x] **Ring builder** (montür + taş seçimi) — `/yuzuk-tasarla`, `/en/design-a-ring`
- [x] Charm / katmanlama (stacking) kurgulayıcı — `/charm-kombinle`, `/en/charm-builder`
- [x] Taş envanteri + **faceted arama** (kesim/renk/berraklık/karat) + karşılaştırma
      · Vitrin `/taslar` + `/en/stones`, yönetim `/yonetim/taslar`
- [x] **Konfigüratörde anlık fiyat** — tasarım×metal×taş seçimi canlı has altın
      kurundan fiyatlanıyor, kırılım (külçe / taş / işçilik / kâr / KDV) açık
      · Montür şablonları `/yonetim/monturler`'den giriliyor; şablonu olmayan
        bileşimde uydurma rakam yerine "görüşmede netleşir" deniyor
      · Rakip konfigüratörler sabit fiyat tablosu kullanıyor; bizimki ürün
        fiyatlarıyla aynı motordan geçtiği için altın hareketini kendiliğinden
        yansıtıyor
- [ ] Gerçek zamanlı 3D render (WebGL) — metal/taş değişimi anında
      · Her montür×metal×taş bileşimi için tarama varlığı gerekiyor; gerçek
        ürün fotoğrafı gelmeden yapılacak iş değil

## 🔨 Paket 6 — Görsel teknolojiler

- [x] **3B görüntüleyici** (`<model-viewer>`, dinamik yüklenir) + **AR yerleştirme**
- [x] 3B varlık hattı: GLB alanı + iOS USDZ alanı, galeriye entegre
- [x] Sanal deneme sağlayıcı yuvası (`lib/media/try-on.ts`) — hesap bekliyor
- [x] Demo GLB üreteci (`npm run demo:3d`) — gerçek tarama gelene kadar
- [ ] **AR sanal deneme adaptörü** ⏳ *sağlayıcı hesabı bekliyor*
- [ ] USDZ dönüştürme hattı (iOS AR Quick Look için)
- [x] **UGC / shoppable galeri** — işaretli ürün bağlantıları, video desteği
- [x] Moderasyon kuyruğu + **yayın izni kuralı** (saf fonksiyon, testli)
- [x] Demo UGC üreteci (`npm run demo:ugc`)

## 📋 Paket 7 — Arama ve keşif

- [x] Site içi arama — **PostgreSQL tam metin + trigram** (Algolia/Typesense YERİNE)
      · Türkçe aksan duyarsız ("altin" = "altın"), büyük harf doğru
      · Çekim eki kırpma ("bilezikler", "bileziği" → bilezik)
      · Yazım hatası toleransı ("bilzik" → bilezik)
      · Eş anlamlı ("evlilik yüzüğü" → alyans), ayar/gramaj/milyem varyantları
      · Anlık önerili arama kutusu (klavye ile gezinilebilir)
- [x] **Faceted ürün filtreleme** (ayar, gramaj, stok, sıralama) — bağlantı tabanlı, JS'siz çalışır
- [x] Taş/renk filtresi — taş envanteri sayfasında faceted olarak çalışıyor
- [x] **Hediye bulucu** (üç soru: kime / niçin / bütçe) — bağlantı tabanlı, JS'siz
      · Puanlama ile sıralama — katı filtre değil, boş sayfa göstermez
      · Bütçe dışı ürün elenmez ama açıkça "bütçe üstü/altı" diye işaretlenir
      · "Neden bu ürün?" rozetleri (bütçenize uygun, isim yazdırılabilir, tek parça)
- [x] **Benzer ürünler** — özellik tabanlı (kategori/ayar/gramaj), ilk günden çalışır
- [x] **Son gezilen ürünler** — çerezde, üyeliksiz; işlevsel çerez (izleme değil)

## 📋 Paket 8 — Büyüme ve elde tutma

- [x] **WhatsApp günlük altın fiyatı kanalı** — abonelik formu (açık rıza + IP +
      zaman damgası), numara normalizasyonu, yönetim kuyruğu `/yonetim/whatsapp`
      · ⏳ Gönderim için WhatsApp Business API hesabı bekliyor. Numara
        doğrulanmadan tek bir mesaj bile çıkmıyor — bilinçli.
- [x] **Hedefli fiyat alarmı** (gram kuru veya ürün fiyatı, düşüş/yükseliş yönü)
      · Çift onay; onaysız alarm ASLA tetiklenmez
      · Bir kez tetiklenir ve susar — eşik salınımında spam yok
      · Bildirim outbox'a yazılır; sağlayıcı çökerse alarm kaybolmaz
- [x] **Stok alarmı gönderimi** — form aylardır kayıt topluyordu, gönderim YOKTU
- [x] Sepet terk hatırlatması — sunucu taraflı opak sepet oturumu, çift onay,
      tek gönderim, çıkış bağlantısı ve 30/90 günlük otomatik temizlik
- [x] Sipariş sonrası e-posta akışları (sipariş, ödeme, kargo, teslim)
      · Canlı gönderim için e-posta sağlayıcısı bekliyor
- [x] Sadakat temeli — işçilik/taş/marj matrahından idempotent puan kazanımı,
      ödeme iadesinde otomatik ters kayıt ve sipariş ekranında bakiye
- [x] **Fiyat grafiği** (satır içi SVG, kütüphanesiz) + erişilebilir tablo
- [x] **"Alsaydınız" getiri hesaplayıcı** (yalnızca gerçek veri aralığında)
- [x] **Rehber altyapısı** (`/rehber`) — veritabanında, panelden yazılır
      · Markdown, **ham HTML'e hiç izin vermeyen** kendi işleyicimiz
      · Canlı önizleme (yayındakiyle AYNI işleyici)
      · Yazıya ürün bağlama — içeriği satışa bağlayan halka
      · Article JSON-LD, sitemap, taslak/yayın ayrımı
- [x] **Rızaya bağlı reklam atfı** — ilk/son UTM teması, tıklama kimliği,
      referrer alan adı ve açılış yolu 30 gün tutulur; tam referrer URL'si yok
      · Sipariş snapshot'ına bağlanır, `/yonetim/pazarlama` paid gelir raporu
      · Çerez reddedilirse hiçbir kampanya kaydı oluşturulmaz

## 🔨 Paket 9 — Operasyon (yönetim paneli)

- [x] **Yönetici girişi** — scrypt parola, sunucu tarafı oturum, kaba kuvvet kilidi
- [x] Rol bazlı yetki (OWNER / ADMIN / STAFF)
- [x] Proxy ile iyimser koruma + sayfa katmanında gerçek doğrulama
- [x] **Manuel kur girme ekranı** (birim hatası koruması dahil)
- [x] Sipariş listesi + detay + **kontrollü durum geçişleri**
- [x] Atölye için kazıma/hediye talimatları sipariş detayında
- [x] **Gün sonu has altın pozisyon raporu**
- [x] Yönetici oluşturma aracı (`npm run admin:create`)
      · Parola komuta argüman olarak yazılmaz, çalışırken sorulur ve yazarken
        ekranda görünmez — argüman olsaydı komut geçmişinde düz metin kalırdı
      · Tek kural: en az 8 karakter. Karmaşıklık ve kelime kara listesi yok —
        panel sahibinin tercihi. Savunmayı hız sınırı ve 2FA üstleniyor
      · `--rastgele` ile üretilen güçlü parola isteğe bağlı
- [x] İki faktörlü doğrulama (2FA) — TOTP (RFC 6238), kurtarma kodları,
      deneme sınırı, tekrar koruması; sır AES-256-GCM ile şifreli saklanır
- [x] Uygulama seviyesinde şifreleme + anahtar rotasyonu (`ENCRYPTION_KEYS`)
- [x] QR kod üretici (bağımlılıksız, bağımsız çözücüyle doğrulanmış)
- [x] **Arka plan görevleri altyapısı** (token korumalı, 6 görev) — `GOREVLER.md`
- [x] **Stok sızıntısı düzeltildi** — süresi dolan rezervasyonlar artık temizleniyor
- [x] **Outbox işleyici** (üstel geri çekilme, çift gönderim koruması)
- [x] Kur anlık görüntüsü zamanlanmış kaydı (fiyat grafiği verisi)
- [x] MASAK saklama süresi dolan kayıtların otomatik imhası
- [x] Ürün yönetimi (ekleme, düzenleme, liste/arama/filtre, görsel yükleme)
- [x] Yayın kapısı — fiyatı hesaplanamayan ürün satışa AÇILAMAZ
- [x] Birim hatası koruması (oran alanlarında canlı yüzde önizlemesi)
- [x] Rol ayrımı: fiyat alanlarını yalnızca yönetici değiştirebilir
- [x] MASAK kimlik tespiti **toplama** akışı (şifreleme + erişim logu)
      · T.C. kimlik no kontrol hanesi doğrulaması
      · Müşteri formu → AES-256-GCM ile şifreli kayıt (anahtar ortamda)
      · Gerekçeli görüntüleme; her erişim `KycAccessLog`'a yazılır
      · Kör indeks ile aynı kişinin diğer kayıtları (structuring şüphesi)
      · **Sevkiyat kapısı:** tespit tamamlanmadan sipariş hazırlanamaz

## 🔨 Paket 14 — Vitrin tasarımı ve dönüşüm

- [x] **Varsayılan tema açık** — altın açık zeminde daha sıcak okunur, ürün
      çekimi tek zemin varsayımıyla yapılır. Koyu tema silinmedi, başlıktaki
      seçiciye alındı; tercih `localStorage`'da, açılış betiğiyle yanıp sönme yok
- [x] **Kategori vitrini** — 22 metin baloncuğu yerine iki kademeli görsel ızgara
      · Öne çıkanlar büyük kartla üstte (`featuredRank` ile yönetimden sıralanır)
      · Görsel alanı hazır; görsel yokken sade yer tutucu, &ldquo;görsel yakında&rdquo; yazmıyor
      · Ürünü olmayan kategori vitrinde hiç görünmüyor
- [x] **Bileklik ve zincir ölçü rehberi** (`/bileklik-olcusu`) — modele göre
      pay tablosu, kolye boyu tablosu; ürün sayfasından bağlantılı
      · Sebep: bilezik iadesinin ana sebebi ölçü; gramaj bileğe oturuşu söylemez
- [x] **Bütçeden ürüne** (`/butce`) — tutarı gir, alabileceklerini gör
      · Bütçenin has altın karşılığı gösteriliyor, &ldquo;bu kadar altın alırsınız&rdquo;
        DEMİYOR — işçilik, kâr ve KDV farkı açıkça yazılı
      · Bütçe dışı ürün elenmiyor, &ldquo;altında/üstünde&rdquo; diye işaretleniyor
- [x] **Ismarlama talebi** (`/ismarlama`) — gramaj, ayar ve ölçü talebi
      · Otomatik fiyat VERMİYOR: ısmarlamada işçilik sabit değil, tutamayacağımız
        söz vermektense bakıp dönmek doğru
      · Ürün sayfasından bağlam taşıyarak açılıyor
      · KVKK rıza kaydı (sürüm + IP + zaman), hız sınırı, yönetim kuyruğu
      · Veri sahibi başvurusunda görünüyor ve silinebiliyor
- [ ] Ürün fotoğrafı hattı (AVIF/WebP, bulanık ön-görüntü, sabit kırpma oranı)
      ⏳ *gerçek çekim bekliyor — boşa çalışmasın*
- [ ] Ölçek referansı (bilek/el üstünde çekim alanı + karşılaştırma görseli)
      ⏳ *aynı sebeple çekim bekliyor*
- [x] **Ana sayfada canlı has altın kuru** — kuyumcu müşterisi siteye bazen
      sadece fiyata bakmaya gelir; şerit onu günlük ziyarete çeviriyor
- [x] **Hizmet kartları** — bütçe, ölçü rehberi, ısmarlama, randevu araçları
      yazılmıştı ama ana sayfada görünmüyordu, yalnızca adresi bilen ulaşıyordu
- [x] **Ana sayfa yeniden sıralandı — ürünler yukarı**
      · Önceden ürünler DÖRDÜNCÜ sıradaydı; ziyaretçi iki bölüm kaydırmadan
        hiçbir ürün görmüyordu. Ana sayfa bir vitrindir
      · Yeni sıra: ürünler → kategori ürün satırları → özel günler →
        kategoriler → hizmetler → müşteri içerikleri
      · İlk ekran artık sabit reklam değil gerçek katalog ürünü ve onun canlı
        fiyatını gösteriyor; ürün arşivlenirse vitrin kendiliğinden değişiyor
- [x] **Hemen satın al** — sepete eklemeyle aynı doğrulama ve fiyat motorunu
      kullanır; ayrı ve zamanla güvensizleşecek ikinci sipariş yolu oluşturmaz
      · Seçim sepete yazılır ve müşteri doğrudan `#odeme` bölümüne iner
- [x] **Kategori bazlı ürün satırları** — her satır kendi başlığı ve
      "tümünü gör" bağlantısıyla; en fazla üç satır (ana sayfa liste sayfası değil)
      · Ürünü 3'ten az olan kategori satır AÇMIYOR — yarım ızgara, hiç
        olmamasından kötü. Katalog büyüdükçe satırlar kendiliğinden çıkıyor
- [x] **Özel gün girişleri** — nişan, yıldönümü, doğum günü, yeni doğan, bayram
      · TR'de mücevher alımı takvime bağlı; müşteri "bilezik" diye değil
        "yıldönümü hediyesi" diye düşünür (emsallerde de var)
      · Yeni sayfa açılmadı: hediye bulucunun önceden doldurulmuş hâline
        gidiyor — tek yerde duran mantık, iki yerde ayrışandan iyidir
- [x] **Koleksiyon kavramı** (Gelin, Hediyelik…) — kategoriden ayrı, duyguya hitap eder
      · Vitrin `/koleksiyon` + `/koleksiyon/[slug]`, yönetim `/yonetim/koleksiyonlar`
      · Bir ürün birden çok koleksiyonda olabilir; kategori ağacında bu mümkün değildi
      · Taslak/arşiv ürün koleksiyondan otomatik düşer, içi boşalan koleksiyon
        vitrinde hiç görünmez — boş koleksiyon yayınlanamıyor
      · `npm run demo:koleksiyon` ile örnek veri
- [ ] Günlük altın fiyatı bildirimi (PWA) — kuyumcu müşterisi fiyata her gün bakar

## 🔨 Paket 17 — Fark yaratıcılar

- [x] **Ürün bazlı fiyat geçmişi** — bu ürünün son 30 gündeki fiyat seyri
      · Geçmiş kurlar ürünün KENDİ formülünden geçiriliyor; rakipler yapamaz
        çünkü fiyatları formülden gelmiyor, elle giriliyor
      · "Fiyatı belirleyen tek değişken altın" iddiası testle doğrulandı:
        işçilik kurdan bağımsız sabit, ürün fiyatı kurdan DAHA AZ oranda artıyor
      · Sınır açıkça yazılı: bu geçmiş SATIŞ fiyatı değil — işçilik/marj
        geçmişi saklanmadığı için bugünkü değerlerle hesaplanmış seri
      · Grafik bileşeni yeniden kullanıldı; yeni JavaScript yok
- [x] **Has altın portföyü** ("Birikimim") — hesaptaki müşteri, aldığı
      ürünlerdeki toplam has altını ve bugünkü karşılığını görüyor
      · Veri zaten vardı: sipariş kalemlerinde has gram snapshot'ı
      · Getiri/kâr-zarar YOK — yatırım tavsiyesi sınırına girmiyor
      · İade edilen ürün hesaptan düşer; kaynak defterdeki `SALE_RETURN`
        hareketi (talep durumundan daha güvenilir: "mal elimde mi" sorusunun
        tam karşılığı)
      · "İçindeki has altının karşılığıdır, ürünün satış değeri değildir"
        uyarısı — müşteri geri satışta hayal kırıklığı yaşamasın
- [x] **Çeyiz / set kurgulama** (`/ceyiz`) — parçaları bir arada seç, toplam
      gramajı ve tutarı canlı gör, hepsini tek seferde sepete ekle
      · Toplam HAS GRAM tutardan önce gösteriliyor — çeyiz konuşması gram
        üzerinden yapılır ("kaç gram taktınız")
      · Seçim adreste taşınıyor: liste WhatsApp'tan paylaşılabiliyor, hesap
        gerekmiyor, JavaScript kapalıyken de çalışıyor
      · Bütçe girilirse aşım/kalan tutar gösteriliyor
      · Fiyatı hesaplanamayan kalem toplama katılmıyor ama SAYILIYOR —
        sessiz eksik toplam, sepette sürpriz demektir
      · Yüzük ölçüsü gereken ürün toplu eklemede atlanıyor: ölçü tahmin edip
        sepete koymak, müşteriye yanlış numarada ürün göndermek olurdu
      · Set indirimi YOK — oran ticari karar, uydurulamaz
- [ ] Cari hesap (TL ve gram ayrı) ⏳ *çalışma şeklinizi bilmeyi gerektiriyor*
- [ ] Eski altın takası ⏳ *geri alım makas oranınız bekliyor*
- [ ] Geri alım şeffaflığı ⏳ *ticari karar*

## 🔨 Paket 16 — Müşteri hesabı (şifresiz)

Mimari düzeltme: `Customer` kaydı **zaten** misafir siparişte açılıyordu ve
aynı e-posta aynı kayda bağlanıyordu. Eksik olan kayıt değil **erişimdi** —
müşteri kendi kaydını göremiyordu. Bu yüzden "üye ol" diye yeni bir varlık
yaratılmadı; var olan kaydın sahipliği doğrulanıyor.

- [x] **Şifresiz giriş** — e-postaya 6 haneli tek kullanımlık kod
      · `Customer` üzerinde parola alanı YOK ve olmayacak: saklamadığımız
        şifre çalınamaz. Zaten parola sıfırlama da e-postaya dayanıyordu,
        yani parola gerçek bir katman eklemiyordu
      · Kod düz metin saklanmıyor; karşılığı (hash) tutuluyor
      · Yeni kod istenince eskiler iptal — birden çok geçerli kod birikmiyor
      · Kayıt başına 5 deneme + IP başına hız sınırı
      · Sabit zamanlı karşılaştırma (kod uzunluğu sızdırmaz)
      · E-posta kayıtlı mı bilgisini dışarı vermiyor (kullanıcı sayımı koruması)
- [x] **Sunucu tarafı oturum** — JWT değil; "tüm cihazlardan çıkış" gerçekten çalışsın
- [x] **Hesabım** (`/hesabim`) — siparişler, ölçüler, istek listesi, güvenlik
- [x] **"Ailem" ölçü hafızası** — eşim/annem için yüzük, bilek, kolye ölçüsü
      · İadenin ana sebebi ölçü; bir kez girilip hatırlanıyor
      · Aynı etiketten iki kayıt olamaz (hangisi güncel belirsizliği doğmasın)
- [x] **İstek listesi hesaba taşınıyor** — çerezde başlar, giriş yapınca birleşir
      · Birleşme, üzerine yazma değil: başka cihazda eklenenler silinmiyor
      · Çıkış yapınca liste bomboş görünmesin diye çerez yerinde kalıyor
- [x] Misafir ödemesi **varsayılan yol olarak kaldı** — zorunlu üyelik, sepet
      terkinin ana sebeplerinden biri (Baymard: %19 doğrudan bu yüzden terk ediyor)
- [x] **Yönetimde müşteri görünümü** (`/yonetim/musteriler`)
      · Hesap sistemi kurulmuştu ama panelden hiçbiri görünmüyordu: KVKK
        başvurusunda müşteriyi elle veritabanından aramak gerekiyordu
      · Liste: sipariş sayısı, harcama (YALNIZCA ödenmiş siparişler), son sipariş
      · Detay: siparişler, kayıtlı ölçüler, adresler, istek listesi, ısmarlama
        talepleri (e-postayla eşleşenler — talepte e-posta isteğe bağlı,
        eşleşmeyenlerin nerede olduğu ekranda yazıyor)
      · Şifre gösterilmiyor çünkü yok; yalnızca açık oturum sayısı var
- [ ] Google ile giriş ⏳ *Google Cloud OAuth istemcisi bekliyor*
- [ ] İngilizce tarafın yeni sayfalara yetişmesi (ceyiz, karsilastir, ismarlama,
      butce, bileklik-olcusu, altin-fiyati, giris, hesabim)
      · Bilinçli erteleme: EN kataloğu boş olduğu için şimdi eklemek boş sayfa
        çoğaltmak olur. Ürün çevirileri girildiğinde topluca yapılacak
- [x] **Sipariş sonrası hesap teklifi** — misafir sipariş verdikten sonra
      sipariş sayfasında "giriş yap" kartı; e-posta önceden dolu geliyor
      · Araştırmadaki desen: "önce misafir olarak sattır, sonra hesap teklif et"
- [x] **Ödeme akışı hesabı tanıyor** — giriş yapmış müşterinin e-postası, adı
      ve KAYITLI ADRESLERİ forma dolduruluyor
      · `Address` modeli şemada duruyordu ama HİÇ kullanılmıyordu (ölü şema);
        hesap sistemi gelince eksik ortaya çıktı
      · Sipariş adresi yine snapshot olarak donuyor — "hangi adrese gönderdik"
        sorusunun cevabı o. Defter ayrı, yeniden kullanım için
      · Aynı adres iki kez kaydedilmiyor (normalleştirilmiş karşılaştırma)
      · Adres kaydı yan iş: yazılamazsa sipariş yine tamamlanıyor
- [ ] Özel gün hatırlatıcısı ⏳ *canlı e-posta gönderimi sağlayıcı bekliyor*
- [x] **Paylaşılabilir istek listesi** — alan kişi ≠ takan kişi sorunu
      · Karşı taraf hesap açmadan görüyor; hesap zorunlu olsaydı bağlantıyı
        hiç açmazdı
      · Paylaşılan sayfada YALNIZCA ürünler var: ad, e-posta, adres, sipariş
        geçmişi yok
      · Anahtar rastgele ve iptal edilebilir; iptalde anahtar siliniyor
        (işaretlenmiyor — olmayan anahtar yanlışlıkla geçerli sayılamaz)
      · Dizine kapalı: kişiye özel bağlantı arama sonucunda çıkmamalı
- [x] **Teslimat tarihi tercihi ve notu** — hediye seçenekleri içinde
      · Hediye alışverişinde zamanlama ürün kadar önemli; hediye paketi ve
        kişisel not vardı ama zamanlama yoktu
      · TAAHHÜT DEĞİL tercih olarak alınıyor ve üç yerde de böyle yazıyor
        (form, sipariş özeti, yönetim) — kargonun teslim günü bizde değil
      · Geçmiş ve 90 günden uzak tarih sessizce yok sayılıyor; tarayıcı
        sınırları sunucudaki ayrıştırmayla AYNI kaynaktan geliyor
      · Tarih UTC gün başlangıcı olarak tutuluyor: saat dilimi kayması
        siparişi listede yanlış güne düşürürdü
- [x] **Mobil yatay kaydırma hatası** — altın grafiğinin ekran okuyucu tablosu
      sayfayı taşırıyordu (`sr-only` bir `<table>` üzerinde çalışmıyor: CSS
      tabloları min-içerik genişliğinin altına inmez). Grafiğin olduğu HER
      sayfayı etkiliyordu; tablo sarmalayıcı `div` içine alındı

## 🔨 Paket 15 — Arama motoru görünürlüğü

- [x] **`Offer` artık fiyat taşıyor** — daha önce fiyat bilinçli olarak
      yazılmıyordu; sonuç, Google'ın zengin sonucu hiç göstermemesiydi
      · Çözüm gizlemek değil, fiyatın hesaplandığı yerde üretmek: sayfadaki
        rakam ile yapısal veridekinin ayrışması artık mimari olarak imkânsız
      · `priceValidUntil` ile fiyatın kalıcı olmadığı bildiriliyor
      · `itemCondition`, 14 günlük iade politikası, kargo bilgisi eklendi
      · Kargo koşulsuz ücretsiz değilse tutar İDDİA EDİLMİYOR
- [x] **Kırıntı navigasyonu** (`BreadcrumbList`) — ürün, kategori ve iniş
      sayfalarında; arama sonucunda çıplak adres yerine yol görünür
- [x] **Koleksiyon işaretlemesi** (`CollectionPage` + `ItemList`) kategori ve
      iniş sayfalarında; ürünler `Product` olarak TEKRAR tanımlanmıyor
      (aynı ürünü iki yerde farklı ayrıntıyla tanımlamak çelişki üretir)
- [x] **Gramaj × ayar iniş sayfaları** (`/altin-fiyati/22-ayar-10-gram-altin-bilezik`)
      · "22 ayar 10 gram bilezik fiyatı" tipi aramaların karşılığı
      · Sayfalar KATALOGDAN türetiliyor — satmadığımız bileşim için sayfa yok;
        ince içerik üretmemenin tek yolu bu
      · Gramaj bandı etiketin ±%10'u: üretimde 10 gram tam çıkmaz, ama 13 gramı
        "10 gram" sayfasında göstermek de yalan olurdu
      · Uydurma adres reddediliyor; ürün kalmazsa sayfa 404
      · Kategori sayfasından iç bağlantı veriliyor (yalnız sitemap'te olan
        sayfayı arama motoru zayıf sayar)
- [ ] `LocalBusiness` / `JewelryStore` şeması ⏳ *dükkân adresi ve çalışma
      saatleri bekliyor — uydurulamaz*
- [ ] Google Merchant Center ⏳ *hesap ve alan adı doğrulaması bekliyor*
      (besleme ucu `/api/merchant-feed` hazır)

## 🔨 Paket 13 — Dükkân tarafı (işletme)

Vitrin satar; bu paket dükkânı döndürür. Sırası bağımlılık sırasıdır:
defter olmadan alım, alım olmadan gerçek marj hesaplanamaz.

- [x] **Stok ve has hareket defteri** — yalnızca eklenen, silinmeyen kayıt
      · Her hareket sebebiyle kaydedilir (açılış, alım, satış, iade, sayım, zayi)
      · Adet **ve** has gram birlikte tutulur — kuyumcu has ile düşünür
      · Hareket anındaki milyem saklanır; ürün sonradan düzeltilse de defter değişmez
      · Değişmezler veritabanı CHECK kısıtlarıyla da kilitli (sıfır hareket yok,
        işaret tutarlılığı, has ≤ brüt, gerekçesiz düzeltme yok)
      · `sourceRef` tekilliği ile mükerrer kayıt imkânsız (webhook iki kez gelse de)
      · Stok güncellemesi ve defter kaydı **tek transaction** — biri olup diğeri olmaz
- [x] **Mutabakat raporu** (`/yonetim/mutabakat`) — ürün kaydındaki sayı ile
      defter toplamı karşılaştırılır; defteri atlayan her değişim yakalanır
- [x] **Açılış devri betiği** (`npm run stok:acilis`) — defter kurulmadan önce
      girilmiş ürünler için tek seferlik geçiş
- [x] **Tedarikçi kaydı** (`/yonetim/tedarikciler`)
- [x] **Alım fişi** (`/yonetim/alimlar`) — taslak → deftere işle → gerekirse iptal
      · Satır bazında gramaj, milyem, mal bedeli ve işçilik ayrı ayrı
      · Alım günü kuruyla **ödenen paranın has karşılığı** hesaplanır —
        mala giren has ile arasındaki fark, tedarikçiye ödenen işçilik ve kârdır
      · İşlenmiş belge silinmez; iptal ters hareketle olur, iki kayıt da kalır
      · Satılmış malın alımı iptal EDİLEMEZ (defter stoğu eksiye düşürmez)
- [x] **İade edilen mal stoğa geri döner** — daha önce hiç dönmüyordu; iade
      onaylanan ürün sonsuza kadar "satılmış" kalıyordu (denetimde bulundu)
- [x] Ürün formundan stok değişimi artık mutlak sayı yazmaz, deftere gerekçeli
      düzeltme hareketi geçer — stoğu değiştiren tek yol defterdir
- [ ] **Cari hesap** (TL ve gram ayrı ayrı) — toptancı borcu genelde gram
      cinsindendir; yalnız TL tutan cari kuyumcuda yanlış rakam üretir
      · Tasarımı çalışma şeklinize bağlı: vade, kapatma, müşteri veresiyesi
- [ ] Müşteriden altın alımı (sarrafiye/hurda) — MASAK ve randevu altyapısı hazır
- [x] **Fiziksel sayım oturumu** (`/yonetim/sayim`)
      · Oturum açılırken her ürünün o anki miktarı DONDURULUYOR — sayım
        sürerken satış olursa fark bozulmasın. Bugünkü stokla hesaplasaydık
        satılan ürün "kayıp" görünür ve deftere ikinci kez düşülürdü
      · Sayılmamış satır SIFIR SAYILMAZ; deftere hiç dokunulmaz
      · Farklar tek transaction içinde `COUNT_ADJUSTMENT` olarak yazılıyor —
        yarısı yazılan bir sayım envanteri düzeltmek yerine bozar
      · Fark kadar hareket yazılıyor, "stok şu olsun" denmiyor: aradaki
        satışlar korunuyor
      · Sonuç rezerve edilmiş adedin altına düşerse hiçbir fark yazılmıyor
- [x] **Tamir / servis takibi** (`/yonetim/servis`)
      · Burada satış yok, EMANET var: müşterinin malını teslim alıyoruz
      · Giriş ağırlığı ve teslim alırken görülen durum kayda geçiyor —
        "bu çizik bende olmadı" tartışmasının tek cevabı
      · Çıkış ağırlığı ayrı tutuluyor, fark gösteriliyor (lehim eklenir,
        parça çıkar; anormal olan farkın açıklanamaması)
      · Emanet mal stok defterine GİRMEZ — sahibi müşteri; girseydi stok ve
        pozisyon raporları şişerdi
      · Teslimde "kime verdik" zorunlu (uygulama + veritabanı kısıtı)
      · Fiyat verilmeden onay kaydedilemiyor
      · Takip kodu ardışık değil (iş hacmi sızdırmaz) ve O/0, I/1 içermiyor
        (kod telefonda okunuyor)

## 📋 Paket 10 — Entegrasyonlar

- [ ] Banka sanal POS adaptörü ⏳ *entegrasyon dokümanı*
- [ ] e-Fatura (özel matrah kırılımı birebir) ⏳ *entegratör hesabı*
- [x] Kargo çekirdeği: beyan değeri + takip + paketleme kanıtı + teslim kaydı
      · Otomatik etiket ve taşıyıcı webhook'u için sözleşme/API bekliyor
- [x] İade / iptal çekirdeği (ürün tipine göre testli cayma ön elemesi,
      müşteri talebi, yönetim kuyruğu, denetim izi; sağlayıcı iadesi webhook'a bağlı)
- [ ] Pazaryeri senkronizasyonu (Trendyol, Hepsiburada)
- [ ] Yedek kur sağlayıcı (Hasfiyat / altinapi)
- [ ] Hata izleme (Sentry) + alarm kuralları

## 🔨 Paket 11 — Kalite ve uyum

- [x] Sipariş numarasından bağımsız, 256-bit HttpOnly sipariş erişim anahtarı
- [x] Kimlik ve ekspertiz uçlarında aynı sipariş erişim doğrulaması
- [x] Sepette ve siparişte aynı sunucu taraflı sigortalı kargo hesabı
- [x] Canonical/OG üretimi için ortam bazlı `metadataBase`

- [x] **Teslimat adresi toplama** + doğrulama (81 il, cep telefonu, zorunlu alanlar)
- [x] Müşteri kaydı (aynı e-posta tekrar sipariş verirse aynı kayda bağlanır)
- [x] **Mesafeli satış + ön bilgilendirme onay kaydı** (sürüm + IP + zaman)
- [x] Yasal metin sayfaları (6 sayfa) — ⚠️ taslak, avukat onayı bekliyor
- [x] **Çerez onayı** (karanlık desen yok, iki seçenek eşit ağırlıkta)
- [x] KVKK açık rıza kaydı (ticari ileti + stok bildirimi)
- [x] **WCAG 2.1 AA denetimi** — 14 sayfa × 2 tema × 2 genişlik, 0 ihlal
      · Bulunan ve düzeltilen: `ink-subtle` kontrastı (açık 3.01:1, koyu 4.08:1)
      · Kontrast artık birim testle korunuyor — göz kararına bırakılmadı
- [x] **Performans bütçesi** (`npm run perf:check`) — JS 144/200, font 165/180, CSS 7/40 KB
      · Fraunces 700 ağırlığı hiç kullanılmıyordu, kaldırıldı (−34 KB)
      · `latin-ext` KORUNDU: Türkçe ğ/ş/ı/İ o alt kümede
- [ ] Gerçek LCP/CLS/INP ölçümü ⏳ *alan verisi; yayından sonra PageSpeed/CrUX*
- [x] **KVKK veri sahibi talep akışı** (`/kvkk-basvuru`) — erişim/düzeltme/silme
      · Çift doğrulama; doğrulanmayan başvuru İŞLEME ALINMAZ
      · 30 günlük yasal süre takibi, gecikme uyarısı (m.13)
      · **Silinemeyen veri dürüstçe ayrılıyor** (VUK faturası, MASAK kimlik)
      · Cevap metni otomatik üretiliyor — "neyi sildik, neyi neden saklıyoruz"
- [x] **Hız sınırlama** — veritabanı tabanlı, atomik; sunucusuz ortamda çalışır
      · Giriş, 2FA, abonelik, yorum, kimlik, arama, sipariş uçlarında
      · Eşzamanlı isteklerde sayaç kaçırmıyor (testle kanıtlı)
- [x] **CSP + güvenlik başlıkları** — `object-src none`, `base-uri self`,
      `form-action self`, `frame-ancestors none`, `connect-src self`,
      Permissions-Policy; panel ve sipariş sayfalarına `no-store`
      ⚠️ `script-src` nonce'suz — sebebi kodda açıklandı (statik kabuk mimarisi)
- [ ] CSRF ⏳ *Next Server Action'ları Origin doğrulaması yapıyor; ek katman değerlendirilecek*
- [x] Yayına çıkış kapısı (`URETIM-STANDARTLARI.md` §7) — `npm run release:check`
      · Kapı yazıldı ve çalışıyor; şu an **kırmızı**: üretim ortam değişkenleri
        (sanal POS, e-Fatura, kargo, e-posta, Sentry, şifreleme anahtarı) girilmedi.
        Kırmızı olması doğru davranış — bunlar dışarıdan gelecek.

## 📋 Paket 12 — Kanal genişleme

- [x] **Çok dilli arayüz (TR/EN)** — `/en` altında katalog, ürün, kategori, arama,
      sepet, sipariş, hediye bulucu, taş envanteri, yüzük tasarla, charm, randevu
      ve tüm bilgi sayfaları; sitemap'te `alternates` ile eşleştirildi
      · Çevirisi olmayan ürün TR metniyle gösterilir ve sayfada bunu **açıkça
        söyler** — sessizce 404 vermez (denetimde bulunup düzeltildi)
      · Liste sayfaları yalnızca EN çevirisi olan ürünleri gösterir; boş liste
        dürüst bir boş durum metni gösterir
      · ⚠️ Kalan iş kod değil **içerik**: ürünlerin EN çevirisi yönetim
        panelindeki EN alanlarından girilecek
- [x] ~~Çok para birimli satış (TRY/USD/EUR)~~ — **bilinçli olarak kaldırıldı**
      · Tahsilat zaten TRY; ikinci bir canlı kur bağımlılığı riski artırıyordu
      · Hedef kitle yurt içi. Yurt dışı satış gerçekten başlarsa yeniden ele alınır
      · `lib/i18n/money.ts` TRY'ye sabitlendi; çağrı arayüzü korundu
- [ ] Yurt dışı kargo + mikro ihracat (ETGB)

---

## Satın alınacaklar (yazılmayacak)

Arama (Algolia/Typesense) · Görsel CDN (Cloudinary) · E-posta/SMS (Klaviyo) ·
Yorumlar (Okendo/Judge.me) · 3D/AR (Threedium/Zakeke) · Hata izleme (Sentry)

> **Kural:** İşimizi rakipten ayıranı yazarız (fiyat motoru, özel matrah, MASAK, stok),
> ayırmayan her şeyi satın alırız.
