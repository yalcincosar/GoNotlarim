# Fonksiyonlar

Girdi alabilen aldığı girdiler üzerinde işlemler yapan ve bunun sonucunda çıktı üreten yapılarımızdır. 

Fonksiyonlar programımızı küçük kod parçalarına bölmemizi sağlar. Bu kod parçaları sayesinde programımızda yeniden kullanılabilirliği ve okunabilirliği sağlarız..

## Fonksiyon Tanımlama ve Kullanım

Go dilinde fonksiyonlar `func` kelimesiyle başlar ardından fonksiyonumuzun ismi sonra parametreleri (tipleriyle birlikte ve virgülle ayırırız) daha sonra dönüş tipi ve fonksiyonumuzun gövdesi gelir. Bir değer döndürmek istersek `return` kelimesini kullanırız.
```go
func fonksiyonumuzunIsmi(parametreAdı tipi) dönüşTipi{
    //fonksiyonumuzun gövdesi


    //eğer bir şeyler döndürmek istersek
    return deger;
}
```
Girilen sayının karesini alan bir fonksiyon yazalım;
```go
package main

import "fmt"

func karesiniAl(sayi int)int{
	return sayi*sayi
}

func main() {
	fmt.Println(karesiniAl(4)) //Fonksiyon çağrısı yapıyoruz
}

```
Çıktı:
```go
16
```
Parametre ve return isteğe bağlıdır. Parametresiz ve herhangi birşey döndürmeyen fonksiyon yazabiliriz. 
```go
package main

import "fmt"

func selamVer(){
    fmt.Println("Selamlar!")
}

func main() {
    selamVer()//Fonksiyon çağrısı yapıyoruz.
}
```
Çıktı:
```go
Selamlar!
```
Eğer girdi veya çıktılarımızın tipi aynıysa bir kere yazıp virgülle ayırmak yeterlidir.
```go
package main

import "fmt"

func carp(sayi1,sayi2 int)int{
    return sayi1*sayi2
}

func main() {
    fmt.Println(carp(2,3))
}
```
Çıktı:
```go
6
```
## Variadic Fonksiyonlar

Girdi sayısı bilinmeyen kısaca n adet girdi için kullandığımız fonksiyon türüdür.

Örneğimizi inceleyelim

```go
package main
import "fmt"

    func carp(girdiler ...int) int {
	    if girdiler!=nil{
		    sonuc := 1
		    for _, n := range girdiler {
			    sonuc *= n
		    }
		    return sonuc
	    }else {return 0};
	}
	func main(){
		fmt.Println(carp(1,2,3))
    fmt.Println(carp(1,2,3,4,5,6))
	}
```
Çıktı:
```go
6
720
```
Girdiler isminde int tipinde bir değişkenimiz var tipinin önündeki  `...` n adet girdi alabileceğini belirtir (n sıfır olabileceğini unutmayalım). Çarpma işleminde 1 sayısı etkisiz eleman olacağı için sonuc değişkenine 1 atadık. Eğer girdimiz olmazsa 0 değerini döndüruyoruz. Örneğimizde for döngüsü içinde range kullandık, range kulllanımı için buraya bakabilirsiniz.

## Birden Çok Değer Döndüren Fonksiyonlar

Fonksiyonda birden çok değer döndürülebilir. Hem karesini hem küpünü alan bir fonksiyon yazalım.

```go
package main

import (
	"fmt"
)

func hemKareHemKupAl(sayi int)(int, int) {

	return sayi*sayi, sayi*sayi*sayi
}

func main() {
	karesi, kupu := hemKareHemKupAl(4)//fonksiyon çağrısı yapıp dönen değerleri değişkenlerimize atıyoruz
	fmt.Println("karesi: ", karesi," küpü: ",kupu)
}
```
Çıktı:
```go
karesi: 16 küpü: 64
```

## Anonim Fonksiyonlar
Go dili anonoim fonksiyon oluşturmaya izin verir.
```go
package main 
  
import "fmt"
  
func main() { 
      
    // Anonim fonksiyon 
   func(){ 
  
      fmt.Println("Bu bir anonim fonksiyon") 
  }() 
    
} 
```
Çıktı:
```go
Bu bir anonim fonksiyon
```
Anonim fonksiyonlara da argüman geçilebilir.
```go
package main

import (
	"fmt"
)

func main() {
	func(s string) {
		fmt.Println("Merhaba", s)
	}("Dünya")
}
```
Çıktı:
```go
Merhaba Dünya
```
## Closure Fonksiyonlar

Özel bir anonim fonksiyondur. Kapsamı dışında tanımlanan değişkenlere erişebilirler.
```go
package main

import (
	"fmt"
)

func main() {
	str := "Merhaba Dünya"
	func() {
		fmt.Println(str)
	}()
}

```
Çıktı:
```go
Merhaba Dünya
```
Örnekte değişkene anonim bir foksiyon atadık ve değişkenimizi fonksiyon gibi kullandık.

# Recursive (Özyineli) Fonksiyonlar 

Fonksiyonu kendi içinde tekrardan çağırıp kullandığımız fonksiyonlardır.
Fonksiyon içinde onu sonlandıracak bir koşul olmazsa fonksiyon kısır döngüye girecektir.
```go
package main

import  "fmt"

func  main()  {

fmt.Println(faktoriyelHesapla(4))

}

func  faktoriyelHesapla(sayi int)  int  {

if sayi ==  0  {

    return  1

}

    return sayi *  faktoriyelHesapla(sayi-1)

}
```
Çıktı:
```
24
```
# Defer 

Defer kelimesi ertemele, sonraya bırakma anlamındadır. Go dilinde bir fonksiyonu daha sonra çalıştırmak için başına defer kelimesini eklenilir. Defer kelimesiyle nitelenen fonksiyon bulunduğu kapsamdaki bütün işlemler yürütüldükten sonra çalışacaktır.

Örnek:
```go
package main

import "fmt"

func birinci() {
  fmt.Println("1.")
}
func ikinci() {
  fmt.Println("2.")
}
func ucuncu() {
  fmt.Println("3.")
}
func main() {
  defer ikinci()
  birinci()
  ucuncu()
}
```
Defer kelimesiyle ikinci() isimli fonksiyon erteleniyor. Kapsam içindeki bütün işlemler bittikten sonra ertelenen fonksiyon çalışıyor 
```go
    1.
    3.
    2.
```
Defer olmasaydı:
```go
    1.
    2.
    3.
```
Peki bir kapsam içinde birden çok defer varsa ne olur? Defer yapısı son giren ilk çıkar dediğimiz LIFO (Last In First Out) denilen son giren ilk çıkar mantığıyla çalışır yani ilk yazılan defer en son, son yazılan defer ise deferler arasında ilk çalışan olur.
```go
package main

import "fmt"

func birinci() {
  fmt.Println("1. Defer")
}
func ikinci() {
  fmt.Println("2. Defer")
}
func ucuncu() {
  fmt.Println("3. Defer")
}
func main() {
  defer birinci()
  defer ikinci()
  defer ucuncu()
}
```
Çıktı:
```go
    3. Defer
    2. Defer
    1. Defer
```
