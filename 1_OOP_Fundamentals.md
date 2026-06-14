# Nesne Yönelimli Programlama (NYP) Temelleri

## Nesne Yönelimli Programlama (NYP) Nedir ve Neden İhtiyaç Duyulmuştur?

**Nesne Yönelimli Programlama (NYP) Nedir?**
Nesne Yönelimli Programlama (NYP), uygulamanın nesneler ve onların ilişkileri çerçevesinde belirli bir iş yapmak için geliştirildiği bir yaklaşımdır. Gerçek dünyadaki varlıklar doğrudan programa yansıtılır. Nesneler, kendi özellikleri (değişkenler) ve davranışları (metotlar) olan yapı taşlarıdır.

**Neden İhtiyaç Duyuldu?**
Eski nesil prosedür tabanlı programlamada uygulamalar tek parça bir bütün olduğundan projeler büyüdükçe ve karmaşıklaştıkça bakım maliyetleri hızla artıyor, projelerin yönetimi zorlaşıyordu. NYP; yazılımların geliştirilmesini, anlaşılmasını ve bakımını kolaylaştırmak; tekrar kullanılabilir, modüler ve esnek yapılar oluşturmak amacıyla bu çıkmaza bir çözüm olarak ortaya çıkmıştır.

---

## Sınıf (Class) ve Nesne (Object) Kavramları

### Sınıf (Class) Nedir?
Sınıf, nesnelerin ortak özelliklerini (durumlarını), davranışlarını (metotlarını) ve başlangıç durumlarını tanımlayan soyut bir **şablondur**. Sınıf isimleri genellikle bir şahıs, yer veya nesneyi temsil eder.
* **Değişkenler (Özellikler / Fields):** Nesnelerin durumlarını temsil eder.
* **Metotlar (Davranışlar / Methods):** Nesnelerin nasıl davranacağını tanımlar.

### Nesne (Object) Nedir?
Nesne, bir sınıftan türetilmiş, o sınıfın somut bir **örneğidir**. 
* Nesneler canlıdır ve her nesnenin kendine özgü bir kimliği vardır.
* Aynı sınıftan üretilen nesneler, taşıdıkları değişkenlerin değerleri bakımından birbirinden farklı olabilir.

---

## Python ile OOP Örneği

Python dili ile yazılmış bir nesne ve class orneği aşağıdadır. Python'da kapsülleme (encapsulation) işlemi değişken adının başına çift alt çizgi (`__`) konularak yapılır. Yapıcı metot (constructor) ise `__init__` fonksiyonudur.

```python
class Araba:
    # Yapıcı Metot (Constructor) - Nesne doğduğunda ilk çalışan bölüm
    def __init__(self, marka, max_hiz, yakit_kapasitesi):
        self.marka = marka
        # Kapsülleme (Encapsulation) - Dışarıdan doğrudan erişimi engellemek için private alanlar
        self.__max_hiz = max_hiz  
        self.__yakit = yakit_kapasitesi 

    # Davranışlar (Metotlar)
    def depoyu_doldur(self, miktar):
        self.__yakit += miktar
        print(f"{self.marka} deposu dolduruldu. Mevcut yakıt: {self.__yakit} L")

    def hiz_goster(self):
        print(f"{self.marka} aracının maksimum hızı: {self.__max_hiz} km/h")
        
    def yakit_goster(self):
        print(f"{self.marka} aracının yakıtı: {self.__yakit} L")

# Nesne (Object) Yaratma İşlemi
bmw = Araba("BMW", 250, 50)
toyota = Araba("Toyota", 180, 40)

# Nesnelerin metotlarını (davranışlarını) çağırma
bmw.hiz_goster()        # Çıktı: BMW aracının maksimum hızı: 250 km/h
toyota.yakit_goster()   # Çıktı: Toyota aracının yakıtı: 40 L

toyota.depoyu_doldur(10) # Çıktı: Toyota deposu dolduruldu. Mevcut yakıt: 50 L
