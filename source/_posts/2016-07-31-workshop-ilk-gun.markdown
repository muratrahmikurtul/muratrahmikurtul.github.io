---
layout: post
title: "Workshop Serüveni İlk Gün"
date: 2016-07-31 13:45:11 +0300
comments: true
categories: 
---
## Öğleden önce

Sabah uyandığımda heyecanlıydım. Ruby on Rails konulu workshop için sabırsızlanıyordum. Kahvaltı bile yapmadan duş alıp direk workshop a gittim. Eğitimi en sağlıklı nerden takip edebilirim diyerek bir tahminde bulundum ve yerimi aldım. Öğleden önce bize değerli eğitmenimiz [Tayfun Bey](https://twitter.com/toziserikan?lang=tr) yardımcı olacaktı , geldi ufak bir konuşmanın ardından workshop a start verdi.Temel giriş bilgileriyle ısınma turları atarken not tutmadığımı farkeden eğitmenimin tepkisiyle hemen kalkıp kağıtlarımı alıp not tutmaya başladım. Burdan da kendisine bu vesileyle özürlerimi iletiyorum.İlk gün işlenen konular arasında **semantic versiyonlama,tasarım şablonları,mvc yapısı,activerecord associations,polimorfizm,web serverlar(server,domain,dns gibi kavramlar),restful mantığı** vardı.

##### Semantic Versiyonlama

X.Y.Z | p şeklinde gösterilen versiyonlama da **X** major , **Y** minor , **Z** patch , **P** ise opsiyonel i ifade edermiş. Ruby on Rails ile uğraşanlar 'gem'leri bilirler. Gemlerin versiyonları vardır ve gemfile da örneğin ; gem 'haml', '~> 4.0' , gibi ifadeler bulunur. Semantic versiyonlama konusundan sonra gemin yanındaki rakamlar anlamlı hale gelmişti kafamda.Fakat halen kafamda birkaç soru vardı. Bu rakamlar neye göre değişiyordu? Tam ben bunları düşünürken eğitmenimizden cevap gecikmedi. **Z** bir hata düzeltimi sonrası , **Y** yeni bir özellik ekleme sonrası , **X** ise köklü bir yenilik sonrası değişirmiş. Kafamdaki soru işaretinin giderilemisin mutluluğunu yaşarken Tayfun Beyden bir soru geldi. Versiyondan önce gelen '~>' bu işaretin anlamı neydi? Kimse bilmiyordu. Böylece günün ilk ödevini almış olduk.Bu arada ilk yazımın hatrına cevabı verip sizi araştırma derdinden kurtarayım :). Örneğin '~> 2.0.3' demek '>= 2.0.3 ve <2.1' le aynı demektir. Semantic versiyonlama ile ilgili daha detaylı bilgilere [Lab2023 Playbook](https://github.com/lab2023/playbook/blob/develop/development/cevik_proje_yonetimi.md) dan ulaşabilirsiniz.
  
##### MVC Yapısı

MVC yapısını isminin baş harflerinden de anlaşılacağı üzere **M**odel , **V**iew , ve **C**ontroller oluşturur.

 * **Model** ; Rails'te anladığım kadarıyla modeller veritabanı ilişkileri için kullanılır. Genellikle veritabanınızdaki bir tablo için bir model oluşturursunuz. Örneğin kitap modeli veritabanınızdaki kitap tablosuyla ilişkilidir. Tablo üzerinde CREATE DELETE UPDATE gibi işlemleri kitap modelinizle yaparsınız. Örneğin Kitap tablosunda bir arama sorgu kodu örneği verecek olursak ;
 
```sh
 
    Book.where(:name => 'Ruby on Rails').first
        
```
 
 * **Controller** ; MVC yapısının kalbidir. İstek ve cevapları yönetir. İstekleri alır işlemden geçirir ve View a sonuç gönderir. İşlemlerin kaydı için verileri Model kısmına gene Controller yollar.


```sh
       class ExampleController < ApplicationController
        
         def index
           # logic to list all recipes
         end
        
         def show
           # logic to show a particular recipe
         end
        
         def create
           # logic to create a new recipe
         end
        
         def update
           # logic to update a particular recipe
         end
       end
 ```
 * **View** ; yaptığınız uygulamanın kullanıcıya görünen kısmı viewlardır. View da database tarafıyla yapacağınız bir etkileşim yani model ile kuracağınız ilişki mutlaka controller üzerinden olmalıdır çünkü MVC yapısının gereği zaten budur. View da kullanıcıyla iletişime buttonlar,formlar vb. ile kullanıcıyla iletişime geçilir. View ve Controller ı  ilişkilendirmek için bir de **Helper** lar vardır fakat buna ikinci günü anlatırken değineceğim.

 
##### Polimorfizm (Active Record Associations)

Ruby on Rails da polimorfizm önemli bir konudur. Örneğin bir yazarın birden fazla kitabı bir kitabın ise bir tane yazarı olabilir.Bir kategoride birden fazla kitap bir kitabın ise bir kategorisi olur gibi ilişkiler vardır. İşte bu ilişkileri kurmak için kullandığımız anahtar kelimeler şunlardır ;

* belongs_to
* has_one
* has_many
* has_many :through
* has_one :through
* has_and_belongs_to_many

![Örnek](http://guides.rubyonrails.org/images/habtm.png)

Daha fazla bilgi için [Guide Ruby on Rails](http://guides.rubyonrails.org/association_basics.html)


##### Web Server Tarafı

Dünyada en çok bilinen 3 web server ı sıralayacak olursak ;

* 3) iis
* 2) Apache
* 1) Nginx

İnternet üzerinde iletişim DNS denen "Domain Name Server" lar tarafından yapılır. Basitçe bu dns lerin görevi ismi ip ye ip yi isme çevirmektir denebilir. ns1.blablabla.net ns2.blablabla.net şeklindedirler. **Domain** dediğimiz şey alan adıdır. Örneğin "www.rubyonrails.org.tr" adresinde **www** kısmı sub domain , **rubyonrails** kısmı body , **org** kısmı uzantı , **tr** kısmı ise country yani ülke kodu kısmıdır. **rubyonrails.org.tr** kısmının tamamına ise root denir. Temel olarak kullanıcı sunucuya istek yapar ve sunucudan da kullanıcıya bir dönüş olur. İşte bu dönüşlerin http status code ları vardır yani biraz açacak olursak http default port : 80 dir ssh ise 22 dir. Eğer http status code u 200 dönerse siteye giriş yapabilirsiniz. Eğer 40X kodu dönerde "bulunamadı" hatası alırsınız. Herkesin bileceği üzere 404:Not Found :). Fakat 50X gibi bir status code dönerse şu anlama gelmektedir sayfa bulundu fakat sunucu kapalı.Burda değinilmesi gereken bir diğer konu ise virtual hosting sayesinde bir ip den çok sayfaya ulaşılabilindiğidir.

Basitçe çizecek olursak Kullanıcı -> Web Server(Nginx) -> App server(Rack) : puma,uniqorn,webrick -> App(Controller,View,Model)

* App Server ; App serverın görevi request gelen dataları objeye yani ruby classına çevirmektir.

İstek tipleri POST,GET,PATCH,PUT,DELETE , dir. POST u CREATE e , GET i  SHOW/INDEX e , PATCH/PUT u UPDATE e , DELETE i DESTROY a çevirir. Yani şöyle bir mantık yürütebiliriz. url+metod = create , url+id+metod = update gibi.

İşte bu yukarıda yazan mimariye göre kodlama yapmaya **restful** denir.

Son olarak url nin root/resource/action/params kısımlarından oluştuğuna ve bu kısımların her birine **segment** dendiğine değinip [Tayfun Bey](https://twitter.com/toziserikan?lang=tr) sözlerine son vererek eğitimin öğleden önceki kısmını bitirdi.Kendisine burdan bu güzel eğitim için tekrar teşekkürlerimi sunuyorum.



### Öğleden Sonra

Sabahki güzel eğitimin ardından öğle arası sabah kahvaltı yapmamış olmanın verdiği açlık hissiyle bayağı bir yemek yedim :) ve geri döndüm. Öğleden sonra bize yardımcı olacak olan eğitmenimiz [İsmail Bey](https://twitter.com/isoakbudak?lang=tr)di. Kendisiyle birlikte Ruby on Rails de kendisi ve [şirketinin](https://www.lab2023.com) oluşturduğu bir tool olan [Cybele](https://github.com/lab2023/cybele) ile basit bir blog geliştirecektik. Öncelikle gerekli kurulumlarımızı yaparak projemizi oluşturduk.İsmail Bey ilk işimizin -daha yukarılarda kısaca değindiğim- 'gem'leri tanımamız olduğunu söyledi.Herkes gemfile dosyasını açtı ve İsmail Bey anlatmaya başladı.Aklımda kalan bir kaç gem Devise,Hierapolis.Zaten bütün gemleri aklınızda tutmanızın imkanı yok.

* Devise Gemi ; Kısaca kullanıcı girişini denetlemenizi sağlayan gem.
* Hierapolis Gemi ; Admin teması.

Şimdi aklınıza şöyle bir soru gelmiş olabilir. İşime yarayacak gem i nerden bulacağım? Çok kolay. [Burdan](https://www.ruby-toolbox.com/) , adrese gidip Categories sekmesine tıklarsanız orada istediğiniz kategoriye uygun gemleri üstelik istatistiklerini de görerek ulaşabilirsiniz.

Öğleden sonranın tamamı kurulum ve gem tanımayla geçti. İkinci gün, nihayet kodlamaya, basit bir blog uygulaması yapmaya geçebilecektik. Burada yazımı şimdilik sonlandırıyorum. İkinci günü diğer yazımda anlatacağım. Hepinize bol RoR lu günler dilerim! :)




 
 
 