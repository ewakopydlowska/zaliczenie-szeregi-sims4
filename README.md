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

Motywacja: rynek DLC w Sims 4 jest intensywny, kontrowwrsyjny i budzi dyskusje w społeczności. Chcę sprawdzić, czy na podstawie historii wydań da się przewidzieć tempo publikacji w kolejnych miesiącach.

Horyzont prognozy: do 36 miesięcy (3 lata), niżej powiem dlaczego.

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
Model: SARIMA (SSSezonowy ARIMA) dopasowany do miesięcznego szeregu y(t).

Podejście:
- podział na zbiór treningowy i testowy (walidacja na końcówce szeregu),
- baseline: „ostatnia obserwacja” jako punkt odniesienia,
- ocena jakości na teście: MAE i RMSE,
- prognoza na 36 miesięcy oraz przedziały ufności.

Plik z prognozą: `data/forecast_36_months_sarima.csv`.

5. Walidacja
Metryki:
- MAE (średni błąd bezwzględny)
- RMSE (pierwiastek z MSE)

Porównuję model SARIMA z baseline.

6. Wnioski
- Model lepiej przewiduje niż baseline (na zbiorze testowym).
- Przedziały ufności rosną wraz z horyzontem, co oznacza dużą niepewność przy prognozie 3-letniej
- Prognoza na 36 miesięcy jest „pokazowa” i służy spełnieniu wymagań zadania, ale realistycznie bardziej sensowny horyzont to krótszy (np. 6–12 miesięcy), bo tempo wydań zależy od decyzji biznesowych i planów wydawniczych, które mogą się zmienić.

7. Wnioski z wykonanych działan

Projekt analizuje liczbę premier DLC do The Sims 4 w ujęciu miesięcznym, to juz wiemy.

# Dane i częstość premier
- Zakres danych: 2014-09 do 2026-02 (138 miesięcy).
- Łącznie: 112 premier DLC w analizowanym okresie.
- Średnio historycznie: ok. *0,81 DLC/miesiąc* (112 / 138), czyli ok. *9,74 DLC/rok* (0,81 * 12).
- Rozkład miesięczny:
  - 0 premier: 59 miesięcy
  - 1 premiera: 54 miesiące
  - 2 premiery: 19 miesięcy
  - 3 premiery: 5 miesięcy
  - 5 premier: 1 miesiąc
Wniosek: dane są rzadkie (dużo zer) i nieregularne (czasem pojawiają się piki), co utrudnia dokładne przewidywanie na poziomie pojedynczych miesięcy, ale nie było mi to straszne.

# Zmiana tempa wydawania w czasie
Liczba premier rocznie (pełne lata):
- 2015: 8
- 2016: 7
- 2017: 7
- 2018: 5
- 2019: 6
- 2020: 5
- 2021: 12
- 2022: 11
- 2023: 12
- 2024: 13
- 2025: 21
(Uwaga: 2014 i 2026 są niepełnymi latami w zbiorze, bo gra wyszła w 2014 a teraz mamy początek 2026)

Średnie tempo (rocznie):
- 2015–2020: **~6,33 DLC/rok**
- 2021–2024: **~12,0 DLC/rok**
Różnica: po 2020 średnio jest **~+5,67 DLC/rok** więcej (ok. podwojenie).

Wzrost roczny (w prostych liczbach):
- od 2015 do 2025: z 8 do 21 DLC/rok, czyli **+13 DLC/rok** w 10 lat (średnio ok. **+1,3 DLC/rok**, każdy Simmer to odczuł - a twórcy na YT mieli dużo pracy!).
- tempo względne (CAGR, orientacyjnie): **~10% rocznie** (8 → 21 w latach 2015–2025)

Wniosek: po 2020 tempo wyraźnie rośnie ( niemalże podwojenie, niesamowite), a 2025 jest dodatkowo mocnym skokiem (21 premier). Przyczyna może być miks: zmiana strategii wydawniczej + większa liczba mniejszych formatów dodatków (np. kits) + przejęcie EA + rezygnacja z Projektu Renee (aka The Sims 5) i pełne skupienie na rozwoju The Sims 4.

# Prognoza na 36 miesięcy (03.2026 → 02.2029)
Zbudowałam model SARIMA i wykonanałam prognozę na 36 miesięcy.
- Suma w horyzoncie 36 miesięcy: **~67,12 premier DLC**
- Średnio w prognozie: **~1,86 DLC/miesiąc**, czyli ok. **~22,37 DLC/rok** (1,86 * 12).

Agregacja roczna prognozy (suma miesięcznych prognoz):
- 2026 (mar–gru): **~16,14**
- 2027 (pełny rok): **~21,78**
- 2028 (pełny rok): **~24,76**
- 2029 (sty–lut): **~4,44**

Wniosek: model sugeruje utrzymanie wysokiego tempa z 2025 oraz lekką tendencję wzrostową w 2027–2028.

# Dlaczego przedziały ufności są duże?
- Szereg jest zliczeniowy i nieregularny a do tego zawiera dużo zer oraz sporadyczne piki.
- W danych widać zmianę tempa wydawania po 2020 (możliwa zmiana strategii biznesowej, na pewno - pandemia + przejęcie EA), co zwiększa niepewność modeli opartych o historię.
- i jeszcze raz - im dalszy horyzont prognozy (36 miesięcy), tym naturalnie szerszy przedział niepewności.

Interpretacja praktyczna: prognoza ma największy sens jako przewidywanie **średniego tempa (miesięcznie/rocznie)**, a nie jako dokładne wskazanie, w którym konkretnie miesiącu wyjdzie dokładnie X dodatków. Ale to i tak ciekawe. 


***Jak uruchomić projekt, nie ma za co***
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python -m notebook

