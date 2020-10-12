# Diziler (Arrays) 
Dizi aynı tip elemanları art arda sıralı bir şekilde tutan, sabit uzunluklu bir veri yapısıdır. Dizi elemanlarına indis veya index denilen değerlerle erişiriz. İndis 0 değerinden başlar. Diziler için bellek sıralı bir şekilde tahsis edilir. Bu nedenle dizi içinde hareket etmek hızlıdır. İndis aritmatiği ile dizi içinde dolaşabiliriz.
### <img src="https://github.com/FYalcincosar/GoNotlarim/blob/main/Notlar/Array%20Slice%20Range%20Map/img/Array.svg?example=foo&sanitize=true>">

## Tanımlama ve Başlatma (Initializing)

Dizi tanımlarken elemanlarının hangi tipten olacağı ve dizinin boyutunun ne olacağı belirterek tanımlanır. Tanımlama yapıldıktan sonra eleman tipi ve dizinin boyutu değiştirilemez. Daha fazla boyuta ihtiyaç olması halinde yeni bir dizi tanımlanır, elemanlar yeni diziye kopyalanır.

```go
var array [6]int //6 elemanlı int tipinden bir array tanımladık
```
Go dilinde bir dizi tanımladığımızda dizinin elemanları 0 değeri ile başlatılır. Yukarıda tanımladığımız dizinin elemanlarını ekrana yazdıralım.
```go
package main
import "fmt"
func main() {
	var array [6]int
	
	fmt.Println(array)
}
```
Çıktı:
```go
[0 0 0 0 0 0]
```
Diziyi 0 ile başlatmak yerine değer vererek  başlatabiliriz.
```go
package main
import "fmt"
func main() {

	array:= [6]int{50,60,70,80,90,100}
	
	fmt.Println(array)
}
```
Çıktı:
```go
[50 60 70 80 90 100]
```
Eleman sayısı belirtmeden değer vererek diziyi başlatabiliriz. Go, dizinin uzunluğunu kendi belirler. Bu kullanım için eleman sayısı yerine `...` yazmalıyız.
```go
 array:= [...]int{50,60,70,80,90,100}
```
Dizide istediğimiz indislere değer vermek istersek;
```go
 array:= [6]int{1: 60, 3: 80, 5: 100}
```
Değer verilmeyen indislere otomatik olarak 0 değeri atanacaktır.

Bir dizinin elemanlarına `dizi_adı[indis]` şeklinde erişiriz.

```go
package main
import "fmt"
func main() {

	array:= [6]int{50,60,70,80,90,100}
	fmt.Println("Dizi : ",array)
    array[3]=5  // 3. indisin değerini 5 yapıyoruz 

	fmt.Println("Yeni hali : ",array)
}
```
Çıktı;
```go
Dizi :  [50 60 70 80 90 100]
Yeni hali :  [50 60 70 5 90 100]
```
Go dilinde diziler de bir değerdir aynı tipteki başka bir diziye atanabilir.

```go
package main
import "fmt"
func main() {
    var dizi1 [3]int

    dizi2:= [3]int{1,2,3}
    
    dizi1=dizi2
    	
    fmt.Println(dizi1)
}
```
Çıktı:
```go
[1 2 3]
```

## Çok Boyutlu Diziler

Elemanları dizi olan diziler diyebiliriz. Bu kısma kadar gördüğümüz diziler tek boyutluydu. Çok boyutlu dizilere matrisler örnek verilebilir. 
Örnek olarak 2x2 matris oluşturalım.
```go
package main
import "fmt"
func main() {

array := [2][2]int{{1, 2},{3, 4}}

	for i:=0; i<2; i++{
	
	   for k:=0; k<2; k++{
		fmt.Print(array[i][k]," ")
	  }
	fmt.Println()	
	}
	
}

```
Çıktı:
```go
1 2 
3 4
```

Çok boyutlu diziler aynı tür oldukları sürece birbirlerine kopyalanabilir.

Bir diziyi fonksiyonun parametresine geçirebiliriz fakat bu işlem dizinin boyutuna bakılmaksızın diziyi fonksiyona kopyalanacağı için maliyetlidir. Bunun yerine işaretçi (pointer) kullanmak daha performanslı olacaktır.

# Dilimler (Slices)

Dilim, dizi gibi tek veri tipinden elemanlara ve uzunluğa sahip, Üç bileşenli veri yapılarıdır. Bu bileşenler: İşlem yaptığımız dizinin ilk elemanının adresini tutan pointer, uzunluk ve kapasitedir. Dilim, dizinin üstünde bulunan dinamik bir katman gibi düşünülebilir.

### <img src="https://github.com/FYalcincosar/GoNotlarim/blob/main/Notlar/Array%20Slice%20Range%20Map/img/Slice.svg?example=foo&sanitize=true>">

Dilimin diziye göre en önemli farkı boyutun değişmesine imkan vermesidir.

Bir dilimin uzunluk değerini öğrenmek için `len()` fonksiyonundan, kapasitesini öğrenmek için ise `cap()` fonksiyonundan yararlanırız.

### Dilim Oluşturma ve Başlatma

Dilim oluşturmanın birkaç yolu vardır. Bunlardan biri dilim uzunluğunun belirli olduğu durumlar için `make()` fonksiyonunu kullanmaktır.

```go
package main
import "fmt"
func main() {

   dilim := make([]byte, 5)
   
   fmt.Println("Kapasitesi :",cap(dilim)," Uzunluğu: ", len(dilim))
}
```
Çıktı:
```go
Kapasitesi : 5  Uzunluğu:  5
```
Uzunluğu ve kapasitesi 5 olan bir byte dilimi oluşturduk. Sadece uzunluğu belirtirsek dilimin kapasitesi de aynı olur. İstersek kapasite ve uzunluğu farklı verebiliriz. 

Uzunluğu 2 eleman kapasitesi 5 olan dilim 
```go
package main
import "fmt"
func main() {

   dilim := make([]byte,2, 5)
   
   fmt.Println("Kapasitesi :",cap(dilim)," Uzunluğu: ", len(dilim))
}
```
Çıktı:
```go
Kapasitesi : 5  Uzunluğu:  2
```
Uzunluk değerinden daha küçük bir kapasite değeri ile dilim oluşturmaya çalışmak hataya sebep olur.
```go
package main
import "fmt"
func main() {
    x := make([]byte, 5, 2)
}
```
Çıktı:
```go
len larger than cap in make([]byte)
Go build failed.
```
Dilim oluşturmanın ikinci bir yolu ise eleman sayısı vermeden dizi oluşturmaya benzemekte
```go
dilim:= []string{"yalcin","cosar"}
```
Uzunluğu ve kapasitesi eklediğimiz eleman sayısına göre değişecektir. 
Bu yöntemi kullanarak da dilimin uzunluk ve kapasitesini ayarlayabilirsiniz. Örnek olması açısından 10 kapasite ve uzunluğa sahip string tipinden bir dilim oluşturalım.
```go
package main
import "fmt"
func main() {

   dilim:= []string{9: ""}
   
   fmt.Println("Kapasitesi :",cap(dilim)," Uzunluğu: ", len(dilim))
}
```
Çıktı:
```go
Kapasitesi : 10  Uzunluğu:  10
```
## Nil ve Boş Dilimler

### <img src="https://github.com/FYalcincosar/GoNotlarim/blob/main/Notlar/Array%20Slice%20Range%20Map/img/NilSliceRepresentation.svg?example=foo&sanitize=true>">
Nil diliminin uzunluğu ve kapasitesi sıfırdır üzerinde işlem yaptığı dizisi yoktur bundan dolayı adres değeri tutan işaretçisi de nildir. 

### <img src="https://github.com/FYalcincosar/GoNotlarim/blob/main/Notlar/Array%20Slice%20Range%20Map/img/EmptySliceRepresentation.svg?example=foo&sanitize=true>">
Boş diliminin de uzunluğu ve kapasitesi sıfırdır fakat boş dilimlerin üzerinde işlem yaptığı sıfır uzunluğa sahip dizisi vardır yani adres tutan işaretçisi nil değildir.


Konumuzla bağlantılı olarak go dilinde dilim oluşturmanın en yaygın yolu nil dilim oluşturmaktır. Bir nil dilimi aşağıdaki gibi oluşturulur.

```go
var dilim []int
```
Nil dilimini test edelim.
```go
package main
import "fmt"
func main() {
	var dilim []int
	fmt.Println(dilim == nil)
}
```
Çıktı:
```go
true
```

Boş bir dilim oluşturmak demek kapasitesi ve uzunluğu sıfır olması fakat işlem yaptığımız dizinin adres değerinin de olması demekti. Bunu `make()` fonksiyonu ile yapabiliriz. 
```go
dilim := make([]int, 0)
```
Şimdi nil mi değil mi test edelim.

```go
package main
import "fmt"
func main() {
	dilim := []int{}
	fmt.Println(dilim == nil)
}
```
Çıktı: 
```go
false
```
Boş dilim oluşturmanın bir başka yolu:

```go
dilim := []int{}  //boş dilim oluşturuyoruz
```

## Dilimlerle İşlem Yapma

Dilimde bir indise değer atamak dizilerde yaptığımızın aynsıdır.

```go
dilim := []int{50,100,150}

dilim[1] = 0 //1.indise 0 değerini atıyoruz..
```
Var olan bir dilimden dilim oluşturmak için:
```go
package main
import "fmt"
func main(){
    dilim := []int{1,2,3,4,5}
    yeniDilim:= dilim[1:4]
    fmt.Println(yeniDilim)
}
```
Çıktı:
```go
[2 3 4]
```
Örnekte [başlangıç indisi:bitiş indisi] şeklinde bir kullanım vardır. Başlangıç ve bitiş indisleri arasındaki değerlerimiz yeni dilimimizdir.

Burada ikisininde üzerinde işlem yaptığı dizi aynıdır. Bir dilim üzerinde yapılan bir değişiklik diğer dilimi etkiler. Önemli bir ayrıntı olarak kodumuzdaki `yeniDilim`'in adres pointerı `dilim`'in işaret ettiği dizinin 1. indisini işaret eder yani `yeniDilim` isimli dilimimiz `dilim` isimli dilimde bulunan 0. indisimiz içindeki 1 değerini göremez ona asla erişemez.


## Yeni Eleman Eklemek

Dilime yeni eleman eklemek için `append()` fonksiyonu kullanılır. 2 parametresi vardır. Bunlar sırayla eleman eklemek istediğimiz dilim ve ekleyeceğimiz elemanlardır. Birden çok eleman ekleyebiliriz. Birden çok eleman eklerken virgül kullanırız.

```go
package main
import "fmt"
func main() {
    dilim := []string{}
    dilim = append(dilim, "1.eleman")
    dilim = append(dilim, "2.eleman", "3.eleman")

    fmt.Println(dilim)
}
```
Çıktı:
```
[1.eleman 2.eleman 3.eleman]
```

# Range

String, dizi (array), dilim (slice), map, ve channel üzerinde iterasyon (yineleme) yapmak için for döngüsü ile birlikte range kullanılır. Range 2 tane değer döndürür (channel hariç). Bu dönüş değerleri, üzerinde iterasyon yaptığımız veri yapısına göre değişiklik gösterir. dizi ya da dilim için ilk değer indis ikinci değer o indise karşılık gelen eleman, map için ilk değer key ikici değer value, string için ilk değer indis ikinci değer o indise karşılık gelen rune tipinde eleman, channel için ilk değer channel'ın elemanı ikinci değer zaten yoktur. 

Dilim üzerinde range kullanımı:

```go
package main
import "fmt"
func main() {

	slice := []int{10, 20, 30, 40, 50}
	
	for indis, deger := range slice {

        fmt.Println("indis", indis, "deger", d)
	}
}

```
Çıktı:
```go
indis 0 eleman 10
indis 1 eleman 20
indis 2 eleman 30
indis 3 eleman 40
indis 4 eleman 50
```
Ayrıca döndürdüğü değerler aslında bizim esas değerlerimiz değil onların bir kopyasıdır. Döndürdüğü elemanların adresleri hep aynıdır.

```go
package main
import "fmt"
func main() {

	slice := []string{"A", "B", "C", "D", "E"}
	
	for indis, deger := range slice {

        fmt.Printf("DegerAdres: %X elemanAdres: %X \n", &deger,&slice[indis] )
	}
}

```
Çıktı:
```go
DegerAdres: C000010200 elemanAdres: C00005C050 
DegerAdres: C000010200 elemanAdres: C00005C060 
DegerAdres: C000010200 elemanAdres: C00005C070 
DegerAdres: C000010200 elemanAdres: C00005C080 
DegerAdres: C000010200 elemanAdres: C00005C090 
```
Örnekte gördüğünüz gibi `DegerAdres` hep aynı adresi vermiştir. Bu yüzden dönen değerler üzerinden işlem yaparken dikkatli olmalıyız.

# Map

Sırasız bir şekilde key-value çiftleri tutan veri yapısıdır. Key değerleri benzersiz olmalıdır. Key sayesinde valuelara hızlı bir şekilde ulaşırız.

Map oluşturmak için `make()` fonksiyonunu kullanabiliriz.
```go
mapIsmi:=make(map[key tipi]value tipi)
```
Ekleme yaparken:
```go
mapIsmi[key] = value
```
Örnek yapalım,
```go
package main
import "fmt"
func main() {

	ogrenciler := make(map[int]string)
	
	ogrenciler[01]="Ahmet"
	ogrenciler[02]="Selin"
	ogrenciler[03]="Oğuz"
	ogrenciler[04]="Nuray"
	
	fmt.Println(ogrenciler)
}
```
Çıktı:
```go
map[1:Ahmet 2:Selin 3:Oğuz 4:Nuray]
```
Tanımlarken key-value değerleri ile başlatabiliriz.

```go
package main
import "fmt"
func main() {

	ogrenciler := map[int]string{
	
	 01: "Ahmet",
	 02: "Selin",
	 03: "Oğuz",
	 04: "Nuray",
   }

	fmt.Println(ogrenciler)
}
```
Çıktı:
```go
map[1:Ahmet 2:Selin 3:Oğuz 4:Nuray]
```
Nil map tanımlama,
```go
package main
import "fmt"
func main() {
	var mp map[string]int
	fmt.Println("Map nil mi?")
	if mp == nil {
		fmt.Println("nil")
	} else{
		fmt.Println("nil değil")
	}
}
```
Çıktı:
```go
Map nil mi?
nil
```
nil olan bir map'e eleman atamak istersek programımız panic verecektir.
```go
func main() {
	var mp map[string]int

	mp["one hundred"] = 100
}
```
Çıktı:
```go
panic: assignment to entry in nil map

goroutine 1 [running]:
main.main()
        /home/ylcncosar/go/src/Tutorial/main.go:10 +0x4b

```


