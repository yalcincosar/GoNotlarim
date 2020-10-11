# Arayüzler (Interfaces)

Go dilinde arayüz, gövdesiz metotlar kümesini belirtmek için kullanılan özel bir tiptir. Arayüzler soyuttur bu nedenle bir instance (örnek) oluşturamayız. Genelde arayüzler için sözleşme benzetmesi yapılır. Sözleşme maddelerini gövdesiz metotlar olarak düşünelim. Sözleşme maddelerini karşılayanlar için o arayüzü uyguluyor deriz. Go dilinde arayüzleri uygulamak için bazı dillerde olduğu gibi herhangi bir kelime kullanmamıza gerek yoktur. Go dilinde bir tipin arayüzü uygulayıp uygulamadığı otomatik olarak belirlenir.

Arayüz tanımlama:
```go
type Arayüz_Adı interface {
	metot1() metotTipi
    metot2() metotTipi
}
```
Arayüzlerin kullanımına bir örnek yapalım.
```go
package main

import (
	"fmt"
)
//arayüz tanımlaması
type Calisan interface {
	calis() string
}

type Sekreter struct {
}

func (s Sekreter) calis() string {
	return "Sekreter calisiyor."
}

type Mudur struct {
}

func (m Mudur) calis() string {
	return "Mudur calisiyor."
}

type Muhendis struct {
}

func (mh Muhendis) calis() string {
	return "Muhendis calisiyor."
}


func main() {
	calisanlar := []Calisan{Sekreter{}, Mudur{}, Muhendis{}}
	for _, calisan := range calisanlar {
		fmt.Println(calisan.calis())
	}
}
```
Çıktı:
```go
Sekreter calisiyor.
Mudur calisiyor.
Muhendis calisiyor.
```
Örneğimizde Calisan adında calis() metoduna sahip bir arayüz var. calisanlar isminde Calisan türünde bir slice (dilim) oluşturuyoruz ve diğer tiplerimizi (Sekreter,Mudur ve Muhendis) bu slice'a sorunsuzca atayabiliyoruz. Bu atamanın sorun çıkarmamasının nedeni calis() metodunu uygulayan herkes sözleşmeyi yerine getirmiş ve Calisan olarak kabul edilmiştir (Bir tip bir arayüzü uyguluyorsa, arayüz tipinden oluşturulan değişkene atanıp arayüz tipinden temsil edilebilir). Daha sonra slice'ı range ile dolaşıp elemanları üzerinden calis() metodunu çağırmaktayız.

## Arayüzlerde Tip ve Değer

Herhangi bir tip bir arayüzü uyguluyorsa, arayüz tipinden oluşturulan değişkene atanıp arayüz tipinden temsil edilebilirdi. Arayüz tipinden bu değişkenlerin statik ve dinamik olmak üzere iki tipi vardır. Statik tipi arayüzün bizzat kendisidir. Tanımlama aşamasında belirtilir ve değişmezdirler. Dinamik tipi ise kendisine atanan değişkenin tipidir. Arayüz değişkeni değer olarak dinamik tipin değerini saklar.


Bir arayüzün metodu çağrıldığında dinamik tipinin metodu çalışır.


Örneğimizde Calisan arayüzü tipinden bir değişken oluşturup %v ile değerine, %T ile arayüzün tipine bakalım. 
```go
package main

import (
	"fmt"
)

type Calisan interface {
	calis() string
}

func main() {
    var c Calisan
    fmt.Printf("deger: %v tip: %T\n",c,c)
	   
}
```
Çıktı:
```go
deger: <nil> tip: <nil>
```

Arayüzü uygulayan herhangi bir tip atanmadığı için değeri ve dinamik tipi nil.

```go
package main

import (
	"fmt"
)

type Calisan interface {
	calis() string
}

type Muhendis struct {
    isim string
}

func (mh Muhendis) calis() string {
	return "Muhendis calisiyor."
}

func main() {
    var c Calisan
    c = Muhendis{"Ahmet"}
    fmt.Printf("deger: %v tip: %T\n",c,c) 
}
```
Calisan tipindeki c değişkenimiz artık Muhendis tipinin değerini tutuyor.
Artık dinamik tip Muhendis, işaret ettiği dinamik değer ise Ahmet olmuştur.

Çıktı:
```go
deger: {Ahmet} tip: main.Muhendis
```


## Interface{}

Empty interface, yani boş herhangi bir metodu olmayan interfacedir. Metodu olmadığı için tüm tipler bunu otomatik olarak uygular. Bir fonksiyonun parametresi interface{} olursa o fonksiyon argüman olarak her türlü tipi alabilir. Java dilindeki object gibi düşünebiliriz.

```go
package main

import (
	"fmt"
)

type Muhendis struct {
}

func yapBiseyler(i interface{}) {
  fmt.Printf("Tip: %T \n", i)
}

func main() {
	yapBiseyler(Muhendis{})
	sayi:=12
	yapBiseyler(sayi)
	yapBiseyler("bu bir string")
}
```
Çıktı:
```go
Tip: main.Muhendis 
Tip: int 
Tip: string 
```
Gördüğünüz gibi yapBiseyler() fonksiyonunun argümanına farklı tipler geçilmesine rağmen hepsi sorunsuz çalıştı. 

Bu konuyla alakalı sık yapılan hatalardan biri yapBiseyler() fonksiyonunun parametresinde bulunan i değişkeninin her tipten olduğu düşüncesidir. Fakat gerçekte i interface{} tipindendir. Go runtime da bir tip dönüştürme (type conversion) işlemi yapar yani i değişkenine geçilen değeri interface{} tipine dönüştürür(gerekli görürse).


Bu düşüncenin neden hatalı olduğunu ispatlayalım.
```go
package main

import (
    "fmt"
)

func yazdir(s []interface{}) {
    for _, sayi := range s {
        fmt.Println(sayi)
    }
}

func main() {
    sayilar := []int{1, 2, 3, 4}
    yazdir(sayilar)
}
```
Eğer yazdır() fonksiyonunun parametresinde bulunan s değişkeni her tipten olsaydı kodumuzun sorunsuz çalışması gerekirdi fakat kodu çalıştırdığımız durumda `cannot use sayilar (type []int) as type []interface {} in argument to yazdir` hatası ile karşılaşırız. Bu hatanın nedeni Go'nun runtime da s değişkenine geçilen int dilimini []interface{} tipine döndüştürememesidir. Dönüşümün başarısız olması bu iki tipin hazıfazda farklı temsil edilmesiyle açıklanır. Eğer bu işlemi yapmak istiyorsak, dönüştürme işlemini kendimiz yapmalıyız.

```go
package main

import (
    "fmt"
)

func yazdir(s []interface{}) {
    for _, sayi := range s {
        fmt.Println(sayi)
    }
}

func main() {
    sayilar := []int{1, 2, 3, 4}
    sayilarSlice:= make([]interface{}, 4)
    for i, sayi := range sayilar{
        sayilarSlice[i] = sayi
    }
    yazdir(sayilarSlice)
}
```
Çıktı:
```go
1
2
3
4
```


## Tip İddiası (Type Assertion)
Arayüz değişkeninin dinamik bir tipten olduğunu iddia etmek ve sakladığı temel değeri elde etmek için kullanılır. Tip İddiası bir arayüzü başka bir veri tipine dönüştürmez ve sadece arayüzlerde kullanılır. 

x.(T) yazımı x değerinin T tipinden olduğunu ileri sürer.

Kullanımına örnek yapalım
```go
package main

import (  
    "fmt"
)

func iddiaEt(i interface{}) {  
    deger := i.(string) 
    fmt.Println(deger)
}
func main() {  
    var a interface{} = "selam!"
    iddiaEt(a)
}
```
Çıktı:
```go
selam!
```
i değişkeninin tipinin string olduğu iddiasında bulunarak değerini değer isimli değişkene atıyoruz.

Peki i değerinin tipi string olmasaydı ne olurdu?
```go
package main

import (  
    "fmt"
)

func iddiaEt(i interface{}) {  
    deger := i.(string) 
    fmt.Println(deger)
}
func main() {  
    var a interface{} = 3
    iddiaEt(a)
}
```
Çıktı:
```go
panic: interface conversion: interface {} is int, not string
```
Bu panic sorununu "comma ok" ifadesi ile çözebiliriz.

deger, ok := i.(T)  

i'nin tipi T ise ok değişkeni true olup deger isimli değişkene i'nin değeri atanacaktır. Eğer iddiamız doğru değilse ok false olup deger isimli değişkenine sıfır değeri atanacaktır. Böylelikle panic oluşmayacaktır.

```go
package main

import (  
    "fmt"
)

func iddiaEt(i interface{}) {  
    deger, ok := i.(int)
    fmt.Println(deger,ok)
}
func main() {  
    var a interface{} = 3
    iddiaEt(a)
}
```
Çıktı:
```go
3 true
```
