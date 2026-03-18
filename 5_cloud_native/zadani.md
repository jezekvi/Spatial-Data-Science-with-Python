# Lekce 5: Cloud-Native analýza a STAC API

Tato lekce se věnuje přechodu od tradičního stahování souborů k modelu **Cloud-Native Geospatial**. Místo lokálního ukládání dat využijeme v tomto případě infrastrukturu **Microsoft Planetary Computer**, kde s daty pracujeme jako se službou. Z cloudu si v reálném čase "streamujeme" pouze ty pixely, které skutečně potřebujeme pro výpočet.

**Odkaz k oficiální dokumentaci:**  [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/docs/)

---

## 1. Technologické pilíře

Abychom mohli s daty pracovat tímto způsobem, opíráme se o tři základní technologie:

### Cloud Optimized GeoTIFF (COG)
Na rozdíl od standardního GeoTIFFu, který je určen k tomu, abyste ho otevřeli z disku celý, je **COG** modifikován pro síťový přenos. Využívá vnitřní strukturu dlaždicování spolu s metadaty v hlavičce souboru, což umožňuje využít tzv. **HTTP Range Requests**. V praxi to znamená, že si software vyžádá pouze pixely pro konkrétní zájmové území, aniž by musel číst zbytek souboru.

### SpatioTemporal Asset Catalog (STAC)
STAC je standardizovaná specifikace pro indexaci a vyhledávání metadat o geoprostorových datech. Namísto procházení adresářových struktur různých poskytovatelů využíváme jednotné API. Dotazy se konstruují na základě definovaných parametrů skrze `pystac-client`:

* **Prostorové vymezení:** Geometrie nebo Bounding Box.
* **Temporální rozsah:** Časový interval (od–do).
* **Atributy metadat:** Filtrace (maximální přípustná oblačnost, konkrétní platforma atd.).

Výsledkem dotazu je seznam objektů (**Items**), který obsahuje metadata a přímé odkazy na konkrétní datové vrstvy (**Assets**).

### Lazy Evaluation
Tento koncept odděluje definici analytických operací od jejich samotného provedení. Namísto okamžitého stahování a zpracování pixelů si nejdříve pomocí knihovny `stackstac` vytvoříme virtuální **DataCube**. V paměti RAM se v první fázi nevytváří kopie dat, ale drží se pouze metadata a **výpočetní graf** (seznam instrukcí). K fyzickému čtení dat z cloudu a matematickým operacím dochází až v momentě požadavku na konkrétní výstup. Ten vyvoláte například příkazem `.compute()`.

---

## 2. Autorizace přístupu: Shared Access Signature (SAS)

Při práci s Microsoft Planetary Computer je nutné rozlišovat mezi přístupem k metadatům a k samotným datům. Zatímco STAC katalog je veřejný, binární data (jednotlivé COG soubory) jsou uložena v **Azure Blob Storage**, které je z bezpečnostních důvodů chráněno firewallem.

### Mechanismus tokenizace
K odemknutí dat v **Blob Storage** se se využívá **Shared Access Signature (SAS)**. Jde o delegovaný přístup, kdy uživatel obdrží dočasný klíč k danému assetu. V prostředí Pythonu tento proces automatizuje knihovna `planetary_computer`:

1.  **Vyhledání:** Získáte objekt `Item` z katalogu.
2.  **Podepsání:** Zavoláte funkci `pc.sign(item)`.
3.  **Token:** Služba vygeneruje SAS token – unikátní řetězec parametrů, který se připojí k URL adrese **Assetu**. 

Bez tohoto podpisu skončí pokus o čtení dat z Blob Storage chybou. Token má omezenou časovou platnost. Jakmile platnost vyprší, dříve vygenerované URL adresy přestanou fungovat a je nutné proces podepsání `pc.sign(item)` provést znovu.

---

## 3. Další služby a zdroje pro Cloud-Native analýzu

Díky standardu STAC můžete stejné postupy aplikovat i na další archivy. Většina těchto služeb nabízí bezplatný přístup pro akademické a výzkumné účely.

### Hlavní bezplatné alternativy

| Služba | Poskytovatel | Hlavní obsah | Specifika přístupu |
| :--- | :--- | :--- | :--- |
| **Copernicus CDSE** | Evropská unie | Kompletní mise Sentinel | Vyžaduje registraci a **OAuth2** autentizaci. |
| **NASA CMR-STAC** | NASA | Landsat, MODIS, VIIRS | Vyžaduje účet **NASA Earthdata** a autentizaci. |
| **Earth Search** | Element 84 (AWS) | Sentinel-2, Landsat | Přístupné **bez tokenu** pro základní prohlížení. Dále nutný účet AWS |

> *I když je proces vyhledávání dat skrze `pystac-client` u všech poskytovatelů téměř identický, **způsob tokenizace** se liší.*

### Kde hledat další data?
Pokud potřebujete specifická data, která nejsou v základních archivech, využijte globální rozcestníky:

* **[STAC Index](https://stacindex.org/):** Seznam všech veřejně dostupných STAC API na světě. Umožňuje vyhledávat konkrétní kolekce dat napříč poskytovateli.
* **[STAC Browser](https://radiantearth.github.io/stac-browser/):** Webové rozhraní pro vizuální prohlížení katalogů.

---

## 4. Zadání cvičení

Vaším úkolem bude vytvořit časovou řadu hodnot NDVI v Národním parku České Švýcarsko v oblasti zasažené požárem (červenec 2022). Hranici zájmového území definujte pomocí polygonu požáru, který naleznete ve složce `data_pozar`.

### Postup práce

1.  Nainstalujte a importujte knihovny.
2.  Načtěte shapefile ze složky `data_pozar` a transformujte jej do souřadnicového systému **EPSG:4326**.
3.  Inicializujte STAC klienta pro přístup k Microsoft Planetary Computer, definujte časové a prostorové rozmezí a vyhledejte dostupné snímky Sentinel-2 (L2A).
4.  Pomocí `stackstac` vytvořte DataCube obsahující pásma **B04 (Red)**, **B08 (NIR)** a **SCL (Scene Classification)**.
5.  Převeďte hodnoty DN na odrazivost (Reflectance) pomocí koeficientů z metadat.
6.  Proveďte prostorový ořez DataCube podle polygonu požáru.
7.  Pomocí pásma **SCL** vytvořte masku, která filtruje oblačnost.
8.  Vypočítejte NDVI pro všechny dostupné snímky.
9.  Pro každý snímek vypočítejte medián NDVI v rámci celého polygonu požáru a odstraňte časové kroky, které po maskování oblačnosti neobsahují dostatek validních dat.
10. Vykreslete graf časového vývoje NDVI.

### Pomocný kód

```python
# Instalace a import
!pip install -q pystac-client stackstac planetary-computer rioxarray

import pystac_client
import planetary_computer as pc
import stackstac
import geopandas as gpd
import pandas as pd
import rioxarray 
import matplotlib.pyplot as plt
```

```python
# Inicializace STAC klienta
catalog = pystac_client.Client.open("https://planetarycomputer.microsoft.com/api/stac/v1")
```
