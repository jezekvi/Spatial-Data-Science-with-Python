# Praktické cvičení: Git v příkazové řádce

V tomto cvičení si projdeme základy lokálního vývoje. Otevřete si příkazovou řádku a postupujte podle následujících kroků:

---

## Fáze 1: Založení projektu a první Commit

1. **Vytvořte novou složku a vstupte do ní:**
   ```bash
   mkdir projekt_analyza
   cd projekt_analyza
   ```

2. **Inicializujte Git repozitář:**
   ```bash
   git init
   ```

3. **Vytvořte soubor:**
   ```bash
   echo "Uvodni text analyzy uzemi" > analyza.txt
   ```

4. **Zkontrolujte stav:**
   ```bash
   git status
   ```
   
5. **Stage a Commit:**
   ```bash
   git add analyza.txt
   git commit -m "Prvni verze analyzy"
   ```

---

## Fáze 2: Práce s historií a změnami

1. **Upravte soubor:**
   ```bash
   echo "Pridavam data o poctu obyvatel" >> analyza.txt
   ```

2. **Podívejte se na rozdíly**
   ```bash
   git diff
   ```

3. **Uložte druhou verzi:**
   ```bash
   git add .
   git commit -m "Doplneni dat o obyvatelstvu"
   ```

4. **Prohlédněte si historii:**
   ```bash
   git log
   ```

---

## Fáze 3: Branching

1. **Vytvořte novou větev:**
   ```bash
   git branch experiment
   ```
   
2. **Přepněte se do ní:**
   ```bash
   git checkout experiment
   ```

3. **Udělejte změnu ve větvi `experiment`:**
   ```bash
   echo "Vypocet noveho indexu" > vypocet.txt
   git add vypocet.txt
   git commit -m "Pridani experimentalniho vypoctu"
   ```

4. **Návrat do hlavní větve:**
   ```bash
   git checkout master
   ```

---

## Fáze 4: Merge

1. **Sloučení větví:**
   ```bash
   git merge experiment
   ```

2. **Smazání nepotřebné větve:**
   ```bash
   git branch -d experiment
   ```

---

## Fáze 5: Clone

1. **Vystupte ze své složky o úroveň výš:**
   ```bash
   cd ..
   ```

2. **Klonování repozitáře:**
   ```bash
   git clone https://github.com/jezekvi/Spatial-Data-Science-with-Python.git
   ```

3. **Prozkoumejte stažený projekt:**
   ```bash
   cd Spatial-Data-Science-with-Python
   git log
   ```
