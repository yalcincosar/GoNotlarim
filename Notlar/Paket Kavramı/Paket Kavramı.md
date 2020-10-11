# Paket (Package)
Paketler kısaca bir dizi .go dosyalarını içeren yapılardır. Paketlerin kullanımıyla alakalı bir örnek verelim.

Sürekli matematiksel işlemlere ihtiyaç duyduğumuz projelerde çalıştığımızı, üs alma, faktöriyel hesaplama gibi işlemler için kod yazdığımızı farz edelim. 
Bir projeyi bitiriyoruz sonra başka bir proje için tekrardan aynı matematiksel işlemleri yapacak kodları yazıyoruz. Sürekli aynı kodlar... Bu hiçte mantıklı bir iş değil. Kopyala yapıştır yapsak bile bir takım zaman kaybı ve karışıklıklar var olacaktır. Bunun yerine matematik isminde bir paket oluşturup kodlarımızı o paket içindeki .go dosyalarına yazıp ihtiyaç halinde paketimizi projemize dahil edersek kod tekrarından ve karmaşıklıktan kurtulabiliriz.

Her .go dosyasının ilk satırı paket bildirimi ile başlar. Paket bildirimi için `package` kelimesini kullanırız ardından paket ismi gelir.

```go
package paketismi
```

Bu arada her çalıştırılabilir go programı main() fonksiyonunu içermelidir. main() fonksiyonu package main içinde olmalıdır.

Paket kavramını daha iyi anlamak için matematiksel işlemleri yapan basit bir uygulama yazalım Öncelikle main fonksiyonu ve main paketini oluşturarak başlayalım.

matematik adında bir dizin oluşturup main.go dosyamızın içine aşağıdaki kodları yazalım
```go
package main
import "fmt"

func main() {
	fmt.Println("Sonuc: ")
}
```
Öncelikle komut satırında matematik dizine geçip 
```go 
cd ~/Belgeler/Go/matematik
```  
matematik dizininde olduğumuzdan emin olduktan sonra
```go 
go install matematik
```
yazalım. Sorun çıkmazsa binary olarak derlenecek ve ekrana "Basit matematik paketimiz" yazdıracaktır.

Çıktı:
```go
go install: no install location for directory /home/ylcncosar/Belgeler/Go/matematik outside GOPATH
For more details see: 'go help gopath'
```
Şimdilik bu hatamızı düzeltmeyelim go dilinde modül konseptine bakalım.

# Modüller
Go dili 1.11 versiyonuyla bağımlılıkları yönetebilmek için modül konseptini tanıttı. Modül, go paketlerinin gruplanmasıdır. Modül içinde bulunan go.mod dosyası modülün bir nevi künyesidir. Daha önceki go sürümlerinde projeler `$GOPATH` ortamında oluşturulmalıydı, go.mod dosyasıyla `$GOPATH` ortamına duyulan ihtiyaç giderildi. 

Yukarıdaki hatamız GOPATH dışına kurulum yok gibi birşey demekte. Sorunu çözmek için modül oluşturalım.

Tekradan komut satırında matematik dizinine geçelim ve aşağıdaki kodu çalıştıralım.
```go
go mod init matematik  
```
Çıktı:
```go
go: creating new go.mod: module matematik
```
matematik modülümü için go.mod dosyası oluşturuldu. go.mod dosyasında modül ismi ve versiyon numarası yazmakta.
```go
module matematik

go 1.14
```

Kendi paketimizi oluşturmaya başlayalım. Kaynak kodları kendileriyle aynı isimde olan dizinlerde bulunmalıdır. Matematik dizininde faktoriyel isimli bir dizin oluşturup içinde faktoriyel.go oluşturalım. Faktoriyel dizinindeki tüm .go programlarımız `package faktoriyel` ile bildirilmelidir.

```go
//faktoriyel.go
package faktoriyel

func FaktoriyelHesapla(sayi int) int {
	sonuc:=1
	for ;sayi>0;sayi--{
		sonuc*=sayi
	}
	return sonuc
}
```
Burada dikkat edilmesi gereken nokta dışarı aktaracağımız yani dışarıdan erişip kullanacağımız fonksiyonun isminin ilk harfinin büyük olmasıdır. Küçük olması durumunda hata verecektir.

Paketi main programına dahil edelim. Dahil ederken `import` kelimesini kullanırız.
```go
package main

import (
	"fmt"
	"matematik/faktoriyel"
)

func main() {
	fmt.Println("Sonuc:", faktoriyel.FaktoriyelHesapla(5))
}
```
# Init() Fonksiyonu
Main fonksiyonu gibi parametre almaz herhangi bir değer döndürmez. Paket başlatıldığında Go tarafından  çağrılır, herhangi bir yerden init() fonksiyon çağrısı yapamayız. Init() fonksiyonu main() fonksiyonundan önce çalışır. Kontrol yapmak, global değişkenlere atama yapmak gibi işlemler için kullanılabilir. Ayrıca main() fonksiyonu bir kez tanımlanabilirken init() fonksiyonu birden çok tanımlanabilir. Birden fazla init() fonksiyonu olduğunda sırayla çalışırlar.

```go
package main

import "fmt"

func init() {
    fmt.Println("Birinci init")
}

func init() {
    fmt.Println("İkinci init")
}

func init() {
    fmt.Println("Üçüncü init")
}

func init() {
    fmt.Println("Dördüncü init")
}

func main() {
    fmt.Println("main çalıştı")
}
```
Çıktı:
```go
Birinci init
İkinci init
Üçüncü init
Dördüncü init
main çalıştı
```

# Boş Tanımlayıcı

Bir değişken veya paket programda kullanılmadığı zaman Go derleyicisi hata verecektir. Boş tanımlayıcı bu sorunu çözmek için kullanılır. Boş tanımlayıcı .oğunlukla birden çok değer döndüren fonksiyonların ihtiyaç duymadığımız değerlerini yok saymak için kullanılır. Boş tanımlayıcı `_` ile temsil edilir.

```go
package main
 
import (
    "fmt"
)
 
func degerDondur() (int, int) {
    return 42, 44
}
 
func main() {
    _, deger := degerDondur()
    fmt.Println(deger)
}
```
Çıktı:
```go
44
```
Örnekte degerDondur() fonksiyonunun ilk dönen değer olan 42'ye ihtiyaç duymadığımızı farz ettik ve onu boş tanımlayıcıya atadık. Böylelikle Go compiler onu görmezden geldi ve hata vermedi.

Bir paketi ileride kullanmak için import ettiğimiz şuan kullanmadığımız durum için bir örnek yapalım.
```go
package main
import "matematik/faktoriyel"
func main() {
	
}
```
Çıktı:
```go
imported and not used: "matematik/faktoriyel"
```
Kullanmadığımız için Go derleyicisi hata verdi. Yine bu sorunun önüne geçmek için boş tanımlayıcı kullanırız.


```go
package main
import _ "matematik/faktoriyel"

func main() {

}

```
Kod hatasız çalışacaktır.

