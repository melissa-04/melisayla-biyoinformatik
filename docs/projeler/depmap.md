# 4 · DepMap / CRISPR ekranları

!!! question "Projenin sorusu"
    Genom çapında knockout yapıldığında hangi genler hangi kanser hücre hatlarında yaşamsal çıkıyor — ve bu, hedef gen seçimi için ne anlama geliyor?

**Veri:** DepMap'in halka açık CRISPR knockout ekran skorları (düz CSV; boru hattı yok, saf veri analizi). Altı proje içinde en hızlı sonuç veren budur.

**Araçlar:** pandas, seaborn/matplotlib.

## Çekirdek yol

1. Genom çapında CRISPR ekranı nedir, DepMap nasıl üretildi?
2. Veriyi indirmek ve pandas ile tanımak
3. Esansiyellik skorunu okumak: bir genin profili
4. Kanser tipleri arasında karşılaştırma
5. Seçici bağımlılıklar: "sadece bu kanserde yaşamsal" genler
6. Görselleştirme ve yorum
7. Neler ters gider: skorların sınırları
8. Bitirme: kendi seçtiğin genin/kanserin analizi

## İleri modüller

Çekirdek yol tamamlandıkça eklenecek derinleşmeler:

MAGeCK ile ham ekran analizi · Ko-esansiyellik ağları · Halka açık veriyle Perturb-seq'e giriş
