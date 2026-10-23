# Część 4b. Wyniki — grupa 2: od 2 do 6 GB

Serce badania i najliczniejsza grupa. Modele mieszczą się na komputerze z 16 GB bez karty graficznej.

Wszystkie modele na ustawieniach aplikacji (domyślnie `think: false`, 180 tokenów, temperatura 0,2, okno domyślne Ollamy; `gpt-oss:20b` z `think: "low"`). Jedenaście scenariuszy w dwóch zestawach (rozdział 3.1.1), każdy model w czterech przebiegach od czystej pamięci, czyli 44 notatki po polsku. Kolejność wierszy to ranking końcowy grupy według kryteriów z rozdziału 3.7: najpierw liczba scenariuszy z notatkami konsumowalnymi, potem polszczyzna bez wad, przy remisie ustawienia domyślne przed wpisem w konfiguracji i krótszy zimny start.

## Zestawienie modeli

| Miejsce | Model | Test z rozdz. 3 PL | A/B PL | Zestaw 2 | Test z rozdz. 3 EN | A/B EN | 11 scenariuszy: miary / konsumowalne | Główny problem PL | Główny problem EN | Zimny start (PL) | Rozmiar | Licencja | Dodany |
|---:|---|:---:|:---:|:---:|:---:|:---:|:---:|---|---|---:|---:|---|---|
| 1 | **`granite4.1:8b`** | ✓ | ✓ | ✓ | ✓ | ✓ | 11 / **11** | kursy w cyklach bez ruchu wbrew zakazowi, kropka, „spolki”, „Grupa Polsat wzrosło” | kursy w cyklach bez ruchu wbrew zakazowi | 37,9 s | 5,35 GB | Apache 2.0 | 2026-04-06 |
| 2 | `granite4.1:3b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 10 | kropka, „Brak zmian w cenie”; przy czterech spółkach same kursy bez procentów | kursy bez ruchu, wiersz danych zamiast zdania | 9,5 s | 2,10 GB | Apache 2.0 | 2026-04-06 |
| 3 | `qwen3.5:4b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 10 | kursy bez ruchu wbrew zakazowi, „z Twojej listy”; przy czterech spółkach zmiany w złotych zamiast procentów | „prices remain unchanged”, choć kursy się zmieniły | 21,1 s | 3,39 GB | Apache 2.0 | 2026-02-27 |
| 4 | `granite4:7b-a1b-h` | ✓ | ✓ | ✗ | ✗ | ✗ | 10 / 9 | procent niezaokrąglony; w zestawie 2 gubi KGHM | „moving from 97 PLN” — podaje kurs bieżący jako wyjściowy | 10,2 s | 4,23 GB | Apache 2.0 | 2025-09-16 |
| 5 | `phi4-mini` | ✗ | ✗ | ✗ | ✓ | ✓ | 8 / 8 | odwraca kierunek („zmniejszając się do 97 PLN”) | procent niezaokrąglony, kursy bez ruchu | 8,9 s | 2,49 GB | MIT | 2025-02-19 |
| 6 | `gemma4:e2b` | ✗ | ✗ | ✗ | ✗ | ✓ | 10 / 8 | wypisuje kursy wszystkich spółek, kropka, procent niezaokrąglony; o 17:00 kurs Polsatu 15,965 zł jako „15965.00 PLN” | procent niezaokrąglony, kursy innych spółek o 17:00 | 10,6 s | 4,34 GB | Apache 2.0 | 2026-03-02 |
| 7 | `ministral-3:3b` | ✓ | ✓ | ✗ | ✗ | ✗ | 10 / 8 | „Brak spółek z listy zmieniło kurs”, procent z jednym miejscem („−5,3%”) | „2.3%” — jedno miejsce zamiast dwóch | 16,7 s | 2,95 GB | Apache 2.0 | 2025-10-31 |
| 8 | `granite4.2:8b` | ✗ | ✗ | ✗ | ✓ | ✓ | 10 / 7 | „zmieniła kurs o co najmniej 2%” — nie podaje, o ile | komentarze o poleceniach („No company moved…, so no price details”) | 26,8 s | 5,35 GB | Apache 2.0 | 2026-08-07 |
| 9 | `bielik-minitron-7B` | ✗ | ✓ | ✗ | ✗ | ✗ | 8 / 7 | obcina „2,29%”; w zestawie 2 gubi minus („XTB wzrosło o 2.12%”) | „2.2991%”, kursy wszystkich spółek | 38,6 s | 4,50 GB | Apache 2.0\* | 2026-01-22 |
| 10 | `llama3.1:8b` | ✗ | ✓ | ✗ | ✗ | ✗ | 6 / 6 | gubi ruch Modivo („Brak znaczących zmian”, 2/4) | gubi ruch we wszystkich przebiegach | 19,9 s | 4,92 GB | Llama 3.1 Community (warunkowo) | 2024-07-18 |
| 11 | `llama3.2:3b` | ✗ | ✗ | ✗ | ✓ | ✓ | 5 / 4 | jedna zmyślona kwota („0,06 PLN”), dopisuje CD Projekt do ruchu | **bez wad** | 11,8 s | 2,02 GB | Llama 3.2 Community (warunkowo) | 2024-09-18 |
| 12 | `granite4.2:3b` | ✗ | ✗ | ✗ | ✓ | ✗ | 9 / 4 | przepisuje polecenie do notatki; obcina „5,33%” | „none” na początku notatki; raz „moved down” przy wzroście | 14,3 s | 2,24 GB | Apache 2.0 | 2026-08-07 |
| 13 | `qwen3.5:2b` | ✗ | ✗ | ✗ | ✗ | ✗ | 2 / 2 | zmyśla procenty w cyklach bez ruchu | zmyśla ruchy innych spółek | 14,1 s | 2,74 GB | Apache 2.0 | 2026-02-28 |
| 14 | `falcon3:7b` | ✗ | ✗ | ✗ | ✗ | ✓ | 7 / 2 | zmyśla ruchy wszystkich spółek | raz zmyśla ruch XTB | 34,3 s | 4,57 GB | TII Falcon (warunkowo) | 2024-11-29 |
| 15 | `exaone3.5:7.8b` | ✗ | ✗ | ✗ | ✗ | ✓ | 8 / 1 | obcina „2,29%”, zmyśla ruchy | obcina „2.29%” — bez polecenia zaokrąglania już nie | 34,5 s | 4,77 GB | EXAONE 1.1 – NC (niekomercyjna) | 2024-12-01 |
| 16 | `falcon3:3b` | ✗ | ✗ | ✗ | ✓ | ✓ | 3 / 0 | bełkot, zmyślone wyliczenia | wypisuje kursy wszystkich spółek, procent niezaokrąglony | 15,9 s | 2,01 GB | TII Falcon (warunkowo) | 2024-12-14 |
| 17 | `nemotron-3-nano:4b` | ✗ | ✗ | ✗ | ✓ | ✓ | 6 / 0 | zmyśla ruchy, przepisuje polecenie do notatki | „2.2991%”, zdania bez kropek | 15,9 s | 2,84 GB | NVIDIA Open Model | 2026-03-07 |

\* Brak licencji w samym pliku modelu; readme na koncie wydawcy wskazuje Apache 2.0 wraz z regulaminem na bielik.ai/terms.

Testy: ✓ — wszystkie notatki testu konsumowalne (rozdział 3.5.1) i bez błędu treści przy lekturze, we wszystkich czterech przebiegach. A/B to ten sam test bez zdania o zaokrąglaniu. Miary to automat (rozdział 3.4), konsumowalne — notatki z prawdziwymi i kompletnymi danymi mimo usterek formy (rozdział 3.5.1). Pogrubienie: komplet jedenastu scenariuszy z notatkami konsumowalnymi.

## Wyniki per scenariusz

| Scenariusz | Zestaw | Start | Bez błędu w miarach | Konsumowalnych | Najszybszy konsumowalny |
|---|---|---|---:|---:|---|
| 23.09 09:00 — bez ruchu | 1 | **zimny** | 14/17 | 10/17 | `phi4-mini` 8,9 s |
| 23.09 11:00 — bez ruchu | 1 | ciepły | 15/17 | 12/17 | `gemma4:e2b` 4,5 s |
| 23.09 13:00 — bez ruchu | 1 | ciepły | 14/17 | 10/17 | `phi4-mini` 4,5 s |
| 23.09 15:00 — bez ruchu | 1 | ciepły | 14/17 | 11/17 | `phi4-mini` 4,8 s |
| 23.09 17:00 — Modivo +2,30% ▲ | 1 | ciepły | 9/17 | 7/17 | `granite4:7b-a1b-h` 5,9 s |
| 18.09 15:00 — Grupa Polsat −2,63% ▼ | 2 | **zimny** | 10/17 | 8/17 | `phi4-mini` 10,6 s |
| 18.09 23:27 — Grupa Polsat −5,34% ▼ | 2 | ciepły | 10/17 | 8/17 | `granite4:7b-a1b-h` 4,8 s |
| 21.09 11:00 — cztery spółki ▲ | 2 | ciepły | 6/17 | 3/17 | `granite4.2:3b` 12,9 s |
| 19.10 09:00 — bez ruchu | 2 | ciepły | 15/17 | 10/17 | `gemma4:e2b` 4,0 s |
| 19.10 11:00 — bez ruchu, KGHM −1,97% | 2 | ciepły | 14/17 | 10/17 | `gemma4:e2b` 4,0 s |
| 19.10 13:00 — XTB −2,12% ▼ | 2 | ciepły | 12/17 | 8/17 | `qwen3.5:2b` 6,7 s |

Czas to średnia z czterech przebiegów. Ostatnia kolumna dotyczy tylko danego scenariusza — w cyklach bez ruchu najszybciej piszą poprawnie także modele, które przy ruchu zmyślają.

## Test narzędzi

Wywołania to sześć prób: dwa pytania po trzy razy. Pętla to trzy próby pełnego obiegu — wywołanie, wykonanie, odpowiedź z liczbą 24 160 zł. Czasy są średnimi z prób; pierwsza próba każdego modelu startowała na zimno. Pogrubienie oznacza komplet. Opis testu w rozdziale 4.6.

| Model | Wywołania (z 6) | Pętla (z 3) | Czas wywołania | Czas pętli | Uwagi |
|---|:---:|:---:|---:|---:|---|
| `llama3.2:3b` | **6** | **3** | 1,9 s | 4,8 s | — |
| `granite4:7b-a1b-h` | **6** | **3** | 2,4 s | 4,9 s | — |
| `granite4.1:3b` | **6** | **3** | 2,2 s | 5,7 s | — |
| `llama3.1:8b` | **6** | **3** | 4,2 s | 9,9 s | — |
| `qwen3.5:2b` | **6** | **3** | 5,2 s | 12,4 s | — |
| `granite4.1:8b` | **6** | **3** | 5,1 s | 14,4 s | datę wymyśla: „z 5 dni temu (2025-08-29)” (3/3) |
| `qwen3.5:4b` | **6** | **3** | 8,4 s | 19,7 s | — |
| `gemma4:e2b` | **6** | **3** | 8,8 s | 21,4 s | — |
| `granite4.2:3b` | **6** | **3** | 9,0 s | 27,8 s | — |
| `nemotron-3-nano:4b` | **6** | **3** | 9,4 s | 36,4 s | — |
| `granite4.2:8b` | **6** | **3** | 20,6 s | 49,7 s | — |
| `bielik-minitron-7B` | 4 | 3 | 5,2 s | 12,4 s | dwa razy zapisuje wywołanie tekstem (`<tool_call>`) zamiast je wykonać |
| `ministral-3:3b` | 5 | 0 | 7,1 s | 7,7 s | raz o listę spółek odpowiada bez wywołania; w pętli kurs 24 160 podaje jako „241,60 PLN” i datę „19 października 2026” |
| `phi4-mini` | 0 | 0 | 5,9 s | 5,1 s | opisuje wywołanie słowami, zamiast je wykonać |
| `falcon3:7b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |
| `exaone3.5:7.8b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |
| `falcon3:3b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |

## Komentarz

**Grupę drugą wygrywa `granite4.1:8b` — jako jedyny w niej przechodzi wszystkie jedenaście scenariuszy.** Liczby są prawdziwe w każdej notatce, także przy czterech spółkach naraz i przy obu spadkach Polsatu. Wady są w formie: w cyklach bez ruchu wbrew zakazowi wypisuje wszystkie kursy, i to z kropką dziesiętną, a w zestawie drugim zdarza mu się „spolki” bez ogonków i „Grupa Polsat wzrosło”. W teście narzędzi ma komplet, ale w pętli za każdym razem wymyśla datę notowania („z 5 dni temu (2025-08-29)”).

**Podium grupy drugiej jest zaskakująco kruche.** Tuż za zwycięzcą, z dziesięcioma scenariuszami na jedenaście, są dwa modele, każdy z inną słabością. `granite4.1:3b` jest z nich najszybszy (9,5 s zimnego startu), ale przy czterech spółkach naraz podaje same kursy, bez procentów. `qwen3.5:4b` w tym samym scenariuszu podaje zmiany w złotych zamiast procentów, i to zmyślone („Modivo o 3,92 PLN”). `gemma4:e2b` ma osiem: w cyklu o 17:00 zapisuje kurs Polsatu 15,965 zł jako „15965.00 PLN”, a wypisując wbrew zakazowi kursy wszystkich spółek, urywa część notatek na limicie długości. Najczystszą polszczyznę w grupie daje `ministral-3:3b`, ale procenty skraca do jednego miejsca po przecinku („−5,3%”), więc jego notatki w trzech scenariuszach nie są konsumowalne. W teście z promptem po angielsku bez wad pisze `llama3.2:3b`, ale po polsku przechodzi tylko cztery scenariusze.

## Notatki modeli — po polsku

Teksty po oczyszczeniu przez `sanitize()`, czyli tak, jak trafiają na pulpit, z liczbą wystąpień na cztery przebiegi. W cyklach z ruchem — wszystkie wersje; w cyklach bez ruchu — najczęstsze. Pełne odpowiedzi, także surowe, są w `data/badanie-*.jsonl`.

### 1. `granite4.1:8b` — miary 11/11, konsumowalne 11/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 3× Modivo wzrosło o 2,30% do 97 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Zmiana kursu Modivo (MDV.WA) wyniosła 2,30%, kurs teraz wynosi 97 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -2.63%, obecnie notowana na 15.93 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Grupa Polsat zmieniła kurs o -2.63%, obecnie notowana jest na 15.93 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Grupa Polsat spadła o 2.63%, kurs teraz wynosi 15.93 PLN. Pozostałe spolki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5.34%, nowy kurs 15.08 PLN. Pozostałe spolki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Grupa Polsat zmieniła kurs o -5.34%, obecny kurs wynosi 15.08 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%. Nie było znanych przyczyn ruchu dla zmienionej spółki.
> 1× Grupa Polsat zmieniła kurs o -5.34%, obecny kurs wynosi 15.08 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Grupa Polsat zmieniła kurs o -5.34%, obecnie notowana na 15.08 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Zmiany kursów: Modivo wzrosło o 3.93% do 91.52 PLN, CD Projekt wzrosło o 3.05% do 243.60 PLN, Grupa Polsat wzrosło o 5.04% do 15.84 PLN, KGHM wzrosło o 3.65% do 349.35 PLN. Pozostałe spolki na liście nie doświadczyły znaczących zmian.
> 1× Zmiany kursów dla spółek z listy wynoszą co najmniej 2%: Modivo wzrosło do 91,52 PLN, czyli o 3,93%. CD Projekt wzrosło do 243,60 PLN, czyli o 3,05%. Grupa Polsat wzrosła do 15,84 PLN, czyli o 5,04%. KGHM wzrosło do 349,35 PLN, czyli o 3,65%. Kursy pozostałych spółek na liście pozostają niezmienione w zakresie znaczących ruchów.
> 1× Zmiany kursów dla spółek z listy wynoszą co najmniej 2%: Modivo wzrosło do 91,52 PLN (+3,93%). CD Projekt wzrosło do 243,60 PLN (+3,05%). Grupa Polsat wzrosło do 15,84 PLN (+5,04%). KGHM wzrosło do 349,35 PLN (+3,65%). Kursy pozostałych spółek na liście pozostają takie same jak wcześniej.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Zmiana kursu XTB (XTB.WA) wyniosła -2.12%, obecny kurs to 143.16 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%. Nie było żadnych innych ruchów wartościowych wymagających informacji.
> 1× Zmiana kursu XTB o -2.12% do 143.16 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Zmiana kursu XTB (XTB.WA) wyniosła -2.12%, obecny kurs to 143.16 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%.
> 1× Zmiana kursu XTB (XTB.WA) wyniosła -2.12%, obecny kurs to 143.16 PLN. Pozostałe spółki na liście nie doświadczyły znaczących zmian kursowych o co najmniej 2%. Nie było znanych przyczyn ruchu dla XTB.

**Cykle bez ruchu, zestaw 1** (12 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Brak spółek z listy zmienił kurs o co najmniej 2%. Aktualne kursy pozostałych spółek są następujące: XTB 149.82 PLN, PKN Orlen 148.62 PLN, Grupa Kęty 1282 PLN, Pepco 43.17 PLN, LPP 24420 PLN, Modivo 96.70 PLN, CD Projekt 246.10 PLN, Grupa Polsat 16.005 PLN, Benefit Systems 5150 PLN, KGHM 347.65 PLN.
> 2× Brak spółek z listy zmienił kurs o co najmniej 2%. Aktualne kursy pozostają takie, jakie zostały podane wcześniej: XTB 149.90 PLN, PKN Orlen 149.54 PLN, Grupa Kęty 1284 PLN, Pepco 44.12 PLN, LPP 24600 PLN, Modivo 96.42 PLN, CD Projekt 247.50 PLN, Grupa Polsat 16.135 PLN, Benefit Systems 5160 PLN, KGHM 347.25 PLN.
> 2× Brak spółek z listy zmienił kurs o co najmniej 2%. Aktualne kursy pozostają takie, jak podano wcześniej: XTB 152.10 PLN, PKN Orlen 148.32 PLN, Grupa Kęty 1262.00 PLN, Pepco 43.15 PLN, LPP 24300.00 PLN, Modivo 94.82 PLN, CD Projekt 245.80 PLN, Grupa Polsat 15.94 PLN, Benefit Systems 5065.00 PLN, KGHM 343.80 PLN.

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Brak spółek z listy zmienił kurs o co najmniej 2%. Aktualne kursy pozostałych spółek są następujące: XTB 146.26 PLN, PKN Orlen 154.40 PLN, Grupa Kęty 1276.00 PLN, Pepco 47.89 PLN, LPP 26480.00 PLN, Modivo 93.80 PLN, CD Projekt 262.00 PLN, Grupa Polsat 18.00 PLN, Benefit Systems 5635.00 PLN, KGHM 321.30 PLN, Rainbow Tours 124.80 PLN.
> 1× Brak spółek na liście zmienił kurs o co najmniej 2%. Aktualne kursy spółek pozostają takie, jak podane wcześniej: XTB 147.82 PLN, PKN Orlen 154.98 PLN, Grupa Kęty 1292 PLN, Pepco 48 PLN, LPP 26400 PLN, Modivo 92.90 PLN, CD Projekt 262.40 PLN, Grupa Polsat 18.30 PLN, Benefit Systems 5730 PLN, KGHM 327.75 PLN, Rainbow Tours 126.70 PLN.
> 1× Brak spółek z listy zmienił kurs o co najmniej 2%. Aktualne kursy spółek pozostają takie, jak podane wcześniej: XTB 146.26 PLN, PKN Orlen 154.40 PLN, Grupa Kęty 1276 PLN, Pepco 47.89 PLN, LPP 26480 PLN, Modivo 93.80 PLN, CD Projekt 262 PLN, Grupa Polsat 18 PLN, Benefit Systems 5635 PLN, KGHM 321.30 PLN, Rainbow Tours 124.80 PLN.

### 2. `granite4.1:3b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Modivo zwiększył swój kurs o 2.30% do 97 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 3× Grupa Polsat (15.93 PLN) spadło o 2,63%.
> 1× Grupa Polsat (15.93 PLN) zmieniła się o -2.63% od ostatniego sprawdzenia.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 3× Grupa Polsat spadł do 15.08 PLN, co wynosi -5.34% od ostatniego sprawdzenia.
> 1× Grupa Polsat spodniła się o 5.34% do 15.08 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo: 91.52 PLN, CD Projekt: 243.60 PLN, Grupa Polsat: 15.84 PLN, KGHM: 349.35 PLN.
> 1× Modivo: 91.52 PLN, zmiana +3.93%; CD Projekt: 243.60 PLN, zmiana +3.05%; Grupa Polsat: 15.84 PLN, zmiana +5.04%; KGHM: 349.35 PLN, zmiana +3.65%.
> 1× Modivo: 91.52 PLN (+3.93%); CD Projekt: 243.60 PLN (+3.05%); Grupa Polsat: 15.84 PLN (+5.04%); KGHM: 349.35 PLN (+3.65%).

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB spodnięło się o 2.12% do 143.16 PLN.
> 1× XTB spodniło się o 2.12% do 143.16 PLN.

**Cykle bez ruchu, zestaw 1** (5 różnych wersji na 16 notatek, trzy najczęstsze):

> 6× Brak zmian w cenie dla żadnej spółki na liście.
> 5× Brak zmian w cenie dla wszystkich spółek na liście.
> 3× Brak zmian w kursach spółek na liście.

**Cykle bez ruchu, zestaw 2** (3 różnych wersji na 8 notatek):

> 4× Brak zmian w cenie dla żadnej spółki na liście.
> 3× Brak zmian w kursach spółek na liście.
> 1× Brak zmian w cenie dla wszystkich spółek na liście.

### 3. `qwen3.5:4b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Ruszyła się spółka Modivo o 2,30%, a jej obecny kurs wynosi 97,00 PLN. Pozostałe spółki z Twojej listy nie wykazały znaczących zmian ceny w porównaniu do ostatniego sprawdzenia. Nie znając przyczyny takiego ruchu, nie mogę podać dodatkowych informacji.
> 1× Ruszyła się spółka Modivo o 2,30%, a jej obecny kurs wynosi 97,00 PLN. Pozostałe spółki z Twojej listy nie wykazały znaczących zmian w cenie od ostatniego sprawdzenia. Nie znając przyczyny takiego ruchu, nie mogę podać dodatkowych informacji.
> 1× Ruszyła się spółka Modivo o 2,30%, a jej aktualny kurs wynosi 97,00 PLN. Pozostałe spółki z Twojej listy nie wykazały znaczących zmian ceny w porównaniu do ostatniego sprawdzenia. Nie znając przyczyny takiego ruchu, nie mogę podać dodatkowych informacji o sytuacji rynkowej.
> 1× Ruszyła się spółka Modivo o 2,30%, a jej obecny kurs wynosi 97,00 PLN. Pozostałe spółki z Twojej listy nie wykazały znaczących zmian w cenie od ostatniego sprawdzenia. Powód tego ruchu pozostaje niezidentyfikowany.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Ruszyła się Grupa Polsat o 2,63%, a jej kurs wynosi obecnie 15,93 PLN. Pozostałe spółki z listy pozostają bez znaczących zmian w porównaniu do poprzedniego sprawdzenia. Powiadomienie zostało wygenerowane zgodnie z dostarczonymi danymi rynkowymi.
> 1× Ruszyła się Grupa Polsat o 2,63%, a jej kurs wynosi obecnie 15,93 PLN. Pozostałe spółki z listy nie wykazały znaczących zmian w stosunku do poprzedniego sprawdzenia. Powiadomienie służy wyłącznie celowi informacyjnym i nie stanowi rekomendacji inwestycyjnej.
> 1× Ruszyła się Grupa Polsat o 2,63%, a jej kurs wynosi obecnie 15,93 PLN. Pozostałe spółki z listy pozostają bez zmian znaczących. Nie ma informacji o przyczynie takiego ruchu.
> 1× Ruchnoty kursu co najmniej 2% odnotowano u Grupy Polsat, której cena spadła do 15,93 PLN o 2,63%. Pozostałe spółki z listy pozostają bez znaczących zmian.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Ruchnoty kursu co najmniej 2% odnotowano u Grupy Polsat, której cena spadła do 15.08 PLN o 5.34%. Pozostałe spółki z listy utrzymały się na poziomie: XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269 PLN, Pepco 41.87 PLN, LPP 24160 PLN, Modivo 88.06 PLN, CD Projekt 236.4 PLN, Benefit Systems 5135 PLN oraz KGHM 337.05 PLN. Nie podano informacji o przyczynach tego ruchu.
> 1× Ruszyła się Grupa Polsat, spadając o 5,34% do poziomu 15,08 PLN. Pozostałe spółki z listy utrzymały swoje ceny bez znaczących zmian. Powiadomienie zostało wygenerowane zgodnie z parametrami monitoringu.
> 1× Ruchnoty kursu co najmniej 2% odnotowano u Grupy Polsat, której cena spadła do 15.08 PLN o 5.34%. Inne spółki z listy pozostały bez znaczących zmian cenowych.
> 1× Ruchnoty w notowaniach: Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. W pozostałej części listy spółek nie stwierdzono zmian przekraczających próg 2%. Powiadomienie służy wyłącznie celowi informacyjnym i nie stanowi rekomendacji inwestycyjnej.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Ruszyły Modivo o 3,92 PLN do 91,52 PLN, CD Projekt o 7,34 PLN do 243,60 PLN, Grupa Polsat o 0,78 PLN do 15,84 PLN oraz KGHM o 12,67 PLN do 349,35 PLN.
> 1× Ruszyły Modivo (91.52 PLN), CD Projekt (243.60 PLN), Grupa Polsat (15.84 PLN) oraz KGHM (349.35 PLN). Zmiany wyniosły odpowiednio 3.93%, 3.05%, 5.04% i 3.65%. Nie znaleziono informacji o przyczynach tych ruchów.
> 1× Ruchnoty spółki Modivo (91,52 PLN), CD Projekt (243,60 PLN), Grupa Polsat (15,84 PLN) oraz KGHM (349,35 PLN) odnotowały zmiany powyżej progu 2%. Wszelkie przyczyny tych ruchów pozostają nieznane.
> 1× Ruszyły Modivo (91,52 PLN), CD Projekt (243,60 PLN), Grupa Polsat (15,84 PLN) oraz KGHM (349,35 PLN). Wszystkie wymienione spółki odnotowały wzrosty powyżej progu 2% względem poprzedniego sprawdzenia. Nie podano informacji o przyczynach tych ruchów na rynku.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Ruszyła się spółka XTB o 2,12%, a jej obecny kurs wynosi 143,16 PLN. Pozostałe spółki z Twojej listy nie wykazały istotnych zmian cenowych w porównaniu do ostatniego sprawdzenia.
> 1× Ruszyła się spółka XTB o 2,12%, jej obecny kurs to 143,16 PLN. Pozostałe spółki z Twojej listy nie wykazały istotnych zmian w cenie względem poprzedniego sprawdzenia.
> 1× Ruszyła się spółka XTB, której kurs spadł do 143.16 PLN o 2.12% względem poprzedniego sprawdzenia. Inne spółki z listy pozostają bez znaczących zmian. Powiadomienie nie zawiera rekomendacji inwestycyjnych ani przyczyn ruchu cen.
> 1× Ruszyła się spółka XTB o 2,12%, a jej obecny kurs wynosi 143,16 PLN. Pozostałe spółki z listy utrzymały się na poziomie: PKN Orlen 155,02 PLN, Grupa Kęty 1277 PLN, Pepco 47,45 PLN, LPP 26400 PLN, Modivo 93,26 PLN, CD Projekt 259,8 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615 PLN, KGHM 322,45 PLN oraz Rainbow Tours 125,1 PLN.

**Cykle bez ruchu, zestaw 1** (14 różnych wersji na 16 notatek, trzy najczęstsze):

> 2× Brak spółek z listy, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Aktualne ceny spółek na GPW wynoszą: XTB 149.82 PLN, PKN Orlen 148.62 PLN, Grupa Kęty 1282 PLN, Pepco 43.17 PLN, LPP 24420 PLN, Modivo 96.7 PLN, CD Projekt 246.1 PLN, Grupa Polsat 16.005 PLN, Benefit Systems 5150 PLN oraz KGHM 347.65 PLN.
> 2× Brak spółek na liście, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Wszystkie podane poniżej są aktualnymi poziomami cen: XTB 152.10 PLN, PKN Orlen 148.32 PLN, Grupa Kęty 1262.00 PLN, Pepco 43.15 PLN, LPP 24300.00 PLN, Modivo 94.82 PLN, CD Projekt 245.80 PLN, Grupa Polsat 15.94 PLN, Benefit Systems 5065.00 PLN i KGHM 343.80 PLN.
> 1× Brak spółek ze zmianą kursu co najmniej 2%. Aktualne poziomy cen to: XTB 149,90 PLN, PKN Orlen 149,54 PLN, Grupa Kęty 1284,00 PLN, Pepco 44,12 PLN, LPP 24600,00 PLN, Modivo 96,42 PLN, CD Projekt 247,50 PLN, Grupa Polsat 16,14 PLN, Benefit Systems 5160,00 PLN i KGHM 347,25 PLN.

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× Brak spółek na liście, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Wszystkie podane poniżej są aktualnymi poziomami cen bez wskazania ich kierunku: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292 PLN, Pepco: 48 PLN, LPP: 26400 PLN, Modivo: 92.9 PLN, CD Projekt: 262.4 PLN, Grupa Polsat: 18.3 PLN, Benefit Systems: 5730 PLN, KGHM: 327.75 PLN, Rainbow Tours: 126.7 PLN.
> 1× Brak spółek ze zmianą kursu co najmniej 2%. Aktualne ceny: XTB 146,26 PLN, PKN Orlen 154,40 PLN, Grupa Kęty 1276,00 PLN, Pepco 47,89 PLN, LPP 26480,00 PLN, Modivo 93,80 PLN, CD Projekt 262,00 PLN, Grupa Polsat 18,00 PLN, Benefit Systems 5635,00 PLN, KGHM 321,30 PLN, Rainbow Tours 124,80 PLN.
> 1× Brak spółek z Twojej listy, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Wszystkie pozostałe pozostają na stabilnym poziomie bez znaczących ruchów. Nie ma informacji o przyczynach ewentualnych zmian kursu.

### 4. `granite4:7b-a1b-h` — miary 10/11, konsumowalne 9/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Modivo (MDV.WA): 97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA): 15.93 PLN, -2.63% względem poprzedniego sprawdzenia.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA): 15.08 PLN, -5.34% względem poprzedniego sprawdzenia.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Na pulpicie zgłaszam zmiany kursowe dla następujących spółek: Modivo (MDV.WA) wzrosło o 3.93% do 91.52 PLN, CD Projekt (CDR.WA) zwiększyło się o 3.05% do 243.6 PLN, a Grupa Polsat (CPS.WA) podniosła kurs o 5.04% do 15.84 PLN.
> 1× Na pulpicie zgłaszam zmiany kursowe dla następujących spółek: Modivo wzrosło o 3.93% do 91.52 PLN, CD Projekt wzyskało 3.05% do 243.6 PLN, a Grupa Polsat podnieła kurs o 5.04% do 15.84 PLN.
> 1× Modivo (MDV.WA): 91.52 PLN, wzrost o 3.93% względem poprzedniego sprawdzenia. CD Projekt (CDR.WA): 243.6 PLN, wzrost o 3.05% względem poprzedniego sprawdzenia. Grupa Polsat (CPS.WA): 15.84 PLN, wzrost o 5.04% względem poprzedniego sprawdzenia.
> 1× Modivo (MDV.WA): 91.52 PLN, wzrost o 3.93% względem poprzedniego sprawdzenia. CD Projekt (CDR.WA): 243.6 PLN, wzrost o 3.05% względem poprzedniego sprawdzenia. Grupa Polsat (CPS.WA): 15.84 PLN, wzrost o 5.04% względem poprzedniego sprawdzenia. KGHM (KGH.WA): 349.35 PLN, wzrost o 3.65% względem poprzedniego sprawdzenia.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB: 143.16 PLN, -2.12% względem poprzedniego sprawdzenia Aktualne kursy spółek z listy: XTB: 143.16 PLN PKN Orlen: 155.02 PLN Grupa Kęty: 1277 PLN Pepco: 47.45 PLN LPP: 26400 PLN Modivo: 93.26 PLN CD Projekt: 259.8 PLN Grupa Polsat: 17.72 PLN Benefit Systems: 5615 PLN KGHM: 322.45 PLN Rainbow Tours: 125.1 PLN
> 1× XTB (143.16 PLN) zmieniło kurs o -2.12%.
> 1× XTB: 143.16 PLN, -2.12% względem poprzedniego sprawdzenia Aktualne kursy spółek z listy (to POZIOMY CEN, nie zmiany; wszystkie poniższe nazwy to spółki, żadna nie jest giełdą ani miejscem notowań): XTB: 143.16 PLN PKN Orlen: 155.02 PLN Grupa Kęty: 1277 PLN Pepco: 47.45 PLN LPP: 26400 PLN Modivo: 93.26 PLN CD Projekt: 259.8 PLN Grupa Polsat: 17.72 PLN Benefit Systems: 5615 PLN

**Cykle bez ruchu, zestaw 1** (3 różnych wersji na 16 notatek):

> 12× Brak znaczących zmian w kursach spółek z listy.
> 3× Brak znaczących zmian na liście spółek, które zmieniły kurs o co najmniej 2%.
> 1× Brak znaczących zmian na liście spółek.

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 4× Brak znaczących zmian w kursach spółek z listy.
> 1× Nie ma znaczących zmian w kursach spółek z listy.
> 1× Brak znaczących zmian w kursach spółek na liście.

### 5. `phi4-mini` — miary 8/11, konsumowalne 8/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 3× Modivo zmieniło kurs o 2.30%, zmniejszając się do 97,00 PLN.
> 1× Modivo zmieniło kurs o 2.30%, zmniejszając się do 97 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat: -2.63%, 15.93 PLN. Powód: Przecięcie.
> 1× Grupa Polsat: -2.63%, 15.93 PLN
> 1× Grupa Polsat: Kurs zmienił się o -2.63%, nowy kurs 15.93 PLN. Powód: Brak informacji.
> 1× Grupa Polsat (CPS.WA): -2.63%, 15.93 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA): -5.33% - 15.08 PLN
> 2× Grupa Polsat (CPS.WA): -5.33%, 15.08 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× PKN Orlen: 152.42 PLN (+1.02%) Spadek. Grupa Kęty: 1261 PLN (+0.03%) Spadek. Pepco: 41.9 PLN (-0.10%) Spadek. LPP: 23960 PLN (+0.05%) Spadek. Modivo: 91.52 PLN (+3.93%) Spadek. CD Projekt: 243.6 PLN (+3.05%) Spadek. Grupa Polsat: 15.84 PLN (+5.04%) Spadek. KGHM: 349.35 PLN (+3.65%) Spadek.
> 1× PKN Orlen: 152.42 PLN (+1.02%) Zwiększenie z 151.42 PLN. Grupa Kęty: 1261 PLN (Brak zmiany) KGHM: 349.35 PLN (+3.65%) Spadek z 353.00 PLN.
> 1× PKN Orlen: 152.42 PLN (+1.02%) Zmienił kurs o 1.02% od poprzedniego sprawdzenia. Powód: Zmiana kursu.
> 1× PKN Orlen: 152.42 PLN (+1.02%) Zmieniła się o 1.02%. Powód: Brak.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 4× XTB: -2.12% - 143.16 PLN PKN Orlen: 155.02 PLN Grupa Kęty: 1277 PLN Pepco: 47.45 PLN LPP: 26400 PLN Modivo: 93.26 PLN CD Projekt: 259.8 PLN Grupa Polsat: 17.72 PLN Benefit Systems: 5615 PLN KGHM: 322.45 PLN Rainbow Tours: 125.1 PLN

**Cykle bez ruchu, zestaw 1** (2 różnych wersji na 16 notatek):

> 11× Nie było znaczących zmian.
> 5× Brak znaczących zmian.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Nie było znaczących zmian.

### 6. `gemma4:e2b` — miary 10/11, konsumowalne 8/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo (MDV.WA) zmieniła kurs o 2.299093018350566% względem poprzedniego sprawdzenia, jej aktualny kurs to 97.00 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to XTB 153.60 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260.00 PLN, Pepco 43.19 PLN, LPP 24400.00 PLN, Modivo 97.00 PLN, CD Projekt 247.40 PLN, Grupa Polsat 15965.00 PLN i Benefit Systems 5
> 1× Modivo (MDV.WA) zmienił kurs o 2.299093018350566% względem poprzedniego sprawdzenia, aktualny kurs to 97.00 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to XTB 153.60 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260.00 PLN, Pepco 43.19 PLN, LPP 24400.00 PLN, Modivo 97.00 PLN, CD Projekt 247.40 PLN, Grupa Polsat 15.965.00 PLN i Benefit Systems 5
> 1× Modivo (MDV.WA) zmienił kurs o 2.299093018350566% względem poprzedniego sprawdzenia, osiągając 97.00 PLN. Aktualne kursy spółek z listy to XTB 153.60 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260.00 PLN, Pepco 43.19 PLN, LPP 24400.00 PLN, Modivo 97.00 PLN, CD Projekt 247.40 PLN, Grupa Polsat 15965.00 PLN i Benefit Systems 5040.00 PLN, KGHM 341.55 PLN.
> 1× Modivo zmienił kurs o 2.299093018350566% względem poprzedniego sprawdzenia, jego aktualny kurs to 97.00 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to XTB 153.60 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260.00 PLN, Pepco 43.19 PLN, LPP 24400.00 PLN, Modivo 97.00 PLN, CD Projekt 247.40 PLN, Grupa Polsat 15.97 PLN, Benefit Systems 5040.00 PLN i K

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -2,63% względem poprzedniego sprawdzenia. Aktualne kursy spółek z listy to XTB 150,68 PLN, PKN Orlen 150,42 PLN, Grupa Kęty 1267,00 PLN, Pepco 42,03 PLN, LPP 24300,00 PLN, Modivo 87,34 PLN, CD Projekt 238,90 PLN, Grupa Polsat 15,93 PLN, Benefit Systems 5160,00 PLN i KGHM 343,35 PLN.
> 1× Grupa Polsat zmieniła kurs o -2,63% do 15,93 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to XTB 150,68 PLN, PKN Orlen 150,42 PLN, Grupa Kęty 1267 PLN, Pepco 42,03 PLN, LPP 24300 PLN, Modivo 87,34 PLN, CD Projekt 238,9 PLN, Grupa Polsat 15,93 PLN, Benefit Systems 5160 PLN i KGHM 343,35 PLN.
> 1× Grupa Polsat zmieniła kurs o -2.63% do 15.93 PLN. Pozostałe spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy to XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267 PLN, Pepco 42.03 PLN, LPP 24300 PLN, Modivo 87.34 PLN, CD Projekt 238.9 PLN, Grupa Polsat 15.93 PLN, Benefit Systems 5160 PLN i KGHM 343.35 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -5.34% względem poprzedniego sprawdzenia. Aktualne kursy spółek z listy to XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269.00 PLN, Pepco 41.87 PLN, LPP 24160.00 PLN, Modivo 88.06 PLN, CD Projekt 236.40 PLN, Grupa Polsat 15.08 PLN, Benefit Systems 5135.00 PLN i KGHM 337.05 PLN.
> 1× Grupa Polsat zmieniła kurs o -5.34% względem poprzedniego sprawdzenia. Aktualne kursy spółek to XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269.00 PLN, Pepco 41.87 PLN, LPP 24160.00 PLN, Modivo 88.06 PLN, CD Projekt 236.40 PLN, Grupa Polsat 15.08 PLN, Benefit Systems 5135.00 PLN i KGHM 337.05 PLN.
> 1× Grupa Polsat zmieniła kurs o -5.34% względem poprzedniego sprawdzenia. Aktualne kursy spółek to XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269 PLN, Pepco 41.87 PLN, LPP 24160 PLN, Modivo 88.06 PLN, CD Projekt 236.4 PLN, Grupa Polsat 15.08 PLN, Benefit Systems 5135 PLN i KGHM 337.05 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Modivo, CD Projekt, Grupa Polsat i KGHM odnotowały zmiany kursowe o co najmniej 2%. Modivo zmieniło kurs o 3.93%, CD Projekt o 3.05%, Grupa Polsat o 5.04% oraz KGHM o 3.65%. Aktualne kursy tych spółek to Modivo 91.52 PLN, CD Projekt 243.60 PLN, Grupa Polsat 15.84 PLN i KGHM 349.35 PLN.
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM odnotowały zmiany kursowe powyżej 2%. Modivo zmieniło kurs o 3,93%, CD Projekt o 3,05%, Grupa Polsat o 5,04% oraz KGHM o 3,65%. Przyczyn ruchu nie są znane. Aktualne kursy spółek z listy to XTB 149,84 PLN, PKN Orlen 152,42 PLN, Grupa Kęty 1261 PLN, Pepco 41,90 PLN, LPP 23960 PLN, Modivo 91,52 PLN, CD Projekt 243,60 PLN, Grupa Polsat 15,84 PLN i KGHM 34
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM odnotowały zmiany kursowe przekraczające 2%. Modivo zmieniło kurs o 3.93%, CD Projekt o 3.05%, Grupa Polsat o 5.04% oraz KGHM o 3.65%. Aktualne kursy tych spółek to Modivo 91.52 PLN, CD Projekt 243.60 PLN, Grupa Polsat 15.84 PLN i KGHM 349.35 PLN.
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM odnotowały zmiany kursowe przekraczające 2% od ostatniego sprawdzenia. Modivo zmieniło kurs o 3,93%, CD Projekt o 3,05%, Grupa Polsat o 5,04% oraz KGHM o 3,65%. Przy kursach aktualnych: Modivo 91,52 PLN, CD Projekt 243,60 PLN, Grupa Polsat 15,84 PLN i KGHM 349,35 PLN.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB zmienił kurs o -2,12% do 143,16 PLN. Pozostałe spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to: XTB: 143,16 PLN, PKN Orlen: 155,02 PLN, Grupa Kęty: 1277,00 PLN, Pepco: 47,45 PLN, LPP: 26400,00 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems: 5615,00 PLN, KGHM: 322,45 PLN,
> 1× XTB zmienił kurs o -2,12% i jego aktualny kurs to 143,16 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to: XTB: 143,16 PLN, PKN Orlen: 155,02 PLN, Grupa Kęty: 1277,00 PLN, Pepco: 47,45 PLN, LPP: 26400,00 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems: 5615,00 PLN, KGHM: 322,
> 1× XTB zmienił kurs o -2,12% do 143,16 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to: XTB 143,16 PLN, PKN Orlen 155,02 PLN, Grupa Kęty 1277,00 PLN, Pepco 47,45 PLN, LPP 26400,00 PLN, Modivo 93,26 PLN, CD Projekt 259,80 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615,00 PLN, KGHM 322,45 PLN, Rainbow Tours 125,10 PLN.
> 1× XTB zmienił kurs o -2,12% i jego aktualny poziom to 143,16 PLN. Inne spółki z listy nie wykazały zmian kursu o co najmniej 2%. Aktualne kursy spółek to: XTB 143,16 PLN, PKN Orlen 155,02 PLN, Grupa Kęty 1277,00 PLN, Pepco 47,45 PLN, LPP 26400,00 PLN, Modivo 93,26 PLN, CD Projekt 259,80 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615,00 PLN, KGHM 322,45 PLN i Rainbow Tours 125

**Cykle bez ruchu, zestaw 1** (2 różnych wersji na 16 notatek):

> 15× Nie było znaczących zmian w kursach spółek z listy.
> 1× Nie było znaczących zmian w kursach spółek z listy. Aktualne kursy to XTB: 152.10 PLN, PKN Orlen: 148.32 PLN, Grupa Kęty: 1262.00 PLN, Pepco: 43.15 PLN, LPP: 24300.00 PLN, Modivo: 94.82 PLN, CD Projekt: 245.80 PLN, Grupa Polsat: 15.94 PLN i Benefit Systems: 5065.00 PLN, KGHM: 343.80 PLN.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Nie było znaczących zmian w kursach spółek z listy.

### 7. `ministral-3:3b` — miary 10/11, konsumowalne 8/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Modivo (MDV.WA) zmieniło kurs o 2,30% na 97,00 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o –2,6% na poziomie 15,93 PLN.
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.
> 1× Grupa Polsat notowana jest na poziomie 15,93 PLN i zmieniła kurs o –2,63% względem poprzedniego sprawdzenia.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Na liście spółek z GPW zmieniła się cena Grupy Polsat o –5,3% do 15,08 PLN.
> 1× Na liście spółek z GPW zmienił się kurs Grupy Polsat o –5,3% do 15,08 PLN.
> 1× Na liście spółek z GPW zmieniła się cena Grupy Polsat na 15,08 PLN (-5,34%). Inne kursy: XTB – 150,34 PLN PKN Orlen – 151,14 PLN Grupa Kęty – 1269,00 PLN Pepco – 41,87 PLN LPP – 24160,00 PLN Modivo – 88,06 PLN CD Projekt – 236,40 PLN Benefit Systems – 5135,00 PLN KGHM – 337,05 PLN
> 1× Grupa Polsat notowała spadek o 5,34% do 15,08 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Spółki z listy notowały znaczące zmiany kursów: Modivo – 91,52 PLN, +3,9% CD Projekt – 243,60 PLN, +3,0% Grupa Polsat – 15,84 PLN, +5,0% KGHM – 349,35 PLN, +3,6%
> 1× Spółki z listy notowały znaczące zmiany kursów: Modivo +3,9%, CD Projekt +3,0%, Grupa Polsat +5,0%, KGHM +3,6%. Aktualne kursy: Modivo 91,52 PLN, CD Projekt 243,60 PLN, Grupa Polsat 15,84 PLN, KGHM 349,35 PLN.
> 1× Spółki z listy notowały znaczące zmiany kursów: Modivo (+3,93%), CD Projekt (+3,05%), Grupa Polsat (+5,04%), KGHM (+3,65). Aktualne kursy: Modivo 91,52, CD Projekt 243,60, Grupa Polsat 15,84, KGHM 349,35.
> 1× Spółki z listy notowały znaczące zmiany kursów: Modivo (+3,93%), CD Projekt (+3,05%), Grupa Polsat (+5,04%), KGHM (+3,65%). Aktualne kursy: Modivo – 91,52 PLN, CD Projekt – 243,60 PLN, Grupa Polsat – 15,84 PLN, KGHM – 349,35 PLN.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB spadło o 2,12%. Aktualny kurs: 143,16 PLN.
> 1× XTB spadło o 2,12%. Aktualny kurs: 143,16 PLN. Inne spółki nie zmieniły kursu o co najmniej 2%.

**Cykle bez ruchu, zestaw 1** (jedna wersja na 16 notatek):

> 16× Brak spółek z listy zmieniło kurs o co najmniej 2%.

**Cykle bez ruchu, zestaw 2** (3 różnych wersji na 8 notatek):

> 5× Brak spółek z listy zmieniło kurs o co najmniej 2%.
> 2× Brak znaczących zmian kursowych na ostatnim sprawozdaniu.
> 1× Brak znaczących zmian kursowych na ostatnim sprawdzeniu.

### 8. `granite4.2:8b` — miary 10/11, konsumowalne 7/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo zmieniła kurs o co najmniej 2% względem poprzedniego sprawdzenia. Kurs Modivo wynosi 97 PLN. Nie znana jest przyczyna zmiany kursu. Nie zostały podane inne zmiany spółek.
> 1× Modivo zmieniła kurs o co najmniej 2% względem poprzedniego sprawdzenia. Kurs Modivo wynosi 97 PLN. Nie znana jest przyczyna zmiany kursu. Tylko kurs Modivo podano zgodnie z wymogiem.
> 1× Modivo zmieniła kurs o co najmniej 2% względem poprzedniego sprawdzenia. Kurs Modivo wynosi 97 PLN. Nie znana jest przyczyna zmiany kursu. Tytuły spółek na liście nie są przedmiotem rekomendacji inwestycyjnej.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Spółka Grupa Polsat zmieniła kurs o -2,63% względem poprzedniego sprawdzenia. Kurs Grupy Polsat wynosi 15,93 PLN. Nie znana jest przyczyna zmiany kursu. Nie zostały podane inne zmiany spółek o co najmniej 2%.
> 1× Zmienił się kurs Grupy Polsat o -2,63% względem poprzedniego sprawdzenia. Podany kurs Grupy Polsat wynosi 15,93 PLN. Nie znana jest przyczyna zmiany kursu. Nie zostały podane inne zmiany spółek o co najmniej 2%.
> 1× Spółka Grupa Polsat zmieniła kurs o -2,63% względem poprzedniego sprawdzenia. Podany kurs Grupy Polsat wynosi 15,93 PLN. Nie znana jest przyczyna zmiany kursu. Nie zostały podane inne zmiany spółek o co najmniej 2%.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Spółka Grupa Polsat zmieniła kurs o co najmniej 2% względem poprzedniego sprawdzenia, wynikając z zmiany kursu na 15.08 PLN. Nie znana jest przyczyna tego ruchu. Podane są tylko kursy zmienionej spółki: Grupa Polsat – 15.08 PLN.
> 1× Spółka Grupa Polsat zmieniła kurs o co najmniej 2% względem poprzedniego sprawdzenia, wynikając z zmiany kursu na 15.08 PLN. Nie znana jest przyczyna tego ruchu. Podane są tylko kursy spółki Grupa Polsat: 15.08 PLN. Nie było znaczących zmian u innych spółek na liście.
> 1× Grupa Polsat zmieniła kurs o -5,34% względem poprzedniego sprawdzenia. Podano aktualny kurs Grupy Polsat: 15,08 PLN. Nie znana jest przyczyna zmiany kursu. Nie zostały podane inne zmiany spółek ani inne szczegóły.
> 1× Spółka Grupa Polsat zmieniła kurs o co najmniej 2% względem poprzedniego sprawdzenia. Kurs Grupy Polsat wynosi 15.08 PLN. Nie znana jest przyczyna zmiany kursu. Informacja nie stanowi rekomendacji inwestycyjnej.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Zmienił się kurs Modivo o 3,93%, Grupy Polsat o 5,04%, CD Projekt o 3,05% i KGHM o 3,65%. Wszystkie zmiany wynosiły co najmniej 2%. Podane kursy dotyczą aktualnych cen spółek: Modivo 91.52 PLN, Grupa Polsat 15.84 PLN, CD Projekt 243.60 PLN, KGHM 349.35 PLN.
> 1× Zmienił się kurs Modivo o 3,93% względem poprzedniego sprawdzenia, kurs wynosi 91,52 PLN. Zmienił się kurs CD Projekt o 3,05% względem poprzedniego sprawdzenia, kurs wynosi 243,60 PLN. Zmienił się kurs Grupy Polsat o 5,04% względem poprzedniego sprawdzenia, kurs wynosi 15,84 PLN. Zmienił się kurs KGHM o 3,65% względem poprzedniego sprawdzenia, kurs wynosi 349,35 PLN. Nie znane są przyczyny tych zmian.
> 1× Zmienił się kurs Modivo o 3,93%, Grupy Polsat o 5,04%, CD Projekt o 3,05% i KGHM o 3,65%. Wszystkie zmiany wyniosły co najmniej 2%. Podane kursy dotyczą aktualnych cen spółek: Modivo 91.52 PLN, Grupa Polsat 15.84 PLN, CD Projekt 243.60 PLN, KGHM 349.35 PLN.
> 1× Zmienił się kurs Modivo o 3,93%, Grupy Polsat o 5,04%, CD Projekt o 3,05% i KGHM o 3,65% względem poprzedniego sprawdzenia. Modivo: 91.52 PLN, Grupa Polsat: 15.84 PLN, CD Projekt: 243.60 PLN, KGHM: 349.35 PLN. Nie znana jest przyczyna zmian kursów. Nie było znaczących zmian poza wymienionymi spółkami.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Zmienił się kurs XTB o -2.12%. XTB: 143.16 PLN. Spółki z listy nie zostały zmienione w sposób sugerujący wzrost ani spadek poziomu cen; kursy podane są aktualne. Nie znana jest przyczyna zmiany kursu XTB. Nie było znaczących zmian poziomu cen dla innych spółek.
> 1× XTB zmienił kurs o -2.12%. XTB: 143.16 PLN.
> 1× XTB zmienił kurs o -2.12%. Kurs XTB wynosi 143.16 PLN. Inne spółki na liście nie zmieniły kursu o co najmniej 2%. Nie znana przyczyna zmiany kursu XTB.
> 1× XTB zmienił kurs o -2.12%. XTB: 143.16 PLN. Spółki z listy nie zostały zmienione w wyniku tego ruchu. Nie znana jest przyczyna zmiany kursu XTB. Kursy spółek podane są aktualnie.

**Cykle bez ruchu, zestaw 1** (11 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian kursów o co najmniej 2%. Lista spółek, które zmieniły kurs o co najmniej 2%, jest pusta (brak). Aktualne kursy spółek z listy nie są wymieniane w tej notce.
> 3× Nie było znaczących zmian kursów o co najmniej 2%. Lista spółek, które zmieniły kurs o co najmniej 2%, jest pusta (brak). Aktualne kursy spółek z listy są podane wyłącznie do informacji: XTB: 152.10 PLN, PKN Orlen: 148.32 PLN, Grupa Kęty: 1262.00 PLN, Pepco: 43.15 PLN, LPP: 24300.00 PLN, Modivo: 94.82 PLN, CD Projekt: 245.80 PLN, Grupa Polsat: 15.94 PLN, Benefit Systems: 5065.00 PLN, KGHM: 343.80 PLN.
> 2× brak

**Cykle bez ruchu, zestaw 2** (6 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian kursów o co najmniej 2% od ostatniego sprawdzenia. Lista spółek, które zmieniły kurs o co najmniej 2%, jest pusta (brak). Aktualne kursy spółek z listy nie zostały zmienione w ramach tego powiadomienia.
> 1× Nie było znaczących zmian kursów o co najmniej 2% od ostatniego sprawdzenia. Lista spółek, które zmieniły kurs o co najmniej 2%, jest pusta (brak). Aktualne kursy podane są jedynie do informacji: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292 PLN, Pepco: 48 PLN, LPP: 26400 PLN, Modivo: 92.90 PLN, CD Projekt: 262.40 PLN, Grupa Polsat: 18.30 PLN, Benefit Systems: 5730 PLN, KGHM: 327.75 PLN, Rainbow Tours: 126.70 PLN.
> 1× Nie było znaczących zmian kursów o co najmniej 2% od ostatniego sprawdzenia. Lista spółek, które zmieniły kurs o co najmniej 2%, jest pusta. Aktualne kursy spółek z listy nie zostały podane w ramach informacji o zmianach.

### 9. `bielik-minitron-7B` — miary 8/11, konsumowalne 7/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Modivo (MDV.WA) wzrosło o 2.29% do 97 PLN. Pozostałe spółki nie wykazały znaczących zmian.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat odnotowała wzrost kursu o 2,63% do poziomu 15,93 PLN. Pozostałe spółki z listy nie wykazały znaczących zmian kursów.
> 2× Grupa Polsat odnotowała spadek kursu o 2,63% do poziomu 15,93 PLN. Pozostałe spółki z listy nie wykazały znaczących zmian kursów.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 4× Grupa Polsat odnotowała spadek kursu o 5,34% do poziomu 15,08 PLN. Pozostałe spółki z listy nie wykazały znaczących zmian kursów.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 4× Spółki, które zmieniły kurs o co najmniej 2%: Modivo (MDV.WA): 91.52 PLN, +3.93% CD Projekt (CDR.WA): 243.6 PLN, +3.05% Grupa Polsat (CPS.WA): 15.84 PLN, +5.04% KGHM (KGH.WA): 349.35 PLN, +3.65% Aktualne kursy pozostałych spółek: XTB: 149.84 PLN PKN Orlen: 152.42 PLN Grupa Kęty: 12

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 4× XTB wzrosło o 2.12% do 143.16 PLN. Pozostałe spółki nie odnotowały znaczących zmian.

**Cykle bez ruchu, zestaw 1** (6 różnych wersji na 16 notatek, trzy najczęstsze):

> 4× Nie było znaczących zmian kursów spółek. Aktualne kursy: XTB: 149.90 PLN, PKN Orlen: 149.54 PLN, Grupa Kęty: 1284 PLN, Pepco: 44.12 PLN, LPP: 24600 PLN, Modivo: 96.42 PLN, CD Projekt: 247.50 PLN, Grupa Polsat: 16.135 PLN, Benefit Systems: 5160 PLN, KGHM: 347.25 PLN.
> 4× Nie było znaczących zmian kursów spółek. Aktualne kursy: XTB: 151.02 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1288 PLN, Pepco: 43.47 PLN, LPP: 24620 PLN, Modivo: 96.3 PLN, CD Projekt: 245.3 PLN, Grupa Polsat: 16.105 PLN, Benefit Systems: 5120 PLN, KGHM: 348.35 PLN.
> 3× Nie było znaczących zmian kursów spółek. Aktualne kursy: XTB: 152.1 PLN, PKN Orlen: 148.32 PLN, Grupa Kęty: 1262 PLN, Pepco: 43.15 PLN, LPP: 24300 PLN, Modivo: 94.82 PLN, CD Projekt: 245.8 PLN, Grupa Polsat: 15.94 PLN, Benefit Systems: 5065 PLN, KGHM: 343.8 PLN.

**Cykle bez ruchu, zestaw 2** (3 różnych wersji na 8 notatek):

> 4× Nie było znaczących zmian kursów spółek. Aktualne kursy: XTB: 146.26 PLN, PKN Orlen: 154.4 PLN, Grupa Kęty: 1276 PLN, Pepco: 47.89 PLN, LPP: 26480 PLN, Modivo: 93.8 PLN, CD Projekt: 262 PLN, Grupa Polsat: 18 PLN, Benefit Systems: 5635 PLN, KGHM: 321.3 PLN, Rainbow Tours: 124.8 PLN.
> 2× Brak znaczących zmian kursów spółek.
> 2× Nie było znaczących zmian kursów spółek. Aktualne kursy: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292 PLN, Pepco: 48 PLN, LPP: 26400 PLN, Modivo: 92.9 PLN, CD Projekt: 262.4 PLN, Grupa Polsat: 18.3 PLN, Benefit Systems: 5730 PLN, KGHM: 327.75 PLN, Rainbow Tours: 126.7 PLN.

### 10. `llama3.1:8b` — miary 6/11, konsumowalne 6/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo zmieniło kurs o 2.299093018350566% od ostatniego sprawdzenia. Modivo teraz kosztuje 97 PLN.
> 1× Brak znaczących zmian na liście spółek.
> 1× Brak znaczących zmian.
> 1× Modivo zmieniło kurs o 2.299093018350566% względem poprzedniego sprawdzenia. Modivo teraz kosztuje 97 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 4× Brak znaczących zmian.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 4× Brak znaczących zmian.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Nie było znaczących zmian.
> 1× Brak znaczących zmian.
> 1× Brak znaczących zmian na liście spółek.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× Brak znaczących zmian.
> 1× Brak znaczących zmian na liście spółek.
> 1× Brak znaczących zmian w kursach spółek.

**Cykle bez ruchu, zestaw 1** (3 różnych wersji na 16 notatek):

> 11× Nie było znaczących zmian.
> 4× Brak znaczących zmian.
> 1× Brak.

**Cykle bez ruchu, zestaw 2** (4 różnych wersji na 8 notatek, trzy najczęstsze):

> 4× Brak znaczących zmian.
> 2× Nie było znaczących zmian.
> 1× Brak znaczących zmian w kursach spółek z listy.

### 11. `llama3.2:3b` — miary 5/11, konsumowalne 4/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Na pulpicie: Zmieniły się kursy spółek na liście. Modivo: 97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia.
> 1× Najnowsze zmiany w kursach spółek na liście. Modivo: 97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia.
> 1× Spółki z listy GPW, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia: Modivo (97 PLN, 2,299093018350566% względem poprzedniego sprawdzenia).
> 1× Oto notatka do powiadomienia na pulpicie: Najwyższe zmiany w spółkach z listy GPW: Modivo (97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia), CD Projekt (247.4 PLN).

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Oto notatka do powiadomienia na pulpicie: Na liście spółek z GPW wystąpiły znaczące zmiany w kursach. Spółka Grupa Polsat zmieniła kurs o -2,63% względem poprzedniego sprawdzenia.
> 1× Na pulpicie informujemy, że spółka Grupa Polsat zmieniła kurs o -2,63% od ostatniego sprawdzenia. Aktualnie kurs spółki wynosi 15,93 PLN.
> 1× Oto notatka do powiadomienia na pulpicie: Ostatnio na GPW wystąpiły zmiany w kursach następujących spółek: Grupa Polsat (15,93 PLN), Benefit Systems (5160 PLN).
> 1× Po sprawdzeniu listy spółek z GPW, stwierdzono, że nie ma znaczących zmian.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Oto notatka do powiadomienia na pulpicie: Ostatnie sprawdzenie kursów spółek z listy odbyło się 15.08. Spółki, które zmieniły kurs o co najmniej 2% od tego sprawdzenia, to: Grupa Polsat (15.08 PLN, -5.335844318895164%), Benefit Systems (5135 PLN).
> 1× PO POLSKU - Zmiany w kursach spółek na liście. Jedna spółka zmieniła kurs o co najmniej 2%. Grupa Polsat zmieniła kurs na 15,08 PLN.
> 1× PO POLSKU - Zmiany w kursach spółek z listy. Na liście spółek z GPW pojawiły się zmiany w kursach następujących spółek: Grupa Polsat - 15,08 PLN, -5,335844318895164% (spadek), Pepco - 41,87 PLN (spadek), Modivo - 88,06 PLN (spadek), CD Projekt - 236,40 PLN (spadek), KGHM - 337,05 PLN (spadek).
> 1× Oto notatka do powiadomienia na pulpicie: Na liście spółek z GPW wystąpiły zmiany w kursach. Zmieniły się kursy spółek: Grupa Polsat (15.08 PLN), Benefit Systems (5135 PLN).

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Na pulpicie informujemy, że spółka Modivo zmieniła kurs o 3.929% i spółka CD Projekt zmieniła kurs o 3.045%. Spółka Grupa Polsat zmieniła kurs o 5.039%.
> 1× Po sprawdzeniu listy spółek z GPW, stwierdzono, że Modivo i CD Projekt zmieniły kurs o co najmniej 2%. Kurs Modivo wynosi 91,52 PLN, a CD Projekt - 243,60 PLN.
> 1× Najnowsze kursy spółek z listy GPW: Modivo: 91,52 PLN CD Projekt: 243,60 PLN Grupa Polsat: 15,84 PLN KGHM: 349,35 PLN Niestety, nie ma znaczących zmian w kursach spółek na liście.
> 1× Oto notatka do powiadomienia na pulpicie: W ciągu ostatniego sprawdzenia spółki Modivo (91,52 PLN) i CD Projekt (243,60 PLN) zmieniły kurs o co najmniej 2%. Spółka Modivo zwiększyła kurs o 3.929% do 91,52 PLN, a CD Projekt zwiększył kurs o 3.045% do 243,60 PLN.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Na pulpicie informujemy, że spółka PKN Orlen zmieniła kurs o 2,12% względem poprzedniego sprawdzenia. Aktualny kurs spółki wynosi 155,02 PLN.
> 1× Oto notatka do powiadomienia na pulpicie: Jasne, nie ma znaczących zmian.
> 1× Najnowsze kursy spółek z listy GPW: XTB: 143.16 PLN, PKN Orlen: 155.02 PLN, Grupa Kęty: 1277 PLN, Pepco: 47.45 PLN, LPP: 26400 PLN, Modivo: 93.26 PLN, CD Projekt: 259.8 PLN, Grupa Polsat: 17.72 PLN, Benefit Systems: 5615 PLN, KGHM: 322.45 PLN, Rainbow Tours: 125.1 PLN.
> 1× Na pulpicie: Zmiany w kursach spółek na GPW. XTB: -2,12% (143,16 PLN), PKN Orlen: -0,06% (155,02 PLN), Grupa Kęty: -0,03% (1277 PLN), Pepco: -0,02% (47,45 PLN), LPP: -0,03% (26400 PLN), Modivo: -0,03% (93,26 PLN), CD Projekt: -0,03% (259,80 PLN), Grupa Polsat: -0,03% (17,72 PLN), Benefit Systems: -0,03% (5615 PLN), KGHM: -0,03% (322

**Cykle bez ruchu, zestaw 1** (10 różnych wersji na 16 notatek, trzy najczęstsze):

> 4× Niestety, nie ma na liście spółek, które zmieniły kurs o co najmniej 2%.
> 3× Nie było znaczących zmian w kursach spółek na liście.
> 2× Nie było znaczących zmian na listie spółek.

**Cykle bez ruchu, zestaw 2** (6 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian w kursach spółek na liście.
> 1× Nie było znaczących zmian na liście spółek.
> 1× Nie było znaczących zmian na spółkach na liście.

### 12. `granite4.2:3b` — miary 9/11, konsumowalne 4/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo: 97.00 PLN Kurs spółki Modivo wzrosł o 2.30% względem poprzedniego sprawdzenia.
> 1× Modivo zmienił kurs o 2.30% względem poprzedniego sprawdzenia. Kurs: 97.00 PLN.
> 1× Modivo: 97.00 PLN Kurs spółki Modivo zmienił się o 2.30% względem poprzedniego sprawdzenia.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o 2.63% względem poprzedniego sprawdzenia. Kurs: 15.93 PLN. XTB: 150.68 PLN PKN Orlen: 150.42 PLN Grupa Kęty: 1267.00 PLN Pepco: 42.03 PLN LPP: 24300.00 PLN Modivo: 87.34 PLN CD Projekt: 238.90 PLN Benefit Systems: 5160.00 PLN KGHM: 343.35 PLN
> 1× Grupa Polsat: 15.93 PLN, zmienił się o -2.63% względem poprzedniego sprawdzenia. Aktualne kursy zmienionych spółek: Grupa Polsat – 15.93 PLN.
> 1× Grupa Polsat zmieniła kurs o -2.63%. Kurs: 15.93 PLN. Aktualne kursy: XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267.00 PLN, Pepco 42.03 PLN, LPP 24300.00 PLN, Modivo 87.34 PLN, CD Projekt 238.90 PLN, Grupa Polsat 15.93 PLN, Benefit Systems 5160.00 PLN, KGHM 343.35 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o 5.33% względem poprzedniego sprawdzenia. Kurs: 15.08 PLN. XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269.00 PLN Pepco: 41.87 PLN LPP: 24160.00 PLN Modivo: 88.06 PLN CD Projekt: 236.40 PLN Benefit Systems: 5135.00 PLN KGHM: 337.05 PLN
> 1× Grupa Polsat zmieniła kurs o 5.33% względem poprzedniego sprawdzenia. Kurs: 15.08 PLN XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269.00 PLN Pepco: 41.87 PLN LPP: 24160.00 PLN Modivo: 88.06 PLN CD Projekt: 236.40 PLN Benefit Systems: 5135.00 PLN KGHM: 337.05 PLN
> 1× Grupa Polsat: 15.08 PLN, zmienił się o -5.33% względem poprzedniego sprawdzenia. Aktualne kursy zmienionych spółek: Grupa Polsat – 15.08 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 3× Modivo wzrosł o 3.93% – kurs 91.52 PLN. CD Projekt wzrosł o 3.05% – kurs 243.60 PLN. Grupa Polsat wzrosł o 5.04% – kurs 15.84 PLN. KGHM wzrosł o 3.65% – kurs 349.35 PLN.
> 1× Modivo wzrosło o 3.93% – kurs 91.52 PLN. CD Projekt wzrosło o 3.05% – kurs 243.60 PLN. Grupa Polsat wzrosła o 5.04% – kurs 15.84 PLN. KGHM wzrosło o 3.65% – kurs 349.35 PLN.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB spadało o 2.12% względem poprzedniego sprawdzenia, kurs: 143.16 PLN. Aktualne kursy zmienionych spółek: XTB: 143.16 PLN.
> 1× XTB: 143.16 PLN zmienił kurs o -2.12% względem poprzedniego sprawdzenia. Aktualne kursy spółek zmienionych o 2% lub więcej: XTB: 143.16 PLN.
> 1× XTB wzrosł o 2.12% od poprzedniego sprawdzenia, kurs 143.16 PLN. Aktualne kursy: XTB 143.16 PLN.
> 1× XTB: 143.16 PLN – zmienił się o -2.12% względem poprzedniego sprawdzenia. Aktualne kursy zmienionych spółek: XTB: 143.16 PLN.

**Cykle bez ruchu, zestaw 1** (5 różnych wersji na 16 notatek, trzy najczęstsze):

> 11× nie było znaczących zmian, nie wymieniaj kursów i pod żadnym pozorem nie opisuj poziomów cen jako wzrostów ani spadków.
> 2× nie było znaczących zmian, nie wymieniaj kursów ani opisuj poziomów cen jako wzrostów ani spadków. XTB: 149.90 PLN PKN Orlen: 149.54 PLN Grupa Kęty: 1284.00 PLN Pepco: 44.12 PLN LPP: 24600.00 PLN Modivo: 96.42 PLN CD Projekt: 247.50 PLN Grupa Polsat: 16.14 PLN Benefit Systems: 5160.00 PLN KGHM: 347.25 PLN
> 1× Nie było znaczących zmian, nie wymieniaj kursów ani opisuj poziomów cen jako wzrostów ani spadków. XTB: 149.82 PLN PKN Orlen: 148.62 PLN Grupa Kęty: 1282.00 PLN Pepco: 43.17 PLN LPP: 24420.00 PLN Modivo: 96.70 PLN CD Projekt: 246.10 PLN Grupa Polsat: 16.01 PLN Benefit Systems: 5150.00 PLN KGHM: 347.65 PLN

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian, nie wymieniaj kursów. XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292.00 PLN Pepco: 48.00 PLN LPP: 26400.00 PLN Modivo: 92.90 PLN CD Projekt: 262.40 PLN Grupa Polsat: 18.30 PLN Benefit Systems: 5730.00 PLN KGHM: 327.75 PLN Rainbow Tours: 126.70 PLN
> 2× Nie było spółek z zmianą kursu o co najmniej 2%. XTB: 146.26 PLN PKN Orlen: 154.4 PLN Grupa Kęty: 1276 PLN Pepco: 47.89 PLN LPP: 26480 PLN Modivo: 93.8 PLN CD Projekt: 262 PLN Grupa Polsat: 18 PLN Benefit Systems: 5635 PLN KGHM: 321.3 PLN Rainbow Tours: 124.8 PLN
> 1× Brak spółek z zmianą kursu o co najmniej 2%. XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292.00 PLN Pepco: 48.00 PLN LPP: 26400.00 PLN Modivo: 92.90 PLN CD Projekt: 262.40 PLN Grupa Polsat: 18.30 PLN Benefit Systems: 5730.00 PLN KGHM: 327.75 PLN Rainbow Tours: 126.70 PLN

### 13. `qwen3.5:2b` — miary 2/11, konsumowalne 2/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo zmieniło kurs o 2,29% do poziomu 97,00 PLN. Pozostałe spółki na liście nie wykazały znaczących zmian cenowych.
> 1× Modivo zmienił kurs o 2,29% na poziomie 97,00 PLN. Pozostałe spółki z podanej listy nie wykazały znaczących zmian cenowych.
> 1× Modivo zmienił kurs o 2,30% od poprzedniego sprawdzenia. Kurs tej spółki wynosi 97,00 PLN. Pozostałe spółki na liście nie wykazały znaczących zmian cenowych.
> 1× Modivo zmienił kurs o 2,30% na poziomie 97,00 PLN. Pozostałe spółki z listy nie wykazały znaczących zmian w ostatnich sprawdzonych cenach.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 2,63% do poziomu 15,93 PLN. W pozostałych spółach na liście nie wykryto zmian znaczących powyżej dwóch procentowych punktów.
> 1× Grupa Polsat zmieniła kurs o 2,63% spadkiem do 15,93 PLN. XTB podniósł się o 0,45% do 150,68 PLN. PKN Orlen spadł o 0,02% do 150,42 PLN. Grupa Kęty wzrosła o 1,72% do 1267 PLN. Pepco zmienił kurs o 0,03% spadkiem do 42,03 PLN. LPP spadł o 0,01% do 24300 PLN. Modivo zmienił kurs o 0,05% spadkiem do 87,34 PLN. CD Projekt wzrosł o 0,06% do 23
> 1× Grupa Polsat spadła o 2,63% do poziomu 15,93 PLN. Pozostałe spółki na liście nie wykazały zmian znaczących powyżej dwóch procent.
> 1× Grupa Polsat zmieniła kurs o 2,63% spadkiem do 15,93 PLN. Tylko ona spełnia warunek zmiany powyżej dwóch procentowych punktów względem poprzedniego sprawdzenia. Pozostałe spółki na liście nie wykazały znaczących ruchów cenowych.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. Pozostałe spółki na liście nie wykazały zmian znaczących powyżej 2%.
> 1× Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. W pozostałych spółach na liście nie zaobserwowano zmian przekraczających dwa procentowe punkty od ostatniego sprawdzenia.
> 1× Grupa Polsat zmieniła kurs o 5,34% spadkiem do poziomu 15,08 PLN. W pozostałych spółach na liście nie wykazano znaczących ruchów cenowych powyżej dwóch procent.
> 1× Grupa Polsat zmieniła kurs o 5,34% spadkiem do poziomu 15,08 PLN. Pozostałe spółki na liście nie wykazały zmian znaczących powyżej dwóch procent.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Modivo zmienił kurs o 3,92%, CD Projekt o 3,05%, Grupa Polsat o 5,04% oraz KGHM o 3,65%. W pozostałych spółkach na liście nie było znaczących zmian cen.
> 1× Modivo zmienił kurs o 3,92%, CD Projekt o 3,05%, Grupa Polsat o 5,04% oraz KGHM o 3,65%. Brak znaczących zmian na liście spółek.
> 1× Modivo zmienił kurs o 3,92%, CD Projekt o 3,05%, Grupa Polsat o 5,04% oraz KGHM o 3,65%. W pozostałych spółkach na liście nie było zmian znaczących powyżej 2%.
> 1× Modivo zmienił kurs o 3,92%, a Grupa Polsat o 5,04%. Kęty nie podlega notowaniu giełdową, więc nie ma na niej znaczących zmian. W pozostałych spółkach z listy kursy pozostają stabilne bez istotnych ruchów cenowych.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB zmienił kurs o 2,12% spadkiem do poziomu 143,16 PLN. Pozostałe spółki na liście nie wykazały znaczących zmian cenowych.
> 1× XTB zmienił kurs o 2,12% spadkiem do poziomu 143,16 PLN. Pozostałe spółki na liście nie wykazały zmian znaczących powyżej 2%. Nieznajomość przyczyn ruchu w pozostałych firmach.

**Cykle bez ruchu, zestaw 1** (16 różnych wersji na 16 notatek, trzy najczęstsze):

> 1× XTB zmienił kurs o 0,14%, PKN Orlen o 0,32%, Grupa Kęty o 1,98%, Pepco o 0,76%, LPP o 1,52%, Modivo o 1,48%, CD Projekt o 0,58%, Grupa Polsat o 0,08% oraz Benefit Systems o 0,33%. KGHM pozostał bez zmian.
> 1× XTB: kurs wzrósł o 0,68% do 151,02 PLN. PKN Orlen: spadł o 1,43% do 148,18 PLN. Grupa Kęty: zmienił się o 1,79% do 1288 PLN. Pepco: kurs wzrósł o 0,56% do 43,47 PLN. LPP: spadł o 1,01% do 24620 PLN. Modivo: zmienił się o 0,96% do 96,3 PLN. CD Projekt: kurs wzrósł o 0,58% do 245,3 PLN. Grupa Polsat: spadł
> 1× XTB zmienił kurs o 0,52% do 149,82 PLN. PKN Orlen spadł o 1,37% do 148,62 PLN. Grupa Kęty wzrosła o 1,01% do 1282,00 PLN. Pepco zmienił kurs o 0,39% do 43,17 PLN. LPP spadł o 0,99% do 24420,00 PLN. Modivo wzrosł o 0,56% do 96,70 PLN. CD Projekt zmienił kurs o 0,88% do 246,10 PLN. Grupa Polsat spadła o 0,37% do 16,0

**Cykle bez ruchu, zestaw 2** (6 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia.
> 1× XTB zmieniło kurs o 0,18% w górę do poziomu 146,26 PLN. PKN Orlen spadł o 0,35% do 154,40 PLN. Grupa Kęty wzrosła o 1,97% do 1276,00 PLN. Pepco zmieniło kurs o 0,85% w dół do 47,89 PLN. LPP spadło o 1,38% do 26480,00 PLN. Modivo wzrosło o 0,53% do 93,80 PLN. CD Projekt zmieniło kurs o 1,42% w górę do 262,00 PLN. Grupa Polsat spadła o 1
> 1× XTB zmienił kurs o 0,42%, PKN Orlen o 1,35%, Grupa Kęty o 1,98%, Pepco o 1,67%, LPP o 1,99%, Modivo o 1,49%, CD Projekt o 1,89%, Grupa Polsat o 2,03%, Benefit Systems o 1,56%, KGHM o 1,42% i Rainbow Tours o 1,74%.

### 14. `falcon3:7b` — miary 7/11, konsumowalne 2/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× XTB: 153.6 PKN Orlen: 148.18 Grupa Kęty: 1260 Pepco: 43.19 LPP: 24400 Modivo: 97 Modivo zmienił kurs o 2.299093018350566%, zmiana w 2%.
> 1× XTB: 153.6 PKN Orlen: 148.18 Grupa Kęty: 1260 Pepco: 43.19 LPP: 24400 Modivo zmienił kurs o 2.299093018350566% (97 PLN). XTB: 153.6 PKN Orlen: 148.18 Grupa Kęty: 1260 Pepco: 43.19 LPP: 24400 Notatka: Modivo zmienił kurs. Nie znam przyczyny r
> 1× XTB: 153.6 PKN Orlen: 148.18 Grupa Kęty: 1260 Pepco: 43.19 LPP: 24400 Modivo: 97 Modivo zmienił kurs o 2.299093018350566% od ostatniego sprawdzenia.
> 1× XTB: 153.6 PKN Orlen: 148.18 Grupa Kęty: 1260 Pepco: 43.19 LPP: 24400 Modivo: 97 PLN, 2.299% względem ostatniego sprawdzenia. Kursy: XTB: 153.6 PKN Orlen: 148.18 Grupa Kęty: 1260 Pepco: 43.19 LPP: 24400 Modivo: 97 PLN Notatka: Modivo zmienił kurs o 2.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat (15.93 PLN) zmieniła swój kurs o -2.63%.
> 1× Grupa Polsat (15.93 PLN) zmieniła swój kurs o -2.6% od ostatniego sprawdzenia.
> 1× Grupa Polsat zmieniła swój kurs o -2,63% od ostatniego sprawdzenia. Aktualne kursy: XTB: 150.68 PLN PKN Orlen: 150.42 PLN Grupa Kęty: 1267.00 PLN Pepco: 42.03 PLN LPP: 24300.00 PLN Modivo: 87.34 PLN CD Projekt: 238.90 PLN Benefit Systems: 5160.00 PLN KGHM: 343.35 PLN

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat (15.08 PLN, -5.34%) wzajemnie wpłynęła na ceny na pulpicie.
> 1× Grupa Polsat (15.08 PLN, -5.34%) – ceny spółek zmieniły się.
> 1× Grupa Polsat zmieniła kurs o -5,34% od ostatniego sprawdzenia. Aktualne kursy: XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269.00 PLN Pepco: 41.87 PLN LPP: 24160.00 PLN Modivo: 88.06 PLN CD Projekt: 236.40 PLN Benefit Systems: 5135.00 PLN KGHM: 337.05 PLN Grupa Polsat: 15.08 PLN
> 1× Grupa Polsat (15.08 PLN, -5.34%) wzajemnie zmieniła swoje kursy.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 3× Modivo (91.52 PLN) zmieniło kurs 3.93% od ostatniego sprawdzenia. CD Projekt (243.6 PLN) zmieniło kurs 3.05% od ostatniego sprawdzenia. Grupa Polsat (15.84 PLN) zmieniło kurs 5.04% od ostatniego sprawdzenia. KGHM (349.35 PLN) zmieniło kurs 3.65% od ostatniego sprawdzenia. Nie znam przyczyny ruchu w tym
> 1× Modivo (91.52 PLN) zmieniło kurs 3.93% od ostatniego sprawdzenia. CD Projekt (243.6 PLN) zmieniło kurs 3.05% od ostatniego sprawdzenia. Grupa Polsat (15.84 PLN) zmieniło kurs 5.04% od ostatniego sprawdzenia. KGHM (349.35 PLN) zmieniło kurs 3.65% od ostatniego sprawdzenia. W tym momencie nie znam przyczyny r

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB: 143.16 PLN, -2.12% PKN Orlen: 155.02 PLN Grupa Kęty: 1277.00 PLN Pepco: 47.45 PLN
> 1× XTB: 143.16 PLN, -2.12% PKN Orlen: 155.02 PLN Grupa Kęty: 1277.00 PLN Pepco: 47.45 PLN LPP: 26400.00 PLN Modivo: 93.26 PLN CD Projekt: 259.80 PLN Benefit Systems: 5615.00 PLN KGHM: 322.45 PLN Rainbow Tours: 125.10 PLN Nie znaczące zmiany w kursach.
> 1× XTB: 143.16 PLN, -2.12%. PKN Orlen: 155.02 PLN. Grupa Kęty: 1277.00 PLN. Pepco: 47.45 PLN.
> 1× XTB: 143.16 PLN, -2.12% | PKN Orlen: 155.02 PLN | Grupa Kęty: 1277.00 PLN | Pepco: 47.45 PLN

**Cykle bez ruchu, zestaw 1** (14 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× XTB: 149.82 PKN Orlen: 148.62 Grupa Kęty: 1282 Pepco: 43.17 LPP: 24420 Modivo: 96.7 CD Projekt: 246.1 Grupa Polsat: 16.005 Benefit Systems: 5150 KGHM: 347.65 Nie było znaczących zmian.
> 1× XTB: 149.9, PKN Orlen: 149.54, Grupa Kęty: 1284, Pepco: 44.12 LPP: 24600, Modivo: 96.42, CD Projekt: 247.5, Grupa Polsat: 16.135 Benefit Systems: 5160, KGHM: 347.25 Nie było znaczących zmian.
> 1× XTB: 151.02 PKN Orlen: 148.18 Grupa Kęty: 1288.00 Pepco: 43.47 LPP: 24620.00 Modivo: 96.30 CD Projekt: 245.30 Grupa Polsat: 16.105 Benefit Systems: 5120.00 KGHM: 348.35

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× XTB: 147.82 PKN Orlen: 154.98 Grupa Kęty: 1292 Pepco: 48 LPP: 26400 Modivo: 92.9 CD Projekt: 262.4 Benefit Systems: 5730 KGHM: 327.75 Rainbow Tours: 126.7 Nie było znaczących zmian.
> 1× XTB: 146.30, PKN Orlen: 154.50, Pepco: 47.90, Modivo: 93.90 XTB zmienił kurs o 0.04%, PKN Orlen o 0.06%, Pepco o 0.02%, Modivo o 0.02%. Nie wydaje się, że to znaczące.
> 1× XTB: 146.30, PKN Orlen: 154.50, Pepco: 48.00, LPP: 26485.00 XTB i PKN Orlen zmieniły swój kurs o 0.03% i 0.06% w ciągu ostatniego sprawdzenia, a Pepco i LPP zmieniły swój kurs o 0.44% i 0.20% w ciągu ostatniego sprawdzenia.

### 15. `exaone3.5:7.8b` — miary 8/11, konsumowalne 1/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo doświadczyło zmiany kursu o 2.29%, teraz stosię 97,00 PLN. XTB: 153,60 PLN, PKN Orlen: 148,18 PLN, Grupa Kęty: 1260,00 PLN. Inne spóły nie zmieniły się znacząco w cenie.
> 1× Modivo doświadcza znaczącego wzrostu kursu o 2.29%, teraz stosię 97 PLN. XTB: 153.60 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1260.00 PLN. Linia pozostaje stabilna bez znaczących zmian w poziomach cenników innych spółek na liście. Brak znanych przyczyn ruchu intensywnego zmian kursów w pozostałych firmach.
> 1× Modivo obserwuje się wzroście do 97,00 PLN, ±2,29% od poprzedniego poziomu. Brak znanych przyczyn zwiększenia lub spadku w innych spółkach na liście. Nie było znaczących zmian w kursach pozostałych spółek. Pamiętaj: Informacja monitoringu, nie inwestycyjna rekomendacja.
> 1× Modivo zmieniło się o 2.29%, nowy kurs: 97 PLN. XTB: 153.60 PLN, PKN Orlen: 148.18 PLN. LPP: 24400.00 PLN, CD Projekt: 247.40 PLN. Brak znanych przyczyn związanych z znaczącymi zmianami w poziomie cen innych spółek na liście.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat zmieniła kurs o -2.63% do 15.93 PLN. Inne spóły nie przekroczyły zmiany o 2% od ostatniego sprawdzania. Brak znanych przyczyn związanych z znaczącymi zmianami cen spółek na liście. Powiadomienie na pulpicie: Spółka Grupa Polsat pokazuje malą zmianeń, reszta stabilna.
> 1× Grupa Polsat zmieniła kurs o -2.63% do 15.93 PLN. Brak innych spółek z listą związanego z znaczącym kursowym ruchem. Nie było znaczących zmian w kursach pozostałych spółek. Pamiętaj: Informacja monitoringu, nie inwestycyjna rekomendacja.
> 1× Grupa Polsat zmieniła kurs o -2.63% do 15.93 PLN. Inne spóły nie przekroczyły zmiany o 2% od ostatniego sprawdzania. Brak znaczących zmian w kursach pozostałych spółek.
> 1× Grupa Polsat doznawała spadek o 2,63%, kurs teraz 15,93 PLN. PKN Orlen i CD Projekt nie zmieniły się w kursie. Benefit Systems pozostaje najwyższą pozycją z kursem 5160,22 PLN. Brak znaczących zmian w poziomie innych spółek.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat zmnieściała się o 5,34%, kurs teraz 15,08 PLN. Brak innych spółek z listу изменяющихся курсов со значимостью не менее 2%. Nie znane przyczyny ruchu na pozostałych spółkach. Poziomów cen nie zażądano analizy w tym okresie.
> 1× Grupa Polsat zmieniła kurs o -5.34% do 15.08 PLN. Inne spóły na liście nie przekroczyły zmiany o 2%. Brak znanych przyczyn związanych z znaczącymi zmianami kursów innych spółek. Powiadomienie na pulpicie: Grupa Polsat - kurs spadła o 5,34%. Brak innych znaczących zmian kursów.
> 1× Grupa Polsat zmieniła kurs o -5.34% do 15.08 PLN. Inne spóły nie przekraczająły zmianę +/- 2%. Brak znanych przyczyn związanych z znaczącym ruchem ceny w poziomie poziomów spółek na liście. Powiadomienie na pulpicie: Spółka Grupa Polsat zauważyła najwyższą zmiana kursowa, obejmującą ponad 5% w stronę spadku.
> 1× Grupa Polsat zmieniła kurs o -5.34% do 15.08 PLN. Inne spóły na liście nie przekroczyły zmiany o 2%. Brak znanych przyczyn zwiększających lub obniżających kursy innych spółek znakomych na liście. Powiadomienie na pulpicie: Spółka Grupa Polsat obserwuje znaczący spadek kursu.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Modivo i CD Projekt zmieniły się o 3%+ od ostatniego sprawdzania, odpowiadając: 91.52 PLN i 243.60 PLN respektywnie. Grupa Polsat również zobaczyła znaczący przesun około 5%, doszając do 15.84 PLN. Brak znanych przyczyn ruchu w pozostałych spółkach.
> 1× Modivo i CD Projekt zmieniły się o 3%+ do nowych poziomów cen: Modivo (91.52 PLN), CD Projekt (243.60 PLN). Grupa Polsat również osiągnęła znaczący wzrost, doszając do 15.84 PLN. KGHM pozostaje stabilny, z poziomem 349.35 PLN. Brak znanych przyczyn ruchu w pozostałych firmach na liście.
> 1× Modivo i CD Projekt zmieniły się o 3%+ od ostatniego sprawdzania, odpowiadając: 91.52 PLN (+3.9%), 243.6 PLN (+3.0%). Grupa Polsat również zobowiązana, wzrostając o 5.0% do 15.84 PLN. Brak znanych przyczyn ruchu w pozostałych spółkach.
> 1× Modivo i CD Projekt zmieniły się o 3%+ od ostatniego sprawdzania, kursy: Modivo (91.52 PLN), CD Projekt (243.60 PLN). Grupa Polsat również zyskała ponad 5%, kurs teraz 15.84 PLN. Brak znanych przyczyn zwiększających poziom cen w poziomie pozostawionym spółkami.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB kurs zmnieściał się o 2,12%, teraz stosię 143,16 PLN. Poniżej tylko zmienione kurse: XTB: 143.16 PLN Brak znanych przyczyn zwiększających lub obniżających poziomów cen innych spółek o co najmniej 2%.
> 1× XTB kurs zmnieściał się o 2,12%, teraz stosię 143,16 PLN. Poniżej tylko zmienione kurse: XTB: 143,16 PLN PKN Orlen: 155,02 PLN Grupa Kęty: 1277,00 PLN Pepco: 47,45 PLN LPP: 26400,00 PLN Modivo: 93,26 PLN CD Projekt: 259,80 PLN Grupa Polsat: 17,72 PLN Benefit Systems: 5615,00 PLN K
> 1× XTB kurs zmnieściał się o 2,12%, teraz stosię 143,16 PLN. Poniżej tylko zmienione kurse: XTB: 143.16 PLN PKN Orlen: 155.02 PLN Grupa Kęty: 1277 PLN Pepco: 47.45 PLN LPP: 26400 PLN CD Projekt: 259.80 PLN Grupa Polsat: 17.72 PLN Benefit Systems: 5615.00 PLN KGHM: 322.45 PLN Rainbow Tours:

**Cykle bez ruchu, zestaw 1** (12 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Brak znaczących zmian w kursach spółek na liście. PKN Orlen: 149.54 PLN, Grupa Kęty: 1284.00 PLN, Pepco: 44.12 PLN, LPP: 24600.00 PLN, Modivo: 96.42 PLN, CD Projekt: 247.50 PLN, Grupa Polsat: 16.13 PLN, Benefit Systems: 5160.00 PLN, KGHM: 347.25 PLN.
> 3× Brak znaczących zmian w kursach spółek na liście. PKN Orlen: 148.18 PLN, Grupa Kęty: 1288.00 PLN, Pepco: 43.47 PLN, LPP: 24620.00 PLN, Modivo: 96.30 PLN, CD Projekt: 245.30 PLN, Grupa Polsat: 16.10 PLN, Benefit Systems: 5120.00 PLN, KGHM: 348.35 PLN.
> 1× Brak znaczących zmian w kursach spółek na liście. Kursy spółek pozostają niezmienione od ostatniego sprawdzania. Powiadomienie na pulpicie: Spóły XTB, PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Polska Telekomunikacje, Beneficja Systems oraz KGHM pozostają na ich poziomach cen.

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× Nie obserwowano znaczących zmian w kursach spółek na liście. Ponadto: XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292.00 PLN Pepco: 48.00 PLN LPP: 26400.00 PLN Modivo: 92.90 PLN CD Projekt: 262.40 PLN Grupa Polsat: 18.30 PLN Benefit Systems: 5730.00 PLN KGHM: 327.75 PLN
> 1× Brak znaczących zmian w kursach spółek na liście. Poniżej tylko zmienione kurse: XTB: 146.26 PLN PKN Orlen: 154.40 PLN Grupa Kęty: 1276.00 PLN Pepco: 47.89 PLN LPP: 26480.00 PLN Modivo: 93.80 PLN CD Projekt: 262.00 PLN Grupa Polsat: 18.00 PLN Benefit Systems: 5635.00 PLN KGHM:
> 1× Brak znaczących zmian w kursach spółek na liście. Powiązane spóły: brak zmian. Kurse aktualnych: CD Projekt: 262.40 PLN Grupa Kęty: 1292.00 PLN PKN Orlen: 154.98 PLN XTB: 147.82 PLN Grupa Polsat: 18.30 PLN Benefit Systems: 5730.00 PLN KGHM: 327.75 PLN Modivo: 92.90 PLN Pepco: 48.

### 16. `falcon3:3b` — miary 3/11, konsumowalne 0/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo (MDV.WA) - 97 PLN (2.299093018350566%) względem ostatniego sprawdzenia zmienił kurs na 2.299093018350566% od ostatniego sprawdzenia. XTB - 153.6 PLN PKN Orlen - 148.18 PLN Grupa Kęty - 1260 PLN Pepco - 43.19 PLN LPP - 24400 PLN CD Projekt
> 1× Oto informacje o zmianach kursów spółek z GPW od <insert_date_here> do <insert_date_here>: Modivo (MDV.WA): Oto zmiany: Kurs zmienił od 97 PLN do 97.0229 PLN (2.299093018350566% wzgl. ostatniego sprawdzenia). Odpowiedź: Kurs Modivo zmienił się do 97.0229 PLN. XTB:
> 1× Oto informacje o zmianach kursów spółek z GPW od 20.06.2023 do 26.06.2023: Modivo (MDV.WA): Odpowiedź: Kurs zmienił o 2.299093018350566% od ostatniego sprawdzenia. Odpowiedź zawiera: 97 PLN. Najpierw: Kurs zmienił o 2.29909301835056
> 1× Modivo (MDV.WA): Zmiany kursowe: Od 97 PLN do 97.022990930183566 PLN (od 0.022990930183566%, wzgl. poprzedniego sprawdzenia). Nie zmiany w giełdzu i miejsce notowania: 0.022990930183566% Krótką notatkę: 0.023% CD Projekt: -

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Oto informacje o zmianach kursów spółek z GPW od 20.09.2023: XTB (Xetra) Kurs z GPW w 20.09.2023: 150.68 PLN Zmiany kursu o najmniej 2% od ostatniego sprawdzenia: -2.6283618581907073% Grupa Polsat (CPS.WA) Kurs z GPW w 20.09.2023: 15.93 PLN Zmiany kursu o najmniej 2
> 1× Oto informacje o położenia zmian kursów spółek w Polsce w okazji zmiany kursy w 2% lub menażywania, które zmieniły kurs o to. Nie pozostały zmiany kursów spółek w 2% lub menażywania, które zmieniły kursy w Polsce. Jeśli nie znasz przyczyny ruchu, napisz: "Nie pozostały zmiany kursów spółek w 2% lub menażywania, kt
> 1× Oto notatka połączona na pulpicie: Notatka PO POLSKU: Grupa Polsat (CPS.WA): Kurs zmieniony o -2.63% wzgl. ostatniego sprawdzenia. Kurs aktualny: 15.93 PLN. Grupa Kęty: Kurs zmieniony o -2.62% wzgl. ostatniego sprawdzenia. Kurs aktualny: 1267 PLN. Modivo: Kurs zmieniony o -2.63% wzgl. ostatniego sprawdzenia. Kurs akt
> 1× Oto informacje o zmianach kursów spółek z GPW od 20.03.2023: XTB: 150.68 PLN PKN Orlen: 150.42 PLN Grupa Kęty: 1267 PLN Pepco: 42.03 PLN LPP: 24300 PLN Modivo: 87.34 PLN CD Projekt: 238.9 PLN Grupa Polsat: 15.93 PLN Benefit Systems: 5160 PLN KGHM: 343.35 PLN

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Oto informacje o zmianach kursów spółek z GPW od 20.08.2023: XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269 PLN Pepco: 41.87 PLN LPP: 24160 PLN Modivo: 88.06 PLN CD Projekt: 236.4 PLN Grupa Polsat: 15.08 PLN Benefit Systems: 5135 PLN KGHM: 337.05 PLN
> 1× Oto notatka połączona na pulpicie: Notatka PO POLSKU: Grupa Polsat (CPS.WA): Odpowiedź: 15.08 PLN Wzglębimy: -5.335844318895164% Odpowiedź aktualna: 15.08 PLN Krótkie informacje: Grupa Polsat zmienił kurs na -5.335844318895164% względem ostatniego sprawdzenia. Odp
> 1× Oto notatka po powiadomieniu na pulpicie: Notatka PO POLSKU Grupa Polsat (CPS.WA): 15.08 PLN, -5.335844318895164% względem poprzedniego sprawdzenia. Grupa Polsat zmienił kurs na 15.08 PLN, to ostatniego 2% od ostatniego sprawdzenia. Krótką notatkę: Grupa Polsat zmienił kurs na 15.08 PLN, to ostatniego 2% od ostatniego sprawdzenia. Odpow
> 1× Oto informacje o położenia krótkich notatków na pulpicie, jakby zapisywaś na listy spółek z GPW: XTB: 150.34 PLN Najwyższy zdania: "Krótko zmienia kurs na -5.335844318895164% względem ostatniego sprawdzenia. Grupa Polsat (CPS.WA) zmieni kurs na 15.08 PLN." PKN Orlen: 151.14 PLN Najwyższy z

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Oto notatkę PO POLSKU do powiadomienia na pulpicie, zdaniem z listy spółek z GPW, zmiany kursy o najmniej 2% od ostatniego sprawdzenia: CD Projekt (CDR.WA): Odpowiedź: 243.6 PLN Odpowiedź z listy: 243.6 PLN Zmiany kursy: 0% Nie zmieniły kursy. KGHM (KGH.WA):
> 1× Poniższe nazwy spółek z listy: XTB: 149.84 PLN PKN Orlen: 152.42 PLN Grupa Kęty: 1261 PLN Pepco: 41.9 PLN LPP: 23960 PLN Modivo: 91.52 PLN CD Projekt: 243.6 PLN Grupa Polsat: 15.84 PLN Benefit Systems: 5115 PLN KGHM: 349.35 PLN Zmiany kursów z listy: Mod
> 1× CD Projekt (CDR.WA): Kurs zmienił o 3.0457%, w tym ostatniego sprawdzenie. CD Projekt jest znany za pomocą projektu One Piece, która znajduje się w serie anime. LPP (LPP.WA): Kurs zmienił o 3.6493%, w tym ostatniego sprawdzenie. LPP jest znany za pomocą projektu "LPP", który jest osobnośnym zdrowiem. Modivo (MDV.WA): K
> 1× Oto poziomowe cen zmiany kursy spółek z GPW, które zmieniły kurs o najmniej 2% od ostatniego sprawdzenia. Modivo (MDV.WA): Względem poprzedniego sprawdzenia: 91.52 PLN Względem aktualnego: 91.52 PLN Nie zmienił kurs. CD Projekt (CDR.WA): Względem poprzedniego sprawdzenia: 243.6 PLN

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB: 143.16 PLN (-2.12%) PKN Orlen: 155.02 PLN Grupa Kęty: 1277 PLN Pepco: 47.45 PLN LPP: 26400 PLN Modivo: 93.26 PLN CD Projekt: 259.8 PLN Grupa Polsat: 17.72 PLN Benefit Systems: 5615 PLN KGHM: 322.45 PLN Rainbow Tours: 125.1 PLN Odpowiedź: Najwyżej cztery zdania, zmieniały kurs
> 1× W 28-12-2023, krótko pozostały zmiany kursy w XTB (-2.12%) względem ostatniego sprawdzenia. Właściwa się, że współki XTB zmieniły kurs na 143.16 PLN, także w własnym zachodnym zachodnym (w POZIOMY CEN) XTB został 143.16 PLN. Następnie, w 28-12-2023, w PKN Orlen zmieniły k
> 1× XTB: 143.16 PLN (zmieniono kurs na -2.12%) PKN Orlen: 155.02 PLN (nie zmiany) Grupa Kęty: 1277 PLN (nie zmiany) Pepco: 47.45 PLN (nie zmiany) LPP: 26400 PLN (nie zmiany) Modivo: 93.26 PLN (nie zmiany) CD Projekt: 259.8 PLN (nie zmiany) Grupa Polsat: 17.72 PLN (nie zmiany)
> 1× W 28-1go 2023 roku, względem sprawdzenia, kurs XTB zmienił o -2.12% wzgl. poprzednymi zmianami. Jednak warto skoncentować się na aktualne kurs XTB, który w 28-1go to 143.16 PLN. W 28-1go 2023 roku, wzgl. poprzednymi zmianami, kurs XTB wzgl. poprzednymi zmianami wzławno w 143.16 PLN. Aktualnie (w

**Cykle bez ruchu, zestaw 1** (16 różnych wersji na 16 notatek, trzy najczęstsze):

> 1× W pulpicie zdania, zmienia się kursy spółek z listy w Grupa Kęty i LPP. Grupa Kęty: Oto krótką notatkę: Krótka zmiana kursu: 1284 PLN (z 1284 PLN) Oto kilka spółek: 1 Krótka zmiana kursu: 1284 PLN (z 1284 PLN) LPP: Oto k
> 1× Zapisz: Najwyżej cztery zdania: XTB: 151.02 PLN - Oto zmiany, które zmieniły kurs o 2% od ostatniego sprawdzenia. Grupa Kęty: 1288 PLN - Oto zmiany, które zmieniły kurs o 2% od ostatniego sprawdzenia. LPP: 24620 PLN - Oto zmiany, które zmieniły kurs o 2% od ostatniego spraw
> 1× XTB: 149.82 PLN (została odpowiedzonyma) PKN Orlen: 148.62 PLN (została odpowiedzonyma) Grupa Kęty: 1282 PLN (została odpowiedzonyma) Pepco: 43.17 PLN (została odpowiedzonyma) LPP: 24420 PLN (została odpowiedzonyma) Modivo: 96.7 PLN (została odpowiedzonyma) CD Projekt: 246.1 PLN (z

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 4× (pusto)
> 1× Zapisz: Najwyżej cztery zdania: XTB: 147.82 PLN Nie zmieniły kurs. PKN Orlen: 154.98 PLN Nie zmieniły kurs. Grupa Kęty: 1292 PLN Nie zmieniły kurs. CD Projekt: 262.4 PLN Nie zmieniły kurs. LPP: 26400 PLN Nie zmieniły kurs
> 1× Zapisz: Najwyżej cztery zdania: XTB: 147.82 PLN - Oto jedna z dziennych, który zmienił kurs na ostatniego sprawdzenia o 2% od ostatniego. Właściwa, że to nie było znaczących zmiany, nie wymieniają kursów i nie opisują poziomów cen jako wzrostów ani spadków. Grupa Kęty: 1292 PLN - *Jest to jedna z dzi

### 17. `nemotron-3-nano:4b` — miary 6/11, konsumowalne 0/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo ruszyła o 2,29% do 97,00 PLN, CD Projekt wzrosła o 2,29% do 247,40 PLN, PKN Orlen spadła o 2,29% do 148,18 PLN, LPP wzrosła o 2,29% do 24400,00 PLN, PKN Orlen spadła o 2,29% do 148,18 PLN, PKN Orlen spadła o 2,29% do 148,18 PLN, PKN Orlen spadła o 2,29% do 148,18 PLN, PKN Orlen spadła o 2,29% do
> 1× Modivo ruszyła o 2,29% do 97,00 PLN, PKN Orlen o 2,12% do 148,18 PLN, Grupa Kęty o 2,00% do 1260,00 PLN, CD Projekt o 2,00% do 247,40 PLN
> 1× Modivo ruszyła o 2,29% do 97,00 PLN, CD Projekt wzrosła o 2,29% do 247,40 PLN, PKN Orlen spadła o 2,29% do 148,18 PLN, LPP wzrosła o 2,29% do 24400,00 PLN, PKN Orlen, LPP, CD Projekt, Modivo, PKN Orlen, LPP, CD Projekt, Modivo

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN, PKN Orlen wzrosła o 0,28% do 150,42 PLN, Grupa Kęty wzrosła o 0,21% do 1267,00 PLN
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN, PKN Orlen wzrosła o 0,28% do 150,42 PLN, Grupa Kęty wzrosła o 0,15% do 1267,00 PLN, CD Projekt wzrosła o 0,21% do 238,90 PLN, LPP spadła o 0,23% do 24300,00 PLN, Modivo wzrosła o 0,22% do 87,34 PLN, KPN Orlen wzrosła o 0,28% do 150,42 PLN, CD Projekt wzrosła o 0,21%
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN, PKN Orlen wzrosła o 0,26% do 150,42 PLN, Grupa Kęty wzrosła o 0,02% do 1267,00 PLN
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN, a PKN Orlen wzrosła o 0,26% do 150,42 PLN, a LPP spadła o 0,27% do 24300,00 PLN, a Modivo wzrosła o 0,28% do 87,34 PLN, a CD Projekt spadła o 0,29% do 238,90 PLN, a KPN Orlen wzrosła o 0,26% do 150,42 PLN, a LPP spadła o 0,27% do 24300,00 PLN, a Modivo wzrosła o 0

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN, a PKN Orlen wzrosła o 0,07% do 151,14 PLN, a LPP wzrosła o 0,03% do 24160,00 PLN, a Modivo wzrosła o 0,02% do 88,06 PLN, a CD Projekt wzrosła o 0,01% do 236,40 PLN, a KPN Orlen wzrosła o 0,07% do 151,14 PLN, a LPP wzrosła o 0,03% do 24160,00 PLN, a Modivo wzrosła
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN, PKN Orlen wzrosła o 0,07% do 151,14 PLN, LPP spadła o 0,03% do 24160,00 PLN, CD Projekt wzrosła o 0,02% do 236,40 PLN, KPN Orlen wzrosła o 0,07% do 151,14 PLN, LPP spadła o 0,03% do 24160,00 PLN, CD Projekt wzrosła o 0,02% do 236,40 PLN, KPN Orlen wzrosła o 0,07
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN, PKN Orlen wzrosła o 0,07% do 151,14 PLN, LPP wzrosła o 0,03% do 24160,00 PLN, CD Projekt wzrosła o 0,01% do 236,40 PLN, KPN Orlen wzrosła o 0,07% do 151,14 PLN, LPP wzrosła o 0,03% do 24160,00 PLN, CD Projekt wzrosła o 0,01% do 236,40 PLN, KPN Orlen wzrosła o 0,
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN, PKN Orlen wzrosła o 0,07% do 151,14 PLN, LPP spadła o 0,03% do 24160,00 PLN, CD Projekt wzrosła o 0,01% do 236,40 PLN, KPN Orlen wzrosła o 0,07% do 151,14 PLN, LPP spadła o 0,03% do 24160,00 PLN, CD Projekt wzrosła o 0,01% do 236,40 PLN, KPN Orlen wzrosła o 0,07

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo 91,52 PLN, CD Projekt 243,60 PLN, Grupa Polsat 15,84 PLN, KGHM 349,35 PLN, brak
> 1× Modivo 91,52 PLN, CD Projekt 243,60 PLN, Grupa Polsat 15,84 PLN, KGHM 349,35 PLN. .
> 1× Modivo wzrosła do 91,52 PLN, CD Projekt do 243,60 PLN, Grupa Polsat do 15,84 PLN, KGHM do 349,35 PLN, brak

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB spadła o 2,12% do 143,16 PLN, PKN Orlen wzrosła o 2,00% do 155,02 PLN, Grupa Kęty wzrosła o 2,00% do 1277,00 PLN
> 1× XTB spadło o 2,12% do 143,16 PLN, PKN Orlen wzrosło o 2,12% do 155,02 PLN, LPP spadło o 2,12% do 26400 PLN, CD Projekt wzrosło o 2,12% do 259,80 PLN, GKHM wzrosło o 2,12% do 322,45 PLN, Benefit Systems spadło o 2,12% do 5615 PLN, Khgm wzrosło o 2,12% do 322,45 PLN, Benefit Systems spadło o 2,12% do 5615 PLN, Kh
> 1× XTB spadła o 2,12% do 143,16 PLN, PKN Orlen wzrosła o 2,03% do 155,02 PLN, Grupa Kęty wzrosła o 2,00% do 1277,00 PLN

**Cykle bez ruchu, zestaw 1** (3 różnych wersji na 16 notatek):

> 7× brak
> 5× Nie było znaczących zmian, nie wymieniaj kursów i pod żadnym pozorem nie opisuj poziomów cen jako wzrostów ani spadków.
> 4× nie było znaczących zmian, nie wymieniaj kursów i pod żadnym pozorem nie opisuj poziomów cen jako wzrostów ani spadków.

**Cykle bez ruchu, zestaw 2** (2 różnych wersji na 8 notatek):

> 4× brak
> 4× nie było znaczących zmian, nie wymieniaj kursów i pod żadnym pozorem nie opisuj poziomów cen jako wzrostów ani spadków.

