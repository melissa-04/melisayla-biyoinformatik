# Fonksiyonel genomik ve perturbasyon

Genomu gözlemekle kalmayıp dürten analizler: bir geni, bir düzenleyici bölgeyi ya da bir varyantı değiştirdiğimizde ne olduğunu ölçerler.

### Genom ölçekli CRISPR KO / CRISPRi / CRISPRa taramaları

**Nedir:** Binlerce genin tek tek susturulduğu (ya da bastırıldığı/etkinleştirildiği) havuzlu deneylerde guide RNA sayımlarından zenginleşme ve tükenmeyi test eder.
**Hangi soruyu cevaplar:** Bu fenotip (sağkalım, ilaç direnci, marker ifadesi) için hangi genler gerekli ya da engelleyici?
**Ne zaman kullanılır:** Yansız gen keşfinde; sonuç, guide etkinliğine ve tarama derinliğine duyarlıdır.
**Araçlar:** MAGeCK, BAGEL2.

### Tek hücre CRISPR taramaları (Perturb-seq ailesi)

**Nedir:** Her hücrede hangi guide'ın bulunduğunu transkriptomla birlikte okur; perturbasyon başına tam ifade fenotipi verir.
**Hangi soruyu cevaplar:** Bu geni susturmak hücrenin bütün programını nasıl değiştiriyor?
**Ne zaman kullanılır:** Tek fenotip yerine zengin okuma gerektiğinde. Kromatin okumalı (Perturb-ATAC) ve görüntüleme tabanlı türevleri de vardır.
**Araçlar:** Perturb-seq/CROP-seq protokolleri; analizde Scanpy + karışım modelleri.

### Multiome Perturb-seq

**Nedir:** Perturbasyonun etkisini aynı hücrede hem transkriptom hem kromatin düzeyinde eş zamanlı ölçer.
**Hangi soruyu cevaplar:** Müdahale önce düzenleyici katmanı mı, ifadeyi mi değiştiriyor?
**Ne zaman kullanılır:** Mekanizmanın katman sırası sorulduğunda; çok yeni ve maliyetli bir yaklaşımdır.
**Araçlar:** Multiome protokolleri + Perturb-seq analiz çerçeveleri.

### Spatial Perturb-seq

**Nedir:** CRISPR taramasını doku bütünlüğü korunarak, hücrelerin konum bilgisiyle birlikte okuyan sınır yaklaşım.
**Hangi soruyu cevaplar:** Bir perturbasyonun etkisi hücrenin kendisiyle mi sınırlı, komşularına da yayılıyor mu?
**Ne zaman kullanılır:** Doku bağlamına bağlı gen işlevi sorularında; henüz az sayıda laboratuvarın uyguladığı yeni bir yöntemdir.
**Araçlar:** Yönteme özgü, yeni yayınlanan boru hatları.

### Compressed Perturb-seq

**Nedir:** Her hücreye birden çok rastgele perturbasyon verip etkileri hesaplamayla ayrıştırarak tarama maliyetini düşürür.
**Hangi soruyu cevaplar:** Aynı bütçeyle kaç kat fazla perturbasyon taranabilir?
**Ne zaman kullanılır:** Büyük kütüphaneli taramalarda ölçek gerektiğinde; ayrıştırma, etkilerin çoğunlukla seyrek ve toplanabilir olduğu varsayımına dayanır.
**Araçlar:** Yönteme eşlik eden istatistiksel ayrıştırma çerçeveleri.

### Hedefli Perturb-seq (TAP-seq)

**Nedir:** Transkriptomun tamamı yerine seçilmiş bir gen panelini okuyarak hücre başına maliyeti ciddi biçimde düşürür.
**Hangi soruyu cevaplar:** İlgilendiğim yanıt genleri üzerinde binlerce perturbasyonun etkisi ne?
**Ne zaman kullanılır:** Enhancer–gen taramaları gibi hedefi belli, ölçeği büyük deneylerde.
**Araçlar:** TAP-seq protokolü ve analiz betikleri.

### Perturbasyon etkisi tahmin modelleri (GEARS ve benzerleri)

**Nedir:** Görülmüş perturbasyonlardan öğrenip görülmemiş tekli ve kombinasyon müdahalelerin etkisini tahmin eden modeller.
**Hangi soruyu cevaplar:** Hangi kombinasyonu denemek en bilgilendirici olur?
**Ne zaman kullanılır:** Deney önceliklendirmede; tahminler hipotezdir, kıyaslamalarda basit modeller de güçlü çıkabilmektedir.
**Araçlar:** GEARS ve türevi modeller.

### Base-editor / saturation mutagenesis taramaları

**Nedir:** Bir genin ya da bölgenin olası tüm varyantlarını üretip işlev etkilerini toplu ölçer.
**Hangi soruyu cevaplar:** Bu varyant zararlı mı — tahminle değil, ölçümle?
**Ne zaman kullanılır:** Klinik varyant yorumuna işlevsel kanıt üretmek için; okuma fenotipinin klinik fenotipi temsil ettiğinden emin olunmalıdır.
**Araçlar:** Tarama tasarım araçları + MAGeCK tarzı sayım analizi.

### MPRA / STARR-seq (enhancer aktivitesi)

**Nedir:** Binlerce DNA dizisinin düzenleyici aktivitesini raportör sistemlerde paralel ölçer.
**Hangi soruyu cevaplar:** Bu dizi gerçekten enhancer mı; varyant aktiviteyi değiştiriyor mu?
**Ne zaman kullanılır:** Kodlamayan bölge ve varyant işlevi sorularında; plazmit bağlamı, doğal kromatin bağlamından farklı olabilir.
**Araçlar:** MPRAflow ve platforma özgü analiz boru hatları.

### ChIP-seq, CUT&RUN, CUT&Tag

**Nedir:** Bir proteinin (TF, histon işareti) genomda nereye bağlandığını haritalar; CUT&RUN ve CUT&Tag bunu çok daha az hücreyle yapar.
**Hangi soruyu cevaplar:** Bu düzenleyici protein hangi bölgeleri hedefliyor?
**Ne zaman kullanılır:** Doğrudan bağlanma kanıtı gerektiğinde; antikor kalitesi sonucun tavanıdır.
**Araçlar:** MACS2/3, SEACR, deeptools.

### ATAC-seq / DNase-seq

**Nedir:** Açık kromatin bölgelerini genom çapında haritalar; aktif düzenleyici bölgelerin genel envanterini verir.
**Hangi soruyu cevaplar:** Bu hücre durumunda hangi düzenleyici bölgeler kullanıma açık?
**Ne zaman kullanılır:** Düzenleyici manzaranın ilk haritası olarak; motif ve ayak izi analiziyle TF aktivitesine bağlanır.
**Araçlar:** MACS2/3, TOBIAS, deeptools.

### Hi-C / Micro-C / HiChIP (3D genom)

**Nedir:** Genomun üç boyutlu temas haritasını çıkarır: TAD'lar, döngüler, bölmeler.
**Hangi soruyu cevaplar:** Hangi enhancer hangi promotörle fiziksel temas halinde?
**Ne zaman kullanılır:** Uzak düzenleyici bağlantı sorularında; yüksek çözünürlük çok derin dizileme ve ciddi hesaplama ister.
**Araçlar:** cooler, Juicer, HiCExplorer.

### DNA metilasyon analizi (WGBS, EM-seq, Nanopore)

**Nedir:** Sitozin metilasyonunu baz çözünürlüğünde haritalar; Nanopore bunu dönüşümsüz, doğrudan okur.
**Hangi soruyu cevaplar:** Hangi bölgeler metillenmiş ve koşullar arasında nerede değişiyor?
**Ne zaman kullanılır:** Epigenetik programlama, gelişim ve kanser sorularında; tüm-genom yaklaşımlar veri olarak ağırdır.
**Araçlar:** Bismark, methylKit, modkit.

### eQTL analizi

**Nedir:** Genetik varyantların gen ifadesi üzerindeki etkisini popülasyon verisinde haritalar.
**Hangi soruyu cevaplar:** Bu varyant hangi genin ifadesini, hangi dokuda değiştiriyor?
**Ne zaman kullanılır:** GWAS sinyallerini mekanizmaya bağlarken; etkiler doku-özgüdür, GTEx gibi kaynaklar bu yüzden dokuya göre sorgulanır.
**Araçlar:** tensorQTL, MatrixEQTL; kaynak olarak GTEx.

### sQTL, caQTL, pQTL haritalama

**Nedir:** eQTL fikrinin diğer katmanlara uzantısı: splicing, kromatin erişilebilirliği ve protein düzeyi üzerindeki varyant etkileri.
**Hangi soruyu cevaplar:** Varyant etkisini hangi moleküler katmanda gösteriyor?
**Ne zaman kullanılır:** İfade QTL'inin açıklayamadığı sinyallerde; splicing için LeafCutter tarzı intron kullanımı yaklaşımları standarttır.
**Araçlar:** LeafCutter + QTL araçları; katmana özgü boru hatları.

### TWAS, colocalization ve Mendelian randomization

**Nedir:** GWAS sinyalini gen ifadesiyle (TWAS), ortak nedensel varyant testiyle (colocalization) ya da nedensellik çıkarımıyla (MR) ilişkilendiren istatistiksel çerçeveler.
**Hangi soruyu cevaplar:** Bu lokusun ardındaki gen hangisi; ilişki nedensel olabilir mi?
**Ne zaman kullanılır:** GWAS sonrası yorum aşamasında; her üçü de güçlü varsayımlara dayanır ve varsayımlar ihlal edildiğinde güvenle yanıltır.
**Araçlar:** FUSION, coloc, TwoSampleMR.

### Enhancer–gen bağlantılama (ABC) ve variant-to-function

**Nedir:** Aktivite ve temas verilerini birleştirerek her enhancer'ı hedef genine bağlayan modeller ve varyanttan işleve giden çerçeveler.
**Hangi soruyu cevaplar:** Kodlamayan varyant hangi geni, hangi düzenleyici üzerinden etkiliyor?
**Ne zaman kullanılır:** GWAS ve nadir varyant yorumunda kodlamayan bölgeye girildiğinde.
**Araçlar:** ABC modeli uygulamaları, V2F iş akışları.

### Dizi tabanlı derin öğrenme modelleri (Enformer, Borzoi)

**Nedir:** Ham DNA dizisinden ifade ve epigenomik profilleri tahmin eden, uzun bağlamlı derin öğrenme modelleri.
**Hangi soruyu cevaplar:** Bu dizi değişikliği, deney yapmadan önce, hangi düzenleyici etkiyi öngörüyor?
**Ne zaman kullanılır:** In silico mutagenez ve önceliklendirmede; tahminler eğitim verisinin kapsamıyla sınırlıdır ve hücre tipi genellemesi zayıf olabilir.
**Araçlar:** Enformer, Borzoi.

### Çok katmanlı multi-omics entegrasyonu (MOFA, Seurat WNN)

**Nedir:** Transkriptom, kromatin, protein gibi katmanları ortak faktörlerde ya da ağırlıklı komşuluklarda birleştiren yöntemler.
**Hangi soruyu cevaplar:** Katmanların ortak anlattığı ana varyasyon eksenleri ne?
**Ne zaman kullanılır:** Aynı örneklerden birden çok katman varsa; entegrasyon, tek başına hiçbir katmanın gösteremediği yapıyı ortaya çıkarabilir.
**Araçlar:** MOFA+, Seurat WNN.
