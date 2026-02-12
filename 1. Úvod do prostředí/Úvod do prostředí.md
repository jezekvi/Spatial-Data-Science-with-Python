# 1. Teoretický úvod a konfigurace prostředí

Předtím, než začneme psát první řádky kódu, je nezbytné porozumět ekosystému nástrojů, které budeme používat. Ve Spatial Data Science nepoužíváme pouze jeden program, ale kombinaci programovacího jazyka, vývojového prostředí a správce balíčků.

## 1.1 Python

**Python** je interpretovaný, vysokoúrovňový programovací jazyk, který se stal de facto standardem pro Spatial Data Science.

### Proč je Python tak populární?
* **Čitelnost:** Syntaxe Pythonu je blízká přirozené angličtině, což usnadňuje učení a údržbu kódu.
* **Ekosystém knihoven:** Pro Python existují tisíce specializovaných knihoven. V GIS světě jsou to zejména `GeoPandas` (vektory), `Rasterio` (rastry), `PySAL` (prostorová statistika) a mnoho dalších...
* **Komunita:** Díky obrovské základně uživatelů existuje řešení pro téměř každý problém na platformách jako Stack Overflow.

## 1.2 Jupyter Notebook

**Jupyter Notebook** (soubory s příponou `.ipynb`) není programovací jazyk, ale interaktivní prostředí, ve kterém Python běží. 

* **Princip buněk:** Notebook se skládá z buněk, které mohou obsahovat buď **spustitelný kód**, nebo **formátovaný text** (Markdown).
* **Literate Programming:** Tento přístup umožňuje kombinovat kód, výpočty, vizualizace (mapy, grafy) a doprovodný text do jednoho dokumentu.
* **Vztah s Pythonem:** Jupyter slouží jako pracovní rozhraní. Samotný kód vykonává **kernel** – spuštěná instance Pythonu, která vrací výstupy zpět do dokumentu.

## 1.3 Lokální vývojové prostředí: VS Code a Anaconda

Pro efektivní práci s Pythonem na vlastním počítači potřebujeme dvě věci: editor (kde kód píšeme) a distribuci Pythonu (která kód spouští).

### Visual Studio Code (VS Code)
**Odkaz ke stažení:** [VS Code](https://code.visualstudio.com/)  
VS Code je v současnosti nejoblíbenější editor kódu.

**Proč používat VS Code pro Jupyter?**
1.  **Vše v jednom:** Máte v něm editor, terminál, prohlížeč proměnných i interaktivní okna pro mapy.
2.  **Rozšíření:** Pomocí pluginů si VS Code přizpůsobíte (např. podpora pro GitHub nebo specifické GIS formáty).
3.  **Zdarma:** Editor je zcela zdarma a postavený na open-source principech.

### Anaconda / Miniconda
Namísto instalace Pythonu přímo do systému je výhodnější použít distribuci **Anaconda**, která spravuje izolovaná prostředí a minimalizuje konflikty mezi knihovnami.  
[Odkaz ke stažení Anacondy](https://www.anaconda.com/download)  
[Návod na instalaci Anacondy](https://www.anaconda.com/docs/getting-started/anaconda/install)

* **Conda:** Funguje jako správce balíčků a prostředí (**Environment Manager**). Umožňuje vytvořit izolovaný prostor pro každý projekt. 
* **Proč?** Knihovny jako `GeoPandas` mají složité závislosti na C++ knihovnách (GDAL, GEOS, PROJ). Conda tyto závislosti řeší automaticky při instalaci, což standardní nástroje `pip` často nezvládnou.

---

# 2. Inicializace a konfigurace Jupyter Notebooku ve VS Code

Tento manuál popisuje postup od prvního spuštění editoru po zprovoznění interaktivního prostředí Jupyter. Předpokladem je korektně nainstalovaný editor VS Code a distribuce Anaconda.

## 2.1 Instalace nezbytných rozšíření
Visual Studio Code vyžaduje pro interpretaci kódu Python a práci s notebooky instalaci specifických doplňků.

1.  Spusťte **Visual Studio Code**.
2.  Přejděte do sekce **Extensions** (ikona v levém panelu nebo zkratka `Ctrl+Shift+X`).
3.  Vyhledejte a nainstalujte rozšíření **Python** (vydavatel Microsoft).
4.  Vyhledejte a nainstalujte rozšíření **Jupyter** (vydavatel Microsoft).


## 2.2 Vytvoření souboru Jupyter Notebook
Soubory Jupyter Notebook využívají příponu `.ipynb` (IPython Notebook).

1.  V horním menu zvolte **File** > **New File...**
2.  Z nabídky typů souborů vyberte **Jupyter Notebook**.
3.  Alternativně soubor uložte přes **File** > **Save As...** s příponou `.ipynb` (např. `cviceni_01.ipynb`).

## 2.3 Propojení s interpretrem (Výběr Kernelu)
Kernel (jádro) je výpočetní engine, který spouští kód v notebooku. Je nutné propojit dokument s konkrétní instalací Pythonu.

1.  V pravém horním rohu editoru klikněte na tlačítko **Select Kernel**.
2.  V rozbalovacím menu zvolte **Python Environments...**.
3.  Vyberte instanci označenou jako `base (conda)` nebo verzi Pythonu odpovídající vaší instalaci Anacondy.
4.  Úspěšné propojení je signalizováno zobrazením verze Pythonu v pravém horním rohu (např. *Python 3.10.x*).

## 2.4 Verifikace funkčnosti prostředí
Pro ověření integrity spojení mezi editorem a interpretrem proveďte test v první buňce:

1.  Ujistěte se, že je typ buňky nastaven na **Code**.
2.  Vložte následující kód:
    ```python
    import sys
    print(f"Interpret: {sys.executable}")
    ```
3.  Spusťte buňku pomocí ikony **Play** vlevo nebo klávesovou zkratkou **Shift + Enter**.
4.  Korektní výstup bez chybových hlášení potvrzuje připravenost prostředí.

## 2.5 Základní prvky rozhraní
* **Code Cell:** Buňka pro zápis a spouštění algoritmu.
* **Markdown Cell:** Buňka pro strukturovaný text, rovnice a dokumentaci.
* **Variables:** Tlačítko v horní liště pro zobrazení aktivních proměnných v paměti RAM.
* **Restart:** Restartování jádra (vymaže všechny proměnné z paměti, nutné při zacyklení nebo chybě importu).

---

# 3. Online a cloudová řešení (SaaS)

Pokud není k dispozici výkonný hardware nebo je instalace lokálního prostředí (Anaconda) problematická, lze využít cloudová řešení. Tato rozhraní umožňují spouštět Jupyter Notebooky přímo v prohlížeči.

## 3.1 Výhody a nevýhody cloudových řešení

Práce v cloudu přináší specifické benefity, ale i limity, které je nutné při analýze prostorových dat zohlednit.

| Výhody | Nevýhody |
| :--- | :--- |
| **Nulová instalace:** Prostředí je předkonfigurované a připravené k okamžitému použití. | **Závislost na konektivitě:** Stabilní internetové připojení je nezbytnou podmínkou. |
| **Výpočetní výkon:** Přístup k výkonným CPU a GPU zdarma nebo za poplatek. | **Omezení paměti:** U bezplatných verzí může dojít k ukončení procesu při zpracování velkých datasetů (RAM). |
| **Dostupnost dat:** Přímý přístup k satelitním archivům bez nutnosti stahovat stovky GB dat. | **Soukromí a bezpečnost:** Data jsou nahrávána na servery třetích stran (riziko u citlivých údajů). |
| **Kolaborace:** Snadné sdílení notebooků podobně jako u dokumentů Google Docs. | **Dočasnost:** Bezplatná sezení jsou časově omezená; neuložená data mohou být po odpojení smazána. |

---

## 3.2 Přehled vybraných platforem

Níže jsou uvedeny některé portály, které nabízejí Jupyter rozhraní.

### [Google Colab](https://colab.research.google.com/)
Nejoblíbenější platforma pro obecný vývoj v Pythonu. 
* **Hlavní výhoda:** Integrace s Google Drive a bezplatný přístup k výkonným GPU.
* **Využití:** Škálovatelné analýzy, Deep Learning, rychlé testování kódu.

### [Jupyter.org (Try Jupyter)](https://jupyter.org/try)
Oficiální demo rozhraní projektu Jupyter.
* **Hlavní výhoda:** Okamžitý přístup k prostředí bez nutnosti registrace.
* **Využití:** Krátkodobé testování syntaxe, výuka základů Pythonu. Data se po zavření prohlížeče neukládají.

### [Copernicus Data Space Ecosystem](https://dataspace.copernicus.eu/analyse/jupyterlab)
Evropský standard pro práci s daty ze satelitů **Sentinel**.

### [OpenSARLab (ASF)](https://opensarlab-docs.asf.alaska.edu/)
Specializovaná laboratoř Alaska Satellite Facility.

### [ICOS Jupyter Hub](https://www.icos-cp.eu/data-services/tools/jupyter-notebook)
Portál zaměřený na environmentální data o skleníkových plynech.

---

# 4. Dokumentace v Jupyter Notebooku: Markdown

V prostředí Jupyter Notebook není text pouze doprovodným prvkem, ale integrální součástí analýzy. K formátování textových buněk se primárně používá jazyk **Markdown**, doplněný o prvky **HTML** a matematickou notaci **LaTeX**.

## 4.1 Základní syntaxe Markdown
Markdown je navržen tak, aby byl snadno čitelný i v neformátované (raw) podobě.

### Struktura a text

* **Nadpisy:** Definují se počtem znaků `#` na začátku řádku.  
  `# Nadpis 1`  
  `## Nadpis 2`  
  `### Nadpis 3`

* **Tučné písmo:** Text uzavřete do dvojitých hvězdiček `**text**`.  
  **Takhle vypadá tučný text.**

* **Kurzíva:** Text uzavřete do jednoduchých hvězdiček `*text*`.  
  *Takhle vypadá kurzíva.*

* **Seznamy:** Pro odrážky použijte `-` nebo `*`, pro číslovaný seznam `1.`.  
  * Položka A
  * Položka B

* **Citace:** Použijte znak `>` na začátku řádku.  
  > Toto je bloková citace, která je odsazená od zbytku textu.

* **Horizontální čára:** Použijte `---` na samostatném řádku.      
---

* **Emojis:** V mnoha editorech fungují kódy jako `:rocket` 🚀 nebo `:smile` 😄.

* **Zalomení řádku:** Pokud chcete začít na novém řádku bez vytvoření nového odstavce, vložte na konec řádku **dvě mezery**.

### Odkazy a obrázky
* **Odkaz:** `[Název odkazu](URL_adresa)`  
* **Obrázek:** `![Popis obrázku](URL_adresa_k_obrazku)`

### Tabulky

\| Jméno | Příjmení | Bydliště |<br>
\| :--- | :---: | ---: |<br>
\| Jan | Novák | Jilemnice |<br>
\| Jana | Nováková | Stará Paka |

| Jméno | Příjmení | Bydliště |
| :--- | :---: | ---: |
| Jan | Novák | Jilemnice |
| Jana | Nováková | Stará Paka |

### Práce s kódem a technický zápis

* **Inline kód:** Použijte zpětné apostrofy `` `Inline` ``.  
  Například název funkce `gpd.read_file()`.

* **Blok kódu (víceřádkový):** Pro celé bloky kódu použijte tři zpětné apostrofy nad a pod kódem. Za úvodní tři apostrofy napište název jazyka.
    
  \```python<br>
     print("Hello World")<br>
    \```  
   
  ```python
    print("Hello World")
    ```

## 4.2 Matematická notace (LaTeX)
Pro zápis matematických vzorců využívá Jupyter Notebook LaTeX. Abychom pochopili, jak rovnici zapsat, podívejte se na srovnání kódu a výsledku:

| Zdrojový kód (Markdown) | Výsledek (Render) |
| :--- | :--- |
| `$\text{NDVI} = \frac{\text{NIR} - \text{RED}}{\text{NIR} + \text{RED}}$` | $\text{NDVI} = \frac{\text{NIR} - \text{RED}}{\text{NIR} + \text{RED}}$ |
| `$d = \sqrt{x^2 + y^2}$` | $d = \sqrt{x^2 + y^2}$ |

---

## 4.3 Pokročilé formátování: HTML

Markdown je pro běžné psaní skvělý, ale má své limity (např. neumí měnit barvu textu nebo zarovnání). V Jupyteru můžete použít standardní **HTML tagy**.

### Vizuální úpravy textu a prvků

* **Změna barvy a velikosti:** Pomocí tagu `<span>` a parametru `style`:
    ```html
    <span style="color:red; font-size:30px;">Tento text je velký a červený.</span>
    ```

* **Zarovnání textu:** Markdown standardně zarovnává vše doleva. Pro středové zarovnání použijte:
    ```html
    <p style="text-align: center;">Tento text je vycentrovaný.</p>
    ```

* **Zvýraznění textu:** Když potřebujete na něco upozornit přímo uvnitř odstavce, tag `<mark>` funguje jako zvýrazňovač:
    ```html
    Tato část analýzy je <mark style="background-color: #ffff00">klíčová pro pochopení</mark> HTML
    ```

* **Barevné informační boxy:** K vytváření výrazných bloků se používá univerzální kontejner `<div>`:
    ```html
    <div style="background-color: #e3f2fd; border-left: 6px solid #2196f3; padding: 15px;"> Tady je modrý blok. </div>
    ```
 
* **Zalomení řádku:** V HTML se k odřádkování používá tag `<br>` (z anglického break):
    ```html
    První řádek textu. <br> Druhý řádek textu.
    ```
    > V čistém Markdownu dosáhnete stejného výsledku vložením **dvou mezer** na konec řádku.
    
* **Detaily a shrnutí**: Pro dlouhé analýzy nebo technické detaily, které by neměly hned zahltit čtenáře, je skvělý tag `<details>`. Funguje jako "rozbalovátko":
    ```html
    <details>
      <summary><b>Klikněte pro zobrazení zdrojového kódu nebo metodiky</b></summary>
      Zde může být schovaný dlouhý text, tabulka nebo další vysvětlení.
    </details>
    ```
    
---

# 5. Instalace balíčků a správa vývojového prostředí

Python je navržen jako modulární jazyk. Ve své základní distribuci neobsahuje nástroje pro pokročilou prostorovou analýzu, proto je nutné využít externí knihovny. Tato kapitola se věnuje metodice instalace knihovny **GeoPandas**.

## 5.1 Problém více interpretů

V operačním systému se běžně vyskytuje více verzí Pythonu současně (např. systémový Python, verze v rámci ArcGIS, distribuce Anaconda nebo různá virtuální prostředí).

### Rizika instalace přes Shell (`!`)
Častým přístupem k instalaci bývá využití příkazu **Shell Escape `!` **, např. `!pip install`. Tento příkaz dočasně opustí prostředí Pythonu a zavolá systémovou příkazovou řádku. 
* **V čem je problém:** Systémový terminál může mířit na úplně jiný **interpret** (jinou instalaci Pythonu), než který využívá váš aktuálně otevřený notebook. 
* **Následek:** Instalace v terminálu sice proběhne úspěšně, ale v notebooku se následně objeví chyba `ModuleNotFoundError`, protože knihovna byla nahrána "do jiného Pythonu".

### Proč v Jupyteru volit Line Magic (`%`)
Abychom tomuto chaosu předešli, používáme v buňkách notebooku tzv. **Line Magic** `%`. Na rozdíl od vykřičníku je příkaz `%pip install` interní funkcí jádra IPython, která je implementována tak, aby **automaticky identifikovala cestu k aktuálně spuštěnému kernelu**.

---

## 5.2 Metoda Conda (Preferovaný Environment Manager)

Pro práci s komplexními balíčky je **Conda** doporučeným standardem. Na rozdíl od běžných instalátorů funguje jako robustní **správce prostředí (Environment Manager)**.

**Proč volit Condu pro GeoPandas?**
Knihovna `GeoPandas` je závislá na nízkoúrovňových knihovnách psaných v C++ (zejména **GDAL**, **GEOS** a **PROJ**). Zatímco standardní `pip` vyžaduje jejich kompilaci v systému (což bývá zdrojem chyb), Conda je instaluje v již zkompilované a otestované binární podobě.

Vložte do buňky a spusťte:

```python
%conda install -c conda-forge geopandas
```

## 5.3 Alternativní metoda: Pip (Cloudová rozhraní)

V prostředích, která nativně nepodporují správu přes **Conda** (např. Google Colab), využíváme standardní nástroj `pip`.

Vložte do buňky a spusťte:
```python
%pip install geopandas 
```

---

## 5.4 Základní koncept knihovny GeoPandas

**GeoPandas** je open-source knihovna pro jazyk Python, která rozšiřuje datové struktury `Pandas` o podporu vektorových geoprostorových objektů. Umožňuje tak kombinovat klasickou tabulkovou analýzu s prostorovými operacemi.

### Koncepční datový model
GeoPandas definuje dvě základní třídy:

| Třída | Analogie | Význam v GIS |
| :--- | :--- | :--- |
| **GeoSeries** | Sloupec | Seznam prostorových objektů (body, linie, polygony) |
| **GeoDataFrame** | Tabulka | Atributová tabulka, kde má každý řádek svou geometrii |


### Technologický stack
Knihovna **Geopandas** funguje jako integrační rozhraní pro specializované nízkoúrovňové knihovny:

* **Shapely:** Výpočetní geometrie (využívá knihovnu **GEOS**).
* **PyProj:** Matematické transformace mezi souřadnicovými systémy (využívá knihovnu **PROJ**).
* **Fiona/PyOGRIO:** Čtení a zápis vektorových dat (využívá knihovnu **GDAL**)

---

## 5.5 První načtení a vizualizace prostorových dat

### 1. Příprava prostředí:
```python
# 1. Instalace nezbytných balíčků (pokud nejsou přítomny)
%conda install -c conda-forge geopandas

import geopandas as gpd
```

### 2. Načtení dat:
```python
# Načtení dat přímo z oficiálního zdroje Natural Earth
url = "https://naturalearth.s3.amazonaws.com/110m_cultural/ne_110m_admin_0_countries.zip"
world = gpd.read_file(url)

# Zobrazení souřadnicového systému
print(world.crs)

# Zobrazení atributové tabulky (prvních 5 záznamů)
world.head()
```

### 3. Globální vizualizace:
```python
# Vykreslení celého světa
world.plot()

# world[world.NAME == "Czechia"].plot()
```

### 4. Výpočet centroidu:
```python
# 1. Vyfiltrujeme jen Česko
czechia = world[world.NAME == "Czechia"].copy()

# 2. Vytvoříme nový sloupec s centroidem
# GeoPandas automaticky spočítá střed z polygonu hranic
czechia['centroid_bod'] = czechia.centroid

# Podívej se na tabulku - uvidíš dva sloupce s geometrií
czechia[['NAME', 'geometry', 'centroid_bod']].head()
```

### 5. Výpočet rozlohy:
```python
# Původní data Natural Earth jsou ve WGS84 (stupně).
# Ve stupních nelze správně počítat plochu → potřebujeme metry.

# 1. Převedení do souřadnicového systému S-JTSK
czechia_projected = czechia.to_crs(epsg=5514)

# 2. Výpočet plochy
# .area vrací výsledek v jednotkách systému (zde m2)
# Pro kilometry čtvereční musíme dělit 1 000 000
plocha_m2 = czechia_projected.area.item()
plocha_km2 = plocha_m2 / 1_000_000

# 3. Výpis výsledku
print(f"Aktuální souřadnicový systém: {czechia_projected.crs.name}")
print(f"Vypočítaná rozloha ČR: {plocha_km2:.2f} km²")
```
