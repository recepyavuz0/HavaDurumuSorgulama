# HavaDurumuSorgulama

Kullanıcı hava durumunu sorgulamak istediğinde , ülke ve şehir bilgisi girerek ilgili şehirin 5 günlük hava durumunu bilgisini veren bir uygulamadır.

Axios frameworku kullanılmıştır.

## Uygulama Nasıl Kullanılır ?
### 1.Adım <br>
![1. Input](https://github.com/caferyavuz/HavaDurumuSorgulama/blob/master/img/i1.PNG) <br/>
`var inputCountry=prompt("Lütfen ülke veya ülke kodu giriniz.");` komutu ile kullanıcıdan ülke bilgisi alıyoruz.<br/>
Örnek : tr, Tr, türkiye, ru, rusya, us, amerika <br/>
### 2.Adım <br>
![2. Input](https://github.com/caferyavuz/HavaDurumuSorgulama/blob/master/img/i2.PNG) <br/>
`var inputCity=prompt("Lütfen şehir giriniz.");` komutu ile kullanıcıdan şehir bilgisini alıyoruz. <br/>
Örnek : istanbul, İstanbul, los angeles, chicago <br/>

### Sonuç <br>
Ülke ve şehir bilgisini aldıktan sonra ilgili hava durumu bilgileri kullanıcıya gösterilir. <br>
![Hava Durumu](https://github.com/caferyavuz/HavaDurumuSorgulama/blob/master/img/weather.PNG) <br/>


## Axios Kurulumu Nasıl Yapılır ?

### 1.Adım <br>
Axios kütüphanemizi kullanabilmek için `<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>` komutunu **html** dosyamızın **head** etiketinin içine ekliyoruz. <br>
### 2.Adım <br>
API ile gelen verileri göstermek için html css ile istediğimiz tasarımı oluşturuyoruz.
### 3.Adım <br>
projemize javascript dosyası oluşturuyoruz ve içine <br>
```
const url="{api_url/user?ID=11111}";

axios.get(url)
  .then(function (response) {
    //Başarılı işlem sırasında çalışcak kod bloğu
    console.log(response);
  })
  .catch(function (error) {
    //Hatalı işlem sırasında çalışacak kod bloğu
    console.log(error);
  })
  .finally(function () {
   //her zaman çalışacak kod bloğu
  });
```
şeklinde veya
```
const url="{api_url}";
axios.get('/user', {
    params: {
      ID: 11111
    }
  })
.then(function (response) {
    //Başarılı işlem sırasında çalışcak kod bloğu
    console.log(response);
  })
  .catch(function (error) {
    //Hatalı işlem sırasında çalışacak kod bloğu
    console.log(error);
  })
  .finally(function () {
   //her zaman çalışacak kod bloğu
  });
```
kod bloğu ile axios get isteği yollamak için iskeleti oluşturuyoruz. <br>
### 4.Adım <br>
```
var inputCountry=prompt("Lütfen ülke veya ülke kodu giriniz.");
var inputCity=prompt("Lütfen şehir giriniz.");
```
* Kullanıcıdan sorgulamak istenilen ülke ve şehir bilgilerini almak için **prompt** kodu ile veri girişi için pencere oluşturup girilen veriyi değişkenlere atıyoruz.
### 5.Adım <br>
```
var months=["Ocak","Şubat","Mart","Nisan","Mayıs","Haziran","Temmuz","Ağustos","Eylül","Ekim","Kasım","Aralık"];
var date=new Date();
```
projede kullancağımız aylar dizisini ve Date tipinde bir global nesne tanımlıyoruz.
### 6.Adım <br>
```
function TodayInfo(response){
      let city=response.data.city.name;
      let country=response.data.city.country;
      let celsius =response.data.list[0].main.temp;
      let weather=response.data.list[0].weather[0].description;
      let requestDate=date.getDate()+" "+months[(Number((String(response.data.list[0].dt_txt)).slice(5,7)))-1];

      document.getElementById("city").innerText=city+" , "+country;
      document.getElementById("celsius").innerText=celsius.toFixed(0)+"°C";
      document.getElementById("date").innerText=requestDate;
      document.getElementById("result").innerText=Capitalize(weather);

      console.log(`BAŞARILI >>> Şehir : ${city}   Ülke : ${country}   Derece : ${celsius}°C   Hava Durumu : ${weather}   Sorgulama Tarihi : ${requestDate+" "+date.getFullYear()}`);
}

//Sorgulama yaptığınız günün bir gün sonrasının hava durumu verisini gösterir
function OneDayLater(response){
      let weather=response.data.list[8].weather[0].description;
      let celsius =response.data.list[8].main.temp;
      let requestDate =(String(response.data.list[8].dt_txt)).slice(8,10)+" "+months[(Number((String(response.data.list[8].dt_txt)).slice(5,7)))-1];
      
      document.getElementById("one-text").innerText=celsius.toFixed(0)+"°C";
      document.getElementById("one-date").innerText=requestDate;
      document.getElementById("one-weather").innerText=Capitalize(weather);

}

//Sorgulama yaptığınız günün iki gün sonrasının hava durumu verisini gösterir
function TwoDayLater(response){
      let weather=response.data.list[16].weather[0].description;
      let celsius =response.data.list[16].main.temp;
      let requestDate =(String(response.data.list[16].dt_txt)).slice(8,10)+" "+months[(Number((String(response.data.list[16].dt_txt)).slice(5,7)))-1];
      
      document.getElementById("two-text").innerText=celsius.toFixed(0)+"°C";
      document.getElementById("two-date").innerText=requestDate;
      document.getElementById("two-weather").innerText=Capitalize(weather);
}

//Sorgulama yaptığınız günün üç gün sonrasının hava durumu verisini gösterir
function ThreeDayLater(response){
      let weather=response.data.list[24].weather[0].description;
      let celsius =response.data.list[24].main.temp;
      let requestDate =(String(response.data.list[24].dt_txt)).slice(8,10)+" "+months[(Number((String(response.data.list[24].dt_txt)).slice(5,7)))-1];

      document.getElementById("three-weather").innerText=Capitalize(weather);
      document.getElementById("three-text").innerText=celsius.toFixed(0)+"°C";
      document.getElementById("three-date").innerText=requestDate;
}

//Sorgulama yaptığınız günün dört gün sonrasının hava durumu verisini gösterir
function FourDayLater(response){
      let celsius =response.data.list[32].main.temp;
      let requestDate =(String(response.data.list[32].dt_txt)).slice(8,10)+" "+months[(Number((String(response.data.list[32].dt_txt)).slice(5,7)))-1];
      let weather=response.data.list[32].weather[0].description;
      document.getElementById("four-weather").innerText=Capitalize(weather);
      document.getElementById("four-text").innerText=celsius.toFixed(0)+"°C";
      document.getElementById("four-date").innerText=requestDate;

}
```
* Sorguladığımız şehrin 5 günlük hava durumu verilerini çekmek için gerekli fonksiyonları oluşturuyoruz.
* API ile gelen json verileri değişkenlere çekerek html yapımıza yazdırıyoruz.
* **document.getElementById().innerText** ile değişkenlerimizi ilgili id değerine sahip html etiketine yazdırıyoruz. 
### 7.Adım <br>
```
//Gönderilen hava durumunun kelimelerinin baş harflerini büyük harfe çeviren fonksiyon
function Capitalize(weather){
  let weathers=weather.split(" ");
  let text="";
  for (let i = 0; i < weathers.length; i++) {
   weathers[i]=weathers[i][0].toUpperCase()+weathers[i].slice(1).toLowerCase();
  text=text+" "+weathers[i];
  }
  return text;
}
```
* Gönderilen metnin baş harflerini büyük harfe çeviren fonksiyonu script dosyamıza ekliyoruz. 
* Bu fonksiyonu yazmamızın nedeni API ile gelen veri küçük harflerle geldiği için bu fonksiyon ile büyük harfe çevirip ekrana yazdırıyoruz.

### 7.Adım <br>
```
const url=`http://api.openweathermap.org/data/2.5/forecast?q=${inputCity},${inputCountry}&appid=69f27776fcc2ef53fa6dea89dd4a4aeb&lang=TR&units=metric`;

axios.get(url)
  .then(function (response) {
      TodayInfo(response);
      OneDayLater(response);
      TwoDayLater(response);
      ThreeDayLater(response);
      FourDayLater(response);
  })
  .catch(function (error) {
    if (error.response) {
      let message =error.response.data.message;
      let status=error.response.status;
      console.log(`HATA >>> HTTP Kodu : ${status}  Hata Mesajı : ${message} `);
    }else{
      console.log(`HATA >>> Hata Mesajı : ${error} `);
    }
  })
```
* Axios framework iskeletinin **then** bloğunu(başarılı olması durumu) oluşturduğumuz fonksiyonları çağırarak dolduruyoruz.
* Axios framework iskeletinin **catch** bloğunu(hatalı olması durumu) hata şartlarımız ile dolduruyoruz.


## Javacsript Kodlarının Açıklamaları

1. `const url="api_adresi"` <br>
**const** sabit bir değişken tanımlıyoruz ve istek göndereceğimiz apimizin adresini belirliyoruz.

2. `axios.get(url)` <br>
**axios.get()** ile belirlediğimiz url adresine veri geçmek için get isteğinde bulunuruz. Gelen veri json tipinde gelmektedir.

3. `var inputCountry=prompt("Lütfen ülke veya ülke kodu giriniz.")` <br>
**prompt** kullanıcıdan veri almak için açılan pencere için kullanılır.**var** ile global bir değişken oluşturuyoruz.

4. `var months=["Ocak","Şubat","Mart","Nisan","Mayıs","Haziran","Temmuz","Ağustos","Eylül","Ekim","Kasım","Aralık"]`<br>
**[]** işareti ile string bir dizi tanımlıyoruz.

5. `var date=new Date();` <br>
**new Date()** ile Date tipinde bir nesne oluşturuyoruz.

6. `date.getDate()` <br>
**getDate()** ile sistem tarihinin kaçıncı günde olduğunun bilgisine çeker.

7. `celsius.toFixed(0)` <br>
**toFixed()** ile virgülden sonra kaç basamak gösterileceğini belirtir.

8. `(String(response.data.list[0].dt_txt)).slice(5,7)))` <br>
**(String(değer))** Veri dönüşümü yapmak için kullanılır.
**slice(baslangic,bitis)** ile string tipindeki verinin içinden belirli alanı ayırıp kullanmaya yarar. Belirtilen baslangic indexten başlayarak bitis indexine kadar seçer.(baslangic indexi dahil, bitis indexi dahil değil)

9. `let weathers=weather.split(" ")` <br>
**split()** ile metinimizi belirtilen işaretten ayırarak bir diziye aktarır. **let** ile local bir değişken oluştururuz. Tanımlanan parantezler içerisinde kullanılır. 


10. `toUpperCase() , toLowerCase()` <br>
**toUpperCase() ve toLowerCase()** ile metnin tümünü büyük harfe veya küçük harfe çevirir.
