# Übung Pfade -- Lösung

## 🖥 Lokal

### Gegebene Verzeichnisstruktur

    C:\Daten\
    │
    ├── index.html
    ├── Bilder\
    │   ├── Blume.jpg
    │   └── test.html
    └── CSS\
        └── main.css

------------------------------------------------------------------------

## 1️⃣ Absoluter Pfad von `main.css`

    C:\Daten\CSS\main.css

------------------------------------------------------------------------

## 2️⃣ Relativer Pfad von `index.html` zu `Blume.jpg`

    Bilder/Blume.jpg

Beispiel:

``` html
<img src="Bilder/Blume.jpg" alt="Blume">
```

------------------------------------------------------------------------

## 3️⃣ Absoluter Pfad von `main.css` zu `Blume.jpg`

    C:\Daten\Bilder\Blume.jpg

------------------------------------------------------------------------

## 4️⃣ Relativer Pfad von `main.css` zu `Blume.jpg`

    ../Bilder/Blume.jpg

------------------------------------------------------------------------

## 5️⃣ Relativer Pfad von `test.html` zu `Blume.jpg`

    Blume.jpg

------------------------------------------------------------------------

# 🌍 Im Netz

## Gegeben

-   Domain: `ihreadresse.ch`
-   Lokaler Root-Pfad: `/srv/var/www/htdocs`
-   Document Root: `/htdocs`

------------------------------------------------------------------------

## 6️⃣ Lokaler & absoluter Pfad zu `Dokument.pdf`

    /srv/var/www/htdocs/wp-content/uploads/2022/5/Dokument.pdf

------------------------------------------------------------------------

## 7️⃣ Lokaler & absoluter Pfad zu `download.php`

    /srv/var/www/htdocs/wp-content/plugins/neon/files/download.php

------------------------------------------------------------------------

## 8️⃣ URL von `Dokument.pdf`

    https://ihreadresse.ch/wp-content/uploads/2022/5/Dokument.pdf

------------------------------------------------------------------------

## 9️⃣ URL von `download.php`

    https://ihreadresse.ch/wp-content/plugins/neon/files/download.php

------------------------------------------------------------------------

## 🔟 Relativer Pfad von `download.php` zu `Dokument.pdf`

    ../../../uploads/2022/5/Dokument.pdf
