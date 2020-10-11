# Metotlar 
Metotlar kullanıcı tanımlı tiplere davranış (behavior) özelliği kazandırır.
Kabaca struct fonksiyonlarına metot denir diyelim. Daha sonra fonksiyonla farklarına bakalım.

Öncelikle fonksiyon tanımlamayı hatırlayalım:

```go
func fonksiyonAdı(parametreAdı parametreTipi) donusTipi{
//fonksiyon gövdesi
}
```
Şimdi metot nasıl tanımlanır ona bakalım
```go
func (alıcı alıcıTipi) MetotAdı(parametreAdı parametreTipi) donusTipi
```
Dikkat ettiyseniz metot tanımında MetotAdı kısmından önce alıcı ve alıcıTipi adlı argümanı var. Bundan dolayı metotları açıklarken şu tanım daha doğru olacaktır; Bir metot, tanımlanmış alıcısı olan bir fonksiyondur.

Metot kullanımına bir örnek:
```go
package main

import "fmt"

type ogrenci struct {
	ad string
	soyad string
	ogrenciNo int
}
func (o ogrenci) bilgileriGetir(){
	fmt.Println("\n Ad: ",o.ad,"\n Soyad: ",o.soyad,"\n Ogrenci No: ",o.ogrenciNo)
}
func (o *ogrenci) kayitEt(ad string ,soyad string,numara int){
	o.ad=ad
	o.soyad=soyad
	o.ogrenciNo=numara
}
func main() {

	var ogrenci1 ogrenci
	ogrenci1.kayitEt("ahmet","temha",342)
	ogrenci1.bilgileriGetir()

}
```
Çıktı:
```go
 Ad:  ahmet 
 Soyad:  temha 
 Ogrenci No:  342
```
Anlatmak gerekirse 2 adet metodumuz var. Bunlar bilgileriGetir() ve kayitEt()

main() fonksiyonu içinde ogrenci1 isminde ogrenci tipinde bir değişiken oluşturuyoruz. KayıtEt() metodunun alıcı tipi bir değer. Bu metot parametresine geçirilen değişkenin değerini kopyalaması ve onun üzerinden işlem yapması demektir. Yani esas değişkenimiz üzerinden işlem yapmıyoruz onun bir kopyası üzerinden işlem yapıyoruz. bilgileriGetir() metotunda ise alıcı pointer tipinde olduğu için parametreye değişkenimizin adresi geçiriliyor ve direkt olarak kendisi üzerinden işlem yapıyoruz. Kafanız karıştıysa pointer başlığı altında bulunan Pass by Value ve Pass by Pointer konularına göz atabilirsiniz.


Metotların fonksiyonlardan bir diğer farkı ise  aynı isimli metotlar farklı tiplerde tanımlanabilirken aynı isime sahip fonksiyonlar olamaz.
```go
package main

import "fmt"

type ogretmen struct{
	ad string
	soyad string
}

type ogrenci struct {
	ad string
	soyad string
	ogrenciNo int
}
func (o ogrenci) bilgileriGetir(){
	fmt.Println("\n Ad: ",o.ad,"\n Soyad: ",o.soyad,"\n Ogrenci No: ",o.ogrenciNo)
}
func (o ogretmen) bilgileriGetir(){
	fmt.Println("\n Ad: ",o.ad,"\n Soyad: ",o.soyad)
}

func main() {

	ogrenci1:=ogrenci{
		ad: "Ahmet",
		soyad:    "Temha",
		ogrenciNo: 342,
	}
	ogretmen1:=ogretmen{
	        ad: "Ayşe",
		soyad: "Atmaca",
	}
	
	ogrenci1.bilgileriGetir()
	ogretmen1.bilgileriGetir()

}
```
Çıktı:
```go
 Ad:  Ahmet 
 Soyad:  Temha 
 Ogrenci No:  342

 Ad:  Ayşe 
 Soyad:  Atmaca
```

### Alıcısı Struct Olmayan Metotlar
Metot alıcısı struct olmak zorunda değildir, type ile oluşturduğumuz tiplerle metot kullanabiliriz.
```go
package main

import "fmt"

type tamsayi int

func (t tamsayi) topla(t2 tamsayi) tamsayi {
	return t + t2
}

func main() {
	sayi1 := tamsayi(5)
	sayi2 := tamsayi(10)
	toplam := sayi1.topla(sayi2)
	fmt.Println("Toplam", toplam)
}
```
