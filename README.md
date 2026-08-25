# Melisa'yla Biyoinformatik — kurulum ve yayınlama (Windows)

Bu depo, MkDocs + Material ile yapılmış statik bir sitedir.
İçerik `docs/` klasöründeki Markdown dosyalarıdır; tasarım `docs/stylesheets/ozel.css` içindedir.

## 1) Bir kere yapılacak kurulum

1. **Python:** python.org'dan indir, kurulumda **"Add python.exe to PATH"** kutusunu mutlaka işaretle.
2. **Git:** git-scm.com'dan indir, varsayılan ayarlarla kur.
3. **VS Code:** code.visualstudio.com'dan indir (yazı editörü olarak).
4. github.com'da ücretsiz hesap aç.

## 2) Siteyi bilgisayarında çalıştır

Bu klasörün içinde boş bir yere Shift+sağ tık → "PowerShell penceresini burada aç" ve sırayla:

```powershell
pip install -r requirements.txt
mkdocs serve
```

Tarayıcıda `http://127.0.0.1:8000` adresine git. Bu pencere açıkken bir .md dosyasını
her kaydettiğinde site kendini yeniler. Durdurmak için: Ctrl+C.

> `mkdocs` komutu bulunamadı derse `python -m mkdocs serve` yaz.

## 3) GitHub'a koy ve yayınla

github.com'da `melisayla-biyoinformatik` adında **boş** bir depo aç (README ekleme), sonra bu klasörde:

```powershell
git init
git add .
git commit -m "İlk sürüm"
git branch -M main
git remote add origin https://github.com/KULLANICIADIN/melisayla-biyoinformatik.git
git push -u origin main
mkdocs gh-deploy
```

Site birkaç dakika içinde şu adreste yayında olur:
`https://KULLANICIADIN.github.io/melisayla-biyoinformatik/`

Sonraki her güncellemede dört komut yeter:

```powershell
git add .
git commit -m "Ne değiştiyse kısaca yaz"
git push
mkdocs gh-deploy
```

## 4) Sık düzenlenecek yerler

- Yeni rehber: `rehber-sablonu.md` dosyasını `docs/projeler/` altına kopyala,
  doldur ve `mkdocs.yml` içindeki `nav:` bölümüne ekle.
- Site adı/açıklaması: `mkdocs.yml` en üstteki iki satır.
- Renkler: `docs/stylesheets/ozel.css` en üstteki değişkenler.
- Hakkında sayfasındaki isim ve GitHub linki: `docs/hakkinda.md`.
