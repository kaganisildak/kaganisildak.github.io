---
layout: post
title: Sevgili Günlük - Crypto100 WriteUp | DKHOS
date: 2018-02-15 03:06:38.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  timeline_notification: '1518653201'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '14756136393'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2018/02/15/sevgili-gunluk-crypto100-writeup-dkhos/"
---
Yine Prodaft yine harika bir ctf ve yine cinnet geçirten sorular. Ama merak etmeyin bu onlardan biri değildi. Bu soruda bize bir arşiv verilmişti ve bir şeyleri çözmemizi bekliyorlardı. Arşivlerimizi açtığımızda elimizde "public.key" ve "flag.txt.enc" adında iki adet dosya karşılıyordu bizleri.

public.key'e baktığımda kafamda He-Man'in şimşekleri çaktı ve rsa bende artık dedim. [Şuradaki](https://8gwifi.org/PemParserFunctions.jsp) pem readerı kullanarak public keyimin detaylarına ulaştım. Detay diyince öyle "abovvv ne var acaba" demeyin bir n değeri bir de klasik public exponent değeri. n değeri şu şekilde

![Screenshot_1]({{ site.baseurl }}/assets/images/2018/02/screenshot_1.png)

c9bc6e819d316a23a75ea29aa7428634617c09a6e77a25c8884cd405dfb7caa3705d8fd5c7dbd1930eadb3fb4d4601e330add121f4f7cc515253b101d310d9dbb3a72eca36a1de62a6f4d573f5fb71cadb017579dce149e9

Huy mudur bilmem ama bir karekök takıntım var. İki çarpım görünce ne olursa olsun önce karekökünü alırım . Normalde ecm factoring felan gereksin isterdim azcık makine kastıralım diye ama canım ctf'im yormadı bizi. İlgili n değerinin karekökünü aldığımda karşıma tam bir sayı çıkınca "aha tamam" dedim.

p ve q değerimiz 8143859259110045265727809126284429335877899002167627883200914172429324360133004116702003240828777970252499&nbsp;şeklindeydi.

Flag'e çok az kaldı. Private key oluşturup dosyayı decrypt edeceğiz sadece. Private key'i oluşturmak için&nbsp;[https://github.com/ius/rsatool](https://github.com/ius/rsatool) şu aracı kullandım.

![Screenshot_3]({{ site.baseurl }}/assets/images/2018/02/screenshot_3.png)

Bakkal amca, bakkal amca.

-Ne var ?

-Encrypted verin var mı ?

-Var var.

-Private keyin var mı ?

-Var var.

-Ne duruyorsun flag bulsana, flag bulsana.

&nbsp;

Sıra geldi flagi almaya. Openssl ile flagimizi çekelim. "openssl rsautl -decrypt -in flag.txt.enc -out flag.txt -inkey key.pem"

![Screenshot_4.png]({{ site.baseurl }}/assets/images/2018/02/screenshot_4.png)

&nbsp;

