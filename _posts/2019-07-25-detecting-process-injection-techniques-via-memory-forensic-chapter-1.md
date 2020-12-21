---
layout: post
title: Detecting Process Injection Techniques via Memory Forensic - Chapter 1
date: 2019-07-25 12:29:41.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags:
- doppleganging
- forensic
- hollowing
- injection
- memory
- pe
- PE injection
- rekall
meta:
  _thumbnail_id: '661'
  timeline_notification: '1564046985'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '33250393355'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2019/07/25/detecting-process-injection-techniques-via-memory-forensic-chapter-1/"
---
Merhaba dostlar. Bu yazımda sizlere zararlı yazılımlar tarafından kullanılan "process injection" tekniklerinin bellek analizi ile nasıl tespit edebileceğimizi anlatacağım.

Asıl bölüme geçmeden önce ufak bir hatırlatma yapayım. Yazı içerisinde teknikler hakkında kısa detaylar bulunmakta. Eğer process injection teknikleri üzerine hatrınızda kalmış bir şeyler yoksa yazıyı okumadan önce tekniklerin çalışma yapılarını incelemenizi tavsiye ediyorum.

Tespit etmeye çalışacağımız injection teknikleri:

- Process Hollowing
- Process-Doppelganging

### Process Hollowing

Tekniği basitçe açıklayalım. Hedef aldığı bir uygulamayı önce suspended halde başlatıyor. Ardından uygulamanın asıl bölümünü bellekten "UnMap" edip zararlı olan kısım için bellekte açtığı yere yazma gerçekleştirdikten sonra EP'i yeni bölüme yönlendirip amacını gerçekleştiriyor. Sistem uygulaması olarak listelenen bir process'in içerisinde farklı işler yürütebiliyor.

Gelelim RAM imajı üzerinden bu tekniğin kullanılıp/kullanılmadığının tespitine.

İlk olarak imajıma erişip çalışmış olan uygulamalar bakıyorum.

![pslist1]({{ site.baseurl }}/assets/images/2019/07/pslist1.png)

Normal şartlarda "services.exe" child'i olarak başlatan "svchost.exe"'lerden bir tanesinin "evil.exeollowi" altında çalıştığını farkediyoruz.

"evil.exeollowi" process'ine ait bilgileri sorgulayalım.

![hol1]({{ site.baseurl }}/assets/images/2019/07/hol1.png)

Gelen verilere baktığımızda çağırmış olduğu fonksiyonların "burada bir haltlar dönüyor kesin" sınıfına dahil olan fonksiyonlar olduğunu gördük.&nbsp; Teknikte kullanılan fonksiyonlar bariz şekilde peş peşe dizilmiş. Nereye ne yazmış, neresine yazmış diye sormadan duramıyoruz. Merak işte.

Ardından kendisi ile bağlantılı olan "svchost.exe:884"'e bakıyoruz.

![peinfosvchost]({{ site.baseurl }}/assets/images/2019/07/peinfosvchost.png)

##### Bilgicik : Vakalarda kalabalığı azaltmak ve bu teknikle alakalı şüphelileri ortaya çıkarmak için elinizde bulunan legit process'lere ait GUID'leri, ilgili işletim sisteminde bulunan asıl dosyaların GUID'leri ile karşılaştırarak işleri kolaylaştırabilirsiniz.

PE Info ortada bağırıyor, seçilmiş kişiyim diyor. Legit bir process'e ait bu bilgi ali cengiz oyunlarının döndüğünü bize söylüyor. Bellek içerisinde bir değişim olduğunu da anladık ama detaylandıralım.

Hedefimize ait modüllerin listesi.

![modules]({{ site.baseurl }}/assets/images/2019/07/modules.png)

Process'in kullanmış olduğu kütüphaneler map'lenmiş halde ve çıktıdan da bunu anlıyoruz ancak tuhaf olan durum ise "svchost.exe"'nin dosya yolunun burada bulunmaması.&nbsp; Devam edelim kurcalamaya.

dlllist ile process'imize baktığımızda işler biraz daha şekillenmeye başladı.

![dlllist]({{ site.baseurl }}/assets/images/2019/07/dlllist.png)

"svchost.exe" 0xa60000'da var, oradan yürütülüyor. BaseAdress'ler gene o gülüşü attırıyor. Bu kısımda farklı giden bir şeyler olduğunu biliyorduk ve temel olan noktaya geldik.

Process'e ait VAD(Virtual Address Descriptor)'a yığın çıktısına bakıyoruz.

```
VAD, Windows kernel'ı tarafından process'lere ait bellek yönetimi için kullanılan bir yapı.
```

![vad]({{ site.baseurl }}/assets/images/2019/07/vad.png)

Burada baştan beri takip ettiğimiz tuhaflıklar(injection'lar) silsilesinin çıktısını görüyoruz. Genellikle Windows içerisinde normal şartlarda çalışan bir uygulama, bellekte yer edinirken, kendisinin ve fonksiyon çağıracağı kütüphanelerin sistemde bulunan dosya yollarını da ref ederek "Mapped" halde ve "EXECUTE\_WRITECOPY" izniyle tutmakta. Bellek içerisinde bulunan process'lerin herbirine erişimi olduğun için bir process başka bir process'e ait bellek alanında manipülasyon gerçekleştirebilir. Böyle bir işlem yani belleğe yazma işlemi gerçekleşmişse eğer ilgili bölümün perm.'i "EXECUTE\_READWRITE" olarak belirlenmektedir.

Ekran görüntüsünde de görüldüğü üzere bizim minik "svchost.exe:884" içerisinde değişiklik yapılmış bir alana sahip. İlgili bölüm&nbsp;0xa60000&nbsp; adresinden başlıyormuş. BINGO (İlk 3 adımda zaten almıştık ama neyse)  
dlllist çıktısında gördüğümüz adres ile çakıştı ve üzerinden değişiklik olduğunu tam anlamıyla gördük artık.

İlgili adresten legit uygulama üzerine enjekte edilmiş olan arkadaşı dump edip bu teknik ile alakalı kısmı bitirelim.

![mainprog]({{ site.baseurl }}/assets/images/2019/07/mainprog.png)

###### Minik bir HelloWorl'müş :p

### Process Doppelganging

Kendileri TxF nimetinden yararlanarak Transacted bir dosya yaratıp(İstediğiniz dizinde, istediğiniz adda ve uzantıda), bellekte onca can verip ardından Rollback yaparak dosya işlemi hiç gerçekleşmemiş gibi devam eden ve elimize process veren bir teknik. Süreç Hollowing'den tamamen farklı şekilde ilerlemekte.

Bu tekniğe ait detayları önceden yazmıştım. [Şurdan](https://kaganisildak.com/2019/02/10/process-doppelganging/) okuyabilirsiniz.

İmajın hakkı pslisttir diyerek rekall'dan komutumuzu yolluyoruz.

![rekallps]({{ site.baseurl }}/assets/images/2019/07/rekallps.png)

Process'ler önümüzde ancak bu yöntem diğeri gibi olmadığı için tespit işini tekniği tam olarak anlayarak yapmamız gerekecek.

Doppelgänging :

**Transact -\> Load -\> Rollback -\> Animate**

adımlarından oluşuyor. Transact içerisinde bir dosya oluşturma işlemi mevcut ancak bu işlem TxF kullanılarak yapıldığı için klasik süreçten farklı bir durum söz konusu. TxF'de bir dosya işlemi başlattığınızda süreci sonlandırmazsanız ilgili dosya görünmemekte. Yani kullanabildiğiniz bir dosya yaratabiliyorsunuz ve işiniz bittiğinde işlemi sonlandırmayıp Rollback yaparsanız o orda hiç varolmamış sayılıyor.

Böyle bir dosya yaratılıp içine zararlı olan kısım yazılıyor ve ardından bildiğimiz "NtCreateSection" ile dosya hafızaya yüklenip Rollback yapıyor.

Son olarak "NtCreateProcessEx" ile hafızadaki ayırdığı kısmı işaret edip dosya yoluna gidince hiçlikle karşılaşacağımız ya da bilinçli olarak legit uygulama dizini üzerine yazılmış bir process ayağa kaldırıyor.(Permission farketmeksizin TxF ile istediğiniz dizinde transacted bir dosya oluşturabiliyorsunuz)

Şimdi bu anlattığım olayın Windows içerisinde gerçekleşirken monitorlerimize nasıl takıldığına bakalım.

![procdopp]({{ site.baseurl }}/assets/images/2019/07/procdopp.png)

Tekniği kullanarak bir process başlattık. Process'e ait bilgilere baktığımızda "C:\Windows\yolo.txt" dizinini vermiş ve mevcut klasörü de System32.(Uzantılar da bizim :p)

Bir de teknik bu işlemi gerçekleştirirken arkada neler yapmış ona bakalım.

![doppmon]({{ site.baseurl }}/assets/images/2019/07/doppmon.png)

TxF'siydi,Transacted dosyasıydı dedik durduk. Teknik bu işlemi gerçekleştirirken teorikte hiç oluşmamış olacak bir dosya oluşturabilecek ancak arkada API'ler söz konusu olunca tekniği fişekleyen uygulamanın, enjeksiyon için kullanacağı sahte path'i kendi bulunduğu mevcut klasörde oluşturuyor.

Detay az dedik ama burada detay vermezsek yapacaklarımız anlaşılmaz. Şimdi tekniğin bize sunmuş olduğu verilere bakarak belli başlı çıkarımlar yapacağız.

1. Process, bellekte injection tekniği tarafından kendisine atanmış path'i kullanıyor.(in\_mem)
2. Teknik fonksiyonlarını gerçekleştirirken kullanacağı sahte path için bulunduğu dizinde sahte path'e dahil olan dosya adını kullanarak bir dosya oluşturmakta.(mapped)

Şimdi tekrardan imajımıza dönelim.

Bu teknik için incelemekte olduğumuz vakada, tekniğin kendisi sebebi ile işimiz biraz zordu. Çünkü&nbsp; tek tek tüm process'leri gezip procinfo'lara bakıp bellek dökümlerinde olan bitene bakmamız çok zamanımızı alacaktı. Şimdi işleri kolaylaştıralım.

![mem2]({{ site.baseurl }}/assets/images/2019/07/mem2.png)

##### ldrmodules çıktısı

ldrmodules ile process'lere ait unlinked DLL'leri listeleyebiliyorduk. Bu fonksiyonun çıktılarında ise yüklediği dosyalara ait verileri görebiliyoruz. DLL'in load path'i, bellekte mi, adresi gibi gibi. Burada önemli olan noktalardan bir tanesi ise çıktı içerisinde yüklemiş olduğu modülün bellekte bulunan dizini(in\_mem\_path) ve sistemde dosya olarak map edildiği yerin dizini(mapped) bulunmakta.

Az önce 2 çıkarımda bulunmuştuk ve şimdi de aradığımız şeyler elimizde. ldrmodules komutunu paremetre olmadan çalıştırdığımzıda bize RAM imajının içerisinde bulunan tüm process'lere ait bu verileri getirmekte. Cebimize atalım.

Teknik enjeksiyonu gerçekleştirirken disk işleminde farklı bir dizin(mapped), process yaratma işleminde farklı bir dizin(in\_mem\_path - malum sahte path'imiz) kullanıyordu. Eğer biz elimizde bulunan imajda "in\_mem\_path != mapped"&nbsp;olanları çıkarabilirsek Process Doppelgänging tekniği kullanılarak yaratılmış olan dosyayı bulabilir ve ilgili process'e erişebiliriz. Kısaca RAM'de Doppelgänging tekniği kullanan bir process var mı yok mu öğrenebiliriz?

Öyleyse biraz kod yazalım:

![carbon]({{ site.baseurl }}/assets/images/2019/07/carbon.png)

##### [https://gist.github.com/kaganisildak/2552f009198542982648d91a3fc05b1a](https://gist.github.com/kaganisildak/2552f009198542982648d91a3fc05b1a)

Yazı için örnek olsun diye şöyle bir şey karaladım. FP ürettiğinden şuan için basit kontroller ekledim ancak işi garantiye almak için biraz daha düzenlemem şart ama şuan için mühim değil.

Kodumuz ldrmodules çıktısını alıp parse ediyor ve "in\_mem\_path" ve "mapped" bölümleri birbirine eşit olmayan modülleri ve process'i bize veriyor.

![doppsearcher]({{ site.baseurl }}/assets/images/2019/07/doppsearcher.png)

Ve buraya da bir BINGO.&nbsp;

"Explorer.EXE:2236" elimize düştü. Baktığımızda normal dizininde çalışıyor gibi gözüken "explorer"'nin,&nbsp;Doppelgänging kullanılarak oluşturulduğunu tespit ettik.

### Bitti Gibi

Öncelikle yazımda,anlatımda hata varsa affola. Okuduğunuz için teşekkür ederim. Bölüm 1'imiz burada son&nbsp; buldu. Diğer bölümlerde farklı teknikler için neler yapılabileceğine bakacağız. Öneri,şikayet,eksik gedik varsa düzeltelim dikkate alalım.

İyi Analizler

