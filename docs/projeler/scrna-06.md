# Adlandırılmış harita ve ortalamanın sakladığı

!!! question "Bu rehberde"
    Küme numaralarını hücre tipi adlarına çevirecek, haritayı adlarla boyayacak ve iki serinin buluşma noktasını tek sayıyla ölçeceksiniz: toplu ortalama neyi saklar?

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/scrna/06_adli_harita.ipynb){ .md-button .md-button--primary }

## Adları işlemek

`a.obs.leiden.map({...})` her küme numarasını 2.5'te kurduğumuz eşlemenin karşılığıyla değiştirir; sonucu kategorik sütun olarak `a.obs['hucre_tipi']`ne yazıyoruz. Koşumda dokuz tipin dağılımı şöyle: CD4 T 701, T (ribozomal-yüksek) 489, CD14+ monosit 426, B 348, CD8/efektör T 311, FCGR3A+ monosit 223, NK 147, dendritik 36 ve trombosit 13 hücre. `sc.pl.umap(color='hucre_tipi', legend_loc='on data')` adları haritanın üstüne yazar — altı rehber önce satırlar ve sütunlardan ibaret olan matris, artık adları olan bir doku haritası.

## Ortalamanın sakladığı

Birinci serinin kapanış sorusu şuydu: toplu (bulk) ortalama bizden ne saklar? Artık ölçebiliyoruz. `a.raw.to_adata()` tam log-normalize matrisi geri verir; `np.expm1` log1p dönüşümünün tersini alıp değerleri CP10K ölçeğine döndürür. Sonra her gen için iki ortalama karşılaştırılır: genin bütün 2.694 hücredeki ortalaması — bir bulk deneyin göreceği tek sayı — ve aynı genin kendi hücre tipindeki ortalaması.

Koşumun sonuçları: MS4A1'in tüm-hücre ortalaması 1,7 iken B hücrelerinde 11,9 (7 kat); CD8A 0,9'a karşı CD8/efektör T'de 4,2 (5 kat); LYZ 38,8'e karşı CD14+ monositlerde 183,3 (5 kat). Ve cevabın kendisi: **PPBP, tüm-hücre ortalamasında 2,5 — trombositlerde 465,7. Yüz seksen sekiz kat.** On üç hücrelik, işaretçileriyle kanıtlanmış, tamamen gerçek bir hücre tipi, ortalamanın içinde neredeyse yok hükmünde. Bulk RNA-seq bu dokuya baksaydı PPBP'yi önemsiz bir gen sanırdı; sinyal yanlış olduğu için değil, 13 hücrenin sesi 2.694'ün ortalamasında eridiği için. Birinci serinin sorusunun sayısal cevabı bu: ortalama, azınlığı saklar.

## Neler ters gider?

Üç uyarı. Birincisi, eşlemeyi başka veriye taşımak: küme numaraları ve adlar bu koşuya aittir; başka bir veri setinde, başka parametrelerle numaralar da sınırlar da değişir — taşınabilir olan yöntemdir, etiketler değil. İkincisi, ölçek karışıklığı: hangi matrisin ham, hangisinin CP10K, hangisinin log olduğunu her adımda bilmek zorundasınız; expm1'i log alınmamış veriye uygulamak sessizce saçma sayılar üretir. Üçüncüsü, buradaki kat sayılarını istatistiksel test sanmak: bu bir gösterimdir; "bu tip bu geni anlamlı ifade ediyor" iddiası, birinci serideki gibi uygun bir testle kurulur.

## Serinin kapanışı

Altı rehberde şu yol yüründü: veri mutfağı ve Zenodo arşivi (2.1), normalizasyon ve değişken gen seçimi (2.2), PCA-komşuluk-UMAP (2.3), Leiden kümeleme (2.4), işaretçilerle kimlik (2.5) ve adlandırılmış harita ile bulk köprüsü (2.6). Veri, serinin kendi DOI'sinde yaşıyor: 10.5281/zenodo.22655336. Birinci serinin on kuralı burada da geçerliydi; bu seri üstüne üç yenisini ekledi: kümeleme grafikte yapılır, UMAP'ta değil; UMAP'tan nicel iddia çıkmaz; ve etiket taşınmaz, yöntem taşınır.

Öğrenmediklerimiz sonraki serilerin kapıları: çoklu örnek ve parti (batch) düzeltmesi, veri entegrasyonu, hücre yörüngeleri (trajectory), hücre-hücre iletişimi. Sırası geldikçe.

## Kendin dene: son görev

Dört genin tüm-hücre ve tip-içi ortalamalarını kendi koşunuzdan hesaplayıp katları not edin. Sonra tek cümle yazın: bir sonraki projenizde bulk bir sonuç gördüğünüzde kendinize hangi soruyu soracaksınız? O cümle bu serinin size kalan özetidir.
