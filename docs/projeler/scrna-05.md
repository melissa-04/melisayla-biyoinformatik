# İşaretçi genler ve kümelerin kimliği

!!! question "Bu rehberde"
    Her kümenin kendine özgü genlerini rank_genes_groups ile çıkaracak, bilinen işaretçi paneliyle dotplot üzerinde karşılaştıracak ve dokuz kümeye hücre tipi adı vereceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/scrna/05_isaretciler.ipynb){ .md-button .md-button--primary }

## rank_genes_groups

`sc.tl.rank_genes_groups(a, 'leiden', method='wilcoxon')` her kümeyi geri kalan bütün hücrelerle karşılaştırıp genleri sıralar. `method='wilcoxon'` sıralama tabanlı bir testtir; dağılım varsayımı az olduğu için tek hücre verisinde alan standardıdır. Önemli ayrıntı: fonksiyon ifadeleri varsayılan olarak `a.raw`'dan okur — 2.3'te sakladığımız tam log-normalize matristen. Yani yarışa daraltılmış 1.860 gen değil 13.714 genin tamamı girer; işaretçi olabilecek hiçbir gen dışarıda kalmaz. Sonuç `a.uns['rank_genes_groups']` içine yazılır; defterde küme başına ilk 5 genin tablosu ve ilk 10 genin grafiği var.

## Dotplot nasıl okunur

`sc.pl.dotplot(a, panel, groupby='leiden')` bilinen işaretçilerin küme başına ifadesini tek resimde gösterir: noktanın boyu, o kümede geni ifade eden hücre yüzdesi; rengi, ortalama ifade düzeyi. Bir kümenin kimliği, hangi işaretçi sütunlarında hem büyük hem koyu nokta verdiğine bakılarak okunur. Paneldeki eşleşmeler: CD3D/CD3E/IL7R/CCR7 T hücreleri, CD8A sitotoksik T, MS4A1/CD79A B hücreleri, LYZ/CD14 klasik monositler, FCGR3A/MS4A7 FCGR3A+ monositler, NKG7/GNLY NK, FCER1A/CST3 dendritik hücreler, PPBP trombositler.

## Benim eşlemem

Koşumda ilk-5 tablosu ve dotplot birlikte şu eşlemeyi veriyor; her satırda gerekçesi yanında:

- **0 → CD4 T (701 hücre).** CD3D ilk beşte; dotplot'ta CD3 sütunları dolu, CD8A boş, IL7R görünür.
- **1 → T, ribozomal-yüksek (489).** İlk beşi ribozomal protein genleri; CD3 sütunları dolu ama kümeyi 0'dan ayıran özgün bir işaretçi yok. Bu çözünürlükte 0 ile 1 güvenle ayrışmıyor; adı temkinli koyuyorum ve bunu bir bulgu sayıyorum — 2.4'teki "çözünürlük kararını işaretçiler verir" cümlesinin somut hali.
- **2 → CD14+ monosit (426).** S100A9, S100A8, LYZ; dotplot'ta CD14 dolu.
- **3 → B (348).** CD79A ilk beşte, MS4A1 sütunu dolu; HLA sınıf II yüksek.
- **4 → CD8 / efektör T (311).** CCL5, NKG7, GZMA; CD3 ile birlikte CD8A dolu.
- **5 → FCGR3A+ monosit (223).** LST1, FCER1G, AIF1; dotplot'ta FCGR3A ve MS4A7 dolu, CD14 zayıf.
- **6 → NK (147).** GZMB, PRF1, GNLY; CD3 sütunları boş — 4'ten ayrımı bu.
- **7 → Dendritik (36).** HLA sınıf II genleri baskın, FCER1A ve CST3 dolu.
- **8 → Trombosit (13).** PF4, GNG11, PPBP; başka hiçbir sütun konuşmuyor.

Önceki rehberin sorusu burada cevaplanıyor: en küçük iki küme gürültü değil, gerçek ve nadir iki hücre tipi çıktı — 36 hücrelik dendritik grup ve 13 hücrelik trombosit grubu. Bir kümenin gerçekliğini boyu değil, tutarlı işaretçi profili belirler.

## Neler ters gider?

Üç sık hata. Birincisi, ilk-5 tablosunu tek başına kimlik sanmak: tabloda ribozomal ve genel metabolizma genleri de yükselir (küme 1 örneği); kimlik, bilinen işaretçilerle çapraz kontrol ister. İkincisi, dotplot'ta yalnız renge bakmak: az sayıda hücrede çok yüksek ifade ile çok hücrede orta ifade aynı rengi verebilir; boy ve renk birlikte okunur. Üçüncüsü, her kümeye kesin ad dayatmak: ayrışmayan kümeye soru işareti ya da temkinli ad koymak zayıflık değil, raporun güvenilirliğidir.

## Kendin dene

Yazıdaki eşlemeye bakmadan önce kendi tablonuzu kurun: defterin ilk-5 çıktısını ve dotplot'u kullanarak dokuz satırlık küme → hücre tipi eşlemesi yazın, sonra buradakiyle karşılaştırın. Ayrıştıramadığınız küme olduysa hangi ek işaretçi ya da hangi çözünürlük değişikliği ayrımı netleştirirdi, tek cümleyle not edin. Ve en küçük iki kümenin kimliğini önceki rehberdeki tahmininizle karşılaştırın.
