# AdliSurecTakipVeYonetim
Veri Tabanı Yönetim Sistemleri Dersi
# Proje Başlığı: Adli Süreç Takip ve Yönetim Sistemi
## Proje Ekibindeki Kişiler: 
- **225260030** – Sıla Rabia Bayram
- **235260134** – İrem Alliş
- **235260182** – Merve Beril Yılmaz

## 1. Proje Kullanıcıları Gereksinimleri ve Yetkileri

Adli Süreç Takip ve Yönetim Sistemi’nde beş ana kullanıcı türü bulunur: **Dava Tarafları, Avukat, Savcı, Hakim ve Admin**. Her kullanıcı türü, veri tabanındaki verilere farklı düzeylerde erişim ve müdahale yetkisine sahiptir.

### 1.1 Dava Tarafları (Davalı, Davacı, Tanık)
- Sadece kendi dava bilgilerini görüntüleyebilir.
- Duruşma yeri, saati gibi bilgileri görüntüleyebilir.
- Yalnızca görüntüleme yetkisine sahiptir, veri girişi yapamaz.

### 1.2 Avukat
- Baktığı davalarla ilgili dosyalara ve bilgilere erişebilir.
- Randevularıyla ilgili bilgileri görüntüleyebilir.
- Müvekkil adına dosya ekleyebilir.

### 1.3 Hakim
- Baktığı davalarla ilgili dosyalara ve bilgilere erişebilir.
- Dava duruşma tarihlerini ve kararlarını güncelleyebilir.
- Tanık bilgilerini görüntüleyebilir, kararları ve ceza detaylarını kaydedebilir.

### 1.4 Savcı
- Yürüttüğü soruşturmalara erişebilir.
- Delil ve tanık bilgilerini güncelleyebilir.
- İddianameler, suçlamalar gibi dokümanları sisteme ekleyebilir.
- Davalar ve dava tarafları hakkında işlem yapabilir.

### 1.5 Admin
- Sistem genelinde tam yetkiye sahiptir.
- Kullanıcı ve rolleri yönetir.
- Tüm dava ve kullanıcı kayıtlarına erişebilir.

---

## 2. Tablolar ve Varlıkların Özellikleri

Bu veri tabanı modeli, bir dava yönetim sistemindeki davalar, kişiler, duruşmalar, belgeler ve ilişkili diğer yapılar arasındaki ilişkileri organize etmektedir. Aşağıda her bir tablonun içerdiği alanlar ve bu alanların açıklamaları yer almaktadır.

---

### 2.1 DavaDosyalari
Dava dosyalarına ait temel bilgileri içerir.
- **dosyaID**: Dava dosyasının benzersiz kimliği (Primary Key).
- **davaNo**: Dava numarası.
- **mahkemeID**: Davanın görüldüğü mahkemenin ID'si (Foreign Key, `Mahkemeler` tablosuna referans).
- **davaAcilmaTarihi**: Davanın açılış tarihi.
- **davaTürü**: Davanın türü.
- **davaDurumu**: Davanın durumu (açık, kapalı, beklemede).
  
**İlişkiler**: Bir mahkemede (adliye) birden fazla dava dosyası bulunabilir (1:N).

### 2.2 Kisiler
Sistemdeki kişilerin bilgilerini içerir.
- **kisiID**: Kişinin benzersiz kimliği (Primary Key).
- **kisiTC**: Kişinin TC kimlik numarası.
- **kisiAdSoyad**: Kişinin adı ve soyadı.
- **kisiDogumTarihi**: Kişinin doğum tarihi.
- **kisiTelefon**: Kişinin telefon numarası.
- **kisiMail**: Kişinin e-posta adresi.
- **kisiAdres**: Kişinin adres bilgisi.
- **kisiSifresi**: Kişinin sisteme giriş yapabilmesi için şifresi.
- **kisiRolu**: Kişinin rolü (avukat, hakim, savcı, davatarafları, admin).

### 3. **DavaTaraflari**  
Dava dosyalarındaki taraflara ilişkin bilgiler.
- **tarafID**: Tarafın benzersiz kimliği (Primary Key).
- **dosyaID**: Tarafın dahil olduğu dava dosyasının ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **kisiID**: Taraf olarak atanmış kişinin ID'si (Foreign Key, `Kisiler` tablosuna referans).
- **tarafRol**: Tarafın davadaki rolü (davalı, davacı, tanık).
- **ekBilgi**: Taraf hakkında ek bilgiler.

**İlişkiler**: Bir dava dosyasının birden fazla tarafı olabilir, aynı zamanda bir dava tarafının da birden fazla dava dosyası olabilir (N:N). Kişi ile dava tarafı arasında bire bir ilişki vardır (1:1).

---

### 4. **Durusmalar**  
Duruşmalarla ilgili ayrıntılar.
- **durusmaID**: Duruşmanın benzersiz kimliği (Primary Key).
- **dosyaID**: Duruşmanın ait olduğu dava dosyasının ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **durusmaTarihi**: Duruşma tarihi.
- **durusmaSaati**: Duruşmanın saati.
- **durusmaYeri**: Duruşmanın yapıldığı yer.
- **mahkemeID**: Duruşmanın görüldüğü mahkemenin ID'si (Foreign Key, `Mahkemeler` tablosuna referans).
- **hakimID**: Duruşmayı yöneten hakimin ID'si (Foreign Key, `Hakim` tablosuna referans).
- **avukatID**: Duruşmada yer alan avukatın ID'si (Foreign Key, `Avukat` tablosuna referans).
- **durusmaSonuc**: Duruşmanın sonucu (ertelendi, tamamlandı).
- **tarafID**: Duruşmada yer alan tarafın ID'si (Foreign Key, `DavaTaraflari` tablosuna referans).

**İlişkiler**: 
- Bir dosya birden fazla duruşmada kullanılabilir (1:N). 
- Bir mahkemede (adliye) birden fazla duruşma olabilir (1:N). 
- Bir hakim birden çok duruşmaya bakabilir (1:N). 
- Bir avukat birden çok duruşmaya bakabilirken aynı zamanda bir duruşmaya birden fazla avukat da bakabilir (N:N). 
- Bir dava tarafının birden fazla duruşması olabilirken aynı zamanda bir duruşmanın da birden fazla dava tarafı olabilir (N:N).

---

### 5. **Belgeler**  
Dava dosyalarına eklenen belgelerle ilgili bilgiler.
- **belgeID**: Belgenin benzersiz kimliği (Primary Key).
- **dosyaID**: Belgenin ait olduğu dava dosyasının ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **belgeTürü**: Belgenin türü (delil, ifade, karar, rapor).
- **belgeAciklama**: Belge hakkında açıklama.
- **yüklenmeTarihi**: Belgenin sisteme yüklendiği tarih.
- **kisiID**: Belgeyi yükleyen kişinin ID'si (Foreign Key, `Kisiler` tablosuna referans).

**İlişkiler**: 
- Bir belge birden fazla dava dosyasında bulunabilirken aynı zamanda bir dava dosyasında da birden fazla belge bulunabilir (N:N). 
- Bir belgeyi birden fazla kişi yükleyebilirken aynı zamanda bir kişi de birden fazla belge yükleyebilir (N:N).

---

### 6. **Kararlar**  
Davaya ilişkin verilen kararlar.
- **kararID**: Kararın benzersiz kimliği (Primary Key).
- **dosyaID**: Kararın ait olduğu dava dosyasının ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **kararTarihi**: Kararın tarihi.
- **kararDurumu**: Kararın durumu (kesin, temyiz).
- **kararDetayi**: Karar detayları.
- **hakimID**: Kararı veren hakimin ID'si (Foreign Key, `Hakim` tablosuna referans).

**İlişkiler**: 
- Bir dosyanın bir kararı vardır fakat bir karar birden fazla dosyada bulunabilir (N:1). 
- Bir kararın bir tane hakimi olabilirken bir hakim birden fazla kararda bulunabilir (N:1).

---

### 7. **Mahkemeler**  
Mahkemelerle ilgili bilgiler.
- **mahkemeID**: Mahkemenin benzersiz kimliği (Primary Key).
- **mahkemeAd**: Mahkemenin adı.
- **mahkemeAdres**: Mahkemenin adresi.
- **mahkemeTelefon**: Mahkemenin telefon numarası.
- **mahkemeMail**: Mahkemenin e-posta adresi.

---

### 8. **Loglar**  
Sistemdeki işlemlerin kaydedildiği tablo.
- **logID**: Log kaydının benzersiz kimliği (Primary Key).
- **kisiID**: İşlemi yapan kişinin ID'si (Foreign Key, `Kisiler` tablosuna referans).
- **dosyaID**: İşlemin ilgili olduğu dava dosyasının ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **logTarihi**: Log kaydının tarihi.
- **islemTipi**: İşlem tipi (oluşturma, güncelleme, silme).
- **islemDetayi**: İşlemin detayı.

**İlişkiler**: 
- Kişi ile log arasında bire bir ilişki bulunur (1:1). 
- Bir logda birden fazla dosya ile işlem yapılabilirken aynı zamanda bir dosyada birden fazla log da bulunabilir (N:N).

---

### 9. **Avukat**  
Avukatlara ilişkin bilgiler.
- **avukatID**: Avukatın benzersiz kimliği (Primary Key).
- **kisiID**: Avukat olarak kayıtlı kişinin ID'si (Foreign Key, `Kisiler` tablosuna referans).
- **mahkemeID**: Avukatın görev yaptığı mahkeme ID'si (Foreign Key, `Mahkemeler` tablosuna referans).
- **avBuro**: Avukatın bağlı olduğu büro.
- **davaSayisi**: Avukatın temsil ettiği dava sayısı.

**İlişkiler**: 
- Avukat ile kişi arasında bire bir ilişki vardır (1:1). 
- Bir avukat birden fazla mahkemede (adliye) görev alabilirken aynı zamanda bir mahkemede de birden fazla avukat görev alabilir (N:N).

---

### 10. **Savci**  
Savcılara ilişkin bilgiler.
- **savciID**: Savcının benzersiz kimliği (Primary Key).
- **kisiID**: Savcı olarak kayıtlı kişinin ID'si (Foreign Key, `Kisiler` tablosuna referans).
- **savciTuru**: Savcının türü (başsavcı, cumhuriyet).
- **dosyaID**: Savcıya atanan belge ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **mahkemeID**: Savcının görev yaptığı mahkeme ID'si (Foreign Key, `Mahkemeler` tablosuna referans).

**İlişkiler**: 
- Savcı ile kişi arasında bire bir ilişki vardır (1:1). 
- Bir savcıya birden fazla dosya atanabilir aynı zamanda bir dosyaya birden fazla savcı bakabilir (N:N). 
- Bir mahkemede (adliye) birden fazla savcı bulunabilir (1:N).

### 11. **Hakim**  
Hakimlere ilişkin bilgiler.
- **hakimID**: Hakimin benzersiz kimliği (Primary Key).
- **kisiID**: Hakim olarak kayıtlı kişinin ID'si (Foreign Key, `Kisiler` tablosuna referans).
- **hakimTuru**: Hakimin türü (başsavcı, cumhuriyet).
- **dosyaID**: Hakimin baktığı dava dosyası ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **mahkemeID**: Hakimin görev yaptığı mahkeme ID'si (Foreign Key, `Mahkemeler` tablosuna referans).

**İlişkiler**: 
- Hakim ile kişi arasında bire bir ilişki vardır (1:1). 
- Bir hakime birden fazla dosya atanabilir (1:N). 
- Bir mahkemede (adliye) birden fazla hakim bulunabilir (1:N).

---

### 12. **Kanunlar**  
Kanun ve madde bilgileri.
- **kanunID**: Kanunun benzersiz kimliği (Primary Key).
- **kanunKodu**: Kanun kodu.
- **kanunMaddeNo**: Kanun maddesi numarası.
- **maddeMetni**: Maddenin tam metni.
- **cezaTuru**: Cezanın türü.
- **cezaSuresi**: Cezanın süresi.

---

### 13. **Suclar**  
Dava dosyalarına ait suç bilgileri.
- **sucID**: Suçun benzersiz kimliği (Primary Key).
- **dosyaID**: Suçun dahil olduğu dava dosyasının ID'si (Foreign Key, `DavaDosyalari` tablosuna referans).
- **kanunID**: Suçun ilgili olduğu kanun ID'si (Foreign Key, `Kanunlar` tablosuna referans).
- **sucTipi**: Suç tipi.
- **sucYeri**: Suçun işlendiği yer.
- **tarafID**: Suç ile ilişkili taraf ID'si (Foreign Key, `DavaTaraflari` tablosuna referans).
- **sucDurumu**: Suç durumu (inceleme, tamamlandı).

**İlişkiler**: 
- Bir suç birden fazla dosyada bulunabilirken aynı zamanda bir dosyada da birden fazla suç olabilir (N:N). 
- Bir suç bir kanun ile bağlantılıdır (1:1). 
- Bir suçun birden fazla tarafı bulunabilirken aynı zamanda bir tarafın da birden fazla suçu olabilir (N:N).

---

### 14. **Cezalar**  
Suçlara verilen cezalarla ilgili bilgiler.
- **cezaID**: Cezanın benzersiz kimliği (Primary Key).
- **sucID**: Cezanın bağlı olduğu suç ID'si (Foreign Key, `Suclar` tablosuna referans).
- **cezaTuru**: Cezanın türü (para, hapis).
- **cezaSuresi**: Cezanın süresi.
- **cezaTutar**: Cezanın miktarı.
- **basTarih**: Cezanın başlangıç tarihi.
- **bitTarih**: Cezanın bitiş tarihi.
- **cezaAciklama**: Cezaya ilişkin açıklama.

**İlişkiler**: 
- Bir suç birden fazla cezada bulunabilirken aynı zamanda bir cezada da birden fazla suç olabilir (N:N).

---

### 15. **Randevular**  
Avukat ve taraflar arasındaki randevular.
- **randevuID**: Randevunun benzersiz kimliği (Primary Key).
- **avukatID**: Randevunun düzenlendiği avukat ID'si (Foreign Key, `Avukat` tablosuna referans).
- **tarafID**: Randevunun düzenlendiği taraf ID'si (Foreign Key, `DavaTaraflari` tablosuna referans).
- **randevuTarih**: Randevu tarihi.
- **randevuSaat**: Randevu saati.
- **randevuDurumu**: Randevu durumu (tamamlandı, iptal).
- **randevuAciklama**: Randevuya ilişkin açıklama.

**İlişkiler**: 
- Bir avukatın birden fazla randevusu olabilir (1:N). 
- Bir tarafın birden fazla avukatı olabilirken aynı zamanda bir avukat da birden fazla tarafın avukatı olabilir (N:N).

### 16. **Odemeler**  
Randevu ücret ödemeleriyle ilgili bilgiler.
- **odemeID**: Ödemenin benzersiz kimliği.
- **tarafID**: Ödemenin ilgili olduğu taraf ID'si (Foreign Key, `DavaTaraflari` tablosuna referans).
- **randevuID**: Ödemenin ilişkili olduğu randevu ID'si (Foreign Key, `Randevular` tablosuna referans).
- **odemeTutar**: Ödeme tutarı.
- **odemeTarih**: Ödeme tarihi.
- **odemeTuru**: Ödeme türü (nakit, kart).
- **odemeDurum**: Ödeme durumu (ödendi, ödenmedi).

**İlişkiler**: 
- Bir tarafın birden fazla ödemesi olabilir (1:N). 
- Bir kişinin birden fazla randevusu olabilir (1:N).
```
