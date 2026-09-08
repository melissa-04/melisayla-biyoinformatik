# Normalizasyon ve değişken gen seçimi

!!! question "Bu rehberde"
    Veriyi serinin Zenodo DOI'sinden okuyacak, AnnData yapısını tanıyacak, hücreler arası derinlik farkını normalize_total ve log1p ile giderecek ve highly_variable_genes fonksiyonuyla kümeleme için bilgi taşıyan genleri seçeceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/scrna/02_normalizasyon.ipynb){ .md-button .md-button--primary }

## Veri ve AnnData yapısı

Veri bu rehberden itibaren kalıcı adresinden okunuyor: 10.5281/zenodo.22655336. Defterdeki `urllib.request.urlretrieve` satırı dosyayı bu adresten indirir, `sc.read_h5ad` da onu bir AnnData nesnesi olarak açar. AnnData, tek hücre verisinin standart kabıdır ve dört parçası var: `X` hücre × gen matrisidir; `obs` hücre başına bilgi tablosudur ve 2.1'de hesapladığımız kalite ölçüleri (total_counts, n_genes_by_counts, pct_counts_mt) orada durur; `var` gen başına bilgi tablosudur; `layers` ise matrisin adlandırılmış kopyalarını tutar. Bu dört ismi ezberlemeye değer, serinin geri kalanında sürekli geçecekler.

## Hücreler arası derinlik farkı

Benim koşumda hücre başına toplam UMI 548 ile 15.844 arasında; medyan 2.200 ve en derin hücre en sığın 28,9 katı. Birinci projede örnekler arasındaki 2,4 katlık derinlik farkını düzeltmek için normalizasyon kurmuştuk; tek hücre verisinde aynı sorun hücre ölçeğinde ve çok daha büyük boyutta var, çünkü damlacık tabanlı protokoller her hücreden eşit molekül yakalamaz.

İkinci ölçüm matrisin doluluğu: sıfırdan farklı değerlerin oranı yalnızca yüzde 6,2. Üçüncü ölçüm ise yöntem seçimini belirliyor: 13.714 genden her hücrede sıfırdan büyük olan gen sayısı bir (TMSB4X). Birinci serideki medyan-oran normalizasyonu, referansını her örnekte sıfırdan büyük olan genlerden kurar, çünkü geometrik ortalama sıfırla tanımsızdır; toplu veride bu koşulu 18.470 gen sağlıyordu. Tek geni kalan bir veride bu referans kurulamaz — medyan-oran tek hücre verisine bu yüzden doğrudan uygulanmaz.

## normalize_total ve log1p

Defter üç işlemi sırayla yapıyor. Önce `a.layers['sayim'] = a.X.copy()` ham sayımların kopyasını saklıyor; ileride ham sayım isteyen yöntemler (örneğin diferansiyel ifade) bu katmandan okuyacak ve ham veri hiçbir adımda kaybolmayacak. Sonra `sc.pp.normalize_total(a, target_sum=1e4)` her hücrenin bütün değerlerini o hücrenin toplamına bölüp 10.000 ile çarpıyor; sonuç "10.000 sayım başına ifade"dir (CP10K). target_sum bir ölçek sabitidir, 10.000 alanın yerleşik tercihidir; işlem hücreler arası derinlik farkını kaldırır ama hücre kompozisyonundaki gerçek farkları modellemez, sınırını bilerek kullanıyoruz. Son olarak `sc.pp.log1p(a)` her değere doğal logaritma(x+1) uygular: +1 sıfırların logaritmasını tanımlı kılmak için, log ise sağa çarpık sayım dağılışını sıkıştırıp farklı düzeydeki genleri aynı ölçekte karşılaştırılabilir yapmak için. Benim koşumda normalize sonrası her hücrenin toplamı tam 10.000'e oturuyor.

## highly_variable_genes

Kümeleme için bütün genlere ihtiyaç yok: hücreden hücreye değişmeyen bir gen, hücre tiplerini ayıracak bilgi taşımaz. `sc.pp.highly_variable_genes(a, flavor='seurat', min_mean=0.0125, max_mean=3, min_disp=0.5)` her gen için log-normalize veride ortalama ve dispersiyon (varyans/ortalama) hesaplar, genleri ortalama düzeylerine göre kutulara ayırıp dispersiyonu kutu içinde standartlaştırır ve üç eşik uygular. min_mean altında kalan genler elenir çünkü çok düşük ifadede değişkenlik ölçümü güvenilmez; max_mean üstünde kalanlar elenir çünkü çok yüksek ifadeli genlerde dispersiyon kestirimi sistematik farklı davranır; kalanlar arasında standartlaştırılmış dispersiyonu min_disp'i aşanlar bayrak alır. Sonuç a.var.highly_variable sütununa yazılır; benim koşumda 1.860 gen bayraklandı. Gen silinmez, işaretlenir; bir sonraki rehberde PCA yalnız bayraklıları kullanacak. Fonksiyonun grafiği seçimi gösterir: x ekseni ortalama, y ekseni dispersiyon, koyu noktalar seçilenler.

Bir uyarıyı buraya da yazıyorum: bu liste bir analiz aracıdır, biyolojik önem listesi değildir. Ortalaması max_mean eşiğinin üstünde kalan bir gen, hücre tipi işaretçisi olarak ne kadar değerli olursa olsun bayrak alamaz. Hücre tiplerine ad verirken işaretçi genlerin kendisine bakacağız, bayrağa değil; defterdeki üçüncü görev bunun somut bir örneğini buldurtuyor.

## Neler ters gider?

Üç sık hata. Birincisi çifte işlem: elinize hazır bir .h5ad geçtiğinde önce verinin ham mı, normalize mi, log alınmış mı olduğunu kontrol edin — hücre toplamlarına ve maksimum değerlere bakmak yeterlidir; normalize edilmiş veriyi tekrar normalize etmek hata mesajı vermeden yanlış sonuç üretir. İkincisi target_sum'a anlam yüklemek: 10.000 yalnız bir ölçek sabitidir ve bu normalizasyon hücre büyüklüğü bilgisini bilerek siler; sorunuz hücre boyuyla ilgiliyse bu ölçek onu cevaplayamaz. Üçüncüsü değişken gen listesini evrensel sanmak: liste eşiklere ve verinin kendisine bağlıdır; başka veri setinde farklı çıkar ve parti (batch) etkisi olan veride seçim parti içinde yapılır.

## Kendin dene

Defterin sonunda üç görev var. Kendi koşunuzdan toplam UMI'nin min/medyan/maks değerlerini, en derin/en sığ oranını ve matris doluluğunu not edin. "Her hücrede sıfırdan büyük gen" çıktınıza dayanarak tek cümleyle yazın: medyan-oran normalizasyonu bu veriye neden doğrudan uygulanamıyor? Ve işaretçi panelini (LYZ, GNLY, PPBP, MS4A1, CD3D, NKG7, CD14, FCGR3A) a.var tablosuyla karşılaştırın: kaç gen bayraklı, bayrak alamayanlardan ortalaması en yüksek olan hangisi ve hangi eşiğe takılmış — means sütununu eşiklerle karşılaştırarak tek cümleyle açıklayın.
