# Meta Ads → CRM Entegrasyonu

Meta Lead Ads'ten gelen lead'lerin manuel Excel aktarımı yerine doğrudan CRM'e düşmesi, hız ve veri bütünlüğü açısından daha sağlıklı bir yapıdır.

## Temel mimari

```text
Meta Lead Ads
↓
Webhook / Connector / Integration Layer
↓
Field Mapping
↓
Duplicate Check
↓
CRM
↓
Lead Routing
↓
WhatsApp / SMS / E-posta / Arama
```

## CRM'e taşınması gereken alanlar

- Lead ID
- Form ID
- Campaign ID / Campaign Name
- Ad Set ID / Ad Set Name
- Ad ID / Ad Name
- Ad creation time
- Ad / form source
- Ad / project label
- Ad language
- Name
- Phone
- Email
- Custom form answers
- UTM bilgileri mevcutsa UTM alanları

## Field mapping

Meta formundaki alanlarla CRM alanlarının birebir eşleşmesi gerekir.

Örnek:

```text
full_name → NAME
phone_number → PHONE
email → EMAIL
project_interest → PROJECT
country → COUNTRY
campaign_name → UTM_CAMPAIGN
```

## Duplicate kontrolü

Aynı kullanıcı farklı reklam formlarını doldurabilir. CRM'e her gelişte yeni kişi açmak yerine telefon ve e-posta üzerinden tekilleştirme yapılmalıdır.

Örnek politika:

- Aynı telefon varsa mevcut kişiyi güncelle
- Yeni kampanyayı aktivite olarak kaydet
- Son ilgilendiği projeyi ayrıca sakla
- İlk kaynak ve son kaynak alanlarını ayrı tut

## Entegrasyon hataları nasıl izlenir?

- Webhook başarısızlığı
- Eksik telefon
- Hatalı ülke kodu
- CRM API limiti
- Atanmamış lead
- Duplicate conflict
- Zorunlu alan eksikliği

Bu hatalar görünür bir log veya hata kuyruğunda tutulmalıdır.

## İlgili kaynak

https://www.korayyalcin.org/yayinlar-arastirmalar/meta-ads-crm-whatsapp-entegrasyonu-nasil-calisir/
