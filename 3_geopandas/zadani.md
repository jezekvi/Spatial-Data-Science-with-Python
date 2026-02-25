# Praktické cvičení s GeoPandas

Nejdříve se podíváme na oficiální dokumentaci ke GeoPandas:  
[API reference GeoPandas](https://geopandas.org/en/latest/docs/reference.html)

A poté si společně vypracujeme úlohu č. 3 ze semestrální práce pro GIS 1.

K jejímu vypracování budeme potřebovat tato data:
- vrstvu vodních toků `Vodni_tok_HU.shp`
- vrstvu krajinného pokryvu `Land_cover.shp`

> *Pozn: vrstvy naleznete ve složce* `data`

## ÚLOHA 3: Výběr místa pro rezidenci

V řešeném území vyhledejte všechna vhodná místa pro stavbu sídla dle následujících podmínek: 
  - Přípustné typy prostředí jsou: Louky a pastviny; Směsice polí, luk a trvalých plodin; Sady, chmelnice a zahradní plantáže bez omezení rozlohy a Nezavlažovaná orná půda.

```
'2.3.1. Louky a pastviny', 
'2.4.2. Směsice polí, luk a trvalých plodin', 
'2.2.2. Sady, chmelnice a zahradní plantáže', 
'2.1.1. Nezavlažovaná orná půda'
```
    
  -  V okruhu do 1 km nesmí být žádný stálý obyvatel (stálí obyvatelé se zdržují v souvislé i nesouvislé městské zástavbě) 
```
'1.1.1. Souvislá městská zástavba',
'1.1.2. Nesouvislá městská zástavba'
```

  - Maximální vzdálenost od vodního toku jsou 3 km.

**Jak na to:** Pomocí nástrojů geoprocessingu vytvořte vrstvu míst splňující shora uvedené podmínky. Zjistěte celkovou rozlohu vhodných míst (ve smysluplných jednotkách).
