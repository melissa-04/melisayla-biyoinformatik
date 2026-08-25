# 1 · RNA-seq: KO vs. yabani tip

!!! question "Projenin sorusu"
    Bir geni susturduğumuzda (knockout) hangi genlerin ifadesi değişiyor ve bu değişim biyolojik olarak ne anlama geliyor?

**Veri:** GEO'dan gerçek bir knockout vs. yabani tip RNA-seq deneyi (küçültülmüş alt küme; Colab'da dakikalar içinde çalışır).

**Araçlar:** Python ağırlıklı — Salmon, PyDESeq2 (R'daki DESeq2'nin Python uyarlaması), gseapy, matplotlib/seaborn.

## Çekirdek yol

1. RNA-seq nedir, neden yapılır? — deney tasarımından veri türüne
2. FASTQ ve kalite skorları — Phred ölçeğini sökmek
3. Kalite kontrol: FastQC çıktısını okumak
4. Kantifikasyon: Salmon ile transkript sayımı
5. Kara kutuyu aç: pseudoalignment aslında ne yapar?
6. Sayım matrisinden analize: pandas ve AnnData
7. PyDESeq2: normalizasyon ve model
8. Kara kutuyu aç: p-değeri düzeltmesi neden şart?
9. Görselleştirme: volkan grafiği, PCA, ısı haritası
10. gseapy ile yolak zenginleştirme ve biyolojik yorum
11. Neler ters gider: sık hatalar ve kontrol listesi
12. Bitirme: aynı analizi kendi seçtiğin KO verisiyle yap

## İleri modüller

Çekirdek yol tamamlandıkça eklenecek derinleşmeler:

WGCNA ile ko-ekspresyon ağları (PyWGCNA) · Alternatif splicing / DTU · miRNA-seq · Bulk dekonvolüsyon (hücre tipi oranları) · GSEA'nın derinleri
