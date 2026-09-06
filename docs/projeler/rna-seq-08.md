# Duruşma: PyDESeq2 ile diferansiyel ifade

!!! question "Bu rehberde"
    İlk Python paketinizi kurup gerçek bir diferansiyel ifade analizi koşacak, el yapımı cetvelinizi aracınkiyle yüzleştirecek ve binlerce genin kaderini tek grafikte okuyacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/08_durusma.ipynb){ .md-button .md-button--primary }

## Sanığın açıklanışı

Geçen rehberin avında bulduğunuz gen **Ahsp**: serbest alfa-globin zincirlerini çökelmeden tutan eritroid bir şaperon, yani bu serinin gürültücü yıldızı Hba-a1'in bakıcısı. Nakavt farelerde susturulan gen bu. Kan fabrikası benzetmesiyle söylersek, sıradan bir işçi değil montaj hattının ustabaşı sökülmüş — ve yedi rehberdir hazırladığımız her şey tek soruya akıyor: istatistik bu sökümü görüyor mu, onunla birlikte başka kimleri mahkûm ediyor?

## Mahkemenin dili

DESeq2 her gen için dört sütun döker. baseMean genin normalize ortalama sesi; log2FoldChange kat değişimi, log2 dilinde (−1 yarıya inmek, +1 ikiye katlanmak, −13 ise yaklaşık sekiz bin kat çöküş demek); pvalue "hiç fark yokken bu kadar fark görme" olasılığı. Kararlar ise padj'ye dayanır, çünkü aynı anda on binlerce duruşma yürütüyoruz: benim koşumda 16.834 gen test edildi ve 0,05 eşiğiyle pvalue'ya güvenseydik sırf şanstan 840 civarı yalancı mahkûmiyet beklerdik. Çoklu test düzeltmesi bu enflasyonun panzehiri. Bir de tabloda padj'si NA olan satırlar göreceksiniz; hata değil: DESeq2, zaten karar verilemeyecek kadar sessiz genlerde düzeltme bütçesini harcamaz.

Modelin kalbi hakkında iki cümle: grup başına üç örnekle bir genin varyansını tek başına kestiremezsiniz; DESeq2 binlerce genin bilgisini birleştirip her genin dispersiyonunu komşularından ödünç akılla kestirir ve sayım verisinin doğasına uyan negatif binom dağılımını kullanır. Bu yüzden ona ham sayım verilir — cetveli kendisi kurar.

## Cetvellerin yüzleşmesi

Yedinci rehberde on satır pandas ile kurduğunuz cetvel vardı ya; defterde onu aracın kendi kurduğuyla yan yana koyuyoruz. Benim koşumda sonuç şu: 0,76 · 0,63 · 1,98 · 0,88 · 1,28 · 0,96 — **altısı da, virgülden sonra iki hanede, birebir aynı.** Geçen hafta elinizle kurduğunuz şey meğer aracın kalbiymiş; kara kutu diye bir şey yokmuş, sadece henüz açılmamış kutular varmış.

## Sanığın karnesi

Ahsp'nin satırı: log2FoldChange **−12,94** — yani 2¹²·⁹⁴ ≈ **7.850 kat** çöküş — ve padj **3×10⁻²⁶** mertebesinde. Nakavt gerçekten nakavtmış; istatistik, sizin yedinci rehberde çıplak gözle bulduğunuz sanığı ezici kanıtla mahkûm ediyor. Ama yalnız değil: benim koşumda padj < 0,05 ve |log2FC| > 1 eşiğini birden geçen **1.385 gen** var — 630'u yükselmiş, 755'i düşmüş. Ustabaşı gidince atölyede devrilenlerin listesi bu.

Beraat edenler de öğretici. Yedinci rehberde gözünüze çarpan Gapdh, +0,54'lük kıpırtısıyla padj 0,058'de eşiğe takıldı: göz görmüştü, jüri ikna olmadı. Pasta deneyimizin kahramanı Afp −0,04 ile taş gibi duruyor — onu deneyde biz delirtmiştik, gerçekte kılı kıpırdamıyor. Ve işin en tuhafı: sanığın korumakla görevli olduğu Hba-a1, −1,5 civarı düşmüş görünmesine rağmen padj 0,27 ile beraat etti. Globin cephesinde işlerin göründüğünden karışık olduğunun ilk işareti bu; üçüncü görev tam oraya gidiyor.

## Volkan nasıl okunur

Binlerce satırlık tabloyu tek bakışta okumanın yolu volkan grafiği: her nokta bir gen, yatay eksen "kaç kat değişti", dikey eksen "ne kadar eminiz". İki eksen iki ayrı soru sorar ve cevapları aynı olmak zorunda değildir: en uçta değişen gen, en kesin kanıta sahip gen olmayabilir — benim volkanımda da öyle oldu, ama kürsüdeki baş tanığın kim olduğunu söylemeyeceğim; o sizin avınız. Kesikli çizgiler eşikleri gösterir; köşeler, hem büyük hem kesin değişimlerin mahallesidir.

## Neler ters gider?

Üç tuzak. Birincisi artık teorik değil: PyDESeq2'ye normalize edilmiş ya da CPM tablo verirseniz model sessizce yanlış kurulur — yedinci rehberin uyarısı burada gerçek oluyor, girdi daima ham sayımdır. İkincisi, p < 0,05'e tapmak: anlamlılık tek başına önem demek değildir; padj ile etki büyüklüğü eşiği birlikte kullanılır, yoksa 840 yalancı tanık salona doluşur. Üçüncüsü, volkandaki her yıldızı biyoloji sanmak: teknik gölgeler — örneğin kütüphane hazırlığındaki arındırma işlemleri — istatistiği pekâlâ kandırabilir. Model sayıları görür, sayıların başına gelenleri görmez; hüküm vermeden önce Veri sayfası okunur. Üçüncü görev bunun tatbikatı.

## Kendin dene

Defterin sonunda üç görev var. Sanığın karnesini kendi koşunuzdan not edin ve 2**12.94 hesabıyla kat değişimini kendiniz görün. Sonuç tablosunu padj'ye göre sıralayıp kürsüdeki en kesin tanığın adını bulun; sanık çıkmadıysa, volkanın iki ekseninin neden ayrı olduğunu bir cümleyle söyleyin. Ve finali oynayın: volkanda Hbb-y ile Hbb-bs'yi bulun, yönlerine bakın, Veri sayfasındaki arındırma notunu yeniden okuyun ve serinin başından beri masada duran bilmeceyi tek cümleyle kapatın — o cümle, istatistiğin tek başına neden yetmediğinin de özeti olacak.
