# 2. Verzování kódu a kolaborativní vývoj: Git a GitHub

V této lekci se zaměříme na správu **zdrojového kódu**. Ve Spatial Data Science je standardem pro správu verzí systém **Git**. Jeho zvládnutí je zásadní pro udržení pořádku v projektu, hladkou spolupráci s kolegy a jistotu, že vaše práce zůstane v bezpečí, dohledatelná a reprodukovatelná.

# Teoretický úvod do Version Control Systems (VCS)

Systémy pro správu verzí (VCS) sledují historii změn v souborech. Na rozdíl od běžného zálohování (např. Google Drive), které obvykle synchronizuje pouze aktuální stav, VCS uchovává kompletní historii vývoje projektu.

## 1.1 Motivace: Problém reprodukovatelnosti a správy verzí
Tradiční přístup k verzování pomocí přejmenovávání souborů (`analyza_final.ipynb`, `analyza_v2.ipynb`, `analyza_oprava.ipynb`) je nepraktický z několika důvodů:

* **Redundance:** Uchovávání mnoha kopií téměř identických souborů.
* **Absence kontextu:** Z názvu souboru není zřejmé, *co* přesně se změnilo a *proč*.
* **Kolize při spolupráci:** Pokud dva analytici upraví stejný soubor, dochází k přepsání dat nebo nutnosti manuálního slučování.

Git tyto problémy řeší.

## 1.2 Rozdíl mezi Git a GitHub
Je nutné rozlišovat mezi nástrojem a platformou:

| Nástroj | Definice | Funkce |
| :--- | :--- | :--- |
| **Git** | Distribuovaný systém správy verzí (software). | Běží lokálně na vašem počítači. Sleduje změny, vytváří historii, umožňuje větvení. Funguje offline. |
| **GitHub** | Cloudová hostingová služba pro repozitáře Git. | Slouží jako centrální online úložiště, nástroj pro Code Review a projektový management. |

> *Git funguje **zcela bez GitHubu**. GitHub je pouze vzdálený server.*

## 1.3 Tři oblasti Gitu
Abyste pochopili, jak Git funguje, musíte rozumět jeho workflow. Každý soubor se nachází v jedné ze tří oblastí:

1.  **Working Directory:** Aktuální soubory na vašem disku, které právě editujete.
2.  **Staging Area (Index):** „Přípravna“. Zde si skládáte balíček změn, které chcete uložit. Umožňuje vám to vybrat jen konkrétní úpravy.
3.  **Local Repository:** Adresář projektu, kde jsou změny trvale uloženy v historii (ve skryté složce `.git`).

## 1.4 Commit: Safe Point projektu

**Commit** je základní stavební jednotkou historie Gitu. Představuje trvalý a neměnný bod v čase, který zachycuje přesný stav projektu. Na rozdíl od běžných záloh Git nepracuje s kopírováním celých adresářů – **spravuje logickou historii vývoje, nikoliv fyzické kopie souborů.**

Klíčové vlastnosti commitu:

* **Commit Message:** Textový komentář, který dokumentuje, *proč* změna vznikla. Bez srozumitelného popisu je historie po čase nečitelná.
* **Autorství a čas:** Git ke každému zápisu automaticky přikládá jméno autora a časové razítko. Vždy je tedy dohledatelné, kdo a kdy konkrétní úpravu provedl.
* **Unikátní ID (Hash):** Každý commit má svůj specifický kód. Ten slouží jako jednoznačná adresa, díky které se k dané verzi můžete kdykoliv v budoucnu vrátit.
* **Návaznost:** Commity na sebe chronologicky navazují, čímž vzniká souvislý řetězec vývoje.

## 1.5 Branches (Větve)
Větvení (Branching) umožňuje paralelní vývoj. Díky němu můžeme izolovat práci na různých částech projektu, aniž by došlo k ohrožení stabilní verze kódu.

* **Main Branch:** Stabilní produkční verze projektu (zpravidla nazývaná `main` nebo `master`)
* **Feature Branch:** Větev vytvořená pro vývoj konkrétní funkcionality. Změny v této větvi neovlivňují hlavní kód, dokud nejsou explicitně sloučeny.
* **Merge:** Proces sloučení jedné větve do druhé. Git se pokouší automaticky sjednotit paralelní linie vývoje.
* **Conflict:** Stav, kdy Git identifikuje protichůdné úpravy na stejném řádku kódu, které nedokáže automaticky rozhodnout. Vyžaduje manuální zásah uživatele, který musí určit, která verze má být zachována.

## 1.6 Spolupráce a Remote Repository

Zatímco Git spravuje historii na vašem lokálním disku, pro spolupráci v týmu je nezbytný **Remote Repository**.

* **Remote Repository:** Kopie vašeho projektu umístěná na serveru. Slouží jako centrální uzel, se kterým všichni členové týmu synchronizují svou práci.
* **Push:** Odeslání vašich lokálních commitů na vzdálený server, aby byly dostupné ostatním.
* **Pull:** Stažení nejnovějších změn od kolegů ze serveru do vašeho počítače a jejich automatická integrace do vaší lokální historie.

**V následující kapitole se podíváme na platformu GitHub, která slouží jako centrální úložiště pro Remote repository.**

---

# 2. GitHub jako webová platforma pro řízení projektů

GitHub rozšiřuje funkčnost Gitu o nástroje pro týmovou spolupráci a řízení kvality kódu.

## 2.1 Issues
Sekce **Issues** slouží jako integrovaný systém pro hlášení chyb a plánování vývoje. V projektech se využívá pro:
* **Bug Tracking:** Hlášení chyb v algoritmech.
* **Feature Requests:** Návrhy na vylepšení.
> Issues podporují Markdown, lze vkládat kusy kódu, chybové hlášky i obrázky.

## 2.2 Pull Requests a Code Review
**Pull Request** je proces, kterým vývojář žádá o sloučení své větve (Feature Branch) do hlavní větve (Main Branch) před samotnou aplikací **Merge**.
Je to ideální moment pro **Code Review** – kolegové mohou kód zkontrolovat, okomentovat a navrhnout úpravy předtím, než se stane součástí **Main Branch**. 

---

# 3 Co do Gitu nepatří

Git byl navržen primárně pro správu **zdrojového kódu** (textových souborů), nikoliv jako obecné cloudové úložiště

## 3.1 Proč jsou některé soubory pro Git nevhodné?

* **Velké datasety:** Git je primárně optimalizován pro textové soubory, kde ukládá pouze řádkové rozdíly. U **binárních datasetů** (např. `.gpkg`, `.tiff` nebo `.shp`) však nedokáže identifikovat změny a při každém commitu ukládá kompletní soubor. To vede k neúměrnému nárůstu velikosti repozitáře.
    * *Správný postup:* Data ukládejte na externí úložiště (cloud, síťový disk) a v Gitu nechte pouze skript, který je zpracovává.

* **API klíče, hesla a tokeny:** Jakmile commitnete heslo do Gitu a odešlete (push) ho na GitHub, je kompromitované. I když ho v příštím kroku smažete, **zůstává v historii**.

* **Virtuální prostředí (`.venv`, `env`):** Tato složka obsahuje tisíce souborů specifických pro váš operační systém a konkrétní instalaci Pythonu. Kolegovi na jiném počítači by vaše prostředí nefungovalo.
    * *Správný postup:* Verzujte pouze konfigurační soubor, ze kterého si ostatní prostředí sami nastaví.

* **Dočasný balast a Cache:** Složky jako `__pycache__`, `.ipynb_checkpoints` nebo systémové soubory `.DS_Store` se mění při každém spuštění kódu. V historii verzí vytvářejí jen "šum", který zbytečně ztěžuje čtení skutečných změn v logice programu.

## 3.2 Soubor `.gitignore`: Automatický filtr

Abychom nemuseli pokaždé ručně hlídat, co do Gitu přidáváme, používáme konfigurační soubor **`.gitignore`**. Je to prostý textový dokument v kořenovém adresáři projektu, který Gitu říká: *"Tyto soubory a složky úplně ignoruj a nikdy je nenabízej k uložení."*

---

# 4. Instalace a konfigurace prostředí

Pro integraci Gitu je nutná instalace do operačního systému.

## 4.1 Instalace Git
[Oficiální stránka ke stažení (git-scm.com)](https://git-scm.com/downloads)

Stáhněte instalátor a ponechte výchozí nastavení. Následně v terminálu spusťte `git --version`.

## 4.2 Globální konfigurace (Identita)
Aby mohl Git přiřadit změny konkrétnímu autorovi, je nutné nastavit jméno a e-mail. Tyto údaje se propisují do historie commitů.

Otevřete Příkazový řádek (`Win` &rarr; `cmd` &rarr; `Enter`) a zadejte:

```bash
git config --global user.name "Vaše jméno"
git config --global user.email " Váš email"
```

## 4.3 GitHub CLI: Instalace a autorizace

Tradiční Git neví, že existuje nějaký GitHub, dokud mu ručně nepředáte přesnou adresu. Abychom se vyhnuli chybovým hláškám a složitému nastavování, použijeme oficiální nástroj **GitHub CLI** (příkaz `gh`). 

### Instalace nástroje přes příkazovou řádku
Nejrychlejší způsob, jak GitHub CLI nainstalovat bez hledání instalátorů na webu, je použít správce balíčků přímo v terminálu:

```bash
winget install --id GitHub.cli
```

### Autorizace
V terminálu zadejte příkaz:

```bash
gh auth login
```

Průvodce v terminálu projděte následovně (šipkami vybíráte, Enterem potvrzujete):
1. What account do you want to log into?  
   - Vyberte `GitHub.com`

2. What is your preferred protocol for Git operations?  
   - Vyberte `HTTPS`

3. Authenticate Git with your GitHub credentials?  
   - Zvolte `Yes` (Tento krok zajistí, že se přihlášení "propíše" i do Gitu).

4. How would you like to authenticate GitHub CLI?  
   - Vyberte `Login with a web browser`

5. Propojení s prohlížečem:  
   - Terminál vám ukáže osmimístný kód (např. BD21-4A55).  
   - Stiskněte Enter. Automaticky se otevře prohlížeč.  
   - Vložte zobrazený kód a potvrďte tlačítkem Authorize github.  

### Ověření stavu

```bash
gh auth status
```
Měli byste vidět potvrzení: `Logged in to github.com account <vaše-jméno>`.

---
# 5. Git v příkazové řádce: Praktické cvičení

V této části si vyzkoušíte základní workflow přímo v terminálu. Jednotlivé kroky naleznete v samostatném souboru `cviceni_git.md`

## Přehled základních příkazů

| Akce | Příkaz v terminálu | Popis a význam |
| :--- | :--- | :--- |
| **Initialize** | `git init` | Inicializuje nový lokální repozitář v aktuálním adresáři. |
| **Status** | `git status` | Zobrazí stav souborů (změněné, připravené k zápisu, nesledované). |
| **Diff** | `git diff` | Zobrazí konkrétní změny v řádcích u souborů |
| **Stage (+)** | `git add <soubor>` | Přesune konkrétní změny do oblasti připravených změn (**Staging Area**). |
| **Commit** | `git commit -m "zpráva"` | Vytvoří trvalý záznam v historii. |
| **Log** | `git log` | Zobrazí historii provedených commitů a jejich ID. |
| **Branch** | `git branch <název>` | Vytvoří novou vývojovou větev pro izolovanou práci. |
| **Checkout** | `git checkout <název>` | Přepne pracovní adresář do zvolené větve nebo ke konkrétnímu commitu. |
| **Merge** | `git merge <zdroj>` | Integruje změny ze zdrojové větve do té, ve které se právě nacházíte. |
| **Clone** | `git clone <url>` | Vytvoří lokální kopii existujícího vzdáleného repozitáře (typicky z GitHubu). |
| **Push** | `git push` | Propíše vaše lokální commity do vzdáleného repozitáře v cloudu. |
| **Pull** | `git pull` | Stáhne nejnovější změny z cloudu a automaticky je sloučí s vaší lokální verzí. |

---

# 6. Workflow ve VS Code

Visual Studio Code má integrovanou podporu pro Git (záložka **Source Control**). Níže je popsán standardní postup.

## 6.1 Inicializace repozitáře
1.  Otevřete složku projektu ve VS Code.
2.  Přejděte na panel **Source Control** (ikona větvení vlevo, zkratka `Ctrl+Shift+G`).
3.  Zvolte **Initialize Repository**.

## 6.2 Staging a Commit (Uložení změn)
Git rozlišuje mezi "pracovním adresářem" a "historií". Uložení je dvoufázový proces:

1.  **Stage Changes (Příprava):** Vyberte soubory, které chcete zahrnout do commitu, kliknutím na ikonu `+` u názvu souboru.
2.  **Commit (Potvrzení):** Do pole *Message* zadejte popis změny. Potvrďte tlačítkem **Commit** (ikona ✔️).

## 6.3 Práce s větvemi (Branching)
1.  V dolní liště VS Code klikněte na název aktuální větve (obvykle `main` nebo `master`).
2.  Zvolte **+ Create new branch...**.
3.  Zadejte název.
4.  VS Code vás automaticky přepne do nové větve.

## 6.4 Slučování větví (Merge)
Když je práce ve vývojové větvi hotová, je třeba ji včlenit do hlavní linie projektu:

1. **Přepnutí:** Klikněte na název větve v dolní liště a přepněte se zpět do cílové větve (např. `main`).
2. **Výběr příkazu:** Otevřete příkazovou paletu (`Ctrl+Shift+P`), napište `Git: Merge Branch...` a stiskněte Enter.
3. **Zdroj:** Vyberte větev, kterou chcete sloučit (např. vaši rozpracovanou funkci).
4. **Dokončení:** Git se pokusí o automatické sloučení. Pokud narazí na konflikty, VS Code je barevně zvýrazní přímo v editoru, kde můžete zvolit možnost *Accept Current Change* (ponechat verzi z `main`) nebo *Accept Incoming Change* (přijmout verzi z vaší větve).

## 6.5 Publikace na GitHub (Push)
Pro zálohování kódu do cloudu:

1.  V panelu **Source Control** klikněte na **Publish Branch**.
2.  Při prvním použití budete vyzváni k autorizaci přes prohlížeč.
3.  Zvolte možnost **Publish to GitHub public repository** (nebo private).

---

# 7. Specifika práce v cloudu: Google Colab a GitHub

Zatímco ve VS Code pracujeme s lokálním klonem repozitáře, Google Colab běží na dočasném virtuálním stroji. Jakékoli soubory stažené nebo vytvořené mimo připojený Google Drive jsou po ukončení relace smazány. Z tohoto důvodu vyžaduje verzování odlišný přístup.

## 7.1 Nativní integrace (GUI přístup)
Google Colab disponuje vestavěnou funkcí pro přímé ukládání notebooků na GitHub bez nutnosti používat příkazovou řádku.

### Uložení práce na GitHub (Push)
V prostředí Colab vyžaduje standardní příkaz `git push` konfiguraci autentizace při každém spuštění (kvůli dočasnosti instance). Místo toho můžeme využívat nativní API integraci, která je jednodušší:

1.  V horním menu zvolte **Soubor (File)** > **Uložit kopii na GitHub (Save a copy in GitHub)**.
2.  Při prvním použití budete vyzváni k autorizaci propojení Google Colab s vaším GitHub účtem.
3.  V dialogovém okně nastavte:
    * **Repository:** Vyberte cílový repozitář.
    * **Branch:** Zvolte větev.
    * **File path:** Nastavte cestu k souboru.
    * **Commit message:** Popište změny.
4.  Klikněte na **OK**. Notebook se uloží jako nový commit a automaticky se otevře v náhledu na GitHubu.

### Otevření notebooku z GitHubu
Pro načtení existující práce:
1.  V menu zvolte **Soubor (File)** > **Otevřít notebook (Open notebook)**.
2.  Přepněte na záložku **GitHub**.
3.  Vyhledejte repozitář nebo vložte URL adresu.
4.  Kliknutím na ikonu "Open in Colab" (často odznak v `README.md`) nebo výběrem ze seznamu se spustí nová relace.

## 7.2 Správa dat v Colabu
Zásadní rozdíl oproti lokálnímu vývoji je v persistenci dat. V prostředí Colab se data po ukončení session mažou.

* **Kód (Notebook):** Ukládáme na **GitHub** (viz bod 7.1).
* **Velká data (Rastr/Shapefile):** Ukládáme na **Google Drive**.

Pro přístup k datům na Disku je nutné je připojit (Mount):

```python
from google.colab import drive
drive.mount('/content/drive')
```

## 7.3 Práce s větvemi (Branches) v Google Colab

Google Colab nemá plnohodnotné grafické rozhraní pro správu Git repozitáře (jako panel *Source Control* ve VS Code). Práce s větvemi proto probíhá specifickým způsobem.

### Doporučený postup
Jelikož Colab neumožňuje snadné řešení konfliktů (Merge Conflicts), doporučuji tento postup:

1.  Vytvořte si větev na webu GitHubu (tlačítko *Branch* -> *New branch*).
2.  Otevřete tuto větev v Colabu (postup A).
3.  Pracujte a ukládejte přímo do této větve (postup B).
4.  Pokud potřebujete sloučit změny do `main`, vytvořte **Pull Request** na webu GitHubu.

### A. Otevření konkrétní větve
Pokud chcete pracovat na jiné větvi než `main` (např. chcete opravit kód ve větvi `development`):

1.  Jděte na **Soubor (File)** > **Otevřít notebook (Open notebook)**.
2.  Vyberte záložku **GitHub**.
3.  Zadejte URL repozitáře nebo vyhledejte název.
4.  Vedle názvu repozitáře je rozbalovací nabídka **Branch**, kde standardně svítí `main`. **Zde přepněte na požadovanou větev.**
5.  Zobrazí se seznam notebooků dostupných v této konkrétní větvi.

### B. Uložení do nové nebo existující větve
Když máte hotovou práci a chcete ji uložit, Colab vám umožní vybrat cílovou větev v dialogovém okně.

1.  Zvolte **Soubor (File)** > **Uložit kopii na GitHub (Save a copy in GitHub)**.
2.  V okně, které se otevře, se zaměřte na pole **Branch** a vyberte **požadovanou větev**.
3.  Zadejte *Commit message* a potvrďte.
