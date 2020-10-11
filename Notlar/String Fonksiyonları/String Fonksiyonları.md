# String Fonksiyonları

Yaygın olarak kullanılan bazı string fonksiyonlarına göz atalım.

## Compare

Compare fonksiyonu girdi olarak 2 adet string değeri alıp ASCII tablosundaki değerlerine göre karşılaştırma yapmaktadır. İki parametremiz eşitse 0, birinci parametre ikinci parametreden büyükse 1 diğer durumda -1 değerini döndürür.

compare.go dosyasında aşağıdaki gibi tanımlanmıştır
```go
func Compare(a, b string) int {
	if a == b {
		return 0
	}
	if a < b {
		return -1
	}
	return +1
}
```
Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Compare("A", "a"))  // A=65, a=97 => A < a
	fmt.Println(strings.Compare("Istanbul","Isparta")) // t=116, p=112 => t > p
	fmt.Println(strings.Compare("Go","Go")) 
}
```
Çıktı:
```go
-1
1
0
```

## Contains

Bir string değerinin başka bir string değerini içerip içermediğini öğrenmek için kullanılır. Dönüş tipi bool olan Contains fonksiyonunun ikinci parametresi birinci parametrede varsa true yoksa false değerini döndürür.

söz dizimi:
```go
func Contains(s, substr string) bool
```
Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Contains("Go Dili","Dil"))
	fmt.Println(strings.Contains("Go Dili","dili"))
	fmt.Println(strings.Contains("Go Dili","Dilim"))

}
```
Çıktı:
```go
true
false
false
```

## Count

String değeri içerisinde istenilen string değerinin kaç defa geçtiğini int tipinde döndürür. Birinci parametre aranılacak olan string ikinci parametre ise aranan string değeridir.

söz dizimi:
```go
func Count(s, substr string) int 
```
Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Count("Google","o"))
}
```
Çıktı:
```go
2
```

## Fields

String içindeki boşlukları baz alarak stringi ayırır. Ayırdığı string değerlerini bir diziye eleman olarak atar, dönüş tipi string dizisidir.

Söz dizimi:
```go
func Fields(s string) []string {
```
Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	ornekString := "Bu bizim boşluk içeren stringimiz olsun"
	dizi := strings.Fields(ornekString)
	for _, elemanlar := range dizi {
		fmt.Println(elemanlar)
	}
}
```
Çıktı:
```go
Bu
bizim
boşluk
içeren
stringimiz
olsun
```

## Index

String değeri içinde aranan değerinin kaçıncı sırada olduğunu int değeri olarak döner. Stringler aslında bir dizi olduğu için indexin 0'dan başladığını unutmayalım. Aranan değer string içinde yoksa -1 değerini döndürür. 

Söz dizimi:
```go
func Index(s, substr string) int
```
Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Index("GOPHER","P"))
}
```
Çıktı:
```go
2
```

## Replace

String değerinin karakterlerini istenilen karakterlerle değiştip yeni string değerini dönderir.

Söz dizimi:
```go
func Replace(s, old, new string, n int) string
```
Yukarıda s string değeri, old değiştirilmesi istenilen kısım, new değişmek istinilen değer, n değiştirilecek karakter sayısı. 

Eğer n değeri 0 dan küçükse değişikliğin sınırı yoktur.

Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Replace(" Evim evim güzel evim", "im", "ler", 0))
	fmt.Println(strings.Replace("Evim evim güzel evim", "im", "ler", 1))
	fmt.Println(strings.Replace("Evim evim güzel evim", "im", "ler", 2))
	fmt.Println(strings.Replace("Evim evim güzel evim", "im", "ler", -1))
}
```
Çıktı:
```go
Evim evim güzel evim
Evler evim güzel evim
Evler evler güzel evim
Evler evler güzel evler
```

## Split

String değerini istenilen değere göre dilime böler.

Söz dizimi:
```go
func Split(S string, sep string) []string
```

Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	strDilim := strings.Split("a,b,c", ",")
	fmt.Println(strDilim,"\n")
}
```
Çıktı:
```go
[a b c]
```

## Title

String içindeki değerlerin baş harflerini büyük harfe dönüştürür.

Söz dizimi:
```go
func Title(s string) string
```

Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Title("merhaba dünya"))
}
```
Çıktı:
```go
Merhaba Dünya
```

## ToUpper

String içindeki bütün harfleri büyük harf yapar

Söz dizimi:
```go
func ToUpper(s string) string
```

Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToUpper("merhaba dünya"))
}
```
Çıktı:
```go
MERHABA DÜNYA
```

## ToLower

String içindeki bütün harfleri küçük harf yapar.

Söz dizimi:
```go
func ToUpper(s string) string
```

Örnek kullanım:
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToUpper("MERHABA DÜNYA"))
}
```
Çıktı:
```go
merhaba dünya
```