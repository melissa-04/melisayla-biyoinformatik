# Bir genin profili: KRAS

!!! question "Bu rehberde"
    Tek bir genin 1.208 hattaki profilini sonuna kadar okuyacak, bağımlı hatları eşikle ayıracak ve doku karşılaştırmasında ham sayı yerine neden oran kullanıldığını göreceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/depmap/03_gen_profili.ipynb){ .md-button .md-button--primary }

## Neden KRAS

Önceki rehber bir soruyla bitti: KRAS'ın öldürdüğü hatlarla umursamayan hatlar arasındaki fark ne? KRAS'ı örnek seçmemizin nedeni var. İnsan kanserlerinde en sık mutasyona uğrayan onkogenlerden biridir, onlarca yıl "hedeflenemez" sayılmıştır ve son yıllarda ilk inhibitörleri klinikte kullanılmaya başlanmıştır. Yani hem veride hem klinikte karşılığı olan bir gen.

## Dağılımın iki ucu

Koşumda KRAS 1.208 hattın hepsinde ölçülmüş. Medyan −0,52, çeyrekler −0,82 ve −0,34, ama uçlar çok uzakta: en düşük hat −4,46, en yüksek +0,26. Bir gen hem ortak esansiyellerin en ölümcül bölgesine inebiliyor hem de sıfırın üstüne çıkabiliyor — ve bunların hepsi aynı genin aynı ölçümü.

Bağımlı hatları ayırmak için −1 eşiğini kullandık. Gerekçesi 4.1'deki ölçek tanımı: −1, ortak esansiyellerin medyanı kadar güçlü bir etki demek; bir hat KRAS için oraya iniyorsa KRAS'sız yaşayamıyor diyebiliriz. Sonuç: **1.208 hattın 219'u, yani yüzde 18,1'i KRAS-bağımlı.** Geri kalan yüzde 82 için KRAS'ı kapatmak ciddi bir sorun değil.

## Ham sayı değil oran

Bağımlı hatların dokularına bakarken kritik bir tuzak var. Koşumda akciğerden 40, bağırsaktan 39, pankreastan 40 bağımlı hat çıktı — ham sayıya bakan "üçü de benzer" der. Oysa DepMap'te 126 akciğer hattı varken yalnız 48 pankreas hattı var. Oranla bakınca tablo tamamen değişiyor:

- Pankreas: 48 hattın 40'ı bağımlı — **yüzde 83,3**
- Bağırsak: 63 hattın 39'u — yüzde 61,9
- Safra yolları: 34 hattın 13'ü — yüzde 38,2
- Akciğer: 126 hattın 40'ı — yüzde 31,7
- Meme: 53 hattın 11'i — yüzde 20,8

Pankreas hatlarının altıda beşi KRAS'a bağımlı; akciğerinkilerin üçte biri. Ham sayı bu farkı tamamen gizliyordu. Kural genel: bir grubun bir özelliği "ne kadar taşıdığını" sorarken payda mutlaka yazılır. Yanına grup büyüklüğünü de koyuyoruz, çünkü 5 hattan 4'ü ile 48 hattan 40'ı aynı güvenilirlikte değildir.

## Sonuç biyolojiyle uyuşuyor mu

Bu sıralama bağımsız olarak doğrulanabilir bir sonuç ve doğrulanıyor: pankreas duktal adenokarsinomlarının çok büyük çoğunluğu KRAS mutasyonu taşır, kolorektal kanserlerde oran yaklaşık yarıdır, akciğer adenokarsinomlarında daha düşüktür. Bizim tablomuz aynı sırayı veriyor — ve bunu yalnız bağımlılık skorlarına bakarak veriyor; mutasyon verisine hiç dokunmadık.

En bağımlı 15 hattın listesi de aynı hikâyeyi anlatıyor: AsPC-1 (−4,46), Panc 08.13, Panc 04.03, SU.86.86, MIA PaCa-2, SUIT-2, PK-45H — listenin yarıdan fazlası pankreas. Bu hat adları KRAS literatüründe standart model hatlardır.

## Analizin sınırı

Buraya kadar kurduğumuz her cümle korelasyon düzeyinde. "Pankreas hatları KRAS'a bağımlı" diyebiliyoruz; "çünkü KRAS mutasyonu taşıyorlar" diyemiyoruz, çünkü elimizdeki tabloda mutasyon bilgisi yok. Bağımlılığı mutasyonla ilişkilendirmek başka bir DepMap dosyası ister ve 4.5'in konusudur.

Bir sınır daha: bunlar hücre hattı verisidir. Hücre hatları onlarca yıl plastikte çoğaltılmış, tümör mikroçevresinden, bağışıklık sisteminden ve damarlanmadan yoksun modellerdir. Bir gene hücre kültüründe bağımlı olmak, hastadaki tümörde de aynı ölçüde bağımlı olmayı garanti etmez. DepMap bir hipotez üretme aracıdır, klinik kanıt değil.

## Neler ters gider?

Üç tuzak. Birincisi ham sayı-oran karışıklığı; yukarıda sayısıyla gördünüz. İkincisi küçük gruplara güvenmek: 8 hattı olan bir dokuda yüzde 50 oran, tek bir hattın yer değiştirmesiyle yüzde 37,5 olur — bu yüzden tabloyu en az 20 hattı olan dokularla sınırladık. Üçüncüsü eşiği unutmak: yüzde 18,1 oranı −1 eşiğine aittir; eşik −0,5 olsaydı bağımlı hat sayısı katlanırdı, sonuç raporlanırken eşik de yazılır.

## Kendin dene

Defterin sonunda üç görev var. KRAS'ın medyan, çeyrek ve uç değerlerini, bağımlı hat sayısını ve oranını kendi koşunuzdan not edin. Oran tablosundan en yüksek üç dokuyu yazıp ham sayı yerine oranla bakmanın neden gerekli olduğunu tek cümleyle açıklayın. Ve kendi seçtiğiniz bir geni aynı hattan geçirin — BRAF, EGFR, CTNNB1, SOX10 iyi adaylar; hangi dokuda yoğunlaştığını ve bunun bilinen kanser biyolojisiyle uyuşup uyuşmadığını yazın.
