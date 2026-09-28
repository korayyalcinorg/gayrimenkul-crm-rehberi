# Gayrimenkul CRM Seçim Kriterleri

CRM seçimi “hangi ürünün özellik listesi daha uzun?” yarışı değildir. Gayrimenkul ofisinde asıl kayıp; lead’in sahiplenilmemesi, geç ilk temas, mükerrer kayıt ve satılan dairenin reklama geri beslenmemesidir.

Bu bölüm ürün kataloğu değil; **operasyonel seçim çerçevesidir**. Yazılımı şu soruya göre eleyin: *Lead ofise düştükten sonra sistem sızıntıyı otonom kapatıyor mu, yoksa ekip hâlâ manuel yangın mı söndürüyor?*

## Beş zorunlu operasyon kriteri

### 1. Round-Robin (veya kurala dayalı) otomatik atama

Yeni lead’in “kim alır?” tartışmasıyla beklemesi hızı öldürür.

- Sahiplik alanı (`assigned_owner`) zorunlu ve görünür olmalı.
- İş yükü / nöbet / proje / dil / ülke koduna göre kural tanımlananabilir olmalı.
- Gece ve hafta sonu kuyruğu aynı politikaya bağlanmalı.
- Yeniden atama (SLA aşımı, izin, no-response) tetiklenebilir olmalı.

### 2. 15 dakikalık Speed-to-Lead SLA + eskalasyon

İlk temas için hedef süre ölçülmeden “hızlıyız” iddiası denetlenemez.

- Lead oluşturulma zamanı ile ilk nitelikli temas zamanı ayrı tutulmalı.
- Medyan yetmez; **P90** da izlenmeli.
- SLA aşımında satış müdürüne bildirim / yeniden atama olmadan KPI dekoratif kalır.
- Ofisinizin bandı farklıysa (ör. 5 dk / 30 dk) eşiği yazın; ölçüm ve alarm yine zorunludur.

### 3. WhatsApp Business API ile ilk temas + insan handoff

Çoğu aday formdan sonra WhatsApp’tan devam etmek ister.

- İlk HSM / şablon karşılama saniye bazında ölçülebilmeli.
- Otomatik mesaj + kısa sürede insan handoff birlikte tasarlanmalı; bot yazıp kimsenin dönmediği senaryo pahalı sızıntıdır.
- Konuşma kaydı lead kartına bağlanmalı; WhatsApp “kişisel not alanı” olmamalı.

### 4. Telefon ve e-posta bazlı deduplication (mükerrer engeli)

Aynı kişinin Meta ve Google’dan ikinci kez gelmesi sahiplik krizine yol açar.

- Numara standardizasyonu (`+90`, `+44`, boşluk/tire temizliği) giriş anında yapılmalı.
- Eşleşen kayıt tek karta birleşmeli veya mevcut sahibe bildirim düşmeli.
- Dedup yoksa Round-Robin iki danışmana aynı adayı verir; bu komisyon kavgası değil, veri hijyeni problemidir.

### 5. Meta CAPI (veya eşdeğer) offline satış geri bildirimi

Sözleşme kapandığında satışın reklama geri gitmesi kapalı döngüdür.

- Closed-Won → platform offline conversion senkronu doğrulanabilir olmalı.
- Bu bağ kopuksa optimizasyon CPL’ye kilitlenir; gerçek gelir sinyali platforma ulaşmaz.
- Kampanya / ad set / creative kimliği lead kaydında korunmalı (UTM + click id).

## 10 kriterlik değerlendirme matrisi (özet)

Puanlama önerisi: her satır **0 = yok / eklentiyle belki**, **1 = kısmi**, **2 = yerel (native) ve sahada test edildi**.

| # | Kriter | Ne sorulur? | Min. beklenen |
|---|---|---|---|
| 1 | Otomatik atama | Round-Robin veya kural motoru native mi? | Lead ortada kalmıyor |
| 2 | SLA ölçümü | İlk temas zaman damgası + P90 rapor var mı? | Alarm / eskalasyon |
| 3 | WhatsApp | API + şablon + lead kartına kayıt? | Handoff süresi ölçülüyor |
| 4 | Dedup | Telefon/e-posta normalize + birleştirme? | Çift sahiplik engeli |
| 5 | Kapalı döngü | Closed-Won → Meta/Google offline? | Gelir sinyali platformda |
| 6 | Lead capture | Webhook / native lead ads; aracı kopya yok mu? | Kaynak + UTM korunuyor |
| 7 | Pipeline | Gayrimenkule uygun aşamalar özelleşiyor mu? | Lost reason zorunlu |
| 8 | Çok dil / ülke | Dil ve ülke koduna routing? | KKTC / uluslararası ofisler |
| 9 | Raporlama | Contact / Appointment / Show-up / CPS? | Temsilci bazlı görünüm |
| 10 | Kurulum maliyeti | 30 günde canlı döngü mümkün mü? | Önce sızıntı, sonra eklenti |

İndirilebilir puanlama şablonu: [templates/crm-secim-matrisi.csv](../templates/crm-secim-matrisi.csv)

> Yerel (native) kabiliyet ile “eklentiyle belki” arasındaki fark, üç ay sonra operasyon maliyetinde görünür.

## İş modeline göre kısa yönlendirme

| İş modeli | Ağırlık verin | Tipik sınıf (örnek) |
|---|---|---|
| Markalı konut / yüksek hacimli dijital lead | Round-Robin + SLA + CAPI | Bitrix24 / Salesforce / Zoho sınıfı |
| KKTC ve uluslararası yatırımcı | Çok dilli WhatsApp, dövizli plan, ülke routing | Çok dil + WhatsApp güçlü CRM’ler |
| Klasik emlak ofisi / franchise | Portföy paylaşımı, ilan / MLS çıkışı | RE-OS tipi + operasyon katmanı |
| Bireysel danışman | Mobil hız, ilan tarama | Hafif CRM; sahiplik + ilk temas yine şart |

Ürün adı örnekleri **sınıf** belirtmek içindir; bu rehber tek bir yazılımı dayatmaz. Seçim, yukarıdaki beş zorunlu kriterin sahada kanıtına göre yapılmalıdır.

## Önerilen kurulum sırası

1. Meta / Google / site formlarını aracı panoya kopyalamadan **webhook / native** ile CRM’e bağlayın.
2. Dedup kurallarını açın; mükerrerleri mevcut sahibe bildirim olarak ekleyin.
3. Round-Robin + 15 dk SLA alarmını **gerçek lead** ile test edin.
4. WhatsApp karşılama + insan handoff süresini saniye bazında ölçün.
5. Closed-Won → Meta CAPI (veya eşdeğer) offline senkronunu doğrulayın.

Bu sıra “önce tüm özellikleri açalım” yaklaşımından daha ucuzdur: önce sızıntıyı kapatan döngü, sonra stok haritası ve kat planı gibi sektör eklentileri.

## Pratik çıkarım

Gayrimenkulde kazanan sistem; lead’i adil dağıtır, 15 dakikayı (veya ofis bandını) ölçer, WhatsApp ile ilk teması kurar, mükerreri tek kartta tutar ve satışı reklama geri besler. Bu beş madde kapalıysa yazılım değişimi çoğu zaman makyajdır.

## İlgili kaynaklar

- Tam karşılaştırma ve uzun matris: [Gayrimenkul Firmaları İçin CRM Seçim Rehberi ve Karşılaştırma Matrisi](https://www.korayyalcin.org/yayinlar-arastirmalar/gayrimenkul-firmalari-icin-crm-secim-rehberi/)
- Bu repoda: [04 Speed-to-lead](04-speed-to-lead.md), [05 Lead routing](05-lead-routing.md), [08 WhatsApp CRM](08-whatsapp-crm-otomasyonu.md), [12 Kurulum kontrol listesi](12-crm-kurulum-kontrol-listesi.md)
- İlişkili açık veri / vaka (CRM deduplication): Zenodo DOI `10.5281/zenodo.22850349`
