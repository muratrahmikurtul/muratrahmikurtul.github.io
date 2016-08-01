---
layout: post
title: "Workshop Serüveni İkinci Gün"
date: 2016-08-01 14:20:04 +0300
comments: true
categories: 
---
Tekrar merhaba arkadaşlar bugün ikinci günü anlatacağım.


## Öğleden Önce

Önceki gün olduğu gibi sabah tam 09:00 da eğitimin yapılacağı yerde yerimi almıştım. Öğleden önce kaldığımız yerden 11:30 a kadar [İsmail Bey](https://twitter.com/isoakbudak?lang=tr)le devam edecektik.Daha sonra [Leyla Hanım](https://twitter.com/leylakapi?lang=tr) öğle arasına kadar bize front end , yani temel seviye html,css,haml,sass,bootstrap eğitimi verecek öğleden sonra tekrar İsamil Bey ile devam edip workshop ı bitirecektik.

09:00 itibariyle başladık. Fakat bazı arkadaşlar localhost ta çalıştırdıkları server larını kapamadan bilgisayarlarını uykuya aldıkları için bazı sorunlar yaşıyor "rails s" komutunu çalıştıramıyorlardı.Bu hatanın çözümü için İsmail Hocamız localhost ta port değiştirmeyi öneerdi.

* **Port Değiştirme** ; terminal e


```sh

   rails s --port = 3001
```

yazılarak 3000 olan portu 3001 yapıp hatayı gidermiş olduk ve "rails s" komutunu sorunsuz çalıştırdık.

Bu hatayı da giderdikten sonra Cybele tool unu kullanarak kendi basit blog uygulamamızı oluşturduk.


        cybele project_name


Uygulama üzerindeki birkaç basit hatayı düzelttik.Örneğin profiles_controller.rb dosyasının adını profile_controller.rb olarak güncelledik.Projemizdeki controller ve modelleri inceledik.Yönetici olan kişiye hoşgeldin maili göndermek yapmamız gerekenler anlatıldı.Models altındaki admin.rb dosyasına 

```sh
    def send_welcome
        AdminMailer.welcome(self.id).deliver_later!
    end
```

Kodlarını ekledikten sonra controllerdaki admin_mailer.rb dosyasına giderek

```sh
    def welcome admin_id
        @admin      = Admin.find admin_id
        @subject    = t('email.admin.welcome.title')
        mail(to: @admin.email, subject: @subject)
    end
```

Kodlarını ekledik. Ama yönetici olan kişiye hoşgeldin maili atabilmemiz için bir şey daha yapmamız gerekiyordu. O da tekrar admin.rb dosyasına gidip

```sh
     after_commit :send_login_info, on: :create
```
     
satırına

```sh
     after_commit :send_login_info, :send_welcome, on: :create
```     

eklemekti.

Gönderdiğimiz mailin içeriğini de View altındaki admin_mailer dosyası içinde oluşturduğumuz welcome.html.haml dosyası içerisinde yazdık.

```sh
    %h2= t('email.hello')
    
    %p
        = @admin.email
        İyi günler dileriz.
```        


Bazı arkadaşlarımız mail yollamada hata aldılar. Bunun sebebi ise şuydu , controller da oluşturduğunuz action ın adı neyse -benim örneğimde welcome- view daki dosyanızın adı da o olmak zorundaydı.

Bu arada bu maili yolladık ta nasıl yolladık?? Gibi sorular oluşmuş olabilir kafalarda. Bunların cevaplarını öğleden sonra kısmında anlatacağım.

Bunu da yaptıktan sonra saat 11:30 olmuş ve ara vakti gelmişti..

#### Front End

Aradan sonra [Leyla Hanım](https://twitter.com/leylakapi?lang=tr) ile front end e başlamıştık. Benim için uygulama geliştirmenin en zevkli kısmı front end kısmıydı.Ruby on Rails felsefesinden ve temiz kod yazma isteğinden ötürü html.erb yerine haml, css yerine de sass kullanıldığını , sass ın getirdiği kolaylıkları (kalıtım yapısı , extend ve mixin özellikleri) bize güzel bir şekilde açıklayarak artık uygulamaya geçmemizi istedi. Haml , navbar değiştirme , button ekleme css , sass ve son olarak compass a değinerek Leyla Hanım eğitimi bitirecekti.


Bu arada html kodlarınızı haml a çevirmek için ; [Bu Linki](https://html2haml.herokuapp.com/) kullanabilirsiniz.


Css kodlarınızı Sass a çevirmek için ise ; [Bu Linki](http://css2sass.herokuapp.com/) kullanabilirsiniz.


Leyla Hanım bizden bir button eklememizi buttonun içinde hello yazmasını buttonun kırmızı olmasını ve yazının da beyaz olmasını istemişti.Ben tabiki hemen sözde sivri zeka olarak bootstrap ile danger button kullanarak dediğini yapmıştım.Fakat Leyla Hanım kontrolden sonra bootstrap ile istemediğini css ile yapmamız gerektiğini söyleyerek yaptığım çakallığı ve "2 saniyede bitirmiş olan öğrenci" havamı yerle bir etmişti :). Her neyse daha sonra aşağıdaki kodlarla üye ol ve giriş yap buttonlarını navbara ekleme işlemi yaptık.

```sh
      %li
         =link_to  new_user_session_path do
         %i.fa.fa-sign-in{"aria-hidden" => "true"}
         Giriş yap
      %li
         =link_to new_user_registration_path do
         %i.fa.fa-user-plus{"aria-hidden" => "true"}
         Üye ol              
```

Son olarak compass ile button kırpma gibi işlemleri kolayca yapabildiğimizi görüp Leyla Hanım ile olan eğitimimizi tamamladık.

    
Burası HAML kısmı


```sh
    #border-radius.border-radius-example
      %p
        Box with all corners rounded
     
    #border-radius-top-left.border-radius-example
      %p
        Box with only top left corner rounded
     
    #border-radius-top-right.border-radius-example
      %p
        Box with only top right corner rounded
     
    #border-radius-bottom-left.border-radius-example
      %p
        Box with only bottom left corner rounded
     
    #border-radius-bottom-right.border-radius-example
      %p
        Box with only bottom right corner rounded
     
    #border-radius-top.border-radius-example
      %p
        Box with top corners rounded
     
    #border-radius-bottom.border-radius-example
      %p
        Box with bottom corners rounded
     
    #border-radius-left.border-radius-example
      %p
        Box with left corners rounded
     
    #border-radius-right.border-radius-example
      %p
        Box with right corners rounded
     
    #border-radius-combo.border-radius-example
      %p
        Box with different roundings for top/bottom and left/right
```
        
 
Burası SASS kısmı

        
```sh
     @import compass/css3
         
     @import compass/utilities
         
     #demo
       +clearfix
         
     .border-radius-example
          width: 125px
          height: 125px
          background: red
          margin: 20px
          float: left
          padding: 5px
         
      #border-radius
          +border-radius(25px)
         
      #border-radius-top-left
          +border-top-left-radius(25px)
         
      #border-radius-top-right
          +border-top-right-radius(25px)
         
      #border-radius-bottom-left
          +border-bottom-left-radius(25px)
         
      #border-radius-bottom-right
          +border-bottom-right-radius(25px)
         
      #border-radius-top
          +border-top-radius(25px)
         
      #border-radius-bottom
          +border-bottom-radius(25px)
         
      #border-radius-left
          +border-left-radius(25px)
         
      #border-radius-right
          +border-right-radius(25px)
         
      #border-radius-combo
          +border-corner-radius(top, left, 40px)
          +border-corner-radius(top, right, 5px)
          +border-corner-radius(bottom, left, 15px)
          +border-corner-radius(bottom, right, 30px)
```
        

## Öğleden Sonra
        
Eğitimin en yorucu kısmı 2.gün öğleden sonrasıydı. Rollbar , sidekiq , sendgrid gibi konuları işleyecek ve bloğumuzu heroku ya deploy edecektik.Bu arada heroku deploy olayını ileriki yazılarımın birinde ayrıntılı bir şekilde anlatacağım.Bu yazıda ayrıntılı yer vereceğim şey ise sendgrid olacak.


* **Rollbar** anladığım kadarı uygulamanızda oluşan hataları görüntülüyor ve rapor veriyor bu sayede de analiz edip çözmeyi sağlıyor.
* **Sidekiq** Ruby için verimli arkaplan job processing i.
* **Sendgrid** ise bahsettiğim mail yollama işlerini üzerinden yaptığımız yardımcı bir platform.

Sendgrid e üye oluyoruz. Yalnız üye olurken gerçek bilgilerinizi vermenizi öneririm çünkü bir süzgeçten geçiriyolar.Üye olduktan bir süre sonra üyeliğimiz onaylanıyor ve işlemlerimize geçebiliyoruz. **Settings** e gelip **Credentials** a tıklıyoruz.**Add New Credential** diyoruz.Username ve password girip -bu arada username inizi anlamlı vermenizi öneririm- alttaki mail kısmını işaretleyip(check edip) **Create Credential** diyoruz.Daha sonra env.staging dosyamıza gelip
 
 
* SMTP_PASSWORD=
* SMTP_USER_NAME=
* SMTP_ADDRESS=

kısımlarını doldurup kaydediyoruz.Sıradaki adımımız ise bunları herokuya setlemek oluyor.Bunu da aşağıdaki kod ile yapıyoruz.


```sh
    heroku config:set SMTP_PASSWORD= ******
```

Unutmayın password , username ve adress in her biri için setleme yapılmalıdır.

Setleme işlemlerini de bitirdikten sonra heroku dashboard a gidip settings e girip  "Revial Config Vars" a tıklayıp setleyip setleyemediğimizi kontrol ediyoruz.Eğer adımları doğru uyguladıysak set ettiğimiz alanları orada görüyoruz ve artık uygulamamızda gönderdiğimiz mailleri sorunsuzca gönderebiliyoruz.

<span style="color:red;">Dip not : Heroku da resources kısmında worker ınızın on durumda olması gerekmektedir.</span>

Ve bunun sonunda ikinci günü de bitirip workshop ı sonlandırdık.Umarım anlattıklarım içinde herkes kendine yararlı olabilcek şeyler bulmuştur.Kendinize iyi bakın sağlıklı beslenin!! :)
    
    
