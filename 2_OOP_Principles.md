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
---

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


