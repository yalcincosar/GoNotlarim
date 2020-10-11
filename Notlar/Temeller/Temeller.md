# Giriş

Go, basit, güvenilir ve verimli yazılım oluşturmayı kolaylaştıran açık kaynaklı bir programlama dilidir. Robert Griesemer, Rob Pike ve Ken Thompson tarafından 2007 yılında  Google'da geliştirildi, ancak 2009'da açık kaynaklı bir programlama dili olarak tanıtıldı 

### Avantajları
- Eşzamanlılık sağlaması
- Derleme süresinin hızlı olması 
- Garbage collection
- Büyük sistemler için ölçeklenebilir olması
- Statik bir dil olmasına rağmen dinamik hissiyatı vermesi

### Dezavantajları
- Generics yok (şimdilik)
- Hata yönetimi (error handling)

**Kurulum dökümanına [buradan](https://golang.org/doc/) ulaşabilirsiniz**

## İlk Program

İlk programımız olarak ekrana hello world! yazdıralım, adettendir.
```go
package main

import "fmt"

func main() {
fmt.Println("Hello World!")
}
```
Çıktı:
```go
Hello World!
```

### Yorum Satırı

Derleyici tarafından derlenmesini istemediğimiz yani görmezden gelinmesini istediğimiz şeyler için yorum satırını kullanırız. Yorum satırı genelde kod hakkında bilgi vermek için kullanılır. // ve /* */ olmak üzere iki çeşit kullanımı vardır.

### // (Çift Taksim)
Eklenildiği satırı yorum satırı yapar.
```go
package main

func main() {
// BU BİR YORUM SATIRI
}
```

### /* */
Çoklu yorum satırları oluşturmak için kullanılır. /* ile */ arasında kalan alanlar derleyici tarafından görmezden gelinir.
```go
package main

func main() {
/*
    BU  
    YORUM 
    SATIRIDIR
*/
}
```

# Değişkenler 
Değişken, bir değeri temsil eder. Mesela x = 5, x değişkeni 5 değerini temsil ediyor deriz. Programda kullanılan veriler bellekte saklanır. Bellekte tuttuğumuz verilere bellek adresiyle erişebiliriz. Fakat bu durum karışıklığa yol açar mesela 0x0802 adresinde bir veri tuttuğumuzu düşünelim tek verimiz olsaydı belki karışıklık olmazdı fakat yazdığımız programda birden çok verimiz olduğunda bu adreslerin hangi verilere karşılık geldiğini kestirebilmek oldukça zor olacaktır. Bu sorunun önüne geçmek için değişken kullanırız. Değişken kullanmak bellek adresini isimlendirmektir. Bir değişkene isim verirken seçici davranmak gerekir. Değişkenin programda neyi temsil ettiğiyle ilişkili isim vermek yazdığımız program içindeki değişkenler arttığında karmaşayı önleyip kodumuzu okunaklı kılacaktır.

# Temel Tipler

Go dilinde üç adet temel tipimiz vardır.
Bunlar;
* Sayısal
* String
* Boolean
 
## Sayısal Tipler

Sayısal tipleri tam sayılar (integers) ,kayan noktalı sayılar (floating point numbers) ve karmaşık sayılar (complex numbers) olmak üzere 3 kısımda inceleyelim.

### Tam Sayılar (Integers)

Adından da anlaşılacağı üzere tam sayıları temsil etmek için kullandığımız tiplerdir. Tam sayı oldukları için ondalık kısma sahip değillerdir.
Tanımlama yaparken temsil edeceğimiz değerin işaretli (signed) ve işaretsiz (unsigned) olmasına göre **int** veya **uint** kelimelerini kullanırız.
İşaretsiz demek pozitif sayı demektir. Hiç eksili değerimiz olmayacağı için işaret bitine ihtiyaç yoktur.

Farklı tipler bellekte farklı adres boyutuna sahiptir bu yüzden temsil edeceğimiz sayıların belirli limitleri vardır. İhtiyacımıza göre değişken tipini seçeriz. Mesela int8 tipi bellekte 8 bit yer kaplar. İşaretli sayı olduğu için -128 ile +128 değer aralığına sahiptir. uint8 tipini 8 bit yer kaplayan işaretsiz sayılar için kullanırız. Sınır olarak 0-255 değerlerini temsil eder. Eğer sınırı aşarsak overflow hatası ile karşılaşırız.


| Tipler | Alabileceği değer aralıkları               |
|--------|--------------------------------------------|
| int    | 32bit değer veya 64bit değer alır          |
| int8   | -128 , 127                                 |
| int16  | -32768 , 32767                             |
| int32  | 2147483648 , 2147483647                    |
| int64  | -9223372036854775808 , 9223372036854775807 |
| uint   | 32bit değer veya 64bit değer alır          |
| uint8  | 0 , 255                                    |
| uint16 | 0 , 65535                                  |
| uint32 | 0 , 4294967295                             |
| uint64 | 0 , 18446744073709551615                   |
| byte   | 0 , 255                                    |
| rune   | -2147483648 , 2147483647                   |

byte ve rune dan bahsetmedik. Bahsetmek gerekirse `byte` ` uint8` ile aynıdır, `rune` ise`int32` ile aynıdır.

### Kayan Noktalı Sayılar (Floating Point Numbers)

Kısaca küsüratlı sayılardır ondalık kısımları vardır. Aynı tam sayılardaki kullanım gibi  `float32` ve `float64` ile tanımlanırlar 32bit ve 64bit olarak ayrılmıştırlar.

### Karmaşık Sayılar (Complex Numbers)

Reel ve imajiner kısımları vardır. `complex64`,`complex128` olarak tanımlanırlar.

## String

Bir metni temsil etmek için kullandığımız karakter dizisidir. String değerleri **" "** veya **' '** kullanılarak atanır. 
Aralarındaki fark tek tırnak arasına değer verirken `\n`(yeni satıra geçmek için) `\t`(bir tab boşluk bırakmak için) gibi özel ifadelere izin vermesidir. 

## Boolean

Bir değişkenin mantıksal değer tutmasını istersek `bool` tipinden faydalanırız. Bu tip 1 bit tam sayı kullanır. Bunlar 0 ve 1 değerleridir. 
0 değişkenimizin yanlış (false) 1 ise doğru (true) olduğunu temsil eder. Varsayılan olarak **false** değer alır.   

# Operatörler

Değişkenler üzerinde birtakım işlemler yapan sembollerdir.

## Aritmatik Operatörler

Aritmatik işlemler yapan operatörlerdir.

| Operator |  Tanım  | Form |
|:--------:|:-------:|:----:|
|     +    | Toplama |  x+y |
|     -    | Çıkarma |  x-y |
|     *    |  Çarpma |  x*y |
|     /    |  Bölme  |  x/y |
|     %    |  Modül  |  x%y |
|    ++    | Artırma |  x++ |
|    --    | Azaltma |  x-- |

## İlişkisel Operatörler

Koşul ifadelerini karşılaştır sonuca göre true ya da false değer döndürür.

| Operatör |      Açıklama      | Örnek |
|:--------:|:------------------:|:-----:|
|     >    |      Büyüktür      |  x>y  |
|     <    |      Küçüktür      |  x<y  |
|    >=    | Büyük veya Eşittir |  x>=y |
|    <=    | Küçük veya Eşittir |  x<=y |
|    ==    |       Eşittir      |  x==y |
|    !=    |    Eşit Değildir   |  x!=y |

## Mantıksal Operatörler

Mantıksal işlemler için kullanılır.

| Operatör | Açıklama                                                                                     | Örnek  |
|----------|----------------------------------------------------------------------------------------------|--------|
| &&       | VE operatörü, her iki operandın sıfır olmadığı durumlar için true döndürür                   | x&&y   |
| \|\|     | VEYA operatörü, her iki operanddan herhangi biri true ise true döndürür.                     | x\|\|y |
| !        | DEĞİL operatörü, tersini döndürür. Operand true ise false döndürür, false ise true döndürür. | !x     |

## Atama Operatörleri

Değişkenlere değer atamak için kullanılır.

| Operatör |                           Açıklama                           | Örnek | Eşiti |
|:--------:|:------------------------------------------------------------:|:-----:|:-----:|
|     =    |               Sağına yazılan değeri soluna atar              |  x=y  |   -   |
|    +=    |     Sağdaki değerle soldaki değeri topla sonucu sola ata     |  x+=y | x=x+y |
|    -=    |     Sağdaki değeri soldaki değerden çıkar sonucu sola ata    |  x-=y | x=x-y |
|    *=    |      Sağdaki değeri soldaki değerle çarp sonucu sola ata     |  x*=y | x=x*y |
|    /=    |       Sağdaki değeri soldaki değere böl sonucu sola ata      |  x/=y | x=x/y |
|    %=    | Sağdaki değeri soldaki değere böl bölümünden kalanı sola ata |  x%=y | x=x%y |

## Bitwise Operatörler

Değişkene ait değerin bitleri üzerinden işlem yapmak için kullanılır.

| Operatör |                                                                                       Açıklama                                                                                      | Örnek A 1100, B 1010 |
|:--------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------:|
|     &    |                                                             Bit düzeyinde VE operatörü, karşılıklı bitler 1 ise 1 üretir                                                            |    A&B sonuç 1000    |
|    \|    |                                                     Bit düzeyinde VEYA operatörü, karşılıklı biterden bir tanesi 1 ise 1 üretir                                                     |    A\|B sonuç 1110   |
|     ^    | Bit düzeyinde YA DA operatörü, karşılıklı bitlerin değeri birbirinde farklı ise 1 aynı ise 0 üretir. *Bir değişkeni kendisi ile YA DA işlemine sokarsak değerini sıfırlamış oluruz. |    A^B sonuç 0110    |
|    <<    |                                                         Sola öteleme, sağdaki değer ne kadar sola kaydırılacağını belirtir.                                                         |  A<<3 sonuç 1100000  |
|    >>    |                                                          Sağa öteleme, sağdaki değer ne kadar sağa kaydırılacağını belirtir                                                         |     A>>3 sonuç 1     |

# Değişken Tanımlama ve Inferred Typing

Değişkenler **var** kelimesi ile tanımlanabilir. Bu tanımlamanın bazı yolları vardır.
var kelimesinden sonra değişkenleri paranteze alıp tanımlaya biliriz.
```go 
var (
    isim     string 
    numara      int
    soyisim  string
)
```
Eğer aynı tipten değişken tanımlıyorsak bunları tek satırda virgül ile ayırarak tanımlayabiliriz.
```go
var (
	isim, soyisim string
    numara int
)
```
Topluca tanımlamak yerine tek tek tanımlayabiliriz.
```go
var isim string
var numara int
var soyisim string
```
Ayrıca başlangıç değerlerini tanımlarken verebiliriz.
```go
var (
    isim     string = "yalcin"
    numara      int = 342
    soyisim  string = "cosar"
)
```
Yine istersek tip kısmını atlayıp atama yapabiliriz, atanan değere göre çıkarım yaparak değişken tipi belirlenir. Bu olaya **inferred typing** denir.
```go
var (
    isim     = "yalcin"
    numara   = 342
    soyisim  = "cosar"
)
```
İşleri biraz daha kolaylaştırmak istersek tek bir satırda değişkenlerimizi tanımlayıp başlangıç değerlerini atayabiliriz.
```go
var (
	isim, numara, soyisim = "yalcin", 342, "cosar"
)
```
Bir fonksiyon içinde  **var** yerine **:=** kullanarak da değişken bildirimi yapabiliriz.
```go
func main(){
    isim,soyisim := "yalcin","cosar"
    numara := 342
}
```
Go dilinde değişkenler herhangi bir tür içerebilir. Mesela fonksiyon atayabiliriz. 
```go
func main() {
    yapBiseyler := func() {
        fmt.Println("ne gibi mesela")
    	} 
    yapBiseyler()
}
```
**= vs :=**
Kısaca = ile değişken tanımlamak için var kelimesini kullanmalıyız fakat := kullanıyorsak var kelimesine gerek yoktur bu kullanımda unutulmaması gereken fonksiyon dışında **:=** ifadesi ile atama yapamayız.

# Tip Dönüşümü (Type Conversion)

Tipler arası dönüşüm yapmak isteyebiliriz. Bunu `dönüştürmekİstediğimizTip(dönüştürülecekOlanDegisken)` şeklinde yaparız.
```go
var sayi int = 10;
var sayi2 float32 =float32(sayi); 
```
Yukarıdaki örnekte `sayi` isminde bir `int` tanımayıp 10 değerini atadık. Daha sonra `sayi2` ismiyle `float32` tipinde bir değişkene int olan sayi isimli değişkenimizi `float32` tipine dönüştürerek atadık.

# Sabitler (Constants)

Sabit dediğimiz değişkenler ilk değerlerini aldıktan sonra değiştirilemezler. Atama yapılmış bir sabiti değiştirmek compiler-time (derlenme zamanı) hatası ile sonuçlanır. 

Sabitlerin tanımlanması değişkenlere benzer fakat bazı farklılıklar vardır. 
Hatırlarsanız değişken tanımlarken **var** kullanıyorduk sabitlerde ise **const** kelimesi kullanılır. 
```go
    const x string = "Hello World"
```
Bir diğer farkımız ise sabitlere atama yaparken := ifadesi kullanılamaz. 
```go
	const Pi:=3.14 //hata verecektir
```

# Kapsam (Scope)
	
Değişkenin nerede tanımlandığı, değişkenin görünürlüğü için önemlidir. Kapsam bir değişkenin görünebilir olduğu alandır. Go da üç farklı yerde değişken tanımlama yapılabilir, tanımlanın yapıldığı yere göre Global değişken, yerel değişken ve parametre değişkeni denilir. 

```go
  { //KAPSAM BAŞLANGIÇ
   BU BÖLGE BİR KOD BLOĞU
    }//KAPSAM BİTİŞ
```
## Global Değişken

Herhangi bir fonksiyon içinde ya da bir kod bloğu içinde tanımlanmayan genellikle programın en üstünde tanımlanan değişkenlerdir. Bu değişkenler her yerden görünebilir.

## Yerel Değişken

Bir fonksiyon ya da bir kod bloğu içinde tanımlanan değişkenlerdir. Sadece tanımlandığı blok veye fonksiyon içinden görünürdür.

Aşağıda bulunan örneğe bakalım. main() fonksiyonunun başladığı küme parantezi  ile bittiği küme parantezi bizim yerel kapsamımızdır. 
Burada tanımladığımız  x değişkeni **yerel değişkendir**. main() fonksiyonu kendi sınırları içerisinde bulunan bu değişkene ulaşmakta sorun yaşamaz.
Peki ya dışarıdan x değişkenimize ulaşabilir miyiz? 
 	
```go
package main

import "fmt"
var g string ="Merhaba Evren"
func main() {
  var x string = "Merhaba Dünya"
  fmt.Println(x)//Ekrana Merhaba Dünya yazdırır
}
fmt.Println(x)
```
Çıktı:
```go
syntax error: non-declaration statement outside function body
```
Cevabımız gördüğünüz gibi hayır.

Programımızda g isminde bir değişkenimiz daha var. Bu değişkenimiz global değişkendir ve heryerden erişebiliriz. main() fonksiyonu içinden g değişkenimize erişmeye çalışalım.

```go
package main

import "fmt"
var g string ="Merhaba Evren"
func main() {
  var x string = "Merhaba Dünya"
  fmt.Println(g)  
  fmt.Println(x)
}
```
Çıktı:
```go
Merhaba Evren
Merhaba Dünya
```
Görüldüğü gibi g değişkenine sorunsuz bir şekilde erişebildik.

İsmi aynı olan global ve lokal olmak üzere iki farklı değişken tanımlayabiliriz. Peki fonksiyon içinde değişkeni kullanmak istedik. Go hangisini tercih eder ? 
```go
package main

import "fmt"
var g string ="Merhaba Evren"
func main() {
  var g string = "Merhaba Dünya"
  fmt.Println(g)  
}
```
Çıktı:
```go
Merhaba Dünya
```
Lokal değişkeni kullanmayı tercih eder.

## Parametre Değişkeni

Fonksiyon parametresindeki değişkenlerdir. Yerel değişkenler gibi fonksiyon içinde heryerden erişilebilir fakat dışarıdan erişilemezler

```go
func topla(a, b int) int {
    return a+b
}
```
a ve b bizim parametre değişkenlerimizdir.


