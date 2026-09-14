# Bölgeler ve komşuluk

!!! question "Bu rehberde"
    Küme numaralarını anatomik bölge adlarına bağlayacak ve komşuluk zenginleştirmesiyle hangi bölgelerin doku üzerinde yan yana durduğunu ölçeceksiniz — tek hücre verisinde sorulamayan soru.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/spatial/04_bolgeler_komsuluk.ipynb){ .md-button .md-button--primary }

## İşaretçiden adrese

Adlandırma yöntemi 2.5'teki dotplot'un uzamsal karşılığıdır: her işaretçi geni için ortalama ifadeyi küme küme hesaplar, sıralarız; tepedeki küme o işaretçinin adresidir. İşaretçiler `v.raw` içinden okunur, çünkü daraltılmış matriste bulunmayabilirler.

Koşumun tablosu net adresler verdi. Mbp ve Mobp — iki bağımsız miyelin proteini — aynı kümeyi işaret ediyor: küme 3 (5,13 ve 3,82). İki ayrı işaretçinin aynı kümede buluşması, adlandırmanın en güçlü kanıt biçimidir. Hpca küme 12'de (3,74), Ttr küme 11'de (4,83) zirve yapıyor. Slc17a7, Snap25 ve Nrgn ise 12, 0, 6 ve 13 arasında paylaşılıyor; bunlar genel nöronal işaretçiler olduğu için tek bir bölgeyi değil gri maddenin tamamını gösterirler ve tam da bu yüzden korteks katmanlarını birbirinden ayırmaya yetmezler.

Buradan çıkan adlandırma: küme 3 ak madde, küme 12 hipokampus, küme 11 koroid pleksus, küme 0 korteks. Geri kalan on küme numarasıyla kalıyor ve bu bir eksiklik değil, dürüstlüktür: elimizdeki yedi işaretçi on dört bölgeyi adlandırmaya yetmez; daha fazla ad için daha fazla işaretçi ya da bir referans beyin atlası gerekir.

## Komşuluk zenginleştirmesi

`sq.gr.nhood_enrichment(v, cluster_key='leiden')` serinin en özgün aracıdır. Uzamsal komşuluk grafiğindeki her komşu spot çiftinin hangi kümelere ait olduğunu sayar, sonra küme etiketlerini rastgele karıştırıp aynı sayımı bin kez tekrarlar ve gerçek sayının rastgele dağılımdan kaç standart sapma uzak olduğunu verir. Sonuç bir z-skoru matrisidir: yüksek pozitif değer iki kümenin komşu olmayı "tercih ettiğini", negatif değer birbirinden kaçındığını gösterir.

Koşumda matris 14×14 ve değerler −18 ile 80 arasında. Köşegen medyanı 61: her küme en çok kendi kendisiyle komşu, ki beklenen sonuç — bir kümenin spotları zaten bir arada oturur. Bilgi köşegenin dışındadır.

En güçlü komşuluk küme 4 ile 12 arasında (z=30,5). Küme 12 hipokampus; küme 4'ün ayırt edici genleri ise ağırlıkla mitokondriyal (mt-Nd3, mt-Nd2, mt-Atp6) ve yanlarında Camk2a duruyor — yani yüksek enerji tüketimli, nöron yoğun bir bölge. Hipokampusun piramidal hücre katmanı tam olarak böyle bir bölgedir ve bu iki kümenin komşuluğu anatomiyle uyuşur.

En güçlü kaçınma küme 0 ile 1 arasında (z=−18,1). Küme 0'ın işaretçileri Tbr1 ve Dclk1: korteks nöronları. Küme 1'de Agt, Slc6a11 ve Tcf7l2 var: astrosit ve talamus sinyalleri. İkisi dokunun farklı bölgelerinde oturuyor, dolayısıyla komşu çıkmaları için bir sebep yok — negatif z-skorunun anlamı budur, bir kusur değil bir mesafe ölçüsüdür.

## Bu analiz neden tek hücrede yapılamaz

İkinci seride hücre tiplerini bulduk ama hangi hücrenin hangisinin yanında durduğunu bilemedik: doku, süspansiyon hazırlanırken parçalanmıştı ve konum bilgisi geri döndürülemez biçimde kayboldu. Komşuluk zenginleştirmesi doğrudan `obsm['spatial']` koordinatlarına dayanır; koordinat yoksa komşuluk grafiği kurulamaz, kurulamayınca soru sorulamaz. Uzamsal verinin tek hücreye kattığı asıl şey budur ve bu serinin varlık sebebidir.

## Neler ters gider?

Üç uyarı. Birincisi, komşuluğu etkileşim sanmak: z-skoru iki bölgenin yan yana durduğunu söyler, birbirleriyle sinyalleştiğini değil; hücre-hücre iletişimi ayrı yöntemler ve ayrı kanıt ister. İkincisi, adlandırmayı zorlamak: yedi işaretçiyle on dört kümeyi adlandırmaya kalkarsanız uydurma adlar üretirsiniz — numara olarak kalan küme, yanlış adlandırılmış kümeden iyidir. Üçüncüsü, spot düzeyini hücre düzeyi saymak: "ak madde kümesi" o bölgedeki spotların baskın içeriğini anlatır, her spotun saf oligodendrosit olduğunu değil; hücre tipi oranları için dekonvolüsyon gerekir.

## Serinin kapanışı

Dört rehberde şu yol yüründü: Visium verisinin klasör düzeni, spot kavramı ve doku bağımlı kalite eşikleri (3.1); tanıdık kümeleme hattının uzamsal veriye uygulanması ve anatominin ifadeden kendiliğinden çıkması (3.2); Moran's I ile uzamsal desen ölçümü ve "değişken gen ≠ uzamsal gen" ayrımı (3.3); işaretçilerle adlandırma ve komşuluk zenginleştirmesi (3.4). Veri kendi DOI'sinde yaşıyor: 10.5281/zenodo.22758390.

Önceki serilerin kuralları burada da geçerliydi — özellikle "eşik dokuya göre seçilir", ki bu seri onu yüzde 15,3'lük mitokondriyal medyanla somutladı. Üstüne üç yenisi eklendi: spot hücre değildir; ifade uzayındaki komşuluk ile fiziksel komşuluk iki ayrı graftır; ve uzamsal tutarlılık, kümeleme sonucunu bağımsız olarak sınamanın yoludur.

Öğrenmediklerimiz sonraki adımların kapıları: dekonvolüsyon (her spotun hücre tipi karışımını tek hücre referansıyla çözmek), çoklu kesit karşılaştırması, ligand-reseptör analiziyle hücre-hücre iletişimi ve tek hücre tabanlı platformlar. Meme tümörü kesitlerinde asıl ilginç soruların çoğu bu kapıların arkasında.

## Kendin dene

Defterin sonunda üç görev var. İşaretçi tablonuzdan en az dört kümeyi anatomik bölgeyle eşleştirip adlandırılmış haritanızı çizin ve her ad için dayandığınız işaretçiyi yazın. Komşuluk matrisinizdeki en yüksek z-skorlu çifti bulup iki kümenin adını yazın ve bu komşuluğun bilinen anatomiyle uyuşup uyuşmadığını tek cümleyle değerlendirin. Ve serinin kapanış sorusunu cevaplayın: bu analiz tek hücre verisinde neden yapılamazdı?
