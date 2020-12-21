---
layout: post
title: EasyHook ile Windows API Hooking
date: 2019-01-24 13:20:53.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Genel
tags:
- ".net api hooking"
- api hooking
- easyhook
meta:
  _thumbnail_id: '580'
  timeline_notification: '1548325257'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '26874371854'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
  last_name: IŞILDAK
permalink: "/2019/01/24/easyhook-ile-windows-api-hooking/"
excerpt: Selamlar herkese. Bu yazıda birazcık API Hooking denen şeyin ne olduğundan
  ve ardından EasyHook kütüphanesini kullanarak .NET ortamında API Hooking'in nasıl
  yapıldığından bahsettim. Çerezlik bir şey. Güzel gider.
---
Selamlar herkese. Bu yazıda birazcık API Hooking denen şeyin ne olduğundan ve ardından&nbsp;[EasyHook](http://easyhook.github.io)&nbsp;kütüphanesini kullanarak .NET ortamında API Hooking'in nasıl yapıldığından bahsettim. Çerezlik bir şey. Güzel gider.

## API Hooking?

Windows üzerinde çalışan uygulamalar sahip oldukları fonksiyonları çalıştırırken genelde direkt olarak işletim sistemi üzerinden system call yaratmazlar. Fonksiyonlar öncelikle Win32 DLL'lerine çağrıyı yapar ve ardından bu DLL'ler kalan kısmı üstlenir. ![indir (2)]({{ site.baseurl }}/assets/images/2019/01/indir-2.png)

Kabaca şöyle bir yapı işte.(Detaysız, çirkin, leş bir flowgraph biliyorum ama mevzu bu değil)

Misal C#'ta process oluşturmak için&nbsp;[Process.Start](https://docs.microsoft.com/tr-tr/dotnet/api/system.diagnostics.process.start?view=netframework-4.7.2#System_Diagnostics_Process_Start)&nbsp;fonksiyonunu kullanıyoruz. Bu fonksiyon önce gidiyor Kernel32.dll üzerinden&nbsp;[CreateProcessW](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessw)fonksiyonunu çağırıyor, ardından o fonksiyon NTDLL üzerinden NtCreateUserProcess'i çağırıyor.

Hooking dediğimiz işlem bu fonksiyon çağrılarının arasında bulunup ilgili fonksiyonu incelemek, yönetmek, yönlendirmek, sonlandırmak... Canımız ne isterse kısaca. Anlamı karşılıyor. Kancalıyoruz.

Bu hooking işini ikiye ayırabiliriz.

- Local hooking - Hedef bir process'in kullandığı kütüphanelerdeki fonksiyonları hooklamak
- Global hooking - Tüm sistem process'lerini etkileyecek şekilde hooklamak

Bu yazıda local hooking'i örneklendireceğim. Diğer yazılarda global hookinge bakarız.

## EasyHook

Windows'ta hooking yaparken işi kanser hala getirmemek açısından kütüphane kullanmak gerekli bence. Bu iş için ise güzel kütüphaneler mevcut.

Microsoft'un [Detours](https://github.com/Microsoft/Detours)'u,&nbsp;[mhook](https://github.com/martona/mhook),&nbsp;[MinHook](https://github.com/TsudaKageyu/minhook),&nbsp;[Funchook](https://github.com/kubo/funchook)&nbsp;ve bu yazıda kullanacağımız&nbsp;[EasyHook](https://github.com/EasyHook/EasyHook)&nbsp;o güzel kütüphanelerden bazıları.

### Neden EasyHook?

Huyum kurusun üşengeç biriyim. Üşengeçlikten mütevellit kolay olan daha çok hoşuma gider. Ufak ufak araştırırken bu kütüphaneyi görünce bir kanım kaynadı, sevdim. Çünkü kendileri native olarak C++'ı destekliyor bunun yanı sıra .NET desteği mevcut. Yani C# ile çatır çatır işinizi görebiliyorsunuz. Vaktini çokça .NET ile geçiren ben için bu büyük bir nimet ve ondan dolayı tercih sebebim oldu. (Native tarafta Detours ve MinHook sevdiklerimden. Onlara da bir şeyler karalarım gibi) Kendisi şuan&nbsp;[@spazzarama](https://twitter.com/spazzarama)'nın ellerinde. Projeyi ilk piyasaya süren Christoph Husse adında bir abi. Bir göz gezdirin derim.

### [The Show Must Go On](https://www.youtube.com/watch?v=t99KH0TR-J4)

EasyHook .NET Framework 3.5 ve üzeri için kullanılabilir durumda. Ufak demomuzu yazmaya başlamadan önce&nbsp;[API dökümantasyonunu](http://easyhook.github.io/api/html/N_EasyHook.htm)&nbsp;açın kenarda dursun.

Kütüphanemiz hooking için iki seçenek tanımlamış. Local ve Remote. Local dediğimiz kütüphanenin import edildiği uygulama içerisinde kullanılan Win32 API'lerini hook etmek.

Remote ise biraz enjeksiyon ile farklı bir process üzerinde kullanılan fonksiyonları hook etmek. Demomuzu remote hooking üzerinden anlatacağım.&nbsp;[Kütüphanenin kendi örneklerini incelemek başlangıç tarafında yardımcı olur.](http://easyhook.github.io/tutorials.html)

Öncelikle 2 adet projemiz olacak. Bunlardan bir tanesi injector, diğeri ise hookingleri barındıran kütüphane.

DLL'imizi diğer process'lere enjekte etmek için minik injector'umuzu yazmaya başlayalım.

Remote hooking yapacağımızı belirttik. Elimizde bir PID olacak ve injector'un temel amacı Hook DLL'imizi bu process'e enjekte etmek.

API dökümantasyonuna baktığımızda&nbsp;[RemoteHooking](http://easyhook.github.io/api/html/Methods_T_EasyHook_RemoteHooking.htm)sınıfının&nbsp;[Inject](http://easyhook.github.io/api/html/M_EasyHook_RemoteHooking_Inject.htm)adlı bir fonksiyona sahip olduğunu görüyoruz. Açıklaması da zaten "ben buyum" diyor.

> Injects the given user library into the target process. No memory leaks are left in the target, even if injection fails for unknown reasons.

```
public static void Inject( int InTargetPID, InjectionOptions InOptions, string InLibraryPath\_x86, string InLibraryPath\_x64, params Object[] InPassThruArgs)
```

![carbon (3)]({{ site.baseurl }}/assets/images/2019/01/carbon-3.png)

Örnek kodumuz bu şekilde. Normalde benim amacım hook işleminden sonra bir bilgi almadan işlemin bitmesiydi ancak kütüphaneye ait örneği incelerken IPC tarafı hoşuma gitti ve buraya ekledim. [IPC](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.remoting.channels.ipc?view=netframework-4.7.2)'nin açılımı Interprocess Communication şeklinde geçiyor. Bağlantıda bulunan açıklamada görüldüğü üzere kendisi aynı fiziksel makinede bulunan process'lerin birbirleriyle HTTP ya da TCP kullanmadan haberleşmesini sağlayan bir sistem. EasyHook'da bu protokolu kullanmamıza imkan sağlıyor. kısmını unutmayalım az sonra orda ne yaptığımızı anlatacağım. IPC Server'ı yarattıktan sonra hooking'i yapacak kütüphanemizin dosya yolunu tanımlayıp Inject fonksiyonu ile enjeksiyonu başlatıyoruz. Hooking kütüphanemiz x64/x86 farketmez şekilde derleneceğinden fonksiyonu çağırırken o kısımlara aynı atamayı yaptık. Son olarak verdiğimiz channelName adlı IPC server ismi InPassThruArgs&nbsp;kısmına gidiyor. Burası aldığı objeleri enjekte ettiği DLL'in EP'sine veriyor. IEntryPoint&nbsp;şeklinde tanımlanmış. EP için atanmış sınıf içerisinde bulunan Run&nbsp;fonksiyonuna ve .ctor'apaslıyor.

Ardından konsol uygulamasında olduğum için ve IPC Client'ımız olan hooking dll'inden gelen verileri dinlemek için ReadKey'i bastım geçtim.

Burada injectorumuzu bitirdik ve sıra geldi hookinge.

Class Lib. olarak tanımlanmış projemizde totalde 2 adet sınıfımız bulunacak. Bunlardan bir tanesi IPC için, bir tanesi de hooking işlemi için.

IPC için injector'de HookServer adı ile çağırmıştık.(Kodu yazarken orada ref attığımızdan injectoru yazarken bu kodu yazmanız gerekiyor zaten niye sonraya bıraktım ben de bilmiyorum neyse.)  
 ![carbon (4)]({{ site.baseurl }}/assets/images/2019/01/carbon-4.png)

HookServer'a ait kodlar bu şekilde. IsInstalled fonksiyonu enjeksiyon işleminden haberdar olmamız için. ReportMessage fonksiyonu ise hooking sınıfımızın "kancaladığı" fonksiyonların kullanılma durumuna göre verileri server tarafına iletmek için.

Gelelim hookinge. Öncelikle buradaki temel olay native olan Win API'i .NET ortamında normal olarak tanımlamak. Sonrasında gene bu fonksiyonu çağıracak şekilde tasarlanmış hook edilecek fonksiyonu yazmak. Son olarak EasyHook fonksiyonlarını kullanarak sonradan tanımladığımız "kanca başı" fonksiyonu aktifleştirmek.

Ben bu örnek içerisinde CreateProcessW&nbsp;fonksiyonunu hook eden ufak yapıyı göstereceğim. Zaten iskelet yapısı anlaşıldıktan sonra geriye bir şey kalmıyor. Sadece fonksiyonları tanımlamak gerekli.

CreateProcessW fonksiyonu Kernel32.dll içerisinde yer almakta. Kendileri adından anlaşılacağı üzere yeni bir process create etmek için kullanılıyor.

![carbon (5)]({{ site.baseurl }}/assets/images/2019/01/carbon-5.png)

Fonksiyonun aldığı değişkenler bu şekilde.&nbsp;[Fonksiyona ait daha detaylı bilgi için şu adres işin anası zaten.](https://docs.microsoft.com/en-us/windows/desktop/api/processthreadsapi/nf-processthreadsapi-createprocessw)&nbsp;Başka fonksiyonları eklerken eğer WinAPI'leri , syscall tablolarını ezberlemiş bir psikopat değilseniz illa bakacaksınız kaçarı yok.

Gelelim bu arkadaşı C#'ta tanımlamaya. .NET o kadar güzel bir şey ki hep mutlu ediyor beni. Canımız .NET'imizde dinamik olarak bir DLL tanımlamak isterseniz [System.Runtime.InteropServices](https://docs.microsoft.com/tr-tr/dotnet/api/system.runtime.interopservices?view=netframework-4.7.2)&nbsp;içerisinde DllImportAttribute&nbsp;adında bir sınıf bulunmakta. Bu sınıf ile herhangi bir DLL'i omuzlayıp içerisinde bulunan fonksiyonu çağırabiliyorsunuz.

![carbon (7)]({{ site.baseurl }}/assets/images/2019/01/carbon-7.png)

C#'ta ilgili fonksiyonu şu şekilde tanımladık. Şimdi burada fonksiyondan fonksiyona alınan paremetreler felan değiştiğinden her seferinde oturup baştan fonksiyon tanımlamak belli bir süreden sonra kanser edebilir. Yani zaman kaybı gibi. Bu şekilde düşünen bir yüce beyin&nbsp;[pinvoke.net](http://www.pinvoke.net)&nbsp;adında bir şey yapmış. Sayfayı ziyaret ettiğinizde şükürler olsun dersiniz bence. Ben bulduğumda baya sevinmiştim. Burada WinAPI'lerin .NET için tanımlanmış hallerine erişebiliyorsunuz. Ek olara VStudio eklentisi de var o da kullanışlı. Burdan fonksiyonların hazır tanımlanmış hallerine ulaşabilirsiniz.

Fonksiyonun asılhalini tanımladık. Sıra geldi bunu hook edecek fonksiyonu yazmaya.

![carbon (8)]({{ site.baseurl }}/assets/images/2019/01/carbon-8.png)

EasyHook ile bir fonksiyonu hook ederken LocalHook'u kullanıyoruz. LocalHook içerisinden çağırdığımız [Create](http://easyhook.github.io/api/html/M_EasyHook_LocalHook_Create.htm)fonksiyonu 3 adet paremetre almakta. İlki hedef fonksiyonun adresi, ikincisi hook sonrası kullanılacak fonksiyonun temsilcisi(delegate), üçüncüsü ise callback'i alacak olan obje.

Resimde tanımlanmış 2 şey bulunuyor. Temsilci fonksiyonumuz(CreateProcessW\_Delegate) ve hook içerisinde kullanılacak olan fonksiyon(CreateProcessW\_Hooked).

Hooked fonksiyonunu incelediğimizde yapmak istediğimiz iş gereği aynı paremetreleri alıyor ve ardından orijinal fonksiyonu çağırıyor. Sonrasında ise IPC Server'a aldığı paremetrelerin bir kısmını bildiriyor. Burada orijinal fonksiyonu akışın düzgün devam etmesi için bilerek çağırdık. Amacımız fonksiyonu loglamak olduğundan bir manipülasyon yapmadık.

Son olarak orijinal fonksiyonun döndüğü değer returnleniyor.

Hook içerisinde kullanılan fonksiyonu da yazdığımıza göre artık DLL'in ana tetikleyicilerini yazma vakti geldi.

![carbon (9)]({{ site.baseurl }}/assets/images/2019/01/carbon-9.png)

İlk olarak IPC için hazırlamış olduğumuz sınıfı burada tanımlıyoruz.

Ardından constructor'u(.ctor) ve Run fonksiyonunu tanımlıyoruz. Bunları tanımlarken ek bir paremetre olarak channelName'i ekledik. Sebebi ise evvelden bahsettiğimiz enjeksiyon işleminde alından paremetre bu iki arkadaşa paslanıyordu.

.ctor içerisinde client olan biz denizi tanımlamış olduğumuz server'a bağlıyoruz.

Run içerisinde önce server'ı bilgilendirmek için load olduk mu olmadık mı bir kontrol ettiriyoruz.

Ardından hook yaratma işlemi için bahsettiğim fonksiyon çağırma işlemi LocalHook üzerinden gerçekleştiriliyor.

Daha sonra tanımlanan hook aktifleştiriliyor, server gene bilgilendirildikten sonra enjekte olduğumuz uygulama devam ettiriliyor.(Enjeksiyonda bir suspend bulunduğundan bu WakeUp yapılıyor)

Sonsuzluğa gene giriyoruz ve bitmez tükenmez bir döngü açıyoruz. Bunun sebebi hook işleminden sonra fonksiyonun ayakta kalması çalışır durumda olması ve böylece IPC üzerinden iletimi sağlaması.

Daha sonrasında hook'u kaldırıp bu işlemi yayıyoruz.

Kütüphane ile alakalı anlatacaklarım şimdilik bu kadar. Başlangıç için yani. Gene üzerine bol bol yazıp çizerim. Sıra geldi demoya. İzleyelim efenim.

[youtube https://www.youtube.com/watch?v=QlOaGjLm438&w=560&h=315]

Hooked içerisindeki ufak bir değişiklikle şöyle de yapabiliriz.

[youtube https://www.youtube.com/watch?v=uB1PakAXprY&w=560&h=315]

[Örnek projeye burdan ulaşabilirsiniz.](https://github.com/kaganisildak/EasyHookSample)

