# CRM Segmentli Retargeting ve Offline Dönüşüm Geri Beslemesi

Reklam platformları yalnızca "form doldurdu" sinyalini görürse, form dolduran herkesi aynı değerde sayar ve ucuz ama niteliksiz lead getirmeye devam eder. Bu bölüm, CRM'deki satış aşamalarını reklam platformlarına geri göndererek (kapalı devre / closed-loop) ve retargeting kitlelerini CRM segmentlerinden üreterek bu sorunu nasıl çözebileceğinizi anlatır.

> Kaynak vaka: Koray Yalçın, *Vaka Analizi: Lüks Gayrimenkul Projesinde Programatik Retargeting ve ROAS Optimizasyonu* (2026) — [korayyalcin.org](https://www.korayyalcin.org/yayinlar-arastirmalar/case-study-gayrimenkul-programmatic-retargeting-roas-optimizasyonu/) · DOI: [10.5281/zenodo.22850353](https://doi.org/10.5281/zenodo.22850353)

---

## Neden gerekli?

- Jenerik retargeting, sitedeki herkese aynı logo banner'ını gösterir; ilgilenilen proje ve fiyat bandı bilinmez.
- Reklam algoritması satışa yaklaşan adayı tanımazsa bütçe "form dolduran ama telefonu açmayan" kitleye kayar.
- CRM'de satış temsilcisinin işaretlediği gerçek ilerleme (ofis ziyareti, teklif, kapora) en değerli optimizasyon sinyalidir.

## Mimari

```text
Web sitesi / proje detay ziyareti (first-party veri)
                 ↓
CRM + CDP eşleşmesi (Project_ID / Price_Tier / niyet grubu)
                 ↓
        ┌────────┴─────────┐
        ↓                  ↓
Offline dönüşüm         Segment bazlı
geri beslemesi          retargeting kitlesi
(Meta CAPI,             (Meta, Google, DSP /
 Google Ads offline)     açık web)
        ↓                  ↓
Algoritma nitelikli     Kişiye uygun proje,
adayı öğrenir           taksit ve teslim bilgisi
```

## 1. Segmentleri CRM'den üretin

Retargeting kitlesi "siteyi ziyaret edenler" gibi tek bir havuz olmamalıdır. Önerilen minimum alanlar:

| Alan | Açıklama | Örnek |
|---|---|---|
| `Project_ID` | Adayın ilgilendiği proje | `BODRUM-VILLA-01` |
| `Price_Tier` | Fiyat bandı / segment | `A+`, `A`, `B` |
| `Unit_Type` | İlgilenilen birim tipi | `3+1 Villa` |
| `Intent_Group` | Davranışa göre niyet grubu | `genel_ziyaret`, `kat_plani_indirdi`, `fiyat_istedi` |
| `CRM_Stage` | Güncel satış aşaması | `Qualified`, `Appointment` |
| `Consent_Marketing` | Pazarlama izni var mı? | `true` / `false` |

Örnek ayrım: sadece genel sayfayı gezen ziyaretçi ile kat planını indirip uzun süre inceleyen aday aynı kitlede olmamalı; ikincisi daha yüksek niyet grubuna alınmalıdır.

## 2. Satış aşamalarını olay olarak geri gönderin

Temsilci CRM'de aşamayı değiştirdiğinde ilgili olay reklam platformuna iletilir. Eşleştirme tablosu için [offline dönüşüm olayları şablonuna](../templates/offline-donusum-olaylari.csv) bakın.

Temel kurallar:

- **Tek kaynak:** Olayı yalnızca CRM aşama değişikliği tetiklesin; aynı olayı hem formdan hem CRM'den göndermeyin.
- **Eşleşme anahtarı:** Meta için lead ID ve/veya hash'lenmiş (SHA-256) telefon / e-posta; Google Ads için GCLID saklanmalıdır. Bu alanlar lead ilk geldiğinde CRM'e yazılmazsa geri besleme sonradan kurulamaz (bkz. [Bölüm 09](09-meta-ads-crm-entegrasyonu.md), [Bölüm 13](13-veri-modeli-ve-zorunlu-alanlar.md)).
- **Zaman damgası:** Olay zamanı, aşamanın CRM'de değiştiği andır; toplu gönderimde de bu zaman korunmalıdır.
- **Değer:** Kapora / satış gibi olaylara gerçek veya beklenen değer verin; ara aşamalara sabit, göreli değerler yeterlidir.
- **Olay adları:** Meta'nın standart olayları (`Lead`, `Contact`, `Schedule`, `SubmitApplication`, `Purchase`) karşılığı olan aşamalarda kullanılır; karşılığı olmayan aşamalar (ör. nitelikli aday, kapora) için özel (custom) olay adı tanımlanır. Aynı satışı iki kez `Purchase` olarak saymayın.
- **Geri alma:** Yanlış işaretlenen aşama düzeltildiğinde olay tekrar gönderilmemeli; bunun için olay ID'si (deduplication) kullanın.

## 3. Kreatifi segmente bağlayın

- Kitle `Project_ID` + `Price_Tier` ile tanımlıysa banner da o projenin birim tipini, taksit ve teslim bilgisini göstermelidir.
- Satışı kapanan (`Won`) ve kaybedilen (`Lost` + kalıcı neden) adaylar retargeting kitlesinden çıkarılmalıdır.
- Pazarlama izni olmayan kayıtlar kitlelere eklenmez (bkz. [Bölüm 15](15-veri-koruma-ve-izinli-iletisim.md)).

## 4. Ne ölçülmeli?

| KPI | Neden önemli? |
|---|---|
| Medyan CPL | Lead maliyeti düşerken kalite korunuyor mu? |
| Nitelikli aday oranı | Algoritmanın doğru kitleyi öğrenip öğrenmediği |
| ROAS | Harcanan reklam bütçesinin ciro getirisi |
| CAC | Bir satış için toplam edinme maliyeti |
| Geri besleme gecikmesi | Aşama değişikliği ile olayın platforma ulaşması arasındaki süre |

Kaynak vakada (İstanbul ve Bodrum lüks konut projeleri, 60 gün, 2.150 aday) CRM segmentli retargeting ve kapalı devre geri besleme sonrası raporlanan değerler: medyan CPL $42,00 → $24,30; nitelikli aday oranı %18,5 → %44,2; ROAS 3,2x → 16,8x; CAC $4.800 → $1.950. Bu sonuçlar tek bir vakaya aittir; kendi projenizde aynı baseline ölçümünü yapmadan genelleme yapmayın.

## Kontrol listesi

- [ ] Lead ID, GCLID ve UTM alanları ilk temas anında CRM'e yazılıyor
- [ ] `Project_ID`, `Price_Tier`, `Intent_Group` alanları zorunlu
- [ ] CRM aşama → reklam olayı eşleştirme tablosu yazılı
- [ ] Olaylar tekilleştirme ID'si ile gönderiliyor
- [ ] Won / Lost adaylar kitlelerden otomatik çıkıyor
- [ ] Pazarlama izni olmayan kayıtlar kitlelere girmiyor
- [ ] Geri besleme gecikmesi haftalık izleniyor

---

**Atıf:** Yalçın, K. (2026). *Vaka Analizi: Lüks Gayrimenkul Projesinde Programatik Retargeting ve ROAS Optimizasyonu.* KorayYalcin.org Vaka Araştırmaları Serisi. https://doi.org/10.5281/zenodo.22850353
