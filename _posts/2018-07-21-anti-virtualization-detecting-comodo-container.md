---
layout: post
title: 'Anti-Virtualization : Detecting Comodo Container'
date: 2018-07-21 01:02:48.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags: []
meta:
  _thumbnail_id: '540'
  timeline_notification: '1532124183'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '20224826376'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2018/07/21/anti-virtualization-detecting-comodo-container/"
---
"There's no escape! Nothing can save you now."

Seto abinin bu güzel sözüyle başlayayım bu minnak yazıma. Aktaracağım bilgiler basit ancak genel olarak paylaşılan anti-analiz yöntemlerinde ve incelediğim çoğu zararlıda göremediğim bir özelliğin nasıl oluşturulabileceğinden bahsedeceğim. Biliyorsunuz güzel siber dünyamızda zararlı yazan abiler ile onlarla mücadele eden kardeşler arasında sonsuz bir "çelınc" var. Müdahale etmeye çalışan kardeşler hedef olan şüpheliyi en kısa sürede ifşa etmeye çalışırken saldıran taraf yarattıkları o sanat eseri zararlıların daha geç ifşa olması amacıyla bir çok modül geliştirmektedirler. Bu modüller kısaca anti-analiz üzerinedir. Okunduğu anda biraz düşünülürse anlaşılabilecek bir tanım ancak azcık daha açıklamamız gerekiyor. Bildiğiniz üzere bir zararlıyı incelerken analist alet çantasında bulunan bir çok aracı kullanmakta. E haliyle malware geliştiren abiler bu "kalleş" analistlerin kendi sanat eserlerini hemencecik ifşa etmemeleri için onların önüne bulabildikleri her cins taşı koymaya çalışıyorlar. Kara listeden process elemek ile başlayıp CPU sıcaklığına kadar bakan analist abilerin bu yaptıkları işleme anti-analiz diyoruz. Sadede geldiğimizde de yapacağımız işlem bu tekniklerin içine minik bir özellik eklemek. O da Comodo'nun Container(sandbox) özelliğini aktif ise tespit etmek.

Eğer sisteminizde Comodo'a ait bilimum bir güvenlik ürünü bulunuyorsa(firewall,antivirus etc) bu container dediğimiz arkadaş aynı şekilde özellik olarak yer alıyor. Kendisi Sandboxie, AVAT Sandbox gibi bir şey aslında. Hedefi bir kum havuzunda oynatmak ve böylece ana sisteme bir şey olmadan o izole alanda hedefi barındırmak. Bireysel kullanıcıları zararlılardan çok güzel korumakta, analist kardeşler de izole alan buldun yapış kafasında zaman zaman bunun içerisinde inceleyebilmekte. Amacımız analistin işini zorlaştırmak olduğundan tekniklerimizin bol olması hiç problem değil. Yazıyı yazmamın sebebi ise Al-Khaser, Pafish gibi araçlarda böyle bir kıstasın bulunmaması.

Gelin ufaktan bakalım ne var ne yok.

![Screenshot_3]({{ site.baseurl }}/assets/images/2018/07/screenshot_3.png)

Normal olarak bir yazılımı başlattığımızda Comodo tarafından sadece guard64.dll enjekte edilmekte ve bu arkadaşın izole ortam ile bir alakası bulunmamakta.

![Screenshot_2]({{ site.baseurl }}/assets/images/2018/07/screenshot_2.png)

Ancak hedef Container içinde çalışıyorsa Comodo bu izolasyon işlemleri için cmdvrt64.dll'i enjekte etmekte.(Hedefin/sistemin mimarisine göre x86/x64 dosya ismine etki etmektedir)

İşin tamamı kolaydı, asıl indikatörü de bulunca her ışıl ışıl her yer, her yer sanki pavyon oluyor ve yapılması/yapmanız/yapmamız gereken tek şeyin basit bir kontrol işlemi olduğunu söyleyip örnek kodumuzu yazıyoruz. ![Screenshot_4]({{ site.baseurl }}/assets/images/2018/07/screenshot_4.png)

C#'ta yardımımıza "Process" 'imiz koşuyor, siz "eğitimsel" ve "deneysel" amaçlı zararlı yazılımlarınıza benzeri şekilde bir test özelliği ekleyebilirsiniz :P

![Screenshot_1]({{ site.baseurl }}/assets/images/2018/07/screenshot_11.png)

Container içinde çalışınca aldığımız çıktı da bu şekilde oluyor.

Diğer yazılarda görüşmek üzereeeee

&nbsp;

