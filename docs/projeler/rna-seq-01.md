# RNA-seq nedir, neden yapılır?

!!! question "Bu rehberde"
    Bu rehberi bitirdiğinizde RNA-seq'in tam olarak neyi ölçtüğünü ve bir deneyin hücreden veri dosyasına nasıl gittiğini kendi cümlelerinizle anlatabileceksiniz. Kod yok; su birazdan başlıyor.

## DNA'ya bakmak neden yetmiyor?

Vücudunuzdaki karaciğer hücresiyle nöronun genomu aynı. Harfi harfine aynı. Ama biri safra üretiyor, öbürü sinyal iletiyor. Fark DNA'da değil; DNA'nın hangi kısımlarının o an *kullanıldığında*.

Ben bunu şöyle düşünüyorum: genom bir kütüphane, RNA ise şu an masalarda açık duran kitaplar. Kütüphane katalogunu ezbere bilmek, içeride bugün ne çalışıldığını söylemez. Merkezi dogmanın özeti de bu zaten: DNA'daki bilgi önce RNA'ya kopyalanır, RNA'dan protein yapılır. Bir gen "ifade ediliyor" demek, o genden o an mRNA kopyaları çıkarılıyor demek. RNA, hücrenin anlık faaliyet raporudur.

Bizim projeden somut örnek: Klf1 knockout farenin DNA'sına bakarsanız yabani tipten tek farkı görürsünüz, silinen o gen. Ama asıl soru bu değil; asıl soru, o gen yokken hücrenin hayatında *nelerin* değiştiği. Bunu size ancak RNA söyler.

## "İfade ölçmek" ne demek?

Bir genin ifadesini ölçmek, o genden o an hücrede kaç mRNA kopyası bulunduğunu saymak demek. Kopya çoksa hücre o proteine yatırım yapıyordur; azsa gen kısılmıştır; hiç yoksa o hücre tipinde kapalıdır.

RNA-seq'in gücü şurada: bu sayımı tek tek genler için değil, hepsinde birden yapar. Deney bittiğinde elinizde yaklaşık yirmi bin satırlık bir envanter olur: her gen için bir sayı, her örnek için bir sütun. Sorular da o zaman karşılaştırmalı sorulabilir hale gelir: knockout ile yabani tip arasında hangi kalemler değişmiş? Bu serinin sonunda çizeceğiniz volkan grafiği, işte o karşılaştırmanın resmi.

## Hücreden FASTQ dosyasına: deneyin hikâyesi

Laboratuvar tarafını kabaca bilmek, veriyi yorumlarken çok işe yarıyor. Hikâye şöyle akıyor:

1. Dokudan ya da hücrelerden toplam RNA izole edilir.
2. RNA'nın büyük kısmı aslında ribozomal RNA'dır ve bizi ilgilendirmez; mRNA'lar seçilir ya da rRNA uzaklaştırılır.
3. RNA kırılgandır; ters transkriptazla daha dayanıklı olan cDNA'ya çevrilir, küçük parçalara bölünür ve uçlarına adaptör denen tutamaçlar takılır. Bu karışıma "kütüphane" denir.
4. Dizileyici, kütüphanedeki milyonlarca parçanın her birinden kısa bir okuma alır; bizim verimizde her okuma 76 harf.
5. Bütün bu okumalar FASTQ denen bir metin dosyasına yazılır. Bilgisayar başındaki işimiz o dosyayla başlar.

Dikkat ederseniz dizileyici genleri okumuyor; anonim kısa parçalar okuyor. "Hangi okuma hangi gene ait ve kaç tane" sorusu tamamen bize, yani biyoinformatiğe kalıyor. Serinin ortası bu soruyla geçecek.

## Bizim deney: bir ustabaşını işten çıkarmak

Bu seride Tallack ve arkadaşlarının 2012'de yayımladığı gerçek bir deneyle çalışacağız: üç Klf1 knockout, üç yabani tip fare fetal karaciğeri. KLF1, eritrosit gelişiminin ustabaşı transkripsiyon faktörlerinden; onsuz fareler doğamadan şiddetli anemiden ölüyor. Deneyin mantığı çok temiz: ustabaşını işten çıkar, fabrikada hangi hatların durduğuna bak. Duran her hat, ustabaşının yönettiği bir hattır. Verinin nereden indirildiği ve kendine has huyları için Veri sayfasına bakın; ben de aynı dosyalarla çalıştım.

## RNA-seq'in ölçmediği şeyler

Yöntemin sınırlarını baştan bilelim, sonda şaşırmayalım. Birincisi, RNA-seq protein ölçmez; mRNA'sı bol olan her genin proteini bol olmak zorunda değil, arada bir çeviri katmanı var. İkincisi, tek bir fotoğraftır; süreç değil an ölçer. Üçüncüsü, bizim yaptığımız "bulk" RNA-seq bir ortalamadır: dokudaki bütün hücre tiplerinin RNA'sı aynı tüpte karışır, azınlıktaki bir hücre tipinin sesi kalabalıkta kaybolabilir. Tek tek hücreleri duymak isteyenler için ayrı bir proje var; sırası gelince oraya da gireceğiz.

## Kendin dene

Kâğıt kalem yeter. Önce kendi alanınızdan, RNA ile cevaplanabilecek ama DNA ile cevaplanamayacak bir soru yazın; "neden DNA yetmez" kısmını bir cümleyle savunun. Sonra tarayıcıdan NCBI GEO'ya girip arama kutusuna GSE33979 yazın; açılan sayfada "Overall design" satırını bulun ve deneyin tasarımını kendi cümlenizle bir yere not edin. O sayfayı okumayı öğrenmek, bu işin gerçek ilk adımıdır; bir sonraki rehberde o sayfadan indirilen dosyanın içini açacağız.

## İleri modüller

Çekirdek yol tamamlandıkça eklenecek derinleşmeler:

WGCNA ile ko-ekspresyon ağları (PyWGCNA) · Alternatif splicing / DTU · miRNA-seq · Bulk dekonvolüsyon (hücre tipi oranları). GSEA 2'nın derinleri
