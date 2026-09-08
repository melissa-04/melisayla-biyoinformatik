# Veri mutfağı: PBMC3k

!!! question "Bu rehberde"
    İkinci projenin verisini kaynağından indirecek, hücre × gen matrisini tanıyacak, üç temel kalite filtresini gerekçeleriyle uygulayacak ve serinin arşivlik dosyasını üretip Zenodo'ya taşıyacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/scrna/01_veri_mutfagi.ipynb){ .md-button .md-button--primary }

## Yeni proje, yeni çözünürlük

Birinci projenin son sorusu şuydu: toplu ortalama bizden ne sakladı? Bu proje o sorunun cevabını verecek teknolojiyle, tek hücre RNA-seq ile çalışıyor. Veri olarak alanın klasiğini seçtik: 10x Genomics'in halka açık PBMC3k seti — sağlıklı bir bağışçının periferik kanından yaklaşık 2.700 mononükleer hücre. Klasik olmasının değeri var: dünyadaki hemen her tek hücre eğitimi bu setle başlar, dolayısıyla her adımınızı toplulukla çapraz kontrol edebilirsiniz. Ve kan, "ortalamanın sakladığı" dersini anlatmak için biçilmiş kaftan: T hücresi, B hücresi, monosit, NK — bir tüpte en az yarım düzine bambaşka kimlik.

## Kaynak ve dürüstlük

Birincil kaynak 10x Genomics'in halka açık veri sayfasıdır ve veri CC-BY lisanslıdır. Defter, ders kolaylığı için üç dosyayı (barkodlar, genler, seyrek matris) bir topluluk aynasından indirir; açıklama ve atıf defterin içinde durur. Bu rehberin sonunda ürettiğimiz temiz dosya Zenodo'ya yüklenecek: DOI'li, sürümlü, kalıcı bir adres. 2.2'den itibaren bütün defterler veriyi oradan okuyacak — birinci projede GitHub'a koyduğumuz sayım tablosunun bir sonraki sürüm işi de bu arada kendini yazdı: o da aynı Zenodo kaydına taşınacak.

## Benim koşumun sayıları

Matris 2.700 hücre × 32.738 gen olarak geliyor; gen listesinde 13 mitokondriyal gen var. Hücre başına medyan 2.197 UMI ve 817 gen ölçtüm; medyan mitokondriyal yüzde 2,0. Uçlara bakınca: MT yüzdesi 10'u aşan 6 hücre, 2.500'den fazla gen tespit edilen 5 hücre var — ilki ölmekte olan hücre şüphesi, ikincisi olası çiftlenmiş damlacık (aynı damlacığa iki hücre) şüphesidir; bu sette ikisi de bir avuç. Üç filtre — hücre başına en az 200 gen, gen başına en az 3 hücre, MT% < 10 — matrisi 2.694 hücre × 13.714 gene indiriyor. Çıkan arşiv dosyası `pbmc3k_ders.h5ad`, 20 MB civarı: Zenodo için ideal boy.

Filtrelerin gerekçeleri tek tek: 200 genin altı, hücre değil boş damlacık gürültüsüdür. Üç hücrenin altında görülen gen, tabloyu şişiren hayalet sütundur. Ve MT%: zarı hasarlanan hücrenin sitoplazmik RNA'sı kaçar ama mitokondri içeride kalır; yüksek MT yüzdesi ölmekte olan hücrenin imzasıdır. Eşik evrensel değildir — kalp kası gibi mitokondrisi bol dokularda %10 normaldir; PBMC için yaygın tercih %5-10 bandıdır.

## Neler ters gider?

Üç tuzak. Birincisi, ham ile filtreli matrisi karıştırmak: 10x'in "raw" sürümünde yüz binlerce boş barkod vardır ve yanlışlıkla onu indiren, 2.700 hücre yerine 737 bin satırla boğuşur; biz Cell Ranger'ın filtreli çıktısıyla başlıyoruz. İkincisi, gen adlarına güvenmek: adlar benzersiz değildir, bu yüzden defterde `make_unique` var — birinci serinin altıncı rehberindeki "ad etikettir, kimlik esastır" dersi tek hücrede de geçerli. Üçüncüsü, MT eşiğini evrensel sanmak: eşik dokunun biyolojisine göre seçilir ve raporda gerekçesiyle yazılır.

## Kendin dene

Defterin sonunda üç görev var. Kendi koşunuzdan filtre öncesi/sonrası boyutları ve medyan UMI ile gen sayısını not edin. MT ve yüksek-gen ölçütlerine takılan hücre sayılarını yazıp tek cümleyle yüksek MT yüzdesinin neden ölüm imzası olduğunu söyleyin. Ve serinin ilk mührünü basın — bu kez bir sayı değil: `pbmc3k_ders.h5ad` dosyasını Zenodo'ya yükleyin (başlıkta proje adı, açıklamada 10x Genomics atfı, lisans CC-BY), yayımlayın ve DOI'nizi yazın. Bundan sonrası o adresten okunacak.
