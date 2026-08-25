# Spatial transkriptomik

İfadeyi dokudaki konumuyla birlikte okuyan analizler. "Hangi hücreler var" sorusuna "neredeler ve kimlerle komşular" boyutunu ekler.

### Dizileme tabanlı platformlar (Visium, Visium HD, Stereo-seq, Slide-seq)

**Nedir:** Doku kesitini konum barkodlu yüzeylere alıp transkriptom genişliğinde dizileyen yaklaşımlar.
**Hangi soruyu cevaplar:** Dokunun hangi bölgesinde hangi ifade programı çalışıyor?
**Ne zaman kullanılır:** Panelle sınırlanmamış keşif için. Klasik Visium'da bir spot birden çok hücre içerir; hücre düzeyi yorum dekonvolüsyon ister.
**Araçlar:** Space Ranger, Scanpy/Squidpy.

### Görüntüleme tabanlı platformlar ve hücre segmentasyonu

**Nedir:** Seçili yüzlerce-binlerce geni moleküler görüntülemeyle tek hücre ve altı çözünürlükte sayan yaklaşımlar.
**Hangi soruyu cevaplar:** Tek tek hücreler dokuda tam olarak nerede ve ne ifade ediyor?
**Ne zaman kullanılır:** Hedef gen paneli yeterliyse; sonuç kalitesini büyük ölçüde hücre segmentasyonunun doğruluğu belirler.
**Araçlar:** Xenium, MERFISH, CosMx platform yazılımları; segmentasyon için Cellpose, Baysor.

### Spot dekonvolüsyonu

**Nedir:** Çok hücreli spotların içeriğini, tek hücre referansı kullanarak hücre tipi oranlarına ayrıştırır.
**Hangi soruyu cevaplar:** Bu spotun sinyali hangi hücre tiplerinin karışımından geliyor?
**Ne zaman kullanılır:** Visium tarzı verilerde neredeyse her zaman; referansın dokuyla uyumu sonucun tavanını belirler.
**Araçlar:** cell2location, RCTD.

### Spatial domain tespiti

**Nedir:** İfade ve konumu birlikte kullanarak dokuyu tutarlı bölgelere (katmanlar, tümör bölmeleri) ayırır.
**Hangi soruyu cevaplar:** Dokunun doğal anatomik/işlevsel bölgeleri neresi?
**Ne zaman kullanılır:** Bölge temelli her karşılaştırmanın ilk adımı olarak; kontrastif öğrenme tabanlı yeni yöntemler bu alanda hızla gelişiyor.
**Araçlar:** BayesSpace, SpaGCN; yeni nesil örnek: DisConST.

### Çok ölçekli ve frekans-alanı spatial modelleme

**Nedir:** Doku desenlerini farklı uzamsal ölçeklerde ya da frekans bileşenlerinde ayrıştıran yeni modelleme yaklaşımları.
**Hangi soruyu cevaplar:** Gördüğüm desen ince mikro-yapı mı, geniş bölgesel gradyan mı?
**Ne zaman kullanılır:** Tek ölçekli kümelemenin karıştırdığı iç içe desenlerde; genç bir literatürdür, yöntemler henüz standartlaşmamıştır.
**Araçlar:** MNiST, SpatioFreq gibi yeni yayınlanan yöntemler.

### Uzamsal değişken gen (SVG) analizi

**Nedir:** İfadesi dokudaki konuma bağlı olarak sistematik değişen genleri bulur.
**Hangi soruyu cevaplar:** Hangi genler mekânsal desen gösteriyor; gradyanlar ve odaklar nerede?
**Ne zaman kullanılır:** Kümeden bağımsız keşif için; bulunan desenlerin hücre bileşiminden mi kaynaklandığını ayrıca sorgulayın.
**Araçlar:** SpatialDE, SPARK, Squidpy (Moran's I).

### Spatial niche / mikroçevre analizi

**Nedir:** Her hücrenin komşuluk kompozisyonunu çıkarıp tekrarlayan mikroçevre tiplerini tanımlar.
**Hangi soruyu cevaplar:** Hangi hücre tipleri sistematik olarak yan yana yaşıyor; hastalıkta bu komşuluklar nasıl bozuluyor?
**Ne zaman kullanılır:** Tümör mikroçevresi, bağışıklık organizasyonu ve doku mimarisi sorularında.
**Araçlar:** Squidpy komşuluk analizleri, niche kümeleme yaklaşımları.

### Spatial hücre–hücre iletişimi

**Nedir:** Ligand–reseptör çıkarımına fiziksel mesafe kısıtı ekler; yalnızca gerçekten komşu olabilen çiftleri değerlendirir.
**Hangi soruyu cevaplar:** Sinyalleşme dokunun neresinde, hangi komşuluklar arasında gerçekleşiyor?
**Ne zaman kullanılır:** scRNA iletişim hipotezlerini doku bağlamında sınamak için; ifade tabanlı çıkarım sınırlamaları burada da geçerlidir.
**Araçlar:** COMMOT, stLearn, Squidpy.

### Spatial multi-omics

**Nedir:** Aynı kesitte transkriptomu proteinle (spatial CITE-seq) ya da kromatin erişilebilirliğiyle (MISAR-seq) birlikte okuyan yöntemler.
**Hangi soruyu cevaplar:** Konum, ifade ve ek moleküler katman birlikte nasıl örtüşüyor?
**Ne zaman kullanılır:** Tek katmanın yetmediği mekanizma sorularında; protokoller yenidir ve veri entegrasyonu ek uzmanlık ister.
**Araçlar:** Platforma özgü boru hatları + çok katmanlı entegrasyon araçları.

### Spatial epigenomik

**Nedir:** Açık kromatin (spatial ATAC) ya da histon işaretlerini (spatial CUT&Tag) doku koordinatlarıyla birlikte haritalar.
**Hangi soruyu cevaplar:** Düzenleyici programlar dokunun hangi bölgelerinde kurulmuş?
**Ne zaman kullanılır:** Bölgesel düzenleyici kimlik sorularında; alan erken aşamadadır, veri seyrek ve analiz tekniktir.
**Araçlar:** Yönteme özgü boru hatları; kavramlar scATAC araçlarıyla ortaktır.

### scRNA-seq ↔ spatial entegrasyonu

**Nedir:** Tek hücre verisindeki zengin profilleri doku koordinatlarına haritalar ya da spatial veriye hücre düzeyi bilgi taşır.
**Hangi soruyu cevaplar:** scRNA'da tanımladığım hücre durumları dokuda nerede oturuyor?
**Ne zaman kullanılır:** İki veri türü aynı dokudan mevcutsa; en yaygın kullanım, panel dışı genlerin konumsal tahmini ve tip yerleştirmedir.
**Araçlar:** Tangram, CellTrek.

### 3D doku rekonstrüksiyonu

**Nedir:** Ardışık kesitlerin hizalanıp üst üste bindirilmesiyle üç boyutlu moleküler doku modeli kurma.
**Hangi soruyu cevaplar:** Desenler kesit düzleminin ötesinde, hacimde nasıl örgütleniyor?
**Ne zaman kullanılır:** Organ ölçekli mimari sorularında; kesit hizalama hataları ana teknik zorluktur.
**Araçlar:** PASTE, STalign.
