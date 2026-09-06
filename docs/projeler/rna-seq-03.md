# Kalite kontrol: FastQC raporunu okumak

!!! question "Bu rehberde"
    Bir FastQC raporunu açıp hangi damganın önemli, hangisinin normal olduğunu ayırt edebileceksiniz. Ve iki rehberdir peşinde olduğumuz gizemi kapatacağız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/03_fastqc.ipynb){ .md-button .md-button--primary }

## FastQC ne yapar?

Geçen rehberde kaliteyi elle saydık; öğrenmek için doğrusu buydu. Gerçek hayatta kimse elle saymaz: FastQC, bir FASTQ dosyasını tarayıp on küsur açıdan sağlık raporu çıkaran standart araçtır. Her modüle bir damga vurur: PASS, WARN ya da FAIL.

İşin püf noktası şu: damgalar trafik lambası değildir. FastQC, "rastgele ve tekdüze bir DNA kütüphanesi" varsayımıyla puanlar; oysa RNA-seq rastgele değildir. Bazı genler milyonlarca kopyayla temsil edilir, okumaların başında kütüphane hazırlığından gelen sistematik bir desen olur. Bu yüzden RNA-seq verisinde bazı WARN ve FAIL damgaları tamamen beklenirdir: "Per base sequence content" başlarda hep dalgalıdır, "Sequence Duplication Levels" ifadesi yüksek genler yüzünden sıklıkla FAIL verir. Rapor okumak, damgaları saymak değil, hangi damganın bu deney için anlamlı olduğunu bilmektir.

## Hangi modüller gerçekten önemli?

Benim öncelik sıram şöyle: önce "Per base sequence quality" (kalite konum boyunca nasıl seyrediyor; kuyrukta çöküş normaldir ama nerede başladığı önemlidir), sonra "Adapter Content" (okuma, parçadan uzunsa cihaz parçanın sonundaki yapay adaptör dizisini de okur; bu yapay harfler referansta olmadığı için eşleşmeyi düşürür), sonra "Overrepresented sequences" (bir dizi anormal sıklıkta tekrarlanıyorsa ya adaptördür ya kirliliktir ya da çok ifade edilen bir gendir; FastQC çoğu zaman kim olduğunu söyler). N içeriği ve okuma uzunluğu zaten geçen rehberden tanıdık.

## Gizemin son perdesi

Durum özeti: KO_1 dosyası şişman çıktı, bunun kalite çeşitliliğinden geldiğini kendimiz saymıştık; ama eşleşme oranının neden düşük olduğu açıktaydı. Defterde iki dosyanın raporlarını yan yana koyuyoruz; özellikle adaptör grafiğine ve overrepresented listesine bakın. Ne bulduğumu buraya yazmıyorum; kendi gözünüzle görün, sonra notlarınızı benimkilerle karşılaştırın. Tek söz veriyorum: cevap, "dosya kötüymüş" kadar sıkıcı değil.

## Neler ters gider?

Birincisi, FAIL görüp veriyi çöpe atmak; RNA-seq'te bazı FAIL'ler türün doğasıdır, bağlamsız damga okumak acemi hatasıdır. İkincisi, tersi: her şeye "normaldir" deyip geçmek; adaptör içeriği ve kuyruk kalitesi gibi düzeltilebilir sorunları görmezden gelirseniz faturayı eşleşme oranı öder. Üçüncüsü, raporu tek dosya için okumak; asıl bilgi örnekleri yan yana koyunca çıkar, tek başına "kötü" görünen şey grupça bakınca "hepsinde aynı" çıkabilir ve o zaman hikâye değişir.

## Kendin dene

Defterdeki üç görev: damga tablolarını karşılaştırın, Images klasöründeki öbür grafiklere bakın, aynı analizi SRR384982 için yapın. Sorularınız için not defteri tutmaya başladıysanız (tutun), şunu da ekleyin: sizce bu veride hangi adım bir sonraki rehberde kaliteyi kurtarabilir? Cevabınızı saklayın; kantifikasyon rehberinde Salmon'un bu sorunlarla nasıl başa çıktığını görünce kendi tahmininizle karşılaştırırsınız.
