# 5 · Klinik varyant yorumlama

!!! question "Projenin sorusu"
    Bir ekzom dizileme verisindeki hangi varyant, gözlenen fenotiple ilişkili olabilir?

**Veri:** Halka açık bir ekzom örneği (1000 Genomes / Genome in a Bottle alt kümesi; tek kromozomla küçültülmüş).

**Araçlar:** bwa/samtools/bcftools komut satırında; filtreleme ve yorum Python'da; anotasyon VEP ile.

## Çekirdek yol

1. Referans genom ve hizalama kavramı
2. SAM/BAM/VCF: üç formatı satır satır okumak
3. Varyant çağırma: küçültülmüş veriyle uçtan uca
4. Varyant filtreleme: kaliteden frekansa
5. VEP ile anotasyon
6. Popülasyon frekansları: gnomAD ve popülasyona uygun kaynaklar
7. ACMG mantığına giriş: kanıt toplamak
8. Vaka çalışması + neler ters gider + bitirme

## İleri modüller

Çekirdek yol tamamlandıkça eklenecek derinleşmeler:

Yapısal varyantlara giriş · Anotasyonun derinleri · Türk popülasyon verisiyle yorum farkları
