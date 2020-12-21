---
layout: post
title: Doğru Yeraltı Türbesi - Kripto400
date: 2016-05-17 19:10:56.000000000 +03:00
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
  _publicize_job_id: '22904063235'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2016/05/17/dogru-yeralti-turbesi-kripto400/"
---
En sevdiğim sorulardan biriydi.İpucunda ecnebice diyince "lan öylemi" diyip translate'yi açmam bir oldu.  
TrueCrypt dosyasıydı.Birde SHA1 hashi vermişlerdi.  
c756c99e409dbe1bc0176a212b69ce7e1ebaf37d = dkhoctf

Düşündüm durdum.Açtım TrueCrypt'ı deniyorum deniyorum yanlış diyor.DKHOCTF'nin çilekeş Twitter hesabına dm attım."Dayı şifreyi nasıl bulacaz"  
O efsanevi kelamları görünce indir indir nidalarıyla rockyou wordlisti indirmem bir oldu.Ama gene işler tuhaftı.Milyon küsür şifre.Hangi makina ile deneyecektik.Gene dm'yi jet hızında attım ve butterfly dediler :D  
Hemen notepad++'dan bookmark line'larımı ayrı dosyaya aktardım.Gene fazla şifre vardı.TrueCrack desek bekleyemezdim.Hemen jet hızıyla ünlü passcovery suite'yi açtım.Birkaç saniye içinde şifreyi buldu ancaaaaaaaak demo sürüm olduğundan sadece ilk 2 karakteri gösteriyordu.Crack aramaya vakit olmadığından amele usulü !B olanları findleyip tek tek denedim.

Veeeeeee buldum.  
!Butterfly şifremizdi.  
Dosyayı mount edip bir resim elde ettim."Kelebek" :D  
Stego varmı diye tararken biryandan exif bilgisine bakıyordum.Comentte yil ekle yazıyordu.Önce plain olarak "yil" zannederken yıl olduğunu anlayınca tuttum flag girişine abanmaya. dkhoctf16 dkhoctf2016 bas babam bas.Öte yandanda dm'den spam atıyorum olmuyor olmuyor diye.

Bir ipucu daha geldi.Container file ve açılış biçimi diye.O sıralar dışarı çıkmam gerektiğinden her adımımı AKINCILAR takımımıza "itina" ile yazdım :D

Çözdüler tabii ki. TrueCrypt'ta gizli bölmeye erişmek için keyfiles kullanılacaktı.  
Password kısmına dkhoctf16, KeyFiles'a "pepelek" resmimiz eklenecekti.Mount edince flag karşımızdaydı  
flag{kelebekler\_ucar\_gider\_durmaz\_yerinde}

