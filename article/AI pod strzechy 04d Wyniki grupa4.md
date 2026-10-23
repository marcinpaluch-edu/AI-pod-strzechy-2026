# Część 4d. Wyniki — grupa 4: powyżej 14 GB, stanowisko hostowe

Mierzone na Ollamie zainstalowanej natywnie w Windows, przy zatrzymanym Podmanie. Do tej grupy należy też `gpt-oss:20b` (13,79 GB), który z marginesem pamięci nie mieści się pod limitem WSL. Dwa ostatnie modele, `qwen3.8:27b` i `laguna-xs-2.1`, dołożyłem po pierwszym przebiegu badania.

Wszystkie modele na ustawieniach aplikacji (domyślnie `think: false`, 180 tokenów, temperatura 0,2, okno domyślne Ollamy; `gpt-oss:20b` z `think: "low"`). Jedenaście scenariuszy w dwóch zestawach (rozdział 3.1.1), każdy model w czterech przebiegach od czystej pamięci, czyli 44 notatki po polsku. Kolejność wierszy to ranking końcowy grupy według kryteriów z rozdziału 3.7: najpierw liczba scenariuszy z notatkami konsumowalnymi, potem polszczyzna bez wad, przy remisie ustawienia domyślne przed wpisem w konfiguracji i krótszy zimny start.

## Zestawienie modeli

| Miejsce | Model | Test z rozdz. 3 PL | A/B PL | Zestaw 2 | Test z rozdz. 3 EN | A/B EN | 11 scenariuszy: miary / konsumowalne | Główny problem PL | Główny problem EN | Zimny start (PL) | Rozmiar | Licencja | Dodany |
|---:|---|:---:|:---:|:---:|:---:|:---:|:---:|---|---|---:|---:|---|---|
| 1 | **`gemma4:26b`** | ✓ | ✓ | ✓ | ✓ | ✓ | 11 / **11** | **bez wad** | **bez wad** | 34,3 s | 18,73 GB | Apache 2.0 | 2026-03-11 |
| 2 | **`qwen3.8:27b`** | ✓ | ✓ | ✓ | ✓ | ✓ | 11 / **11** | **bez wad**; raz „Nie znana jest przyczyna” | **bez wad** | 61,0 s | 17,74 GB | Apache 2.0 | 2026-08-05 |
| 3 | **`granite4:32b-a9b-h`** | ✓ | ✓ | ✓ | ✓ | ✓ | 11 / **11** | kursy pozostałych spółek o 17:00, kropka | **bez wad** | 46,7 s | 19,48 GB | Apache 2.0 | 2025-09-16 |
| 4 | `gemma4:31b` | ✗ | ✓ | ✓ | ✗ | ✓ | 11 / 10 | **bez wad**; raz „2,3%” — jedno miejsce zamiast dwóch | „2.3%” — jedno miejsce zamiast dwóch | 67,7 s | 20,39 GB | Apache 2.0 | 2026-03-11 |
| 5 | `qwen3.6:27b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 10 | **bez wad** w teście z rozdz. 3; w zestawie 2 raz przy czterech spółkach bez procentów | formułka „This is information, not investment advice.”, wypełniacz | 73,5 s | 17,77 GB | Apache 2.0 | 2026-04-21 |
| 6 | `granite4.1:30b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 10 | kropka; w zestawie 2 przy czterech spółkach bez procentów i zmyślone „KGHM +2.5%” | **bez wad** | 68,4 s | 17,49 GB | Apache 2.0 | 2026-04-06 |
| 7 | `gpt-oss:20b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 10 | spacja „2,30 %”; w zestawie 2 raz pusta notatka przy czterech spółkach; wymaga wpisu `low` w konfiguracji | **bez wad** | 24,4 s | 13,79 GB | Apache 2.0 | 2025-08-04 |
| 8 | `granite4.2:30b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 9 | kursy wszystkich spółek, kropka; w zestawie 2 obcina „5,33%” | **bez wad** | 110,3 s | 17,72 GB | Apache 2.0 | 2026-08-07 |
| 9 | `laguna-xs-2.1` | ✗ | ✓ | ✗ | ✓ | ✓ | 11 / 7 | rozwlekła, urywa na limicie; „Aktualne poziomy cen pozostają bez zmian”, „Dane z dnia dzisiejszego”; błędy gramatyczne; spacja „2,30 %” | wypełniacze („Current prices are provided for reference only”) | 38,7 s | 20,27 GB | OpenMDW-1.1 | 2026-06-20 |
| 10 | `mistral-small3.2` | ✗ | ✗ | ✗ | ✓ | ✓ | 6 / 6 | „LPP 244,00 PLN” zamiast 24 400 — bez polecenia zaokrąglania już nie | **bez wad** | 78,0 s | 15,18 GB | Apache 2.0 | 2025-06-19 |
| 11 | `exaone3.5:32b` | ✗ | ✗ | ✗ | ✓ | ✗ | 8 / 2 | „wzrosła o 2,30 PLN” zamiast procentu, zmyślony kurs Grupy Kęty | kursy bez ruchu wbrew zakazowi, kursy XTB i PKN Orlen o 17:00 | 138,5 s | 19,34 GB | EXAONE 1.1 – NC (niekomercyjna) | 2024-12-01 |

Testy: ✓ — wszystkie notatki testu konsumowalne (rozdział 3.5.1) i bez błędu treści przy lekturze, we wszystkich czterech przebiegach. A/B to ten sam test bez zdania o zaokrąglaniu. Miary to automat (rozdział 3.4), konsumowalne — notatki z prawdziwymi i kompletnymi danymi mimo usterek formy (rozdział 3.5.1). Pogrubienie: komplet jedenastu scenariuszy z notatkami konsumowalnymi.

## Wyniki per scenariusz

| Scenariusz | Zestaw | Start | Bez błędu w miarach | Konsumowalnych | Najszybszy konsumowalny |
|---|---|---|---:|---:|---|
| 23.09 09:00 — bez ruchu | 1 | **zimny** | 11/11 | 10/11 | `gpt-oss:20b` 24,4 s |
| 23.09 11:00 — bez ruchu | 1 | ciepły | 11/11 | 10/11 | `gpt-oss:20b` 9,5 s |
| 23.09 13:00 — bez ruchu | 1 | ciepły | 11/11 | 10/11 | `gpt-oss:20b` 11,2 s |
| 23.09 15:00 — bez ruchu | 1 | ciepły | 10/11 | 10/11 | `gpt-oss:20b` 9,2 s |
| 23.09 17:00 — Modivo +2,30% ▲ | 1 | ciepły | 9/11 | 7/11 | `gpt-oss:20b` 14,2 s |
| 18.09 15:00 — Grupa Polsat −2,63% ▼ | 2 | **zimny** | 10/11 | 8/11 | `gpt-oss:20b` 33,3 s |
| 18.09 23:27 — Grupa Polsat −5,34% ▼ | 2 | ciepły | 9/11 | 8/11 | `gpt-oss:20b` 13,6 s |
| 21.09 11:00 — cztery spółki ▲ | 2 | ciepły | 7/11 | 6/11 | `gemma4:26b` 23,5 s |
| 19.10 09:00 — bez ruchu | 2 | ciepły | 10/11 | 9/11 | `gpt-oss:20b` 11,5 s |
| 19.10 11:00 — bez ruchu, KGHM −1,97% | 2 | ciepły | 11/11 | 10/11 | `gpt-oss:20b` 9,1 s |
| 19.10 13:00 — XTB −2,12% ▼ | 2 | ciepły | 10/11 | 9/11 | `gpt-oss:20b` 13,0 s |

Czas to średnia z czterech przebiegów. Ostatnia kolumna dotyczy tylko danego scenariusza — w cyklach bez ruchu najszybciej piszą poprawnie także modele, które przy ruchu zmyślają.

## Test narzędzi

Wywołania to sześć prób: dwa pytania po trzy razy. Pętla to trzy próby pełnego obiegu — wywołanie, wykonanie, odpowiedź z liczbą 24 160 zł. Czasy są średnimi z prób; pierwsza próba każdego modelu startowała na zimno. Pogrubienie oznacza komplet. Opis testu w rozdziale 4.6.

| Model | Wywołania (z 6) | Pętla (z 3) | Czas wywołania | Czas pętli | Uwagi |
|---|:---:|:---:|---:|---:|---|
| `gpt-oss:20b` | **6** | **3** | 6,2 s | 14,6 s | — |
| `granite4:32b-a9b-h` | **6** | **3** | 10,9 s | 25,8 s | — |
| `gemma4:26b` | **6** | **3** | 11,8 s | 28,1 s | — |
| `mistral-small3.2` | **6** | **3** | 13,9 s | 46,4 s | — |
| `granite4.1:30b` | **6** | **3** | 15,5 s | 49,1 s | — |
| `qwen3.8:27b` | **6** | **3** | 26,2 s | 51,1 s | — |
| `gemma4:31b` | **6** | **3** | 29,9 s | 68,3 s | — |
| `qwen3.6:27b` | **6** | **3** | 63,8 s | 124,8 s | — |
| `granite4.2:30b` | **6** | **3** | 57,8 s | 170,9 s | — |
| `laguna-xs-2.1` | 5 | 0 | 10,1 s | 17,8 s | raz o listę spółek odpowiada bez wywołania; w pętli kurs 24 160 podaje jako „24,16 PLN” (3/3) |
| `exaone3.5:32b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |

## Komentarz

**Grupa czwarta ma lidera zdecydowanego: `gemma4:26b`.** Przeszedł bez uwag wszystko, co sprawdzałem: jedenaście scenariuszy, test bez polecenia zaokrąglania, prompt po angielsku i narzędzia agenta. Spadki zapisuje z przecinkiem i poprawnie zaokrągla („Grupa Polsat zmieniła kurs o -5,34%”), przy czterech spółkach naraz nie gubi żadnej, a bez ruchu za każdym razem pisze „Nie było znaczących zmian na liście spółek.” Jako model MoE używa przy każdym tokenie tylko około 4 z 25 mld parametrów, więc jest najszybszy w grupie spośród modeli na ustawieniach domyślnych.

**Drugi jest `qwen3.8:27b` — i to najlepszy w badaniu przykład, że aktualizacja modelu ma sens.** To ten sam model bazowy co `qwen3.6:27b` (rodzina `qwen35`, 27,3 mld parametrów), tylko w nowszym wydaniu, i naprawia jedyny błąd poprzednika: przy czterech spółkach naraz za każdym razem podaje wszystkie cztery procenty. Przechodzi wszystkie jedenaście scenariuszy, pisze po polsku bez wad i ma komplet w teście narzędzi — a do tego startuje szybciej (61 s wobec 73,5 s) i domyka pętlę narzędziową w 51 s zamiast 125 s. Kto ma `qwen3.6:27b`, zyskuje na podmianie w każdym mierzonym wymiarze.

Trzeci jest `granite4:32b-a9b-h`, też z kompletem scenariuszy, ale z wadami formy: o 17:00 wypisuje kursy pozostałych spółek z kropką dziesiętną. Laguna od Poolside, model MoE z około 3 mld aktywnych parametrów, jest w grupie najszybsza po Gemmie (38,7 s zimnego startu, 15,7 s ciepłej notatki) i liczby podaje bezbłędnie, ale pisze rozwlekle: w czterech notatkach wyczerpuje limit długości i traci przez to cztery scenariusze, dopisuje „Dane z dnia dzisiejszego” i twierdzi, że kursy „pozostają bez zmian”, choć się zmieniły; w pętli narzędziowej kurs 24 160 zł podaje jako „24,16 PLN”. Dalej `gemma4:31b` (67,7 s), który pisze bez wad, ale raz skraca procent do „2,3%” i traci przez to jeden scenariusz. Najszybszy w grupie `gpt-oss:20b` (24,4 s) raz zwraca pustą notatkę i jako jedyny potrzebuje osobnego ustawienia w konfiguracji. Najsłabiej wypadają `exaone3.5:32b`, który pisze „wzrosła o 2,30 PLN” zamiast procentu, i `mistral-small3.2` — „LPP 244,00 PLN” zamiast 24 400.

Modele tej grupy wymagają komputera z 32 GB RAM i Ollamy uruchomionej bezpośrednio w systemie. Odczyt pamięci z `/api/ps` zaniża przy tym rodzinę Gemma 4: `gemma4:26b` raportuje 8,6 GB, a proces modelu zajmuje naprawdę około 19 GB.

## Notatki modeli — po polsku

Teksty po oczyszczeniu przez `sanitize()`, czyli tak, jak trafiają na pulpit, z liczbą wystąpień na cztery przebiegi. W cyklach z ruchem — wszystkie wersje; w cyklach bez ruchu — najczęstsze. Pełne odpowiedzi, także surowe, są w `data/badanie-*.jsonl`.

### 1. `gemma4:26b` — miary 11/11, konsumowalne 11/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Modivo (MDV.WA) zmieniło kurs o 2,30%, a jego aktualna cena wynosi 97 PLN. Nie podano przyczyny tego ruchu.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63%. Aktualny kurs wynosi 15,93 PLN. Nie znam przyczyny tego ruchu.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63%. Aktualny kurs Grupa Polsat wynosi 15,93 PLN. Nie znam przyczyny tego ruchu.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63%. Aktualny kurs wynosi 15,93 PLN. Nie podano przyczyny tego ruchu.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA) zmieniła kurs o -5,34%. Aktualny kurs Grupa Polsat wynosi 15,08 PLN. Nie znam przyczyny tego ruchu.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 4× Modivo (MDV.WA) zmieniło kurs o 3,93% na poziomie 91,52 PLN, CD Projekt (CDR.WA) o 3,05% na poziomie 243,60 PLN, Grupa Polsat (CPS.WA) o 5,04% na poziomie 15,84 PLN oraz KGHM (KGH.WA) o 3,65% na poziomie 349,35 PLN. Nie znam przyczyny tych ruchów.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB (XTB.WA) zmieniło kurs o -2,12%. Kurs XTB wynosi 143,16 PLN. Nie znam przyczyny tego ruchu.
> 2× XTB (XTB.WA) zmieniło kurs o -2,12%. Aktualny kurs XTB wynosi 143,16 PLN. Nie znam przyczyny tego ruchu.

**Cykle bez ruchu, zestaw 1** (jedna wersja na 16 notatek):

> 16× Nie było znaczących zmian na liście spółek.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Nie było znaczących zmian na liście spółek.

### 2. `qwen3.8:27b` — miary 11/11, konsumowalne 11/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) zmieniło kurs o 2,30% względem ostatniego sprawdzenia, notując poziom 97,00 PLN. Nie znam przyczyny tego ruchu.
> 1× Modivo (MDV.WA) zmieniło kurs o 2,30% względem ostatniego sprawdzenia, osiągając poziom 97,00 PLN. Nie dysponuję informacjami o przyczynach tego ruchu.
> 1× Modivo (MDV.WA) zmieniło kurs o 2,30% względem ostatniego sprawdzenia, osiągając poziom 97,00 PLN. Nie znam przyczyny tego ruchu.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) spadła o 2,63% do poziomu 15,93 PLN. Nie znam przyczyny tego ruchu.
> 1× Grupa Polsat spadła o 2,63% do poziomu 15,93 PLN. Nie znam przyczyny tego ruchu.
> 1× Grupa Polsat (CPS.WA) spadła o 2,63% do poziomu 15,93 PLN. Nie znana jest przyczyna tego ruchu.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. Nie znam przyczyny tego ruchu.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do poziomu 15,08 PLN. Nie dysponuję informacjami o przyczynach tego ruchu.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% względem ostatniego sprawdzenia, osiągając kurs 15,08 PLN. Nie znam przyczyny tego ruchu.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do poziomu 15,08 PLN. Nie znam przyczyny tego ruchu.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Zauważono ruchy przekraczające 2% u czterech spółek: Modivo (91,52 PLN, zmiana 3,93%), CD Projekt (243,60 PLN, zmiana 3,05%), Grupa Polsat (15,84 PLN, zmiana 5,04%) oraz KGHM (349,35 PLN, zmiana 3,65%). Nie dysponuję informacjami o przyczynach tych zmian.
> 1× Zmiany przekraczające 2% odnotowano u Modivo (wzrost o 3,93% do 91,52 PLN), CD Projekt (wzrost o 3,05% do 243,60 PLN), Grupa Polsat (wzrost o 5,04% do 15,84 PLN) oraz KGHM (wzrost o 3,65% do 349,35 PLN). Nie dysponuję informacjami o przyczynach tych ruchów.
> 1× Zauważono ruchy kursowe w spółkach Modivo, CD Projekt, Grupa Polsat i KGHM. Modivo wzrosło o 3,93% do 91,52 PLN, CD Projekt o 3,05% do 243,60 PLN, Grupa Polsat o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Nie dysponuję informacjami o przyczynach tych zmian.
> 1× Zmiany przekraczające 2% odnotowały Modivo (+3,93%), CD Projekt (+3,05%), Grupa Polsat (+5,04%) oraz KGHM (+3,65%). Aktualne kursy tych spółek wynoszą odpowiednio 91,52 PLN, 243,60 PLN, 15,84 PLN i 349,35 PLN. Brak informacji o przyczynach tych ruchów cenowych.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB spadło o 2,12% do poziomu 143,16 PLN. Nie znam przyczyny tego ruchu.
> 1× XTB spadło o 2,12% do poziomu 143,16 PLN. Nie jestem w stanie wskazać przyczyny tego ruchu.

**Cykle bez ruchu, zestaw 1** (2 różnych wersji na 16 notatek):

> 15× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia.
> 1× Brak spółek z listy, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia.

### 3. `granite4:32b-a9b-h` — miary 11/11, konsumowalne 11/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 3× Modivo (MDV.WA) wzrosło o 2.30% do 97 PLN. Pozostałe spółki: XTB - 153.60 PLN, PKN Orlen - 148.18 PLN, Grupa Kęty - 1260.00 PLN, Pepco - 43.19 PLN, LPP - 24400.00 PLN, CD Projekt - 247.40 PLN, Grupa Polsat - 15.97 PLN, Benefit Systems - 5040.00 PLN, KGHM - 341.55 PLN.
> 1× Modivo (MDV.WA) wzrosło o 2.30% do 97 PLN. Pozostałe spółki: XTB 153.60 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260.00 PLN, Pepco 43.19 PLN, LPP 24400.00 PLN, CD Projekt 247.40 PLN, Grupa Polsat 15.97 PLN, Benefit Systems 5040.00 PLN, KGHM 341.55 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 3× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63% do 15,93 PLN. Pozostałe spółki na liście nie wykazały znaczących zmian.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63% do 15,93 PLN. Pozostałe spółki nie wykazały znaczących zmian.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) zmieniła kurs o -5.34% do 15.08 PLN. Pozostałe spółki: XTB - 150.34 PLN, PKN Orlen - 151.14 PLN, Grupa Kęty - 1269.00 PLN, Pepco - 41.87 PLN, LPP - 24160.00 PLN, Modivo - 88.06 PLN, CD Projekt - 236.40 PLN, Benefit Systems - 5135.00 PLN, KGHM - 337.05 PLN.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5,34% do 15,08 PLN. Pozostałe spółki: XTB 150,34 PLN, PKN Orlen 151,14 PLN, Grupa Kęty 1269,00 PLN, Pepco 41,87 PLN, LPP 24160,00 PLN, Modivo 88,06 PLN, CD Projekt 236,40 PLN, Benefit Systems 5135,00 PLN, KGHM 337,05 PLN.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5,34% do 15,08 PLN. Pozostałe spółki: XTB - 150,34 PLN, PKN Orlen - 151,14 PLN, Grupa Kęty - 1269,00 PLN, Pepco - 41,87 PLN, LPP - 24160,00 PLN, Modivo - 88,06 PLN, CD Projekt - 236,40 PLN, Benefit Systems - 5135,00 PLN, KGHM - 337,05 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 3× Modivo (MDV.WA) wzrosło o 3,93% do 91,52 PLN. CD Projekt (CDR.WA) wzrosło o 3,05% do 243,60 PLN. Grupa Polsat (CPS.WA) wzrosło o 5,04% do 15,84 PLN. KGHM (KGH.WA) wzrosło o 3,65% do 349,35 PLN.
> 1× Spółki, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia: Modivo (MDV.WA) o 3,93% do 91,52 PLN, CD Projekt (CDR.WA) o 3,05% do 243,60 PLN, Grupa Polsat (CPS.WA) o 5,04% do 15,84 PLN oraz KGHM (KGH.WA) o 3,65% do 349,35 PLN.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB (XTB.WA) zmieniło kurs o -2.12% do 143.16 PLN. Pozostałe spółki nie wykazały znaczących zmian.
> 1× XTB (XTB.WA) zmieniło kurs o -2.12% do 143.16 PLN. Pozostałe spółki na liście nie wykazały znaczących zmian.

**Cykle bez ruchu, zestaw 1** (2 różnych wersji na 16 notatek):

> 8× Nie było znaczących zmian w kursach spółek na liście.
> 8× Nie ma znaczących zmian w kursach spółek na liście.

**Cykle bez ruchu, zestaw 2** (2 różnych wersji na 8 notatek):

> 7× Nie było znaczących zmian w kursach spółek na liście.
> 1× Nie ma znaczących zmian w kursach spółek na liście.

### 4. `gemma4:31b` — miary 11/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 3× Kurs Modivo zmienił się o 2,30% i wynosi 97,00 PLN. Przyczyna ruchu jest nieznana.
> 1× Modivo zmieniło kurs o 2,3% i obecnie kosztuje 97,00 PLN. Przyczyna ruchu jest nieznana.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -2,63% i wynosi 15,93 PLN. Przyczyna ruchu jest nieznana.
> 2× Grupa Polsat zmieniła kurs o -2,63% i wynosi on 15,93 PLN. Przyczyna ruchu jest nieznana.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 3× Grupa Polsat zmieniła kurs o -5,34% i wynosi on 15,08 PLN. Przyczyna ruchu jest nieznana.
> 1× Grupa Polsat zmieniła kurs o -5,34% i obecnie kosztuje 15,08 PLN. Przyczyna ruchu jest nieznana.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 4× Znaczące zmiany kursu: Modivo 91,52 PLN (+3,93%), CD Projekt 243,6 PLN (+3,05%), Grupa Polsat 15,84 PLN (+5,04%), KGHM 349,35 PLN (+3,65%). Przyczyny ruchów są nieznane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 4× XTB zmienił kurs o -2,12% i wynosi 143,16 PLN. Przyczyna ruchu jest nieznana.

**Cykle bez ruchu, zestaw 1** (jedna wersja na 16 notatek):

> 16× Nie było znaczących zmian.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Nie było znaczących zmian.

### 5. `qwen3.6:27b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Modivo zmieniło kurs o 2,30% względem ostatniego sprawdzenia. Aktualna cena akcji wynosi 97,00 PLN. Nieznana jest przyczyna tej zmiany.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63% względem ostatniego sprawdzenia. Aktualna cena akcji wynosi 15,93 PLN. Nieznana jest przyczyna tej zmiany.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63% względem ostatniego sprawdzenia. Aktualna cena akcji wynosi 15,93 PLN. Nieznana jest przyczyna tej zmiany. To informacja, nie rekomendacja inwestycyjna.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -2,63% względem ostatniego sprawdzenia. Aktualny poziom cenowy tej spółki wynosi 15,93 PLN. Nieznana jest przyczyna tej zmiany.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat (CPS.WA) odnotowała spadek kursu o 5,34% względem ostatniego sprawdzenia. Aktualna cena akcji wynosi 15,08 PLN. Nieznana jest przyczyna tej zmiany.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5,34% względem ostatniego sprawdzenia. Aktualny poziom ceny tej spółki wynosi 15,08 PLN. Nieznana jest przyczyna tej zmiany.
> 1× Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. Nie znana jest przyczyna tej zmiany. To informacja, nie rekomendacja inwestycyjna.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5,34% względem ostatniego sprawdzenia. Aktualna cena akcji wynosi 15,08 PLN. Nieznana jest przyczyna tej zmiany.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Na monitorowanej liście zaobserwowano zmiany kursów przekraczające 2%. Grupa Polsat wzrosła o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Modivo podrożało o 3,93% do 91,52 PLN, a CD Projekt o 3,05% do 243,60 PLN. Nieznana jest przyczyna tych ruchów cenowych.
> 1× Kursy Modivo, CD Projekt, Grupy Polsat oraz KGHM wzrosły o co najmniej 2% od ostatniego sprawdzenia. Modivo notowane są po 91,52 PLN, CD Projekt po 243,60 PLN, Grupa Polsat po 15,84 PLN, a KGHM po 349,35 PLN. Przyczyny tych ruchów rynkowych nie są znane.
> 1× Na monitorowanej liście zaobserwowano zmiany kursów przekraczające 2% dla czterech spółek. Grupa Polsat wzrosła o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Modivo zyskało 3,93% osiągając 91,52 PLN, natomiast CD Projekt podrożało o 3,05% do 243,60 PLN. Przyczyn tych ruchów nie można jednoznacznie określić na podstawie dostępnych danych.
> 1× Na monitorowanej liście zaobserwowano zmiany kursów przekraczające 2%. Grupa Polsat wzrosła o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Modivo podrożało o 3,93% do 91,52 PLN, a CD Projekt o 3,05% do 243,60 PLN. Przyczyn tych ruchów nie można jednoznacznie określić.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB spadł o 2,12% do poziomu 143,16 PLN. Nieznana jest przyczyna tej zmiany. Pozostałe spółki z listy nie odnotowały istotnych ruchów.
> 1× XTB spadł o 2,12% do poziomu 143,16 PLN. Nieznana jest przyczyna tej zmiany. Pozostałe spółki z listy nie przekroczyły progu 2% zmiany.

**Cykle bez ruchu, zestaw 1** (12 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Nie odnotowano znaczących zmian kursowych na monitorowanej liście spółek. Brak spółek, których cena zmieniła się o co najmniej 2% od ostatniego sprawdzenia.
> 3× Nie odnotowano znaczących zmian kursowych na monitorowanej liście spółek z GPW. Brak spółek, których cena zmieniła się o co najmniej 2% od ostatniego sprawdzenia.
> 1× Nie odnotowano znaczących zmian kursowych na monitorowanej liście spółek. Żadna z pozycji nie zmieniła wartości o co najmniej 2% od ostatniego sprawdzenia. Sytuacja na rynku pozostaje stabilna w zakresie obserwowanych instrumentów. Brak danych sugerujących konieczność reakcji inwestycyjnej.

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× Nie odnotowano znaczących zmian kursowych na monitorowanej liście spółek z GPW. Żadna z podanych pozycji nie zmieniła wartości o co najmniej dwa procenty od ostatniego sprawdzenia. Sytuacja na rynku pozostaje stabilna w zakresie obserwowanych instrumentów. Brak danych sugerujących konieczność natychmiastowej reakcji inwestorskiej.
> 1× Nie odnotowano znaczących zmian kursowych na monitorowanej liście spółek z GPW. Żadna z podanych pozycji nie zmieniła wartości o co najmniej 2 procent od ostatniego sprawdzenia. Brak danych sugerujących istotne ruchy cenowe w tym okresie.
> 1× Nie odnotowano znaczących zmian kursowych na monitorowanej liście spółek. Żadna z pozycji nie zmieniła wartości o co najmniej 2 procent od ostatniego sprawdzenia. Rynek pozostaje stabilny w zakresie obserwowanych instrumentów. Brak danych sugerujących konieczność natychmiastowej reakcji inwestycyjnej.

### 6. `granite4.1:30b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) zmieniła kurs o 2.30% na poziomie 97.00 PLN. Przyczyny tego ruchu nieznane.
> 1× Modivo (MDV.WA) zmieniła kurs o 2.30% i wynosi teraz 97.00 PLN. Przyczyna tego ruchu nieznana.
> 1× Modivo (MDV.WA) zmieniła kurs o 2.30% i wynosi teraz 97.00 PLN. Przyczyny tego ruchu nieznane.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA) spadła o 2.63% do 15.93 PLN. Przyczyny tego ruchu nieznane.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA) spadła o 5.34% do 15.08 PLN. Przyczyny tego ruchu nieznane.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo, CD Projekt, Grupa Polsat i KGHM zmieniły kurs o co najmniej 2%. Modivo wzrosł do 91.52 PLN, CD Projekt do 243.60 PLN, Grupa Polsat do 15.84 PLN, a KGHM do 349.35 PLN. Przyczyny tych zmian nieznane.
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM zmieniły kurs o co najmniej 2%. Modivo osiągnął 91.52 PLN (+3.93%), CD Projekt 243.60 PLN (+3.05%), Grupa Polsat 15.84 PLN (+5.04%) i KGHM 349.35 PLN (+3.65%). Przyczyny tych zmian nieznane.
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM zarejestrowały zmiany kursu na GPW przekraczające 2%. Modivo wzrosł o 3.93% do 91.52 PLN, CD Projekt wzrosł o 3.05% do 243.60 PLN, Grupa Polsat wzrosła o 5.04% do 15.84 PLN, a KGHM wzrosła o 3.65% do 349.35 PLN. Przyczyny tych ruchów nie są znane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× Kurs akcji XTB spadł o 2.12% do 143.16 PLN. Przyczyny tego ruchu nieznane. Pozostałe spółki na liście nie wykazały zmian powyżej 2%.
> 1× Kurs akcji XTB spadł o 2.12% do 143.16 PLN. Przyczyny tego ruchu nieznane.

**Cykle bez ruchu, zestaw 1** (2 różnych wersji na 16 notatek):

> 15× Nie było znaczących zmian na liście spółek.
> 1× Nie było znaczących zmian na GPW.

**Cykle bez ruchu, zestaw 2** (2 różnych wersji na 8 notatek):

> 7× Nie było znaczących zmian na liście spółek.
> 1× Nie było znaczących zmian.

### 7. `gpt-oss:20b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo (MDV.WA) ruszyło o 2,30 % i osiągnęło 97,00 PLN.
> 1× Modivo ruszyło o 2,30 % i jest teraz na poziomie 97,00 PLN.
> 1× Modivo zmieniło kurs o 2,30 % i jest teraz na poziomie 97,00 PLN.
> 1× Modivo zmieniło kurs o 2,30 % i osiągnęło 97,00 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat spadła o 2,63 % i jest teraz 15,93 PLN.
> 1× Grupa Polsat zmieniła kurs o -2,63 %. Nie znana przyczyna. Kurs wynosi 15,93 PLN.
> 1× Grupa Polsat spadła o 2,63 % do 15,93 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat zmieniła kurs o 5,34 % w dół, spadł do 15,08 PLN. Nieznana przyczyna ruchu. Wszystkie pozostałe spółki nie wykazały zmian ponad 2 %.
> 1× Grupa Polsat spadła o 5,34 % i obecnie notuje się po 15,08 PLN.
> 1× Grupa Polsat zmniejszyła się o 5,34 % i jest teraz w cenie 15,08 PLN.
> 1× Grupa Polsat spadła o 5,34 % i jest teraz na poziomie 15,08 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo wzrosło o 3,93 % do 91,52 PLN, CD Projekt o 3,05 % do 243,60 PLN, Grupa Polsat o 5,04 % do 15,84 PLN oraz KGHM o 3,65 % do 349,35 PLN.
> 1× (pusto)
> 1× Modivo kurs wzrósł o 3,93 % do 91,52 PLN, CD Projekt o 3,05 % do 243,60 PLN, Grupa Polsat o 5,04 % do 15,84 PLN oraz KGHM o 3,65 % do 349,35 PLN. Przyczyny ruchów nie są znane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB zmniejszył się o 2,12 % i jest teraz na poziomie 143,16 PLN.
> 1× XTB (XTB.WA) spadła o 2,12 % i jest teraz 143,16 PLN.
> 1× XTB zmienił kurs o -2,12 % i obecnie wynosi 143,16 PLN.
> 1× XTB zmienił kurs o -2,12 % i aktualnie wynosi 143,16 PLN.

**Cykle bez ruchu, zestaw 1** (2 różnych wersji na 16 notatek):

> 14× Nie było znaczących zmian.
> 2× Brak znaczących zmian.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Nie było znaczących zmian.

### 8. `granite4.2:30b` — miary 10/11, konsumowalne 9/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) zmieniła kurs o 2.30% względem poprzedniego sprawdzenia, aktualny kurs wynosi 97.00 PLN. Nie znam przyczyny tego ruchu. XTB: 153.60 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1260.00 PLN, Pepco: 43.19 PLN, LPP: 24400.00 PLN, CD Projekt: 247.40 PLN, Grupa Polsat: 15.97 PLN, Benefit Systems: 5040.00 PLN, KGHM: 341.55 PLN.
> 1× Modivo zmieniła kurs o 2.30% względem poprzedniego sprawdzenia, aktualny kurs wynosi 97.00 PLN. Nie znam przyczyny tego ruchu. Nie było znaczących zmian w innych spółkach z listy.
> 1× Modivo (MDV.WA) zmieniła kurs o 2.30% względem poprzedniego sprawdzenia, aktualny kurs wynosi 97.00 PLN. Nie znany jest powód tego ruchu. XTB: 153.60 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1260.00 PLN, Pepco: 43.19 PLN, LPP: 24400.00 PLN, CD Projekt: 247.40 PLN, Grupa Polsat: 15.97 PLN, Benefit Systems: 5040.00 PLN, KGHM: 341.55 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) zmieniła kurs o minus 2.63 procent względem poprzedniego sprawdzenia, aktualny kurs wynosi 15.93 PLN. Nie znam przyczyny tego ruchu. Aktualne kursy innych spółek z listy to XTB: 150.68 PLN, PKN Orlen: 150.42 PLN, Grupa Kęty: 1267.00 PLN, Pepco: 42.03 PLN, LPP: 24300.00 PLN, Modivo: 87.34 PLN, CD Projekt: 238.90 PLN, Benefit Systems: 5160.00 PLN, KGHM: 343.35 PLN.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o minus 2.63 procent względem poprzedniego sprawdzenia. Aktualny kurs Grupy Polsat wynosi 15.93 PLN. Przyczynę tego ruchu nie znam i nie zgaduję. Brak innych spółek z przekroczonego progu zmiany.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -2.63% względem poprzedniego sprawdzenia, aktualny kurs wynosi 15.93 PLN. Nie znam przyczyny tego ruchu. XTB: 150.68 PLN, PKN Orlen: 150.42 PLN, Grupa Kęty: 1267.00 PLN, Pepco: 42.03 PLN, LPP: 24300.00 PLN, Modivo: 87.34 PLN, CD Projekt: 238.90 PLN, Grupa Polsat: 15.93 PLN, Benefit Systems: 5160.00 PLN, KGHM: 343.35 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5.33% względem poprzedniego sprawdzenia, aktualny kurs wynosi 15.08 PLN. Nie znam przyczyny tego ruchu. Pozostałe spółki z listy nie wykazały zmian powyżej 2% od ostatniego sprawdzenia.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5.33% względem poprzedniego sprawdzenia, aktualny kurs wynosi 15.08 PLN. Nie znany jest powód tego ruchu. Pozostałe spółki z listy nie wykazały zmian powyżej 2% od ostatniego sprawdzenia.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5.34% względem poprzedniego sprawdzenia, aktualny kurs wynosi 15.08 PLN. Nie znam przyczyny tego ruchu. Pozostałe spółki z listy nie wykazały zmian powyżej 2% od ostatniego sprawdzenia.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5.33% względem poprzedniego sprawdzenia, aktualny kurs wynosi 15.08 PLN. Nie znasz przyczyny ruchu, zamiast zgadywać podaj to wprost. Aktualne kursy innych spółek z listy to XTB: 150.34 PLN, PKN Orlen: 151.14 PLN, Grupa Kęty: 1269.00 PLN, Pepco: 41.87 PLN, LPP: 24160.00 PLN, Modivo: 88.06 PLN, CD Projekt: 236.40 PLN, Benefit Systems: 5135.00 PLN, KGHM: 337.05

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo zmieniła kurs o 3.93 % względem poprzedniego sprawdzenia, CD Projekt o 3.05 %, Grupa Polsat o 5.04 % i KGHM o 3.65 %. Modivo wynosi 91.52 PLN, CD Projekt 243.60 PLN, Grupa Polsat 15.84 PLN oraz KGHM 349.35 PLN. Przyczyny ruchu nie są znane.
> 1× Modivo zmieniła kurs o 3.93 %, CD Projekt o 3.05 %, Grupa Polsat o 5.04 % i KGHM o 3.65 % względem poprzedniego sprawdzenia. Przyczyna tego ruchu nie jest znana. Modivo: 91.52 PLN, CD Projekt: 243.60 PLN, Grupa Polsat: 15.84 PLN, KGHM: 349.35 PLN.
> 1× Modivo zmieniła kurs o 3.93 % względem poprzedniego sprawdzenia, CD Projekt o 3.05 %, Grupa Polsat o 5.04 % i KGHM o 3.65 %. Modivo wynosi 91.52 PLN, CD Projekt 243.60 PLN, Grupa Polsat 15.84 PLN oraz KGHM 349.35 PLN. Przyczyny tego ruchu nie są znane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB (XTB.WA) zmienił kurs o -2.12% względem poprzedniego sprawdzenia, aktualny kurs wynosi 143.16 PLN. Nie znam przyczyny tego ruchu. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems, KGHM, Rainbow Tours nie zmieniły kursu o co najmniej 2% od ostatniego sprawdzenia.
> 1× XTB (XTB.WA) zmieniła kurs o -2.12% względem poprzedniego sprawdzenia, aktualny kurs wynosi 143.16 PLN. Nie znany jest powód tego ruchu. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems, KGHM i Rainbow Tours nie wykazały zmian przekraczających 2% od ostatniego sprawdzenia.
> 1× XTB (XTB.WA) zmieniła kurs o –2.12% względem poprzedniego sprawdzenia, aktualny kurs wynosi 143.16 PLN. Nie znam przyczyny tego ruchu. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems, KGHM i Rainbow Tours nie zmieniły kursu o co najmniej 2% od ostatniego sprawdzenia.
> 1× XTB (XTB.WA) zmienił kurs o –2.12% względem poprzedniego sprawdzenia, aktualny kurs wynosi 143.16 PLN. Nie znam przyczyny tego ruchu. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems, KGHM i Rainbow Tours nie wykazały zmian powyżej 2% i nie są wymienione w tej wiadomości.

**Cykle bez ruchu, zestaw 1** (8 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian w kursach spółek z listy. XTB: 149.90 PLN, PKN Orlen: 149.54 PLN, Grupa Kęty: 1284.00 PLN, Pepco: 44.12 PLN, LPP: 24600.00 PLN, Modivo: 96.42 PLN, CD Projekt: 247.50 PLN, Grupa Polsat: 16.14 PLN, Benefit Systems: 5160.00 PLN, KGHM: 347.25 PLN.
> 3× Nie było znaczących zmian w kursach spółek z listy. XTB: 151.02 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1288.00 PLN, Pepco: 43.47 PLN, LPP: 24620.00 PLN, Modivo: 96.30 PLN, CD Projekt: 245.30 PLN, Grupa Polsat: 16.10 PLN, Benefit Systems: 5120.00 PLN, KGHM: 348.35 PLN.
> 3× Nie było znaczących zmian w kursach spółek z listy. XTB: 149.82 PLN, PKN Orlen: 148.62 PLN, Grupa Kęty: 1282.00 PLN, Pepco: 43.17 PLN, LPP: 24420.00 PLN, Modivo: 96.70 PLN, CD Projekt: 246.10 PLN, Grupa Polsat: 16.01 PLN, Benefit Systems: 5150.00 PLN, KGHM: 347.65 PLN.

**Cykle bez ruchu, zestaw 2** (3 różnych wersji na 8 notatek):

> 4× Nie było znaczących zmian w kursach spółek z listy. XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292.00 PLN, Pepco: 48.00 PLN, LPP: 26400.00 PLN, Modivo: 92.90 PLN, CD Projekt: 262.40 PLN, Grupa Polsat: 18.30 PLN, Benefit Systems: 5730.00 PLN, KGHM: 327.75 PLN, Rainbow Tours: 126.70 PLN.
> 3× Nie było znaczących zmian w kursach spółek z listy. XTB: 146.26 PLN, PKN Orlen: 154.40 PLN, Grupa Kęty: 1276.00 PLN, Pepco: 47.89 PLN, LPP: 26480.00 PLN, Modivo: 93.80 PLN, CD Projekt: 262.00 PLN, Grupa Polsat: 18.00 PLN, Benefit Systems: 5635.00 PLN, KGHM: 321.30 PLN, Rainbow Tours: 124.80 PLN.
> 1× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Nie było znaczących zmian. XTB: 146.26 PLN, PKN Orlen: 154.40 PLN, Grupa Kęty: 1276.00 PLN, Pepco: 47.89 PLN, LPP: 26480.00 PLN, Modivo: 93.80 PLN, CD Projekt: 262.00 PLN, Grupa Polsat: 18.00 PLN, Benefit Systems: 5635.00 PLN, KGHM: 321.30 PLN, Rainbow Tours: 124.80 PLN.

### 9. `laguna-xs-2.1` — miary 11/11, konsumowalne 7/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo (MDV.WA) wzrósł o 2,30 % do 97,00 PLN. Brak innych spółek z listy GPW notowanych z większym ruchem.
> 1× Modivo (MDV.WA) wzrósł o 2,30 % do 97,00 PLN. Brak innych spółek z listy GPW notowanych z większą niż 2 % zmianą.
> 1× Modivo (MDV.WA) wzrósł o 2,30 % do 97,00 PLN. Brak innych spółek z listy notowanych z większą niż 2 % ruchem. Kursy pozostałych spółek: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260,00 PLN, Pepco 43,19 PLN, LPP 24400,00 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,97 PLN, Benefit Systems 5040,00 PLN, KGH
> 1× Modivo (MDV.WA) wzrósł o 2,30 % do 97,00 PLN. Brak innych spółek z listy notowanych z większą niż 2 % zmianą.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat (CPS.WA) notuje spadek o 2,63% do 15,93 PLN od ostatniego sprawdzenia. Kursy pozostałych spółek z listy: XTB 150,68 PLN, PKN Orlen 150,42 PLN, Grupa Kęty 1267,00 PLN, Pepco 42,03 PLN, LPP 24300,00 PLN, Modivo 87,34 PLN, CD Projekt 238,90 PLN, Benefit Systems 5160,00 PLN, KGHM 343,35 PLN. Nie znam prz
> 1× Grupa Polsat (CPS.WA) notuje spadek o 2,63% do 15,93 PLN od ostatniego sprawdzenia. Warto zauważyć, że ruch ten nie wynika z znanych czynników makroekonomicznych ani publikacji, które mogłyby go wywołać. Kursy pozostałych spółek z listy pozostają bez zmian. Brak istotnych ruchów w pozostałych pozycjach.
> 1× Grupa Polsat (CPS.WA) notuje spadek o 2,63% do 15,93 PLN od ostatniego sprawdzenia. Warto zauważyć, że ruch ten wystąpił bez wyraźnego powodu w dostępnych informacjach. Pozostałe spółki z listy utrzymują dotychczasowe poziomy cen. Brak dodatkowych istotnych zmian w innych pozycjach portfela.
> 1× Grupa Polsat (CPS.WA) notuje spadek o 2,63% do 15,93 PLN od ostatniego sprawdzenia. Warto zauważyć, że ruch ten wystąpił bez wyraźnej przyczyny w dostępnych informacjach. Kursy pozostałych spółek pozostają bez zmian w tej notacji. Brak istotnych ruchów w pozostałych pozycjach listy.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat (CPS.WA) notuje spadek o 5.34% do 15.08 PLN od ostatniego sprawdzenia. Innych spółek z listy nie wykazano z większą zmianą niż 2%. Brak istotnych ruchów w pozostałych pozycjach.
> 1× Grupa Polsat (CPS.WA) notuje spadek o 5.34% do 15.08 PLN od ostatniego sprawdzenia. Inne spółki z listy nie zmieniły kursu o co najmniej 2%. Nie znam przyczyn takiego ruchu.
> 1× Grupa Polsat (CPS.WA) notuje spadek o 5.34% do 15.08 PLN od ostatniego sprawdzenia. Inne spółki z listy nie zmieniły kursu o co najmniej 2%. Brak istotnych ruchów w pozostałych pozycjach.
> 1× Grupa Polsat (CPS.WA) notuje spadek o 5.34% do 15.08 PLN od ostatniego sprawdzenia. Kursy pozostałych spółek z listy: XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269.00 PLN, Pepco 41.87 PLN, LPP 24160.00 PLN, Modivo 88.06 PLN, CD Projekt 236.40 PLN, Benefit Systems 5135.00 PLN, KGHM 337.05 PLN. Nie znam prz

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo (MDV.WA) wzrosła o 3,93% do 91,52 PLN, CD Projekt (CDR.WA) o 3,05% do 243,60 PLN, Grupa Polsat (CPS.WA) o 5,04% do 15,84 PLN, KGHM (KGH.WA) o 3,65% do 349,35 PLN. Nie znam przyczyn tych ruchów.
> 1× Na liście spółek z GPW, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia, znajdują się: Modivo (MDV.WA) – 91.52 PLN, +3.93%; CD Projekt (CDR.WA) – 243.60 PLN, +3.05%; Grupa Polsat (CPS.WA) – 15.84 PLN, +5.04%; KGHM (KGH.WA) – 349.35 PLN, +3.65%. Nie znam przyczyn tych ruchów.
> 1× Spółki z listy monitorowanej prywatnej zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia: Modivo (MDV.WA) wzrasta o 3,93% do 91,52 PLN, CD Projekt (CDR.WA) o 3,05% do 243,60 PLN, Grupa Polsat (CPS.WA) o 5,04% do 15,84 PLN oraz KGHM (KGH.WA) o 3,65% do 349,35 PLN. Przyczyny ruchu nie są znane. Aktualne kursy pozostałych spó

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB (XTB.WA) spadło o 2.12% do 143.16 PLN. Brak innych spółek z zmianą kursu co najmniej 2%.
> 1× XTB (XTB.WA) notuje spadek o 2.12% do 143.16 PLN. Inne spółki z listy nie wykazały zmian większych niż 2%. Kursy pozostałych spółek pozostają bez zmian. Brak istotnych ruchów w pozostałych pozycjach.
> 1× XTB (XTB.WA) notuje spadek o 2.12% do 143.16 PLN. Inne spółki z listy nie wykazały zmian większych niż 2%. Kursy pozostałych spółek pozostają bez zmian. Brak istotnych ruchów w portfelu.
> 1× XTB (XTB.WA) spadło o 2.12% do 143.16 PLN. Brak innych znaczących zmian w portfelu.

**Cykle bez ruchu, zestaw 1** (13 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Na liście monitoringowej nie odnotowano żadnych spółek z kursową zmianą większą niż 2% od ostatniego sprawdzenia. Brak istotnych ruchów na rynku. Aktualne notowania pozostają bez zmian.
> 2× Na liście monitoringowej nie odnotowano żadnych spółek z kursem zmienionym o co najmniej 2% od ostatniego sprawdzenia. Brak istotnych ruchów w notowaniach. Aktualne poziomy cen spółek pozostają bez zmian.
> 1× Na liście monitoringowej nie odnotowano żadnych spółek z kursem zmienionym o co najmniej 2% od ostatniego sprawdzenia. Brak istotnych ruchów w notowaniach. Dane z dnia dzisiejszego.

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Na liście monitoringowej nie odnotowano żadnych spółek z kursem zmienionym o co najmniej 2% od ostatniego sprawdzenia. Nie wiadomo przyczyn ewentualnych ruchów. Brak istotnych zmian w notowaniach. Dane zostały zaktualizowane.
> 1× Na liście monitoringowej nie odnotowano żadnych spółek z kursem zmienionym o co najmniej 2% od ostatniego sprawdzenia. Nie wiadomo przyczyn ewentualnych ruchów. Dane cenowe aktualnych notowań pozostają niezmienione. Brak istotnych zmian na rynku.
> 1× Na liście monitoringowej nie odnotowano żadnych spółek z kursem zmienionym o co najmniej 2% od ostatniego sprawdzenia. Brak istotnych ruchów w notowaniach. Aktualne poziomy cen spółek pozostają bez zmian.

### 10. `mistral-small3.2` — miary 6/11, konsumowalne 6/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo zmienił kurs o 2,30% i notuje 97,00 PLN. Aktualne kursy: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260,00 PLN, Pepco 43,19 PLN, LPP 244,00 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,97 PLN, Benefit Systems 5040,00 PLN, KGHM 341,55 PLN. Przyczyna ruchu Modivo nie jest znana.
> 1× Modivo zmienił kurs o 2,30% i notuje się na poziomie 97,00 PLN. Aktualne kursy: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260,00 PLN, Pepco 43,19 PLN, LPP 244,00 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,97 PLN, Benefit Systems 5040,00 PLN, KGHM 341,55 PLN.
> 1× Modivo (MDV.WA) wzrosła o 2,30% do 97,00 PLN. Aktualne kursy: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260,00 PLN, Pepco 43,19 PLN, LPP 24400,00 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,97 PLN, Benefit Systems 5040,00 PLN, KGHM 341,55 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -2,63% i wynosi 15,93 PLN. Aktualne kursy: XTB 150,68 PLN, PKN Orlen 150,42 PLN, Grupa Kęty 1267,00 PLN, Pepco 42,03 PLN, LPP 243,00 PLN, Modivo 87,34 PLN, CD Projekt 238,90 PLN, Benefit Systems 5160,00 PLN, KGHM 343,35 PLN.
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN. Pozostałe spółki z listy nie zmieniły kursu o co najmniej 2%.
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN. Pozostałe spółki z listy: XTB 150,68 PLN, PKN Orlen 150,42 PLN, Grupa Kęty 1267,00 PLN, Pepco 42,03 PLN, LPP 243,00 PLN, Modivo 87,34 PLN, CD Projekt 238,90 PLN, Benefit Systems 5160,00 PLN, KGHM 343,35 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat zmieniła kurs o -5,34% i wynosi 15,08 PLN. Aktualne kursy: XTB 150,34 PLN, PKN Orlen 151,14 PLN, Grupa Kęty 1269,00 PLN, Pepco 41,87 PLN, LPP 241,60 PLN, Modivo 88,06 PLN, CD Projekt 236,40 PLN, Benefit Systems 5135,00 PLN, KGHM 337,05 PLN.
> 1× Grupa Polsat zmieniła kurs o -5,34% i obecnie notowana jest na poziomie 15,08 PLN. Aktualne kursy: XTB 150,34 PLN, PKN Orlen 151,14 PLN, Grupa Kęty 1269,00 PLN, Pepco 41,87 PLN, LPP 241,60 PLN, Modivo 88,06 PLN, CD Projekt 236,40 PLN, Benefit Systems 5135,00 PLN, KGHM 337,05 PLN.
> 1× Grupa Polsat zmieniła kurs o -5,34% do 15,08 PLN. Aktualne kursy: XTB 150,34 PLN, PKN Orlen 151,14 PLN, Grupa Kęty 1269,00 PLN, Pepco 41,87 PLN, LPP 241,60 PLN, Modivo 88,06 PLN, CD Projekt 236,40 PLN, Benefit Systems 5135,00 PLN, KGHM 337,05 PLN.
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN. Pozostałe spółki z listy: XTB 150,34 PLN, PKN Orlen 151,14 PLN, Grupa Kęty 1269,00 PLN, Pepco 41,87 PLN, LPP 241,60 PLN, Modivo 88,06 PLN, CD Projekt 236,40 PLN, Benefit Systems 5135,00 PLN, KGHM 337,05 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 3× Modivo wzrosła o 3,93% do 91,52 PLN, CD Projekt o 3,05% do 243,60 PLN, Grupa Polsat o 5,04% do 15,84 PLN, KGHM o 3,65% do 349,35 PLN. Przyczyny ruchów nie są znane.
> 1× Modivo (MDV.WA) wzrosła o 3,93% do 91,52 PLN, CD Projekt (CDR.WA) o 3,05% do 243,60 PLN, Grupa Polsat (CPS.WA) o 5,04% do 15,84 PLN, KGHM (KGH.WA) o 3,65% do 349,35 PLN. Przyczyny ruchów nie są znane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB zmienił kurs o -2,12% i notuje 143,16 PLN. PKN Orlen: 155,02 PLN, Grupa Kęty: 1277,00 PLN, Pepco: 47,45 PLN, LPP: 264,00 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems: 5615,00 PLN, KGHM: 322,45 PLN, Rainbow Tours: 125,10 PLN. Przyczyna ruchu kursu XTB nie jest znana.
> 1× XTB.WA zmienił kurs o -2,12% i notuje 143,16 PLN. PKN Orlen: 155,02 PLN, Grupa Kęty: 1277,00 PLN, Pepco: 47,45 PLN, LPP: 264,00 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems: 5615,00 PLN, KGHM: 322,45 PLN, Rainbow Tours: 125,10 PLN. Przyczyna ruchu kursu XTB.WA nie jest znana.
> 1× XTB zmienił kurs o -2,12% i notuje 143,16 PLN. Pozostałe spółki z listy nie przekroczyły progu 2% zmian. Nie było znaczących zmian w kursach.

**Cykle bez ruchu, zestaw 1** (jedna wersja na 16 notatek):

> 16× Nie było znaczących zmian.

**Cykle bez ruchu, zestaw 2** (2 różnych wersji na 8 notatek):

> 7× Nie było znaczących zmian.
> 1× Nie było znaczących zmian. Aktualne kursy spółek: XTB 147,82 PLN, PKN Orlen 154,98 PLN, Grupa Kęty 1292,00 PLN, Pepco 48,00 PLN, LPP 264,00 PLN, Modivo 92,90 PLN, CD Projekt 262,40 PLN, Grupa Polsat 18,30 PLN, Benefit Systems 5730,00 PLN, KGHM 327,75 PLN, Rainbow Tours 126,70 PLN.

### 11. `exaone3.5:32b` — miary 8/11, konsumowalne 2/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo wzrosła do 97,00 PLN o 2,30%. XTB: 153,60 PLN, PKN Orlen: 148,18 PLN, Grupa Kęty: 1260,00 PLN, Pepco: 43,19 PLN, LPP: 24400,00 PLN, CD Projekt: 247,40 PLN, Grupa Polsat: 15,97 PLN, Benefit Systems: 5040,00 PLN, KGHM: 341,55 PLN. Nie było innych znaczących zmian.
> 1× Modivo wzrosła o 2,30 PLN do poziomu 97,00 PLN. Inne spekulowane spółki nie wykazały zmian o więcej niż 2%.
> 1× Modivo wzrosła o 2,30 PLN, obecnie 97,00 PLN. Inne spółki na liście nie wykazują zmian o więcej niż 2%.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA) spadła o -2,63% do poziomu 15,93 PLN. Inne spółki na liście nie wykazują zmian o więcej niż 2%.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) spadła o 5,34% do poziomu 15,08 PLN. Inne spółki na liście nie wykazują zmian o więcej niż 2%.
> 1× Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. Inne spady lub wzrosty o co najmniej 2% nie wystąpiły. XTB: 150,34 PLN, PKN Orlen: 151,14 PLN, Grupa Kęty: 1269,00 PLN, Pepco: 41,87 PLN, LPP: 24160,00 PLN, Modivo: 88,06 PLN, CD Projekt: 236,40 PLN, Benefit Systems: 5135,00 PLN, KGHM: 337,05 PLN.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do 15,08 PLN. Inne spółki na liście nie wykazują zmian o więcej niż 2%.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Grupa Polsat, CD Projekt, Modivo, KGHM zmieniły kurs o więcej niż 2%. Grupa Polsat: 15,84 PLN CD Projekt: 243,60 PLN Modivo: 91,52 PLN KGHM: 349,35 PLN
> 1× Grupa Polsat, KGHM, CD Projekt, Modivo zmieniły kurs o więcej niż 2%. Grupa Polsat: 15,84 PLN KGHM: 349,35 PLN CD Projekt: 243,60 PLN Modivo: 91,52 PLN
> 1× Grupa Polsat, CD Projekt, Modivo, KGHM zmieniły się o więcej niż 2%. Grupa Polsat: 15,84 PLN CD Projekt: 243,60 PLN Modivo: 91,52 PLN KGHM: 349,35 PLN

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB spadło o -2,12% do 143,16 PLN. Inne spekulowane spółki nie wykazują zmian o więcej niż 2%.
> 1× XTB spadło o 2,12%, obecny kurs: 143,16 PLN. Inne sposoby na liście nie wykazują znaczących zmian.
> 1× XTB spadło o -2,12% do 143,16 PLN. Inne spekulowane spółki nie wykazują zmian o więcej niż 2%. PKN Orlen: 155,02 PLN, Grupa Kęty: 1277 PLN, Pepco: 47,45 PLN, LPP: 26400 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems: 5615 PLN, KGHM: 322,45 PLN, Rainbow Tours: 125,
> 1× XTB spadło o 2,12% do 143,16 PLN. Inne sposoby na liście nie wykazywali znaczących zmian. PKN Orlen: 155,02 PLN, Grupa Kęty: 1277 PLN, Pepco: 47,45 PLN, LPP: 26400 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems: 5615 PLN, KGHM: 322,45 PLN, Rainbow Tours: 125,10 PLN.

**Cykle bez ruchu, zestaw 1** (12 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Nie było spółek na liście z kursami zmienionymi o co najmniej 2%. Nie wystąpiły znaczących zmian cenowych. XTB: 152.10 PLN, PKN Orlen: 148.32 PLN, Grupa Kęty: 1260.00 PLN, Pepco: 43.15 PLN, LPP: 24300.00 PLN, Modivo: 94.82 PLN, CD Projekt: 245.80 PLN, Grupa Polsat: 15.94 PLN, Benefit Systems: 5065.00 PLN, KGHM: 3
> 2× Nie było spółek z listy, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Stan poziomów cen jest stabilny. XTB: 149.90 PLN, PKN Orlen: 149.54 PLN, Grupa Kęty: 1284.00 PLN, Pepco: 44.12 PLN, LPP: 24600.00 PLN, Modivo: 96.42 PLN, CD Projekt: 247.50 PLN, Grupa Polsat: 16.13 PLN, Benefit Systems: 5160.00 PLN, KGHM: 3
> 2× Nie było znaczących zmian w kursach spółek na liście.

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Brak znaczących zmian w kursach spółek na liście.
> 2× Nie było znaczących zmian w kursach spółek na liście.
> 1× Nie było spółek na liście z kursami zmienionymi o co najmniej 2%. Stan poziomów cen jest stabilny bez znaczących zmian. XTB: 146,26 PLN, PKN Orlen: 154,40 PLN, Grupa Kęty: 1276,00 PLN, Pepco: 47,89 PLN, LPP: 26480,00 PLN, Modivo: 93,80 PLN, CD Projekt: 262,00 PLN, Grupa Polsat: 18,00 PLN, Benefit Systems: 5635,00 PLN, KGHM: 3

