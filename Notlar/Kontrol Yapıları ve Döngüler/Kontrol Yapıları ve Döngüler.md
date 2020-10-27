# If Else Yapısı

Bazen bir koşul belirtir, koşulumuzun sağlandığı ya da sağlanmadığı durumlara göre bir takım işlemler yapmak isteriz. Bu durumlar için if-else yapısını kullanırız. If kelimesinden hemen sonra koşulu (koşul bool tipindedir, true ise sağlanır, false ise sağlanmaz) belirtir, daha sonra koşulun sağlanması durumunda çalışmasını istediğimiz kodlarımızı bir kod bloğu içine yazarız. 
```go	
	if koşul{ 
    	...koşulun sağlandığı durum için çalışmasını istediğimiz kodlar
	}
```
Koşulun sağlanmadığı durumda ise yapmak istediğimiz işlemler için Else kelimesinden sonra bir kod bloğu açar içine kodlarımızı yazarız.
```go	
	if koşul{ 
    	...koşulun sağlandığı durum için çalışmasını istediğimiz kodlar
	}else{
        ...koşulun sağlanmadığı durum için çalışmasını istediğimiz kodlar
    }
```
 Burada bir kontrol yapısı daha var; else if yapısı. Bu yapıyı gerçekleşebilecek ihtimallerin sayısı ikiden fazlayla kullanırız. If else kontrol bloklarımız yukarıdan aşağıya doğru değerlendirilir. Eğer hiçbir koşul sağlanmadıysa else bloğu çalışır.
```go
	
	if koşul{ 
    	    ...
	} else if koşul 2{ 
    	    ...
	} else if koşul 3{ 
            ...
	}else{
    	    ...
	}
```
Genelde programlama dillerinde koşul kısmı parantez içine yazılır. Go dilinde ise koşul için paranteze gerek yoktur.Bir başka farklılık ise else ifadesi if bloğunun bittiğini belirten küme parantezi ile aynı satırda olmalıdır yoksa program hata verecektir.

```go
package main

import (
	"fmt"
)
func main() {
	
	if 2+2==4 {
		fmt.Println("Ee yani")
	} 
    else{
		fmt.Println("2+2=5'tir")
	}
}
```
Çıktımız:
```go
./prog.go:11:2: syntax error: unexpected else, expecting }
```
Gördüğünüz gibi hatamızı aldık. Bunun nedeni go'nun otomatik olarak `;` eklemesidir. Bu konuda daha fazla bilgi için [bağlantıya](https://golang.org/ref/spec#Semicolons) göz atabilirsiniz.

Ufak bir örnek yapalım int tipinde ortalama isimli değişkenimiz olsun. Eğer ortalamamız 50 üstündeyse dersi geçmiş olalım ve ekrana başarılı, ortalamamız 40-49 arası ise ekrana şartlı geçiş, diğer durumlar dersten kaldığımız durumlardır bu durumlar için ekrana başarısız yazdıralım.

```go
package main

import (
	"fmt"
)
func main() {
	var ortalama int
	fmt.Println("Ortalamanızı giriniz...")
	fmt.Scanf("%d",&ortalama)
	if ortalama>=50 {
		fmt.Println("Başarılı")
	} else if ortalama>=40 && ortalama<=49{
		fmt.Println("Şartlı geçiş")
	} else{
		fmt.Println("Başarısız")
	}
}
```
Çıktı:
```go
Ortalamanızı giriniz...
45
Şartlı geçiş
```
Ortalama olarak 45 değerini girdim, ve programımız ekrana şartlı geçiş yazdırdı.

İf yapısında değişken oluşturup atama yapabiliriz.
```go
package main

import (
	"fmt"
)
func main() {
	if yas:=18; yas >=18 {
		fmt.Println("Hoş Geldiniz")
	} else{
		fmt.Println("18 Yaş altında iseniz giremezsiniz")
	}
}
```
Bu kullanımda dikkat etmemiz gereken değişkenimizi sadece if-else yapısında kullanabiliyor olmamızdır.
```go
package main

import (
	"fmt"
)
func main() {
	if yas:=18; yas >=18 {
		fmt.Println("Hoş Geldiniz")
        yas++
	} else{
		fmt.Println("18 Yaş altında iseniz giremezsiniz")
	}
    fmt.Println("yas: ",yas)
}
```
Çıktı:
```go
./prog.go:13:25: undefined: yas
```

# For Döngüsü

Bir kodun tekrar tekrar çalışması için döngüleri kullanırız. Go programlama dilinde döngü olarak for döngüsü vardır. Diğer dillere nazaran for kullanımı biraz daha esnektir. Bu esneklik ile diğer döngü türlerini de oluşturabiliriz. For döngümüzün genel yapısını 3 kısımda inceleyelim.

```go
for baslatma; koşul; güncelleme {
    //Döngü içinde yapılacak işlemler	
}
```

Başlatma: Döngüyü kontrol etmek için sayısal tipte bir değişkenimizin başlatıldığı kısımdır. Genelde 0'dan başlatılır.

Koşul: Eğer koşul sağlanırsa programımız for bloğunun içine girer (yani döngüye girer).

Güncelleme: Döngüyü kontrol etmek için oluşturduğumuz değişkenin değerinin arttırıldığı yada azaltıldığı kısımdır.

Örnek olarak;	
```go
package main

import (
	"fmt"
)
func main() {
	for i :=  10; i > 0; i--  {
	fmt.Println("i: ",i)	
   }
 }

```

Esneklikten bahsetmiştik bu esneklik sadece koşul kısmıyla da for döngüsü oluşturabiliriz. Yani başlatma ve güncelleme kısmı olmadan da döngümüz sorunsuz çalışacaktır. Bu esneklik sayesinde diğer dillerde olan while döngüsünü oluşturabiliriz. Ufak bir örnek yapalım.
```go
package main

import (
	"fmt"
)
func main() {
	var sayac = 10

	for sayac > 0 {
    	fmt.Println("sayac: ",sayac)
	sayac--
    }
}
```
## Sonsuz Döngü (Forever Loop)

Koşul kısmını yazmadan da döngümüz sorunsuz çalışacaktır hatta o kadar sorunsuzdur ki koşul olmadığı için sonsuza kadar dönebilir.
 ```go
package main

func main() {
	for {
	}
}
```

# Switch Case Yapısı

If else blokları yerine switch case yapısı kullanılabilir. Çalışma mantığı switch e karşılık gelen ifadenin değeri case bloklarında aranır. Eğer bir eşleşme olursa case bloğu içindeki kod çalışır. Örnek vermek gerekirse;
```go
package main

import "fmt"

func main() {
    v:=1
	switch v {
	case 0:
		fmt.Println("v değişkeninin değeri 0")
	case 1:
		fmt.Println("v değişkeninin değeri 1")
	}
}
```
Eğer bir eşleşme bulunamazsa `default` kelimesini kullanarak bir blok oluştururuz. Switch in case e karşılık gelmediği durumlarda default bloğu çalışır. 
```go
package main

import "fmt"

func main() {
    v:=2
	switch v {
	case 0:
		fmt.Println("v değişkeninin değeri 0")
	case 1:
		fmt.Println("v değişkeninin değeri 1")
    default:
        fmt.Println("default bloğu çalıştı")
	}
}
```
Bu yapıda dikkat etmemiz gereken şey yalnızca aynı tipden değerleri karşılaştırabilmemizdir. Bu arada başka bir programlama dili tecrübeniz varsa `break` kullanmadığımızı fark etmişsinizdir. Go dilinde case den sonra `break`kullanımına ihtiyaç yoktur. Ama bazı yerlerde ihtiyacınız olabilir çıkmak için `break` kullanabilirsiniz

Case kısmında ifade kullanabiliriz, mesela değer hesaplayabiliriz.
```go
package main

import "fmt"

func main() {
    v:=1
	switch v {
	case 0:
		fmt.Println("v değişkeninin değeri 0")
	case 3-2:
		fmt.Println("v değişkeninin değeri 1")
   
}
```
Switch i boş bırakarak koşulsuz switchler oluşturabiliriz. Koşullarımızı case kısmında veriyoruz.

```go
package main

import "fmt"

func main() {
    v := 3
	
	switch  {
	case v<2:
		fmt.Println("v değişkeni 2den küçüktür")
	case v==3:
		fmt.Println("v değişkeni 3'e eşittir")
   
	}
}
```
Case ifadesinin birden çok değeri olabilir mesela v değişkenine rakam atayalım, bu rakamın tek mi çift mi olduğunu yazdıralım. (Çift veya tek sayı kontrolü için mantıklı bir yok değil öğrenme amaçlı verilmiş bir örnek)
```go
package main

import "fmt"

func main() {
    v := 2
	
	switch  v{
	case 0, 2, 4, 6, 8:
		fmt.Println("çift")
	case 1, 3, 5, 7, 9:
		fmt.Println("tek")
   
	}
}
```
`fallthrough` ifadesini kullanarak koşulun sağlandığı ilgili case bloğundan sonra switch case yapısından çıkmayı engelleyebilir sonraki case bloğunu çalışmaya alabilirsiniz. 
```go
package main

import "fmt"

func main() {
	n := 3
	switch n {
	case 0:
		fmt.Println("0")
		fallthrough
	case 1:
		fmt.Println("1")
		fallthrough
	case 2:
		fmt.Println("2")
		fallthrough
	case 3:
		fmt.Println("3")
		fallthrough
	case 4:
		fmt.Println("4")
		fallthrough
	case 5:
		fmt.Println("5")
		fallthrough
	case 6:
		fmt.Println("6")
		fallthrough
	case 7:
		fmt.Println("7")
		
	case 8:
		fmt.Println("8")
		fallthrough
	default:
		fmt.Println("default")
	}
}

```
Çıktımız
```go
    3
    4
    5
    6
    7
```
Case 7 de `fallthrough` kullanmadığımız için case 8 e geçmedi bu yüzden 8 ve default yazdırmadı. Switch case yapımızdan çıkmış olduğumuz için case 8 de `fallthrough` yazdırmanın bir anlamı yok.

