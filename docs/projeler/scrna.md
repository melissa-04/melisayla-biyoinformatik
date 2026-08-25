# 2 · scRNA-seq (Tek hücre)

!!! question "Projenin sorusu"
    Bu dokuda hangi hücre tipleri var ve koşullar arasında (hastalık, tedavi, genotip) bu tipler nasıl değişiyor?

**Veri:** 10x'in klasik PBMC veri seti ile başlangıç; ardından GEO'dan bir hastalık veri seti.

**Araçlar:** Scanpy, AnnData, scrublet; görselleştirme UMAP.

## Çekirdek yol

1. Tek hücre teknolojisi: damlacıktan barkoda
2. AnnData ve Scanpy'ye giriş
3. Kalite kontrol: mitokondriyal oran, doublet tespiti
4. Normalizasyon ve yüksek değişkenlikli genler
5. Boyut indirgeme: PCA ve UMAP — kara kutuyu aç
6. Leiden ile kümeleme
7. Marker genler ve hücre tipi anotasyonu
8. Koşul karşılaştırması: pseudobulk yaklaşımı
9. Görselleştirme ve figür okuryazarlığı
10. Neler ters gider + bitirme projesi

## İleri modüller

Çekirdek yol tamamlandıkça eklenecek derinleşmeler:

Trajectory / pseudotime (PAGA) · RNA velocity (scVelo) · Hücre–hücre iletişimi (liana-py) · Veri entegrasyonu (Harmony, scVI) · Referans atlasa haritalama
