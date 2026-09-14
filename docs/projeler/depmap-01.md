# Veri mutfağı: DepMap CRISPR ekranları

!!! question "Bu rehberde"
    Genom çapında CRISPR nakavt ekranlarının çıktısını tanıyacak, Chronos bağımlılık skorunun ölçeğini bilinen genler üzerinden okuyacak ve ders alt kümesini çıkarıp Zenodo'ya taşıyacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/depmap/01_veri_mutfagi.ipynb){ .md-button .md-button--primary }

## Dördüncü proje: boru hattı yok

İlk üç projede ham veriden başlayıp tabloyu kendimiz ürettik. Burada tablo hazır geliyor ve iş, ona doğru soruları sormak. Veri Broad Institute'un DepMap projesinden: yüzlerce kanser hücre hattında genom çapında CRISPR-Cas9 nakavt ekranı yapılmış, her hatta her gen tek tek devre dışı bırakılmış ve hücrelerin çoğalmasına ne olduğu ölçülmüş. Çıktı, hücre hattı × gen boyutunda bir bağımlılık skoru matrisi.

Projenin sorusu şu: bir geni kapattığınızda hangi kanser hücreleri ölüyor, hangileri umursamıyor — ve bu fark ilaç hedefi seçimi için ne anlama geliyor?

## Chronos skoru ve ölçeğin gerçeği

Matristeki her sayı bir Chronos skorudur ve iki referans noktasına göre ölçeklenir: sıfır, hiçbir geni hedeflemeyen kontrol sgRNA'ların medyanıdır — yani "nakavt hiçbir şey yapmadı" noktası. Eksi bir ise bilinen ortak esansiyel genlerin **medyanına** karşılık gelir.

Bu ikinci cümledeki "medyan" kelimesi önemli ve koşum bunu somutluyor. Ribozom proteini RPL23A'nın medyanı −2,43, proteazom alt birimi PSMA1'inki −2,36. Yani eksi bir bir taban değil; ortak esansiyellerin ortası. En ölümcül genler o çizginin çok altına iner — RPL23A'nın en düşük değeri −4,00. Ölçeği "−1 ölümdür" diye ezberlerseniz, −2,4'lük bir skoru yorumlayamazsınız.

Üçüncü gen tabloyu tamamlıyor: KRAS'ın medyanı −0,52, ama aralığı −4,46 ile +0,26 arasında. Ortalamaya bakan biri "orta derecede esansiyel" der ve yanılır. Bazı hatlarda KRAS'ı kapatmak hücreyi öldürüyor, bazılarında hiçbir şey yapmıyor, birkaçında çoğalmayı hafifçe artırıyor. Bu dağılım bu projenin bütün meselesinin özeti ve 4.4'ün konusu.

Bir de eksik bulduğumuz gen var: kontrol olarak seçtiğim koku alma reseptörü OR2T1 matriste yok. Ekranda 18.531 gen taranmış, yani insan genomunun tamamı değil; kütüphaneye hangi genlerin girdiği bir tasarım kararıdır ve aradığınız gen orada olmayabilir.

## Tablonun boyutları

CRISPRGeneEffect matrisi 1.208 hücre hattı × 18.531 gen. Model künyesinde ise 2.154 hücre hattı kayıtlı — yani DepMap'in tanımladığı hatların yarısından biraz fazlasının CRISPR ekranı var. İki tablo `ModelID` (ACH-XXXXXX) üzerinden eşleşir ve analiz kesişimle yapılır.

Doku dağılımı dengesiz: en çok hat akciğerde (126), sonra lenfoid (96), beyin/MSS (91), baş-boyun (77), deri (75). Meme 53 hatla onuncu sırada. Bu dengesizlik ileride kanser tipleri karşılaştırılırken hesaba katılacak — 126 hatlı bir dokuda ölçülen ortalama ile 20 hatlı bir dokudakinin güvenilirliği aynı değildir.

## Alt küme ve beklenmedik bir sonuç

Tam matris 400 MB'ın üzerinde, her defterde indirilemez. Alt kümeyi üç parçadan kurduk: en değişken 4.000 gen (hatlar arası standart sapmaya göre), ölçeği anlatmak için 500 ortak esansiyel referans, ve dağılımı dürüst göstermek için rastgele seçilmiş 1.000 gen.

Ortadaki adım ilginç bir sonuç verdi: en düşük medyanlı 500 genin **hepsi** zaten en değişken 4.000'in içindeydi, yani ayrıca eklenecek gen kalmadı. Sebebi düşünmeye değer. Ortak esansiyel genler her hatta ölümcüldür ama ölümcüllüğün derecesi hattan hatta oynar; −1,5 ile −3,5 arasında gezinen bir gen, sıfır çevresinde titreşen bir genden çok daha yüksek standart sapmaya sahiptir. Yani varyansa göre seçim, esansiyel genleri kendiliğinden içine alır.

Rastgele 1.000 geni bu yüzden ekledik: yalnız yüksek varyanslı genlerle çalışırsanız, DepMap'in en temel gerçeğini göremezsiniz — herhangi bir hücre hattında genlerin büyük çoğunluğu vazgeçilebilirdir ve skorları sıfır etrafında toplanır. Alt küme 1.208 hat × 5.000 gen ve 40,5 MB — kendi DOI'sinde yayımlandı: [10.5281/zenodo.22759375](https://doi.org/10.5281/zenodo.22759375). Sonraki bütün defterler veriyi doğrudan oradan okuyacak.

## Neler ters gider?

Üç tuzak. Birincisi, skoru mutlak sanmak: Chronos iki referans arasına yerleştirilmiş göreli bir ölçüdür ve sürümden sürüme yeniden hesaplanır; bu yüzden sürüm numarası raporda yazılır. İkincisi, medyanla hüküm vermek: KRAS örneği gösterdi, dağılıma bakmadan verilen karar yanlıştır — birinci serinin "dağılıma bakmadan hüküm yok" kuralı burada da geçerli. Üçüncüsü, bir genin yokluğunu "etkisiz" sanmak: matriste olmayan gen taranmamış demektir, ölçülüp sıfır çıkmış demek değildir.

## Kendin dene

Defterin sonunda üç görev var. Tam matrisin boyutlarını, ekranı olan hat sayısını ve en çok hattı olan üç dokuyu not edin. Üç genin medyan ve aralık değerlerini yazıp tek cümleyle açıklayın: RPL23A'nın medyanı neden −1 değil de −2,43 ve bu, ölçeğin nasıl kurulduğu hakkında ne söylüyor? Ve alt küme dosyalarını Zenodo'ya yeni bir kayıt olarak yükleyip DOI'nizi alın; sonraki bütün defterler veriyi o adresten okuyacak.
