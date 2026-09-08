# PCA, komşuluk grafiği ve UMAP

!!! question "Bu rehberde"
    Matrisi değişken genlere daraltacak, scale ile genleri ortak ölçeğe çekecek, PCA'nın varyans oranlarını okumayı öğrenecek ve komşuluk grafiği üzerinden ilk UMAP haritanızı çizeceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/scrna/03_boyut_indirgeme.ipynb){ .md-button .md-button--primary }

## raw kopyası ve daraltma

Defter önce önceki rehberin tarifini tekrar koşar (indir, normalize et, log al, bayrakla), sonra iki kritik satır gelir. `a.raw = a`, tam log-normalize matrisin dondurulmuş bir kopyasını saklar; ileriki rehberlerde işaretçi genleri çizerken scanpy varsayılan olarak bu kopyadan okur, dolayısıyla daraltma hiçbir genin ifadesini kaybettirmez. `a = a[:, a.var.highly_variable]` ise matrisi yalnız bayraklı sütunlara indirir — benim koşumda 2.694 hücre × 1.860 gen. Gerekçe önceki rehberden: değişmeyen genler ayrım bilgisi taşımaz.

## scale

`sc.pp.scale(a, max_value=10)` her geni ortalama 0, standart sapma 1 olacak şekilde dönüştürür; buna z-skor denir. Gerekçe: PCA varyansı en büyük yönleri arar ve ölçekleme yapılmazsa ortalaması büyük genler eksenleri tek başına belirler. `max_value=10` parametresi, z-skoru 10'u aşan uç değerleri 10'da keser; tek tük aşırı değerin bir ekseni kendine çekmesini önler. Bir yan etkiyi bilin: bu işlem matrisi yoğunlaştırır, seyrek saklama avantajı burada biter.

## PCA ve sayıları okumak

`sc.tl.pca(a, svd_solver='arpack', n_comps=50)` ilk 50 temel bileşeni hesaplar; her bileşenin açıkladığı varyans oranı `a.uns['pca']['variance_ratio']` içinde döner. Benim koşumda ilk beş bileşen yüzde 2,13 / 1,18 / 0,94 / 0,81 / 0,52 açıklıyor; ilk 10 bileşenin toplamı yüzde 6,7.

Bu sayılar birinci seriden gelen birine küçük görünecek — orada PC1 tek başına yüzde 45,8'di. Fark bozukluk değil, verinin doğasıdır: orada iki gruba ayrılmış altı örnek vardı ve tek bir eksen (genotip) varyansın yarısını taşıyordu; burada 2.694 hücre ve hücre tipi, hücre döngüsü, teknik gürültü gibi birbirinden bağımsız birçok değişkenlik kaynağı var. Varyans çok eksene yayılır, yüzdeler küçülür, ama bileşenlerin sıralaması bilgi taşımaya devam eder. Kaç bileşenle devam edileceği varyans oranlarının azalım eğrisine bakılarak seçilir; bu seride öğretim standardı 10 bileşen. Bu bir ayar düğmesidir ve raporda gerekçesiyle yazılır.

## Komşuluk grafiği

`sc.pp.neighbors(a, n_neighbors=15, n_pcs=10)` her hücre için, ilk 10 PCA bileşeninin uzayında en yakın 15 hücreyi bulur ve bir komşuluk (kNN) grafiği kurar. Grafiğin ham gen uzayı yerine PCA uzayında kurulmasının iki nedeni var: çok yüksek boyutta uzaklık ölçüleri ayırt ediciliğini kaybeder, ve hesap maliyeti düşer. Bu grafik yalnız görselleştirmenin altyapısı değil; bir sonraki rehberde kümeleme algoritmasının doğrudan girdisi olacak.

## UMAP ve tek kural

`sc.tl.umap(a)` komşuluk grafiğini iki boyutlu bir yerleşime çevirir; `sc.pl.umap(a, color=...)` haritayı obs tablosundaki herhangi bir sütunla boyar. Kural baştan: UMAP bir görselleştirme aracıdır. Eksenlerin adı yoktur, koordinatlar anlam taşımaz, iki nokta arasındaki uzaklık ya da bir adanın büyüklüğü nicel yorumlanmaz; güvenilir olan, hangi hücrelerin birlikte gruplandığıdır. İlk boyamayı kalite ölçüleriyle yapıyoruz (n_genes_by_counts ve pct_counts_mt): amaç, adaların biyolojiden değil teknik ölçülerden kaynaklanıp kaynaklanmadığını kontrol etmek. Sağlıklı bir haritada kalite ölçüleri adalar arasında keskin sınırlar çizmez.

## Neler ters gider?

Üç sık hata. Birincisi, scale'siz PCA: eksenler en yüksek ifadeli genlerin listesine dönüşür ve hücre tipi sinyali gömülür. İkincisi, UMAP üzerinden nicel iddia: "bu küme şuna daha yakın, demek ki daha benzer" ya da "bu ada büyük, demek ki önemli" cümleleri kurulmaz; bu tür iddialar grafiğin kendisinden değil, sayısal analizden gelir. Üçüncüsü, n_pcs ve n_neighbors değerlerini sabit gerçek sanmak: bunlar ayar düğmeleridir; makul aralıkta oynatınca haritanın şekli esner ama yapısı değişmemelidir — değişiyorsa bulgunuz parametreye bağımlıdır ve rapor edilmez.

## Kendin dene

Defterin sonunda üç görev var. İlk 5 bileşenin varyans yüzdelerini ve 10 bileşenin kümülatifini kendi koşunuzdan not edin; tek cümleyle PC1'in burada neden yüzde 2 civarında olduğunu açıklayın. Tek cümleyle, komşuluk grafiğinin neden PCA uzayında kurulduğunu yazın. Ve UMAP haritanızda göz kararı kaç ayrı hücre grubu saydığınızı yazın; kalite renklerinden bir gözlem ekleyin. Bu sayı, bir sonraki rehberde kümeleme algoritmasının bulduğu sayıyla karşılaştırılacak.
