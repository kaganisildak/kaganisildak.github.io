---
layout: post
title: Process Doppelgänging
date: 2019-02-10 12:23:30.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags:
- backdoor
- blackhat
- injection
- pe
- PE injection
- Process Doppelgänging
- Process Doppelgänging nedir
meta:
  _thumbnail_id: '572'
  timeline_notification: '1549790614'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '27475866692'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2019/02/10/process-doppelganging/"
---
Selamlar herkese.

Bu yazıda PE injection tekniklerinden biri olan Process Doppelgänging'in teknik detaylarından bahsettim. Çerezlik bir yazı daha.

Blackhat Europe 2017'de enSilo'dan Tal Liberman ve Eugene Kogan "[Lost in Transaction: Process Doppelgänging](https://www.blackhat.com/docs/eu-17/materials/eu-17-Liberman-Lost-In-Transaction-Process-Doppelganging.pdf)" adlı bir sunum yaptılar.

Sunum PE injection için yarattıkları yeni tekniğe ait süreci içeriyor. Öncesinde eski nesil teknikler, ilgili tekniklere ait problemler vs yer almakta. Yazının temel odağı yeni tekniğe ait detaylar olduğundan öncesi ile alakalı kısımlara sunum üzerinden erişebilirsiniz. Detaylı girmem zor gibi. :/

### Nedir bu Process Doppelgänging?

Kendisi temel olarak zararlı bir uygulamayı sistem parçası olan bir uygulama altında çalıştırmayı sağlıyor. İlgili teknik RunPE(Process Hollowing) tekniği ile aynı amacı gütmekte ancak tekniğin çalışma yapısı, kullandığı API'ler farklı olduğundan dolayı RunPE'den ayrılıyor. (Tekniğin kullandığı API'ler farklı olduğundan dolayı ilk ortaya çıktığı zaman antivirüs'ler tarafından tespiti bir hayli zor olmuş. Sunumun son kısımlarında test edilmiş güvenlik ürünleri yer alıyor.)

Bu farklılığı tam anlatmak için Process Hollowing tekniğinin hangi adımlardan oluştuğuna bakmakta fayda var.(Görselleri sunumdan arakladım!)

> #### Process Hollowing ?\_?
> 
> Tekniğin temel amacının sistem parçası uygulama altında zararlı aktivitelere sahip başka bir uygulamayı çalıştırma olduğunu söylemiştik.
> 
> İlk olarak hedef uygulama suspended şekilde başlatılıyor.([CreateProcess](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessa)&nbsp;fonksiyonu ve&nbsp;[Process Creation Flags](https://docs.microsoft.com/en-us/windows/desktop/procthread/process-creation-flags))
> 
> ![g1]({{ site.baseurl }}/assets/images/2019/02/g1.png)
> 
> Ardından&nbsp;[NtUnmapViewOfSection](https://undocumented.ntinternals.net/index.html?page=UserMode%2FUndocumented%20Functions%2FNT%20Objects%2FSection%2FNtUnmapViewOfSection.html)&nbsp;fonksiyonu ile az sonra belleğe taşıyacağımız zararlı için ortamı hazırlıyoruz.
> 
> ![g2]({{ site.baseurl }}/assets/images/2019/02/g2.png)
> 
> Elimizde RAW data halde bulunan zararlımızı Windows üzerinde koşturabilmek için hafızada yerimizi belirlememiz, rezerve etmemiz lazım.&nbsp;[VirtualAllocEx](https://docs.microsoft.com/en-us/windows/desktop/api/memoryapi/nf-memoryapi-virtualallocex)
> 
> ![g3]({{ site.baseurl }}/assets/images/2019/02/g3.png)
> 
> Zararlımız PE formatında ve her bölümü hafızaya yazmamız gerekmekte.[WriteProcessMemory](https://docs.microsoft.com/en-us/windows/desktop/api/memoryapi/nf-memoryapi-writeprocessmemory)
> 
> ![g4]({{ site.baseurl }}/assets/images/2019/02/g4.png)
> 
> Son olarak ilgili process'i devam ettirmeden önce EP'i güncellememiz lazım.&nbsp;[SetThreadContext](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setthreadcontext)
> 
> ![g5]({{ site.baseurl }}/assets/images/2019/02/g5.png)
> 
> Ver elini başlasın, zararlı kodlar fink atsın.
> 
> Teknik bu şekilde.&nbsp;[Şuradan örneklemeye ulaşabilirsiniz.](https://github.com/m0n0ph1/Process-Hollowing)

Şimdi geri gelelim Doppelgänging'e.

Teknik 4 adımdan oluşuyor.

1. Transact
2. Load
3. Rollback
4. Animate

### Transact

[About Transactional NTFS](https://docs.microsoft.com/tr-tr/windows/desktop/FileIO/about-transactional-ntfs)

Teknik bu kısımda TxF denen mekanizmayı kullanmakta. Detayına girsek kolay çıkamayacağımız bu mekanizmanın ve sağladığı API'lerin aslında temel amacı çoklu işlemleri tek ele almak ve dosya güncellemeleri, [DTC](https://support.microsoft.com/tr-tr/help/903944/the-microsoft-distributed-transaction-coordinator-service-must-run-und)&nbsp;gibi durumlar için kullanmak.

Yani işlem içerisinde dosya yarattığınızda işlem dizisini sonuçlandırmadığınız sürece ilgili dosya gözükmez. Devamında ilgili dosyaya normalde olduğu gibi yazım yapabilir ve kullanabilirsiniz.

Teknik bu adımda sistem parçası olan ya da belirtilmiş path içerisinde bir Transacted dosya yaratıyor. Ardından yarattığı bu dosyaya zararlı dosyanın içeriğini yazıyor.(Burada zararlı olan illa dosya olarak bulunmak zorunda değil. İşlem üzerinde zaten byte buffer şeklinde bulunacağından loader'ın kendisinde de saklanabilir. Zevk meselesi.)

### Load

TxF ile hiç varolmamışçasına herhangi bir path üzerinde bulunuyormuş gibi dosya oluşturulmuş ve zararlı uygulama yazılmış dedik. Ardından teknik NtCreateSection ile bu dosyamızı hafızaya yüklüyor.

### Rollback

TxF'in sunduğu nimetlerden birisi de Rollback. İlk başta Transacted bir dosya yaratıldığını söyledik. Bu işlem dizisi yani yaratma süreci Rollback ile geri alınabilmekte. Yani Load'a kadar normal şekilde işlemler giderken Rollback ile işlemi geri alıp işletim sisteminin dosya&nbsp; sisteminde hiç oluşturulmamış gibi davranmasını sağlayabiliyoruz. Teknik aynen bu işlemi yapıyor ve üst taraftan bakınca ortada dosya felan yok kardeşim dedirtiyor.

### Animate

Bu noktada artık teknik oluşturduklarına can veriyor. Load adımında zararlının memory'e aktarıldığını, bir section yaratıldığını söylemiştik. Bu adımda NtCreateProcessEx ile bir process yaratılıyor ve işlem esnasında hafızada oluşturulan section kullanılıyor.

Tekniğin adımları bu şekilde. Klasik olan teknikten farkı da bariz şekilde ortada. Hollowing içerisinde bu dosya işlemleri normal şekilde yapıldığından ve kullanılan API'ler bariz olduğundan(Önce process'in ayağa kalkması ve ardından zararlı dosyanın okunup yazılması gibi gibi) AV'lerin kucağında. Doppelgänging'de TxF nimeti sayesinde dosya sürecinin farklılaşması işin seyrini tamamiyle değiştiriyor.

[hasherezade ablamızın örneklemesi](https://github.com/hasherezade/process_doppelganging)

[3gstudent'a ait örnekleme](https://github.com/3gstudent/Inject-dll-by-Process-Doppelganging)

Üst tarafta bulunan linklerden örnek kodlara ulaşabilirsiniz.

## Demo

[youtube https://www.youtube.com/watch?v=PnMj3yi0tz8&w=560&h=315]

