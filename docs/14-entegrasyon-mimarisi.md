# CRM Entegrasyon Mimarisi

Gayrimenkul CRM sistemi tek başına çalışmaz. Reklam platformları, web formları, WhatsApp, telefon sistemi, takvim, e-posta ve raporlama araçlarıyla birlikte düşünülmelidir.

## Örnek mimari

```text
Meta Ads        Google Ads        Portallar        Web Formları
   \               |                 |                /
    \              |                 |               /
             Integration Layer
                    |
             Data Validation
                    |
              Deduplication
                    |
                  CRM
                    |
        +-----------+-----------+
        |           |           |
     WhatsApp      SMS        E-posta
        |           |           |
        +-----------+-----------+
                    |
                Sales Team
                    |
               Appointments
                    |
               Won / Lost
                    |
               BI / Reporting
```

## Integration layer neden faydalıdır?

Doğrudan her sistemi CRM'e bağlamak yerine bir entegrasyon katmanı kullanmak bazı durumlarda daha yönetilebilir olabilir.

Bu katman:

- Alan eşleştirme
- Veri temizliği
- Hata yakalama
- Duplicate kontrolü
- Loglama
- Retry mekanizması
- Routing ön işlemleri

yapabilir.

## Kullanılabilecek yaklaşımlar

- CRM'in native entegrasyonları
- Webhook
- REST API
- Make
- Zapier
- n8n
- Özel middleware

## Hata yönetimi

Entegrasyonlarda yalnızca başarılı işlemleri değil, başarısız işlemleri de takip edin.

Örnek hata kuyruğu alanları:

- event_id
- source
- created_at
- payload_status
- error_code
- error_message
- retry_count
- resolved_at

## Idempotency

Aynı webhook iki kez gelirse CRM'de iki farklı lead oluşturmamalıdır. Mümkünse platform lead ID'si veya benzersiz işlem ID'si üzerinden duplicate kontrolü yapılmalıdır.

## Loglama

En azından şu adımlar zaman damgasıyla izlenebilmelidir:

1. Lead platformda oluştu
2. Entegrasyon aldı
3. CRM'e gönderildi
4. CRM kaydı oluştu
5. Temsilciye atandı
6. İlk aksiyon yapıldı

Bu kayıtlar speed-to-lead ölçümünü daha güvenilir hale getirir.
