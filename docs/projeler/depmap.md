# 4 · DepMap / CRISPR ekranları

!!! question "Projenin sorusu"
    Genom çapında nakavt yapıldığında hangi genler hangi kanser hücre hatlarında yaşamsal çıkıyor — ve bu, hedef gen seçimi için ne anlama geliyor?

Serinin dördüncü projesi öncekilerden farklı: ham okuma, hizalama, sayım yok. Broad Institute'un DepMap projesi 1.208 kanser hücre hattında genom çapında CRISPR-Cas9 nakavt ekranı yapmış ve sonuçları hazır bir tablo olarak yayımlamış. Buradaki beceri boru hattı kurmak değil, büyük bir tabloya doğru soruları sormak: pandas ve görselleştirme ağırlıklı, saf veri analizi.

**Veri:** DepMap Public 25Q2'den türetilmiş ders alt kümesi — 1.208 hücre hattı × 5.000 gen. Kendi DOI'sinde yayımlandı: [10.5281/zenodo.22759375](https://doi.org/10.5281/zenodo.22759375). Defterler veriyi doğrudan oradan okur; kurulum gerekmez, dosya indirmezsiniz.

![Genlerin medyan bağımlılık skoru ve hatlar arası değişkenliği](gorseller/06_bagimlilik_haritasi.png)
*Her nokta bir gen. İyi ilaç hedefleri sol alt köşede değil, mor bölgede aranır.*
