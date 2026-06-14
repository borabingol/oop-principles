## `2_OOP_Principles.md`

# 2. OOP Prensipleri (4 Ana Prensip)

Nesne Yönelimli Programlamanın temelini oluşturan ve yazılım mimarisini şekillendiren 4 ana prensibin teorik açıklamaları, gerçek hayat analojileri ve kod örnekleri bu dökümanda yer almaktadır.

---

## 🧬 2.1 Abstraction (Soyutlama)

### 💡 Teorik Açıklama
Soyutlama (Abstraction), önemli özelliklere odaklanmak için ayrıntıları göz ardı etme sürecidir. Bu prensibin asıl amacı, her sınıfın kendine ait özelliklerini belirlemek ve o sınıfı daha anlaşılır hâle getirmektir. Temel ve kritik özelliklere odaklanmayı sağlayarak, o anki probleme çözüm üretmek için gereksiz olan detayları bir kenara bırakır.

### 🎯 Gerçek Hayat Analojisi
* **Kutu Örneği:** Bir sistem tasarlanırken "Kutu" sınıfı için uzunluk, genişlik ve yükseklik gibi boyutlar önemli özellikler olarak belirlenir. Ancak, kutunun yapıldığı malzeme veya rengi gibi ayrıntılar o anki ihtiyaca yönelik değilse göz ardı edilebilir.
* **Araba Örneği:** "Araba" sınıfı ile ilgilenirken yalnızca kullanım ve performansla ilgili özelliklere odaklanılır. Aracın maksimum hızı, yakıt kapasitesi ve markası gibi temel detaylar önemli kabul edilirken ; iç döşeme rengi, radyo markası veya cam tipi gibi gereksiz detaylar göz ardı edilir.

### 💻 Kod Örneği

Aşağıdaki Python kodunda, dokümandaki çalışma sorularında istenildiği gibi `Tasit` adında soyut (abstract) bir sınıf kurgulanmış ve sadece problem için gerekli olan özelliklere odaklanılmıştır.

```python
from abc import ABC, abstractmethod

# Temel ve soyut (abstract) sınıfımızı tanımlıyoruz.
# Bir abstract sınıf (Tasit) oluşturuyoruz[cite: 1114].
class Tasit(ABC):
    def __init__(self, marka):
        # Yalnızca problemin çözümü için önemli olan özelliklere odaklanıyoruz (Soyutlama)[cite: 597, 598].
        self.marka = marka

    # Alt sınıfların kendi davranışlarını belirlemesini zorunlu kılan soyut metotlar.
    @abstractmethod
    def hizlan(self, miktar):
        pass

    @abstractmethod
    def yavasla(self, miktar):
        pass

# Tasit sınıfından türetilmiş Araba sınıfı[cite: 1114].
class Araba(Tasit):
    def __init__(self, marka, maksimum_hiz, yakit_kapasitesi):
        super().__init__(marka)
        # Sadece kullanım ve performansla ilgili detayları alıyoruz (Maksimum hız, yakıt vb.)[cite: 616, 618, 619].
        # Radyo markası veya döşeme rengi gibi gereksiz detayları eledik[cite: 617].
        self.__maksimum_hiz = maksimum_hiz 
        self.__yakit_kapasitesi = yakit_kapasitesi
        self.__guncel_hiz = 0

    # Hızlanma ve yavaşlama davranışlarını (metotlarını) tanımlıyoruz[cite: 1113].
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

# Nesne oluşturulması ve kullanımı[cite: 1115]:
benim_arabam = Araba(marka="Toyota", maksimum_hiz=180, yakit_kapasitesi=50)

# Davranışların test edilmesi:
benim_arabam.hizlan(60)
benim_arabam.hizlan(150) # Maksimum hızı aşmaya çalışıyoruz
benim_arabam.yavasla(40)
