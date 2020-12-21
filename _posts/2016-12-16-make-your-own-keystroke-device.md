---
layout: post
title: Kendi USB RubberDucky'nizi Yapın
date: 2016-12-16 21:08:23.000000000 +03:00
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
  _publicize_job_id: '30067711742'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/12/16/make-your-own-keystroke-device/"
---
Selamlar arkadaşlar. Bloguma ya da youtube kanalıma baktıysanız BadUINO adında birçok video görmüşsünüzdür. Ekim ayında başladığım projemde amacım USB RubberDucky'i daha ucuza mâl etmek ve cihazın işleyişini kavramaktı. Ufak araştırmam sonrası elde ettiğim bilgileri aktarayım.

Cihazın basit&nbsp;işleyişi : Kendini klavye olarak tanıt \> Önceden yüklenen komutları sırayla yollamaya başla

Detaylar...

Kendini klavye olarak tanıtma dediğimiz [HID(Human Interface Device)](https://en.wikipedia.org/wiki/Human_interface_device)&nbsp;aslında. Mike Van tarafından ilk defa kullanılan bu sözcükler insan ile etkileşime geçip veri aktarabilen cihazlar için kullanılıyor. Klavye de bu cihazlardan biri. USB RubberDucky de kendini HID Keyboard olarak tanıtıyor. Bu tanıtım olayı ise mikrokontrolcü ile alakalı bir olay. Bu yazımda&nbsp;atmega32u4'e sahip bir kart ile cihazımızı yapacağız lakin Phison2303 kontrolcüye sahip bir USB bellek ile de bunu yapabilirsiniz. Phison için&nbsp;[buraya](http://null-byte.wonderhowto.com/how-to/make-your-own-bad-usb-0165419/), Türkçe okumak istiyorum diyorsanız&nbsp;[Mert Sarıca'nın bu yazısına yönlendireyim sizleri](https://www.mertsarica.com/bad-bad-usb/). USB RubberDucky de&nbsp;Amtel AT32UC3B1256 kontrolcü ile işlerini yerine getiriyor.

Önceden yüklenen komutları sırayla yollamak ise klavye olmamızın bir getirisi. Yazımda arduino ile yüklediğimiz kod üzerinden komutlar yollanıyor. RubberDucky ise payloadı convert ettikten sonra firmware'i güncelliyor.

Basitleştirilmiş teknik bilgilerden sonra prototipimizi yapmaya başlayalım. 32u4 kontrolcü Arduino Micro/Leonardo ve A-Star 32u4 Micro kartlarında bulunuyor. Arduino'nun Keyboard fonksiyonunu doğal olarak kullanabiliyorlar. Arduino ile BadUSB mantığında nasıl kodlar geliştirebilirim diyerek&nbsp;[bu videoyu çekmiştim.](https://www.youtube.com/watch?v=51G5LXRFeRI)&nbsp;İlk prototipim Leonardo idi ardından boyutsal küçültmeye giderek A-Star 32u4 micro ve "b to a" dönüştürücü aldım.

Ürünlere şuralardan ulaşabilirsiniz :

[Pololu A-Star](https://www.pololu.com/product/3101)

[Dönüştürücü Set](https://tr.aliexpress.com/item/UN2F-10pcs-set-OTG-5pin-F-M-Changer-Adapter-Converter-USB-Male-to-Female-Micro-USB/32613315988.html?spm=2114.13010608.0.0.SiH9na&detailNewVersion=&categoryId=70806)

[gallery ids="263,264" type="rectangular"]

&nbsp;

Ürünlerimiz geldi bir heves ile açtık Keyboard lib'ini kullanarak komut yolladık, teorik bilgimizi fiziksel hale büründürdük ve önümüze çıkan sorunları yazalım.

•Arduino için yazmakla uğraşmak sizi yorabilir ve detaylı payloadlarınızı kodlarken çıldırma durumuna gelebilirsiniz. Vaktiniz değerli unutmayın.

• Library ingilizce klavyeye göre yazılmış ve TR klavye yerleşimi bakımından farklı olduğundan iletişim esnasında sıkıntılar çıkacak ve koda yazdığınız karakterler çalışma esnasında farklı olarak yollanacak.

Bu sorunlar başıma geldiğinden çözmek için çalışmalara başladım. Öncelikle Türkçe karakter sorununu Arduino kütüphanesine dokunmadan çözmem gerekiyordu. Türkçe klavye ve İngilizce klavye arasındaki bağlantı için 9-10 deneme ardından metodumu belirledim ve her karakter için Türkçe klavyedeki karşılığını buldum. Projenin en sıkıcı kısmı burasıydı.

![screenshot_2]({{ site.baseurl }}/assets/images/2016/12/screenshot_2.png)

Detaylı çalışmam sonucu hatasız bir liste oluşturdum. 2. sorunum ortadan kalktı ve geriye yapay payload dilini yapılandırmak kaldı. .NET ile bir arayüz yazmak istedim ve başladım."STR" ile string yollanacak "DLY" ile delay verilecek ve diğer tuşlar kullanılacaktı. Payloadları arduino koduna çevirerek yolu belirtilmiş arduino uygulaması üzerinden seçilen porttaki karta yükleyecekti.

[gallery ids="280,281" type="rectangular"]

2 günlük bir çalışma sonucu arayüzü oluşturarak projemi tamamladım ve kendimi payload yazmaya adadım.

![screenshot_5]({{ site.baseurl }}/assets/images/2016/12/screenshot_5.png)

Test ettiğimce hataları kapatmaya çalıştım ve genel olarak biten projemi herkesin kullanabilmesi amacıyla sizlerle paylaşıyorum. Vakit buldukça geliştirmeye devam ediyorum. Bu noktada geri dönüşleriniz benim için önemli. Görüşlerinizi bekliyorum.

[Kaynak kodlar](https://github.com/kaganisildak/baduino)

[YouTube Kanalım](https://www.youtube.com/channel/UCK3KgUrj5NmA4dNTvd5JdIw)

&nbsp;

CyberWarrior - ProgrammerMan

AKINCILAR

&nbsp;

