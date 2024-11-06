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
