# Eşzamanlılık (Concurrency)

Eşzamanlılık sıklıkla paralellikle karıştırılır. Öncelikle bu iki kavramın tanımlarını yapalım 

> “Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once.”

Eşzamanlılık, aynı anda birden çok şeyle başa çıkmaktır. Paralellik ise aynı anda birden çok şey yapmaktır.

Bu kavramları daha iyi anlayabilmek için gerçek hayattan örnek verelim.

Diyelim ki koşuya çıktınız ve koşarken ayakkabınızın bağcıkları çözüldü. Koşmayı bırakıp hemen bağcıklarınızı bağladınız ve tekrar koşuya devam ettiniz. işte bu klasikleşmiş bir eşzamanlılık örneğiydi. 

Aynı örneğini paralellik için biraz değiştirelim. Kulağınızda kulaklık var ve en sevdiğiniz playlist çalıyor sizde bir yandan koşuyorsunuz işte bu durum bir paralellik örneğidir. Hem koşuyorsunuz hem müzik dinliyorsunuz aynı anda birden çok iş yapıyorsunuz.



## Go Dilinde Eşzamanlılık 

Go dilinde eşzamanlılık fonksiyonların veya metotların birbirlerinden bağımsız çalışmasıdır. Bu bağımsız çalışmayı sağlamak için fonksiyonları veya metotları goroutine olarak oluşturmalıyız. Goroutine oluşturmanın maliyeti, thread oluşturmaktan oldukça ucuzdur. Boyutları başlangıçta birkaç kb'dır ihtiyaç durumuna göre boyutları büyüyebilir, threadlerin boyutları daha büyük ve sabittirler. Bu duruma göre goroutineleri hafif thread (lightweight thread) olarak düşünülebiliriz (bazıları bu görüşe katılmamakta).

### Go Runtime 

Öncelikle goroutineler OS Thread değildirler. Go Runtime görevlerinden biri de kaç adet OS thread yaratılacağını ve goroutinelerin nasıl atanıp OS Threadlerde çalışacağını yönetmektir.

Go programımız başladığında sistemde tanımlı her sanal çekirdekler için bir mantıksal işlemci verilir. Hyper-Threading teknolojisine sahip bir donanımınız varsa her fiziksel çekirdek başına 2 sanal işlemci demektir.

## Goroutine Başlatmak

Bir goroutine başlatmak istiyorsak bunu bir fonksiyonun başına `go` kelimesi ekleyerek yaparız.

```go
package main

import (  
    "fmt"
)

func hello() {  
    fmt.Println("Goroutineden merhabalar")
}
func main() {  
    go hello()
    fmt.Println("main fonksiyonu")
}
```
Yukarıda bulunan örnek yeni bir goroutine başlatır. Main() kısmı main (ana) goroutine olarak isimlendirilir. Eğer kodu çalıştırdıysanız ekrana sadece `main fonksiyonu` yazdırdığını fark etmişsinizdir. Peki neden? Neden başlattığımız goroutine çalışmadı?

Bunun nedeni bir goroutine çağrısı yaptığımız zaman programımız, fonksiyon çağrıları gibi fonksiyonun görevini yerine getirmesini beklemez yani goroutinenin işini bitirmesini beklemez, main fonksiyonumuz çalışmaya devam eder ve çalıştıracak birşey bulamayan main() sonlanır. Main fonksiyonunun sonlanması demek programımızın sonlanması demektir. Bundan dolayı goroutineler çalışma şansı bulamamıştır.

Bu sorunu çözmek için WaitGroup sayma semaforunu kullanalım. WaitGroup değişkeninin değeri 0'dan büyük ise Wait() fonksiyonu bloklanır yani kod aşağı satıra geçmez. Add() fonksiyonu semaforun değerini 1 arttırır. Done() fonksiyonu semaforun değerini 1 azaltır. Eğer semafor 0 değerine ulaşırsa artık Wait() fonksiyonu bloklanmaz serbest bırakılır. Eğer semaforun değeri negatif olursa programda panic oluşur.
```go
package main

import (  
    "fmt"
    "sync"	
)
var wg sync.WaitGroup
func hello() { 
    defer wg.Done()
    fmt.Println("Goroutineden merhabalar")
}
func main() { 
   
    wg.Add(1) 	 	 
    go hello()
    wg.Wait()
    fmt.Println("main fonksiyonu")

}
```
Çıktı:
```go
Goroutineden merhabalar
main fonksiyonu
```
Done() fonksiyonunu defer yapmamızın nedeni hatırlarsanız defer ile nitelendirilmiş fonksiyonların en son çalışacağıdır. Yani Done() fonksiyon çağrısı yapılacağını garantiye alıyoruz. Fonksiyon içinde işlerimiz bitince Done() fonksiyonu çalışıyor ve semaforun değerini 1 azaltılıyor.

Go scheduler, bir goroutinenin mantıksal işlemcide uzun süre çalışıp işlemciyi rehin tutmasını engellemek için çalışan goroutineyi durdurup sıra bekleyen bir goroutineyi çalışmaya alabilir daha sonra durdurduğu goroutineyi tekrardan çalışmaya alabilir.

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"time"
)

var wg sync.WaitGroup


func main() {

	runtime.GOMAXPROCS(1)


	wg.Add(2)

	fmt.Println("Çalışma Başladı")
	go calis("A")
	go calis("B")



	wg.Wait()


}
func calis(isci string) {
	
	defer wg.Done()


	for i := 0; i < 5; i++ {
	  time.Sleep(250 * time.Millisecond)	
	  fmt.Printf("Çalışan işçi: %s\n",isci)
		
	}

}
```
Çıktı:
```go
Çalışma Başladı
Çalışan işçi: A
Çalışan işçi: B
Çalışan işçi: B
Çalışan işçi: A
Çalışan işçi: A
Çalışan işçi: B
Çalışan işçi: B
Çalışan işçi: A
Çalışan işçi: A
Çalışan işçi: B
```

## Yarış Durumu (Race Condition)

Senkronize edilmemiş goroutinelerin paylaşılan kaynakta okuma-yazma yaptığı zaman karşımıza gelebilecek bir yürütme sırası problemidir. Bir banka hesabını 2 kişinin kullandığını düşünelim. Bu hesabın bakiyesi 100TL olsun. Aynı anda 2 kişininde 100TL yatırdığı bir durumda atm'de gerçekleşmesi beklenilen komutlar:

| A Kişisi                     | B Kişisi                     |
|------------------------------|------------------------------|
| Anlık Bakiyeyi Getir (100TL) |                              |
| 100TL Yatır                  |                              |
| Yeni Bakiye (200TL)          |                              |
|                              | Anlık Bakiyeyi Getir (200TL) |
|                              | 100TL Yatır                  |
|                              | Yeni Bakiye (300TL)          |

fakat eş zamanlı çalıştıkları için runtime da hangisinin çalıştırılmaya alındığı hangisinin beklemeye alındığı hakkında kesin birşey söyleyemeyiz bu nedenle komutlar birbirine girebilir. Bu durumda karşılaşabileceğimiz senaryo:

| A Kişisi                     | B Kişisi                     |
|------------------------------|------------------------------|
| Anlık Bakiyeyi Getir (100TL) |                              |
|                              | Anlık Bakiyeyi Getir (100TL) |
| 100TL Yatır                  |                              |
|                              | 100TL Yatır                  |
| Yeni Bakiye (200TL)          |                              |
|                              | Yeni Bakiye (200TL)          |

Kodumuz hata vermeden sorunsuz çalışacaktır fakat bakiyemiz 300TL olması gerekirken 200TL oldu. Bu durum için A ve B yarış durumundadır diyebiliriz.
```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)
var (
	bakiye int = 100
	wg sync.WaitGroup


)
func main(){
	wg.Add(2)
	go paraYatir(100)
	go paraYatir(100)
	wg.Wait()
	fmt.Println("Bakiye: ",bakiye)

}
func paraYatir(miktar int){

	defer wg.Done()
	anlikBakiye:=bakiye
	runtime.Gosched() 
	anlikBakiye+=miktar
	bakiye=anlikBakiye
}
```
runtime.Gosched() fonksiyonu diğer goroutinelerin çalışmalarına fırsat verir. Eğer kullanmasaydık işlem basit olduğu için goroutineler hızlıca sırayla çalışacaktı bu durumda yarış durumunu göremeyebilirdik.  

Yarış durumunu engellemenin bir yolu paylaşılan kaynağa erişimi kilitleyerek senkronizasyonu sağlamaktır.

## Paylaşılan Kaynağı Kilitleme

sync paketinde bulunan atomik fonksiyonlar ve mutex ile paylaşılan kaynağı kilitleyebiliriz. Bu kilitleme ile goroutineler arası senkronizasyonu sağlarız.

### Atomik Fonksiyon

Tam sayılara ve işaretçilere erişirken donanım seviyesinde senkronizasyonu sağlar.
```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"sync/atomic"
)

var (
	counter int32          
	wg      sync.WaitGroup 
)

func main() {
	wg.Add(3)
	
	go increment("Python")
	go increment("Java")
	go increment("Golang")

	wg.Wait() 
	fmt.Println("Counter:", counter)

}

func increment(name string) {
	defer wg.Done() 
	for range name {
		atomic.AddInt32(&counter, 1)
		runtime.Gosched() 
	}
}
```


### Mutex

Paylaşılan kaynağa erişimi senkronize etmek için diğer bir yolumuz mutex kullanmaktır. Ortak olarak paylaşılan kaynakta değişiklik yapabilen kod bölümlerine kritik bölge (Critical Section) denir. Kritik bölgeye aynı anda birden fazla goroutine girmesi yarış durumuna sebep olabilir. Mutexler lock() ve unlock() ile kritik bölgelere eş zamanlı olarak birden fazla goroutine girişini engeller. Bir goroutine kritik bölgeye girdiğinde işini bitirene kadar diğer goroutineler bekler. Böylelikle senkronizasyon sağlanmış olur.

Para yatırma örneğimizdeki yarış sorununu mutex ile çözelim
```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)
var (
	bakiye int = 100
	wg sync.WaitGroup
	mutex sync.Mutex

)
func main(){
	wg.Add(2)
	go paraYatir(100)
	go paraYatir(100)

	wg.Wait()

	fmt.Println("Bakiye: ",bakiye)

}
func paraYatir(miktar int){

	defer wg.Done()
	mutex.Lock()
	anlikBakiye:=bakiye
	runtime.Gosched()
	anlikBakiye+=miktar
	bakiye=anlikBakiye
	mutex.Unlock()

}
```
Çıktı:
```go
Bakiye: 300
```
Mutex kullanarak sorun çözüldü.



## Channel 

Çoğu programlama dilinde iletişim için paylaşılan bellek modeli kullanılır. Paylaşılan bellek modelinde senkronizasyonu sağlayabilmek için veriler kilitlerle korunur. Go dilinde ise CSP (Communicating Sequential Processes) denilen yaklaşım tercih edilir. Bu yaklaşım da veriye erişimi senkronize etmek için kilit yerine kanallar kullanılır. Goroutineler ve channellar CSP yaklaşımının bir uygulamasıdır.

Go eşzamanlılık sloganı:

"Belleği paylaşarak iletişim kurmayın onun yerine iletişim kurarak belleği paylaşın"

Buradan anlaşılacağı üzere Go channel kullanımını önerir.

Channellar, goroutineler arası iletişim ve senkronizasyonu sağlamak için kanal görevi üstlenen yapılardır. Bir uçtan diğer uca veri gönderilebilir veya alınabilir.

Bir channel tanımlarken paylaşılacak verinin tipi belirtilmelidir. Belirtilen tip dışındaki veriler taşınmaz.

Channel unbuffered ve buffered olmak üzere iki çeşittir.

### Channel Tanımlama

Channel, make() fonksiyonu ile tanımlanır. make() fonksiyonunun içinde chan kelimesi sonrası channel içinde veri transferinde kullanılacak tip belirtilir. Eğer buffered bir channel oluşturmak isteniliyorsa ikinci parametre olarak buffer boyutu verilmelidir.
```go
unbuffered := make(chan int)
buffered := make(chan string, 10)
```
Kanala veri göndermek için <- operatörü kullanılır.
```go
kanal1 := make(chan int)
kanal1 <- 2
```
Okun yönü kanal1'i işaret ettiği için 2 değerini kanal1'e gönderiyoruz.

Bir kanaldan veri okumak için aynı operatörü kullanıyoruz fakat bu sefer önüne := operatörünü de koyuyoruz.
```go
gelen := <- kanal1
```

### Unbuffered Channel

Unbuffered channel, tutulmadan önce herhangi bir değeri saklama kapasitesi olmayan bir kanaldır. Verinin hem gönderilme hemde alınması için iki goroutinenin aynı anda hazır olması gerekir. Eğer aynı anda hazır olmazlarsa kanal goroutineyi bekler. Böylelikle senkronizasyon sağlanır. 

![Alt text](https://www.ardanlabs.com/images/goinggo/Screen+Shot+2014-02-16+at+10.10.54+AM.png "baslik")
 
Şekil iki goroutine arasında bir değeri kanal kullanarak paylaşmayı temsil etmekte. 1 numaralı durumda goroutineler kanala ellerini sokmadıkları için herhangi bir işlem gerçekleşmez. 2.durumda solda bulunan goroutine (elindeki yeşil çubuK transfer edilecek veri) kanala elini sokar bu durum bir gönderme işlemini yapmak istediğini temsil eder. İşlem tamamlanana kadar kilitlenir. 3.durumda sağda bulunan goroutine de kanala elini sokarak kanaldan bir veri alma işlemini temsil eder. Bu goroutinede kanalda transfer işlemi bitene kadar kilitli kalır. Transfer işlemleri 4 ve 5. adımdan sonra bitmiş olur. Artık kilitler kalkar 6.adımda her iki goroutine de ellerini kanaldan çıkartmıştır.

Channel ile toplama örneği yapalım.
```go
package main

import (
	"fmt"
)

func toplamaYap(sayi1,sayi2 int,kanal chan int) {
	fmt.Println("toplamaYap goroutine")
	kanal <- sayi1+sayi2
}
func main() {
	fmt.Println("main goroutine")
	kanal := make(chan int)
	go toplamaYap(2,3,kanal)
	sonuc:=<-kanal
	fmt.Println("Sonuc:",sonuc)
}
```
Çıktı:
```go
main goroutine
toplamaYap goroutine
Sonuc: 5
```
Daha önceki örneklerimizde goroutinelerin çalışmasına fırsat vermek için WaitGroup semaforunu kullanıyorduk fakat dikkat ettiyseniz yukarıdaki örnekte WaitGroup semaforuna ihtiyaç duymadık. Bunun nedeni `sonuc:=<-kanal` satırının olduğu yerde kanal isimli channeldan bir değer alıyoruz ve bu değer channeldan gelene kadar main goroutine bloklanıyor. Bundan dolayı program bir alt satıra geçemiyor böylelikle main goroutine tamamlanıp diğer goroutinelerin çalışmaması gibi bir durumu ortaya çıkmıyor. toplamaYap goroutine çalışıp channel'a değeri yolladığı anda bloklama durumu ortadan kalkar ve sonucu ekrana yazdırdıktan sonra program sonlanır.

#### Deadlock (Ölümcül Kilitlenme)

Bir goroutine channeldan bir veri almak istiyorsa diğer goroutinenin veriyi göndermesi yada bu durumun tam tersi sağlanması gerekir. Eğer bu sağlanmaz ise deadlock ortaya çıkar ve program panic verir.

Mesela bir channel oluşturup ondan veri almayı bekleyelim.
```go
package main


func main() {  
    kanal := make(chan string)
    <-kanal
}
```
Çıktı
```go
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.main()
	/tmp/sandbox173286568/prog.go:6 +0x4d
```
#### Tek Yönlü Channel
Sadece veri göndermek veya sadece veri almak için tek yönlü channel oluşturabiliriz. 

**Sadece Veri Almak İçin**
kanal1:= make(<- chan bool)

**Sadece Veri Göndermek İçin**
kanal2:= make(chan<-bool

Tek yönlü channellar sayesinde çift yönlü channelları tek yönlü bir kanala dönüştürebiliriz. Tek yönlüyü çift yönlüye dönüştürmek mümkün değildir.

```go
package main

import "fmt"

func gonder(kanal chan<- string) {
	kanal <- "veri gönderildi"
	
}

func main() {
	kanal := make(chan string)
	go gonder(kanal)
	fmt.Println(<-kanal)
}
```
Çıktı:
```go
veri gönderildi
```
#### Close Channel

Channellar genellikle gönderilecek veri kalmayınca alıcıya artık veri gönderilmeyeceğini bildirmek için close() fonksiyonu ile kapatılır. 

```go
close(kanal)
```
Kapalı bir channela veri göndermek panic oluşmasına neden olur. Fakat kapalı channeldan veri almaya çalışmak panic durumuna neden olmaz eğer kanalda veri yoksa veri tipinin varsayılan (sıfır) değerini döndürülür.
```go
package main

import "fmt"


func main() {
	kanal := make(chan bool,1)
	close(kanal)
	fmt.Println(<-kanal)

}
```
Çıktı:
```go
false
```
Kanaldan veri alımı yaparken channel ın kapanıp kapanmadığını kontrol etmek için ek bir değişken kullanabiliriz.
```go
alınan, ok := <- ch  
```
Yukarıdaki kullanımda ok değişkeni, kanaldan başarılı bir alım gerçekleşmişse true olur. Eğer ok değeri false ise kapalı bir kanaldan veri almaya çalıştığımızı belirtir.

Ok değişkeninden dönen değere göre kanaldan veri aldığımız bir örnek yapalım.
```go
package main

import (
	"fmt"
)

func kanalaGonder(knl chan int) {
	for i := 0; i < 10; i++ {
		knl <- i
	}
	close(knl)
}
func main() {
	kanal := make(chan int)
	go kanalaGonder(kanal)
	for {
		alınan, ok := <-kanal
		if !ok {
			fmt.Println("Channel Kapalı")
			break

		}
		fmt.Println("Kanaldan alınan:", alınan)
	}
}
```
Çıktı
```go
Kanaldan alınan: 0
Kanaldan alınan: 1
Kanaldan alınan: 2
Kanaldan alınan: 3
Kanaldan alınan: 4
Kanaldan alınan: 5
Kanaldan alınan: 6
Kanaldan alınan: 7
Kanaldan alınan: 8
Kanaldan alınan: 9
Channel Kapalı
```
### Buffered Channel

