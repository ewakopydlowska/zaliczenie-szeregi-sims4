# Zaliczenie – Analiza szeregów czasowych: The Sims 4 DLC
Uwaga: to prognoza statystyczna na podstawie historii — nie przeciek planów EA, proszę mnie nie pozywać, ja chcę tylko zdać semestr
Temat: Prognozowanie intensywności publikacji dodatków (DLC) do The Sims 4 w ujęciu miesięcznym (2014–2026)

Dane to lista dodatków i dat premier, zebrana z publicznych źródeł społecznościowych.
Zakres: 2014-09 do 2026-02.

Cel analizy:
1) Zbudowanie miesięcznego szeregu czasowego liczby premier DLC
2) Zbudowanie modelu prognozy i ocena na zbiorze testowym
3) Opisanie obserwacji (trend, zmiany strategii wydawniczej EA)

Repo zawiera notebook Jupyter z analizą, wykresami, modelowaniem oraz moimi wnioskami.


1. Definicja problemu
Celem projektu jest analiza i prognoza liczby dodatków (DLC/pakietów) wydawanych do gry The Sims (głównie Sims 4) w ujęciu miesięcznym. Zmienną prognozowaną jest:
- y(t) = liczba DLC wydanych w danym miesiącu.

Motywacja: rynek DLC w Sims 4 jest intensywny i budzi dyskusje w społeczności. Chcę sprawdzić, czy na podstawie historii wydań da się przewidzieć tempo publikacji w kolejnych miesiącach.

Horyzont prognozy: do 36 miesięcy (3 lata), z komentarzem o sensowności tak długiej prognozy.

2. Dane i źródło
Dane: zestawienie DLC i pakietów z datami wydania i metadanymi.
Plik wejściowy: `data/sims4_dlc.xlsx` (zaktualizowany do 02.2026).

Przygotowanie szeregu czasowego:
- konwersja „Release Date” do daty,
- agregacja do miesięcy (MS),
- zbudowanie pełnej osi czasu i uzupełnienie braków zerami.

3. Eksploracja danych (EDA)
Sprawdzam:
- rozkład liczby wydań na miesiąc,
- trend w czasie,
- sezonowość (czy są miesiące z większą liczbą premier).

Wykresy zapisane w `figures/`.

4. Model i prognoza
Model: SARIMA (sezonowy ARIMA) dopasowany do miesięcznego szeregu y(t).

Podejście:
- podział na zbiór treningowy i testowy (walidacja na końcówce szeregu),
- baseline: „ostatnia obserwacja” jako punkt odniesienia,
- ocena jakości na teście: MAE i RMSE,
- prognoza na 36 miesięcy oraz przedziały ufności.

Plik z prognozą: `data/forecast_36_months_sarima.csv`.

5. Walidacja
Metryki:
- MAE (średni błąd bezwzględny),
- RMSE (pierwiastek z MSE).

Porównuję model SARIMA z baseline.

6. Wnioski
- Model lepiej przewiduje niż baseline (na zbiorze testowym).
- Przedziały ufności rosną wraz z horyzontem, co oznacza dużą niepewność przy prognozie 3-letniej.
- Prognoza na 36 miesięcy jest „pokazowa” i służy spełnieniu wymagań zadania, ale realistycznie bardziej sensowny horyzont to krótszy (np. 6–12 miesięcy), bo tempo wydań zależy od decyzji biznesowych i planów wydawniczych, które mogą się zmienić.



***Jak uruchomić projekt
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m notebook
