# CRM Pipeline ve Satış Aşamaları

Gayrimenkul CRM pipeline'ı, lead'in ilk kayıttan satış veya kayıp sonucuna kadar geçtiği aşamaları gösterir.

## Önerilen temel pipeline

```text
Yeni Lead
↓
İlk Temas
↓
NR1 / NR2 / NR3
↓
Contacted
↓
Qualified
↓
Appointment
↓
Follow-up
↓
Won / Lost
```

## Aşamalar ne anlama gelir?

### Yeni Lead
Henüz satış temsilcisinin işlem yapmadığı yeni kayıt.

### İlk Temas
İlk arama veya mesaj girişimi yapılmış lead.

### NR1 / NR2 / NR3
No Response aşamalarıdır. Temas kurulamadığında lead hemen kayıp sayılmaz; kontrollü tekrar denemeleri yapılır.

### Contacted
Müşteriyle iki yönlü gerçek iletişim kurulmuştur.

### Qualified
Bütçe, ihtiyaç, zamanlama, lokasyon ve satın alma niyeti açısından satış takibine uygun bulunmuştur.

### Appointment
Telefon, online veya fiziksel görüşme planlanmıştır.

### Follow-up
Müşteri aktif olarak değerlendiriyor, teklif bekliyor veya karar sürecindedir.

### Won
Satış tamamlanmıştır.

### Lost
Fırsat kapanmıştır. Mümkünse kayıp nedeni zorunlu alan olmalıdır.

## Pipeline tasarım prensipleri

- Aynı anlamı taşıyan gereksiz aşamalar oluşturmayın.
- Her aşamanın giriş ve çıkış kriteri net olsun.
- Satış temsilcileri aynı tanımları kullansın.
- Aşama değişikliği mümkün olduğunca zaman damgası ile kayıt altına alınsın.
- Kayıp nedenleri standart listeden seçilsin.
- Follow-up aşaması sonsuz bekleme alanı olmamalıdır; bir sonraki görev tarihi bulunmalıdır.

## Örnek kayıp nedenleri

- Bütçe yetersiz
- Proje uygun değil
- Lokasyon uygun değil
- Finansman uygun değil
- Satın alma ertelendi
- Rakip proje tercih edildi
- Yanlış / geçersiz lead
- Ulaşılamadı
- İlgisini kaybetti

## Şablon

İçe aktarılabilir örnek için: [gayrimenkul-crm-pipeline.csv](../templates/gayrimenkul-crm-pipeline.csv)
