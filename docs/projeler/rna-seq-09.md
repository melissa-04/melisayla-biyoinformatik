# Zenginleştirme: 1.385 genin ortak süreçleri

!!! question "Bu rehberde"
    Anlamlı gen listesini tek tek okumak yerine takımlar hâlinde sorgulayacak, zenginleştirme testini (ORA) hipergeometrik dağılımla kendi elinizle kuracak ve bu testin en klasik tuzağını bir deneyle göreceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/09_zenginlestirme.ipynb){ .md-button .md-button--primary }

## Listeden anlama

Sekizinci rehber bize 1.385 gen bıraktı; benim koşumda ad düzeyinde 753 düşen, 629 yükselen. Bu boyuttaki bir listeyi tek tek okumak hem imkânsız hem yanıltıcıdır: göz, bildiği genleri seçer ve hikâyeyi onlara göre kurar. Doğru soru gen değil süreç sorar: bu liste rastgele bir kalabalık mı, yoksa belli biyolojik takımlar topluca mı hareket etmiş?

## Testin bütün malzemesi: dört sayı

ORA'nın tamamı dört sayıya dayanır. N, evren: DESeq2'nin gerçekten test ettiği genler — benim koşumda 16.814. n, listenin boyu: 753. K, sorgulanan setin evrendeki üyeleri. k, setin listeyle örtüşmesi. Şans beklentisi n·K/N'dir: 11 genlik eritrosit zar iskeleti seti için 753 × 11 / 16.814 ≈ 0,5 gen. Yani şans, bu setten listeye yarım gen koyar. Gözlenen örtüşme bunun çok üstündeyse hipergeometrik dağılım kesin bir p değeri verir; on iki seti birden test ettiğimiz için p'lere sekizinci rehberden tanıdığınız Benjamini-Hochberg düzeltmesi uygulanır.

Katalog konusunda dürüst olayım: defterdeki on iki set benim derlemem — deneyin biyolojisine yakın süreçler artı birkaç ilgisiz kontrol. Gerçek analizde binlerce setlik hazır kataloglar kullanılır (GO, MSigDB'nin .gmt dosyaları) ve aynı testi dev kataloglarla koşan hazır araçlar vardır: Enrichr, g:Profiler, gseapy. Mekanizma birebir bu defterdekidir. Bir de pratik not: kataloglar gen sembolüyle konuşur, bu yüzden bu rehberde kimlikten ada geçiyoruz; yaklaşık yirmi adın birden fazla kimliğe denk gelmesi bu ölçekte sonucu değiştirmez.

## Benim koşumun tablosu

Düşenlerde tepe beşli şöyle: eritrosit zar iskeleti 6/11 örtüşmeyle padj 3,6×10⁻⁵; hem biyosentezi 5/9 ile 1,2×10⁻⁴; eritroid transkripsiyon faktörleri 3/8 ile 0,017; demir metabolizması 3/10 ile 0,025; yetişkin globinler 2/4 ile 0,027. Beşi birden aynı cümleyi kurar: düşen liste, alyuvar yapım programının kendisidir — zarı, hem sentezi, demiri, ana düğme genleri.

Tablonun dibi de en az tepesi kadar değerlidir. Otofaji, translasyon, mitokondriyal solunum ve hücre döngüsü setleri 1,0 civarında: ilgisiz kontroller zenginleşmiyor. Negatif kontrolsüz bir zenginleştirme tablosu okunmaz; her şeye "anlamlı" diyen bir test hiçbir şeye anlamlı diyemez.

## Evren tuzağı

En yaygın hata, evren olarak bütün genomu almaktır. Deney defterde: aynı düşen liste, aynı set, ama evren 16.814 yerine tablonun tamamı, 56.065. Sonuç: zar iskeletinin p değeri 3,0×10⁻⁶'dan 2,6×10⁻⁹'a iner — bin kat "iyileşir". Ama bu iyileşme sahtedir: fetal karaciğerde hiç ifade edilmeyen on binlerce gen o listeye zaten giremezdi; onları paydaya koymak her örtüşmeyi olduğundan nadir gösterir. Evren, listenin seçilebileceği genlerdir; genom değil.

## Yükselenler

Aynı testi yükselen 629 gene uygulayınca tepeye küçük bir set çıkıyor: embriyonik globinler, 3 üyesinden 2'siyle, benim koşumda padj 0,049. Sekizinci rehberin volkanında tek tek gördüğünüz Hbb-y ve Hbb-bh1 yükselişi, takım düzeyinde de aynı hikâyeyi anlatıyor. Dikkat: 0,049, eşiğin tam kıyısıdır; sürüm farkı sizde çizginin öbür yanına düşürebilir ve bu da başlı başına bir derstir — eşikler doğa kanunu değil, çizgidir.

## Tekrarlanabilirlik notu

Bu seride yaşandı, buraya yazıyorum: aynı veri, aynı kod ve aynı pydeseq2 sürümü, iki farklı yazılım ortamında (altta farklı pandas/scipy) bir genin padj sırasını 14'ten 1'e taşıdı, çünkü ortamlar tek bir genin dispersiyonunu farklı kestirdi ve p değeri 10⁻⁶⁷'den 10⁻¹⁴⁵'e kaydı. İki ders: analiz raporuna yalnız ana aracın değil bütün yığının sürümleri yazılır (defterde tek satırlık alışkanlığı var), ve bu mertebedeki p değerlerinde tekil sıralamaya değil kümeye güvenilir — iki koşuda da tepede aynı eritroid takım vardı, sadece dizilişleri farklıydı.

## Neler ters gider?

Üç tuzak. Birincisi evren seçimi; yukarıda sayısıyla gördünüz, tekrar etmiyorum. İkincisi küçük setler ve çoklu test: üç genlik bir sette 2 örtüşme etkileyici görünür ama p değeri kırılgandır ve on iki set bile BH düzeltmesi ister; yüzlerce setlik gerçek kataloglarda düzeltmesiz tablo çöp üretir. Üçüncüsü aşırı yorum: zenginleşme "bu süreç listede kalabalık" der, "bu süreç bozulduğu için hücre şu fenotipi gösterdi" demez; mekanizma iddiası ayrı deney ister. Bir de yön hatırlatması: yükselen ve düşen listeler ayrı test edilir; birleştirirseniz zıt yönlü sinyaller birbirini söndürür.

## Kendin dene

Defterin sonunda üç görev var. Düşenler tablonuzdan en güçlü setin adını, örtüşme sayılarını ve padj değerini not edin. Evren deneyinden aynı setin iki p değerini yan yana yazıp tek cümleyle yönünü ve nedenini açıklayın. Ve yükselenleri kendiniz test edin: tepeye çıkan seti değerleriyle not edin, sekizinci rehberdeki hangi bulguyla aynı hikâyeyi anlattığını tek cümleyle söyleyin — eşiğin öbür yanına düştüyse onu da yazın, çünkü o da bir bulgudur.
