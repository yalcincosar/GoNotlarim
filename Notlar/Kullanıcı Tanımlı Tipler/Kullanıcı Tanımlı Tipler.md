# Kullanıcı Tanımlı Tipler

Go dili kendi tiplerimizi tanımlamamıza izin verir. Kullanıcı tanımlı tip tanımlamanın birkaç yolu vardır. En yaygını bileşik bir tür oluşturmamızı sağlayan `struct` kelimesi ile tanımlama yapmaktır.

## Struct

Struct, farklı tipleri tek bir tipte gruplandırmayı sağlar. OOP konseptli dillerdeki sınıflara benzer. Alanları (fields) vardır, gerçek hayattaki varlıkları temsil ederken kullanılır. Structlar inheritance desteklemez, composition destekler. Bundan dolayı hafif sınıf (lightweight class) olarak nitelendirilirler.

### Struct Tanımlama

```go
type ogrenci struct {
    ad string
    soyad string
    ogrenciNo int
}
```
Öğrenci tipinde bir struct tanımladık. Tanımlama `type` ile başlar ardından yeni tipin adı daha sonra ise `struct` kelimesi gelir.
Yukarıda bulunan örnekte 3 tane alanımız (field) var bunlar; `ad`, `soyad`, `ogrenciNo` . Alanlara `.` ile ulaşabiliriz. Örnek olarak `ogrenci.ad` gibi.

Şimdi tanımladığımız tipten değişken oluşturabiliriz.
```go
var ahmet ogrenci // ahmet adında ogrenci tipinde oge oluşturduk
```
ahmet adlı değişkende bulunan alanlara (field) atama yapmadığımız için varsayılan değerleri ile başlatılır. Varsayılan değerlerimiz sayısal tipler için sıfır, boolean tipi için false string için ise boştur.

Struct içindeki alanlara (fields) istediğimiz değerleri vererek başlatabiliriz.
```go
package main

import (
	"fmt"
)
type ogrenci struct {
    ad string
    soyad string
    ogrenciNo int
}
func main() {

	ahmet:=ogrenci{
		ad: "ahmet",
		soyad: "temha",
		ogrenciNo: 342,
	}

	fmt.Print(ahmet)
}
```
Çıktı:
```go
{ahmet temha 342}
```
Örnekte bulunan := operatörü bir değişkeni hem bildirir hem de başlatır.

Yukarıdaki gibi tek tek yazmak yerine değerleri yan yana yazıp virgülle ayırabiliriz. Bu kullanımda sıra önemlidir.
```go
package main

import (
	"fmt"
)
type ogrenci struct {
    ad string
    soyad string
    ogrenciNo int
}
func main() {

	ahmet:=ogrenci{"ahmet","temha",342}
	fmt.Println(ahmet)
}
```
Çıktı:
```go
{ahmet temha 342}
```
### İç İçe Struct
Bir struct başka bir structın alanı olabilir.

```go
package main

import (
	"fmt"
)
type ogrenci struct {
	ad string
	soyad string
	ogrenciNo int
}
type sinif struct {
	ogrenci ogrenci
	isim string
}
func main() {

	onIkiC :=sinif{
		ogrenci: ogrenci{
		ad: "ahmet",
		soyad: "temha",
		ogrenciNo: 342,
	},
	isim: "12C",
	}
	
	fmt.Println(onIkiC)
}
```
Çıktı:
```go
{{ahmet temha 342} 12C}
```
### Anonim Struct
Go anonim struct oluşturmamıza izin verir.
```go
package main

import (
	"fmt"
)

func main() {
	 u := struct {
        isim       string
        soyisim string
    }{
        isim:       "Ahmet",
        soyisim: "Temha",
    }
	
	fmt.Println(u.isim)
	fmt.Println(u.soyisim)
}
```
Çıktı:
```go
Ahmet
Temha
```

### Anonim Alanlar
Struct içindeki alanların isimleri olmayıp sadece tipleri olabilir.
```go
type Ogrenci struct {  
    string
    int
}
```
İsimleri olmasa da varsayılan olarak alanın adı tipinin adıdır. Anonim alanlara erişmek için tip adlarını kullanırız.
```go
package main

import (
	"fmt"
)

type Ogrenci struct {
	string
	int
}

func main() {
	ogr1 := Ogrenci{
		string: "Ahmet",
		int:    342,
	}
	fmt.Println(ogr1.string)
	fmt.Println(ogr1.int)
}
```
Çıktı:
```
Ahmet
342
```
### Type ile Tip Bildirimi
Kullanıcı tanımlı yeni bir tip tanımlamanın ikinci yolu ise var olan tipi yeni bir tip bilirimi ile tanımlamaktır.
Örnek olarak;
```go
type karmasikSayi complex64
```
