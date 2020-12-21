---
layout: post
title: 'Nasıl Çalışır : Base64'
date: 2016-06-08 22:51:13.000000000 +03:00
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
  _publicize_job_id: '23643729551'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/06/08/nasil-calisir-base64/"
---
[youtube https://www.youtube.com/watch?v=0VytdQcCc7A]

Bu konumda/videomda yazılımlarımızda sıkça kullandığımız Base64 adlı arkadaşın nasıl çalıştığını anlattım.  
Öncelikle bu arkadaşın amacı binary datayı alıp sadece ASCII karakterlerin kullanıldığı ortama iletmek saklamak.

Peki bu işi nasıl yapıyor ?  
Öncelikle binary datayı alıp 6 bitlik bloklar haline getiriyor.Yeri gelmişken hemen söyleyelim Base64’te çıktı karakter 4 ve 4’ün katı halinde olmalı.Bu sebepten dolayı bloklaşma sırasında eksik bitler olabileceğinden 6 bit olana kadar 0 ile doldurulur. 000000’lık blok base64’te "=" işaretine denk gelir.Natural Char. olarak adlandırılır.

6 Bitlik bloklar decimal’a çevrilir. Elde edilen bu decimal türünden veriler ise aşağıda yer alan Base64 Tablosundan karakterlerle eşleştirilerek şifrelenmiş veri elde edilir.Bu işlemler ile binary datamız ASCII karakterlere dönüştürülür.

![]({{ site.baseurl }}/assets/images/2016/06/7vgpzN.png)

