# Kantifikasyon: Salmon okumaları nasıl sayar?

!!! question "Bu rehberde"
    Milyonlarca anonim okumanın genlere nasıl dağıtılıp sayıldığını anlayacak ve ilk quant.sf dosyanızı okuyacaksınız. Adaptör bulgumuzun faturasını da burada göreceğiz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/04_salmon.ipynb){ .md-button .md-button--primary }

## Sorun: anonim parçalar

İkinci rehberde görmüştük: dizileyici genleri okumaz, 76 harflik anonim parçalar okur. Elimizdeki soru "hangi gen ne kadar ifade edilmiş" ama elimizdeki veri milyonlarca imzasız cümle parçası. Bu ikisinin arasındaki köprüye kantifikasyon deniyor ve bizim aracımız Salmon.

## Salmon'un kurnazlığı: kitap yerine dizin

Klasik yol, her okumayı koca genomda harf harf hizalamaktır; doğru ama hantal. Salmon daha akıllı bir yol izler. Önce transkriptomu alır: genomun tamamı değil, hücrenin üretebildiği bilinen bütün mRNA'ların kataloğu; bizde GENCODE'un fare kataloğu, yüz elli bine yakın transkript. Sonra bu katalogdan bir indeks kurar: kitabın arkasındaki dizin gibi, katalogdaki her 31 harflik parçanın (k-mer) hangi transkriptlerde geçtiğini önceden çıkarır. Artık her okuma geldiğinde Salmon harf harf aramaz; okumanın 31'liklerini dizinden bakar ve uyumlu transkriptlere yerleştirir.

Peki bir okuma birden çok transkripte uyarsa? Ki hep uyar; aynı genin izoformları aynı ekzonları paylaşır. Salmon bunu istatistikle çözer: bütün okumaların dağılımına bakıp "bu transkript şu kadar bol olsaydı bu okumaları hangi olasılıkla üretirdi" hesabını döngüyle inceltir ve okumaları olasılıkla paylaştırır. quant.sf'deki NumReads sütununun küsuratlı olmasının sebebi bu: 2437,8 okuma, "paylaştırılmış pay" demektir, sayım hatası değil.

## quant.sf nasıl okunur?

Her örnek için bir quant.sf çıkar: satırlar transkript, sütunlarda uzunluk, TPM ve NumReads. Şimdilik NumReads'e bakın; TPM'in ne olduğu ve neden ham sayıyla karıştırılmaması gerektiği normalizasyon rehberinin konusu. Bir de aux_info klasörü var: Salmon'un günlükleri. İçindeki eşleşme oranı, verinizin sağlık karnesidir; düşükse bir şey anlatıyordur ve bizim veride anlattığını artık biliyorsunuz: adaptör kuyrukları referansta karşılık bulamaz. Defterde WT ile KO'yu yan yana sayıp bu farkı kendi gözünüzle ölçeceksiniz.

Benim koşumun sonuçları, karşılaştırma için: 1 milyonluk alt kümelerde WT_1 %66,7, KO_1 %50,2 eşleşti; tam verideki tabloyla (%67,9 ve %48,8) neredeyse aynı. Yani adaptör faturası alt kümede de aynen kesildi; FastQC'de gördüğümüz kirlilik, sayım aşamasında yaklaşık on altı puanlık eşleşme farkına dönüştü. Bir gözlem daha: iki örnekte de en çok okuma alan transkriptler büyük ölçüde aynı kodlar (ENSMUST00000082402 gibi); bunlar fetal karaciğerin en gürültülü konuşan genleri ve kim olduklarını sayım tablosu rehberinde gen adlarına çevirince göreceğiz.

## Dürüstlük köşesi

İki sınırımızı bilerek çalışıyoruz. Birincisi, indeksimiz decoy'suz: en iyi uygulama, transkriptoma genomu da "yem" olarak eklemektir ki transkriptoma benzeyen ama transkript olmayan bölgelerden gelen okumalar yanlış sayılmasın; Colab'ın belleği yetmediği için eklemedik, etkisi bu veri için küçüktür ama var. İkincisi, adaptör kırpması yapmadık: hizalama tabanlı klasik akışlarda kırpma standarttır; Salmon'un seçici hizalaması adaptörlü kuyruklara kısmen dayanıklıdır ve bedeli eşleşme oranında ödedik, sayımların tutarlılığını da Klf1 kontrolüyle doğruladık. İkisi de bilinçli takas; takasını bilmeden araç kullanmayın.

## Kendin dene

Defterdeki görevler: üçüncü örneği sayın, Salmon'un kütüphane tipi kararını (lib_format_counts.json) bulun ve en çok okuma alan transkriptleri not edin. Bir de düşünme sorusu: 1 milyonluk alt kümenin eşleşme oranı, tam verininkiyle neden birebir aynı çıkmaz? İpucu: alt kümemiz dosyanın neresinden alınmıştı?
