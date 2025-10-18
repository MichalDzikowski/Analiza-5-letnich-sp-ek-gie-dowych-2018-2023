# Analiza-5-letnich-spóek-giełdowych-2018-2023
Projekt ten stanowi kompleksową analizę 5-letniego zbioru danych giełdowych (2018-2023) dla 491 spółek, pobranego z platformy Kaggle. Analiza skupia się na identyfikacji trendów, ocenie ryzyka i zwrotu oraz zrozumieniu dynamiki rynkowej.

Źródło: https://www.kaggle.com/datasets/iveeaten3223times/massive-yahoo-finance-dataset
# 1. Cel Projektu

Głównym celem projektu było przeprowadzenie wielowymiarowej analizy historycznych danych giełdowych w celu:
* **Identyfikacji trendów wzrostu** i wyłonienia liderów rynkowych w badanym okresie.
* **Kwantyfikacji ryzyka** (mierzonego jako zmienność) oraz **zwrotu** (mierzonego jako całkowita stopa zwrotu).
* **Zbadania relacji między ryzykiem a zwrotem** (Risk-Return Trade-Off) dla całego rynku.
* **Analizy dynamiki sektorowej**, ze szczególnym uwzględnieniem płynności (wolumenu) oraz poziomu korelacji między głównymi spółkami technologicznymi.


## 2. Proces Analityczny

Analiza została przeprowadzona krok po kroku, obejmując następujące etapy:

1.  **Wczytanie i Czyszczenie Danych (ETL):**
    * Załadowanie pliku `stock_details_5_years.csv` do ramki danych `pandas`.
    * Inspekcja danych (`.info()`, `.head()`) w celu identyfikacji typów danych i braków.
    * Kluczowy etap czyszczenia: konwersja kolumny `Date` z typu `object` na poprawny format `datetime64`.

2.  **Analiza Wzrostu i Zwrotu:**
    * **Normalizacja Wzrostu:** Stworzenie znormalizowanego wykresu wzrostu dla 5 spółek "Big Tech" (`AAPL`, `MSFT`, `GOOGL`, `AMZN`, `NVDA`), aby porównać ich wyniki od wspólnego punktu startowego.
    * **Obliczenie Całkowitego Zwrotu:** Obliczenie procentowej stopy zwrotu dla wszystkich 491 spółek (od pierwszej do ostatniej ceny zamknięcia). Zastosowano filtr, aby uwzględnić tylko spółki z co najmniej 3-letnią historią danych (750+ dni).

3.  **Analiza Ryzyka:**
    * Obliczenie **dziennych zwrotów procentowych** (`.pct_change()`) dla każdej spółki.
    * Obliczenie **rocznej zmienności** (roczne odchylenie standardowe dziennych zwrotów) jako miernika ryzyka.

4.  **Analiza Łączona (Ryzyko vs. Zwrot):**
    * Połączenie wyników z Kroków 2 i 3 w jedną ramkę danych, aby uzyskać parę (Ryzyko, Zwrot) dla każdej spółki.
    * Wizualizacja tej relacji na wykresie punktowym (scatter plot).

5.  **Analiza Rynkowa:**
    * **Płynność:** Obliczenie średniego dziennego wolumenu obrotu w celu identyfikacji najbardziej płynnych/handlowanych aktywów.
    * **Korelacja:** Stworzenie tabeli przestawnej (pivot) dziennych zwrotów dla spółek "Big Tech", a następnie obliczenie i wizualizacja macierzy korelacji za pomocą mapy ciepła (heatmap).
  

## 3. Wnioski Analityczne

### Wniosek 1: Dominacja Sektora AI
Analiza znormalizowanego wzrostu wykazała, że o ile cały sektor technologiczny rósł, **Nvidia (NVDA)** zdeklasowała konkurencję. Jej wykres odrywa się od reszty grupy, co jest bezpośrednim wskaźnikiem rynkowego entuzjazmu dla rewolucji AI i kluczowej roli NVDA jako dostawcy infrastruktury.

<img width="929" height="514" alt="comparative_growth_chart" src="https://github.com/user-attachments/assets/e2a5b24b-e45e-430f-bc37-10873e479ad0" />


### Wniosek 2: Zrozumienie Relacji Ryzyko-Zwrot
Wykres punktowy ryzyka vs. zwrotu doskonale ilustruje kompromis rynkowy.
* **Prawy górny róg:** Potwierdza, że najwyższe zwroty (jak `NVDA`) były obarczone bardzo wysokim ryzykiem (zmiennością).
* **Lewy dolny róg:** Identyfikuje "bezpieczne przystanie" – spółki o niskiej zmienności i niskim, ale stabilnym zwrocie (np. `JNJ`, `PG`).
* **Prawy dolny róg:** Pokazuje "pułapki" – aktywa o wysokim ryzyku, które nie dostarczyły zwrotu, generując straty.

<img width="371" height="364" alt="risk_vs_return_scatter" src="https://github.com/user-attachments/assets/4c6c0a5d-5e11-44cc-9d3c-ab6398dc9c4a" />


### Wniosek 3: Ukryte Ryzyko Korelacji Sektorowej
Najważniejszy wniosek dla zarządzania portfelem: spółki "Big Tech", choć różne, poruszają się niemal w idealnym tandemie.

<img width="837" height="786" alt="correlation_heatmap" src="https://github.com/user-attachments/assets/abbb27fb-0b8b-41c4-aa71-2d5213c5f4df" />


Mapa ciepła pokazuje **wysoką korelację dodatnią** (od $0.61$ do $0.76$) między wszystkimi gigantami.
* **Implikacja:** Portfel składający się wyłącznie z `AAPL`, `MSFT`, `GOOGL`, `AMZN` i `NVDA` **nie jest zdywersyfikowany**. Jest to w rzeczywistości jedna, silnie skoncentrowana pozycja na cały sektor technologiczny.
* **Najsilniejsza para:** `MSFT` i `GOOGL` ($0.76$) są niemal substytutami z punktu widzenia ruchów cenowych, co odzwierciedla ich bezpośrednią konkurencję w chmurze i oprogramowaniu.

## 4. Wykorzystane Technologie

* **Język:** Python 3.10+
* **Środowisko:** Jupyter Notebook
* **Analiza Danych:**
    * **Pandas:** Kluczowa biblioteka do wczytywania (CSV), czyszczenia, transformacji (`groupby`, `merge`, `pivot_table`) i agregacji danych.
    * **Numpy:** Wykorzystany do obliczeń numerycznych (np. `np.sqrt` przy obliczaniu rocznej zmienności).
* **Wizualizacja Danych:**
    * **Matplotlib:** Użyty do szybkich, statycznych wizualizacji (np. wykres liniowy ceny `AAPL`).
    * **Altair:** Główna biblioteka wizualizacyjna projektu, użyta do stworzenia interaktywnych i złożonych wykresów (wykresy słupkowe, wykres punktowy, mapa ciepła).
 
