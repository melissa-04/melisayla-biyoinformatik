# Uzamsal kümeleme

!!! question "Bu rehberde"
    İkinci serinin kümeleme hattını uzamsal veriye uygulayacak, sonucu hem UMAP hem doku üzerinde çizecek ve kümeleme koordinatları hiç görmediği halde anatomiyi bulup bulmadığını sınayacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/spatial/02_uzamsal_kumeleme.ipynb){ .md-button .md-button--primary }

## Aynı hat, yeni sınav

Bu rehberin kodunda yeni bir yöntem yok: normalizasyon, değişken genler, PCA, komşuluk grafiği, Leiden — hepsi 2.2-2.4'ten tanıdık ve gerekçeleri orada. Yeni olan, sonucu iki ayrı uzayda birden görebilmek. UMAP ifade benzerliğine göre kurulmuş bir yerleşimdir; `sc.pl.spatial` ise aynı kümeleri dokunun gerçek koordinatlarında gösterir. Bu iki resmin karşılaştırılması, uzamsal verinin tek hücreye kattığı asıl şeydir.

Bir sınırı baştan yazalım: buradaki normalizasyon spot başına toplamı eşitler. Spotlar farklı sayıda hücre içerdiğinden bu işlem, hücre sayısı farkını da ifade farkının içine katar; CP10K bu iki kaynağı ayıramaz. Uzamsal veride bu bilinen bir sınırdır ve aşağıdaki yorumlar bu sınırla birlikte okunur.

## Koşumun sayıları

Değişken gen sayısı 2.614 — PBMC'deki 1.860'tan fazla. İlk beş bileşen varyansın yüzde 8,51 / 3,67 / 3,36 / 2,43 / 1,68'ini açıklıyor; ilk on bileşenin toplamı yüzde 24,3.

Bu yüzdeler ikinci seriden gelen biri için dikkat çekici: orada PC1 yüzde 2,1 ve ilk on bileşen yüzde 6,7 idi. Fark, verinin yapısından geliyor. PBMC'de her satır tek bir hücreydi ve hücreler onlarca farklı yönde değişiyordu; burada her satır birkaç hücrelik bir bölgenin ortalaması ve bölgeler dokunun katmanlı mimarisini izliyor. Ortalama alınması hücre düzeyindeki gürültüyü söndürüyor, geriye kalan sinyal ise daha az sayıda güçlü eksene toplanıyor.

Kümeleme resolution=0.5 ile 14, resolution=1.0 ile 21 küme verdi. Küme boyları 0.5'te 377'den 21'e uzanıyor; 21 kümelik çözünürlük bu doku için ayrıntılı, adlandırma çalışmasına 14 ile başlamak daha yönetilebilir.

## Asıl gözlem

Kümeleme algoritması koordinatları hiç görmedi. Girdisi yalnız gen ifadesiydi: komşuluk grafiği PCA uzayında kuruldu, Leiden o grafiği böldü. Buna rağmen kümeler doku üzerinde çizildiğinde dağınık bir serpinti değil, derli toplu ve simetrik bölgeler oluşturuyor — kesitin iki yarısında birbirinin aynası konumlarda.

Bunun anlamı şu: dokunun anatomik mimarisi, gen ifadesine yazılıdır. Beynin bölgeleri birbirinden yalnız konum olarak değil, transkriptom olarak da ayrışır; algoritma bu ayrımı yakalayınca konumu kendiliğinden geri bulmuş olur. Uzamsal veri bu yüzden çifte bir doğrulama sağlar: kümeleme sonucunuzun anlamlı olup olmadığını, bağımsız bir bilgi kaynağıyla — dokunun kendi resmiyle — sınayabilirsiniz. Tek hücre verisinde böyle bir dış ölçüt yoktur.

## Anatomik işaretçiler

Küme numaralarını bölge adlarına bağlamak için bilinen işaretçileri doku üzerinde çiziyoruz: Snap25 nöronal sinaps proteinidir ve nöron yoğun gri maddeyi işaretler; Mbp ve Plp1 miyelin proteinleridir ve ak maddeyi gösterir; Hpca hipokampusta zenginleşir; Ttr koroid pleksus işaretçisidir. Bu genler `v.raw` içinde durduğu için daraltmadan etkilenmezler — `sc.pl.spatial` varsayılan olarak oradan okur.

Yöntem şöyle işler: bir işaretçinin yoğunlaştığı bölgeye bakarsınız, aynı bölgede hangi kümenin oturduğunu görürsünüz, kümeye o anatomik adı verirsiniz. İkinci serinin dotplot'uyla aynı mantık; tek fark, burada kanıtın doku üzerindeki konum olması.

## Neler ters gider?

Üç uyarı. Birincisi, uzamsal tutarlılığı doğruluk kanıtı sanmak: komşu spotlar doğal olarak benzer içeriğe sahiptir, bu yüzden kümelerin bir miktar öbeklenmesi beklenir; ilginç olan öbeklenmenin kendisi değil, bilinen anatomiyle örtüşmesidir. İkincisi, küme sayısını doku bilgisiyle karıştırmak: 14 küme "bu kesitte 14 anatomik bölge var" demek değildir; bir bölge birden çok kümeye bölünebilir, iki komşu bölge tek kümede birleşebilir. Üçüncüsü, spotu hücre saymak: bir küme "nöron kümesi" değil, nöron ağırlıklı bölgelerin kümesidir; hücre tipi iddiası için dekonvolüsyon gerekir ve o bu serinin ilerisinde.

## Kendin dene

Defterin sonunda üç görev var. Değişken gen sayınızı, ilk beş bileşenin yüzdelerini ve iki çözünürlükteki küme sayılarını not edip PCA yüzdelerini 2.3'teki PBMC değerleriyle karşılaştırın. Kümelerin doku üzerindeki dağılımına bakıp bu rehberin ana sorusunu kendi cümlenizle cevaplayın. Ve dört işaretçi haritasını kullanarak en az iki kümeyi anatomik bir bölgeyle eşleştirin, hangi işaretçiye dayandığınızı yazın.
