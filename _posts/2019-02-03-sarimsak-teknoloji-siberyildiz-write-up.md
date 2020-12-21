---
layout: post
title: Sarımsak Teknoloji | SiberYıldız Write-Up
date: 2019-02-03 13:07:17.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  _thumbnail_id: '597'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '27229434701'
  timeline_notification: '1549188441'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2019/02/03/sarimsak-teknoloji-siberyildiz-write-up/"
---
Selamlar herkese.

Çözüp flag giremediğimiz özel bir soru kendisi.(Son ana bırakmak kötü bir şey anladım artık)

![b6c5865b-5229-45ff-8700-4da35946edd0]({{ site.baseurl }}/assets/images/2019/02/b6c5865b-5229-45ff-8700-4da35946edd0.jpg)

Soruyu ilk açtığımızda şu şekilde bir metin ile karşılaşıyoruz. Amaç akış sonunda elde ettiklerimizle flag'i bulmak.

Zip içerisinde "onemli.docm" adında makro bazlı bir dosya var. İlk olarak bu dosyanın ne yaptığına, nasıl bir önyükleyici olduğuna karar vermek için neşteri elimize almamız gerekiyor.

[https://github.com/unixfreak0037/officeparser](https://github.com/unixfreak0037/officeparser)

Ofis dosyalarına ait nesneleri kazımak için şu aracı kullandım. İçeride makro var ve o güzel yolu gösterecek.

![carbon (10)]({{ site.baseurl }}/assets/images/2019/02/carbon-10.png)

Aracımız ile dosyamıza attığımız neşter darbesi sonucu "onemli.bas" adında bir modül ortaya çıktı. İlgili modülü açtığımız zaman klasik bir karıştırma metodu(pek karışık değil) ile oluşturulmuş, temel amacı uzak sunucuda bulunan powershell scriptini indirip çalıştırmak olan bir modül olduğunu görüyoruz.

[http://85.111.95.27/264dc8a2e84a32512a24142ca203cd86/tgh43ft56du7asw59oa02smawx2cbnm.ps1](http://ZARARLI MI BAĞLANTI?)

![carbon (11)]({{ site.baseurl }}/assets/images/2019/02/carbon-11.png)

Çalıştırılan scripti indirip baktığımızda kendisinin içinde bulundurduğu base64 ile encode edilmiş farklı bir scripti decode edip çalıştıran önyükleyici olduğunu anlıyoruz.

(Encoded stringler uzun ondan açıklayıcı farklı stringler kullanmayı tercih ettim :P)

![carbon (15)]({{ site.baseurl }}/assets/images/2019/02/carbon-15.png)

Çalıştırdığı diğer scripti elde etmek için şöyle ufak bir script yeterli.

![carbon (14)]({{ site.baseurl }}/assets/images/2019/02/carbon-14.png)

Çarşaflara dolanmış bu zararlımızın bu scriptinde ise artık gerçek yüzünü görmeye başlıyor gibiyiz. Kendileri bir DLL'e ve tekrardan çalıştırmak için farklı bir payload'a sahip.

![carbon (16)]({{ site.baseurl }}/assets/images/2019/02/carbon-16.png)

Oluşturduklarını görmek amacıyla son kısımda şöyle bir değiştirme yapmamız yeterli.

&nbsp;

![carbon (17)]({{ site.baseurl }}/assets/images/2019/02/carbon-17.png)

DLL ve artık zararlıya ait son payload elimizde.

Bu arkadaş ilk olarak kendini dinamik analiz esnasında analistin gözlerine kum atmak amacıyla bazı kontroller yapıyor. Basit bir anti-analiz fonksiyonu.

Ardından o decode edilen DLL'i "C:\ProgramData" altında "ScreenSaver.dll" adı ile kaydediyor.

Bu işlemden sonra çalıştığı domainin isminin "TEKNOSARIMSAK" olup olmamasına göre bir takım işlemler yapıyor.

Soruda bahsi geçen muhabbet burada gerçekleşiyor. Onlarda farklı bizde logger olayı.

Şayet çalıştığı bilgisayar bu kurala uygun değilse zararlımız hedef DLL'i "logger" fonksiyonunu tetikleyerek başlatıyor.

Öncesinde zamanlanmış görev olarak kendini oluşturuyor.

Kurala uyuyor ise VoidFunc fonksiyonu ile bu işlem gerçekleştiriliyor.

![yes-yes-disassemble]({{ site.baseurl }}/assets/images/2019/02/yes-yes-disassemble.jpg)

Scriptler için neşter yeterliydi. Şimdi elimize samuray kılıcını alma vakti geldi.

İlgili fonksiyon çok uzun olduğundan tümünü sığdırmam biraz zor ondan kabaca ne yaptığını anlatayım. Hedef sistemde bu fonksiyon ile çalıştırıldıktan ilk olarak "3572" portunu dinlemeye alıyor. Port üzerinden bir veri aldıysa aldığı veriyi bir dizi kontrolden geçiriyor.

Kontrollerden ilki CRC doğrulaması. Hedefe verilen verinin CRC32 sonucu EE1E51C3 değerine denk mi değil mi diye bir kontrol yapılıyor. Denk değilse "Hatali CRC (Patchlebeni!") şeklinde bir mesaj veriliyor.

Bu kontrolün ardından 6 adet karakter bazlı bir doğrulama yapılıyor.

CRC doğrulamasının verebileceği pek bir şey yok zaten. 6 adet karakter doğrulamasından girdinin 6 karakterli olması gerektiğini de bariz şekilde anlıyoruz.

&nbsp;

![Screenshot_13]({{ site.baseurl }}/assets/images/2019/02/screenshot_13.png)

Karakter doğrulama bu şekilde gerçekleşiyor.

Karakter doğrulama olayında kullanılan bir taslak dizi var. "102030405060" şeklinde.

Her işlemde bu dizi üzerinde oynama yapıldıktan sonra sub\_74181EB0 fonksyionu çağrılıyor ve ardından elde edilen çıktı ile bizim girdimizin karakterleri sırayla karşılaştırılıyor.

cmp&nbsp; &nbsp; &nbsp; &nbsp; cl, [eax+4]

Karakter sırasına ait veri de burada yer alıyor.

![Screenshot_15]({{ site.baseurl }}/assets/images/2019/02/screenshot_15.png)

Dizi modifikasyonunda sonra çağrılan fonksiyon ise girdiyi "A5" ile xor'layıp dönütlüyor.

![maxresdefault]({{ site.baseurl }}/assets/images/2019/02/maxresdefault.jpg)

Fonksiyon var, veri var. Yapalım öyleyse.

eax+4 6954030201h = 0  
eax+2 6054D00201h = u  
eax+3 60590030201h = 5  
eax+1 605403DD01h = x  
eax+0 6054030295h = 0  
eax+5 0C8054030201h = m

Sırasıyla hangi verilerin karşılaştırıldığına bakıp kendimiz xor işlemini yaptığımızda yukarıda bulunan değerleri elde ediyoruz.

Elde ettiklerimizi sıralarsak

0xu50m bize kapıları açacak olan anahtar olmuş oluyor.

![Screenshot_16]({{ site.baseurl }}/assets/images/2019/02/screenshot_16.png)

F\_L\_4\_G: USOM\_MHh1NTBt

![bucket]({{ site.baseurl }}/assets/images/2019/02/bucket.png)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

