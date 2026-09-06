# Harita: PCA ve ısı haritası

!!! question "Bu rehberde"
    Altı örneğin tamamını iki gözetimsiz resimde okuyacak, normalizasyonun haritadaki karşılığını görecek ve haritanın size künyede yazmayan bir değişkeni nasıl ihbar ettiğini keşfedeceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/10_harita.ipynb){ .md-button .md-button--primary }

## Kamerayı geri çekmek

Sekizinci rehber genleri tek tek yargıladı, dokuzuncusu takım takım sorguladı. Bu rehber kamerayı geri çekiyor: örneklerin kendisi birbirine ne kadar benziyor? Bunun iki klasik aracı var ve ikisi de gözetimsiz — kimin WT kimin KO olduğunu onlara söylemeyiz. Veri kendi kendini doğru dizmiyorsa gen düzeyindeki hiçbir sonuca güvenilmez; bu yüzden gerçek projelerde bu resimler analizin sonunda değil başında çizilir. Biz sona sakladık, çünkü burada ikinci bir iş daha görecekler.

## Hazırlığın üç kararı

Defterde üç karar veriyoruz ve üçünün de gerekçesi var. Sayımlara log1p uygulanır, çünkü sayım verisi çarpıktır ve birkaç dev gen logaritmasız resmi tek başına ele geçirir. İfade tabanı konur (normalize ortalama en az 20; benim koşumda 14.069 gen kaldı), çünkü zar zor duyulan genlerin log-varyansı yapaydır. Ve harita en değişken 500 genle çizilir, çünkü örnekleri ayıran şey değişen genlerdir; altı örnekte de aynı olan gen eksen kuramaz.

## PCA: sonuç

PCA'nın bütün hesabı iki satırdır — merkezle, SVD al — ve çıktısı bir eksen listesidir: her eksen, toplam varyansın bir yüzdesini taşır. Benim koşumda ilk üç pay yüzde 45,8, 27,3 ve 10,9.

Asıl haber PC1'de. Bütün WT'ler eksinin, bütün KO'lar artının tarafında ve iki grubun arasında boşluk var: verinin en büyük varyans yönü, kimseye sormadan genotipi buldu. Daha da güzeli WT_3'ün yeri: −19,4 ile kendi grubunun içinde oturuyor. Altı rehberdir uğraştığımız 2,4 katlık derinlik farkı bir eksen kuramadı, çünkü ilk hücredeki cetvel — yedinci rehberde elle kurduğunuz median-of-ratios — onu haritaya girmeden ödedi.

Bunu bir deneyle de sınadım: aynı haritayı normalizasyonsuz, ham sayımların logaritmasıyla çizince ayrım kaybolmuyor — bu deneyin etkisi o kadar büyük — ama WT_3 kendi grubundan kopup PC1'de −35,7'ye savruluyor ve resmin köşesinde tek başına kalıyor. Derinlik, biyolojinin içine eksen olarak sızıyor; etkisi bizimki kadar ezici olmayan bir deneyde bu sızıntı ayrımın kendisini yutar.

PC2 ve PC3'e gelince: yüzde 27 küçük bir pay değil, ama altı örnekle bir eksenin kimliğini ilan etmek aceleciliktir. Benim koşumda PC2'nin yükleri ağırlıkla -ps ve Gm adlı genlerde — büyük olasılıkla teknik bir gölge. PC3'ün yükleri ise başka bir şey söylüyor; onu ısı haritası ele verecek.

## Isı haritası: üç tabaka

İkinci resim gen düzeyine iner: en değişken 30 gen satırlarda, altı örnek sütunlarda, her satır kendi içinde z-skorunda — mutlak değere değil desene bakarız. Benim haritamda liste üç tabakaya ayrılıyor.

Birinci tabaka tanıdıklar: en tepede Ahsp, çevresinde Apol8, Fn3k, Dmtn, Kcnq4, Nxpe2. Volkanın yıldızları buraya kimse "anlamlı" demeden, kendi varyanslarıyla geldi; gözetimsiz haritanın gözetimli sonucu kendiliğinden doğrulaması, analizde bulabileceğiniz en ucuz güven tazeleyicidir. İkinci tabaka, adı -ps ve Gm ile başlayan kalabalık: sahte genler ve tanımsız gen modelleri. Haritalama gürültüsüne yatkındırlar; varlıkları not edilir, üzerlerine tek başına hüküm kurulmaz. Üçüncü tabaka ise bu deneyin hikâyesine ait görünmeyen üç isim: Xist, Ddx3y, Eif2s3y. Ne oldukları, örnek örnek okununca ne söyledikleri ve künyeye hangi sütunu ekletecekleri bu rehberin görevi — burada yazmayacağım.

Şu kadarını söyleyeyim: benim koşumda bu üç genin volkanda padj değeri bile yoktu, üçü de NA. Grup içi varyansları o kadar büyüktü ki DESeq2 onları teste sokmaya değer bulmadı. Harita, volkanın hiç konuşmadığı bir şeyi ihbar ediyor.

## Neler ters gider?

Üç tuzak. Birincisi, PCA'yı ham sayımla çizmek; yukarıda sayısıyla gördünüz, derinlik resme sızar. İkincisi, az örnekle eksenlere aşırı anlam yüklemek: eksen bir varyans yönüdür, mekanizma değil; altı örnekte PC2'nin "kimliği" kırılgandır, bir sonraki veri setinde bambaşka çıkabilir. Üçüncüsü, keşfedilen gizli değişkeni rapordan saklamak: gözetimsiz bir resim size künyede yazmayan bir değişken gösterdiyse, o değişken künyeye yazılır ve bir sonraki analizde modele eklenir — DESeq2'nin tasarım formülü tam bunun için "~degisken + genotip" biçimini alabilir. Bu kapı bu serinin ufkunun hemen ötesinde; varlığını bilin yeter.

## Kendin dene

Defterin sonunda üç görev var. Kendi koşunuzdan ilk üç varyans payını ve PC tablosunu not edin: PC1 grupları ayırıyor mu, WT_3 kimin yanında, ve derinlik neden eksen kuramadı — tek cümle. Sonra Xist, Ddx3y ve Eif2s3y'nin örnek başına normalize değerlerini çekin, altı örneğin her birine bu üç satıra bakarak bir etiket verin ve künyeye eklenecek sütunu adlandırın; bir örnek kurala uymayacak, hangisi olduğunu ve üç genin onun için ne söylediğini de yazın. Son olarak bu yeni değişkenin gruplara dağılımına bakın ve iki kısa cümleyle bitirin: genotiple karışmış mı, ve bu dağılım sekizinci rehberin sonuçları için neden önemli?
