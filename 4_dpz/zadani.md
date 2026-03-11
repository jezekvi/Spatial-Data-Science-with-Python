# Zpracování družicových dat Sentinel-2

V této lekci si ukážeme jak pracovat s družicovými daty mise **Sentinel-2**. Zaměříme se na to, jak zpracovávat multispektrální rastry pomocí knihoven `xarray` a `rioxarray`.

---

## 1. Princip práce s rastry v xarray

Družcové snímky neinterpretujeme jako prostá obrazová data, nýbrž jako **vícerozměrná pole** (N-dimensional, ND).

* Pracujeme s objektem `xarray.DataArray`. Ten kromě samotných hodnot pixelů nese i metadata o souřadnicovém systému, prostorovém rozlišení, jednotkách atd.
* Typický rastr je v `xarray` reprezentován třemi dimenzemi: `(band, y, x)`.
* `rioxarray`: Geosprostorové rozšíření pro `xarray`. Přidává schopnost rozumět souřadnicovým systémům, provádět reprojekce, ořezy, exportovat výsledky do standardních GIS formátů atd.

### Odkazy k dokumentaci:
* [Dokumentace xarray](https://docs.xarray.dev/en/stable/)
* [Dokumentace rioxarray](https://corteva.github.io/rioxarray/stable/)

---

## 2. Zadání cvičení

Vaším úkolem je zpracovat data úrovně **L2A**  ze složky `s2_data` a provést základní analýzu vegetace.

### Postup práce:

1.  Nainstalujte potřebné knihovny
2.  Načtěte jednotlivá pásma
3.  Prohlédněte si metadata načtených pásem
4.  Definujte oblast zájmu a proveďte ořez
5.  **Kalibrace:** Převeďte hodnoty DN na odrazivost
6.  Proveďte vizualizaci ve **4-3-2 True Color** a **8-4-3 False Color**
7.  Vyberte několik reprezentativních pixelů pro různé povrchy a vykreslete spektrální křivky pro tyto pixely napříč dostupnými pásmy.
8.  Maskujte oblačnost pomocí pásma **SCL**
9.  Vypočítejte NDVI
10.  Na základě vypočteného NDVI vytvořte masku vegetace (NDVI > 0.7)
11.  Masku vegetace exportujte do GeoTIFF
