---
layout: post
title: ZEMANA AntiKeylogger'ı Nasıl ByPassladım ?
date: 2017-08-16 18:49:07.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags:
- backdoor
- keylogger bypass
- zemana antikeylogger bypass
meta:
  _publicize_job_id: '8326700891'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2017/08/16/zemana-antikeyloggeri-nasil-bypassladim/"
---
&nbsp;

Selamlar arkadaşlar. Bu yazımda Zemana’ya ait AntiKeylogger yazılımını nasıl bypass’ladığımı, bypass sürecinde hangi bilgilerden yararlandığımı ve pratiğe nasıl döktüğümü anlatacağım.

## **Bilgi Toplama Evresi :** 

Zemanayı bilgisayara indirip kuruduğumuzda 3 ayrı noktaya dosya yerleştirmekte.

Bunların 2 tanesi Program Files’ın içinde bulunup klasör isimleri Zemana AntiLogger(Yazılıma ait ana uygulama ve yan paketleri)&nbsp; ve KeyCryptSDK(Kullandığımız process’lere enjekte olan ve zararlıların kayıt almasını engelleyen birnevi koruma kabu diyebileceğimiz DLL’ler). Diğeri ise AppData\Local&nbsp;içerisinde Zemana(Ayar dosyaları, zararlılara ait imzala verileri, raporlar ve karantina altına alınan uygulamalar) şeklinde bulunmakta.

Kritik Bilgilendirme: Bu durum AntiMalware için de geçerlidir. AppData içerisinde tutulan settings.db yazılımın hariç tutulan listesini de barındırmaktadır. Normal okumaya çalıştığınızda şifreli olduğunu farkedeceksiniz ancak kritik nokta şu. Yapılan şifreleme işleminde kullanılan anahtar tüm yazılımlarda ve bilgisayarlarda aynı olduğundan ayar dosyasının değiştirilmesi ile zararlı yazılım kullanıcının haberi olmadan temiz listesine eklenmesine olanak sağlıyor.

Zemana Kayıt Defterinde aktif olarak rol oynayan 2 adet kayıt açmakta. HKLM\Software içerisinde Zemana ve KeyCryptSDK şeklinde oluşturulmakta. Zemana içerisinde yazılıma ait anahtarlar bulunurken, KeyCryptSDK içerisinde koruma kabuğumuza ait DLL’lerin dizinleri ve yazılımlara DLL’ler enjekte edilmiş mi bunun kayıtları bulunmakta.

![]({{ site.baseurl }}/assets/images/2017/08/image2.png)&nbsp; ![]({{ site.baseurl }}/assets/images/2017/08/image3.png)

Uygulamanızı başlattığınız zaman process’inizi korumak amacıyla kendini enjekte etmekte.

ÖNEMLİ NOT :&nbsp;Modülleri unload yapmak korumayı devre dışı bırakmamakta, process’i crash’e uğratmaktadır.

## **Nasıl ByPassladım ?**

Asıl koruma işlemini KeyCrypt DLL’lerinin sağladığını Bilgi Toplama bölümünde belirtmiştim. İlk önce basit bir powershell scripti yazdım. Kendisi user32.dll’i kullanarak klavyeden basılan tuşları kaydetmekteydi.

Kritik not : Zemana’da bahsettiğim script zararlı olarak işaretlenmiş durumda ve scantime’da virüs uyarısı vermekte fakat runtime’da Zemana bunu engelleyemedi. Sebebi powershell üzerinden yorumlanması ve Zemana’nın powershell’e erişebilecek bir fonksiyonunun bulunmamasıydı.

Oluşturduğum script doğal olarak normal şekilde çalışırken doğru şekilde herhangi bir tuş kaydı yapamamakta. Aldığı kayıtlar ise rastgele değerler olmaktaydı. Bu noktada Zemana’nın önüne geçmemiz gerekmekte.

•İlk olarak Zemana’nın servis ve arayüzünü “force-kill” metodu ile kapattım. Ancak sonuç olumsuz oldu DLL’ler enjekte halde olduğundan var olan process’ler üzerinden kayıt gerçekleştiremedim.

•İkinci yöntem olarak aktif çalışan pencerenin adı üzerinden process’e ulaşıp ilgili DLL’leri unload ettim ve yazılım anında crash verdi. Zemana hala gücünü koruyor :)

•Üçüncü olarak KeyCryptSDK dosyasına erişerek var olan DLL’leri değiştirmeye çalıştım. Scriptim çalıştıktan sonra gözle görülür bir değişiklik olmadı sebebi ise process’lerin aktif olarak kullanması ve servisinin de DLL’i kullanarak erişimi engellemesiydi.

•Dördüncü son ve başarılı denememde ise Regedit kayıtlarıyla oynadım. Bilgi toplama bölümünde yazmış olduğum KeyCryptSDK’ya ait kayıt defteri yolundaki değerleri tümüyle önce kaldırdım.(Yazılım otomatik olarak bu değerleri takip etmemekte ve yokluğunda geri düzeltmemkte ki zafiyetin temel sebebi bu.) Kayıt defteri yolunda DLL Pathleri bulunduğundan aktif olarak DLL enjekte eden fonksiyon boş değer almakta ve herhangi bir hata mesajı vermeden normal şekilde çalışmasına devam etmekteydi. Ardından kayıt scriptimi çalıştırdığımda başarılı bir şekilde kayıtları aldım. Aşağıda test videosunu izleyebilirsiniz.

Emre Tinaztepe hocama bildirim sürecinde yardımcı olduğu için teşekkür ederim.

[youtube https://www.youtube.com/watch?v=IvZ8YhHWROg&w=560&h=315]  
[youtube https://www.youtube.com/watch?v=CJ8bG073m\_U&w=560&h=315]

