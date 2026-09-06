# Sanık mı, kurban mı?

!!! question "Bu rehberde"
    Bir analiz hatasının anatomisini çıkaracaksınız: en çok çöken genin neden nakavt olmadığını sayılarla görecek, hedef ağını okuyacak ve "kriter neyi ararsa onu bulur" kuralını kendi cümlenizle yazacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/11_sanik.ipynb){ .md-button .md-button--primary }

## Hata

Bu rehber bir düzeltmedir ve düzeltilen hata bu serinin yazarınındır — yani benim. Sekizinci rehber Ahsp'yi "nakavt edilen gen" ilan etti. Oysa Rehber 1 deneyi baştan tarif etmişti: bu bir Klf1 nakavtı. İki metin aylarca aynı sitede, birbirine üç tık uzaklıkta, birbiriyle çelişerek durdu. Yakalanma şekli de öğretici: bir okuyucu değil, bir dosya taşıma işlemi sırasında Rehber 1'i yeniden okumak yakaladı. Sonucu tasarım belgesiyle çapraz kontrol etmeyen herkesin başına gelebilir; bana geldi.

## Kanıt: iki desen

Benim koşumda, normalize ortalamalarla: Klf1, WT'de 11.449'dan KO'da 1.296'ya iniyor — KO/WT oranı 0,11. Ahsp ise 99.368'den 13'e — oran 0,0001. İkisi de "düşüş" ama desenler farklı türden. Klf1 sekizde birine inmiş, fakat susmamış: KO örneklerinde bin küsur sayımlık bir kalıntı var. Nakavt kasetleri geride sıklıkla işlevsiz bir transkript bırakır ve RNA-seq molekül sayar, işlev ölçmez — Rehber 1'in "RNA-seq'in ölçmediği şeyler" listesine bir madde daha. Ahsp'nin susuşu ise mutlak: bir transkripsiyon faktörü gittiğinde kendi geni azalır, ama tamamen ona bağımlı bir hedefin promotörü kapanır.

Yedinci rehberdeki av kriterim "en küçük KO/WT oranı"ydı. O kriter, tanımı gereği, nakavtı değil en bağımlı kurbanı bulur. Klf1 o gün de oradaydı — volkanda lfc −3,1 ve 10⁻³³ mertebesinde padj ile — ama kimse ona bakmadı, çünkü soru başka şey soruyordu.

## Hedef ağı

Tek kurban hikâye kurmaz; ağ kurar. KLF1'in bilinen hedeflerinden bir panelin KO/WT oranları, benim koşumda: Ahsp 0,0001 · Dmtn 0,006 · Hbb-bs 0,015 · E2f2 0,10 · Epb42 0,34 · Slc4a1 0,34 — ve embriyonik Hbb-y ters yönde, 9,4 kat yukarıda, çünkü KLF1 embriyonik→yetişkin globin anahtarlamasının da düğmesidir. Geriye dönüp bakınca serinin bütün resimleri aynı şeyi çekmiş: sekizin volkanındaki sol kanat, dokuzun zenginleştirme tablosundaki eritroid takım, onun ısı haritasındaki tanıdıklar tabakası — hepsi tek bir düzenleyicinin ağının çöküşü.

Hbb-bs bilmecesi de son hâlini alıyor. Sekizde vardığımız cümle "düşüş teknikle karışık, ihtiyatla" idi; şimdi tamamlanıyor: düşüşün sağlam bir biyolojik bacağı var — Hbb-bs bir KLF1 hedefidir — ve arındırma belirsizliği bu bacağın üstüne biner. Yön güvenilir, büyüklük değil.

## Üç kural

Bir: kriter neyi ararsa onu bulur. "En çok çöken gen" sorusunun dürüst cevabı nakavt değil, en bağımlı hedeftir; nakavtı arıyorsanız soruya tasarım bilgisini katmak zorundasınız. İki: tasarım belgesi analizin süsü değil parçasıdır. Sonuç, deneyin tarifiyle çapraz kontrol edilmeden ilan edilmez; bu seride tarif Rehber 1 ve Veri sayfasıydı. Üç: yanlış ilan edildiyse düzeltme görünür yapılır. Sekizinci rehberin başındaki düzeltme kutusu silinmeyecek; bilim yazısında yara izi, yara olmadığı iddiasından daha güven vericidir.

## Kendin dene

İki kısa görev. Kendi koşunuzdan Klf1 ve Ahsp'nin KO/WT oranlarını yazın ve tek cümleyle bitirin: sanık hangisi, kanıtınız ne? Sonra kendi kuralınızı yazın: bir sonraki projede elinize bir DE listesi geçtiğinde "nakavt edilen gen hangisi" sorusuna nasıl yaklaşacaksınız — tek cümle, sizin cümleniz.
