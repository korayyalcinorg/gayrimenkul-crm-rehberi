# Veri Koruma ve İzinli İletişim Notları

CRM ve pazarlama otomasyonu kurulurken yalnızca teknik akış değil, veri koruma ve iletişim izinleri de dikkate alınmalıdır.

> Bu doküman hukuki danışmanlık değildir. Uygulama yapılacak ülke ve pazara göre güncel mevzuat için hukuk danışmanıyla çalışılmalıdır.

## Temel prensipler

- Yalnızca gerekli veriyi toplayın.
- Verinin neden toplandığını açıkça belirleyin.
- Erişim yetkilerini rol bazlı yönetin.
- Eski veya gereksiz kişisel verileri süresiz tutmayın.
- Kullanıcı iletişim istemediğini belirttiğinde CRM'de görünür biçimde işaretleyin.
- Otomasyon sistemlerinde opt-out durumunu diğer kanallara da yansıtın.

## CRM'de tutulabilecek izin alanları

- communication_consent
- whatsapp_consent
- email_consent
- sms_consent
- consent_source
- consent_date
- opt_out_date

## Yetkilendirme

Herkesin tüm CRM verisini görmesi gerekmeyebilir.

Örnek roller:

- Satış temsilcisi: kendi lead/fırsatları
- Takım lideri: ekibi
- Pazarlama: kaynak ve kampanya raporları
- CRM admin: sistem yapılandırması
- Yönetim: toplu KPI ve sonuçlar

## Hassas veri minimizasyonu

Satış için gerekmeyen hassas bilgileri CRM'de toplamaktan kaçının. Serbest metin not alanlarının kontrolsüz kullanımına dikkat edin.

## Mesajlaşma otomasyonu

Bir kişinin daha önce form doldurmuş olması, her tür ve süresiz pazarlama mesajının otomatik olarak uygun olduğu anlamına gelmez. Kanal, amaç, izin ve yürürlükteki kurallar birlikte değerlendirilmelidir.
