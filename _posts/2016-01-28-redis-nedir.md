---
layout: post
title: REDİS NEDİR ve UYGULAMALI ANLATIM
date: 2016-01-28 22:35:53.000000000 +02:00
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
  _publicize_job_id: '19224686963'
  _thumbnail_id: '11'
author:
  login: kaganisildak
  email: kaganisildak@outlook.com
  display_name: kaganisildak
  first_name: Kağan
permalink: "/2016/01/28/redis-nedir/"
---
[vimeo 153282749 w=500 h=282]

**REDİS Nedir ?**

REDİS saf sürüm olarak Linux üzerine yazılmış açık kaynaklı NoSQL(NoSQL : Şemasal olarak “ilişkisel olmayan” verileri depolayan veritabanı sistemlerine verilen ad.NoSQL tam anlamıyla “SQL Kullanılmıyor” değil “Sadece SQL Kullanılmıyor” anlamına gelen “not-only-SQL” anlamında kullanılıyor.) yazılımıdır.  
REDİS,Client’ten aldığı verileri önbelleğe alarak depo eder ve istenilirse diske kaydeder. REDİS’i avantaj ve dezavantaj olarak değerlendirirsek verileri bellekte tuttuğundan bellekteki veriyi hızlıca okuyup dönütleyebildiğinden projelerde performans açısından yarar sağlar,verileri aldığı anda diske yazdırmadığından CPU kullanımını aza indirger ve disk kullanımını azaltarak sunucu dostu bir kimliğe bürünür. Bunlar REDİS’in avantajlarından lakin her iyi şeyin kötü yanları olabiliyor. REDİS’in verileri şifreleyerek depo etmemesi ve bağlantı güvenliği açısından ek sistemlerin dahil edilmesine muhtaç olması REDİS’in dezavantajlarından.  
**REDİS’in Kurulumu(Windows)**  
redis.io adresine girdikten sonra üst barda yer alan “Download” butonuna basarak yüklenen sayfada “Other versions” kısmı altında yer alan “Windows” bloğu üzerinde yer alan “Learn more” linkine tıklayarak MSOT tarafından Windows için çevrilen REDİS GitHub sayfasına ulaşabilirsiniz.Ardından yayınlanmış sürüm(releases) bölümünden REDİS’in Windows sürümünü edinebilirsiniz.  
İndirdikten sonra redis-server.exe’yi çalıştırdığınızda Windows sizden bağlantı için güvenlik onayı isteyecek. Onayı verdikten sonra artık REDİS Server’ınız emrinize amade durumda.  
**Konfigürasyon**  
REDİS ayarlarını redis.conf(redis.windows.conf) üzerinde tutmakta.Manuel olarak dosyayı düzenleyip ayarlarınızı yapabileceğiniz gibi REDİS’te bulunan CONFIG komutu ile bu ayarları değiştirme imkanınız var.Herhangi bir ayara ulaşmak için CONFIG GET ayaradi ile ayar bilgilerine ulaşabilirsiniz.Tüm ayarlara ulaşmak için ise CONFIG GET \* komutunu kullanabilirsiniz.Varolan bir ayarı değiştirmek için ise CONFIG SET ayaradi ayardegeri ile ayarı değiştirebilirsiniz.  
**Veri Tipleri**  
REDİS verileri anahtar-değer olarak kaydetmekte. Yani anahtarlar üzerinden verilere ulaşır.Bu verileri kontrol edebilmek için anahtarları kullanmamız gerekir.Bu anahtarları REDİS üzerinde aşağıda olan komutlar ile kontrol edeceğiz.  
•Herhangi bir anahtar oluşturmak ve bu anahtara bir değer atamak için SET komutu kullanılır. SET anahtaradi atanandeger  
•Varolan anahtardaki yüklü değeri yansıtmak için ise GET komutu kullanılır.  
GET anahtaradi  
•Anahtarı silmek için DEL komutu kulanılır. DEL anahtaradi  
•Anahtar var mı yok mu kontrol etmek için ise EXIST kullanılır. EXIST anahtaradi  
Dönüt olarak 1 yada 0 yansıtır. 1 anahtar var, 0 anahtar yok anlamına gelir.  
•Anahtara süre atamak yani süre bittikten sonra otomatik olarak yok olmasını sağlamak için EXPIRE kullanılr. EXPIRE anahtaradi dakika  
•Varolan anahtarları listelemek için KEYS kullanılır. KEYS \*  
Bu komutu KEYS kesisim\* şeklinde kullanarak kesisim’e sahip anahtarları listeleyebiliriz.  
•Anahtarı yeniden adlandırmak için RENAME kullanılr. RENAME eskiad yeniad  
•Anahtarı varolan DB’den farklı bir DB’ye taşımak için MOVE kullanılr.  
MOVE anahtaradi dbindex

Anahtarların genel kullanımları bu şekilde.REDİS bu anahtarlı 5 adet veri tipi ile kullanmamıza olanak sağlyor.Strings  
Hashes  
Lists  
Sets  
Sorted Sets

**-Strings**  
Karakter dizeleridir.Temel veri tipimizdir.REDİS maksimum 512mb’lık bir sting’i depo edebilir.  
•String atamak ve çekmek için SET GET komutları aynı şekilde kullanılmakta.

•Bunun haricinde anahtardaki stringi sınırlara bölerek sınır içindeki veriyi yansıtmak için GETRANGE kullanılır. GETRANGE anahtaradi baslangıc son  
•Anahtardaki eski değeri dönütleyip yerine yeni bir değer atamak için GETSET kullanılır. GETSET anahtaradi yenideger  
•Anahtarımız var ve uzunluğunu öğrenmek istiyoruz STRLEN komutunu kullanacağız. STRLEN anahtaradi  
•Anahtarı baştan değiştirmek yerine son kısmına yeni veriyi girmek için APPEND kullanılır. APPEND anahtaradi eklenecekdeger  
•Tek satırda birden fazla anahtar ve değer eklemek için MSET kullanılır.  
MSET anahtar1 deger1 anahtar2 deger2  
•Birden çok anahtarın değerini dönütlemek için MGET kullanılır.  
MGET anahtar1 anahtar2  
**-Hashes**  
Birnevi bloklama işlemidir. Haritalama olarak adlandırabiliriz. Birden fazla değerimiz ve anahtarımız var. Bu anahtarları tek tek ekleyip karmaşıklaştırmak yerine düzenli bloklar halinde hashlerde haritalarını oluşturabiliriz. Kısaca anahtar içinde ufak anahtarlar oluşturmamıza olanak sağlar.  
•Temel olarak hash oluşturmak için HMSET kullanılır.  
HMSET anahtaradi belirtec1 “deger1” belirtec2 “deger2”  
•Hash içindeki ufak anahtardaki değere ulaşmak için HGET kullanılır.  
HGET anahtaradi belirtecadi  
•Hash içindeki tüm belirteçleri listelemek için HGETALL kullanılr.  
HGETALL anahtaradi  
•Hashi silmek için ise HDEL kullanılır. HDEL anahtaradi

**-Lists**  
Listeler de REDİS tarafından desteklenmekte.  
•Liste oluşturmak ve içine eleman eklemek için LPUSH komutu kullanılır. LPUSH anahtaradi eleman  
•Liste içindeki elemanları listelemek için LRANGE’yi kullanıyoruz.  
LRANGE anahtaradi baslangıc son  
•Listede herhangi bir sıradaki elemanı dönütlemek için LINDEX kullanılır. LINDEX anahtaradi sira  
•Listede seçilen elemanın önüne yahut ardına eleman eklemek için LINSERT kullanılır.  
LINSERT anahtaradi [BEFORE/AFTER] “eleman” “eklenecekeleman”  
**-Sets**  
•Kümler.Küme kullanacaksak önce küme oluşturmamız gerek.Komutumuz SADD. SADD anahtaradi eleman  
•Kümedeki elemanları sıralamak için SMEMBERS kullanılır.  
SMEMBERS anahtaradi  
•Bir kümede bulunan elamanı başka kümeye aktarmak için SMOVE kullanılır. SMOVE anahtar1 anahtar2 “elemanadı”  
•Bir küme ile diğer kümeyi kıyaslamak ve ilk yazdığımız kümede olup diğer kümede olmayan elemanları yansıtmak için SDIFF kullanılır. SDIFF anahtar1 anahtar2

**-Sorted Sets**  
•Sıralı kümeler.Eklenen elemana sayısal değer iliştirmemiz mümkün.Sıralı bir küme oluşturmak ve eleman eklemek için ZADD komutu kullanılır. ZADD anahtaradi skor “eleman”  
•Kümedeki elemanları listelemek için ZRANGE kullanılır.  
ZRANGE anahtaradi baslangıc son [WITHSCORES] -\> WITHSCORES ile elemanlara ait skorlar da listeye dahil edilir.  
•Elemana ait skoru yansıtmak için ise ZCORE kullanılır.  
ZSCORE anahtaradi elemanadi

**PUB/SUB**  
Publish-Subscribe anlamına gelir.Açıklamak gerekirse yayınlayan ve takip eden bulunur. PUB kanala mesajı yolladığı anda kanalı takip eden SUB mesajı görür. PUB/SUB işlemi SUB’lara iletildikten sonra görevini yerine getirmiştir ve REDİS server tarafından kaydedilmez.  
•REDİS’te bir kanala mesaj yollamak için PUBLISH komutu kullanılır.  
PUBLISH kanaladi mesaj  
•Kanalı takip etmek ve gelen mesajları yansıtmak için SUBSCRIBE kullanılır.SUBSCRIBE kanaladi

**Multi İşlem**  
REDİS sıralı komutları MULTI adı altında toplamış, toplamak demeyelim de MULTI komutunu yazdıktan sonra REDİS bizi dinlemeye alıyor ve girdiğimiz komutları sıraya koyuyor.Komutlarla işimizi bitirdikten sonra EXEC diyerek komutlarımızı başlatabiliriz.  
Örnek olarak :

> MULTI  
> SET redis test  
> GET redis  
> EXEC

&nbsp;

**Bağlantı ve Güvenlik**  
•REDİS’te bağlantı esnasında işlem yapabilmek için yetkili şifresi girmek gerekiyorsa yapmamız gerekenler basit.Öncelikle ayarlarda bulunan “requirepass” CONFIG SET requirepass “sifre” şeklinde değiştirilir.Ardından yeni bir işlem esnasında yetkili olabilmek için AUTH kullanılır.AUTH sifre  
•Bulunan DB’den farklı bir DB’ye geçmek için SELECT kullanılır.  
SELECT dbindex  
•Server’ın dönüt hızını yahut aktif halde olup olmadığını anlamak için PING komutu kullanılır.Dönüt PONG :)  
•QUIT ile bağlantımızı sonlandırıyoruz.

**SERVER**  
•Veriler, bağlantılar, güvenlik anlattık lakin Server ile ilgili de komutlarımız yok değil.  
Açık bir server ve bağlı kişileri görmek istiyoruz. CLIENT LIST ile bağlı clientleri dönütleyebiliriz.  
•Server’a bağlandınız ve kendinize bir ad adamak istiyorsunuz.  
CLIENT SETNAME isim şeklinde bağlantınızı adlandırabilirsiniz.  
•Bağlantınızı adlandırıp adlandırmadığınızı hatırlamıyorsunuz yahut şuan kayıtlı adınızı öğrenmek istiyorsanız CLIENT GETNAME işimizi görecek.  
•Veriler depolandı. Peki ne kadar anahtar kayıtlı. Bunun için ise DBSIZE komutunu yazmak ve beklemek kalıyor(mecaz olarak beklemek yoksa REDİS tak diye yansıtır :))  
•Tüm DB’lerin yok olma vakti geldi ise FLUSHDBALL, sadece bulunduğunuz DB’yi silmek için FLUSHDB, girişini yaptığınız anahtarları diske kaydetmek için SAVE komutunu, bu işlemi arkaplanda gerçekleştirmek BGSAVE komutunu kullanabiliriz.  
•Server hakkında bilgi edinmek için INFO, en son kaydın ne zaman yapıldığını öğrenmek için LASTSAVE(dönüt Unix tarihi olarak çıkacak online olarak convert edebilirsiniz), server saatinin güncel olup olmadığını kontrol etmek için TIME, server’ı kapatmak için SHUTDOWN [SAVE/NOSAVE] kullanılır.

