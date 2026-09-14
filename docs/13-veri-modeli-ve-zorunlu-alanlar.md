# CRM Veri Modeli ve Zorunlu Alanlar

CRM başarısının önemli bir kısmı doğru veri modeline bağlıdır. Çok az alan tutulursa analiz yapılamaz; çok fazla zorunlu alan ise satış ekibinin sistemi kullanmasını zorlaştırabilir.

## Kimlik alanları

- ad_soyad
- telefon
- e_posta
- ulke
- sehir
- dil

## Kaynak alanları

- lead_source
- platform
- campaign_name
- ad_set_name
- ad_name
- form_name
- utm_source
- utm_medium
- utm_campaign
- first_source
- last_source

## Gayrimenkul ihtiyaç alanları

- proje
- bolge
- mulk_tipi
- oda_sayisi
- butce_min
- butce_max
- pesinat
- satin_alma_zamani
- yatirim_amaci

## Operasyon alanları

- assigned_user
- crm_stage
- first_contact_at
- last_contact_at
- next_action_at
- nr_count
- appointment_at
- lost_reason

## Hangi alanlar zorunlu olmalı?

Yeni lead geldiği anda yalnızca gerçekten gerekli alanlar zorunlu olmalıdır. Qualification sırasında ek alanlar doldurulabilir.

Örnek:

### Lead oluşturulurken
- Telefon veya e-posta
- Kaynak
- Kampanya
- İlgilenilen proje

### Contacted olduğunda
- Ülke
- Dil
- İhtiyaç / proje

### Qualified olduğunda
- Bütçe
- Satın alma zamanı
- Amaç

### Lost olduğunda
- Kayıp nedeni

## First-touch ve last-touch ayrımı

İlk kaynak, müşterinin CRM'e ilk kez hangi kanaldan girdiğini gösterir. Son kaynak ise son etkileşim veya yeniden kazanım kanalını gösterebilir. İkisini tek alanda sürekli ezmek attribution analizini bozar.

## Veri hijyeni

- Telefonları ülke kodu ile normalize edin.
- E-posta formatını doğrulayın.
- Duplicate kayıtları birleştirin.
- Serbest metin yerine mümkün olduğunca seçenek listeleri kullanın.
- Kayıp nedenlerini standardize edin.
- Kullanılmayan alanları periyodik temizleyin.
