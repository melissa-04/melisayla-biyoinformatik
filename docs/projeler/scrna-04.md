# Kümeleme: Leiden

!!! question "Bu rehberde"
    Komşuluk grafiği üzerinde Leiden algoritmasıyla kümeleme yapacak, resolution parametresinin küme sayısını nasıl değiştirdiğini iki koşuda görecek ve "hangi çözünürlük doğru" sorusunun cevabının nerede aranacağını öğreneceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/scrna/04_kumeleme.ipynb){ .md-button .md-button--primary }

## Kümeleme nerede yapılır

Kural baştan: kümeleme UMAP resmi üzerinde değil, önceki rehberde kurduğumuz komşuluk grafiği üzerinde yapılır. UMAP iki boyuta indirilmiş bir gösterimdir ve indirirken bilgi kaybeder; algoritmanın çalıştığı yer, hücreler arası gerçek komşulukları tutan grafiktir. UMAP'ı yalnız sonucu boyamak için kullanacağız.

## Leiden ve resolution

`sc.tl.leiden(a, resolution=..., key_added=...)` bir topluluk bulma algoritmasıdır: grafiği, içinde bağlantıların sık, gruplar arasında seyrek olduğu parçalara böler. `resolution` bölünme inceliğini kontrol eder — büyük değer daha çok ve daha küçük küme, küçük değer daha az ve daha iri küme üretir; doğru bir değeri yoktur, veri ve soruya göre seçilir. `key_added` sonucun `a.obs` tablosuna hangi sütun adıyla yazılacağını belirler; sonuç her hücreye bir küme numarası veren kategorik bir sütundur. Kurulumda `scanpy`'ye ek olarak `leidenalg` ve `igraph` paketleri gerekir; Leiden'ın motoru bunlardır.

## Benim koşumun sayıları

Aynı grafiğe iki çözünürlükle baktım. resolution=1.0 ile 13 küme çıktı; en büyüğü 433, en küçüğü 13 hücre. resolution=0.5 ile 9 küme çıktı: 701, 489, 426, 348, 311, 223, 147, 36 ve 13 hücre. İki sonuç da aynı verinin meşru bir bölümlemesidir; algoritma hata yapmıyor, aynı yapıya iki farklı incelikte bakıyor.

Küme boyu tablosunu okumayı öğrenin: numaralar boya göre sıralıdır ve numara bir etikettir, kimlik değildir — "küme 0" bir hücre tipi adı değil, geçici bir addır. Tablonun dibindeki küçük kümeler ayrıca dikkat ister: 13 hücrelik bir grup gerçek ama nadir bir hücre tipi de olabilir, kümeleme gürültüsü de. Bunu küme boyu söyleyemez; cevabı bir sonraki rehberin konusu olan işaretçi genler verir.

## Hangi çözünürlük doğru

Bu aşamada karar verilemez ve verilmemelidir. Karar ölçütü şudur: iki ayrı kümenin işaretçi genleri birbirinden ayrışmıyorsa çözünürlük fazladır, tek bir kümenin içinde iki farklı işaretçi profili saklanıyorsa azdır. Bir sonraki rehberde her kümenin işaretçilerini çıkarıp bu soruya veriyle döneceğiz.

## Neler ters gider?

Üç sık hata. Birincisi, kümelemeyi UMAP koordinatları üzerinde yapmak: iki boyuta sıkıştırılmış gösterim üzerinde kümeleme, gösterimin bozulmalarını sonuca taşır; girdi her zaman komşuluk grafiğidir. İkincisi, resolution'ı verinin özelliği sanmak: o bir analiz kararıdır ve raporda değeriyle birlikte yazılır; "13 hücre tipi bulduk" cümlesi, "resolution=1.0'da 13 küme bulduk" cümlesinin kestirmesi değildir. Üçüncüsü, küme numarasına kimlik yüklemek: numaralar koşudan koşuya, hatta paket sürümünden sürüme değişebilir; kalıcı olan, işaretçilerle kurulacak hücre tipi adlarıdır.

## Kendin dene

Defterin sonunda üç görev var. İki çözünürlükteki küme sayılarını ve boy tablolarını kendi koşunuzdan not edin. Önceki rehberde UMAP'a bakarak göz kararı saydığınız grup sayısını hatırlayın ve tek cümleyle yazın: gözünüz hangi çözünürlüğe daha yakın saymış? Ve en küçük kümenin hücre sayısını yazıp tek cümleyle cevaplayın: bu grubun gerçek bir hücre tipi mi, gürültü mü olduğunu ne belirler? Cevabınız bir sonraki rehberin ilk cümlesi olacak.
