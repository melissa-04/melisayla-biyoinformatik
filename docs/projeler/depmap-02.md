# Tabloyu tanımak: skorların dağılımı

!!! question "Bu rehberde"
    Altı milyon bağımlılık ölçümünün nasıl dağıldığını görecek, genleri medyan ve değişkenliğe göre haritalayacak ve ortak esansiyel ile etkisiz genleri sayıyla ayıracaksınız.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/depmap/02_dagilim.ipynb){ .md-button .md-button--primary }

## Altı milyon ölçüm

Alt küme 1.208 hücre hattı × 5.000 gen, yani yaklaşık 6 milyon bağımlılık ölçümü. İlk resim hepsini tek histogramda gösteriyor.

Koşumda medyan −0,257; birinci yüzdelik −2,75, doksan dokuzuncu +0,33. Ölçümlerin yüzde 34,1'i −0,5'in altında. Bu sayıları birlikte okuyun: dağılımın kütlesi sıfırın hemen solunda toplanıyor, uzun bir kuyruk aşağı doğru uzanıyor, üst tarafta neredeyse hiç şey yok. Anlamı şu: taranan genlerin çoğunu kapatmak, çoğu hücre hattı için ciddi bir sorun değil. Kanser hücresi genomunun büyük kısmı, laboratuvar koşullarında çoğalması için vazgeçilebilirdir.

Bir uyarıyı burada yapalım: bu oran alt kümenin bileşimine bağlıdır. 4.1'de en değişken 4.000 geni bilerek seçmiştik, yanına dürüstlük için 1.000 rastgele gen eklemiştik. Tam matriste sıfır çevresindeki kütle daha da büyük olurdu. Dağılımın şekli gerçek, kesin yüzde ise alt kümeye özgü.

## Medyan ve değişkenlik haritası

İkinci resim soruyu gene çeviriyor: her gen için bütün hatlardaki medyan ve standart sapma. İki sayı iki ayrı şey söyler — medyan tipik ölümcüllüğü, standart sapma hatlar arası değişkenliği.

Koşumun özeti: gen medyanlarının ortalaması −0,467, en düşüğü −4,166; standart sapmalar 0,083 ile 0,951 arasında. Haritada üç bölge tanınır. Sol alt köşe: her hatta ölümcül, hattan hatta az değişen genler. Sağ alt köşe: etkisiz ve sabit. Ve ortada yukarı doğru uzanan bölge: medyanı ılımlı ama değişkenliği yüksek genler — bazı hatlarda ölümcül, bazılarında değil. Projenin aradığı genler oradadır ve 4.4 bu haritaya geri dönecek.

## İki uç, sayıyla

Tanımları eşikle kuruyoruz. Bir geni "ortak esansiyel" saymak için hatların en az yüzde 90'ında skorunun −0,5 altında olmasını, "etkisiz" saymak için hiçbir hatta bu eşiği geçmemesini istedik. Eşik bir karardır: sıfır çevresindeki gürültüden yeterince uzak, −1 kadar da katı değil; raporda değeriyle yazılır.

Koşumda 5.000 genin **961'i** ortak esansiyel, **287'si** hiçbir hatta etkisiz, **3.752'si** ikisinin arasında. Aradaki bu geniş küme, bu projenin asıl çalışma alanı.

En ölümcül on genin listesi ise kendi başına bir ders: RAN (−4,17), RPL17, RPS8, RRM1, SNRPF, SNRPA1, PSMA6, RPL12, PCNA, PSMB3. Ribozom proteinleri, spliceosome bileşenleri, proteazom alt birimleri, nükleotid sentezi ve DNA replikasyonu. Yani hücrenin en temel makineleri — protein üretimi, RNA işlenmesi, protein yıkımı, DNA kopyalanması.

## Neden bu genler kötü ilaç hedefleridir

Sezgi tersini söyler: en çok öldüren gen en iyi hedef olmalı. Ama ilaç hastanın bütününe verilir ve bu genler sağlıklı hücrelerde de aynı ölçüde yaşamsaldır. Ribozomu durduran bir ilaç tümörü öldürürken kemik iliğini, bağırsak epitelini, üretken bütün dokuları da öldürür. Aranan şey en güçlü öldürücü değil, **terapötik pencere**: tümör hücresini öldürüp normal hücreyi görece bağışlayan bir fark.

Bu yüzden ortak esansiyel listesi, aday listesi değil eleme listesidir. Bir hedef önerisi geldiğinde ilk kontrol şudur: bu gen zaten her yerde mi ölümcül? Cevap evetse, bağımlılık skoru ne kadar çarpıcı olursa olsun hedef değildir.

## KRAS dağılımı ve sıradaki soru

Defterin son resmi üç geni yan yana koyuyor: en ölümcül ortak esansiyel, etkisizlerden biri ve KRAS. İlk ikisinin dağılımı dar ve tek tepeli — biri çok solda, diğeri sıfırda. KRAS ise yayılmış: bir kısmı hatlar sıfır civarında, bir kısmı çok aşağıda. 4.1'de gördüğümüz −0,52'lik medyan, bu iki grubun ortalamasından ibaretmiş.

Buradan doğru soru çıkıyor: KRAS'ın öldürdüğü hatlarla umursamayan hatlar arasındaki fark ne? Cevap bir sonraki rehberin konusu.

## Neler ters gider?

Üç tuzak. Birincisi, medyanla karar vermek: KRAS örneği gösterdi, bimodal bir dağılımın ortası hiçbir hattı temsil etmez. İkincisi, eşiği veri özelliği sanmak: −0,5 ve yüzde 90 bizim seçimimizdir, başka eşikler başka sayılar verir ve sonuç raporlanırken eşik de yazılır. Üçüncüsü, oranları alt kümeden genele taşımak: buradaki yüzdeler 5.000 genlik ders kümesine aittir, genom çapındaki gerçek oranlar değil.

## Kendin dene

Defterin sonunda üç görev var. Bütün skorların medyanını, uç dilimlerini ve −0,5 altındaki ölçüm oranını not edip tek cümleyle yazın: bu dağılım taranan genlerin çoğu hakkında ne söylüyor? Ortak esansiyel ve etkisiz gen sayılarınızı yazın, en ölümcül on genin işlevlerine bakın ve neden kötü ilaç hedefi olduklarını tek cümleyle açıklayın. Ve üç histogramı karşılaştırıp KRAS'ınkinin farkını tarif edin.
