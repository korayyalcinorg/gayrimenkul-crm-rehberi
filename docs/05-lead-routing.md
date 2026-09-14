# Lead Routing ve Otomatik Atama

Lead routing, yeni gelen lead'in tanımlı kurallara göre doğru satış temsilcisine otomatik atanmasıdır.

## Neden gerekir?

Manuel atama;

- gecikme,
- dengesiz iş yükü,
- sahipsiz lead,
- çift arama,
- ekip içi anlaşmazlık

gibi sorunlara yol açabilir.

## Kullanılabilecek atama kriterleri

- Ülke / şehir
- Dil
- İlgilenilen proje
- Lead kaynağı
- Reklam kampanyası
- Temsilci uzmanlığı
- Çalışma saatleri
- Mevcut aktif lead sayısı
- Round-robin dağıtım
- VIP / yüksek bütçeli lead

## Örnek routing mantığı

```text
IF country = TR AND project = Karpaz
→ Türkiye ekibi

IF language = EN
→ İngilizce satış temsilcileri

IF project = Esentepe AND budget > threshold
→ Senior consultant pool

ELSE
→ round-robin
```

## Round-robin nedir?

Lead'lerin sırayla ekip üyelerine dağıtılmasıdır. Basit ve dengeli bir yöntemdir; ancak temsilci kapasitesi ve uzmanlığı hesaba katılmıyorsa tek başına yeterli olmayabilir.

## Atama sonrası SLA

Routing tamamlandıktan sonra CRM şu aksiyonları otomatik oluşturabilir:

1. Temsilciye bildirim
2. İlk arama görevi
3. WhatsApp mesaj görevi
4. Belirli sürede işlem yoksa uyarı
5. Süre aşılırsa yeniden atama veya yönetici bildirimi

## Sağlıklı routing için dikkat edilmesi gerekenler

- Aynı lead iki kişiye atanmasın.
- Temsilci izinliyse havuzdan otomatik çıkarılabilsin.
- Atama geçmişi saklansın.
- Manuel değişiklikler kayıt altına alınsın.
- VIP veya özel kampanyalar için ayrı kurallar tanımlanabilsin.

## Şablon

Örnek yapı: [lead-routing-kurallari.json](../templates/lead-routing-kurallari.json)
