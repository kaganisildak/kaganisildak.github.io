---
layout: post
title: Hadi hoppala vede cuppala - Forensic 300 WriteUp | DKHOS
date: 2018-02-15 03:31:13.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  timeline_notification: '1518654676'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '14756684728'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2018/02/15/hadi-hoppala-vede-cuppala-forensic-300-writeup-dkhos/"
---
Hikayenin sonunda boynuz yediğini öğrendiğimde ağlayasım gelen baş kahramanımız Mahmut Pelinsunun diskini omuzlar ve bir dosya elde eder. Dosyadan flag bulun diyorlar. Dosyayı indirdim baktım sıkıntılı, bozuk bu. Tamir edeceğimizi anladım ve başladım araştırmaya. Dosyayı editörde açtım ve kenara&nbsp;[şu listeyi](https://www.garykessler.net/library/file_sigs.html)çektim. Birazcık bakınma ile 7z olduğunu anlamıştım dosyanın. (¼ şu yardımcı oldu) Biranda dedim çözdük lan basit hemen düzeltirim. Baktım olmadı , gözümden bir damla yaş. Ez flag demek içimde kaldı. Malum Prodaft var başta, dedirtirler mi adama öyle. "Burası sörvayvır"

Neyse takımım ile odaklandık ve araştırmaya koyulduk. Önce klasik ve en saçma hatayı yaptım. Otomatize araç kullanmaya kalkıştım. Kenar mahalle ctflerinde işe yarardı belki ama burada vakit kaybettirdi sadece. Ardından ipucu geldi. Başı sonu bozuk olabilir anlamına gelen bir ipucuydu. Dedim aga tamam altı da düzeltiriz. Düzelttik ama eksik düzelttik. Ardından bir cevher bir beyin parladı, yankılattı tüm grubu. Neden dökümanlara bakmıyoruz. Direkt olarak&nbsp;[http://www.7-zip.org/recover.html&nbsp;](http://www.7-zip.org/recover.html)eriştik buraya. Arkadaşımın uzun denemeleri ve uğraşları sonucu dosyayı orjinaline çevirebildik. Canım ctfciğim uzunlukla oynamış. Yahu sadistmisiniz, kör oldum diyen oldu yapmayın tmm mı ? Neyse dedik geldi flag. Açtık dosyayı. Dedik soru hakkını veriyor. "Hoppala ve de cuppala" derken bulduk kendimizi. Arşivimiz şifreliydi. Genel bir ipucu vardı ya şifre direkt verilir ya da denersiniz diye. CINT 500 ile For400'de direkt almıştık ancak burada bir kaba kuvvet gerekiyordu. Parolanın hakkı rockyou'dur diyerek koyulduk teste. "piggies" çıktı. Ardından gene arkada Bakkal Amca çalar.

Flag =&nbsp;DKHOS\_{4l\_G1rd1n\_g1rd1n}

&nbsp;

