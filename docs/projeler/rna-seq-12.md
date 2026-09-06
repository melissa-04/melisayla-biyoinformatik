# Kapanış: uçtan uca tek sayfa

!!! question "Bu rehberde"
    Bu sayfada kod yok: yolun tek sayfalık haritası, serinin sayılarla özeti ve on kural. Koşmak isteyenler için serinin tamamını tek akışta toplayan kapanış defteri aşağıdaki düğmede.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/rna-seq/12_uctan_uca.ipynb){ .md-button .md-button--primary }

## Yolun haritası

Bir tüpteki RNA ile başladık ve bir düzeltme kutusuyla bitirdik; arası şöyleydi. Hücreden çıkan RNA, kütüphane hazırlığından geçip FASTQ dosyalarına döküldü (1.1-1.3); FastQC ile okumaların sağlığına baktık (1.4); Salmon, pseudoalignment ile okumaları transkriptlere saydı (1.5-1.6); sayımları gen düzeyinde bir tabloya toplayıp tanıdık (1.7); median-of-ratios cetvelini elle kurup örnekleri aynı ölçeğe getirdik (1.8); PyDESeq2 ile diferansiyel ifadeyi test edip volkanı çizdik (1.9); anlamlı listeyi takımlar hâlinde sorguladık (1.10); PCA ve ısı haritasıyla kamerayı geri çekip örneklerin kendisine baktık (1.11); ve en çok çöken genin nakavt olmadığını fark edip sanığın kimliğini düzelttik (1.12).

## Serinin sayılarla özeti

Tablo 56.065 gen satırıyla geldi; toplamı 10'un altında kalan sessizler elenince 21.994 gen kaldı. Kütüphaneler 12 ile 32 milyon okuma arasındaydı ve WT_3 en derin örnek olarak 2,4 katlık bir fark taşıyordu; elle kurduğumuz size factor altılısı (0,76 · 0,63 · 1,98 · 0,88 · 1,28 · 0,96) bu farkı ödedi ve PyDESeq2'nin kendi cetveliyle virgülden sonra iki hanede birebir çıktı. Test, benim koşumda 1.388 geni anlamlı buldu: 632 yükselen, 756 düşen. Aynı defter başka bir makinede 1.385 bulur; üç genlik bu fark 7. kuralın canlı örneğidir ve kararların tekil sayıya değil kümeye yaslanmasının sebebidir. Zenginleştirme, düşenlerin eritrosit zar iskeletinde (6/11 örtüşme) ve hem biyosentezinde toplandığını; PCA, ilk eksenin yüzde 45,8'lik payla genotipi kendiliğinden ayırdığını gösterdi. Isı haritası, künyede yazmayan bir değişkeni — cinsiyeti — ihbar etti; dağılım iki grupta da karışık çıktığı için sonuçları genotiple karıştırmadı, ama bir sonraki analizin modeline yazılmayı hak etti.

Ve kimlikler: nakavt edilen gen Klf1 (KO/WT oranı 0,11 — sessiz ama kesin, padj 10⁻³³ mertebesinde), en derin çöküş ise onun en bağımlı hedefi Ahsp'de (0,0001). Ağın geri kalanı aynı hikâyeyi anlattı: Dmtn 0,006, Hbb-bs 0,015, E2f2 0,10, Epb42 ve Slc4a1 0,34 — ve embriyonik Hbb-y ters yönde 9,4 kat, çünkü KLF1 globin anahtarlamasının düğmesi.

## On kural

1. Tasarım belgesi analizin parçasıdır; sonuç, deneyin tarifiyle çapraz kontrol edilmeden ilan edilmez.
2. Kriter neyi ararsa onu bulur; soruyu yazarken cevabın türünü de yazmış olursun.
3. Modele ham sayım verilir; normalize tablo gözün, ham tablo istatistiğindir.
4. Evren, listenin seçilebileceği genlerdir; genom değil.
5. Karar padj ile ve etki büyüklüğüyle birlikte verilir; tekil p değerine tapılmaz.
6. Anlamlı, güvenilir demek değildir; teknik gölgeler istatistiği kandırabilir — hüküm vermeden veri sayfası okunur.
7. Araç sürümleri rapora yazılır; uç p değerlerinde tekil sıraya değil kümeye güvenilir.
8. Negatif kontrolsüz tablo okunmaz; her şeye anlamlı diyen test hiçbir şeye anlamlı diyemez.
9. Gözetimsiz resimler analizin başında çizilir; ihbar ettikleri değişken künyeye yazılır ve modele eklenir.
10. Hata görünür düzeltilir; yara izi, yara olmadığı iddiasından daha güven vericidir.

## Öğrenmediklerimiz

Bu seri bir çekirdek yoldu; kapıları açık bıraktık. Cinsiyeti modele katan çok değişkenli tasarım (~cinsiyet + genotip), izoform düzeyi ve alternatif splicing (DTU), ko-ekspresyon ağları (WGCNA), GEO'dan gerçek veri indirip aynı hattı sıfırdan koşmak — ve dokunun ortalamasında kaybolan sesleri duymak için tek hücre, yani ikinci proje. Sırası geldikçe hepsine gireceğiz.

## Kendin dene: son görev

Kâğıt kalem yeter. Yukarıdaki on kuraldan birini seçin ve bir sonraki projenizde onu nasıl uygulayacağınızı üç cümleyle yazın. Kuralların ezberi olmaz; ancak sizin cümlenizle yazıldıklarında sizin olurlar. Seri burada bitiyor; yol bitmiyor.
