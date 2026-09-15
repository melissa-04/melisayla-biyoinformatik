# Kapanış: kendi hedefini seç

!!! question "Bu rehberde"
    Dört rehberde kurulan adımları tek parametrik akışa toplayacak, seçtiğiniz kanser tipine özgü bağımlılıkları bulacak ve bir adayı eleme listesinden geçirerek kısa bir rapor yazacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/depmap/06_kendi_hedefin.ipynb){ .md-button .md-button--primary }

## Akış

Defterde değiştireceğiniz tek şey iki satır: `DOKU` ve istersen `GEN`. Gerisi dört rehberin adımlarını sırayla koşar — veriyi okur, seçilen dokunun hatlarını ayırır, o dokuya özgü bağımlılıkları süzer, bir adayı yakından çizer ve mutasyonla ilişkilendirmeyi dener.

Ölçüt 4.4'ün mantığını bir adım ileri taşıyor. Orada "bazı hatlarda ölümcül" diyorduk; burada "bu dokuda ölümcül, diğerlerinde değil". Üç koşul: seçilen dokudaki hatların en az yüzde 30'u bağımlı olsun (sinyal yaygın olsun), diğer dokulardaki bağımlılık oranı yüzde 10'un altında kalsın (terapötik pencere açık kalsın) ve iki grubun medyanı arasındaki fark en az 0,3 olsun (fark yalnız eşik oyunundan doğmasın).

## Örnek koşu: pankreas

Pankreası seçtim — 48 hat, 1.160 karşılaştırma hattı. Sonuç beklenmedikti ve tam da bu yüzden anlatmaya değer: **ölçütü geçen tek bir gen çıktı.**

NCKAP1. Pankreas hatlarının yüzde 35,4'ü bağımlı, diğer dokularda yüzde 8,4; medyanlar −0,79 ve −0,45, fark 0,34. Eşiklerin hepsini kıl payı geçen bir aday.

Peki KRAS neden bu listede yok? 4.3'te pankreas hatlarının yüzde 83,3'ünün KRAS'a bağımlı olduğunu görmüştük. Cevap ikinci koşulda: KRAS diğer dokularda da bolca bağımlılık yaratıyor — bağırsakta yüzde 61,9, safra yollarında yüzde 38,2, akciğerde yüzde 31,7. "Diğer dokularda yüzde 10'un altında" koşulu KRAS'ı eliyor.

Bu bir hata değil, ölçütün tanımı. Burada aradığımız şey "bu kanserde önemli gen" değil, "yalnız bu kanserde ölümcül gen". KRAS birincisidir, ikincisi değil. Kanser tipine özgü hedef ararken kurduğunuz ölçüt, aradığınız şeyin tanımıdır — ve tanımı biraz değiştirince liste tamamen değişir.

## Tek aday ne anlama geliyor

Bir adaylık liste iki şey söylüyor olabilir ve ikisini ayırmak gerekir.

Birincisi: pankreas kanserinin bağımlılık profili gerçekten de diğer dokulardan keskin biçimde ayrışmıyor olabilir. Pankreas tümörlerinin sürücüsü büyük ölçüde KRAS'tır ve KRAS bağımlılığı pankreasa özgü değildir; RAS yolağını kullanan her doku aynı bağımlılığı paylaşır.

İkincisi: ölçüt fazla katı olabilir. Yüzde 30 eşiğini 20'ye indirmek, "diğer" oranını yüzde 15'e çıkarmak ya da fark eşiğini düşürmek listeyi büyütür. Defterdeki en öğretici deneme budur: eşikleri oynatıp listenin nasıl değiştiğini görmek. Çıkan sayı verinin özelliği değil, sizin sorunuzun sonucudur.

NCKAP1'in kendisi de not edilmeye değer: WAVE kompleksinin bir üyesi, hücre iskeleti düzenlenmesinde çalışır ve hotspot mutasyon matrisinde yok. Yani bu bağımlılık bir onkogen mutasyonuyla açıklanmıyor — sebebi ifade düzeyi, paralog kaybı ya da doku kökenli bir bağlam olabilir. Defter bunu açıkça söylüyor ve bu da bir bulgudur.

## Eleme listesi

Bir aday hedef olarak önerilmeden önce beş sorudan geçer; hiçbiri kod değil, hepsi yorum. Ortak esansiyel mi — genel medyanına bakın. Grup yeterince büyük mü — doku hat sayısı ve bağımlı hat sayısı sonucu taşıyabilir mi? Bilinen biyolojiyle uyuşuyor mu — uyuşmuyorsa yanlış değildir ama açıklama ister. Hedeflenebilir mi — ürünü ilaçla bağlanabilir bir protein mi? Ve hangi kanıt eksik — bunlar hücre hattı verileridir; mikroçevre, bağışıklık sistemi ve damarlanma yoktur.

## Serinin kapanışı

Altı rehberde şu yol yüründü: CRISPR ekran çıktısını tanımak ve Chronos ölçeğini okumak (4.1), altı milyon ölçümün dağılımı ve ortak esansiyel-etkisiz ayrımı (4.2), tek bir genin profili ve oran-ham sayı farkı (4.3), seçici bağımlılıkların sistematik taranması (4.4), bağımlılığı mutasyonla ilişkilendirmek ve sentetik letalite (4.5), ve kendi kanser tipiniz için uçtan uca bir tarama (4.6). Veri kendi DOI'sinde: [10.5281/zenodo.22759375](https://doi.org/10.5281/zenodo.22759375).

Bu projenin önceki üç seriden farkı, beceri türüydü: boru hattı kurmak değil, hazır bir tabloya doğru soruyu sormak. Buradan çıkan dört kural şöyle özetlenebilir. En çok öldüren gen en iyi hedef değildir; aranan şey terapötik penceredir. Gruplar karşılaştırılırken payda yazılır, ham sayı yanıltır. Eşikler sonucun parçasıdır ve raporda yer alır. Ve dosya yapısı varsayılmaz — yeni bir tablo açtığınızda ilk iş sütun adlarını yazdırmaktır.

Açık bıraktığımız kapılar: ifade ve kopya sayısı verisiyle ilişkilendirme, ko-esansiyellik ağları (birlikte gerekli olan genlerden yolak çıkarmak), MAGeCK ile ham ekran verisinin kendi başına analizi ve ilaç duyarlılığı veri setleriyle birleştirme.

## Kendin dene: bitirme görevi

Kendi kanser tipinizi seçip akışı koşun, sonra beş maddelik kısa bir rapor yazın: seçtiğiniz doku ve hat sayısı; aday sayınız ve en güçlü üç adayın değerleri; yakından incelediğiniz gen ve kutu grafiğinin yorumu; mutasyon karşılaştırmasının sonucu; ve eleme listesinden geçirdiğinizde bu adayı önerip önermeyeceğiniz, gerekçesiyle. Aday listeniz boş ya da çok kısa çıkarsa eşikleri gevşetip tekrar koşun — ve değiştirdiğiniz eşiği raporunuza yazın.
