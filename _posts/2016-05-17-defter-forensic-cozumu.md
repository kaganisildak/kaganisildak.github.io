---
layout: post
title: Defter - Forensic Çözümü
date: 2016-05-17 20:13:24.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  _oembed_0bc42368538708906255b2f633a5410f: "{{unknown}}"
  _oembed_7d7e97326d9321f7ea7ae6f3dc390640: "{{unknown}}"
  _oembed_651329b53c010b836e30d33518e451f6: "{{unknown}}"
  _oembed_a50b8681a385133fcd92318dd1edcb32: "{{unknown}}"
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '22905872018'
  _oembed_ab54de170c0c2e0fc0bebf79cc23a2f6: "{{unknown}}"
  _oembed_c5f4d3bb46c415bf22beccc04fc12141: "{{unknown}}"
  _oembed_84d85ccf6c89c5a7a65e8fc7368bf32c: "{{unknown}}"
  _oembed_2b8f9225c8c60b051d4d8f4682346153: "{{unknown}}"
  _oembed_43e9ff3ee139735d96a0255b9da8d3f2: "{{unknown}}"
  _oembed_e0bb5fd7a67232190f61322779b6fbc4: "{{unknown}}"
  _oembed_f5e0f7148c28e05e830f84b6ab6e6678: "{{unknown}}"
  _oembed_18d032e481db3a7ce6fb7ed45559b81e: "{{unknown}}"
  _oembed_c120e242601006e963ed4c8daafa4f67: "{{unknown}}"
  _oembed_0ec5b9f3470fedb3a0497398e7f53729: "{{unknown}}"
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/05/17/defter-forensic-cozumu/"
---
Soruya baktığımızda nur topu gibi bir uzantısız dosya bizi bekliyordu.  
Hemen checkfiletype.com'u açıp ne olduğuna baktım(notepad'le açıp reg dosyası olduğunu anladım ama detaylar önemliydi)  
WindowsNT reg dosyası. Hemencecik bir reg viewer buldum  
http://www.gaijin.at/dlregview.php

Attım içeri ve girdilere bakmaya başladım.Soruda başlangıç diyordu.Hemencecik Software\>Microsoft\>Windows\>Current Version\>Run'a baktım.  
caesar3 diyordu. Hemen flag girmek için çabaladım.Yanlış dedi.Nete baktım oyun diyor şirket ismi denettim yok.  
O sırada AKINCILAR'dan hızırımız Numan yetişti imdadıma.Run kısmını .reg olarak export ettik.  
İncelediğimizde beklediğimiz karşımızdaydı.Sezarlanmış bir metin.  
Iodj+E4e4q\_exug4\_nd4bg1\_b0n+

Hemen online olarak decrypt ettik  
http://www.braingle.com/brainteasers/codes/caesar.php

Ve flag{Flag+B4b4n\_burd4\_ka4yd1\_y0k+}

