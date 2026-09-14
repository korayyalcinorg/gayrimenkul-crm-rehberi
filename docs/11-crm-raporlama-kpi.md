# CRM Raporlama ve KPI'lar

CRM kurulmuş olması tek başına yeterli değildir. Yönetim ekibi satış hunisinin nerede tıkandığını görebilmelidir.

## Ana KPI grupları

### 1. Hacim
- Yeni lead
- Kaynak bazlı lead
- Kampanya bazlı lead
- Proje bazlı lead

### 2. Hız
- Lead → CRM gecikmesi
- CRM → atama süresi
- Atama → ilk temas süresi
- Medyan ilk yanıt süresi

### 3. Temas
- Contact Rate
- NR1 / NR2 / NR3 dağılımı
- İlk gün temas oranı

### 4. Qualification
- Qualified lead sayısı
- Qualification Rate
- Bütçe uygunluk oranı
- Satın alma zamanı dağılımı

### 5. Randevu
- Appointment sayısı
- Appointment Rate
- Show-up Rate
- İptal oranı

### 6. Satış
- Won deals
- Sales Conversion Rate
- Kaynak bazlı satış
- Proje bazlı satış
- Temsilci bazlı satış

### 7. Maliyet
- CPL
- Cost per Qualified Lead
- Cost per Appointment
- Cost per Sale

## Temel formüller

```text
Contact Rate = Contacted Lead / Total Lead

Qualification Rate = Qualified Lead / Contacted Lead

Appointment Rate = Appointment / Contacted Lead

Lead-to-Sale Conversion = Sales / Total Lead

Cost per Lead = Ad Spend / Leads

Cost per Sale = Ad Spend / Sales
```

## Dashboard'da tek başına sayı göstermeyin

Örneğin sadece "500 lead geldi" bilgisi yeterli değildir.

Daha faydalı görünüm:

```text
500 Lead
↓ %62 contacted
310 Contacted
↓ %21 appointment
65 Appointment
↓ %12 sale
8 Sales
```

Bu yaklaşım darboğazın nerede olduğunu görünür hale getirir.

## Temsilci performansı

Temsilci kıyaslarken yalnızca satış sayısına bakmak yanıltıcı olabilir. Lead hacmi ve lead kalitesi farklı olabilir.

Birlikte değerlendirin:

- Atanan lead
- Contact Rate
- Ortalama yanıt süresi
- Appointment Rate
- Follow-up disiplini
- Sales Conversion
- Kayıp nedeni kalitesi

## Günlük rapor

Örnek CSV: [gunluk-crm-kpi.csv](../templates/gunluk-crm-kpi.csv)
