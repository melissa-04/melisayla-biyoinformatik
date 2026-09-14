# Seçici bağımlılıklar

!!! question "Bu rehberde"
    Beş bin gen içinden KRAS gibi davrananları sayısal bir ölçütle ayıklayacak, onları medyan-değişkenlik haritasına işaretleyecek ve her adayın bağımlılığının hangi dokuda yoğunlaştığını bulacaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/depmap/04_secici_bagimliliklar.ipynb){ .md-button .md-button--primary }

## Soruyu tersine çevirmek

4.3'te tek bir genin nasıl seçici davrandığını gördük. Burada soru tersine dönüyor: elimizde 5.000 gen var, hangileri bu profili gösteriyor? Aradığımız şey bir grup hatta ölümcül, geri kalanında etkisiz olan genler — ne ortak esansiyeller (4.2'de gördüğümüz gibi kötü hedefler), ne de hiçbir yerde iş görmeyenler.

Ölçütü üç koşulla kurduk ve her birinin gerekçesi var. Bağımlı hat sayısı en az 10 olmalı, çünkü 2-3 hatlık bir sinyal aykırı değer olabilir ve ilaç geliştirmek için yeterince geniş bir hasta grubu tanımlamaz. Bağımlı hat oranı yüzde 40'ın altında kalmalı, çünkü üstüne çıkınca gen ortak esansiyele yaklaşır ve terapötik pencere kapanır. Ve bağımlı olmayan hatlarda medyan skor −0,25'in üstünde olmalı — bu, "geri kalan hatlar gerçekten umursamıyor" koşulu; olmazsa her yerde biraz ölümcül olan genler listeye sızar.

## Sonuç: 297 aday

Koşumda 5.000 genden **297'si** bu ölçütü geçti, yani yaklaşık yüzde 6. Sayının kendisi öğretici: bağımlılık haritasının büyük kısmı ya her yerde ölümcül ya hiçbir yerde etkili genlerden oluşuyor, kullanılabilir seçicilik nadir bir özellik.

Haritada nerede durduklarına bakın: adaylar sol alt köşede değil, medyanı sıfıra yakın ama standart sapması yüksek olan sağ üst bölgede toplanıyor. Projenin tezi bu resimde: iyi hedef en çok öldüren gen değil, **bazılarını öldürüp diğerlerini bağışlayan** gendir.

## Adayların adresleri

Bir seçici bağımlılık ancak bağımlı hatların ortak bir özelliği varsa kullanışlıdır. En basit ortak özellik dokudur ve koşumun tablosu bunu çarpıcı biçimde veriyor:

- **NMNAT1** — 139 bağımlı hat, lenfoid hatların yüzde 68,8'i
- **CBFB** — 107 hat, miyeloid hatların yüzde 64,4'ü
- **CTNNB1** — 72 hat, bağırsak hatlarının yüzde 60,3'ü
- **HNF1B** — 78 hat, böbrek hatlarının yüzde 60'ı
- **KLF5** — 133 hat, bağırsak hatlarının yüzde 46'sı
- **EFR3A** — 152 hat, pankreas hatlarının yüzde 37,5'i

Bu satırların birkaçı bilinen kanser biyolojisiyle doğrudan örtüşüyor. CTNNB1 beta-katenin kodlar ve Wnt yolağının merkezindedir; kolorektal kanserlerin çok büyük kısmında bu yolak APC ya da CTNNB1 mutasyonlarıyla sürekli açık kalır — bağırsak hatlarının beta-katenine bağımlı çıkması beklenen sonuçtur. CBFB, akut miyeloid lösemide sık görülen füzyon onkogenlerinin ortağıdır; miyeloid hatlarda yoğunlaşması aynı hikâyeyi anlatır. BRAF listede 60 bağımlı hatla görünüyor ve melanom biyolojisiyle uyuşuyor.

Ve en önemlisi: bu ilişkilerin hiçbirini biz veriye söylemedik. Ölçüt tamamen istatistikseldi — bağımlı hat sayısı, oran, kalan medyan. Bilinen onkogenlerin ve doku ilişkilerinin kendiliğinden yukarı çıkması, yöntemin işlediğinin göstergesi.

## Listenin dürüst okunuşu

Tablodaki her satır bir ilaç hedefi değildir ve bunu açıkça söylemek gerekir. Üç sınır var.

Birincisi, seçicilik nedeni her zaman onkogen bağımlılığı değildir. NMNAT1 gibi bir NAD sentez enzimi ya da PMM2 gibi bir metabolik enzim, belirli hatlarda paralog kaybı ya da metabolik bağlam yüzünden yaşamsal hale gelebilir — bu da gerçek bir bağımlılıktır ama mekanizması farklıdır.

İkincisi, ölçüt eşiklere bağlıdır. Yüzde 40 yerine yüzde 25 deseydiniz liste küçülür, −1 yerine −0,75 deseydiniz büyürdü. Eşikler raporda yazılır ve sonuçlar onlarla birlikte okunur.

Üçüncüsü, "hedeflenebilirlik" bu tabloda yoktur. Bir genin ürünü ilaçla bağlanabilir bir cebe sahip olmayabilir; transkripsiyon faktörleri (KLF5, IRF4, HNF1B) bunun tipik örneğidir. Bağımlılık listesi hedef listesinin girdisidir, kendisi değil.

## Sıradaki soru

Adres tablosu "hangi dokuda" sorusunu cevapladı ama "neden" sorusunu açık bıraktı. Bağırsak hatları CTNNB1'e neden bağımlı — hepsi Wnt yolağı mutasyonu mu taşıyor? Bu, bağımlılık skorunu mutasyon ve ifade verisiyle ilişkilendirmeyi gerektirir ve bir sonraki rehberin konusudur. Sentetik letalite kavramı da oradan girecek.

## Kendin dene

Defterin sonunda üç görev var. Seçici aday sayınızı ve en çok bağımlı hattı olan beş geni not edin. Adres tablosundan bilinen kanser biyolojisiyle uyuşan en az iki satır seçip neden uyuştuğunu tek cümleyle yazın. Ve haritadaki mor noktaların konumuna bakıp tek cümleyle açıklayın: ilaç hedefi ararken neden sol alt köşeye değil sağ üst bölgeye bakıyoruz?
