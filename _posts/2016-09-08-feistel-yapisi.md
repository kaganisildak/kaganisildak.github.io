---
layout: post
title: Feistel Yapısı
date: 2016-09-08 22:12:04.000000000 +03:00
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
  _publicize_job_id: '26625798176'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/09/08/feistel-yapisi/"
---
Selamlar arkadaşlar.  
Bu konuda Feistel yapısından bahsedeceğim.

Öncelikle Feistel kriptografide blok şifrelemede kullanılan bir yapıdır.  
DES'in babası Lucifer'de görülmüştür. Doğal olarak DES'te de aynı yapı korunmakta.

İmplemente sırasında avantajı vardır. Şifreleme sırasındaki gidişat çözme esnasındaki ile aynı şekilde ilerler. Tek farkı anahtar kullanım sırasıdır. E haliyle bu sırayı belirlemek baştan farklı bir çözüm algoritmasını implemente etmekten daha kolaydır ve bu sayede zamandan kazandırır.

Şeması :  
 ![lqnaov]({{ site.baseurl }}/assets/images/2016/09/lqnaov.png)  
Şifreleme esnasında Plain Text iki bloğa bölünür ( normalde blok uzunlukları eşittir ancak dengesiz feistel'de bunlar eşit olmayabilir)  
.  
Bölünen bloklar seçilen parçalar ve anahtar ile fonksiyona sokulur(ki bu fonksiyon algoritmadan algoritmaya değişiklik gösterebilir)  
Fonksiyonlar genellikle xor, bit değişimi yahut subst. kullanır.  
İstenilen şekilde tekrara tabii tutulur. Her tekrar için farklı anahtarlar kullanılır(aynı kullanılırsa amacımız kalmaz değil mi :) )  
Bu anahtar üretimide algoritmadan algoritmaya değişiklik gösterebilir.

Sonuçta şifreli metin elde edilir.

Çözümü de şemada gördüğünüz üzere sadece tersine uygulamaktır.

**Dengesiz Feistel**  
L0 ve R0 eşit uzunlukta değildir.

