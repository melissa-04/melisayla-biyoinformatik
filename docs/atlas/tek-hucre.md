# Tek hücre transkriptomiği

Her hücrenin ayrı ayrı okunduğu analizler. Bulk'ın ortalamasında kaybolan hücre tiplerini, durumlarını ve azınlık popülasyonlarını görünür kılar.

### scRNA-seq temel iş akışı

**Nedir:** Kalite kontrol, normalizasyon, boyut indirgeme (PCA/UMAP) ve kümelemeden oluşan standart analiz zinciri.
**Hangi soruyu cevaplar:** Bu örnekte hangi hücre popülasyonları var ve hangi genlerle tanımlanıyorlar?
**Ne zaman kullanılır:** Her tek hücre projesinin başlangıcı; sonraki bütün analizlerin kalitesi bu zincirin doğruluğuna bağlıdır.
**Araçlar:** Scanpy, Seurat, AnnData.

### Doublet tespiti ve ambient RNA düzeltmesi

**Nedir:** Aynı damlacığa iki hücre düşmesiyle oluşan yapay profillerin ve çözeltide yüzen serbest RNA kirliliğinin ayıklanması.
**Hangi soruyu cevaplar:** Gördüğüm "ara" popülasyon gerçek mi, teknik artefakt mı?
**Ne zaman kullanılır:** Her damlacık tabanlı deneyde; atlanırsa sahte hücre tipleri ve yanıltıcı marker'lar üretir.
**Araçlar:** scrublet, scDblFinder; ambient için SoupX, CellBender.

### Hücre tipi anotasyonu

**Nedir:** Kümelerin bilinen marker genlerle elle ya da referans veriyle otomatik olarak isimlendirilmesi.
**Hangi soruyu cevaplar:** Bu kümeler biyolojik olarak hangi hücre tiplerine karşılık geliyor?
**Ne zaman kullanılır:** Kümelemeden hemen sonra. Otomatik araçlar hız kazandırır ama marker kontrolü olmadan tek başına güvenilmez.
**Araçlar:** CellTypist, SingleR, Azimuth; elle anotasyon için literatür marker listeleri.

### Batch düzeltme ve veri entegrasyonu

**Nedir:** Farklı deney, gün ya da platformdan gelen verilerin teknik farklarını gidererek ortak bir uzayda birleştirme.
**Hangi soruyu cevaplar:** Örnekler arasındaki ayrışma biyoloji mi, teknik batch etkisi mi?
**Ne zaman kullanılır:** Çok örnekli her çalışmada. Tuzağı aşırı düzeltmedir: gerçek biyolojik farkın da silinmesi.
**Araçlar:** Harmony, scVI, scANVI, Scanorama.

### Referans atlasa haritalama

**Nedir:** Yeni bir veri setinin, önceden oluşturulmuş büyük bir hücre atlasının üzerine yansıtılması.
**Hangi soruyu cevaplar:** Benim hücrelerim atlastaki hangi tiplere denk düşüyor; beklenmeyen bir popülasyon var mı?
**Ne zaman kullanılır:** İyi bir referans atlas mevcutsa; sıfırdan kümeleme yerine hızlı ve standart anotasyon sağlar.
**Araçlar:** scArches, Azimuth, Symphony.

### Trajectory / pseudotime analizi

**Nedir:** Hücreleri bir gelişim ya da farklılaşma sürecinin "sanal zaman" ekseni üzerine dizer.
**Hangi soruyu cevaplar:** Hücreler hangi ara durumlardan geçerek hangi kaderlere ilerliyor?
**Ne zaman kullanılır:** Örneklemde sürecin bütün ara aşamaları temsil ediliyorsa; kesikli, tamamlanmış süreçlerde yanıltıcıdır.
**Araçlar:** Monocle 3, Slingshot, PAGA.

### RNA velocity

**Nedir:** Olgun (spliced) ve olgunlaşmamış (unspliced) transkript oranlarından her hücrenin ifade değişim yönünü tahmin eder.
**Hangi soruyu cevaplar:** Bu hücre şu an hangi duruma doğru gidiyor?
**Ne zaman kullanılır:** Dinamik süreçlerde pseudotime'a yön eklemek için. Model varsayımları güçlüdür; okların yorumunda ihtiyatlı olun.
**Araçlar:** scVelo, velocyto.

### Regülatör velocity

**Nedir:** Velocity tahminine düzenleyici bilgiyi ekleyen yeni nesil modeller. MultiVelo, eşleşmiş RNA+ATAC (multiome) verisinden kromatin dinamiğini modele katar; scKINETICS ise hücre hızlarını, epigenetik ve ko-ekspresyon önselleriyle desteklenen bir gen düzenleyici ağla birlikte öğrenir.
**Hangi soruyu cevaplar:** İfade değişiminin yönünü hangi düzenleyiciler sürüyor?
**Ne zaman kullanılır:** Yönün ardındaki mekanizma sorulduğunda. MultiVelo multiome verisi gerektirir; scKINETICS yalnızca transkriptomla çalışır. Genç ve gelişmekte olan bir literatürdür.
**Araçlar:** MultiVelo, scKINETICS.

### Hücre–hücre iletişimi (ligand–reseptör)

**Nedir:** Bir hücre tipindeki ligand ifadesi ile diğerindeki reseptör ifadesini eşleştirerek olası sinyalleşmeyi çıkarır.
**Hangi soruyu cevaplar:** Dokuda kim kime hangi sinyali gönderiyor olabilir?
**Ne zaman kullanılır:** Mikroçevre ve etkileşim sorularında. Çıktı hipotezdir, kanıt değil; ifade birlikte-varlığı temas anlamına gelmez.
**Araçlar:** CellPhoneDB, CellChat, LIANA.

### Gen regülatör ağ (GRN) çıkarımı

**Nedir:** Transkripsiyon faktörleri ile hedef genleri arasında düzenleyici modüller (regulonlar) çıkarır.
**Hangi soruyu cevaplar:** Bu hücre durumunu hangi transkripsiyon faktörleri yönetiyor?
**Ne zaman kullanılır:** Mekanizma ve sürücü TF arayışında; motif bilgisiyle desteklenen çıkarımlar daha güvenilirdir.
**Araçlar:** SCENIC/pySCENIC, CellOracle; derin öğrenme tabanlı yeni yöntemler gelişmektedir.

### Diferansiyel abundans analizi

**Nedir:** Koşullar arasında hücre bolluğu değişimini, kümelere bağlı kalmadan komşuluk düzeyinde test eder.
**Hangi soruyu cevaplar:** Hastalıkta hangi hücre durumları çoğalıyor ya da azalıyor?
**Ne zaman kullanılır:** Değişim küme sınırlarına oturmuyorsa; klasik oran karşılaştırmasından daha duyarlıdır.
**Araçlar:** Milo (miloR/milopy).

### Pseudobulk DGE

**Nedir:** Her örnekteki hücreleri toplayıp örnek düzeyinde sayım oluşturarak klasik DGE araçlarıyla test etme.
**Hangi soruyu cevaplar:** Koşullar arasındaki ifade farkı, örnek sayısı dikkate alındığında da anlamlı mı?
**Ne zaman kullanılır:** Koşul karşılaştırmalarında standart yol; hücreleri bağımsız sayan testlerin ürettiği yalancı-tekrar şişmesini önler.
**Araçlar:** decoupler/Scanpy toplama + PyDESeq2, edgeR.

### scATAC-seq

**Nedir:** Tek hücre düzeyinde açık kromatin bölgelerini okur; ifadenin bir adım öncesindeki düzenleyici durumu gösterir.
**Hangi soruyu cevaplar:** Bu hücre tipinde hangi düzenleyici bölgeler erişilebilir, hangi motifler aktif?
**Ne zaman kullanılır:** Düzenleyici kimlik ve TF aktivitesi sorularında; veri seyrektir, analiz RNA'dan daha tekniktir.
**Araçlar:** ArchR, Signac, snapATAC2, chromVAR.

### Multiome (RNA + ATAC) analizi

**Nedir:** Aynı hücreden hem transkriptom hem kromatin erişilebilirliği okunur; iki katman doğrudan eşleştirilir.
**Hangi soruyu cevaplar:** Hangi açık bölge hangi genin ifadesiyle bağlantılı?
**Ne zaman kullanılır:** Peak–gen bağlantılama ve düzenleyici mekanizma sorularında; iki katmanı ayrı deneylerle eşlemekten çok daha güçlüdür.
**Araçlar:** Signac, ArchR, Seurat WNN, MultiVelo.

### CITE-seq (RNA + yüzey proteini)

**Nedir:** Oligo-etiketli antikorlarla yüzey proteinlerini RNA ile aynı hücrede sayar.
**Hangi soruyu cevaplar:** RNA'nın belirsiz bıraktığı popülasyonları protein belirteçleri ayırıyor mu?
**Ne zaman kullanılır:** Özellikle immünolojide; protein katmanı anotasyonu netleştirir. Antikor arka planı ayrı normalizasyon ister.
**Araçlar:** totalVI, dsb, Seurat.

### Uzun okuma tek hücre ve spatial RNA-seq

**Nedir:** Tek hücre ya da spatial kütüphanelerin uzun okuma platformlarında dizilenmesiyle izoform düzeyinde çözünürlük.
**Hangi soruyu cevaplar:** Hücre tipleri yalnızca gen düzeyinde değil, izoform kullanımında da farklılaşıyor mu?
**Ne zaman kullanılır:** İzoform sorusu hücre tipine bağlıysa; barkod ve UMI atama hataları ana teknik zorluktur.
**Araçlar:** FLAMES, epi2me wf-single-cell, IsoQuant.

### TCR/BCR repertuar (scVDJ) analizi

**Nedir:** T ve B hücrelerinin antijen reseptör dizilerini tek hücre düzeyinde okuyup klonotipleri çıkarır.
**Hangi soruyu cevaplar:** Bağışıklık yanıtında hangi klonlar genişlemiş ve hangi fenotiple eşleşiyor?
**Ne zaman kullanılır:** İmmünoloji, aşı ve tümör immünolojisi çalışmalarında; transkriptomla birleştiğinde en güçlüdür.
**Araçlar:** Cell Ranger VDJ, scirpy, Immcantation.

### Klonal takip / lineage tracing

**Nedir:** Doğal (mitokondriyal DNA varyantları) ya da sentetik barkodlarla hücrelerin soy ağacını izler.
**Hangi soruyu cevaplar:** Bu hücreler hangi ortak atadan geliyor; klonlar kaderlerine nasıl dağılıyor?
**Ne zaman kullanılır:** Gelişim, rejenerasyon ve kanser evrimi sorularında; barkod yöntemleri deney tasarımına en baştan girmelidir.
**Araçlar:** mgatk (mtDNA), barkod tabanlı izleme boru hatları.

### Sabit transkriptomlarda klonal genotipleme

**Nedir:** Somatik mutasyon bilgisini ifade profiliyle aynı hücrede birleştirerek klonal heterojeniteyi çözen yaklaşımlar.
**Hangi soruyu cevaplar:** Mutant ve yabani tip hücreler aynı dokuda farklı programlar mı çalıştırıyor?
**Ne zaman kullanılır:** Kanser ve klonal hematopoez çalışmalarında; hedefe yönelik genotipleme yöntemleri (GoT ve türevleri) bu alanı hızla ölçeklendirmektedir.
**Araçlar:** GoT tarzı hedefli protokoller ve eşlik eden analiz araçları.

### Tek hücre foundation modelleri

**Nedir:** Milyonlarca hücreyle ön-eğitilmiş, anotasyon ve gösterim (embedding) gibi görevlere aktarılabilen büyük modeller.
**Hangi soruyu cevaplar:** Elimdeki veriyi etiketlemeden, öğrenilmiş genel temsille ne kadar ileri gidebilirim?
**Ne zaman kullanılır:** Hızlı ön-anotasyon ve keşifte. Bağımsız kıyaslamalarda sonuçlar görev bazında değişkendir; klasik yöntemlerle karşılaştırmadan güvenmeyin.
**Araçlar:** scGPT, Geneformer.

### Perturbasyon yanıtı tahmini

**Nedir:** Görülmemiş bir genetik müdahalenin ifade etkisini, görülmüş perturbasyonlardan öğrenerek tahmin eden modeller.
**Hangi soruyu cevaplar:** Denemediğim knockout ya da kombinasyon ne yapardı?
**Ne zaman kullanılır:** Deney önceliklendirmede. Alan gençtir; basit taban çizgileri bazı kıyaslamalarda karmaşık modellerle yarışır, tahminler deney yerine geçmez.
**Araçlar:** GEARS ve benzeri modeller.

### LLM / ajan tabanlı tek hücre analizi

**Nedir:** Büyük dil modellerinin analiz adımlarını planladığı, kod ürettiği ya da sonuçları yorumladığı yeni yaklaşımlar.
**Hangi soruyu cevaplar:** Analiz sürecinin ne kadarı güvenilir biçimde otomatikleştirilebilir?
**Ne zaman kullanılır:** Keşif ve hızlandırma amaçlı; her çıktı, klasik yöntemlerle doğrulanmadan sonuç sayılmamalıdır.
**Araçlar:** Hızla değişen deneysel araç ekosistemi.
