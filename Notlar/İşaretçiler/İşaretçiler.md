# İşaretçiler (Pointers)

Pointer bellek adreslerini saklayan değişkenlerdir. Herhangi bir değişkeni tanımladığımızda hafızada değişkene bir yer tahsis edilir. Pointerlar bu yerlerin adreslerini tutar. Go dilinde pointer kullanılır fakat C dilindeki gibi bir pointer aritmatiği yoktur.

Bir değişkenin hafızadaki adresini değişkenin önüne ampersand (**&**) koyarak öğreniriz. 
```go
var isim = "yalcin"
fmt.Println(&isim) //isim değişkeninin hafızadaki adresini print edecek, örnek olarak 0xc000010200
```
Eğer elimizde bir hafıza adresi varsa ve bu adresin içinde bulunan değeri öğrenmek istiyorsak hafıza adresini tutan değişkenin önüne `*`  koyarız.
```go
var isim = "yalcin"
var p = &isim //isim değişkeninin hafızadaki adresini p değişkenine atadık
fmt.Println(*p) //ekrana yalcin yazdırır.
```

## Pass by Value

Bir değişkeni fonksiyonun parametresine geçirdiğimizde bellekte değişkenin yeni bir kopyası oluşturulur ve fonksiyona aktarılır. Yeni oluşan kopyanın bellekteki adresi, parametreye geçirdiğimiz değişkenin bellek adresinden farklı olmasından dolayı iki değişkenin birbirinden farklı olduğu sonucu ortaya çıkar. Yani fonksiyonda kullanılan kopya değişken üzerinden yapılan işlemler esas değişkenimizi değiştiremez.

Bu duruma bir örnek verelim;
```go
package main

import "fmt"

func birArttir(x int) {
  x +=1
}
func main() {
  x := 3
  birArttir(x)
  fmt.Println(x) 
}
```
Çıktı:
```go
3
```
Görüldüğü gibi x değişkeninin değeri 4 olmadı 3 olarak kaldı.

## Pass by Pointer

Pointer tarafından değişkenimizi parametreye geçirmemiz durumunda aynı bellek adresini işaret eden yeni bir kopya pointerımız olur. Burada anlaşılması gereken fonksiyona değişkenimizin adresini yolladığımız ve fonksiyonun parametresine geçirdiğimizde oluşan kopya pointerın bu adresi işaret etmesidir. Böylelikle fonksiyonda yapılan işlem bizim değişkenimizi değiştirebilir.

Bu duruma bir örnek verelim

```go
package main

import "fmt"

func birArttir(x *int) {
  *x +=1
}
func main() {
  x := 3
  birArttir(&x)
  fmt.Println(x) 
}
```
Çıktı:
```go
4
```
## new()

Pointer oluşturmanın diğer bir yolu `new()` fonksiyonunu kullanmaktır.
new() fonksiyonu argüman olarak değişken alır ve aldığı değişkenin boyutu kadar bellekte yer ayırıp geriye bir pointer döndürür.
```go
package main

import "fmt"

func birArttir(p *int) {
  *p +=1
}
func main() {
  p := new(int)
  *p = 1		
  birArttir(p)
  fmt.Println(*p) 
}
```
Çıktı:
```go
2
```
