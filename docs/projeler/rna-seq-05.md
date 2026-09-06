# Kara kutuyu aç: pseudoalignment aslında ne yapar?

!!! question "Bu rehberde"
    Salmon'un içindeki iki fikri, elle sayabileceğiniz kadar küçük bir örnekte kendiniz kuracaksınız: k-mer dizini ve olasılıkla paylaştırma. Bundan sonra quant.sf'ye bakışınız değişecek.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/05_kara_kutu.ipynb){ .md-button .md-button--primary }

## Neden kara kutu açıyoruz?

Bir aracı çalıştırmak ile ona güvenmeyi bilmek farklı şeyler. Salmon'a altı örnek saydırdık; ama "NumReads neden küsuratlı", "eşleşme neden düşer", "izoformlar nasıl ayrılır" sorularının cevabı aracın içinde. Bu rehberde içeri giriyoruz; veri yok, oyuncak var: üç transkript, dört okuma, k=5. Gerçekte k=31 ve sayılar milyonlarca; mantık birebir aynı.

## Birinci fikir: kitap yerine dizin

Dizin (indeks) şu soruyu önceden cevaplar: kataloğun her 5 harflik parçası hangi transkriptlerde geçiyor? Bir okuma geldiğinde artık aramayız; okumanın bütün 5'liklerini dizinden bakar, hepsinin *birden* geçtiği transkriptleri buluruz. Kesişim tek transkriptse okuma imzalıdır; birden çoksa kararsızdır; boşsa eşleşmemiştir. Üçüncü durum tanıdık gelmeli: adaptör kuyruklu okumaların k-mer'leri katalogda yoktur, kesişim boşalır, okuma düşer. FastQC rehberindeki bulgumuzun mekanizması budur.

## İkinci fikir: tanıklar oy kullanır

Kararsız okumalar kaçınılmaz; izoformlar aynı ekzonları paylaşır. Salmon bunları zorla bir yere yazmaz, olasılıkla paylaştırır ve paylaştırmayı döngüyle inceltir (adı EM algoritması): bir transkriptin *imzalı* okumaları çoksa, kararsız okumalardan alacağı pay da her turda büyür. Defterde bunu gözle görüyorsunuz: T1'in tek tanığı, iki kararsız okumayı tur tur kendine çekiyor. NumReads'in küsuratı işte bu payların toplamıdır; 2437,8 okuma bir hata değil, dürüst bir muhasebedir.

## Bu bilgi ne işe yarar?

Üç yerde. Birincisi, quant.sf okurken: transkript düzeyi sayılar paylaştırma içerir, bu yüzden gen düzeyine toplamak (bizim yaptığımız) daha sağlamdır. İkincisi, sınırları bilirken: imzalı okuması az olan izoformların sayıları oynaktır; ileri modüllerdeki DTU analizinin bütün zorluğu budur. Üçüncüsü, hata ayıklarken: eşleşme oranı düştüğünde artık "araç bozuk" demezsiniz; "okumalarımın k-mer'leri katalogda karşılık bulamıyor; neden?" diye sorarsınız ve bu soru sizi adaptöre, kirliliğe ya da yanlış referansa götürür.

## Neler ters gider?

Oyuncağımız bilerek kırılgan: tek harf hatası okumayı düşürür. Gerçek Salmon bundan dayanıklıdır; kesin kesişim yerine daha esnek bir arama ve doğrulama yapar, yine de aynı ilkeyle. İkinci uyarı: EM sihir değildir; tanık yoksa (defterin üçüncü deneyi) paylaştırma belirsiz kalır ve araç size bunu söylemez, küsuratlı ama oynak sayılar verir. Aracın sustuğu yerde şüpheyi siz taşıyacaksınız.

## Kendin dene

Defterin sonundaki üç deney sırayla derinleşiyor: harf boz, tanık ekle, tanığı sil. Üçüncüsünü yaptıktan sonra şu cümleyi kendi kelimelerinizle tamamlayın: "İzoform kantifikasyonu zordur, çünkü..." Cevabınızı saklayın; DTU modülüne geldiğimizde ilk cümleniz bu olacak.
