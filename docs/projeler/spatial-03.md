# Uzamsal olarak değişken genler

!!! question "Bu rehberde"
    squidpy ile fiziksel komşuluk grafiğini kuracak, Moran's I istatistiğiyle hangi genlerin doku üzerinde desenli dağıldığını ölçecek ve "değişken gen" ile "uzamsal gen" ayrımını sayılarla göreceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/spatial/03_uzamsal_genler.ipynb){ .md-button .md-button--primary }

## İki komşuluk grafiği

Bu rehberin anahtarı, iki farklı komşuluk kavramını ayırt etmek. 2.3'te kurduğumuz `sc.pp.neighbors` grafiği PCA uzayında kuruluyordu: iki spot, gen ifadeleri benzerse komşuydu. Bu defterde kurduğumuz `sq.gr.spatial_neighbors` grafiği ise `obsm['spatial']` koordinatlarına dayanır: iki spot, doku üzerinde yan yanaysa komşudur. İfade benzerliği ile fiziksel yakınlık iki ayrı bilgidir ve uzamsal analizin bütün gücü, bu ikisini karşılaştırabilmesinden gelir.

Visium'un spot düzeni altıgendir, yani her iç spotun altı komşusu vardır; koşumda spot başına ortalama komşu sayısı 5,8 çıktı — kenar spotlarının komşusu daha az olduğu için altının biraz altında. `squidpy`, scanpy'nin uzamsal analiz için geliştirilmiş kardeş kütüphanesidir ve bu grafiği düzeni tanıyarak kurar.

## Moran's I nedir

Moran's I, bir değerin uzamsal olarak kümelenip kümelenmediğini ölçer. Her spotun değeri komşularının değerleriyle karşılaştırılır: yüksek değerli spotların komşuları da yüksekse ve düşüklerin komşuları düşükse istatistik 1'e yaklaşır; değerler komşuluktan bağımsız dağılmışsa 0 civarında kalır; komşular sistematik olarak zıtsa negatife iner.

`sq.gr.spatial_autocorr(v, mode='moran', genes=..., n_perms=None)` bunu her gen için hesaplar. `n_perms=None` parametresi p değerlerinin permütasyon yerine normal yaklaşımla hesaplanmasını sağlar; permütasyon daha titizdir ama binlerce gen için dakikalar sürer, normal yaklaşım saniyeler. Sonuç `v.uns['moranI']` tablosuna I değerine göre sıralı yazılır ve p değerleri Benjamini-Hochberg ile düzeltilir — binlerce gen test edildiği için birinci seriden tanıdık zorunluluk.

## Koşumun sayıları

Test edilen 2.614 değişken genin **2.395'i** uzamsal olarak anlamlı (FDR<0,05). Tablonun tepesi: Nrgn I=0,889, Camk2n1 0,870, Prkcd 0,867, Slc17a7 0,864, Pmch 0,849. Bu isimler tesadüfi değil — Slc17a7 uyarıcı nöronların glutamat taşıyıcısıdır, Camk2n1 ve Nrgn nöronal sinyal proteinleridir, listenin biraz aşağısındaki Mobp ise ak madde miyelin proteinidir. İstatistik, kendisine hiçbir biyolojik bilgi verilmeden, beynin anatomik olarak en keskin ayrışan genlerini tepeye taşımış.

Listenin dibinde Gm49741 gibi tanımsız gen modelleri var; I değerleri sıfırın hafif altında, yani komşuluktan tamamen bağımsız dağılıyorlar. Tepe ile dibi doku üzerinde yan yana çizmek, istatistiğin ne ölçtüğünü tek bakışta gösterir: üstte bölgelere oturmuş net desenler, altta doku boyunca rastgele serpinti.

## Değişken gen ile uzamsal gen aynı şey değildir

Sayı şu: 2.614 değişken genin **886'sının** I değeri 0,1'in altında. Yani bu genler spotlar arasında gerçekten değişiyor — `highly_variable_genes` onları boşuna bayraklamadı — ama bu değişim doku üzerinde bir desen oluşturmuyor.

İki soru iki farklı şey sorar. `highly_variable_genes` koordinatları hiç kullanmaz; yalnız "bu genin değeri spotlar arasında oynuyor mu" diye bakar. Moran's I ise "bu oynama konumla ilişkili mi" diye sorar. Desensiz değişkenliğin birkaç meşru kaynağı olabilir: teknik gürültü, düşük ifadeli genlerde örnekleme dalgalanması, ya da anatomik bölgelere değil de dokuya serpiştirilmiş hücre tiplerine (örneğin damar hücreleri, mikroglia) ait sinyaller — bunlar gerçek biyolojidir ama uzamsal olarak yerel değil, dağınıktır.

Pratik sonuç: uzamsal bir çalışmada gen listesi Moran's I ile kurulur, değişken gen listesiyle değil. Değişken gen seçimi kümelemenin girdisidir; uzamsal desen arıyorsanız ölçütünüz otokorelasyondur.

## Neler ters gider?

Üç uyarı. Birincisi, yüksek I'yi tek başına biyolojik önem sanmak: doku üzerindeki geniş bir teknik gradyan da yüksek I verir; bu yüzden tepe genler mutlaka doku üzerinde çizilerek gözle kontrol edilir. İkincisi, permütasyonsuz p değerlerini titiz sonuç saymak: normal yaklaşım hızlıdır ve sıralamayı doğru verir, ancak kritik bir yayında `n_perms` ile permütasyon testi koşulur. Üçüncüsü, düşük I'yi "önemsiz gen" diye yorumlamak: az önceki 886 genin içinde dokuya dağılmış gerçek hücre tiplerinin işaretçileri olabilir; Moran's I yerel olmayan sinyali göremez, görmemesi de onun tanımıdır.

## Kendin dene

Defterin sonunda üç görev var. Tablonun ilk üç genini ve I değerlerini not edip 3.2'deki küme haritasıyla karşılaştırın: hangi anatomik bölgeye denk geliyorlar? En yüksek ve en düşük I değerli genlerin doku görüntülerini karşılaştırıp Moran's I'nın tam olarak neyi ölçtüğünü tek cümleyle yazın. Ve hem değişken bayraklı hem uzamsal deseni zayıf çıkan genler için tek cümlelik bir açıklama üretin: bu nasıl mümkün ve bu genler ne tür bir değişkenliği temsil ediyor olabilir?
