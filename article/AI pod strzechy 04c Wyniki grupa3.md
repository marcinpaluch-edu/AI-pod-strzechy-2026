# Część 4c. Wyniki — grupa 3: od 6 do 14 GB

W kontenerze możliwe wyłącznie przy 32 GB RAM z WSL-em na 16 GB.

Wszystkie modele na ustawieniach aplikacji (domyślnie `think: false`, 180 tokenów, temperatura 0,2, okno domyślne Ollamy; `gpt-oss:20b` z `think: "low"`). Jedenaście scenariuszy w dwóch zestawach (rozdział 3.1.1), każdy model w czterech przebiegach od czystej pamięci, czyli 44 notatki po polsku. Kolejność wierszy to ranking końcowy grupy według kryteriów z rozdziału 3.7: najpierw liczba scenariuszy z notatkami konsumowalnymi, potem polszczyzna bez wad, przy remisie ustawienia domyślne przed wpisem w konfiguracji i krótszy zimny start.

## Zestawienie modeli

| Miejsce | Model | Test z rozdz. 3 PL | A/B PL | Zestaw 2 | Test z rozdz. 3 EN | A/B EN | 11 scenariuszy: miary / konsumowalne | Główny problem PL | Główny problem EN | Zimny start (PL) | Rozmiar | Licencja | Dodany |
|---:|---|:---:|:---:|:---:|:---:|:---:|:---:|---|---|---:|---:|---|---|
| 1 | **`gemma4:12b`** | ✓ | ✓ | ✓ | ✓ | ✓ | 11 / **11** | **bez wad** w teście z rozdz. 3; w zestawie 2 kropka dziesiętna przy spadkach Polsatu (5 z 8 notatek) | **bez wad** | 34,6 s | 8,02 GB | Apache 2.0 | 2026-05-23 |
| 2 | **`ministral-3:14b`** | ✓ | ✗ | ✓ | ✓ | ✗ | 8 / **11** | kursy bez ruchu wbrew zakazowi, separator tysięcy kropką („1.288,00”) | „Current watchlist prices unchanged for others”, choć się zmieniły | 72,0 s | 9,08 GB | Apache 2.0 | 2025-10-31 |
| 3 | `gemma4:e4b` | ✗ | ✓ | ✓ | ✓ | ✓ | 10 / 10 | raz „2,29%” zamiast 2,30; „XTB utrzymuje kurs”, choć się zmienił | wypisuje kursy wszystkich spółek o 17:00 | 16,9 s | 9,61 GB | Apache 2.0 | 2026-03-02 |
| 4 | `qwen3.5:9b` | ✓ | ✓ | ✗ | ✓ | ✓ | 10 / 10 | kursy bez ruchu, kropka, szyk zdania; w zestawie 2 „wzrosło o 3.93 PLN” zamiast procentu | wypełniacze o poleceniach („so specific price levels are not reported”) | 37,8 s | 6,55 GB | Apache 2.0 | 2026-02-27 |
| 5 | `phi4` | ✓ | ✗ | ✗ | ✓ | ✓ | 11 / 9 | wypisuje kursy pozostałych spółek, także o 17:00 | **bez wad** | 67,5 s | 9,05 GB | MIT | 2024-12-11 |
| 6 | `ministral-3:8b` | ✗ | ✗ | ✗ | ✓ | ✗ | 10 / 8 | raz „97,30 PLN” zamiast 97,00; kursy bez ruchu wbrew zakazowi | **bez wad** | 54,6 s | 6,02 GB | Apache 2.0 | 2025-10-31 |
| 7 | `bielik-11b` | ✗ | ✓ | ✗ | ✗ | ✗ | 9 / 4 | kursy Polsatu jako „16.105.00 PLN”; kursy bez ruchu, „Brak spółek zmieniło kurs”, kropka | obcina „2.29%” — także bez polecenia zaokrąglania | 58,5 s | 6,72 GB | Apache 2.0\* | 2025-11-07 |
| 8 | `qwen3:14b` | ✗ | ✓ | ✗ | ✓ | ✓ | 10 / 4 | po opisie ruchu „Nie było znaczących zmian.”; w zestawie 2 przy czterech spółkach bez procentów | wypełniacz „Current price levels are available for reference” | 75,6 s | 9,28 GB | Apache 2.0 | 2025-04-27 |
| 9 | `falcon3:10b` | ✗ | ✗ | ✗ | ✓ | ✓ | 8 / 3 | „Grupa Polsat 16100 PLN”, ceny zaokrąglone do całości, zmyślony ruch KGHM | kursy w cyklach bez ruchu wbrew zakazowi | 48,4 s | 6,29 GB | TII Falcon (warunkowo) | 2024-12-14 |

\* Brak licencji w samym pliku modelu; readme na koncie wydawcy wskazuje Apache 2.0 wraz z regulaminem na bielik.ai/terms.

Testy: ✓ — wszystkie notatki testu konsumowalne (rozdział 3.5.1) i bez błędu treści przy lekturze, we wszystkich czterech przebiegach. A/B to ten sam test bez zdania o zaokrąglaniu. Miary to automat (rozdział 3.4), konsumowalne — notatki z prawdziwymi i kompletnymi danymi mimo usterek formy (rozdział 3.5.1). Pogrubienie: komplet jedenastu scenariuszy z notatkami konsumowalnymi.

## Wyniki per scenariusz

| Scenariusz | Zestaw | Start | Bez błędu w miarach | Konsumowalnych | Najszybszy konsumowalny |
|---|---|---|---:|---:|---|
| 23.09 09:00 — bez ruchu | 1 | **zimny** | 8/9 | 6/9 | `gemma4:e4b` 16,9 s |
| 23.09 11:00 — bez ruchu | 1 | ciepły | 7/9 | 7/9 | `gemma4:e4b` 12,6 s |
| 23.09 13:00 — bez ruchu | 1 | ciepły | 6/9 | 6/9 | `gemma4:e4b` 10,8 s |
| 23.09 15:00 — bez ruchu | 1 | ciepły | 9/9 | 6/9 | `gemma4:e4b` 13,4 s |
| 23.09 17:00 — Modivo +2,30% ▲ | 1 | ciepły | 7/9 | 6/9 | `qwen3.5:9b` 23,2 s |
| 18.09 15:00 — Grupa Polsat −2,63% ▼ | 2 | **zimny** | 9/9 | 8/9 | `gemma4:e4b` 23,1 s |
| 18.09 23:27 — Grupa Polsat −5,34% ▼ | 2 | ciepły | 9/9 | 7/9 | `gemma4:e4b` 17,6 s |
| 21.09 11:00 — cztery spółki ▲ | 2 | ciepły | 6/9 | 6/9 | `gemma4:e4b` 18,4 s |
| 19.10 09:00 — bez ruchu | 2 | ciepły | 9/9 | 5/9 | `gemma4:e4b` 14,1 s |
| 19.10 11:00 — bez ruchu, KGHM −1,97% | 2 | ciepły | 8/9 | 7/9 | `gemma4:e4b` 16,8 s |
| 19.10 13:00 — XTB −2,12% ▼ | 2 | ciepły | 9/9 | 6/9 | `gemma4:e4b` 11,6 s |

Czas to średnia z czterech przebiegów. Ostatnia kolumna dotyczy tylko danego scenariusza — w cyklach bez ruchu najszybciej piszą poprawnie także modele, które przy ruchu zmyślają.

## Test narzędzi

Wywołania to sześć prób: dwa pytania po trzy razy. Pętla to trzy próby pełnego obiegu — wywołanie, wykonanie, odpowiedź z liczbą 24 160 zł. Czasy są średnimi z prób; pierwsza próba każdego modelu startowała na zimno. Pogrubienie oznacza komplet. Opis testu w rozdziale 4.6.

| Model | Wywołania (z 6) | Pętla (z 3) | Czas wywołania | Czas pętli | Uwagi |
|---|:---:|:---:|---:|---:|---|
| `bielik-11b` | **6** | **3** | 6,7 s | 17,6 s | — |
| `qwen3.5:9b` | **6** | **3** | 16,1 s | 29,9 s | — |
| `gemma4:12b` | **6** | **3** | 13,4 s | 31,9 s | — |
| `gemma4:e4b` | **6** | **3** | 18,2 s | 43,0 s | — |
| `ministral-3:8b` | 6 | 2 | 5,9 s | 16,7 s | raz „241,60 PLN”; datę wymyśla: „19 października 2026” (3/3) |
| `qwen3:14b` | 6 | 0 | 32,1 s | 46,3 s | wywołuje bezbłędnie, ale po otrzymaniu wyniku zwraca pustą odpowiedź |
| `ministral-3:14b` | 2 | 0 | 9,4 s | 17,4 s | zapisuje wywołanie tekstem (`kurs_historyczny[ARGS]{…}`) zamiast je wykonać |
| `phi4` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |
| `falcon3:10b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |

## Komentarz

**Model `gemma4:12b` jest wyraźnym liderem grupy trzeciej.** Jako jedyny w tej grupie pisze po polsku bez wady powtarzalnej, a test z rozdziału trzeciego za każdym razem tak samo — w cyklu bez ruchu „Nie było znaczących zmian w kursach spółek na liście.” Procent z piętnastoma miejscami po przecinku zaokrągla sam, kurs „97” uzupełnia do „97,00”, pisze przecinkiem i nie wymienia kursów, kiedy nie trzeba. W zestawie drugim zdarza mu się jedynie kropka dziesiętna przy spadkach Polsatu — w pięciu notatkach z ośmiu. Ma też komplet w teście narzędzi.

Wszystkie jedenaście scenariuszy przechodzi w tej grupie jeszcze `ministral-3:14b`. Przegrywa z `gemma4:12b` polszczyzną — wbrew zakazowi wypisuje kursy pozostałych spółek — i nie nadaje się na agenta, bo wywołanie narzędzia zapisuje jako tekst, zamiast je wykonać. `phi4` ma dziewięć: też wypisuje kursy wszystkich spółek i w trzech notatkach wyczerpuje przy tym limit długości; narzędzi nie obsługuje w ogóle. Polski `bielik-11b` przechodzi dziewięć scenariuszy: dwa cykle bez ruchu traci przez kursy Polsatu zapisane jako „16.105.00 PLN” i „16.005.00 PLN”, a pozostałe jego wady — kropka dziesiętna, kursy w ciszy, niegramatyczne „Brak spółek zmieniło kurs” — nie odbierają notatkom konsumowalności. Grupa jest najsłabsza pod kątem narzędzi: komplet ma w niej czterech z dziewięciu.

## Notatki modeli — po polsku

Teksty po oczyszczeniu przez `sanitize()`, czyli tak, jak trafiają na pulpit, z liczbą wystąpień na cztery przebiegi. W cyklach z ruchem — wszystkie wersje; w cyklach bez ruchu — najczęstsze. Pełne odpowiedzi, także surowe, są w `data/badanie-*.jsonl`.

### 1. `gemma4:12b` — miary 11/11, konsumowalne 11/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 4× Spółka Modivo zmieniła kurs o 2,30% względem poprzedniego sprawdzenia. Aktualny kurs Modivo wynosi 97,00 PLN. Nie znam przyczyny tego ruchu.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 3× Spółka Grupa Polsat zmieniła kurs o -2,63%. Aktualny kurs Grupa Polsat wynosi 15,93 PLN. Nie znam przyczyny tego ruchu.
> 1× Spółka Grupa Polsat zmieniła kurs o -2.63% względem poprzedniego sprawdzenia. Aktualny kurs Grupa Polsat wynosi 15.93 PLN. Nie znam przyczyny tego ruchu.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 4× Spółka Grupa Polsat zmieniła kurs o -5.34% względem poprzedniego sprawdzenia. Aktualny kurs Grupa Polsat wynosi 15.08 PLN. Nie znam przyczyny tego ruchu.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Odnotowano istotne zmiany kursów dla czterech spółek. Modivo zmienił kurs o 3,93% do poziomu 91,52 PLN, CD Projekt o 3,05% do 243,60 PLN, Grupa Polsat o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Nie znam przyczyn tych ruchów.
> 1× Wykazały znaczące zmiany kursy Modivo (91,52 PLN, +3,93%), CD Projekt (243,6 PLN, +3,05%), Grupa Polsat (15,84 PLN, +5,04%) oraz KGHM (349,35 PLN, +3,65%). Nie znam przyczyn tych ruchów.
> 1× Wykazano istotne zmiany kursów dla czterech spółek. Modivo zmienił się o 3,93% do poziomu 91,52 PLN, CD Projekt o 3,05% do poziomu 243,60 PLN, Grupa Polsat o 5,04% do poziomu 15,84 PLN, a KGHM o 3,65% do poziomu 349,35 PLN. Nie znam przyczyn tych ruchów.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 4× Spółka XTB (XTB.WA) zmieniła kurs o -2,12% względem poprzedniego sprawdzenia. Aktualny kurs XTB wynosi 143,16 PLN. Nie znam przyczyny tego ruchu.

**Cykle bez ruchu, zestaw 1** (jedna wersja na 16 notatek):

> 16× Nie było znaczących zmian w kursach spółek na liście.

**Cykle bez ruchu, zestaw 2** (jedna wersja na 8 notatek):

> 8× Nie było znaczących zmian w kursach spółek na liście.

### 2. `ministral-3:14b` — miary 8/11, konsumowalne 11/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo zmienił kurs o 2,30% na 97,00 PLN. Pozostałe spółki z listy nie wykazały ruchu przekraczającego 2%. Aktualne kursy pozostałych spółek pozostają bez zmian znaczącej dynamiki.
> 1× Modivo zmienił kurs o 2,30% (97,00 PLN), przyczyna ruchu nieznana. Pozostałe spółki z listy utrzymują stabilne poziomy cen.
> 1× Modivo (MDV.WA) zmienił kurs o 2,30% do 97,00 PLN. Pozostałe spółki z listy nie wykazały ruchu przekraczającego 2%. Aktualne kursy pozostałych spółek pozostają bez zmian znaczącej dynamiki.
> 1× Modivo zmienił kurs o 2,30% do 97,00 PLN. Pozostałe spółki z listy nie wykazują zmian powyżej 2%. Kursy pozostałych spółek pozostają na poziomie: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260,00 PLN, Pepco 43,19 PLN, LPP 24400,00 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,97 PLN, Benefit Systems 5040,00 PLN, KGHM 341,55 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat (CPS.WA) zmieniła kurs o 2,63% w dół, obecnie notowana jest na 15,93 PLN. Pozostałe spółki z listy nie wykazały ruchu przekraczającego 2%. Przyczyny ruchu Grupy Polsat nie są znane.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o 2,63% w dół, obecnie notowana jest na 15,93 PLN. Pozostałe spółki z listy nie wykazały ruchów przekraczających 2%. Przyczyny zmiany nie są znane.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o 2,63% w dół, obecny poziom to 15,93 PLN. Pozostałe spółki z listy nie wykazały ruchu przekraczającego 2%. Przyczyny ruchu nie są znane.
> 1× Grupa Polsat (CPS.WA) spadła o 2,63% do 15,93 PLN. Pozostałe spółki z listy nie wykazały zmian powyżej 2%. Aktualne kursy pozostałych spółek dostępne na żądanie.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do 15,08 PLN. Pozostałe spółki z listy nie wykazały zmian powyżej 2%. Pozostałe kursy pozostają bez zmian znaczącej dynamiki.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do 15,08 PLN, przyczyny ruchu nie są znane. Pozostałe spółki z listy nie wykazały zmian przekraczających 2%.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do 15,08 PLN, przyczyny ruchu nie są znane. Pozostałe spółki na liście nie wykazały zmian o co najmniej 2%.
> 1× Grupa Polsat (CPS.WA) spadła o 5,34% do 15,08 PLN. Pozostałe spółki z listy nie wykazały zmian powyżej 2%. Aktualne kursy pozostałych spółek pozostają bez istotnych ruchów.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Na liście spółek z GPW znaczące zmiany notowań: Modivo (91,52 PLN, +3,93%), CD Projekt (243,60 PLN, +3,05%), Grupa Polsat (15,84 PLN, +5,04%) i KGHM (349,35 PLN, +3,65%). Przyczyny ruchu nie są znane. Pozostałe spółki utrzymują stabilne poziomy cen.
> 1× Na liście spółek z GPW notowanych powyżej 2% zmian: Modivo (91,52 PLN, +3,93%), CD Projekt (243,60 PLN, +3,05%), Grupa Polsat (15,84 PLN, +5,04%), KGHM (349,35 PLN, +3,65%). Przyczyny ruchu nie są znane. Pozostałe spółki w granicach oczekiwanych wahnięć.
> 1× Na liście spółek z GPW w porównaniu do ostatniego sprawdzenia zmiany przekraczające 2% wystąpiły u Modivo (91,52 PLN, +3,93%), CD Projekt (243,60 PLN, +3,05%), Grupy Polsat (15,84 PLN, +5,04%) i KGHM (349,35 PLN, +3,65%). Przyczyny ruchu nie są znane. Pozostałe spółki utrzymują stabilne poziomy cen.
> 1× Modivo (91,52 PLN) wzrosła o 3,93%, CD Projekt (243,60 PLN) o 3,05%, Grupa Polsat (15,84 PLN) o 5,04%, KGHM (349,35 PLN) o 3,65%. Przyczyny ruchu nie są znane. Pozostałe spółki utrzymują stabilne poziomy cen.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB.WA spadła o 2,12% do 143,16 PLN, przyczyna ruchu nie jest znana. Pozostałe spółki z listy utrzymują się bez znaczących zmian.
> 1× XTB.WA spadła o 2,12% do 143,16 PLN, przyczyna ruchu nie jest znana. Pozostałe spółki z listy nie wykazały zmian o co najmniej 2%.
> 1× XTB.WA spadła o 2,12% do 143,16 PLN, przyczyna ruchu nie jest znana. Pozostałe spółki z listy nie wykazały zmian powyżej 2%.

**Cykle bez ruchu, zestaw 1** (14 różnych wersji na 16 notatek, trzy najczęstsze):

> 2× Nie było znaczących zmian (2%+) w kursach spółek z listy. Aktualne poziomy cen pozostają na tym samym poziomie.
> 2× Nie było znaczących zmian (2%+) w kursach spółek z listy. Aktualne poziomy cen pozostają na podobnym poziomie jak poprzednio.
> 1× Nie było znaczących zmian (ponad 2%) wśród obserwowanych spółek. Aktualne kursy pozostają na poziomie: XTB 149,90 PLN, PKN Orlen 149,54 PLN, Grupa Kęty 1284,00 PLN, Pepco 44,12 PLN, LPP 24600,00 PLN, Modivo 96,42 PLN, CD Projekt 247,50 PLN, Grupa Polsat 16,14 PLN, Benefit Systems 5160,00 PLN, KGHM 347,25 PLN.

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Nie było znaczących zmian (2%+) w kursach spółek z listy. Aktualne poziomy cen pozostają bez istotnych ruchów od ostatniego monitoringu.
> 1× Nie było znaczących zmian (2%+) w kursach spółek z listy. Aktualne poziomy cen pozostają na poziomie: XTB 146,26 PLN, PKN Orlen 154,40 PLN, Grupa Kęty 1276,00 PLN, Pepco 47,89 PLN, LPP 26480,00 PLN, Modivo 93,80 PLN, CD Projekt 262,00 PLN, Grupa Polsat 18,00 PLN, Benefit Systems 5635,00 PLN, KGHM 321,30 PLN, Rainbow Tours 124,80 PLN.
> 1× Nie było znaczących zmian (ponad 2%) wśród spółek z listy. Aktualne poziomy cen pozostają na podobnym poziomie jak przy ostatnim sprawdzeniu.

### 3. `gemma4:e4b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) wzrosło o 2,30% do poziomu 97,00 PLN. Aktualny kurs Modivo wynosi 97,00 PLN. XTB utrzymuje się na poziomie 153,60 PLN, a PKN Orlen na poziomie 148,18 PLN.
> 1× Modivo (MDV.WA) wzrosło o 2,29% do poziomu 97,00 PLN. Aktualny kurs Modivo wynosi 97,00 PLN. XTB utrzymuje kurs na poziomie 153,60 PLN, a PKN Orlen na 148,18 PLN.
> 1× Modivo (MDV.WA) wzrosło o 2,30% do poziomu 97,00 PLN. Aktualny kurs Modivo wynosi 97,00 PLN. XTB notuje się na poziomie 153,60 PLN, a PKN Orlen na poziomie 148,18 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -2,63% do poziomu 15,93 PLN. Pozostałe spółki utrzymują kursy: XTB 150,68 PLN, PKN Orlen 150,42 PLN, Grupa Kęty 1267,00 PLN, Pepco 42,03 PLN, LPP 24300,00 PLN, Modivo 87,34 PLN, CD Projekt 238,90 PLN, Benefit Systems 5160,00 PLN, KGHM 343,35 PLN.
> 1× Grupa Polsat zmieniła kurs o -2,63% i obecnie notuje się na poziomie 15,93 PLN. Pozostałe spółki utrzymują swoje ostatnie poziomy cen.
> 1× Grupa Polsat zmieniła kurs o -2,63% do poziomu 15,93 PLN. Aktualne kursy to: XTB: 150,68 PLN, PKN Orlen: 150,42 PLN, Grupa Kęty: 1267,00 PLN, Pepco: 42,03 PLN, LPP: 24300,00 PLN, Modivo: 87,34 PLN, CD Projekt: 238,90 PLN, Grupa Polsat: 15,93 PLN, Benefit Systems: 5160,00 PLN, KGHM: 343,35 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat zmieniła kurs o -5,34% od ostatniego sprawdzenia, a jej aktualny kurs wynosi 15,08 PLN. Pozostałe spółki nie wykazały zmian przekraczających 2%.
> 1× Grupa Polsat zmieniła kurs o -5,34% do poziomu 15,08 PLN. Pozostałe spółki utrzymują kursy: XTB 150,34 PLN, PKN Orlen 151,14 PLN, Grupa Kęty 1269,00 PLN, Pepco 41,87 PLN, LPP 24160,00 PLN, Modivo 88,06 PLN, CD Projekt 236,40 PLN, Benefit Systems 5135,00 PLN, KGHM 337,05 PLN.
> 1× Grupa Polsat zmieniła kurs o -5,34% względem poprzedniego sprawdzenia, aktualny kurs to 15,08 PLN. Pozostałe spółki nie wykazały zmian przekraczających 2%. XTB notuje się na poziomie 150,34 PLN, PKN Orlen na poziomie 151,14 PLN, Grupa Kęty na poziomie 1269,00 PLN, Pepco na poziomie 41,87 PLN, LPP na poziomie 24160,00 PLN, Modivo na poziomie 88,06 PLN, CD Projekt na poziomie 236,40 PLN, Benefit Systems na poziomie 5135,00 PLN, KGHM na poziomie 337,05 PLN.
> 1× Grupa Polsat zmieniła kurs o -5,34% od ostatniego sprawdzenia, a jej aktualny kurs to 15,08 PLN. Pozostałe spółki utrzymują następujące kursy: XTB: 150,34 PLN, PKN Orlen: 151,14 PLN, Grupa Kęty: 1269,00 PLN, Pepco: 41,87 PLN, LPP: 24160,00 PLN, Modivo: 88,06 PLN, CD Projekt: 236,40 PLN, Benefit Systems: 5135,00 PLN, KGHM: 337,05 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo (MDV.WA) wzrosło o 3,93% do poziomu 91.52 PLN, CD Projekt (CDR.WA) wzrosło o 3,05% do 243.60 PLN, Grupa Polsat (CPS.WA) wzrosła o 5,04% do 15.84 PLN, a KGHM (KGH.WA) wzrosło o 3,65% do 349.35 PLN.
> 1× Modivo (MDV.WA) wzrosła o 3,93% do 91.52 PLN, CD Projekt (CDR.WA) wzrosło o 3,05% do 243.60 PLN, Grupa Polsat (CPS.WA) wzrosła o 5,04% do 15.84 PLN, a KGHM (KGH.WA) wzrosło o 3,65% do 349.35 PLN.
> 1× Modivo (MDV.WA) wzrosło o 3.93% do 91.52 PLN, CD Projekt (CDR.WA) wzrosło o 3.05% do 243.60 PLN, Grupa Polsat (CPS.WA) wzrosło o 5.04% do 15.84 PLN, a KGHM (KGH.WA) wzrosło o 3.65% do 349.35 PLN.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB zmienił kurs o -2.12% i obecnie notuje się na poziomie 143.16 PLN. Pozostałe spółki nie wykazały zmian przekraczających 2% od ostatniego sprawdzenia.
> 1× XTB zmienił kurs o -2.12% i wynosi obecnie 143.16 PLN. Pozostałe spółki nie wykazały zmian przekraczających 2%.
> 1× XTB zmienił kurs o -2.12% do poziomu 143.16 PLN. Pozostałe spółki nie wykazały zmian przekraczających 2%.
> 1× XTB zmienił kurs o -2,12% i obecnie notuje się na poziomie 143,16 PLN. Pozostałe spółki nie wykazały zmian przekraczających 2% od ostatniego sprawdzenia.

**Cykle bez ruchu, zestaw 1** (7 różnych wersji na 16 notatek, trzy najczęstsze):

> 5× Nie było znaczących zmian na monitorowanej liście spółek.
> 5× Nie było znaczących zmian na liście spółek.
> 2× Nie było znaczących zmian na liście spółek. XTB: 151.02 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1288.00 PLN, Pepco: 43.47 PLN, LPP: 24620.00 PLN, Modivo: 96.30 PLN, CD Projekt: 245.30 PLN, Grupa Polsat: 16.11 PLN, Benefit Systems: 5120.00 PLN, KGHM: 348.35 PLN.

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Nie było znaczących zmian na liście spółek.
> 2× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy to: XTB: 146.26 PLN, PKN Orlen: 154.40 PLN, Grupa Kęty: 1276.00 PLN, Pepco: 47.89 PLN, LPP: 26480.00 PLN, Modivo: 93.80 PLN, CD Projekt: 262.00 PLN, Grupa Polsat: 18.00 PLN, Benefit Systems: 5635.00 PLN, KGHM: 321.30 PLN, Rainbow Tours: 124.80 PLN.
> 2× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy spółek to: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292.00 PLN, Pepco: 48.00 PLN, LPP: 26400.00 PLN, Modivo: 92.90 PLN, CD Projekt: 262.40 PLN, Grupa Polsat: 18.30 PLN, Benefit Systems: 5730.00 PLN, KGHM: 327.75 PLN, Rainbow Tours: 126.70 PLN.

### 4. `qwen3.5:9b` — miary 10/11, konsumowalne 10/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Zmieniły kurs spółki Modivo (MDV.WA) na poziomie 97,00 PLN o 2,30%. Powód ruchu nie jest znany. Pozostałe spółki z listy: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260,00 PLN, Pepco 43,19 PLN, LPP 24400,00 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,97 PLN, Benefit Systems 5040,00 PLN oraz KGHM 341,55 PLN.
> 1× Zmienił kurs Modivo na 97,00 PLN (wzrost o 2,30%). Brak innych istotnych ruchów cenowych w pozostałych spółkach. Przyczyna zmiany nie jest znana.
> 1× Zmieniły kurs spółki Modivo (MDV.WA) o 2,30% na poziomie 97,00 PLN. Brak innych istotnych ruchów cenowych w pozostałych pozycjach listy. Powód zmiany nie jest znany.
> 1× Modivo zmieniła kurs o 2.30% na poziomie 97,00 PLN. Brak innych znaczących ruchów w pozostałych spółkach z listy. Nieznana jest przyczyna tej zmiany.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 2.63% do 15.93 PLN, a przyczyna tego ruchu nie jest znana. Pozostałe spółki z listy nie zmieniły kursu o co najmniej 2%. Nie ma innych znaczących zmian na rynku.
> 1× Grupa Polsat zmieniła kurs o -2.63% na poziomie 15.93 PLN. Brak innych znaczących ruchów w pozostałych spółkach z listy. Nieznana jest przyczyna tej zmiany.
> 1× Grupa Polsat zmieniła kurs o -2.63% na poziomie 15.93 PLN, a przyczyna ruchu nie jest znana. Pozostałe spółki z listy nie wykazały istotnych zmian wyceny.
> 1× Grupa Polsat zmieniła kurs o -2,63% na poziomie 15,93 PLN. Brak innych spółek z listy spełniających warunek zmiany o co najmniej 2%. Nie znam przyczyny tego ruchu.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5.34% do poziomu 15.08 PLN, co przekracza próg 2%. Brak innych znaczących zmian na liście monitorowanych spółek. Przyczyna ruchu kursowego nie jest znana.
> 1× Grupa Polsat spadła o 5,34% do poziomu 15,08 PLN. Powód tego ruchu nie jest znany. Pozostałe spółki z listy nie zmieniły kursu o co najmniej 2%.
> 1× Grupa Polsat spadła o 5,34% do kursu 15,08 PLN. Brak innych znaczących zmian na liście spółek. Nieznana jest przyczyna tego ruchu cenowego.
> 1× Grupa Polsat spadła o 5.34% do kursu 15.08 PLN, co przekracza próg 2%. Brak innych istotnych zmian na liście spółek. Powód ruchu nie jest znany.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Modivo wzrosło o 3.93% do 91.52 PLN, CD Projekt o 3.05% do 243.60 PLN, Grupa Polsat o 5.04% do 15.84 PLN, a KGHM o 3.65% do 349.35 PLN. Nie znaleziono przyczyn tych zmian.
> 1× Zmiany notowań zaobserwowano w Modivo (91,52 PLN), CD Projekt (243,60 PLN), Grupa Polsat (15,84 PLN) oraz KGHM (349,35 PLN). Spółki te zmieniły kurs odpowiednio o 3,93%, 3,05%, 5,04% i 3,65%. Nie znaleziono przyczyn tych ruchów.
> 1× Zmieniły kurs Modivo (91,52 PLN), CD Projekt (243,60 PLN), Grupa Polsat (15,84 PLN) oraz KGHM (349,35 PLN). Spółki te poruszyły się odpowiednio o 3,93%, 3,05%, 5,04% i 3,65% względem poprzedniego sprawdzenia. Nie znam przyczyn tych ruchów cenowych.
> 1× Modivo wzrosło o 3.93 PLN do 91.52 zł, CD Projekt o 3.05 PLN do 243.60 zł, Grupa Polsat o 5.04 PLN do 15.84 zł, a KGHM o 3.65 PLN do 349.35 zł. Przyczyn tych ruchów nie jest znana. Aktualne poziomy cen pozostałych spółek z listy nie uległy istotnej zmianie w ostatnim sprawdzaniu.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× Spółka XTB zmieniła kurs o -2.12% na poziomie 143.16 PLN. Brak innych znaczących ruchów na liście monitorowanych spółek. Nie znana jest przyczyna tej zmiany.
> 1× Spółka XTB zmieniła kurs o -2,12% na poziomie 143,16 PLN. Brak innych znaczących ruchów w pozostałych pozycjach monitorowania. Nieznana przyczyna tego spadku.
> 1× Spółka XTB zmieniła kurs o -2.12% na poziomie 143.16 PLN. Brak innych istotnych zmian w pozostałych pozycjach listy. Nie znana jest przyczyna tego ruchu cenowego.

**Cykle bez ruchu, zestaw 1** (16 różnych wersji na 16 notatek, trzy najczęstsze):

> 1× Brak spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Aktualne poziomy cen to: XTB: 149.90 PLN, PKN Orlen: 149.54 PLN, Grupa Kęty: 1284.00 PLN, Pepco: 44.12 PLN, LPP: 24600.00 PLN, Modivo: 96.42 PLN, CD Projekt: 247.50 PLN, Grupa Polsat: 16.14 PLN, Benefit Systems: 5160.00 PLN, KGHM: 347.25 PLN.
> 1× Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne poziomy cen to: XTB: 151.02 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1288 PLN, Pepco: 43.47 PLN, LPP: 24620 PLN, Modivo: 96.3 PLN, CD Projekt: 245.3 PLN, Grupa Polsat: 16.105 PLN, Benefit Systems: 5120 PLN, KGHM: 348.35 PLN.
> 1× Brak spółek z listy, które zmieniły kurs o co najmniej 2%. Aktualne poziomy cen to: XTB: 149.82 PLN, PKN Orlen: 148.62 PLN, Grupa Kęty: 1282 PLN, Pepco: 43.17 PLN, LPP: 24420 PLN, Modivo: 96.7 PLN, CD Projekt: 246.1 PLN, Grupa Polsat: 16.005 PLN, Benefit Systems: 5150 PLN, KGHM: 347.65 PLN.

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× Brak spółek na liście monitoringu, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Wszystkie podane pozycje notowane są obecnie bez znaczących wahań cenowych. Nie ma potrzeby podawania aktualnych poziomów dla tych instrumentów.
> 1× Brak spółek na liście, które zmieniły kurs o co najmniej 2%. Aktualne poziomy cen to: XTB: 146.26 PLN, PKN Orlen: 154.40 PLN, Grupa Kęty: 1276.00 PLN, Pepco: 47.89 PLN, LPP: 26480.00 PLN, Modivo: 93.80 PLN, CD Projekt: 262.00 PLN, Grupa Polsat: 18.00 PLN, Benefit Systems: 5635.00 PLN, KGHM: 321.30 PLN, Rainbow Tours: 124.80 PLN.
> 1× Brak spółek ze znaczącymi zmianami kursowymi w monitorowanej liście. Aktualne poziomy cen: XTB 147,82 PLN, PKN Orlen 154,98 PLN, Grupa Kęty 1292 PLN, Pepco 48 PLN, LPP 26400 PLN, Modivo 92,9 PLN, CD Projekt 262,4 PLN, Grupa Polsat 18,3 PLN, Benefit Systems 5730 PLN, KGHM 327,75 PLN, Rainbow Tours 126,7 PLN.

### 5. `phi4` — miary 11/11, konsumowalne 9/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) zmienił kurs o 2,30% względem poprzedniego sprawdzenia, osiągając 97 PLN. Aktualne kursy innych spółek to: XTB 153,60 PLN, PKN Orlen 148,18 PLN, Grupa Kęty 1260 PLN, Pepco 43,19 PLN, LPP 24400 PLN, CD Projekt 247,40 PLN, Grupa Polsat 15,965 PLN, Benefit Systems 5040 PLN, KGHM 341,55 PLN. Nie znane są przyczyny ruchu kursu Modivo.
> 1× Modivo (MDV.WA) zmienił kurs o 2.30% względem poprzedniego sprawdzenia, obecnie wynoszący 97 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2%. Aktualne poziomy cen: XTB 153.6 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260 PLN, Pepco 43.19 PLN, LPP 24400 PLN, CD Projekt 247.4 PLN, Grupa Polsat 15.965 PLN, Benefit Systems 5040 PLN, KGHM 341.55 PLN.
> 1× Modivo (MDV.WA) zmienił kurs o 2,30% względem poprzedniego sprawdzenia, osiągając 97 PLN. Aktualne kursy spółek z listy to: XTB: 153,6 PLN, PKN Orlen: 148,18 PLN, Grupa Kęty: 1260 PLN, Pepco: 43,19 PLN, LPP: 24400 PLN, CD Projekt: 247,4 PLN, Grupa Polsat: 15,965 PLN, Benefit Systems: 5040 PLN, KGHM: 341,55 PLN. Nie znane są przyczyny ruchu kursu Modivo.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat zmieniła kurs o -2.63% i obecnie wynosi 15.93 PLN. Inne spółki na liście nie wykazały znaczących zmian. Aktualne kursy: XTB: 150.68 PLN, PKN Orlen: 150.42 PLN, Grupa Kęty: 1267 PLN, Pepco: 42.03 PLN, LPP: 24300 PLN, Modivo: 87.34 PLN, CD Projekt: 238.9 PLN, Benefit Systems: 5160 PLN, KGHM: 343.35 PLN.
> 1× Grupa Polsat zmieniła kurs o -2.63%, obecnie wynosząc 15.93 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy dla innych spółek to: XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267 PLN, Pepco 42.03 PLN, LPP 24300 PLN, Modivo 87.34 PLN, CD Projekt 238.9 PLN, Benefit Systems 5160 PLN, KGHM 343.35 PLN.
> 1× Grupa Polsat zmieniła kurs o -2.63%, obecnie wynosząc 15.93 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2%. Aktualne kursy: XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267 PLN, Pepco 42.03 PLN, LPP 24300 PLN, Modivo 87.34 PLN, CD Projekt 238.9 PLN, Benefit Systems 5160 PLN, KGHM 343.35 PLN. Nie znane są przyczyny ruchu kursu Grupy Polsat.
> 1× Grupa Polsat zmieniła kurs o -2.63% i obecnie wynosi 15.93 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2%. Aktualne kursy: XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267 PLN, Pepco 42.03 PLN, LPP 24300 PLN, Modivo 87.34 PLN, CD Projekt 238.9 PLN, Benefit Systems 5160 PLN, KGHM 343.35 PLN. Nie znane są przyczyny zmiany kursu Grupy Polsat.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat zmieniła kurs o -5.34% do 15.08 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy dla pozostałych spółek to: XTB: 150.34 PLN, PKN Orlen: 151.14 PLN, Grupa Kęty: 1269.00 PLN, Pepco: 41.87 PLN, LPP: 24160.00 PLN, Modivo: 88.06 PLN, CD Projekt: 236.40 PLN, Benefit Systems: 5135.00 PLN, KGHM: 337.05 PLN. Nie znane są przyczyny ruch
> 1× Grupa Polsat zmieniła kurs o -5.34% i obecnie wynosi 15.08 PLN. Aktualne kursy innych spółek to: XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269 PLN, Pepco 41.87 PLN, LPP 24160 PLN, Modivo 88.06 PLN, CD Projekt 236.4 PLN, Benefit Systems 5135 PLN, KGHM 337.05 PLN. Nie znane są przyczyny ruchu.
> 1× Grupa Polsat zmieniła kurs o -5.34% i obecnie wynosi 15.08 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy dla pozostałych spółek to: XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269 PLN, Pepco 41.87 PLN, LPP 24160 PLN, Modivo 88.06 PLN, CD Projekt 236.4 PLN, Benefit Systems 5135 PLN, KGHM 337.05 PLN. Nie znane są przyczyny ruchu kursu Grupy Polsat.
> 1× Grupa Polsat zmieniła kurs o -5.34%, obecnie wynosząc 15.08 PLN. Inne spółki na liście nie wykazały zmiany kursu o co najmniej 2%. Aktualne kursy: XTB 150.34 PLN, PKN Orlen 151.14 PLN, Grupa Kęty 1269 PLN, Pepco 41.87 PLN, LPP 24160 PLN, Modivo 88.06 PLN, CD Projekt 236.4 PLN, Benefit Systems 5135 PLN, KGHM 337.05 PLN. Nie znane są przyczyny ruchu kursu Grupy Polsat.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Modivo wzrosło o 3,93% do 91,52 PLN, CD Projekt o 3,05% do 243,6 PLN, Grupa Polsat o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Aktualne kursy: XTB 149,84 PLN, PKN Orlen 152,42 PLN, Grupa Kęty 1261 PLN, Pepco 41,9 PLN, LPP 23960 PLN, Benefit Systems 5115 PLN. Nie znane są przyczyny tych zmian.
> 1× Modivo wzrósł o 3,93% do 91,52 PLN, CD Projekt wzrósł o 3,05% do 243,6 PLN, Grupa Polsat wzrosła o 5,04% do 15,84 PLN, a KGHM wzrósł o 3,65% do 349,35 PLN. Nie znane są konkretne przyczyny tych ruchów. Aktualne kursy: Modivo 91,52 PLN, CD Projekt 243,6 PLN, Grupa Polsat 15,84 PLN, KGHM 349,35 PLN.
> 1× Modivo wzrosło o 3,93% do 91,52 PLN, CD Projekt o 3,05% do 243,6 PLN, Grupa Polsat o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Aktualne kursy: XTB 149,84 PLN, PKN Orlen 152,42 PLN, Grupa Kęty 1261 PLN, Pepco 41,9 PLN, LPP 23960 PLN, Benefit Systems 5115 PLN. Nie znane są przyczyny tych ruchów.
> 1× Modivo wzrósł o 3,93% do 91,52 PLN, CD Projekt o 3,05% do 243,6 PLN, Grupa Polsat o 5,04% do 15,84 PLN, a KGHM o 3,65% do 349,35 PLN. Aktualne kursy: XTB 149,84 PLN, PKN Orlen 152,42 PLN, Grupa Kęty 1261 PLN, Pepco 41,9 PLN, LPP 23960 PLN, Benefit Systems 5115 PLN. Nie znane są przyczyny tych zmian.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Kurs XTB spadł o 2,12% do 143,16 PLN. Pozostałe spółki na liście nie wykazały zmiany kursu o co najmniej 2% od ostatniego sprawdzenia. Aktualne poziomy cen dla innych spółek to: PKN Orlen 155,02 PLN, Grupa Kęty 1277 PLN, Pepco 47,45 PLN, LPP 26400 PLN, Modivo 93,26 PLN, CD Projekt 259,8 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615 PLN, KGHM 322,45 PLN, Rainbow Tours 125,1 PLN. Nie znane są konkretne przyczyny ruchu kurs
> 1× Kurs XTB spadł o 2,12% do 143,16 PLN. Pozostałe spółki na liście nie wykazały zmian kursu o co najmniej 2% od ostatniego sprawdzenia. Kursy pozostałych spółek to: PKN Orlen 155,02 PLN, Grupa Kęty 1277 PLN, Pepco 47,45 PLN, LPP 26400 PLN, Modivo 93,26 PLN, CD Projekt 259,8 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615 PLN, KGHM 322,45 PLN, Rainbow Tours 125,1 PLN.
> 1× Kurs XTB spadł o 2,12% do 143,16 PLN. Pozostałe spółki nie wykazały zmiany kursu o co najmniej 2% od ostatniego sprawdzenia. Aktualne poziomy cen dla innych spółek to: PKN Orlen 155,02 PLN, Grupa Kęty 1277 PLN, Pepco 47,45 PLN, LPP 26400 PLN, Modivo 93,26 PLN, CD Projekt 259,8 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615 PLN, KGHM 322,45 PLN, Rainbow Tours 125,1 PLN. Przyczyn ruchu kursu XTB nie podano.
> 1× Kurs XTB (XTB.WA) spadł o 2,12% do 143,16 PLN. Pozostałe spółki na liście nie wykazały zmiany kursu o co najmniej 2% od ostatniego sprawdzenia. Aktualne poziomy cen dla innych spółek to: PKN Orlen 155,02 PLN, Grupa Kęty 1277 PLN, Pepco 47,45 PLN, LPP 26400 PLN, Modivo 93,26 PLN, CD Projekt 259,8 PLN, Grupa Polsat 17,72 PLN, Benefit Systems 5615 PLN, KGHM 322,45 PLN, Rainbow Tours 125,1 PLN. Nie znane są przyczyny r

**Cykle bez ruchu, zestaw 1** (10 różnych wersji na 16 notatek, trzy najczęstsze):

> 4× Nie było znaczących zmian w kursach spółek z listy. Aktualne poziomy cen to: XTB: 151,02 PLN, PKN Orlen: 148,18 PLN, Grupa Kęty: 1288,00 PLN, Pepco: 43,47 PLN, LPP: 24620,00 PLN, Modivo: 96,30 PLN, CD Projekt: 245,30 PLN, Grupa Polsat: 16,105 PLN, Benefit Systems: 5120,00 PLN, KGHM: 348,35 PLN.
> 2× Nie było znaczących zmian w kursach spółek z listy. Aktualne poziomy cen to: XTB: 149,90 PLN, PKN Orlen: 149,54 PLN, Grupa Kęty: 1284,00 PLN, Pepco: 44,12 PLN, LPP: 24600,00 PLN, Modivo: 96,42 PLN, CD Projekt: 247,50 PLN, Grupa Polsat: 16,14 PLN, Benefit Systems: 5160,00 PLN, KGHM: 347,25 PLN.
> 2× Nie było znaczących zmian w kursach spółek z listy. Aktualne poziomy cen to: XTB: 149,82 PLN, PKN Orlen: 148,62 PLN, Grupa Kęty: 1282,00 PLN, Pepco: 43,17 PLN, LPP: 24420,00 PLN, Modivo: 96,70 PLN, CD Projekt: 246,10 PLN, Grupa Polsat: 16,005 PLN, Benefit Systems: 5150,00 PLN, KGHM: 347,65 PLN.

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian w kursach spółek z listy. Aktualne poziomy cen to: XTB: 147,82 PLN, PKN Orlen: 154,98 PLN, Grupa Kęty: 1292,00 PLN, Pepco: 48,00 PLN, LPP: 26400,00 PLN, Modivo: 92,90 PLN, CD Projekt: 262,40 PLN, Grupa Polsat: 18,30 PLN, Benefit Systems: 5730,00 PLN, KGHM: 327,75 PLN, Rainbow Tours: 126,70 PLN.
> 2× Nie było znaczących zmian w kursach spółek z listy. Aktualne poziomy cen to: XTB: 146.26 PLN, PKN Orlen: 154.4 PLN, Grupa Kęty: 1276 PLN, Pepco: 47.89 PLN, LPP: 26480 PLN, Modivo: 93.8 PLN, CD Projekt: 262 PLN, Grupa Polsat: 18 PLN, Benefit Systems: 5635 PLN, KGHM: 321.3 PLN, Rainbow Tours: 124.8 PLN.
> 1× Nie było znaczących zmian w kursach spółek z listy. Aktualne poziomy cen to: XTB: 147,82 PLN, PKN Orlen: 154,98 PLN, Grupa Kęty: 1292,00 PLN, Pepco: 48,00 PLN, LPP: 26 400,00 PLN, Modivo: 92,90 PLN, CD Projekt: 262,40 PLN, Grupa Polsat: 18,30 PLN, Benefit Systems: 5730,00 PLN, KGHM: 327,75 PLN, Rainbow Tours: 126,70 PLN.

### 6. `ministral-3:8b` — miary 10/11, konsumowalne 8/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) skoczyła o 2,30% – aktualnie 97,00 PLN. Pozostałe spółki nie przekroczyły progów zmian.
> 1× Modivo (MDV.WA) skoczyła o 2,30% do 97,00 PLN. Pozostałe spółki na liście nie przekroczyły progu 2% zmiany. Aktualne ceny pozostałych spółek dostępne w pełnej aktualizacji.
> 1× Na liście zmiany: Modivo (MDV.WA) – kurs 97,30 PLN, ruch o 2,30%. Aktualne ceny pozostałych spółek z ruchami: Modivo: 97,30 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN. Pozostałe spółki z listy nie przekroczyły progów zmian. Aktualne kursy pozostałych spółek dostępne w notyfikacji.
> 1× Grupa Polsat spadła o 2,63% – obecnie 15,93 PLN. Pozostałe spółki nie przekroczyły progów zmian.
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN. Pozostałe spółki nie przekroczyły progu 2% zmiany. Aktualne ceny dostępne w liście.
> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN. Pozostałe spółki nie przekroczyły progu 2% zmiany.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN. Pozostałe spółki z listy nie przekroczyły progu 2% zmiany. Aktualne kursy pozostałych spółek dostępne w pełnej wyszczególnieniu.
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN. Pozostałe spółki nie przekroczyły progów zmian. Aktualne kursy spółek z listy dostępne poniżej.
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN. Pozostałe spółki z listy nie przekroczyły progów zmian. Aktualne ceny pozostałych spółek dostępne w pełnej liście.
> 1× Grupa Polsat spadła o 5,34% do 15,08 PLN. Pozostałe spółki nie przekroczyły progu 2% zmiany.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Na liście ruchu: Modivo (91,52 PLN, +3,93%), CD Projekt (243,60 PLN, +3,05%), Grupa Polsat (15,84 PLN, +5,04%), KGHM (349,35 PLN, +3,65%). Przyczyny ruchu nieznane.
> 1× Na liście ruchy: Modivo (91,52 PLN, +3,93%), CD Projekt (243,60 PLN, +3,05%), Grupa Polsat (15,84 PLN, +5,04%), KGHM (349,35 PLN, +3,65%). Przyczyny zmian nieznane.
> 1× Na liście zmiany: Modivo (91,52 PLN) wzrosła o 3,93%, CD Projekt (243,60 PLN) o 3,05%, Grupa Polsat (15,84 PLN) o 5,04%, KGHM (349,35 PLN) o 3,65%. Przyczyny ruchu nieznane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB.WA spadła o 2,12% – aktualnie 143,16 PLN. Pozostałe spółki na liście nie przekroczyły progu 2%.
> 1× XTB.WA spadła o 2,12% – aktualnie 143,16 PLN. Pozostałe spółki na liście nie przekroczyły progów zmian.
> 1× XTB.WA spadł o 2,12% – aktualny kurs: 143,16 PLN. Pozostałe spółki nie przekroczyły progów zmian.
> 1× XTB.WA spadła o 2,12%, aktualny kurs to 143,16 PLN. Aktualne ceny: XTB: 143,16 PLN PKN Orlen: 155,02 PLN Grupa Kęty: 1277,00 PLN Pepco: 47,45 PLN LPP: 26400,00 PLN Modivo: 93,26 PLN CD Projekt: 259,80 PLN Grupa Polsat: 17,72 PLN Benefit Systems: 5615,00 PLN KGHM: 322,45 PLN Rainbow Tours:

**Cykle bez ruchu, zestaw 1** (14 różnych wersji na 16 notatek, trzy najczęstsze):

> 2× Nie było znaczących zmian. Aktualne ceny na liście: XTB: 149,82 PLN PKN Orlen: 148,62 PLN Grupa Kęty: 1282,00 PLN Pepco: 43,17 PLN LPP: 24420,00 PLN Modivo: 96,70 PLN CD Projekt: 246,10 PLN Grupa Polsat: 16,01 PLN Benefit Systems: 5150,00 PLN KGHM: 347,65 PLN.
> 2× Nie było znaczących zmian. Aktualne kursy: XTB: 149,82 PLN PKN Orlen: 148,62 PLN Grupa Kęty: 1282,00 PLN Pepco: 43,17 PLN LPP: 24420,00 PLN Modivo: 96,70 PLN CD Projekt: 246,10 PLN Grupa Polsat: 16,01 PLN Benefit Systems: 5150,00 PLN KGHM: 347,65 PLN.
> 1× Nie było znaczących zmian. Aktualne kursy na liście: XTB: 149,90 PLN PKN Orlen: 149,54 PLN Grupa Kęty: 1284,00 PLN Pepco: 44,12 PLN LPP: 24600,00 PLN Modivo: 96,42 PLN CD Projekt: 247,50 PLN Grupa Polsat: 16,14 PLN Benefit Systems: 5160,00 PLN KGHM: 347,25 PLN

**Cykle bez ruchu, zestaw 2** (6 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Nie było znaczących zmian. Aktualne kursy na liście: XTB: 146,26 PLN PKN Orlen: 154,40 PLN Grupa Kęty: 1276,00 PLN Pepco: 47,89 PLN LPP: 26480,00 PLN Modivo: 93,80 PLN CD Projekt: 262,00 PLN Grupa Polsat: 18,00 PLN Benefit Systems: 5635,00 PLN KGHM: 321,30 PLN Rainbow Tours: 124,80 PLN
> 1× Nie było znaczących zmian. Aktualne ceny na liście: XTB: 147,82 PLN PKN Orlen: 154,98 PLN Grupa Kęty: 1292,00 PLN Pepco: 48,00 PLN LPP: 26400,00 PLN Modivo: 92,90 PLN CD Projekt: 262,40 PLN Grupa Polsat: 18,30 PLN Benefit Systems: 5730,00 PLN KGHM: 327,75 PLN Rainbow Tours: 126,70 PLN
> 1× Nie było znaczących zmian. Aktualne kursy na liście: XTB: 147,82 PLN PKN Orlen: 154,98 PLN Grupa Kęty: 1292,00 PLN Pepco: 48,00 PLN LPP: 26400,00 PLN Modivo: 92,90 PLN CD Projekt: 262,40 PLN Grupa Polsat: 18,30 PLN Benefit Systems: 5730,00 PLN KGHM: 327,75 PLN Rainbow Tours: 126,70 PLN

### 7. `bielik-11b` — miary 9/11, konsumowalne 4/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo (MDV.WA) wzrosło o 2.30% do 97 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.
> 1× Modivo (MDV.WA) wzrosło o 2.30% do 97 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%. Aktualne kursy: XTB 153.60 PLN, PKN Orlen 148.18 PLN, Grupa Kęty 1260.00 PLN, Pepco 43.19 PLN, LPP 24400.00 PLN, CD Projekt 247.40 PLN, Grupa Polsat 15.97 PLN, Benefit Systems 5040.00 PLN, KGHM 341.55 PLN.
> 1× Modivo (MDV.WA) wzrosło o 2.30% do 97.00 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 4× Grupa Polsat (CPS.WA) zmieniła kurs o -2.63% do 15.93 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat (CPS.WA) zmieniła kurs o -5.34% do 15.08 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.
> 1× Grupa Polsat (CPS.WA) zmieniła kurs o -5.34% do 15.08 PLN. Pozostałe spółki nie odnotowały znaczących zmian.
> 1× Grupa Polsat zmieniła kurs o -5.34% do 15.08 PLN. Pozostałe spółki nie odnotowały znaczących zmian.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo (MDV.WA) wzrosło o 3.93% do 91.52 PLN, CD Projekt (CDR.WA) wzrósł o 3.05% do 243.60 PLN, Grupa Polsat (CPS.WA) wzrosła o 5.04% do 15.84 PLN, KGHM (KGH.WA) wzrósł o 3.65% do 349.35 PLN. Przyczyny zmian nie są znane.
> 1× Modivo wzrosło o 3.93% do 91.52 PLN, CD Projekt wzrósł o 3.05% do 243.60 PLN, Grupa Polsat wzrosła o 5.04% do 15.84 PLN, KGHM wzrósł o 3.65% do 349.35 PLN. Przyczyny zmian nie są znane.
> 1× Modivo (MDV.WA) wzrosło o 3.93% do 91.52 PLN, CD Projekt (CDR.WA) wzrósł o 3.05% do 243.60 PLN, Grupa Polsat (CPS.WA) wzrosła o 5.04% do 15.84 PLN, KGHM (KGH.WA) wzrósł o 3.65% do 349.35 PLN. Przyczyny zmian nieznane.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 3× XTB zmieniło kurs o -2.12% do 143.16 PLN. Pozostałe spółki nie odnotowały znaczących zmian.
> 1× XTB zmienił kurs o -2.12% do 143.16 PLN. Pozostałe spółki nie zmieniły kursu o co najmniej 2%.

**Cykle bez ruchu, zestaw 1** (9 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Brak spółek zmieniło kurs o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy: XTB: 151.02 PLN, PKN Orlen: 148.18 PLN, Grupa Kęty: 1288.00 PLN, Pepco: 43.47 PLN, LPP: 24620.00 PLN, Modivo: 96.30 PLN, CD Projekt: 245.30 PLN, Grupa Polsat: 16.105.00 PLN, Benefit Systems: 5120.00 PLN, KGHM: 348.
> 3× Brak spółek zmieniło kurs o co najmniej 2%. Aktualne kursy: XTB: 149.82 PLN, PKN Orlen: 148.62 PLN, Grupa Kęty: 1282.00 PLN, Pepco: 43.17 PLN, LPP: 24420.00 PLN, Modivo: 96.70 PLN, CD Projekt: 246.10 PLN, Grupa Polsat: 16.005.00 PLN, Benefit Systems: 5150.00 PLN, KGHM: 347.65 PLN.
> 2× Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy: XTB: 149.90 PLN, PKN Orlen: 149.54 PLN, Grupa Kęty: 1284.00 PLN, Pepco: 44.12 PLN, LPP: 24600.00 PLN, Modivo: 96.42 PLN, CD Projekt: 247.50 PLN, Grupa Polsat: 16.14 PLN, Benefit Systems: 5160.00 PLN, KGHM: 347.25 PLN.

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292.00 PLN, Pepco: 48.00 PLN, LPP: 26400.00 PLN, Modivo: 92.90 PLN, CD Projekt: 262.40 PLN, Grupa Polsat: 18.30 PLN, Benefit Systems: 5730.00 PLN, KGHM: 327.75 PLN, Rainbow Tours:
> 2× Brak spółek zmieniło kurs o co najmniej 2% od ostatniego sprawdzenia. Aktualne kursy: XTB: 146.26 PLN, PKN Orlen: 154.40 PLN, Grupa Kęty: 1276.00 PLN, Pepco: 47.89 PLN, LPP: 26480.00 PLN, Modivo: 93.80 PLN, CD Projekt: 262.00 PLN, Grupa Polsat: 18.00 PLN, Benefit Systems: 5635.00 PLN, KGHM: 321.30 PLN
> 1× Brak spółek zmieniło kurs o co najmniej 2%. Aktualne kursy: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292.00 PLN, Pepco: 48.00 PLN, LPP: 26400.00 PLN, Modivo: 92.90 PLN, CD Projekt: 262.40 PLN, Grupa Polsat: 18.30 PLN, Benefit Systems: 5730.00 PLN, KGHM: 327.75 PLN, Rainbow Tours: 12

### 8. `qwen3:14b` — miary 10/11, konsumowalne 4/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Zmieniła się cena Modivo o 2,30% względem poprzedniego sprawdzenia. Aktualny kurs Modivo to 97,00 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.
> 1× Zmieniła się cena Modivo o 2,30% względem poprzedniego sprawdzenia. Kurs Modivo to 97,00 PLN. Nie znana jest przyczyna zmiany. Brak innych spółek z listy z zmianą co najmniej 2%.
> 1× Zmieniła się cena Modivo o 2,30% względem poprzedniego sprawdzenia. Kurs Modivo to 97,00 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.
> 1× Zmieniła się cena Modivo o 2,30% względem poprzedniego sprawdzenia. Kurs spółki wynosi 97,00 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -2,63% od ostatniego sprawdzenia. Aktualny kurs spółki to 15,93 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.
> 1× Grupa Polsat zmieniła kurs o -2,63% od ostatniego sprawdzenia. Grupa Polsat: 15,93 PLN. Nie znana jest przyczyna zmiany kursu. Nie było znaczących zmian.
> 1× Grupa Polsat zmieniła kurs o -2,63% od ostatniego sprawdzenia. Grupa Polsat: 15,93 PLN. Nie znana jest przyczyna ruchu. Nie było znaczących zmian.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat zmieniła kurs o -5,34% od ostatniego sprawdzenia. Kurs spółki wynosi obecnie 15,08 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.
> 1× Grupa Polsat zmieniła kurs o -5.34% względem poprzedniego sprawdzenia. Aktualny kurs spółki wynosi 15.08 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.
> 1× Grupa Polsat zmieniła kurs o -5,34% od ostatniego sprawdzenia. Kurs spółki wynosi 15,08 PLN. Nie znana jest przyczyna zmiany. Nie było znaczących zmian.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Zmieniły kurs o co najmniej 2% Modivo (91.52 PLN), CDR.WA (243.60 PLN), CPS.WA (15.84 PLN) i KGH.WA (349.35 PLN). Nie znana jest przyczyna ruchu. Brak innych znaczących zmian. Kursy spółek podane są z dokładnością do dwóch miejsc po przecinku.
> 1× Zmieniły kurs o co najmniej 2% Modivo (MDV.WA): 91.52 PLN, CDR.WA: 243.60 PLN, CPS.WA: 15.84 PLN, KGH.WA: 349.35 PLN. Nie znana przyczyna ruchu. Pozostałe spółki na liście utrzymują poziomy cen z poprzedniego sprawdzenia. Brak informacji o wydarzeniach wpływających na zmiany.
> 1× Modivo, CDR.WA, Grupa Polsat, CPS.WA i KGHM zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Modivo: 91.52 PLN, CDR.WA: 243.60 PLN, CPS.WA: 15.84 PLN, KGHM: 349.35 PLN. Przyczyna ruchu nie jest znana. Nie było znaczących zmian.
> 1× Zmieniły kurs o co najmniej 2% Modivo (MDV.WA): 91.52 PLN, CDR.WA: 243.60 PLN, CPS.WA: 15.84 PLN, KGH.WA: 349.35 PLN. Nie znana przyczyna ruchu. Brak innych spółek z istotnymi zmianami. Aktualne kursy spółek z listy: XTB: 149.84 PLN, PKN Orlen: 152.42 PLN, Grupa Kęty: 1261.00 PLN, Pepco: 41.90 PLN, LPP: 23960.0

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× Zmieniła kurs o co najmniej 2% spółka XTB: 143,16 PLN. Nie znana jest przyczyna ruchu. Pozostałe spółki na liście nie spełniają kryterium zmiany kursu o 2% lub więcej. Kursy spółek: XTB: 143,16 PLN.
> 1× Zmieniła kurs o co najmniej 2% spółka XTB: 143,16 PLN. Nie znana jest przyczyna ruchu. Brak innych spółek z listy z takimi zmianami. Aktualne kursy spółek: XTB: 143,16 PLN, PKN Orlen: 155,02 PLN, Grupa Kęty: 1277,00 PLN, Pepco: 47,45 PLN, LPP: 26400,00 PLN, Modivo: 93,26 PLN, CD Projekt: 259,80 PLN, Grupa Polsat: 17,72 PLN, Benefit Systems
> 1× Zmieniła kurs o co najmniej 2% spółka XTB: 143,16 PLN. Nie znana jest przyczyna ruchu. Pozostałe spółki na liście nie miały znaczących zmian. Kursy spółek podane są bez oceny ich trendu.

**Cykle bez ruchu, zestaw 1** (8 różnych wersji na 16 notatek, trzy najczęstsze):

> 4× Nie było znaczących zmian. Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy spółek z listy: XTB: 149,90 PLN, PKN Orlen: 149,54 PLN, Grupa Kęty: 1284,00 PLN, Pepco: 44,12 PLN, LPP: 24600,00 PLN, Modivo: 96,42 PLN, CD Projekt: 247,50 PLN, Grupa Polsat: 16,135 PLN, Benefit Systems: 5160,00 PLN, KGHM: 347,25 PLN
> 3× Nie było znaczących zmian. Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy spółek z listy: XTB: 152,10 PLN, PKN Orlen: 148,32 PLN, Grupa Kęty: 1262,00 PLN, Pepco: 43,15 PLN, LPP: 24300,00 PLN, Modivo: 94,82 PLN, CD Projekt: 245,80 PLN, Grupa Polsat: 15,94 PLN, Benefit Systems: 5065,00 PLN, KGHM: 343,80 PLN.
> 3× Nie było znaczących zmian. Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy spółek z listy: XTB: 151,02 PLN, PKN Orlen: 148,18 PLN, Grupa Kęty: 1288,00 PLN, Pepco: 43,47 PLN, LPP: 24620,00 PLN, Modivo: 96,30 PLN, CD Projekt: 245,30 PLN, Grupa Polsat: 16,11 PLN, Benefit Systems: 5120,00 PLN, KGHM: 348,35 PLN.

**Cykle bez ruchu, zestaw 2** (3 różnych wersji na 8 notatek):

> 4× brak
> 3× Nie było znaczących zmian. Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy spółek z listy: XTB: 147,82 PLN, PKN Orlen: 154,98 PLN, Grupa Kęty: 1292,00 PLN, Pepco: 48,00 PLN, LPP: 26400,00 PLN, Modivo: 92,90 PLN, CD Projekt: 262,40 PLN, Grupa Polsat: 18,30 PLN, Benefit Systems: 5730,00 PLN, KGHM: 327,75 PLN,
> 1× Nie było znaczących zmian. Brak spółek, które zmieniły kurs o co najmniej 2%. Aktualne kursy spółek to: XTB: 147.82 PLN, PKN Orlen: 154.98 PLN, Grupa Kęty: 1292.00 PLN, Pepco: 48.00 PLN, LPP: 26400.00 PLN, Modivo: 92.90 PLN, CD Projekt: 262.40 PLN, Grupa Polsat: 18.30 PLN, Benefit Systems: 5730.00 PLN, KGHM: 327.75 PLN, Rainbow Tours

### 9. `falcon3:10b` — miary 8/11, konsumowalne 3/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Polska: Modivo (MDV.WA) 97 PLN, zmiana 2.30%.
> 1× Polska: Modivo (MDV.WA) zmieniło kurs o 2.299093018350566%, teraz stanowi 97 PLN.
> 1× Polska: Modivo (97 PLN) zmieniło kurs o 2.299093018350566%.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× PO Polsku: Grupa Polsat (CPS.WA) zmieniła kurs o -2.63% do 15.93 PLN. Kursy innych spółek: XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267 PLN, Pepco 42.03 PLN, LPP 24300 PLN, Modivo 87.34 PLN, CD Projekt 238.9 PLN, Benefit Systems 5160 PLN, KGHM 343.35 PLN.
> 1× Polsat Group: 15.93 PLN, -2.63% od ostatniego sprawdzenia.
> 1× Polsat kursy zmieniły się o -2.63%. Aktualne kursy: XTB 150.68 PLN, PKN Orlen 150.42 PLN, Grupa Kęty 1267 PLN, Pepco 42.03 PLN, LPP 24300 PLN, Modivo 87.34 PLN, CD Projekt 238.9 PLN, Polsat 15.93 PLN, Benefit Systems 5160 PLN, KGHM 343.35 PLN.
> 1× Polsat Group (CPS.WA) spadło o 2,63% do 15,93 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 3× Polsat Group: 15.08 PLN, -5.34% od ostatniego sprawdzenia.
> 1× PO Polsku: Grupa Polsat (CPS.WA) spadła o 5,34% do 15,08 PLN.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Ostatnie sprawdzenie: Modivo (91.52 PLN, 3.93%), CD Projekt (243.6 PLN, 3.05%), Grupa Polsat (15.84 PLN, 5.04%), KGHM (349.35 PLN, 3.65%).
> 1× W Polsce dzisiaj aktualizacja: Modivo (91.52 PLN), CD Projekt (243.6 PLN), Grupa Polsat (15.84 PLN), KGHM (349.35 PLN) zmienili kursy o co najmniej 2% od ostatniego sprawdzenia.
> 1× W Polsce dzisiaj: Modivo (91.52 PLN) wzrosło o 3.93%, CD Projekt (243.6 PLN) o 3.05%, Grupa Polsat (15.84 PLN) o 5.04%, KGHM (349.35 PLN) o 3.65%.
> 1× Oto krótkie notatka PO POLSKU: Modivo (91.52 PLN) wzrosło 3.93%, CD Projekt (243.6 PLN) 3.05%, Grupa Polsat (15.84 PLN) 5.04%, KGHM (349.35 PLN) 3.65%.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB (XTB.WA) 143.16 PLN, -2.12% od ostatniego sprawdzenia.
> 1× XTB (XTB.WA) spadło o 2,12% na 143,16 PLN.
> 1× XTB (XTB.WA) zmieniło kurs o -2.12% na 143.16 PLN. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems, KGHM, Rainbow Tours - aktualne kursy: 155.02 PLN, 1277 PLN, 47.45 PLN, 26400 PLN, 93.26 PLN, 259.8 PLN, 17.72 PLN, 5615 PLN, 322.45 PLN, 125.1 PLN.
> 1× XTB (XTB.WA) zmieniło kurs o -2.12% do 143.16 PLN.

**Cykle bez ruchu, zestaw 1** (13 różnych wersji na 16 notatek, trzy najczęstsze):

> 2× PO Polsku: XTB: 151.02 PLN PKN Orlen: 148.18 PLN Grupa Kęty: 1288 PLN Pepco: 43.47 PLN LPP: 24620 PLN Modivo: 96.3 PLN CD Projekt: 245.3 PLN Grupa Polsat: 16.105 PLN Benefit Systems: 5120 PLN KGHM: 348.35 PLN Brak spółek z zmianą kursu o co najmniej 2%.
> 2× PO Polsku: XTB: 150 PLN PKN Orlen: 149 PLN Grupa Kęty: 1280 PLN Pepco: 43 PLN LPP: 24400 PLN Modivo: 97 PLN CD Projekt: 246 PLN Grupa Polsat: 16000 PLN Benefit Systems: 5200 PLN KGHM: 348 PLN Brak znaczących zmian kursów.
> 2× PO Polsku: XTB: 150 PLN PKN Orlen: 150 PLN Grupa Kęty: 1280 PLN Pepco: 44 PLN LPP: 24600 PLN Modivo: 96 PLN CD Projekt: 248 PLN Grupa Polsat: 16100 PLN Benefit Systems: 5160 PLN KGHM: 347 PLN Brak znaczących zmian kursów.

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Ostatni sprawdzenie na liście GPW w Polsce nie wykazał znaczących zmian kursów spółek.
> 1× PO Polsku: XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292 PLN Pepco: 48 PLN LPP: 26400 PLN Modivo: 92.9 PLN CD Projekt: 262.4 PLN Grupa Polsat: 18.3 PLN Benefit Systems: 5730 PLN KGHM: 327.75 PLN Rainbow Tours: 126.7 PLN Brak spółek z zmianą kursu o co najmniej 2%.
> 1× Spółki zmieniające kurs o co najmniej 2% od ostatniego sprawdzenia: brak.

