## `2_OOP_Principles.md`

# 2. OOP Prensipleri (4 Ana Prensip)

Nesne Yönelimli Programlamanın temelini oluşturan ve yazılım mimarisini şekillendiren 4 ana prensibin teorik açıklamaları, gerçek hayat analojileri ve kod örnekleri bu dökümanda yer almaktadır.

---

##  2.1 Abstraction (Soyutlama)

###  Teorik Açıklama
Soyutlama (Abstraction), önemli özelliklere odaklanmak için ayrıntıları göz ardı etme sürecidir. Bu prensibin asıl amacı, her sınıfın kendine ait özelliklerini belirlemek ve o sınıfı daha anlaşılır hâle getirmektir. Temel ve kritik özelliklere odaklanmayı sağlayarak, o anki probleme çözüm üretmek için gereksiz olan detayları bir kenara bırakır.

###  Gerçek Hayat Analojisi
* **Kutu Örneği:** Bir sistem tasarlanırken "Kutu" sınıfı için uzunluk, genişlik ve yükseklik gibi boyutlar önemli özellikler olarak belirlenir. Ancak, kutunun yapıldığı malzeme veya rengi gibi ayrıntılar o anki ihtiyaca yönelik değilse göz ardı edilebilir.
* **Araba Örneği:** "Araba" sınıfı ile ilgilenirken yalnızca kullanım ve performansla ilgili özelliklere odaklanılır. Aracın maksimum hızı, yakıt kapasitesi ve markası gibi temel detaylar önemli kabul edilirken ; iç döşeme rengi, radyo markası veya cam tipi gibi gereksiz detaylar göz ardı edilir.

###  Kod Örneği

Aşağıdaki Python kodunda, Tasit adında soyut (abstract) bir sınıf oluşturulmuş ve soyutlama (abstraction) prensibi doğrultusunda yalnızca problemin çözümü için gerekli olan temel özellik ve davranışlara yer verilmiştir.

```python
from abc import ABC, abstractmethod

# Temel ve soyut (abstract) sınıfımızı tanımlıyoruz.
# Bir abstract sınıf (Tasit) oluşturuyoruz.
class Tasit(ABC):
    def __init__(self, marka):
        # Yalnızca problemin çözümü için önemli olan özelliklere odaklanıyoruz (Soyutlama).
        self.marka = marka

    # Alt sınıfların kendi davranışlarını belirlemesini zorunlu kılan soyut metotlar.
    @abstractmethod
    def hizlan(self, miktar):
        pass

    @abstractmethod
    def yavasla(self, miktar):
        pass

# Tasit sınıfından türetilmiş Araba sınıfı.
class Araba(Tasit):
    def __init__(self, marka, maksimum_hiz, yakit_kapasitesi):
        super().__init__(marka)
        # Sadece kullanım ve performansla ilgili detayları alıyoruz (Maksimum hız, yakıt vb.).
        # Radyo markası veya döşeme rengi gibi gereksiz detayları eledik.
        self.__maksimum_hiz = maksimum_hiz 
        self.__yakit_kapasitesi = yakit_kapasitesi
        self.__guncel_hiz = 0

    # Hızlanma ve yavaşlama davranışlarını (metotlarını) tanımlıyoruz.
    def hizlan(self, miktar):
        if self.__guncel_hiz + miktar <= self.__maksimum_hiz:
            self.__guncel_hiz += miktar
            print(f"{self.marka} hızlandı. Güncel hız: {self.__guncel_hiz} km/s")
        else:
            self.__guncel_hiz = self.__maksimum_hiz
            print(f"Maksimum hıza ulaşıldı! Hız: {self.__guncel_hiz} km/s")

    def yavasla(self, miktar):
        if self.__guncel_hiz - miktar >= 0:
            self.__guncel_hiz -= miktar
            print(f"{self.marka} yavaşladı. Güncel hız: {self.__guncel_hiz} km/s")
        else:
            self.__guncel_hiz = 0
            print("Araba tamamen durdu.")

# Nesne oluşturulması ve kullanımı:
benim_arabam = Araba(marka="Toyota", maksimum_hiz=180, yakit_kapasitesi=50)

# Davranışların test edilmesi:
benim_arabam.hizlan(60)
benim_arabam.hizlan(150) # Maksimum hızı aşmaya çalışıyoruz
benim_arabam.yavasla(40)
```

## 2.2 Encapsulation (Kapsülleme)
### Teorik Açıklama
Kapsülleme (Encapsulation), bir sınıfa ait değişkenleri kontrol altına almak, istenmeyen durumların önüne geçmek ve veri doğrulaması yaparak güvenliği sağlamak amacıyla kullanılır. Bu prensibe göre değişkenler private (özel) olarak tanımlanır ve dışarıdan doğrudan erişime kapatılır. Bu değişkenlere okuma veya yazma işlemleri yalnızca sınıf içinde tanımlanan metotlar (Get/Set) veya özellikler (Property - get/set blokları) üzerinden kontrollü bir şekilde sağlanır. Böylece değerler her zaman filtrelenerek ve doğrulanarak atanır.

### Gerçek Hayat Analojisi
* **OBS Örneği:** Bir öğrenci bilgi sistemi düşünelim. Bir öğrencinin sınav notu negatif bir değer veya 100'den büyük bir sayı olamaz. Eğer not değişkeni herkese açık (public) bırakılırsa, sisteme kazara hatalı bir değer girilebilir. Ancak kapsülleme kullanıldığında, "not" özelliği gizlenir (private yapılır) ve yalnızca "not ata" (SetNot) mekanizması ile değiştirilebilir. Bu mekanizma, sisteme gönderilen değerin 0 ile 100 arasında olup olmadığını kontrol eder; eğer hatalıysa işlemi reddeder veya varsayılan bir değer (örneğin 0) atayarak verinin güvenliğini sağlar.


### Kod Örneği

Aşağıdaki Python kodunda, Encapsulation (Kapsülleme) mekanizması gösterilmiştir. Sınıf dışından doğrudan erişimi engellemek amacıyla özelliklerin başına çift alt çizgi (__) eklenerek bu özellikler private olarak tanımlanmıştır.

```python
# Öğrenci sınıfını oluşturuyoruz.
class Ogrenci:
    def __init__(self, isim, ogrenci_notu):
        # İsim ve not özellikleri dışarıdan doğrudan erişilemesin diye private (__isim, __not) tanımlanır.
        self.__isim = isim
        
        # Değer ataması doğrudan değil, metot üzerinden yapılarak kontrol (Encapsulation) sağlanır.
        self.set_not(ogrenci_notu)

    # GetNot metodu: Private değişkendeki değeri dışarıya kontrollü şekilde okutur.
    def get_not(self):
        return self.__not
    
    # İsmi okumak için bir Get metodu
    def get_isim(self):
        return self.__isim

    # SetNot metodu: Private değişkene dışarıdan değer atanmasını denetler.
    def set_not(self, yeni_not):
        # Girilen not 0-100 arasında değilse, not 0 olarak atanacak.
        if 0 <= yeni_not <= 100:
            self.__not = yeni_not
        else:
            print(f"Hata: {self.__isim} için girilen not ({yeni_not}) geçersiz! Not 0 olarak ayarlanıyor.")
            self.__not = 0

# Nesnelerin oluşturulması ve kullanımı
ogrenci_1 = Ogrenci("Ahmet", 85)
print(f"Öğrenci: {ogrenci_1.get_isim()}, Notu: {ogrenci_1.get_not()}")

# İstenmeyen/Hatalı durumun önüne geçme testi
ogrenci_2 = Ogrenci("Ayşe", 150) # 100'den büyük geçersiz not gönderiyoruz.
print(f"Öğrenci: {ogrenci_2.get_isim()}, Notu: {ogrenci_2.get_not()}")

# Doğrudan private özelliğe erişmeye çalışmak Python'da AttributeError verecektir:
# print(ogrenci_1.__not) # Yorum satırından çıkarılırsa hata fırlatır!
```

---

## 2.3 Polymorphism (Çok-şekillilik)

### Teorik Açıklama
Çok-şekillilik (Polymorphism), nesne yönelimli programlamanın en temel yapılarından biridir ve bir nesnenin ya da bir metodun farklı durumlarda veya farklı sınıflarda tamamen kendine özgü şekillerde davranabilmesi özelliğidir. Kelime anlamı olarak "çok biçimlilik" anlamına gelir. Bu prensip sayesinde, aynı isim altında tanımlanmış ortak bir eylem (metot), farklı sınıflarda o sınıfın iç mantığına ve kurallarına göre bambaşka biçimlerde işlenebilir. Yazılım mimarisinde kodun esnekliğini, modülerliğini ve genişletilebilirliğini artırarak; ortak bir çatı altında toplanan farklı nesnelerin birbirlerinin yerine kullanılabilmesine olanak tanır.

### Gerçek Hayat Analojisi
* **Çalışan Türleri Örneği:** Bir şirkette görev yapan tüm personeller için ortak bir Calisan tanımı yapılabilir ve hepsinin gerçekleştirmesi gereken bir maas_hesapla() eylemi bulunur. Ancak bu eylemin işleyiş şekli personelin türüne göre değişir: Bir "Müdür" için maaş hesaplanırken temel ücrete performans primleri eklenir; bir "Muhasebeci" için farklı kalemler dikkate alınır; bir "Stajyer" için ise sadece çalıştığı gün sayısı baz alınır. Tetiklenen komut hepsinde aynıdır (maas_hesapla()), fakat ortaya çıkan davranış her çalışanın kendi kimliğine göre şekillenir.

### Kod Örneği

Aşağıdaki Python kodunda, gerçek hayat analojisindeki "Çalışan" senaryosu modellenerek çok-şekillilik (polymorphism) prensibi uygulanmıştır. Tüm alt sınıflar aynı metot ismini (maas_hesapla) ortak bir çatı altında kullanır ancak her nesne çağrıldığında kendi sınıfına özgü bir çıktı üretir.

```python
# Temel sınıf (Base Class)
# Ortak ata sınıf (Base Class)
class Calisan:
    def __init__(self, ad, temel_maas):
        self.ad = ad
        self.temel_maas = temel_maas

    def maas_hesapla(self):
        # Ortak metot tanımı, alt sınıflar tarafından özelleştirilecek
        pass

# Calisan sınıfından türetilen alt sınıflar
class Mudur(Calisan):
    def __init__(self, ad, temel_maas, prim):
        super().__init__(ad, temel_maas)
        self.prim = prim

    def maas_hesapla(self):
        # Müdür sınıfına özgü maaş hesaplama davranışı
        toplam_maas = self.temel_maas + self.prim
        print(f"Müdür {self.ad} için maaş hesaplandı (Temel Maaş + Prim): {toplam_maas} TL")

class Stajyer(Calisan):
    def __init__(self, ad, temel_maas, calisilan_gun):
        super().__init__(ad, temel_maas)
        self.calisilan_gun = calisilan_gun

    def maas_hesapla(self):
        # Stajyer sınıfına özgü maaş hesaplama davranışı
        toplam_maas = (self.temel_maas / 22) * self.calisilan_gun
        print(f"Stajyer {self.ad} için maaş hesaplandı ({self.calisilan_gun} gün üzerinden): {int(toplam_maas)} TL")

# Çok-şekillilik (Polymorphism) özelliğini kullanan ortak fonksiyon
def maas_bordrosu_yazdir(calisan_nesnesi):
    # Bu fonksiyon, kendisine gelen nesnenin tam olarak hangi alt sınıftan olduğunu bilmek zorunda değildir.
    # Sadece onun bir 'Calisan' olduğunu ve 'maas_hesapla' metoduna sahip olduğunu bilir.
    # Metot çağrıldığında, nesne kendi sınıfına uygun olan şekliyle (çok-şekilli) yanıt verir.
    calisan_nesnesi.maas_hesapla()

# Nesnelerin oluşturulması
mudur_ahmet = Mudur(ad="Ahmet", temel_maas=50000, prim=15000)
stajyer_mehmet = Stajyer(ad="Mehmet", temel_maas=20000, calisilan_gun=15)

# Çok-şekillilik davranışı test ediliyor
print("--- Şirket Maaş Ödeme Dönemi ---")
maas_bordrosu_yazdir(mudur_ahmet)     # Çıktı: Müdür sınıfının özelleştirilmiş davranışı
maas_bordrosu_yazdir(stajyer_mehmet)  # Çıktı: Stajyer sınıfının özelleştirilmiş davranışı
```
