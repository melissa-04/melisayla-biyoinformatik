# FASTQ ve kalite skorları: dosyanın içinde ne var?

!!! question "Bu rehberde"
    Bir FASTQ kaydını satır satır okuyabilecek ve kalite harflerinin ne anlattığını açıklayabileceksiniz. Bu, serinin ilk uygulamalı rehberi; su bugün başlıyor.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/02_fastq.ipynb){ .md-button .md-button--primary }

## Dört satırlık dünya

Dizileyiciden çıkan her okuma FASTQ dosyasında dört satır kaplar. Birinci satır kimliktir, @ ile başlar. İkinci satır dizinin kendisidir; bizim veride 76 harf. Üçüncü satır bir artı işaretidir; tarihsel bir kalıntı, boş verin. Dördüncü satır kalitedir ve işin kalbi orasıdır: her karakter, dizideki aynı konumdaki harfin güvenilirlik notudur.

Yani cihaz size sadece "burada A gördüm" demez; "A gördüm ve bundan şu kadar eminim" der. Bu dürüstlük, sonraki bütün analizlerin temelidir.

## Phred ölçeği: harften olasılığa

Kalite karakterini sayıya çevirmek için ASCII kodundan 33 çıkarırsınız; çıkan sayıya Phred skoru denir. Skorun anlamı logaritmiktir: Q10 "on okumada bir hata", Q20 "yüzde bir", Q30 "binde bir" demektir. Q30 üstü altın standarttır; Q20 altı şüphelidir.

Neden harf? Çünkü her konuma bir karakter düşünce kalite satırı dizi satırıyla aynı uzunlukta kalır; dosya şişmez. Zarif bir mühendislik çözümü.

## Defterde ne yapıyoruz?

Defter iki dosya indiriyor: bir yabani tip, bir knockout. Önce ilk kaydı çıplak gözle okuyoruz; sonra kalite harflerini sayıya çevirip ilk yüz bin okumanın ortalamasını alıyoruz. Ve asıl eğlence: Veri sayfasında bıraktığım gizemi hatırlayın; KO_1 ve KO_3 dosyaları aynı okuma sayısında iki kat büyük, eşleşme oranları da en düşük. İki dosyanın ortalama kalitesini yan yana koyunca ilk ipucu ortaya çıkıyor. Ne bulduğumu buraya yazmıyorum; defteri çalıştırın, kendiniz görün.

## Neler ters gider?

Kalite satırındaki bazı karakterler size tuhaf gelecek: noktalama işaretleri, harfler, rakamlar karışık. Normaldir; ASCII tablosunun 33'ten sonraki her karakteri kullanılır. Bir de eski verilerde (2010 öncesi) 33 yerine 64 çıkarılan bir kodlama vardı; yanlış sabitle çevirirseniz bütün skorlar saçmalar. Bizim veri 33'lü; ama gün gelir eski bir dosya açarsanız aklınızda olsun.

Son ders, kendi payımıza düşen: dosya boyutundan kalite tahmini yapmayın. Sıkıştırma, içeriğin çeşitliliğini ölçer, kalitesini değil; bol N'li tek düze bir dosya küçücük kalırken, çeşitli ve temiz bir dosya şişman görünebilir. Biz bunu kendi verimizde yanlış tahmin edip defterde sayarak öğrendik.

## Kendin dene

Defterin sonundaki iki görev: aynı karşılaştırmayı öbür şişman dosya (SRR384982) için yapın ve dosyalardaki N harflerini sayın; N, cihazın "bu konumu okuyamadım" itirafıdır. Bulduklarınızı not edin. Bir sonraki rehberde aynı sorulara FastQC ile, yani alanın standart aracıyla bakacağız ve gizemi kapatacağız.
