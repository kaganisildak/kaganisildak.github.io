---
layout: post
title: 'GSM Güvenliği : A5/1 Algoritması'
date: 2017-07-05 17:47:11.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  _edit_last: '100361993'
  _oembed_6fcad304420c2fc16736a4b7e761012f: "{{unknown}}"
  geo_public: '0'
  _publicize_job_id: '6793268837'
  _oembed_abc9746a51926d3ceeb7e4a994696b15: "{{unknown}}"
  _oembed_06a84f26b3d0b170b0e80bec3c11eed1: "{{unknown}}"
  _oembed_3e734b56aea5accfea46dbf9795b2fa4: "{{unknown}}"
  _oembed_b635aacc6113f893c3f7e26b5e6ac06b: "{{unknown}}"
  _oembed_f16d2bc58a1b308b887ad7d81f9ff43b: "{{unknown}}"
  _oembed_bc66b9683c5a551c126caeaa82f7f33a: "{{unknown}}"
  _oembed_822f192c635a963e7134b36674aabca6: "{{unknown}}"
  _oembed_c766ae4378ccac10840cfdc814b49916: "{{unknown}}"
  _oembed_9b60cf2cd9940273d0939f84a9a0710e: "{{unknown}}"
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2017/07/05/gsm-guvenligi-a51-algoritmasi/"
---
&nbsp;

Merhaba. Bu yazımda GSM güvenliğinde vakti zamanında rol oynamış olan A5/1 algoritmasının işleyişinden ve olası saldırı tiplerinden bahsedeceğim.

Nedir bu A5/1 ?

Ellerimizde akıllı telefonlar bulunmakta ve son 4.5G ağının tam olarak yaygınlaşmadığını düşünürsek hala 2G aktif kullanımda. 2G dediğimiz bu iletişim ağı/bandı veri aktarımı sırasında güvenliğe ihtiyaç duyuyor ve A5/1 bu ihtiyacı karşılamak amacıyla hazırlanmış algoritma aleminin hızlılarından olan akış şifreleme(stream cipher) ailesine ait bir algoritmadır.

Ancak gel gelelim sistemin kötü abileri burada da devreye giriyor. Amerika ve Avrupa standart olarak A5/2’ye kıyasla daha güçlü olan A5/1’i benimsedi. Diğer ülkeler nedendir bilinmez A5/2’yi standart olarak kabul etti. 1999 da A5/2’nin algoritmasının halka açılmasıyla bir ay içerisinde kırıldı. A5/1’in normalde 128 bit uzunluğunda bir anahtarla çalışacağı düşünülüyordu ancak 64 bit anahtara sahip olarak ortaya çıktı. Bunun sebebi ellerindeki imkanın o zaman bu boyutu desteklemesi yahut mevcut hükümetlerin ileriye yönelik olarak planlarından dolayı baskı yaparak zayıf bırakılmasını sağlaması olabilir.

Nasıl Çalışır ?

![a5/1.png]({{ site.baseurl }}/assets/images/2017/07/image16.png)

A5/1 Üretecinin Çalışma Yapısı

A) LFSR(Linear Feedback Shift Register) ve Bağlantı Noktaları

Sistemde üç adet LFSR bulunur. LFSR ne derseniz sizi [şuraya](https://www.google.com/url?q=http://bilgisayarkavramlari.sadievrenseker.com/2010/05/19/lfsr-linear-feedback-shift-register/&sa=D&ust=1499269232645000&usg=AFQjCNFRXV6uNPKwixhUWVUqWCoYVW-aaw)&nbsp;alalım.

Üçü

 ![]({{ site.baseurl }}/assets/images/2017/07/image1.png)

periyotlarıyla m&nbsp;dizisini oluşturur.

![]({{ site.baseurl }}/assets/images/2017/07/image2.png)

&nbsp;fonksiyonu LFSR 1’dir ve a(t)’yi oluşturur.

 ![]({{ site.baseurl }}/assets/images/2017/07/image3.png)

fonksiyonu LFSR 2’dir ve b(t)’yi oluşturur.

 ![]({{ site.baseurl }}/assets/images/2017/07/image4.png)

fonksiyonu LFSR 3’tür ve c(t)’yi oluşturur.

Bağlantı noktaları A5/1 için

 ![]({{ site.baseurl }}/assets/images/2017/07/image5.png)

şeklindedir.

B)Majority fonksiyonu

Bu fonksiyon az sonra göreceğiniz&nbsp;Anahtar Akış Üretecinde kullanılan bağlantı noktalarını giriş değeri olarak alan fonksiyondur.

 ![]({{ site.baseurl }}/assets/images/2017/07/image6.png)

şeklinde tanımlanmıştır.

 ![]({{ site.baseurl }}/assets/images/2017/07/image7.png)&nbsp;

&nbsp; &nbsp; &nbsp;

 ![]({{ site.baseurl }}/assets/images/2017/07/image8.png)

![Untitled Diagram.png]({{ site.baseurl }}/assets/images/2017/07/image15.png) ![sad]({{ site.baseurl }}/assets/images/2017/07/image14.png)

Çıktı dizisi

 ![]({{ site.baseurl }}/assets/images/2017/07/image9.png)

. t zamanını baz alır.

 ![]({{ site.baseurl }}/assets/images/2017/07/image10.png)

şeklindedir ve

 ![]({{ site.baseurl }}/assets/images/2017/07/image11.png)

 ![]({{ site.baseurl }}/assets/images/2017/07/image12.png)

şeklinde tanımlanan sayılar stop-and-go(dur/kalk) saatiyle kontron edilir. Değerini buradan alır.

![algoritmaa51.png]({{ site.baseurl }}/assets/images/2017/07/image13.png)

Kaynaklar :

https://asecuritysite.com/encryption/a5

http://cgi.di.uoa.gr/~halatsis/Crypto/Bibliografia/Crypto\_Lectures/Stinson\_lectures/lec08-ch6b.pdf

https://www.emsec.rub.de/media/crypto/attachments/files/2010/04/da\_gendrullis.pdf

