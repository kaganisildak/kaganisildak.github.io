---
layout: post
title: Gizlice takip edenleri öğrenirken gizlice gitmeyin
date: 2017-09-23 16:30:36.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '9585076920'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2017/09/23/gizlice-takip-edenleri-ogrenirken-gizlice-gitmeyin/"
---
Selamlar. Bugün Facebook’ta gezinirken art arda farklı sayfa isimlerinde aynı reklam sponsorlu olarak gözüme gözüktü. “Seni gizlice takip edenleri hemen öğren” diyerek halkımızın can damarı olan merakından başarıya ulaşmaya çalışan bu reklam tahminimce hayatını sosyal medyaya bağlayan kişileri avlamıştır.

![Screenshot_1]({{ site.baseurl }}/assets/images/2017/09/screenshot_1.png)

İlgili reklama tıkladığımızda bu sayfa karşılıyor bizi ve sosyal medyayı kullanan kitlenin genelinin yapmak istediği şeyleri tek tek gözümüze sokarak iyice cezbediyor beynimizi ve “harekete geç” dercesine sağdaki seçeneklere tıklamamızı istiyor.

![Screenshot_2]({{ site.baseurl }}/assets/images/2017/09/screenshot_2.png)

Seçeneklerden birine tıkladığımız zaman otomatik olarak bir eklenti yükle pop-up’ı açıyor. Bu noktadan sonra eklentiyi analiz etmek için kolları sıvamamız gerekiyor ve mağazadan eklentimize ulaşıyoruz.

![Screenshot_3]({{ site.baseurl }}/assets/images/2017/09/screenshot_3.png)

Hızlıca kaynak kodlarına ulaşmak istediğimden https://johankj.github.io/convert-crx-to-zip/ burayı kullanarak eklentimizi zip formatına dönüştürdük ve “akıllı dostlarımızın” kodlarına bir adım daha yaklaştık.

![Screenshot_4]({{ site.baseurl }}/assets/images/2017/09/screenshot_4.png)  
Gönüllerimizin efendisi, en “güvenilir” dostumuz js karşıladı baştan.(Yanlışlıkla ellerim felan diye uzantıyı değiştirdim. bg.js orijinal dosya adı)  
Hızlıca kodları okumaya başlayalım diyerek bir hışımla açtık js dosyamızı. O da nesi ?

![Screenshot_5]({{ site.baseurl }}/assets/images/2017/09/screenshot_5.png)

Zeki dostlarımız harika bir obfuscate yöntemi ile kodları karıştırmışlar ve benim işimi 3 kat zorlaştırdılar !  
“Google'dan” javascript deobfuscator diye aratarak ulaştığım ve işimi hep güzelleştiren “JSNice” yardımcı oldu bu konuda. http://jsnice.org/

![Screenshot_6]({{ site.baseurl }}/assets/images/2017/09/screenshot_6.png)

Saldırganların, eklentiyi kullananların Facebook cookilerine göz diktiği bariz olarak belli. Tanımladıkları diziye saldırganca komutlarını eleman olarak eklemişler ve “cookieleri\_getir” fonksiyonu ile “chrome” fonksiyonuna “cookies” “getAll” parametrelerini verip facebook.com’a ait tüm çerezleri çekmek istemişler ve atamışlar. Ardından “rapor\_gonder” fonksiyonuna “success” ve çektiği çerezleri JSON formatında vermiş.  
Bu fonksiyon aldığı verileri “http://thiskuki.club/gez.php” adresine yollayan bir fonksiyon. Resimde görüldüğü üzere request için gerekli verileri ve domaini aynı diziden çekmekte ve başarıya ulaşmaya çalışmaktadır.  
Gönderilen siteyi henüz inceleme imkanım olmadı ancak vaktim olduğunda onu da inceleyeceğim. Genel olarak saldırının detayları bu şekilde. Daha bugün harekete geçtiler. Dikkat edin, aldanabilecek arkadaşlarınızı uyarın.

