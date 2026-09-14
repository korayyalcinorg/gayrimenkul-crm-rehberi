# Gayrimenkul CRM Rehberi

Gayrimenkul şirketleri için **CRM, lead yönetimi, lead takibi, WhatsApp otomasyonu, satış süreçleri ve pazarlama otomasyonu** üzerine açık ve uygulanabilir Türkçe kaynak.

**Hazırlayan: Koray Yalçın**  
Araştırmalar ve makaleler: **https://www.korayyalcin.org**

> Bu repo, yalnızca CRM yazılımı seçimini değil; reklamdan gelen bir lead'in satışa kadar nasıl yönetileceğini sistematik biçimde anlatır.

---

## Bu rehber kimler için?

- Gayrimenkul geliştiricileri ve proje satış ekipleri
- Emlak ofisleri ve broker ekipleri
- Dijital pazarlama yöneticileri
- CRM ve RevOps ekipleri
- Tele-satış ve çağrı merkezi yöneticileri
- Meta Ads / Google Ads üzerinden lead toplayan şirketler
- WhatsApp, SMS ve e-posta otomasyonu kurmak isteyen ekipler

---

## Amaç

Gayrimenkulde sorun çoğu zaman "lead gelmemesi" değil, gelen lead'in doğru şekilde işlenmemesidir.

Sağlıklı bir sistem şu sorulara cevap vermelidir:

1. Lead nereden geldi?
2. CRM'e otomatik düştü mü?
3. Tekilleştirildi mi?
4. Kime atandı?
5. Kaç dakika içinde ilk temas yapıldı?
6. Ulaşılamadıysa hangi takip senaryosu çalıştı?
7. Müşterinin bütçesi, lokasyonu ve satın alma zamanı kaydedildi mi?
8. Randevuya dönüştü mü?
9. Satış ekibi hangi aşamada takip ediyor?
10. Kaybedilen lead'in nedeni ölçülüyor mu?

Bu repo bu yapıyı adım adım kurmak için hazırlanmıştır.

---

## Önerilen CRM akışı

```text
Meta Ads / Google Ads / Portal / Web Sitesi / WhatsApp
                         ↓
                    Lead Capture
                         ↓
             UTM + Kaynak + Kampanya
                         ↓
                 Duplicate Kontrolü
                         ↓
                    Lead Routing
                         ↓
                         CRM
                         ↓
         Telefon + WhatsApp + SMS + E-posta
                         ↓
                    Qualification
                         ↓
                     Randevu
                         ↓
                     Follow-up
                         ↓
                  Satış / Kayıp
                         ↓
                Raporlama + Öğrenme
```

---

## Rehber bölümleri

| Bölüm | Konu |
|---|---|
| [01](docs/01-gayrimenkulde-crm-nedir.md) | Gayrimenkulde CRM nedir? |
| [02](docs/02-crm-pipeline-ve-satis-asamalari.md) | CRM pipeline ve satış aşamaları |
| [03](docs/03-lead-yonetimi.md) | Lead yönetimi ve yaşam döngüsü |
| [04](docs/04-speed-to-lead.md) | Speed-to-lead ve yanıt süresi |
| [05](docs/05-lead-routing.md) | Lead routing ve otomatik atama |
| [06](docs/06-lead-scoring.md) | Lead scoring ve önceliklendirme |
| [07](docs/07-no-response-leadler.md) | No Response lead yönetimi |
| [08](docs/08-whatsapp-crm-otomasyonu.md) | WhatsApp CRM otomasyonu |
| [09](docs/09-meta-ads-crm-entegrasyonu.md) | Meta Ads → CRM entegrasyonu |
| [10](docs/10-bitrix24-gayrimenkul.md) | Bitrix24 gayrimenkul kullanım modeli |
| [11](docs/11-crm-raporlama-kpi.md) | CRM raporlama ve KPI'lar |
| [12](docs/12-crm-kurulum-kontrol-listesi.md) | CRM kurulum kontrol listesi |
| [Sözlük](docs/crm-sozlugu.md) | Türkçe CRM ve lead yönetimi sözlüğü |

---

## İndirilebilir şablonlar

- [Gayrimenkul CRM Pipeline CSV](templates/gayrimenkul-crm-pipeline.csv)
- [Lead Scoring Modeli JSON](templates/lead-scoring-modeli.json)
- [Lead Routing Kuralları JSON](templates/lead-routing-kurallari.json)
- [WhatsApp Follow-up Şablonu](templates/whatsapp-followup-sablonu.md)
- [CRM Günlük KPI Şablonu](templates/gunluk-crm-kpi.csv)

---

## Temel kavramlar

### Lead
Bir kampanya, web sitesi, portal, WhatsApp veya başka bir kanaldan iletişim bilgisi bırakan potansiyel müşteridir.

### Qualified Lead
Bütçe, ihtiyaç, lokasyon, zamanlama ve satın alma niyeti açısından satış ekibinin takip etmeye değer bulduğu leaddir.

### Speed-to-Lead
Lead'in sisteme düştüğü an ile ilk gerçek temas girişimi arasındaki süredir.

### Lead Routing
Yeni lead'in dil, lokasyon, kampanya, proje, temsilci kapasitesi veya başka kurallara göre otomatik olarak doğru kişiye atanmasıdır.

### Lead Scoring
Lead'in davranış ve profil sinyallerine göre puanlanarak önceliklendirilmesidir.

### Lead Nurturing
Henüz satın almaya hazır olmayan lead'in WhatsApp, SMS, e-posta, arama ve içeriklerle kontrollü biçimde olgunlaştırılmasıdır.

### No Response
Arama veya mesajlara henüz yanıt vermeyen, fakat doğrudan "kayıp" kabul edilmemesi gereken lead grubudur.

---

## Örnek CRM aşamaları

```text
Yeni Lead
↓
İlk Arama
↓
NR1 – Ulaşılamadı
↓
NR2 – İkinci Deneme
↓
NR3 – Son Aktif Deneme
↓
Contacted – Temas Kuruldu
↓
Qualified – Nitelikli
↓
Appointment – Randevu
↓
Follow-up – Aktif Takip
↓
Won / Lost
```

Her şirket aynı isimleri kullanmak zorunda değildir. Önemli olan aşamaların ölçülebilir olması ve satış temsilcilerinin aynı tanımları kullanmasıdır.

---

## Minimum CRM veri alanları

Bir gayrimenkul lead kaydında mümkün olduğunca şu alanlar tutulmalıdır:

- Ad soyad
- Telefon
- E-posta
- Ülke / şehir
- Tercih edilen dil
- Lead kaynağı
- Platform
- Kampanya adı
- Reklam seti / reklam
- UTM source / medium / campaign
- İlgilendiği proje
- Mülk tipi
- Bütçe aralığı
- Peşinat kapasitesi
- Satın alma zamanlaması
- Yatırım / yaşam amacı
- İlk temas zamanı
- Son temas zamanı
- Atanan satış temsilcisi
- CRM aşaması
- Randevu tarihi
- Kayıp nedeni

---

## Ölçülmesi gereken ana KPI'lar

| KPI | Ne anlatır? |
|---|---|
| Lead sayısı | Talep hacmi |
| CPL | Bir lead'in reklam maliyeti |
| İlk yanıt süresi | Operasyon hızı |
| Contact Rate | Kaç lead ile gerçek temas kurulduğu |
| Qualification Rate | Kaç lead'in nitelikli olduğu |
| Appointment Rate | Kaç lead'in randevuya dönüştüğü |
| Show-up Rate | Randevuların gerçekleşme oranı |
| Follow-up Rate | Aktif takipte kalan lead oranı |
| Sales Conversion | Lead → satış dönüşümü |
| Cost per Sale | Bir satışın pazarlama maliyeti |
| Lost Reason | Lead'lerin neden kaybedildiği |

---

## CRM sadece yazılım değildir

CRM projesinin başarısı üç katmana bağlıdır:

**1. Veri**  
Hangi alanların toplandığı ve verinin ne kadar temiz olduğu.

**2. Süreç**  
Lead geldiğinde kim, ne zaman, hangi kanaldan ve kaç kez iletişim kuracak?

**3. Otomasyon**  
Atama, bildirim, mesaj, görev, randevu ve raporlamanın ne kadarı otomatik?

Yazılım bu üç katmanı destekleyen araçtır; tek başına süreç değildir.

---

## Koray Yalçın kaynakları

Gayrimenkul CRM, lead yönetimi, WhatsApp otomasyonu ve dijital büyüme üzerine daha kapsamlı çalışmalar:

- https://www.korayyalcin.org
- https://www.korayyalcin.org/yayinlar-arastirmalar/meta-ads-crm-whatsapp-entegrasyonu-nasil-calisir/
- https://www.korayyalcin.org/yayinlar-arastirmalar/no-response-leadler-nasil-geri-kazanilir/
- https://www.korayyalcin.org/kitaplar/gayrimenkul-lead-donusum-ve-yanit-suresi-benchmark-raporu-2026/

---

## İngilizce sürüm

Uluslararası sürüm ve teknik örnekler için:

**https://github.com/korayyalcinorg/real-estate-crm-playbook**

---

## Not

Bu repodaki örnekler uygulanabilir başlangıç şablonlarıdır. Her şirket kendi satış sürecine, kullandığı CRM'e, hedef pazara, veri koruma yükümlülüklerine ve mesajlaşma izinlerine göre uyarlama yapmalıdır.
