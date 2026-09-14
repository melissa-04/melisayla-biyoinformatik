# Veri mutfağı: Visium

!!! question "Bu rehberde"
    Uzamsal transkriptomik verisinin klasör düzenini tanıyacak, read_visium ile sayımları, koordinatları ve doku görüntüsünü tek nesnede toplayacak, spot düzeyinde kaliteye bakacak ve arşivlik dosyayı üretip Zenodo'ya taşıyacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/spatial/01_veri_mutfagi.ipynb){ .md-button .md-button--primary }

## Üçüncü proje: koordinatlı veri

İkinci seride her hücreyi tek tek okuduk ama hücrelerin dokunun neresinde durduğunu bilmiyorduk; doku, süspansiyon hazırlanırken parçalanmıştı. Visium bu bilgiyi korur: doku kesiti, üzerinde barkodlu noktalar taşıyan bir lam üstüne yerleştirilir ve her noktanın koordinatı kayıtlıdır.

Buradan çıkan temel kural şu: **spot hücre değildir.** Her spot 55 mikrometre çapındadır ve altına denk gelen yaklaşık 1-10 hücrenin RNA'sını birlikte yakalar. Yani matrisin her satırı, küçük bir bölgenin karışımıdır. Bu, önceki serinin kapanışındaki "ortalama azınlığı saklar" dersinin küçük ölçekte tekrarıdır — ama karşılığında dokunun mimarisini kazanırız.

Veri: 10x Genomics'in halka açık **V1_Adult_Mouse_Brain** seti, yetişkin fare beyninin koronal kesiti. Beyin öğretim için iyi bir seçim, çünkü anatomisi bellidir: korteks katmanları, hipokampus, ak madde. Uzamsal desenler gözle doğrulanabilir.

## Klasör düzeni ve read_visium

`sc.read_visium` Space Ranger'ın standart klasör düzenini bekler, bu yüzden defter dosyaları indirip o düzene yerleştirir: `filtered_feature_bc_matrix.h5` spot × gen sayım matrisidir; `spatial/tissue_positions.csv` her spotun koordinatlarını tutar; `spatial/scalefactors_json.json` koordinatları görüntü piksellerine çeviren katsayıları verir; `tissue_hires_image.png` ve `tissue_lowres_image.png` kesitin fotoğraflarıdır.

Fonksiyon bunları tek AnnData'da toplar ve önceki serilerden bildiğiniz yapıya yeni bir kat ekler: sayımlar `X`'te, spot bilgileri `obs`'ta, gen bilgileri `var`'da — buraya kadar aynı — ve yeni olarak koordinatlar `obsm['spatial']`de, görüntülerle ölçek katsayıları `uns['spatial']`de. `var_names_make_unique` yine gerekli; gen adları benzersiz değildir.

## Koşumun sayıları

Matris 2.702 spot × 32.285 gen olarak geliyor; kütüphane adı V1_Adult_Mouse_Brain, iki çözünürlükte doku görüntüsü ve 2.702 satırlık koordinat tablosu nesnenin içinde. Spot başına medyan 28.943 UMI ve 6.018 gen ölçtüm. Bu sayılar PBMC'deki 2.197 UMI ve 817 genin yaklaşık on üç katı ve bu beklenen bir sonuç: bir spot tek hücre değil, birkaç hücrelik bir bölge.

Asıl dikkat çeken ölçü mitokondriyal yüzde: medyan 15,3. PBMC serisinde bu sayı 2,0 idi ve yüzde 10'u eşik olarak kullanmıştık. Aynı eşik burada uygulansaydı sağlıklı dokunun yarıdan fazlası elenirdi. Sebep teknik değil biyolojik: nöronlar enerji tüketimi yüksek hücrelerdir ve mitokondrileri boldur. İkinci serinin kuralı burada sayısıyla doğrulanıyor — MT eşiği evrensel değildir, dokunun biyolojisine göre seçilir ve gerekçesiyle raporlanır. Bu dokuda MT filtresi uygulamıyoruz; yalnız üçten az spotta görülen genleri eliyoruz ve matris 19.653 gene iniyor.

## Doku üstünde ilk bakış

Serinin yeni aracı `sc.pl.spatial`: verilen obs sütununu doku fotoğrafının üzerine, her spotu kendi koordinatında boyayarak çizer; koordinat-piksel çevirisini `scalefactors` yapar. İlk boyamayı kalite ölçüleriyle yaptık ve buradaki bakış, tek hücre serisindeki kalite kontrolünden farklı bir soru sorar. Orada "kalite ölçüleri kümeleri sürüklüyor mu" diye bakıyorduk; burada ek olarak şunu soruyoruz: sayımlar doku üzerinde rastgele mi dağılıyor, yoksa anatomiyi mi izliyor? Toplam UMI'nin anatomik yapıları takip etmesi bir kusur değil, beklenen bir sonuçtur — farklı beyin bölgelerinin hücre yoğunluğu ve transkripsiyon etkinliği farklıdır. Bu ayrım önemli: uzamsal veride "teknik gradyan" ile "biyolojik desen" birbirine benzer görünür ve ayırt etmek dokuyu tanımayı gerektirir.

## Arşiv ve veri

Bu rehberin ürettiği dosya `spatial_ders.h5ad`, 181,6 MB. Önceki serinin 20 MB'lık dosyasından büyük olmasının iki nedeni var: spotlar çok daha derin dizilenmiş ve doku görüntüleri dosyanın içinde taşınıyor. Dosya kendi Zenodo kaydında yayımlandı ve bu serinin bütün defterleri veriyi oradan okuyacak: **10.5281/zenodo.22758390**. Ayrı kayıt, çünkü kaynak ve atıf PBMC setinden farklı.

## Neler ters gider?

Üç sık hata. Birincisi, mitokondriyal öneki yanlış yazmak: fare genlerinde önek küçük harfli `mt-`, insanda `MT-`'dir; yanlış yazarsanız sıfır mitokondriyal gen bulursunuz ve hata mesajı almazsınız, yalnız yanlış bir kalite tablosu elde edersiniz. İkincisi, eşikleri önceki projeden taşımak: bu rehberin merkezindeki ders budur. Üçüncüsü, spotu hücre sanmak: spot başına gen sayısı yüksek çıkar ve bu "kaliteli hücre" işareti değildir, birkaç hücrenin toplamıdır; sonraki rehberlerde her yorum bu sınırla birlikte kurulacak.

## Kendin dene

Defterin sonunda üç görev var. Kendi koşunuzdan spot × gen boyutlarını, medyan UMI ve gen sayısını, medyan MT yüzdesini not edin. Tek cümleyle yazın: PBMC'deki yüzde 10'luk MT eşiği bu dokuya uygulansaydı ne olurdu ve bu neden yanlış olurdu? Ve dosyayı Zenodo'ya yeni bir kayıt olarak yükleyip DOI'nizi alın — önceki kayda sürüm eklemek değil, ayrı kayıt: kaynak ve atıf farklı.
