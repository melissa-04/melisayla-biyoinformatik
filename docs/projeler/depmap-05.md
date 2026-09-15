# Bağımlılığın nedeni: mutasyon

!!! question "Bu rehberde"
    Bağımlılık skorlarını mutasyon verisiyle birleştirecek, onkogen bağımlılığını sayıyla ölçecek ve mutasyonun neden güçlü ama yetersiz bir yordayıcı olduğunu göreceksiniz.

[Colab'da aç](https://colab.research.google.com/github/melissa-04/melisayla-biyoinformatik/blob/main/notebooks/depmap/05_mutasyon.ipynb){ .md-button .md-button--primary }

## Önce dosyayı doğru okumak

Bu rehber yeni bir dosyayla çalışıyor: DepMap 25Q2'nin hotspot mutasyon matrisi. Ve ilk dersi analizden önce geliyor — dosya beklediğiniz biçimde gelmiyor.

Skor matrisinde satır adları doğrudan hücre hattı kimliğiydi. Bu dosyada ise satır adı yok; onun yerine `SequencingID`, `ModelID`, `ModelConditionID`, `IsDefaultEntryForModel` gibi künye sütunları var ve gen sütunları bunlardan sonra başlıyor. Dosyayı alışkanlıkla `index_col=0` ile okursanız satır adı olarak sıra numaraları gelir, iki tablonun kesişimi sıfır çıkar ve pandas bunu hata olarak bildirmez — sessizce boş bir analiz yaparsınız.

Üç düzeltme gerekti. `ModelID` sütunu satır adı yapıldı. Gen sütunları `SEMBOL (EntrezID)` kalıbına göre ayrıldı — künye sütunlarını elle saymak yerine, çünkü koşumda tam da bu yüzden bir sürpriz çıktı: `ZNF781 (Unknown)` diye bir sütun var, yani her gen sütununda düzgün bir Entrez kimliği yok ve elle sayım yanlış sonuç verirdi. Son olarak tekrarlayan satırlar: aynı hücre hattının birden fazla dizileme kaydı olabildiği için dosyada 3.044 satır var; `IsDefaultEntryForModel` ile süzünce 1.968 kayıt kaldı ve skor tablosuyla kesişim tam 1.208 hücre hattı oldu — yani hiçbir hat kaybedilmedi.

## Onkogen bağımlılığı, sayıyla

Kavram şu: bir tümör hücresi sürekli açık kalmış bir onkogeni sürücü olarak kullanıyorsa, o genin ürününü kaybetmeye tahammülü yoktur. Mutasyonun kendisi hücreyi o gene bağımlı hale getirir. Koşumun sonuçları bunu net biçimde ölçüyor:

- **KRAS**: 177 mutant hatta medyan −1,89; 1.031 yabanıl hatta −0,47. Fark 1,42.
- **NRAS**: 62 mutant hatta −1,39; yabanılda −0,14. Fark 1,25.
- **BRAF**: 91 mutant hatta −1,13; yabanılda −0,08. Fark 1,05.
- **CTNNB1**: 31 mutant hatta −0,94; yabanılda −0,14. Fark 0,81.
- **PIK3CA**: 120 mutant hatta −0,95; yabanılda −0,41. Fark 0,53.

4.4'ün açık bıraktığı soru burada cevaplanıyor. Bağırsak hatları CTNNB1'e beta-katenin yolağı sürekli açık olduğu için bağımlı; mutant hatların medyanı bağımlılık eşiğine yaklaşırken yabanıl hatlar sıfır civarında duruyor.

Oran tablosu farkı daha da keskin gösteriyor: KRAS mutant hatların **yüzde 87'si** bağımlı (skor < −1), yabanıllarınsa yalnız yüzde 6,3'ü. BRAF'ta bu ikili yüzde 62,6'ya karşı yüzde 0,3 — yani BRAF yabanıl bir hattın BRAF'a bağımlı olması neredeyse hiç görülmüyor.

## Mutasyon yeterli değil

Aynı tablo tersini de söylüyor. KRAS mutant hatların yüzde 13'ü bağımlı değil; PIK3CA'da mutantların yalnız yüzde 42,5'i bağımlı. Mutasyon güçlü bir yordayıcıdır ama kesin bir kural değildir.

Nedenleri sıralanabilir: hücre aynı yolağı başka bir üyeyle sürdürüyor olabilir (KRAS yabanıl ama NRAS mutant bir hat yine RAS yolağına bağımlıdır), gen mutasyonsuz ama amplifiye olabilir, ya da hücre o yolağı bırakıp başka bir sürücüye geçmiş olabilir. PIK3CA'nın düşük oranı bu açıdan öğretici: PI3K yolağı hücrelerin sıklıkla yedeği olan bir yolaktır, dolayısıyla mutasyon her zaman bağımlılık yaratmıyor.

EGFR ise sınır durumu gösteriyor: yalnız 13 mutant hat var ve iki grubun medyanı arasındaki fark 0,01. Bu "EGFR önemsiz" demek değil — 13 hat, bir sonuç kurmak için çok az. Küçük gruplarda fark yokluğu, yokluğun kanıtı değildir.

## Sentetik letalite ve dürüst bir başarısızlık

Bağımlılığın en değerli biçimi çapraz olanıdır: bir gendeki kayıp, başka bir geni yaşamsal hale getirir. Buna sentetik letalite denir ve ilaç geliştirme açısından onkogen bağımlılığından daha çekicidir, çünkü hedeflenen gen sağlıklı hücrede vazgeçilebilir kalır — terapötik pencere doğal olarak açılır.

Ders kitabı örneğini denedik: VHL kaybı olan böbrek hücreli karsinom hatlarının HIF2A (gen adı EPAS1) bağımlılığı. Sonuç beklediğimiz gibi çıkmadı. Koşumda 35 böbrek hattında EPAS1 medyanı −0,06, diğer 1.173 hatta 0,01; bağımlı oranı yüzde 2,9'a karşı yüzde 0. Yani bu veride belirgin bir HIF2A bağımlılığı görünmüyor.

Sonucu saklamak yerine yazıyorum, çünkü nedeni öğretici. Bu karşılaştırmayı dokuya göre yaptık — elimizdeki matris hotspot mutasyonlarını içeriyor, VHL kaybı ise işlev *yitiren* mutasyonlarla olur ve o veri başka bir dosyada (`OmicsSomaticMutationsMatrixDamaging.csv`). Böbrek hatlarının hepsi VHL-kayıplı değildir, dolayısıyla doku, mutasyon durumunun kaba bir vekilidir. Ayrıca DepMap'teki böbrek hatları içinde berrak hücreli karsinom (VHL kaybının tipik görüldüğü alt tip) azınlıkta olabilir. Doğru test, hatları VHL durumuna göre ayırmayı gerektirir; doku ile yapılan yaklaşık test bunu gösteremiyor.

Ders şu: bir sonuç beklentinizi karşılamıyorsa önce sorunun doğru sorulup sorulmadığına bakın. Burada veri yanlış değildi, vekil değişken yeterince keskin değildi.

## Neler ters gider?

Üç tuzak. Birincisi dosya yapısını varsaymak; yukarıda sessiz boş analizi gördünüz — yeni bir dosya açtığınızda ilk iş sütun adlarını yazdırmaktır. İkincisi küçük gruplardan sonuç çıkarmak: 13 hatlık EGFR karşılaştırması ne olumlu ne olumsuz bir kanıttır. Üçüncüsü vekil değişkeni gerçek değişken sanmak: "böbrek hattı" ile "VHL-kayıplı hat" aynı şey değildir ve aradaki fark sonucu tersine çevirebilir.

## Kendin dene

Defterin sonunda üç görev var. Okuma sorununu kendi sözlerinizle anlatın: dosya neden doğrudan okunamıyordu ve düzeltilmeseydi analiz ne verirdi? KRAS için iki grubun medyanlarını ve farkı not edip onkogen bağımlılığını tanımlayın, sonra yabanıl hatlarda da bağımlılık görülmesinin olası bir nedenini yazın. Ve EPAS1 sonucunu not edip tek cümleyle açıklayın: sentetik letalite ilaç geliştirme açısından neden daha değerlidir, ve bu testin sonuç vermemesinin nedeni ne olabilir?
