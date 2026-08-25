# Tamamlayıcı analizler

Yukarıdaki her şeyin altında yatan ortak zemin: hizalama, varyantlar, anotasyon ve işlev çıkarımı.

### Referans genom ve pangenom tabanlı hizalama

**Nedir:** Okumaları tek bir doğrusal referansa ya da popülasyon çeşitliliğini içeren pangenom grafına yerleştirme.
**Hangi soruyu cevaplar:** Bu okuma genomun neresinden geliyor?
**Ne zaman kullanılır:** Neredeyse her analizin ilk adımı; pangenom yaklaşımı, referansta olmayan çeşitliliğin yarattığı yanlılığı azaltır.
**Araçlar:** BWA-MEM2, minimap2; pangenom için vg, minigraph.

### Varyant çağırma ve yapısal varyant analizi

**Nedir:** Küçük varyantların (SNV, indel) ve büyük yapısal değişikliklerin (delesyon, duplikasyon, translokasyon) tespiti.
**Hangi soruyu cevaplar:** Bu genom referanstan nerede ve nasıl ayrılıyor?
**Ne zaman kullanılır:** Genetik tanı ve popülasyon çalışmalarının temeli; yapısal varyantlar uzun okumayla çok daha güvenilir yakalanır.
**Araçlar:** GATK, DeepVariant; SV için Manta, Sniffles2.

### Genom anotasyonu

**Nedir:** Çıplak dizinin üzerine gen modellerinin, ekzon sınırlarının ve işlevsel öğelerin yerleştirilmesi.
**Hangi soruyu cevaplar:** Bu genomda genler nerede ve yapıları ne?
**Ne zaman kullanılır:** Yeni dizilenen genomlarda zorunlu adım; kanıt olarak RNA-seq ve protein homolojisi kullanılır, kalite BUSCO ile ölçülür.
**Araçlar:** BRAKER, MAKER, BUSCO.

### Gen ontolojisi ve yolak kaynakları

**Nedir:** Genleri işlev, süreç ve yolaklara bağlayan kontrollü sözlükler ve veri tabanları; zenginleştirme analizlerinin hammaddesi.
**Hangi soruyu cevaplar:** Bu gen listesi hangi biyolojik dile çevriliyor?
**Ne zaman kullanılır:** Her yorum aşamasında; kaynağın güncelliği ve anotasyon yanlılığı (çok çalışılan genler lehine) akılda tutulmalıdır.
**Araçlar:** GO, KEGG, Reactome; erişim için gseapy, clusterProfiler.

### Ağ tabanlı gen fonksiyonu tahmini

**Nedir:** İşlevi bilinmeyen genlere, etkileşim ve ko-ekspresyon ağlarındaki komşularından işlev atayan yaklaşımlar.
**Hangi soruyu cevaplar:** Bu bilinmeyen gen büyük ihtimalle ne iş yapıyor?
**Ne zaman kullanılır:** Aday gen karakterizasyonunda hipotez üretmek için; "komşusuna bak" çıkarımı kanıt değil, yönlendirmedir.
**Araçlar:** STRING, ağ yayılım yöntemleri, graf sinir ağları.
