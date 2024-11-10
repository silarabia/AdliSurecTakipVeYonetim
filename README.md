# AdliSurecTakipVeYonetim
Veri Tabanı Yönetim Sistemleri Dersi
# Proje Başlığı: Adli Süreç Takip ve Yönetim Sistemi
## Proje Ekibindeki Kişiler: 
- **225260030** – Sıla Rabia Bayram
- **235260134** – İrem Alliş
- **235260182** – Merve Beril Yılmaz

  ---

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

### 2.1 DavaDosyalari
Dava dosyalarına ait temel bilgileri içerir.
| Alan Adı            | Açıklama                                                | Veri Tipi      | Özellikler        |
|---------------------|---------------------------------------------------------|----------------|-------------------|
| dosyaID             | Dava dosyasının benzersiz kimliği                        | INTEGER        | Primary Key       |
| davaNo              | Dava numarası                                           | STRING         |                   |
| mahkemeID           | Davanın görüldüğü mahkemenin ID'si                       | INTEGER        | Foreign Key, Mahkemeler tablosuna referans |
| davaAcilmaTarihi    | Davanın açılış tarihi                                   | DATE           |                   |
| davaTürü            | Davanın türü                                            | STRING         |                   |
| davaDurumu          | Davanın durumu (açık, kapalı, beklemede)                | STRING         |                   |

**İlişkiler**: Bir mahkemede (adliye) birden fazla dava dosyası bulunabilir (1:N).


### 2.2 Kisiler
Sistemdeki kişilerin bilgilerini içerir.
| Alan Adı          | Açıklama                                                 | Veri Tipi      | Özellikler          |
|-------------------|----------------------------------------------------------|----------------|---------------------|
| kisiID            | Kişinin benzersiz kimliği                                 | INTEGER        | Primary Key         |
| kisiTC            | Kişinin TC kimlik numarası                                | STRING         |                     |
| kisiAdSoyad       | Kişinin adı ve soyadı                                     | STRING         |                     |
| kisiDogumTarihi   | Kişinin doğum tarihi                                      | DATE           |                     |
| kisiTelefon       | Kişinin telefon numarası                                  | STRING         |                     |
| kisiMail          | Kişinin e-posta adresi                                    | STRING         |                     |
| kisiAdres         | Kişinin adres bilgisi                                     | STRING         |                     |
| kisiSifresi       | Kişinin sisteme giriş yapabilmesi için şifresi            | STRING         |                     |
| kisiRolu          | Kişinin rolü (avukat, hakim, savcı, davatarafları, admin) | STRING         |                     |


### 3. **DavaTaraflari**  
Dava dosyalarındaki taraflara ilişkin bilgiler.
| Alan Adı   | Açıklama                                                   | Veri Tipi | Özellikler                                  |
|------------|------------------------------------------------------------|-----------|---------------------------------------------|
| tarafID    | Tarafın benzersiz kimliği                                  | INTEGER   | Primary Key                                 |
| dosyaID    | Tarafın dahil olduğu dava dosyasının ID'si                 | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| kisiID     | Taraf olarak atanmış kişinin ID'si                         | INTEGER   | Foreign Key, `Kisiler` tablosuna referans   |
| tarafRol   | Tarafın davadaki rolü (davalı, davacı, tanık)              | STRING    |                                             |
| ekBilgi    | Taraf hakkında ek bilgiler                                 | STRING    |                                             |

**İlişkiler**: Bir dava dosyasının birden fazla tarafı olabilir, aynı zamanda bir dava tarafının da birden fazla dava dosyası olabilir (N:N). Kişi ile dava tarafı arasında bire bir ilişki vardır (1:1).


### 4. **Durusmalar**  
Duruşmalarla ilgili ayrıntılar.
| Alan Adı         | Açıklama                                                    | Veri Tipi | Özellikler                                    |
|------------------|-------------------------------------------------------------|-----------|-----------------------------------------------|
| durusmaID        | Duruşmanın benzersiz kimliği                                 | INTEGER   | Primary Key                                   |
| dosyaID          | Duruşmanın ait olduğu dava dosyasının ID'si                 | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| durusmaTarihi    | Duruşma tarihi                                              | DATE      |                                               |
| durusmaSaati     | Duruşmanın saati                                            | TIME      |                                               |
| durusmaYeri      | Duruşmanın yapıldığı yer                                    | STRING    |                                               |
| mahkemeID        | Duruşmanın görüldüğü mahkemenin ID'si                       | INTEGER   | Foreign Key, `Mahkemeler` tablosuna referans   |
| hakimID          | Duruşmayı yöneten hakimin ID'si                             | INTEGER   | Foreign Key, `Hakim` tablosuna referans       |
| avukatID         | Duruşmada yer alan avukatın ID'si                           | INTEGER   | Foreign Key, `Avukat` tablosuna referans      |
| durusmaSonuc     | Duruşmanın sonucu (ertelendi, tamamlandı)                   | STRING    |                                               |
| tarafID          | Duruşmada yer alan tarafın ID'si                            | INTEGER   | Foreign Key, `DavaTaraflari` tablosuna referans |

**İlişkiler**: 
- Bir dosya birden fazla duruşmada kullanılabilir (1:N). 
- Bir mahkemede (adliye) birden fazla duruşma olabilir (1:N). 
- Bir hakim birden çok duruşmaya bakabilir (1:N). 
- Bir avukat birden çok duruşmaya bakabilirken aynı zamanda bir duruşmaya birden fazla avukat da bakabilir (N:N). 
- Bir dava tarafının birden fazla duruşması olabilirken aynı zamanda bir duruşmanın da birden fazla dava tarafı olabilir (N:N).


### 5. **Belgeler**  
Dava dosyalarına eklenen belgelerle ilgili bilgiler.
| Alan Adı         | Açıklama                                                   | Veri Tipi | Özellikler                                 |
|------------------|------------------------------------------------------------|-----------|--------------------------------------------|
| belgeID          | Belgenin benzersiz kimliği                                 | INTEGER   | Primary Key                                |
| dosyaID          | Belgenin ait olduğu dava dosyasının ID'si                  | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| belgeTürü        | Belgenin türü (delil, ifade, karar, rapor)                 | STRING    |                                            |
| belgeAciklama    | Belge hakkında açıklama                                    | STRING    |                                            |
| yüklenmeTarihi   | Belgenin sisteme yüklendiği tarih                          | DATE      |                                            |
| kisiID           | Belgeyi yükleyen kişinin ID'si                             | INTEGER   | Foreign Key, `Kisiler` tablosuna referans  |

**İlişkiler**: 
- Bir belge birden fazla dava dosyasında bulunabilirken aynı zamanda bir dava dosyasında da birden fazla belge bulunabilir (N:N). 
- Bir belgeyi birden fazla kişi yükleyebilirken aynı zamanda bir kişi de birden fazla belge yükleyebilir (N:N).


### 6. **Kararlar**  
Davaya ilişkin verilen kararlar.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| kararID       | Kararın benzersiz kimliği                            | INTEGER   | Primary Key                                 |
| dosyaID       | Kararın ait olduğu dava dosyasının ID'si             | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| kararTarihi   | Kararın tarihi                                       | DATE      |                                             |
| kararDurumu   | Kararın durumu (kesin, temyiz)                       | STRING    |                                             |
| kararDetayi   | Karar detayları                                     | TEXT      |                                             |
| hakimID       | Kararı veren hakimin ID'si                           | INTEGER   | Foreign Key, `Hakim` tablosuna referans    |

**İlişkiler**: 
- Bir dosyanın bir kararı vardır fakat bir karar birden fazla dosyada bulunabilir (N:1). 
- Bir kararın bir tane hakimi olabilirken bir hakim birden fazla kararda bulunabilir (N:1).


### 7. **Mahkemeler**  
Mahkemelerle ilgili bilgiler.
| Alan Adı         | Açıklama                      | Veri Tipi | Özellikler    |
|------------------|--------------------------------|-----------|---------------|
| mahkemeID        | Mahkemenin benzersiz kimliği   | INTEGER   | Primary Key   |
| mahkemeAd        | Mahkemenin adı                 | STRING    |               |
| mahkemeAdres     | Mahkemenin adresi              | STRING    |               |
| mahkemeTelefon   | Mahkemenin telefon numarası    | STRING    |               |
| mahkemeMail      | Mahkemenin e-posta adresi      | STRING    |               |


### 8. **Loglar**  
Sistemdeki işlemlerin kaydedildiği tablo.
| Alan Adı        | Açıklama                                           | Veri Tipi | Özellikler                                  |
|-----------------|----------------------------------------------------|-----------|---------------------------------------------|
| logID           | Log kaydının benzersiz kimliği                     | INTEGER   | Primary Key                                 |
| kisiID          | İşlemi yapan kişinin ID'si                         | INTEGER   | Foreign Key, `Kisiler` tablosuna referans   |
| dosyaID         | İşlemin ilgili olduğu dava dosyasının ID'si        | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| logTarihi       | Log kaydının tarihi                                | DATE      |                                             |
| islemTipi       | İşlem tipi (oluşturma, güncelleme, silme)          | STRING    |                                             |
| islemDetayi     | İşlemin detayı                                     | STRING    |                                             |

**İlişkiler**: 
- Kişi ile log arasında bire bir ilişki bulunur (1:1). 
- Bir logda birden fazla dosya ile işlem yapılabilirken aynı zamanda bir dosyada birden fazla log da bulunabilir (N:N).


### 9. **Avukat**  
Avukatlara ilişkin bilgiler.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| avukatID      | Avukatın benzersiz kimliği                           | INTEGER   | Primary Key                                 |
| kisiID        | Avukat olarak kayıtlı kişinin ID'si                  | INTEGER   | Foreign Key, `Kisiler` tablosuna referans   |
| mahkemeID     | Avukatın görev yaptığı mahkeme ID'si                 | INTEGER   | Foreign Key, `Mahkemeler` tablosuna referans |
| avBuro        | Avukatın bağlı olduğu büro                           | STRING    |                                             |
| davaSayisi    | Avukatın temsil ettiği dava sayısı                   | INTEGER   |                                             |

**İlişkiler**: 
- Avukat ile kişi arasında bire bir ilişki vardır (1:1). 
- Bir avukat birden fazla mahkemede (adliye) görev alabilirken aynı zamanda bir mahkemede de birden fazla avukat görev alabilir (N:N).


### 10. **Savci**  
Savcılara ilişkin bilgiler.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| savciID       | Savcının benzersiz kimliği                           | INTEGER   | Primary Key                                 |
| kisiID        | Savcı olarak kayıtlı kişinin ID'si                   | INTEGER   | Foreign Key, `Kisiler` tablosuna referans   |
| savciTuru     | Savcının türü (başsavcı, cumhuriyet)                 | STRING    |                                             |
| dosyaID       | Savcıya atanan belge ID'si                           | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| mahkemeID     | Savcının görev yaptığı mahkeme ID'si                 | INTEGER   | Foreign Key, `Mahkemeler` tablosuna referans |

**İlişkiler**: 
- Savcı ile kişi arasında bire bir ilişki vardır (1:1). 
- Bir savcıya birden fazla dosya atanabilir aynı zamanda bir dosyaya birden fazla savcı bakabilir (N:N). 
- Bir mahkemede (adliye) birden fazla savcı bulunabilir (1:N).


### 11. **Hakim**  
Hakimlere ilişkin bilgiler.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| hakimID       | Hakimin benzersiz kimliği                            | INTEGER   | Primary Key                                 |
| kisiID        | Hakim olarak kayıtlı kişinin ID'si                   | INTEGER   | Foreign Key, `Kisiler` tablosuna referans   |
| hakimTuru     | Hakimin türü (başsavcı, cumhuriyet)                  | STRING    |                                             |
| dosyaID       | Hakimin baktığı dava dosyası ID'si                   | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| mahkemeID     | Hakimin görev yaptığı mahkeme ID'si                  | INTEGER   | Foreign Key, `Mahkemeler` tablosuna referans |

**İlişkiler**: 
- Hakim ile kişi arasında bire bir ilişki vardır (1:1). 
- Bir hakime birden fazla dosya atanabilir (1:N). 
- Bir mahkemede (adliye) birden fazla hakim bulunabilir (1:N).


### 12. **Kanunlar**  
Kanun ve madde bilgileri.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| kanunID       | Kanunun benzersiz kimliği                            | INTEGER   | Primary Key                                 |
| kanunKodu     | Kanun kodu                                           | STRING    |                                             |
| kanunMaddeNo  | Kanun maddesi numarası                               | INTEGER   |                                             |
| maddeMetni    | Maddenin tam metni                                   | TEXT      |                                             |
| cezaTuru      | Cezanın türü                                         | STRING    |                                             |
| cezaSuresi    | Cezanın süresi                                       | STRING    |                                             |


### 13. **Suclar**  
Dava dosyalarına ait suç bilgileri.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| sucID         | Suçun benzersiz kimliği                              | INTEGER   | Primary Key                                 |
| dosyaID       | Suçun dahil olduğu dava dosyasının ID'si             | INTEGER   | Foreign Key, `DavaDosyalari` tablosuna referans |
| kanunID       | Suçun ilgili olduğu kanun ID'si                      | INTEGER   | Foreign Key, `Kanunlar` tablosuna referans  |
| sucTipi       | Suç tipi                                             | STRING    |                                             |
| sucYeri       | Suçun işlendiği yer                                  | STRING    |                                             |
| tarafID       | Suç ile ilişkili taraf ID'si                         | INTEGER   | Foreign Key, `DavaTaraflari` tablosuna referans |
| sucDurumu     | Suç durumu (inceleme, tamamlandı)                    | STRING    |                                             |

**İlişkiler**: 
- Bir suç birden fazla dosyada bulunabilirken aynı zamanda bir dosyada da birden fazla suç olabilir (N:N). 
- Bir suç bir kanun ile bağlantılıdır (1:1). 
- Bir suçun birden fazla tarafı bulunabilirken aynı zamanda bir tarafın da birden fazla suçu olabilir (N:N).


### 14. **Cezalar**  
Suçlara verilen cezalarla ilgili bilgiler.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| cezaID        | Cezanın benzersiz kimliği                            | INTEGER   | Primary Key                                 |
| sucID         | Cezanın bağlı olduğu suç ID'si                       | INTEGER   | Foreign Key, `Suclar` tablosuna referans    |
| cezaTuru      | Cezanın türü (para, hapis)                           | STRING    |                                             |
| cezaSuresi    | Cezanın süresi                                       | STRING    |                                             |
| cezaTutar     | Cezanın miktarı                                      | DECIMAL   |                                             |
| basTarih      | Cezanın başlangıç tarihi                             | DATE      |                                             |
| bitTarih      | Cezanın bitiş tarihi                                 | DATE      |                                             |
| cezaAciklama  | Cezaya ilişkin açıklama                              | STRING    |                                             |

**İlişkiler**: 
- Bir suç birden fazla cezada bulunabilirken aynı zamanda bir cezada da birden fazla suç olabilir (N:N).


### 15. **Randevular**  
Avukat ve taraflar arasındaki randevular.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| randevuID     | Randevunun benzersiz kimliği                         | INTEGER   | Primary Key                                 |
| avukatID      | Randevunun düzenlendiği avukat ID'si                 | INTEGER   | Foreign Key, `Avukat` tablosuna referans    |
| tarafID       | Randevunun düzenlendiği taraf ID'si                  | INTEGER   | Foreign Key, `DavaTaraflari` tablosuna referans |
| randevuTarih  | Randevu tarihi                                       | DATE      |                                             |
| randevuSaat   | Randevu saati                                        | TIME      |                                             |
| randevuDurumu | Randevu durumu                                       | STRING    |                                             |
| randevuAciklama| Randevuya ilişkin açıklama                           | STRING    |                                             |

**İlişkiler**: 
- Bir avukatın birden fazla randevusu olabilir (1:N). 
- Bir tarafın birden fazla avukatı olabilirken aynı zamanda bir avukat da birden fazla tarafın avukatı olabilir (N:N).


### 16. **Odemeler**  
Randevu ücret ödemeleriyle ilgili bilgiler.
| Alan Adı      | Açıklama                                             | Veri Tipi | Özellikler                                  |
|---------------|------------------------------------------------------|-----------|---------------------------------------------|
| odemeID       | Ödemenin benzersiz kimliği                           | INTEGER   | Primary Key                                 |
| tarafID       | Ödemenin ilgili olduğu taraf ID'si                   | INTEGER   | Foreign Key, `DavaTaraflari` tablosuna referans |
| randevuID     | Ödemenin ilişkili olduğu randevu ID'si               | INTEGER   | Foreign Key, `Randevular` tablosuna referans |
| odemeTutar    | Ödeme tutarı                                        | DECIMAL   |                                             |
| odemeTarih    | Ödeme tarihi                                         | DATE      |                                             |
| odemeTuru     | Ödeme türü (nakit, kart)                             | STRING    |                                             |
| odemeDurum    | Ödeme durumu (ödendi, ödenmedi)                     | STRING    |                                             |

**İlişkiler**: 
- Bir tarafın birden fazla ödemesi olabilir (1:N). 
- Bir kişinin birden fazla randevusu olabilir (1:N).

