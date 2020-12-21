---
layout: post
title: Raspberry Pi Mono XSP Sunucu Kurulumu
date: 2016-11-19 19:43:29.000000000 +03:00
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
  _publicize_job_id: '29086710677'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/11/19/raspberry-pi-web-serverda-asp-neti-calistirmak/"
---
Selamlar arkadaşlar.

Bu yazımda Raspberry Pi üzerinde ASP.NET uygulamalarını nasıl çalıştırırız sorusunun yanıtını vermeye çalışacağım.

Microsoft .NET Core'i yayınladı lakin bir problem olmalıdır ki Debian için indiremedim.

Bundan dolayı Mono ile işimizi halletmeye çalışacağız.

> `sudo` `apt-get ``install` `mono-complete`

Komutu ile Mono'yu yüklüyoruz. Kurulum tamalandıktan sonra

> `sudo` `apt-get ``install` `mono-xsp4`

komutu ile Mono XSP sunucusunu kuruyoruz.

İşlemimiz bittikten sonra yayınlamak istediğimiz ASP.NET uygulamamızın bulunduğu klasöre gidip

> xsp4 --port=80

komutunu verdikten sonra test edebiliriz.

