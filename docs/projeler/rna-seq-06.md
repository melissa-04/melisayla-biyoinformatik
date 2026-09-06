# Sayım tablosundan analize: sayı yakasına geçiş

!!! question "Bu rehberde"
    Sayım tablosunu pandas ile okuyup tanıyacak, gizemli transkript kodlarının kimliğini çözecek ve veriyi istatistiğin kapısına kadar getireceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/06_sayim_tablosu.ipynb){ .md-button .md-button--primary }

## Projenin dönüm noktası

Beş rehber boyunca dosyalarla uğraştık: FASTQ, kalite, sayım. Bundan sonra tek bir tabloyla yaşıyoruz: 56.065 gen × 6 örnek. Bu geçiş önemli; çünkü buradan sonrası "biyoinformatik araç bilgisi"nden çok "veriyle düşünme". Tablo küçük görünür ama projenin bütün cevapları içinde.

## Tabloyu tanımak: üç bakış

Birinci bakış, kütüphane büyüklükleri: her sütunun toplamı. Benim koşumda WT_3'ün toplamı WT_1'in 2,4 katı çıktı; Veri sayfasında baz sayılarından tahmin ettiğimiz derinlik farkının sayım karşılığı bu. Aynı genin WT_1'de 100, WT_3'te 240 okuması olması hiçbir biyoloji anlatmaz; sadece WT_3'ün daha çok dizilendiğini anlatır. "Ham sayılar doğrudan karşılaştırılamaz" cümlesinin kanıtı artık kendi tablonuzda; bir sonraki rehberin tamamı bu cümlenin çözümü.

İkinci bakış, zirve: en yüksek toplam sayıma sahip genler. Benim koşumda tepede mitokondri genleri (mt-Co1, mt-Co2), protein sentezinin demirbaşı Eef1a1 ve fetal karaciğerin imza ürünleri Afp ile Alb var; doku, "ben enerji harcayan bir protein fabrikasıyım" diyor. Bir gariplik daha: eritroid bir dokuda zirvede globin yok. Şaşırmayın; Veri sayfasını okuyanlar sebebini biliyor, RNA'lar globinden arındırılmıştı. Zirve listesi bile verinin hazırlanışını ele veriyor.

Üçüncü bakış, dip: 24 binden fazla gen bütün örneklerde tamamen sıfır. Bu bir hata değil; katalog bütün dokuların genlerini içerir, fetal karaciğer hepsini çalıştırmaz.

## Gizemin kimliği ve eşleme

Dördüncü rehberde not ettiğiniz ENSMUST kodları bu tabloda görünmez; tablo gen düzeyinde. Defterde GENCODE kataloğundan bir sözlük kurup o kodların hangi genlere ait olduğunu çözüyoruz; kendi not ettiklerinizi de sözlüğe sorun. Aynı sözlük mantığı, ileride hangi veriyle çalışırsanız çalışın lazım olacak: transkript, gen kimliği ve gen adı üç ayrı katmandır ve aralarında çeviri hep sizin işinizdir.

## İstatistiğin istediği biçim

PyDESeq2 iki şey ister: satırları örnek, sütunları gen olan sayım tablosu (bizimkinin devriği) ve her örneğin grubunu söyleyen küçük bir künye. Bir de filtre: toplam sayımı 10'un altındaki genleri eliyoruz; benim koşumda 21.994 gen analizde kaldı. Bu genler zaten karar verilemeyecek kadar sessiz; onları taşımak istatistiğe güç değil gürültü katar.

## Neler ters gider?

Üç klasik tuzak. Gen adları benzersiz değildir; aynı ad birden fazla gen kimliğine bağlanabilir, bu yüzden analiz hep gen_id üzerinden yürür, ad sadece etikettir. CSV'yi Excel'de açıp kaydetmek gen adlarını bozabilir (bazı gen adlarını tarihe çevirmesiyle meşhurdur); tabloya sadece pandas ile dokunun. Ve devrik alırken satır-sütun karışıklığı: PyDESeq2'ye genleri satırda verirseniz hata bile almadan saçma sonuç alırsınız; şekilleri yazdırıp kontrol etmek iki saniyedir, alışkanlık edinin.

## Kendin dene

Defterdeki üç görev: kendi ENSMUST kodlarınızın kimliği, WT_3/WT_1 oranı ve Hbb-bs satırı. Üçüncüsü küçük bir vicdan testi: sayıyı görünce "knockout globini düşürmüş" demek isteyeceksiniz; Veri sayfası neden diyemeyeceğinizi söylüyor.
