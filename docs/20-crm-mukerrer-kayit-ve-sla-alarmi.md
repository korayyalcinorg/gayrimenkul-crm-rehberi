# CRM Mükerrer Kayıt (Deduplication) ve SLA Alarmı

Aynı kişi Meta'dan, Google'dan ve bir ilan portalından art arda form bıraktığında CRM bunu üç yeni lead sayarsa üç temsilci aynı insanı arar, sahiplik dağılır ve ilk yanıt süresi ölçülemez hale gelir. Bu bölüm, kaydı içeri almadan önce eşleştirmeyi, mükerrer başvuruyu yeni lead yapmak yerine mevcut karta bağlamayı ve yalnızca gerçekten yeni olan kayıtta SLA sayacını başlatmayı anlatır.

Atama kurallarının kendisi [Bölüm 05 — Lead routing](05-lead-routing.md) içindedir; burada o bölüm tekrarlanmaz. İlk temasın ne sayılacağı [Bölüm 04 — Speed-to-lead](04-speed-to-lead.md) tanımına bırakılmıştır.

> Kaynak vaka: Koray Yalçın, *Vaka Analizi: İnşaat Projesinde CRM Lead Sızıntılarını Önleme ve Temsilci Performans Analizi* (2026) — [korayyalcin.org](https://www.korayyalcin.org/yayinlar-arastirmalar/case-study-insaat-firmasi-crm-deduplication-ve-lead-routing/) · DOI: [10.5281/zenodo.22850349](https://doi.org/10.5281/zenodo.22850349)

Kaynak vaka, 450 konut ve ticari üniteden oluşan karma bir inşaat projesinde 120 gün ve 5.800 dijital lead üzerinde telefon/e-posta eşleştirmesi, round-robin atama ve 15 dakikalık SLA alarmını birlikte uygulamıştır. Aşağıda sayılar yalnızca o vakadan aktarılmıştır; kendi ofisinizde aynı taban çizgisini ölçmeden genellemeyin.

---

## Eşleştirme anahtarları

Eşleştirme, serbest metin karşılaştırması değildir. Kayıt CRM'e yazılmadan önce üç anahtar üretilir. Uygulanabilir satırlar: [mükerrer kayıt eşleştirme kuralları](../templates/mukerrer-kayit-eslestirme-kurallari.csv).

### 1. Telefon — E.164

Vakada mükerrerliğin ana kaynağı, aynı kişinin Meta ve Google reklamlarına tekrar tıklamasıdır. Telefon bu yüzden birincil anahtardır.

- Boşluk, tire, parantez ve baştaki `00` temizlenir; ülke kodu eklenerek [E.164](https://www.itu.int/rec/T-REC-E.164) biçiminde saklanır (örnek: `05xx xxx xx xx` → `+905xxxxxxxxx`).
- Karşılaştırma yalnızca normalize edilmiş değerde yapılır. Ekranda yerel biçim gösterilebilir; anahtar yerel biçimde tutulmaz.
- Eşleşme tipi tam eşleşmedir. "Bir hane bir hat" durumunda (eş, kardeş, ofis santrali) otomatik birleştirme yapılmaz; kayıt bağlanır ve incelemeye düşer.
- Ham değer silinmez. Normalize anahtar ayrı alanda durur; böylece yanlış ülke kodu sonradan düzeltilebilir (bkz. [Bölüm 13](13-veri-modeli-ve-zorunlu-alanlar.md)).

### 2. E-posta — küçük harf

- Baş ve sondaki boşluk atılır, tamamı küçük harfe çevrilir.
- Eşleşme tam eşleşmedir. Nokta veya `+` takma adını aynı adres saymak ofis kararıdır; varsayılan kural bunu yapmaz, çünkü farklı kişileri birleştirebilir.
- Telefon tutmuyorsa bile aynı e-posta, vakadaki kural gereği ikinci bir lead açmaz.

### 3. Ad + proje — bulanık, otomatik birleştirmez

Telefon ve e-posta yoksa veya çelişiyorsa ad soyad ile proje birlikte bakılır.

- Türkçe harfler korunarak küçük harfe çevrilir (`İ`/`ı` karışması ayrı ele alınır), fazla boşluklar teke indirilir.
- Proje kimliği tam eşleşmelidir. Farklı projedeki benzer ad, aynı kişi sayılmaz.
- Ad benzerliği yalnızca inceleme kuyruğu üretir. Takma ad, aile bireyi ve saha yazım hatası birbirine benzediği için bu anahtar tek başına merge tetiklemez.

Vakanın uyguladığı otomatik kural telefon **veya** e-postadır: aynı numara veya aynı e-posta ile gelen 2. ve 3. başvurular yeni lead açılmadan, mevcut temsilcinin kartına "Aynı Reklama Tekrar Tıkladı" aktivitesi olarak eklenmiştir.

## Birleştirme (merge) ve bağlama (link)

| | Bağla (link) | Birleştir (merge) |
|---|---|---|
| Ne zaman | Telefon veya e-posta mevcut açık kayda tam oturuyorsa; ya da ad+proje yalnızca benziyorsa | Bir insan, iki kartın aynı kişi olduğuna karar verdikten sonra |
| Kayıt sayısı | İkinci başvuru ayrı lead olmaz; kaynak, kampanya ve zaman mevcut karta aktivite olarak yazılır | Yinelenen kart kapanır veya arşivlenir; alanlar hayatta kalan karta, aşağıdaki kuralla taşınır |
| Sahiplik | Değişmez. Yeni temsilciye round-robin uygulanmaz | Hayatta kalan kartın sahibi korunur; birleştirme bahanesiyle sahip değiştirilmez |
| SLA | Yeni sayaç açılmaz | Kapanan kartın sayacı düşer; hayatta kalan kartın sayacı sıfırlanmaz |

Otomatik iş yalnızca bağlamadır. Merge, inceleme kuyruğundan ve kayıtlı bir gerekçeyle yapılır.

## Hangi kayıt ayakta kalır

Hayatta kalan kayıt, eşleşen **en eski açık** kayıttır. Gerekçe: ilk teması kuran temsilcinin emeği ve müşterinin bağlamı o karttadır.

- Sahip, aşama ve notların üzerine yazılmaz.
- Boş alanlar yeni başvurudaki değerle doldurulabilir. Dolu alan ancak çelişki kuyruğunda ve elle güncellenir.
- Yeni başvurunun kaynağı, kampanyası, ilan veya form kimliği ve zaman damgası aktivite olarak eklenir; ham başvuru silinmez.
- Satışı kapanmış kartta yeni bir ilgi gelirse, vakadaki gibi bunu sessizce başka temsilciye yeni lead diye açmayın. Ya aynı kişi kaydına bağlı yeni bir fırsat açın ya da mevcut sahibe aktivite düşün; ofis bu iki seçenekten birini yazılı seçer ve ikisini birden çalıştırmaz.

## Temsilciler arası lead sızıntısı

Vakada taban çizgide lead'lerin %32,0'ı aranmadan veya takip tarihi verilmeden sistemde kalmıştır (takipsiz kayıp / veri sızıntısı). Mükerrer kart, sızıntının ikinci yüzüdür: ikinci temsilci "yeni lead" sandığı kaydı sahiplenir, birinci temsilci habersiz kalır, müşteri iki farklı hikâye duyar.

Bunu kesen kurallar:

- Eşleşme varsa kayıt routing kuyruğuna girmez ve ikinci bir sahip almaz.
- Aktiviteyi mevcut sahip ve satış müdürü görür. Başka temsilcinin açık listesine düşmez.
- Kişisel WhatsApp veya Excel'e kopyalanan numara CRM dışı sızıntıdır; takip kaydı kartta değilse yapılmış sayılmaz (izinli iletişim için [Bölüm 15](15-veri-koruma-ve-izinli-iletisim.md)).
- Portalın kendi lead kimliği kişi anahtarı değildir. Üç portal, üç farklı kimlik gönderebilir; kişi anahtarı normalize telefon veya e-postadır.

## Lead routing ile sıra

Sıra sabittir: önce eşleştirme, sonra — ve yalnız eşleşme yoksa — atama.

```text
Meta / Google / portal / web formu
            ↓
   normalize telefon + e-posta
            ↓
      tam eşleşme var mı?
      ├── evet → mevcut karta aktivite (routing yok, yeni sahip yok)
      └── hayır → Bölüm 05 atama kuralları
                     ↓
              tek sahip + SLA sayacı
```

Round-robin, vaka mimarisinde yalnızca yeni kayıt içindir ve temsilcinin anlık iş yükü ile nöbet çizelgesine göre çalışır. Mükerrer başvuru bu dağıtıma girmez; mevcut temsilciye aktivite olarak eklenir. Atama ölçütlerinin listesi ve örnek mantık [Bölüm 05](05-lead-routing.md) dosyasındadır.

## SLA alarmı

Sayaç, kaydın oluşturulup tek sahibe atandığı anda başlar. Mükerrer başvuruda yeni sayaç açılmaz; mevcut sahibin kartına görev düşebilir, fakat bu görev kaydı başka bir temsilciye devretmez ve süreyi sıfırdan başlatmaz.

### İlk yanıt eşiği

Vakada satış SLA sayacı **15 dakikadır**: 15 dakika içinde aranmayan lead için satış müdürüne sesli veya mesajlı ihlal uyarısı gitmiştir. İlk temasın tanımı (aramayı denemek ile iki yönlü konuşma) [Bölüm 04](04-speed-to-lead.md) ile aynı tutulmalıdır; aksi halde alarm "aranmış gibi" kapanır.

Ofisin bandı farklıysa eşiği yazılı değiştirin. Eşik yoksa alarm da yoktur. Vakadaki 15 dakika bir hedef örneğidir, her ofisin ölçülmüş sonucu değildir.

### Eskalasyon

Vakanın uyguladığı adım, süre dolunca müdüre otomatik uyarıdır. Tasarımda adımlar birbirinin yerine geçmez:

1. Atama anında sahip bildirilir, sayaç başlar.
2. Eşik (vakada 15 dakika) dolmuş ve nitelikli arama girişimi yoksa satış müdürüne uyarı gider. Uyarı, kaydı kapatmaz.
3. Müdürün gördüğü kartta sahip, kaynak ve geçen süre durur; "kimdeydi?" sorusu Excel'e düşmez.

### Yeniden atama

Vaka metni otomatik yeniden atamayı sonuç olarak raporlamaz; raporlanan müdahale müdür uyarısıdır. Yeniden atama, o uyarıdan sonra işletilmesi gereken ayrı bir ofis kuralıdır:

- Hâlâ aranmamış kayıt, nöbetçi ve kapasitesi uygun bir temsilciye geçer.
- Eski sahip notu kartta kalır; iki açık sahip aynı anda olmaz.
- Yeniden atama yeni bir lead açmaz ve eşleştirme anahtarını değiştirmez.
- Mükerrer diye bağlanmış aktivite, yeniden atama kuyruğuna alınmaz.

## KPI

Aşağıdaki tablo kaynak vakanın kendi öncesi / sonrası ölçümüdür (450 konut ve ticari ünite, 120 gün, 5.800 dijital lead). Başka bir ofisin hedefi olarak kopyalanmaz.

| Gösterge | Müdahale öncesi | Müdahale sonrası | Vakanın yazdığı değişim |
|---|---|---|---|
| Mükerrer kayıt oranı | %24,0 | %2,1 | −21,9 puan |
| Takipsiz veri sızıntısı | %32,0 | %0,0 | sızıntı sıfırlandı |
| Ortalama ilk iletişim süresi | 28 saat | 14 dakika | %99,1 hızlanma |
| Temsilci başına günlük arama | 18 | 42 | 2,33 kat |
| Toplam satış dönüşüm oranı | %1,10 | %2,90 | 2,63 kat |

İzlenmesi gereken, vakada hedef değer verilmeden tanımlı tutulan operasyon göstergeleri: eşleşip bağlanan başvuru oranı, inceleme kuyruğunda bekleyen ad+proje sayısı, SLA ihlalinde müdür uyarısının gecikmesi, yeniden atamadan sonra tek sahibi olan kayıt oranı. Bunlar için bu bölümde hedef sayı yoktur; ofis kendi taban çizgisini yazmalıdır.

## Kontrol listesi

- [ ] Telefon E.164 anahtarı girişte üretiliyor, ham numara silinmiyor
- [ ] E-posta trim + küçük harf ile tam eşleşiyor
- [ ] Ad + proje yalnızca inceleme kuyruğu açıyor, otomatik merge yok
- [ ] Telefon veya e-posta eşleşince yeni lead açılmıyor; aktivite mevcut karta yazılıyor
- [ ] Eşleşen kayıt [Bölüm 05](05-lead-routing.md) dağıtımına girmiyor
- [ ] Hayatta kalan kayıt en eski açık kart; sahip ve aşama üzerine yazılmıyor
- [ ] Yeni kayıtta SLA sayacı atama anında başlıyor (vakadaki örnek eşik: 15 dakika)
- [ ] Eşik aşımında satış müdürüne uyarı gidiyor
- [ ] Yeniden atama kuralı yazılı; aynı anda iki sahip olmuyor
- [ ] Portal lead kimliği kişi anahtarı olarak kullanılmıyor
- [ ] Kurallar [CSV şablona](../templates/mukerrer-kayit-eslestirme-kurallari.csv) işlendi

## Ek: PropTech araçları ve CRM entegrasyonu

PropTech (property technology), mülkün pazarlanması ve işlemesinin dijital katmanıdır: sanal tur, canlı envanter, e-imza ve ilan dağıtımı aynı kişi için birden fazla "başvuru" üretebilir. Bu ek, o araçların mükerrer kayıt ve SLA sayacını nasıl etkilediğini bağlar. Ayrıntılı çerçeve: [PropTech nedir ve gayrimenkul sektörünü nasıl değiştiriyor?](https://www.korayyalcin.org/yayinlar-arastirmalar/proptech-nedir-gayrimenkul-sektorunu-nasil-degistiriyor/)

Aynı makale, PropTech yatırımlarının satış döngülerini ortalama %35 kısalttığını belirtir. Bu oran bu rehberde yeniden ölçülmemiştir; aşağıdaki maddeler oran iddia etmez.

### Sanal tur

Makale, fiziksel ziyaret olmadan gezilen sanal turları (örnek araç: Matterport veya benzeri) ve müşterinin hangi projenin hangi planına ne kadar baktığının CRM'e aktarılmasını önerir. Tur formundan gelen telefon, web formundaki telefonla aynı kişi olabilir. Tur oturumu ayrı bir lead açarsa ikinci sahip ve ikinci SLA doğar. Doğru bağ: tur izleme ve "planı inceledi" olayı, eşleşen kartta aktivitedir; sayaç yalnızca eşleşme yoksa ve kayıt yeni sahibe atandıysa başlar.

### Envanter yönetimi ve CRM senkronu

Makale, mülk portföyünün CRM'de dijital ve web sitesiyle senkron tutulmasını önerir. Envanter kaydı (ünite, durum, fiyat) kişi kaydı değildir. Stok güncellemesi veya "bu üniteyi görüntüledi" olayı yeni lead açmamalı, açık karttaki proje alanına veya aktiviteye yazılmalıdır. Ünite kimliği kişi anahtarı yapılmaz; aksi halde aynı alıcı üç daireye bakınca üç lead olur.

### E-imza

Makale, sözleşme için DocuSign veya Adobe Sign gibi e-imza araçlarını anar. İmzadaki ad, formdaki kısa addan farklı durabilir; telefon biçimi de dağınık gelebilir. İmza paketi yeni lead değildir ve SLA başlatmaz. Taraf, normalize telefon veya e-posta ile mevcut kişiye bağlanır; bağlanamazsa inceleme kuyruğuna düşer, satış temsilcisinin açık listesine "taze talep" diye girmez.

### Portal ve ilan entegrasyonu

Kaynak vaka, dijital kaynağı Meta, Google ve portal olarak aynı eşleştirme kapısına sokar. Aynı ilan birden fazla portala gittiğinde her portal kendi lead kimliğini üretir. Kişi anahtarı yine normalize telefon veya e-postadır. Eşleşme varsa başvuru mevcut sahibin kartına aktivite olur, routing çalışmaz. Eşleşme yoksa kayıt yeni lead'dir, [Bölüm 05](05-lead-routing.md) onu atar ve SLA o atamada başlar. Portal kimliğini "yeni kişi" sanmak, vakada kapatılmak istenen çift aramanın ilan tarafındaki halidir.

---

**Atıf:** Yalçın, K. (2026). *Vaka Analizi: İnşaat Projesinde CRM Lead Sızıntılarını Önleme ve Temsilci Performans Analizi.* KorayYalcin.org Vaka Araştırmaları Serisi. https://doi.org/10.5281/zenodo.22850349

PropTech notu: Yalçın, K. (2026). *PropTech Nedir ve Gayrimenkul Sektörünü Nasıl Değiştiriyor?* KorayYalcin.org Yayınlar ve Araştırmalar. https://www.korayyalcin.org/yayinlar-arastirmalar/proptech-nedir-gayrimenkul-sektorunu-nasil-degistiriyor/
