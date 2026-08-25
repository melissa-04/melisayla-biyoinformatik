# Bulk transkriptomik

Dokudan ya da hücre topluluğundan alınan RNA'nın toplam halde dizilendiği analizler. "Ortalama sinyal" verir: tek tek hücreleri değil, karışımın bütününü görürsünüz.

### Diferansiyel gen ekspresyonu (DGE)

**Nedir:** İki veya daha çok koşul arasında hangi genlerin ifadesinin anlamlı değiştiğini bulan temel analiz.
**Hangi soruyu cevaplar:** Knockout, tedavi ya da hastalık, gen ifadesini nasıl değiştirdi?
**Ne zaman kullanılır:** En az üçer biyolojik tekrarlı karşılaştırmalarda. Tekrarsız veriyle güvenilir p-değeri üretilemez.
**Araçlar:** DESeq2, PyDESeq2, edgeR, limma-voom.

### Gen seti zenginleştirme (GSEA / ORA) ve yolak analizi

**Nedir:** Tek tek genler yerine gen kümelerinin (yolaklar, GO terimleri) toplu davranışını test eder.
**Hangi soruyu cevaplar:** Değişen genler rastgele mi, yoksa belirli bir biyolojik sürecin parçası mı?
**Ne zaman kullanılır:** DGE sonrası yorum aşamasında. En sık hata, arka plan (background) gen listesinin yanlış seçilmesidir.
**Araçlar:** gseapy, clusterProfiler, MSigDB gen setleri.

### Alternatif splicing / diferansiyel transkript kullanımı (DTU)

**Nedir:** Genin toplam ifadesi aynı kalsa bile hangi izoformun kullanıldığının değişip değişmediğini test eder.
**Hangi soruyu cevaplar:** Koşul, genin miktarını değil hangi versiyonunun yapıldığını mı değiştiriyor?
**Ne zaman kullanılır:** Splicing faktörleri, sinir dokusu, kanser gibi izoform-yoğun bağlamlarda; düşük derinlikli veride güvenilmez.
**Araçlar:** rMATS, SUPPA2, DEXSeq, DRIMSeq.

### Ko-ekspresyon ağ analizi (WGCNA)

**Nedir:** Birlikte hareket eden genleri modüllere ayırır ve modülleri fenotiple ilişkilendirir.
**Hangi soruyu cevaplar:** Hangi gen grupları birlikte çalışıyor ve hangi grup incelenen özellikle ilişkili?
**Ne zaman kullanılır:** Çok örnekli veri setlerinde (ideali 15–20 ve üzeri); az örnekle ağ gürültüden ibaret kalır.
**Araçlar:** WGCNA (R), PyWGCNA.

### Transkriptom birleştirme ve yeni izoform keşfi

**Nedir:** Okumalardan transkript modellerini kurar; referans destekli (StringTie) ya da referanssız/de novo (Trinity) yapılır.
**Hangi soruyu cevaplar:** Bu organizmada veya dokuda henüz anote edilmemiş transkriptler var mı?
**Ne zaman kullanılır:** Referansı zayıf organizmalarda; iyi anote edilmiş insan/fare çalışmalarında genellikle gerekmez. De novo yol yüksek bellek ister.
**Araçlar:** StringTie, Trinity, gffcompare.

### Uzun okuma izoform analizi (PacBio Iso-Seq, Nanopore cDNA)

**Nedir:** Tek okumada tam transkript yakalayarak izoform yapısını doğrudan gözler; kısa okumanın tahmin ettiğini görür.
**Hangi soruyu cevaplar:** Hangi ekzon kombinasyonları gerçekten aynı molekülde bir arada?
**Ne zaman kullanılır:** İzoform sorusu merkezi olduğunda; kantifikasyon için genellikle kısa okumayla birlikte kullanılır.
**Araçlar:** minimap2, IsoQuant, FLAIR, SQANTI3.

### Nanopore direct RNA ile epitranskriptomik

**Nedir:** RNA'yı cDNA'ya çevirmeden doğrudan okur; m6A ve Ψ gibi modifikasyonları ve poly(A) kuyruk uzunluğunu ham sinyalden çıkarır.
**Hangi soruyu cevaplar:** RNA'nın üzerinde hangi kimyasal işaretler var, nerede ve ne kadar?
**Ne zaman kullanılır:** RNA modifikasyon biyolojisi çalışılıyorsa; hızla gelişen, veri ve donanım yükü ağır bir alan.
**Araçlar:** dorado, m6anet, xPore, nanopolish.

### Küçük RNA / miRNA-seq

**Nedir:** ~22 nükleotidlik düzenleyici RNA'ların kendine özgü kütüphane ve analiz adımlarıyla profillenmesi.
**Hangi soruyu cevaplar:** Hangi miRNA'lar değişiyor ve hangi mRNA'ları hedefliyor olabilirler?
**Ne zaman kullanılır:** Düzenleyici mekanizma ararken; hedef tahminleri her zaman deneysel doğrulama ister.
**Araçlar:** miRDeep2, sRNAbench, miRBase.

### lncRNA ve circRNA analizi

**Nedir:** Protein kodlamayan uzun RNA'ların ve geri-birleşme (back-splicing) ile oluşan halkasal RNA'ların tespiti.
**Hangi soruyu cevaplar:** Kodlamayan transkriptom bu koşulda nasıl davranıyor?
**Ne zaman kullanılır:** Spesifik bir hipotez varsa. circRNA tespiti kütüphane tipine bağlıdır (rRNA-azaltılmış kütüphane gerekir; poly-A seçimli kütüphanede görünmezler).
**Araçlar:** FEELnc, CPC2, CIRCexplorer2, CIRI2.

### RNA editing (A-to-I)

**Nedir:** ADAR enzimlerinin RNA üzerinde yaptığı ve dizide A→G olarak görünen değişikliklerin tespiti.
**Hangi soruyu cevaplar:** Genomda olmayan ama RNA'da beliren değişiklikler nerede?
**Ne zaman kullanılır:** Sinir bilimi, immünoloji ve kanser bağlamlarında. En büyük tuzak SNP'lerle karışmasıdır; bilinen varyantlara karşı sıkı filtre şarttır.
**Araçlar:** REDItools2, JACUSA2.

### Allel-spesifik ekspresyon (ASE)

**Nedir:** Anneden ve babadan gelen kopyaların eşit ifade edilip edilmediğini heterozigot bölgeler üzerinden ölçer.
**Hangi soruyu cevaplar:** Cis-düzenleyici varyantlar ya da imprinting kopyalardan birini mi kısıyor?
**Ne zaman kullanılır:** Aynı bireyden hem genotip hem RNA verisi varsa; genotip olmadan yapılamaz.
**Araçlar:** GATK ASEReadCounter, phASER.

### Füzyon gen tespiti

**Nedir:** İki genin birleşmesiyle oluşan kimerik transkriptlerin (örn. BCR-ABL1) RNA'dan yakalanması.
**Hangi soruyu cevaplar:** Bu tümörde bilinen ya da yeni bir füzyon sürücü var mı?
**Ne zaman kullanılır:** Kanser transkriptomiğinde standart adım; normal dokudaki bulguların önemli kısmı artefakttır ve filtre ister. Hizalama aşaması yüksek bellek gerektirir.
**Araçlar:** STAR-Fusion, Arriba.

### Ribozom profillemesi (Ribo-seq)

**Nedir:** Ribozomun koruduğu RNA parçalarını dizileyerek çeviriyi (translasyonu) ölçer; mRNA bolluğu ile protein sentezi arasındaki farkı görünür kılar.
**Hangi soruyu cevaplar:** Hangi mRNA'lar gerçekten çevriliyor; hangi uORF'lar aktif?
**Ne zaman kullanılır:** Translasyonel kontrol hipotezi varsa; deneysel tarafı tekniktir ve titizlik ister.
**Araçlar:** riboWaltz, Plastid, RiboCode.

### Nascent RNA analizi (PRO-seq, TT-seq, SLAM-seq)

**Nedir:** Yeni sentezlenen RNA'yı işaretleyip ölçerek kararlı durum yerine anlık transkripsiyon aktivitesini gözler.
**Hangi soruyu cevaplar:** Gördüğüm değişim sentez hızından mı, yıkım hızından mı geliyor?
**Ne zaman kullanılır:** Hızlı ve dinamik yanıtlar çalışılırken; standart RNA-seq'in ayıramadığı sentez/yıkım sorusunda.
**Araçlar:** PRO-seq ve TT-seq boru hatları, GRAND-SLAM.

### Bulk dekonvolüsyon (hücre tipi oranı tahmini)

**Nedir:** Bulk karışımından hücre tipi oranlarını, tek hücre referansı ya da imza matrisleriyle tahmin eder.
**Hangi soruyu cevaplar:** Gördüğüm ifade değişimi gerçek mi, yoksa dokunun hücre bileşimi mi değişmiş?
**Ne zaman kullanılır:** Tümör ve kan gibi heterojen dokularda neredeyse her zaman; referans dokuyla uyumsuzsa sonuçlar yanıltır.
**Araçlar:** CIBERSORTx, quanTIseq, BayesPrism, MuSiC.

### Metatranskriptomik

**Nedir:** Bir topluluktaki (bağırsak, toprak, su) tüm mikroorganizmaların RNA'sını birlikte dizileyip aktif işlevleri okur.
**Hangi soruyu cevaplar:** Mikrobiyom kimlerden oluşuyor sorusunun ötesinde, hangi genleri çalıştırıyor?
**Ne zaman kullanılır:** İşlev sorusu varsa; yalnızca "kim var" sorusu için 16S daha ekonomiktir. Veritabanları büyüktür, yüksek bellek ister.
**Araçlar:** Kraken2/Bracken, HUMAnN.

### Dual RNA-seq (konak–patojen)

**Nedir:** Enfeksiyon sırasında konağın ve patojenin transkriptomlarını aynı örnekten eş zamanlı okur.
**Hangi soruyu cevaplar:** Enfeksiyon diyaloğunda iki taraf birbirinin ifadesini nasıl değiştiriyor?
**Ne zaman kullanılır:** Enfeksiyon modellerinde; patojen RNA'sı azınlıkta olduğundan derin dizileme gerektirir.
**Araçlar:** İki referansa birden hizalama (STAR, bowtie2) + standart DGE araçları.

### Transkriptomik yaşlanma saati

**Nedir:** İfade profilinden biyolojik yaş tahmin eden makine öğrenmesi modelleri; metilasyon saatlerinin transkriptom uyarlaması.
**Hangi soruyu cevaplar:** Bu doku ya da birey kronolojik yaşından "yaşlı" mı görünüyor; bir müdahale saati geri alıyor mu?
**Ne zaman kullanılır:** Yaşlanma araştırmalarında sonuç ölçütü olarak; saatler eğitildikleri doku ve popülasyonun dışında güvenilirliğini yitirir.
**Araçlar:** Elastic net tabanlı yaş tahmin modelleri.
