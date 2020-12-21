---
layout: post
title: Diffie-Hellman MitM
date: 2016-08-25 00:08:01.000000000 +03:00
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
  _publicize_job_id: '26115211660'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/08/25/diffie-hellman-mitm/"
---
Selamun Aleyküm

Bu konumda Diffie-Hellman anahtar değişimine nasıl "ortadaki adam saldırısı" gerçekleştirilir bundan bahsedeceğim.

DHKE yapısı itibari ile basit bir üslü kuralına dayanır. Sistemin prensibi basit ancak geri dönüşümü zor olduğundan güvenlidir.

Saf bir DHKE prensibini kullanan sistemde nasıl anahtarcıkları alırız yahut anahtar yerine sır paylaşanların sırlarına el atarız bunu anlatalım.

DH mantığı şu şekildedir.  
Ortak asal modül ve üreteç seçildikten sonra(n ve g olsun) A ve B kişileri özel anahtarlar belirlerler.(x ve y olsun)

A kişisi B'ye g^x mod n'i yollar.  
B kişisi A'ya g^y mod n'i yollar.

Ardından A kişisi B'den aldığı veri üssü anahtarının mod n'ini alır.  
B'de aynı şekilde A'daki verinin üssü kendi anahtarının mod n'ini alır.

Sonuç olarak aynı veriyi elde ederler.  
Mantıken kanalı dinleyen biri private keyleri bilmediğinden birşey elde edemez ancak DİKKAT. Dinlerse elde edemez. Kanalı manipüle edip kanala dahil olursa işler değişecek ve aşağıda gördüğünüz olay ortaya çıkacak.

![0yljyb]({{ site.baseurl }}/assets/images/2016/08/0yljyb.png)

&nbsp;

Yani saldırgan algoritmaya ellemeden sadece algoritmaya aracı görevinde girer ve A ile B imiş gibi anahtar değiştirirken B ilede A imiş gibi anahtar değiştirir ve artık otomatik olarak dinleyici görevinde iken tüm anahtarlar kendisinde olduğundan istediğini elde edebilir.

Konumuz bu kadar :)  
İyi Günler  
Kağan IŞILDAK

