---
layout: post
title: "Ubuntuda Cybele ile Proje Oluşturma"
date: 2016-08-02 15:19:21 +0300
comments: true
categories: 
---


Evet arkadaşlar tekrar merhaba.Bugün sizlere ubuntuda Cybele tool u ile nasıl Ruby on Rails projesi oluşturabilirsiniz onu anlatacağım.Her şeyden önce Cybele yi indirip kullanabilmeniz için şu anlık gerekli olan en düşük Ruby versiyonu 2.3 , Rails versiyonu ise 4.2 dir.Eğer versiyonlarınız bunlardan eski ise lütfen güncelleyiniz.

Öncelikle Cybele gemini indirmemiz gerekiyor. Bunun için terminalimize ,


        gem install cybele
        

yazıyoruz. Daha sonra indirim tamamlanınca terminalimize ,     

        cybele project_name -d postgresql
        
yazıyoruz ve Cybele ile projemizi oluşturuyoruz. "project_name" kısmı sizin projenize vermek istediğiniz ismi yazacağınız kısım.Ben projemin adını "yeniDeneme" olarak belirledim.Bundan sonraki örneklerde "yeniDeneme" ismi üzerinden devam edeceğim lütfen dikkat edelim. İlk olarak yapmam gereken ruby versiyonumu kontrol ederek projemdeki ".ruby-version" dosyasındakiyle aynı mı değil mi bakmaktır.Değilse dosya içine kendi versiyonumuzu yazıyoruz.
        
Şimdi yapmamız gerekenlerden biri ise kullandığımız derleyicide oluşturduğumuz projeyi açıp **db** dosyasından **migrate** sekmesine ordan da **"devise_create_user"** ve **"devise_crate_admin"** dosyalarının içine girerek ,
          
          t.boolean :is_active

satırını ,

          t.boolean :is_active, default: true  
          
olarak değiştirmektir.Unutmayın user ve admin dosyalarının ikisinin de içini değiştiriyoruz.
          
Sırada ise **config** in altındaki **"settings.yml"** dosyasına giderek
          
          basic_auth:
            username: yeniDeneme
            password: yeniDeneme
          
          sidekiq:
            username: yeniDeneme
            password: yeniDeneme


Kısımlarının bu şekilde olduğundan emin oluyoruz.Bu ayarlar Cybele tool u içindeki gemlerin çalışması için gereklidir. Unutmayın "yeniDeneme" benim projemin adı sizin ekranınızda sizin projenizin ismi yazılı olacaktır.Burayı da kontrol edip gerekliyse değişiklik yaptıktan sonra terminalimizi tekrar açıyoruz ve cd komutu ile public dosyasına girip şunu ,


           ln -s ../VERSION.txt VERSION.txt
             


           
yazıyoruz terminale ve tekrar üst dizine geçip terminale

           bundle
           
yazıyoruz.           
           
Şimdi database işlemlerine geçme vakti geldi.Terminalimize ,
           
           sudo -u postgres psql
           
komutunu yazıyoruz ve şifremizi giriyoruz.(herzamanki bilgisayar şifreniz).Projemize dönerek config dosyası altındaki "database.yml" yi açıyoruz.
           
           development: &default
             adapter: postgresql
             database: yeniDeneme_development
             encoding: utf8
             min_messages: warning
             pool: 5
             timeout: 5000
             host: localhost
             port: 5432
             
olan kodlara username ve passoword ekliyoruz.
             
             development: &default
               adapter: postgresql
               database: yeniDeneme_development
               encoding: utf8
               min_messages: warning
               pool: 5
               timeout: 5000
               host: localhost
               port: 5432
               username: mrk
               password: mrk
               
Ben username ve password umu "mrk" olarak belirledim.Bundan sonraki işlemleri ona göre yapacağım lütfen dikkat ediniz.Tekrar terminale geliyoruz.İlk olarak
                
                CREATE DATABASE ‘yeniDeneme_development’;
                
komutunu yazıyoruz."yeniDeneme_development" yazmamın sebebei "database.yml" dosyasında "database: yeniDeneme_development" yazmasıdır. Daha sonra ,
 
                CREATE USER mrk WITH PASSWORD ‘mrk’;

yazıyoruz. Sonrasında
                
                ALTER USER mrk WITH SUPERUSER;
                 
**veya** dikkat edin lütfen "veya"
                 
                ALTER USER mrk WITH PASSWORD ‘mrk’;
                 
yazarak postgresql işemlerini tamamlıyoruz.Çıkış yapmak için
                 
                \q
                 
yazıyoruz.
                
Şimdi yapmamız gereken şey terminale sırasıyla
                
                bundle exec rake db:create
                
ve
                
                bundle exec rake db:migrate
                
komutlarını yazmaktır.
                
Bu komutları yazdıktan sonra derleyicimizde -ben RubyMine için anlatacağım- View in altından Tool Windows a geliyoruz , Tool Windows un altından Database e tıklıyoruz. Gelen pencerede "+" simgesine tıklayarak Data Source sekmesinden PostgreSQL i seçiyoruz. Ve ekranı şu şekilde dolduruyoruz. Unutmayın siz sizin kendi verilerinize göre dolduracaksınız.
               
<a href="http://tinypic.com?ref=xqj8sp" target="_blank"><img src="http://i67.tinypic.com/xqj8sp.jpg" border="0" alt="Image and video hosting by TinyPic"></a>              
                
Database : "database.yml" kısmında yazan yeniDeneme.development , user ve passwordu gene kendim "database.yml" dosyasında kendi oluşturduğum username ve password olan "mrk" ile doldurup "Test Connection" butonuna basıyorum ve database ile bağlantımı oluşturuyorum.
               
Şimdi terminalime
               
               bundle exec rake dev:seed
               
yazıyorum ve yeni bir terminal açarak aşağıdaki komutla redis server ımı çalıştırıyorum.
               
               redis-server
               
 Cybele ile proje oluşturduğunuzda arka taraftaki işler için redis server ınızın açık olması gerekmektedir.Tekrar eski terminalime geri gelerek
               
               rails s
               
 komutunu çalıştırıp localde projemi çalıştırmış oluyorum ve karşıma şöyle bir ekran geliyor.
              
 <a href="http://tinypic.com?ref=21yasw" target="_blank"><img src="http://i68.tinypic.com/21yasw.png" border="0" alt="Image and video hosting by TinyPic"></a>
              
 Evet arkadaşlar bugün sizlere Cybele ile nasıl Rails projesi oluşturulur bahsettim. Bundan sonra projenizi ne şekilde geliştireceğiniz size kalmış. Hepinize mutlu günler/geceler dileyerek yazımı sonlandırıyorum. Hoşçakalın!
              
 [Kaynak](https://github.com/lab2023/cybele)             