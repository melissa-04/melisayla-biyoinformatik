# Normalizasyon: herkese aynı cetvel

!!! question "Bu rehberde"
    Derinlik farkının yarattığı yanılsamayı görecek, CPM'in neyi çözüp neyi çözemediğini bir deneyle test edecek ve DESeq2'nin size factor cetvelini kendi elinizle kuracaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/07_normalizasyon.ipynb){ .md-button .md-button--primary }

## 2,38'in borcu

Geçen rehberin son sorusu bir sayıydı: WT_3'ün toplamı WT_1'in kaç katı? Benim koşumda 2,38 — yazıda "2,4" diye yuvarladığım sayının ta kendisi. Bu sayı masumken tehlikeli: Gapdh gibi "hep açık" bilinen bir gene bakın. Üç WT örneği aynı genotipin kopyaları, ama Gapdh'nin ham sayımı WT_1'de 14.363 iken WT_3'te 32.744; 2,28 kat. Gen coşmadı, makine daha uzun dinledi. Ham sayılarla iki örneği karşılaştırmak, biri fısıldarken biri megafonla konuşan iki insanın "hangisi daha heyecanlı" yarışını ses düzeyinden okumaya benzer.

## Milyon başına: CPM

İlk düzeltme basit: her sayımı kendi kütüphanesine böl, milyonla çarp. Adı CPM (counts per million) ve anlamı şu: bu gen, o örnekteki her bir milyon okumadan kaçını aldı? Derinlik paydaya girince yanılsama söner: Gapdh'nin WT CPM'leri benim koşumda 1.056, 1.210 ve 1.012 — WT_3'ün megafonu elinden alındı, üç kopya aynı sesle konuşuyor. Çözülen sorunun adını koyalım: **derinlik**. Ama rehber burada bitmiyor, çünkü CPM'in gizli bir varsayımı var.

## Milyon bir pastadır

Varsayım şu: milyon, herkese adil dağıtılan bir bütçedir. Değildir; milyon sabit bir pastadır. Bir gen delirip dev bir dilim alırsa, sayımı hiç değişmeyen bütün öteki genlerin dilimi küçülür.

Defterde bunu bir deneyle gösteriyorum: WT_1'in kopyasını alıp *sadece* Afp'yi 50 katına çıkardım; Gapdh, Actb ve Slc4a1'in sayımlarına dokunmadım. Sonuç: sahte kütüphane %26 büyüdü ve üç suçsuz genin CPM'i de tam %21 düştü — Gapdh 1.056'dan 836'ya, Actb 1.933'ten 1.530'a, Slc4a1 5.997'den 4.748'e. Buradaki parmak izi önemli: herkes *aynı oranda* düşüyorsa suç genlerde değil, pastadadır. Buna kompozisyon etkisi denir ve bizim verinin hikâyesiyle doğrudan bağı var: eritroid dokuda globin mRNA'sı okumaların yarısına varan bir dilim yiyebilir; Veri sayfasındaki arındırmanın bütün sebebi, pastayı bu devden kurtarmaktı.

## DESeq2'nin cetveli: oranların medyanı

Kompozisyona dayanıklı tarif, DESeq2'nin *median-of-ratios* yöntemi. Üç adım: her gen için bütün örneklerin geometrik ortalamasını al — bu hayali "referans örnek" olur; her örnekte her genin sayımını referansa oranla; o örneğin size factor'ı bu oranların medyanıdır. Medyanın sihri şurada: Afp gibi bir dev delirse bile sıralamada sadece bir gendir, ortadaki sıradan gen konuşur; cetveli devler değil, tipik gen kurar.

Defterde bu cetveli hazır fonksiyon olmadan, on satır pandas ile kuruyoruz. Benim koşumda referansa 18.470 gen girdi (geometrik ortalama sıfır sevmez; her örnekte sıfırdan büyük olan genler) ve altı faktör şöyle çıktı: WT_1 için 0,76 · WT_2 için 0,63 · WT_3 için 1,98 · KO_1 için 0,88 · KO_2 için 1,28 · KO_3 için 0,96. Bunları kütüphane büyüklüğünden türeyen faktörlerle karşılaştırınca sapma %2 ile %15 arasında — ve sapmanın *yönü* asıl haber: WT'lerde hep aşağı, KO'larda hep yukarı. İki grup, milyonlarını farklı genlere harcıyor. Kütüphane büyüklüğü bu farkı göremez; oranların medyanı görür. Aynı tarifi sekizinci rehberde PyDESeq2 kendi içinde işletecek ve el yapımı cetvelimizle karşılaştırma şansımız olacak.

## Normalizasyondan sonra

Normalize sayım = ham sayım / size factor. Gapdh'nin WT üçlüsü artık hizada. Ama bir inceliğe dikkat: normalize *toplamlar* eşitlenmez — benim koşumda WT'ler 16-19 milyon bandında, KO'lar 14 milyon civarında kaldı. Bu bir hata değil; medyan-oran toplamı değil *tipik geni* hizalar. Grafikte kalan WT-KO farkı, az önce ölçtüğümüz kompozisyon farkının imzasıdır: bir grubun pastasında devler daha çok yer kaplıyor.

Bir de sınır çizelim: normalizasyon teknik cetveli düzeltir, biyolojiye dokunmaz. Gapdh'nin KO sütunlarındaki yüksekliği normalizasyondan sonra da duruyor; "bu fark gerçek mi, gürültü mü" sorusunun cevabı cetvelde değil istatistikte — yani bir sonraki rehberde.

## Neler ters gider?

Üç klasik tuzak. Birincisi ve en sinsisi: PyDESeq2'ye normalize edilmiş tablo vermek. DESeq2 ailesi **ham sayım** ister; cetveli kendi kurar. Normalize veri verirseniz çifte normalizasyon olur ve hata mesajı bile almadan yanlış sonuç alırsınız. Normalize tablo görselleştirme ve göz kontrolü içindir, modelin girdisi değil. İkincisi: CPM ile aynı örnek *içinde* iki farklı geni karşılaştırmak. Uzun genler sırf uzun oldukları için daha çok okuma toplar; örnek içi gen karşılaştırması uzunluğu da hesaba katan TPM'in işidir — dördüncü rehberdeki quant.sf dosyasının TPM sütununu hatırlayın. CPM örnekler arasını, TPM örnek içini konuşur. Üçüncüsü: size factor hesabında sıfırları unutmak. Geometrik ortalama tek bir sıfır görünce çöker; referansa yalnız her örnekte ifade edilen genlerin girmesi hile değil, tarifin parçasıdır.

## Kendin dene

Defterin sonunda üç görev var. Altı size factor'ı kendi koşunuzdan not edin; tablo da tarif de aynı olduğu için sayılarınız benimkilerle birebir çıkmalı. "Kütüphane büyüklüğüne bölmek yetmez, çünkü..." cümlesini bu rehberde gördüklerinizle tamamlayın. Ve bir av: normalize tabloda WT ortalaması 1.000'in üzerinde olup KO/WT oranı en küçük olan geni bulun. Adını bir kenara yazın ve kimseye söylemeyin; sekizinci rehber, baştan sona o genin duruşması olacak.
