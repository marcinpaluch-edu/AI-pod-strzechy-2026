# Część 4a. Wyniki — grupa 1: poniżej 2 GB

Zmieszczą się wszędzie, także na sprzęcie słabszym niż zakładany. Badam je dla porządku i po to, żeby pokazać, gdzie leży dolna granica sensu. Dla tej grupy dołożyłem badanie ratunkowe: zestaw drugi z promptem angielskim.

Wszystkie modele na ustawieniach aplikacji (domyślnie `think: false`, 180 tokenów, temperatura 0,2, okno domyślne Ollamy; `gpt-oss:20b` z `think: "low"`). Jedenaście scenariuszy w dwóch zestawach (rozdział 3.1.1), każdy model w czterech przebiegach od czystej pamięci, czyli 44 notatki po polsku. Kolejność wierszy to ranking końcowy grupy według kryteriów z rozdziału 3.7: najpierw liczba scenariuszy z notatkami konsumowalnymi, potem polszczyzna bez wad, przy remisie ustawienia domyślne przed wpisem w konfiguracji i krótszy zimny start.

## Zestawienie modeli

| Miejsce | Model | Test z rozdz. 3 PL | A/B PL | Zestaw 2 | Test z rozdz. 3 EN | A/B EN | Zestaw 2 EN | 11 scenariuszy: miary / konsumowalne | Główny problem PL | Główny problem EN | Zimny start (PL) | Rozmiar | Licencja | Dodany |
|---:|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|---|---:|---:|---|---|
| 1 | `qwen3.5:0.8b` | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | 6 / 6 | przy każdym ruchu pisze „spadł”, zmyśla ruchy innych spółek | kierunek dobry, ale zmyśla ruchy wszystkich pozostałych spółek („Grupa Kęty rose 16.92%”) | 4,8 s | 1,04 GB | Apache 2.0 | 2026-02-28 |
| 2 | `granite4:micro-h` | ✗ | ✗ | ✗ | ✓ | ✓ | ✗ | 9 / 6 | zmyśla ruchy innych spółek, obcina „5,33%”; każdą notatkę o ruchu zaczyna od „Wczoraj” | wypisuje kursy wszystkich spółek, czasem zamiast notatki samo „None”; w zestawie 2 gubi CD Projekt przy czterech spółkach i obcina „5.33%” | 16,5 s | 1,94 GB | Apache 2.0 | 2025-09-16 |
| 3 | `llama3.2:1b` | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | 8 / 2 | zamiast notatki przepisuje dane | zmyśla ruchy („PKN Orlen increased by 2.12%”), „increased” przy spadku Polsatu | 6,7 s | 1,32 GB | Llama 3.2 Community (warunkowo) | 2024-09-18 |
| 4 | `granite4:1b-h` | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | 3 / 2 | ruch o 17:00 zwykle dobrze, ale w cyklach bez ruchu zmyśla procenty | pojedyncze ruchy bezbłędnie, ale w ciszy zmyśla „XTB: +1.82”; przy czterech spółkach zostawia samo Modivo | 7,5 s | 1,56 GB | Apache 2.0 | 2025-10-07 |
| 5 | `granite4:350m-h` | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | 6 / 1 | „Polecki asystent monitorujący listę spółek GPW.”, ruch gubi | na każdy prompt „No significant changes” — gubi wszystkie ruchy | 2,5 s | 370 MB | Apache 2.0 | 2025-10-07 |
| 6 | `falcon3:1b` | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | 6 / 0 | bełkot i przepisany prompt | przepisuje surowe procenty, dopisuje XTB ruch Polsatu, „XTB increased” przy spadku | 11,8 s | 1,78 GB | TII Falcon (warunkowo) | 2024-12-14 |
| 7 | `exaone3.5:2.4b` | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | 2 / 0 | obcina procent do „2,29%”, zmyśla ruchy, „zwiększony o −5,34%” | obcina „2.29%”, wypisuje kursy; w zestawie 2 gubi KGHM przy czterech spółkach | 14,5 s | 1,64 GB | EXAONE 1.1 – NC (niekomercyjna) | 2024-12-01 |

Testy: ✓ — wszystkie notatki testu konsumowalne (rozdział 3.5.1) i bez błędu treści przy lekturze, we wszystkich czterech przebiegach. A/B to ten sam test bez zdania o zaokrąglaniu. Miary to automat (rozdział 3.4), konsumowalne — notatki z prawdziwymi i kompletnymi danymi mimo usterek formy (rozdział 3.5.1). Pogrubienie: komplet jedenastu scenariuszy z notatkami konsumowalnymi.

## Wyniki per scenariusz

| Scenariusz | Zestaw | Start | Bez błędu w miarach | Konsumowalnych | Najszybszy konsumowalny |
|---|---|---|---:|---:|---|
| 23.09 09:00 — bez ruchu | 1 | **zimny** | 6/7 | 2/7 | `qwen3.5:0.8b` 4,8 s |
| 23.09 11:00 — bez ruchu | 1 | ciepły | 6/7 | 2/7 | `qwen3.5:0.8b` 2,6 s |
| 23.09 13:00 — bez ruchu | 1 | ciepły | 5/7 | 2/7 | `qwen3.5:0.8b` 2,6 s |
| 23.09 15:00 — bez ruchu | 1 | ciepły | 5/7 | 2/7 | `qwen3.5:0.8b` 2,7 s |
| 23.09 17:00 — Modivo +2,30% ▲ | 1 | ciepły | 1/7 | 1/7 | `llama3.2:1b` 6,1 s |
| 18.09 15:00 — Grupa Polsat −2,63% ▼ | 2 | **zimny** | 4/7 | 2/7 | `llama3.2:1b` 5,7 s |
| 18.09 23:27 — Grupa Polsat −5,34% ▼ | 2 | ciepły | 0/7 | 0/7 | — |
| 21.09 11:00 — cztery spółki ▲ | 2 | ciepły | 1/7 | 0/7 | — |
| 19.10 09:00 — bez ruchu | 2 | ciepły | 5/7 | 2/7 | `qwen3.5:0.8b` 2,4 s |
| 19.10 11:00 — bez ruchu, KGHM −1,97% | 2 | ciepły | 6/7 | 4/7 | `granite4:350m-h` 2,1 s |
| 19.10 13:00 — XTB −2,12% ▼ | 2 | ciepły | 1/7 | 0/7 | — |

Czas to średnia z czterech przebiegów. Ostatnia kolumna dotyczy tylko danego scenariusza — w cyklach bez ruchu najszybciej piszą poprawnie także modele, które przy ruchu zmyślają.

## Test narzędzi

Wywołania to sześć prób: dwa pytania po trzy razy. Pętla to trzy próby pełnego obiegu — wywołanie, wykonanie, odpowiedź z liczbą 24 160 zł. Czasy są średnimi z prób; pierwsza próba każdego modelu startowała na zimno. Pogrubienie oznacza komplet. Opis testu w rozdziale 4.6.

| Model | Wywołania (z 6) | Pętla (z 3) | Czas wywołania | Czas pętli | Uwagi |
|---|:---:|:---:|---:|---:|---|
| `granite4:350m-h` | **6** | **3** | 0,6 s | 1,3 s | — |
| `granite4:1b-h` | **6** | **3** | 2,0 s | 4,3 s | — |
| `granite4:micro-h` | **6** | **3** | 2,6 s | 5,6 s | — |
| `qwen3.5:0.8b` | **6** | **3** | 3,1 s | 5,9 s | — |
| `llama3.2:1b` | 3 | 3 | 1,2 s | 2,6 s | o listę spółek pyta narzędziem kursu historycznego (3/3) |
| `falcon3:1b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |
| `exaone3.5:2.4b` | — | — | — | — | Ollama odrzuca: manifest nie deklaruje narzędzi |

## Komentarz

**Grupa pierwsza nie ma zwycięzcy.** Żaden model nie przechodzi wszystkich jedenastu scenariuszy ani po polsku, ani po angielsku. Najbliżej jest `granite4:micro-h`: obcina procenty („5,33%”), każdą notatkę o ruchu zaczyna od „Wczoraj” — umieszczenie wydarzeń w czasie wynika zapewne z jego danych treningowych, nie z promptu — i raz dopisuje zmyślone „CD Projekt wzruszyło się o 0,77%”. Po angielsku przechodzi test z rozdziału trzeciego, ale zestaw drugi z promptem angielskim, czyli badanie ratunkowe dla tej grupy, oblewają wszystkie siedem modeli: przy czterech spółkach naraz każdy gubi którąś spółkę albo psuje procent — `qwen3.5:0.8b` podaje wszystkie cztery, ale obcina je („3.92%”) albo skraca do jednego miejsca. `granite4:1b-h` podaje po angielsku każdy pojedynczy ruch bezbłędnie, ale w cyklach bez ruchu zmyśla zmiany („XTB: +1.82”).

Wysokie wyniki w miarach są w tej grupie złudne: cykle bez ruchu zalicza automat każdemu modelowi, który nie podaje żadnych liczb — także notatce „Polecki asystent monitorujący listę spółek GPW.”. Konsumowalność tego już nie przepuszcza.

## Notatki modeli — po polsku

Teksty po oczyszczeniu przez `sanitize()`, czyli tak, jak trafiają na pulpit, z liczbą wystąpień na cztery przebiegi. W cyklach z ruchem — wszystkie wersje; w cyklach bez ruchu — najczęstsze. Pełne odpowiedzi, także surowe, są w `data/badanie-*.jsonl`.

### 1. `qwen3.5:0.8b` — miary 6/11, konsumowalne 6/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo spadł o 2,30%, a pozostałe spółki na liście nie zmieniły kursu.
> 1× Modivo spadł o 2,30%, a pozostałe spółki nie zmieniły cen ani kursów w ciągu ostatnich dwóch godzin.
> 1× Modivo spadł 2,30% od poprzedniego sprawdzenia na poziomie 97 PLN. PKN Orlen wzrósł o 1,48%, Grupa Kęty spadła o 5,60%, Pepco wzrosł o 1,70%, LPP spadł o 2,40%, CD Projekt spadł o 3,40%, Grupa Polsat spadła o 3,90%, Benefit Systems spadł o 4,80%.
> 1× Modivo spadł o 2,30%, a pozostałe spółki nie zmieniły cen ani kursów.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 2,63% do 15,93 PLN, a PKN Orlen wzrósł o 0,42%. Grupa Kęty, Pepco i CD Projekt nie zmieniły cen ani nie wykazały zmian.
> 1× Grupa Polsat spadła o 2,6% na poziomie 15,93 PLN, a PKN Orlen zniknął ze listy. Grupa Kęty wzrosła o 1048 PLN do 1267 PLN, Pepco spadł o 17,97 PLN na 42,03 PLN, LPP wzrosł o 21865 PLN do 24300 PLN, Modivo spadł o 8.4 PLN na 87,34 PLN, CD Projekt wzrosł o 15,07 PLN na 238,9 PLN i KGHM spadł o 1.05 PLN na 343,35 PLN.
> 1× Grupa Polsat spadła o 2,63%, a pozostałe spółki nie zmieniły cen w ciągu ostatnich dwóch dni.
> 1× Grupa Polsat spadła o 2,63% od poprzedniego sprawdzenia na poziomie 15,93 PLN. Zmiany w tym zakresie dotyczyły tylko spółek z listy: PKN Orlen (150,42 PLN), XTB (150,68 PLN) oraz Grupa Kęty (1267 PLN).

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat spadła o 5,34% na poziomie 15,08 PLN, a PKN Orlen wzrósł o 2,69% do 151,14 PLN. Grupa Kęty i Benefit Systems nie wykazały istotnych zmian w cenach.
> 1× Grupa Polsat spadła -5,34%, a pozostałe spółki nie zmieniły cen ani nie wykazały istotnych zmian w ciągu ostatnich dni.
> 1× Grupa Polsat spadła 5,34% na poziomie 15,08 PLN, a PKN Orlen wzrósł o 2,76% do 151,14 PLN. Grupa Kęty i CD Projekt również wykazały istotne zmiany cenowe w tym samym zakresie.
> 1× Grupa Polsat spadła o 5,34% na poziomie 15,08 PLN, a PKN Orlen wzrósł o 2,79% do 151,14 PLN. Grupa Kęty i Benefit Systems nie miały znaczących zmian w ciągu ostatnich dwóch dni.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Modivo, CD Projekt i Grupa Polsat zmieniły kurs o co najmniej 2% względem poprzedniego sprawdzenia. W tym przypadku zmiany w poziomie cen nie są istotne dla inwestycji.
> 1× Spółki na listie zmieniły kurs o co najmniej 2% względem poprzedniego sprawdzenia: Modivo (91,52 PLN), CD Projekt (243,6 PLN), Grupa Polsat (15,84 PLN) oraz KGHM (349,35 PLN). Wszystkie pozostałe spółki na listie nie wykazały istotnych zmian w cenach.
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM zmieniły kurs o co najmniej 2% względem poprzedniego sprawdzenia. Modivo spadł z 91,52 PLN do 91,52 PLN, CD Projekt wzrósł z 243,6 PLN do 243,6 PLN, Grupa Polsat spadł z 15,84 PLN do 15,84 PLN a KGHM wzrósł z 349,35 PLN do 349,35 PLN.
> 1× Modivo, CD Projekt, Grupa Polsat i KGHM zmieniły kurs o co najmniej 2% względem poprzedniego sprawdzenia. Spółki XTB, PKN Orlen, Grupa Kęty, Pepco i LPP nie spełniają tego kryterium. Brak znaczących zmian w cenach na liście spółek z GPW.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB spadł o 2,12%, PKN Orlen wzrósł o 0,38%, Grupa Kęty nie zmieniła kursu, Pepco spadł o 0,57%, LPP nie zmieniła kursu, Modivo spadł o 0,49%, CD Projekt spadł o 0,62%, Grupa Polsat spadł o 1,38%, Benefit Systems spadł o 0,98%, KGHM spadł o 0,57%.
> 1× XTB spadł o 2,12% do 143,16 PLN, a PKN Orlen wzrósł o 0,05% do 155,02 PLN. Grupa Kęty i Pepco nie zmieniły kursu.
> 1× XTB spadł o 2,12%, PKN Orlen wzrósł o 0,48%, Grupa Kęty nie zmieniła kursu, Pepco spadł o 9,75%, LPP spadł o 96,35%, Modivo spadł o 1,80%, CD Projekt spadł o 2,28%, Grupa Polsat spadł o 94,35%, Benefit Systems spadł o 9,85%, KGHM spadł o 1,79%.
> 1× XTB spadł 2,12% do 143,16 PLN, a pozostałe spółki na listie nie zmieniły cen ani nie wykazały istotnych zmian w ciągu ostatnich dwóch dni.

**Cykle bez ruchu, zestaw 1** (10 różnych wersji na 16 notatek, trzy najczęstsze):

> 6× Brak znaczących zmian w cenach spółek na listie GPW.
> 2× Brak znaczących zmian w poziomieach cen spółek na listie GPW.
> 1× Brak znaczących zmian w cenach spółek na listę GPW. Wszystkie wymienione pozycje pozostają w stabilnym poziomie cenowym.

**Cykle bez ruchu, zestaw 2** (3 różnych wersji na 8 notatek):

> 6× Brak znaczących zmian w cenach spółek na listie GPW.
> 1× Brak znaczących zmian w poziomieach cen spółek na listie GPW. Wszystkie wymienione pozycje nie zostały zmienione o co najmniej 2% od ostatniego sprawdzenia.
> 1× Brak znaczących zmian w poziomieach cen spółek na GPW.

### 2. `granite4:micro-h` — miary 9/11, konsumowalne 6/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Wczoraj rynek giełdowy pokazał ruch w kierunku wzrostu dla kilku spółek. Modivo (MDV.WA) zarekordował wzrost o 2.30%, spowijaszając się do 97,00 PLN. Innymi spółkami, które doświadczyły podobnych zmian, są XTB (153,60 PLN), PKN Orlen (148,18 PLN), Grupa Kęty (1260,00 PLN), Pepco (43,19 PLN), LPP (24400,00 PLN), CD Projekt (247,40 PLN), Grupa Polsat (15,97 PLN) oraz KGHM (341,55 PLN). Wszystkie pozostałe spół
> 1× Wczoraj ruch na górze był obserwowany na spółce Modivo (MDV.WA), która zmieniła kurs o 2,30% do 97,00 PLN. Pozostałe spółki na liście nie znacznie się zmieniły w cenach. Aktualne ceny to: XTB - 153,60 PLN, PKN Orlen - 148,18 PLN, Grupa Kęty - 1260,00 PLN, Pepco - 43,19 PLN, LPP - 24400,00 PLN, CD Projekt - 247,40 PLN, Grupa Polsat - 15,97 PLN, Benefit Systems - 5040,00 PLN, KGHM - 341,55 PLN. Nie było zn
> 1× Wczoraj ruch na góre obserwujemy w spółkach Modivo (MDV.WA) oraz CD Projekt. Modivo wzruszyło się o 2,30% do 97 PLN, a CD Projekt wzruszyło się o 0,77% do 247,40 PLN. Wszelkie inne spółki na liście nie znacznie się zmieniły w cenach.
> 1× Wczoraj ruch na góre na giełdzie polskiej był obserwowany, zwłaszcza w przypadku Modivo, który wzrósł o 2,30% do 97,00 PLN. Innymi spółkami, które również pokazują te same tendencje, są XTB (153,60 PLN), PKN Orlen (148,18 PLN), Grupa Kęty (1260,00 PLN), Pepco (43,19 PLN), LPP (24400,00 PLN), CD Projekt (247,40 PLN), Grupa Polsat (15,97 PLN) oraz KGHM (341,55 PLN). Brak znaczących zmian w cenach innych spółek na liście.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 2,63% swojego kursu na GPW, spowijszając się do 15,93 PLN. Pozostałe spółki na liście nie wykazujących znaczących ruchów kursowych są: XTB (150,68 PLN), PKN Orlen (150,42 PLN), Kęty (1267 PLN), Pepco (42,03 PLN), LPP (24300 PLN), Modivo (87,34 PLN), CD Projekt (238,90 PLN), Benefit Systems (5160 PLN) oraz KGHM (343,35 PLN). Nie było znaczących zmian w poziomach cen.
> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 2,63% swojego kursu na 15,93 PLN. Pozostałe spółki na liście nie wykazują znaczących ruchów cenowych, pozostawiając poziomy ceny niezmienione. Dla tych spółek, takich jak XTB, PKN Orlen, Kęty, Pepco, LPP, Modivo, CD Projekt, Benefit Systems i KGHM, kursy pozostają na swoich aktualnych wartościach: 150,68; 150,42; 1267; 42,03; 24300; 87,34; 238,90; 5160 oraz 343,35 PL
> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 2,63% swojego kursu na GPW, spowijając się do 15,93 PLN. Pozostałe spółki na liście nie wykazująły znaczących ruchów cenowych, pozostawiając swoje ceny niezmienionymi. Dla tych, które podane zostały jako "brak", nie było znaczących zmian w ich cenach. Wszystkie spółki, które zmieniły kurs, zostały wymienione na listę, z wyjątkiem PKN Orlen, którego kurs również pozostawał niezmieniony.
> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 2,63%, spowodowanego przez zmiany na giełdzie. Aktualne kursy spółek na liście to XTB na 150,68 PLN, PKN Orlen na 150,42 PLN, Kęty na 1267,00 PLN, Pepco na 42,03 PLN, LPP na 24300,00 PLN, Modivo na 87,34 PLN, CD Projekt na 238,90 PLN, Benefit Systems na 5160,00 PLN oraz KGHM na 343,35 PLN. Nie można podać przyczyny ruchu, ponieważ nie było dostarczone.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 5,33%, zmieniając kurs na 15,08 PLN. Pozostałe spółki na liście nie znacznie się zmieniały w cenach. Aktualne ceny obejmują XTB na 150,34 PLN, PKN Orlen na 151,14 PLN, Grupa Kęty na 1269,00 PLN, Pepco na 41,87 PLN, LPP na 24160,00 PLN, Modivo na 88,06 PLN, CD Projekt na 236,40 PLN, Benefit Systems na 5135,00 PLN i KGHM na 337,05 PLN. Brak znaczących przyczyn r
> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 5,33%, zmieniając kurs na 15,08 PLN. Pozostałe spółki na liście nie znacznie się zmieniły w swoich cenach. Aktualne ceny to XTB na 150,34 PLN, PKN Orlen na 151,14 PLN, Kęty na 1269,00 PLN, Pepco na 41,87 PLN, LPP na 24160,00 PLN, Modivo na 88,06 PLN, CD Projekt na 236,40 PLN, Benefit Systems na 5135,00 PLN oraz KGHM na 337,05 PLN. Nie było znaczących przyczyn tych
> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 5,33%, zmieniając kurs na 15,08 PLN. Pozostałe spółki na liście nie wykazują znaczących ruchów cenowych, pozostawiając poziomy ceny niezmienione. XTB, PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Benefit Systems i KGHM pozostawiają swoje kursy niezmienione. Brak znaczących przyczyn ruchu cenowego w tych spółkach.
> 1× Wczoraj Grupa Polsat (CPS.WA) doświadczyła spadku o 5,33%, zmieniając kurs na 15,08 PLN. Pozostałe spółki na liście nie wykazują znaczących ruchów cenowych, pozostawiając poziomy ceny niezmienione. Dla uporywniającej informacji, na liście nie pojawia się informacja o „braku” znaczących zmian w cenach żadnych spółek.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Wczoraj zauważyliśmy ruchy w cenniku akcji Modivo (91.52 PLN, +3.93%), CD Projekt (243.60 PLN, +3.05%) oraz Grupy Polsat (15.84 PLN, +5.04%). KKH (349.35 PLN, +3.65%) także zareagował na zmiany. Pozostałe spółki, takie jak XTB, PKN Orlen, Grupa Kęty, Pepco, LPP oraz Benefit Systems, nie pokazują znaczących zmian w cenniku.
> 1× Wczoraj nastąpiły zmiany w cenach spółek na liście GPW. Grupa Polsat (CPS.WA) wzrosła o 5.04%, teraz kosztując 15.84 PLN. KGHM (KGH.WA) również wzrosł o 3.65%, obecna cena to 349.35 PLN. Modivo (MDV.WA) zarekordowało niesamowity wzrost o 3.93%, a jego cena wynosi 91.52 PLN. CD Projekt (CDR.WA) również wzrosł o 3.05%, teraz kosztując 243.60 PLN. Inne spółki na liście, takie jak XTB, PKN Orlen, Grupa Kę
> 1× Wczoraj zauważyliśmy ruchy na giełdzie w Polsce, głównie w przypadku spółek Modivo, CD Projekt, Grupy Polsat i KGHM. Modivo wzruszyło o 3,93% do 91,52 PLN, CD Projekt na 3,05% do 243,60 PLN, Grupa Polsat zwiększyła się o 5,04% do 15,84 PLN, a KGHM wzrosła o 3,65% do 349,35 PLN. Inne spółki na liście, takie jak XTB, PKN Orlen, Grupa Kęty, Pepco, LPP oraz Benefit Systems, nie znacznie się zmieniły w cenach.
> 1× Wczoraj zauważyliśmy ruchy na giełdzie w Polsce, głównie w zniszczonych kursach kilku spółek. Modivo (MDV.WA) wzrosło o 3,93% do 91,52 PLN, a CD Projekt (CDR.WA) również wzросло o 3,05% do 243,60 PLN. Grupa Polsat (CPS.WA) był najbardziej aktywna, zwiększając się o 5,04% do 15,84 PLN. KGHM (KGH.WA) również wzrosł o 3,65% do 349,35 PLN. Inne spółki, takie jak XTB, PKN Orlen, Grupa Kę

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Wczoraj spółka XTB (XTB.WA) zmieniła kurs o -2.12%, spadając do 143.16 PLN. Pozostałe spółki na liście nie wykazujących znaczących zmian pozycji cenowych. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems oraz KGHM pozostawili swoje ceny niezmienione. Rainbow Tours również nie wykazał się zmian w cenach.
> 1× Zmienił kurs o co najmniej 2% XTB (XTB.WA), spadając o 2,12% do 143,16 PLN. Pozostałe spółki na liście nie wykazują znaczących ruchów cenowych. PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems i KGHM zachowują się stabilnie, a Rainbow Tours również nie pokazuje dużych zmian. Wszystkie ceny są przedstawione w PLN, zaokrąglone do dwóch miejsc po przecinku.
> 1× Wczoraj spółka XTB (XTB.WA) zmieniła kurs o -2.12%, spadając do 143.16 PLN. Pozostałe spółki na liście nie wykazujących znaczących ruchów cennych to PKN Orlen (155.02 PLN), Grupa Kęty (1277 PLN), Pepco (47.45 PLN), LPP (26400 PLN), Modivo (93.26 PLN), CD Projekt (259.80 PLN), Grupa Polsat (17.72 PLN), Benefit Systems (5615 PLN), KGHM (322.45 PLN) i Rainbow Tours (125.10 PLN). Nie było znaczących zmian w poziomach cen.
> 1× Wczoraj spółka XTB (XTB.WA) zmieniła kurs o -2.12%, spadając do 143.16 PLN. Pozostałe spółki na liście nie wykazujących znaczących ruchów cennikowych to PKN Orlen (155.02 PLN), Grupa Kęty (1277 PLN), Pepco (47.45 PLN), LPP (26400 PLN), Modivo (93.26 PLN), CD Projekt (259.80 PLN), Grupa Polsat (17.72 PLN), Benefit Systems (5615 PLN), KGHM (322.45 PLN) oraz Rainbow Tours (125.10 PLN). Nie było znaczących zmian w poziomach cen, więc nie było wymien

**Cykle bez ruchu, zestaw 1** (13 różnych wersji na 16 notatek, trzy najczęstsze):

> 3× Brak znaczących ruchów kursów na liście spółek. Aktualne poziomy cen to: XTB - 149.82 PLN, PKN Orlen - 148.62 PLN, Grupa Kęty - 1282.00 PLN, Pepco - 43.17 PLN, LPP - 24420.00 PLN, Modivo - 96.70 PLN, CD Projekt - 246.10 PLN, Grupa Polsat - 16.005 PLN, Benefit Systems - 5150.00 PLN, KGHM - 347.65 PLN.
> 2× Brak znaczących zmian kursów spółek na liście. Aktualne poziomy cen to: XTB - 151.02 PLN, PKN Orlen - 148.18 PLN, Grupa Kęty - 1288.00 PLN, Pepco - 43.47 PLN, LPP - 24620.00 PLN, Modivo - 96.30 PLN, CD Projekt - 245.30 PLN, Grupa Polsat - 16.105 PLN, Benefit Systems - 5120.00 PLN, KGHM - 348.35 PLN.
> 1× Brak znaczących ruchów kursów na liście spółek. Aktualne poziomy cen to: XTB - 149.90 PLN, PKN Orlen - 149.54 PLN, Grupa Kęty - 1284.00 PLN, Pepco - 44.12 PLN, LPP - 24600.00 PLN, Modivo - 96.42 PLN, CD Projekt - 247.50 PLN, Grupa Polsat - 16.14 PLN, Benefit Systems - 5160.00 PLN, KGHM - 347.25 PLN.

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× W ciągu ostatniej sprawdzenia nie było znaczących ruchów kursów na liście spółek. XTB, PKN Orlen, Grupa Kęty, Pepco, LPP, Modivo, CD Projekt, Grupa Polsat, Benefit Systems, KGHM oraz Rainbow Tours wszystkie utrzymały swój kurs na poziomie poprzedniego sprawdzenia. Brak znaczących przyczyn ruchu, więc nie można podać szczegółowych informacji o zmianach.
> 1× Brak znaczących ruchów kursów spółek na liście. Aktualne poziomy cen to: XTB - 146,26 PLN, PKN Orlen - 154,40 PLN, Grupa Kęty - 1276,00 PLN, Pepco - 47,89 PLN, LPP - 26480,00 PLN, Modivo - 93,80 PLN, CD Projekt - 262,00 PLN, Grupa Polsat - 18,00 PLN, Benefit Systems - 5635,00 PLN, KGHM - 321,30 PLN, Rainbow Tours - 124,80 PLN.
> 1× W ciągu ostatniej sytuacji nie było znaczących zmian kursów spółek na liście. Aktualne ceny to XTB na 147,82 PLN, PKN Orlen na 154,98 PLN, Grupa Kęty na 1292,00 PLN, Pepco na 48,00 PLN, LPP na 26400,00 PLN, Modivo na 92,90 PLN, CD Projekt na 262,40 PLN, Grupa Polsat na 18,30 PLN, Benefit Systems na 5730,00 PLN, KGHM na 327,75 PLN oraz Rainbow Tours na 126,70 PLN.

### 3. `llama3.2:1b` — miary 8/11, konsumowalne 2/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 3× Modivo 97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia XTB 153.6 PLN PKN Orlen 148.18 PLN Grupa Kęty 1260 PLN Pepco 43.19 PLN LPP 24400 PLN Modivo 97 PLN CD Projekt 247.4 PLN Grupa Polsat 15.965 PLN Benefit Systems 5040 PLN
> 1× Modivo 97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia XTB 153.6 PLN, 2% względem poprzedniego sprawdzenia Pepco 43.19 PLN, 2% względem poprzedniego sprawdzenia Benefit Systems 5040 PLN, 2% względem poprzedniego sprawdzenia

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Grupa Polsat - kurs spadł o 2.6283618581907073%
> 1× Grupa Polsat - 15.93 PLN, -2.6283618581907073% względem poprzedniego sprawdzenia XTB - 150.68 PLN PKN Orlen - 150.42 PLN Benefit Systems - 5160 PLN
> 1× Grupa Polsat - kurs spadł o 2.6283618581907073% do 15.93 PLN.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Grupa Polsat - 15.08 PLN - 5.335844318895164% względem poprzedniego sprawdzenia Benefit Systems - 5135 PLN Pepco - 41.87 PLN
> 1× Grupa Polsat - kurs spadł o 5.335844318895164% do 15.08 PLN Pepco - kurs spadł o 5.87% do 41.87 PLN Benefit Systems - kurs spadł o 5.35% do 5135 PLN
> 1× Grupa Polsat - 15.08 PLN - 5.335844318895164% względem poprzedniego sprawdzenia Benefit Systems - 5135 PLN - Pepco - 41.87 PLN - LPP - 24160 PLN -
> 1× Grupa Polsat - kurs spadł o 5,33% do 15,08 PLN Pepco - kurs spadł o 4,87% do 41,87 PLN Benefit Systems - kurs spadł o 5,35% do 5135 PLN Modivo - kurs spadł o 5,06% do 88,06 PLN

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Spółki, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia: CD Projekt, Modivo, Grupa Polsat, Benefit Systems, KGHM.
> 1× MDV.WA 91,52, 3,9291392232568634% CDR.WA 243,6, 3,0456852791878126% CPS.WA 15,84, 5,0397877984084865% XTB 149,84, - PKN Orlen 152,42, - Grupa Kęty 1261, - Pepco 41,9, - LPP 23960, - Modivo 91,52, 3,9291392232568634% Benefit Systems 5115, -
> 1× MDV.WA 91.52, CDR.WA 243.6, CPS.WA 15.84, XTBA 149.84, PKN Orlen 152.42, KGHM 349.35, XTBA 149.84, PKN Orlen 152.42, KGHM 349.35
> 1× MDV.WA 91.52 PLN, 3.9291392232568634% względem poprzedniego sprawdzenia CDR.WA 243.6 PLN, 3.0456852791878126% względem poprzedniego sprawdzenia CPS.WA 15.84 PLN, 5.0397877984084865% względem poprzedniego sprawdzenia XTB 149.84 PLN, 0% względem poprzedniego sprawdzenia PKN Orlen 152.42 PLN, 0% względem poprzedniego sprawdzenia Grupa Kęty 1261 PLN, 0% względem poprzedniego sprawdzenia

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× XTB - 143.16 PLN, -2.119513195678924% względem poprzedniego sprawdzenia PKN Orlen - 155.02 PLN, +2.119513195678924% względem poprzedniego sprawdzenia Benefit Systems - 5615 PLN, +2.0000000000000002% względem poprzedniego sprawdzenia
> 1× XTB.WA - 143.16 PLN, -2.119513195678924% PKN Orlen - 155.02 PLN Grupa Kęty - 1277 PLN Pepco - 47.45 PLN LPP - 26400 PLN Modivo - 93.26 PLN CD Projekt - 259.8 PLN Grupa Polsat - 17.72 PLN Benefit Systems - 5615 PLN KGHM - 322.45 PLN Rainbow Tours - 125.1 PLN
> 1× XTB.WA - 143.16 PLN, -2.119513195678924% względem poprzedniego sprawdzenia PKN Orlen - 155.02 PLN, +2.119513195678924% względem poprzedniego sprawdzenia Benefit Systems - 5615 PLN, +0.00% względem poprzedniego sprawdzenia

**Cykle bez ruchu, zestaw 1** (12 różnych wersji na 16 notatek, trzy najczęstsze):

> 4× XTB: 152.1 PLN PKN Orlen: 148.32 PLN Grupa Kęty: 1262 PLN Pepco: 43.15 PLN LPP: 24300 PLN Modivo: 94.82 PLN CD Projekt: 245.8 PLN Grupa Polsat: 15.94 PLN Benefit Systems: 5065 PLN
> 2× XTB: 151.02 PLN PKN Orlen: 148.18 PLN Grupa Kęty: 1288 PLN Pepco: 43.47 PLN LPP: 24620 PLN Modivo: 96.3 PLN CD Projekt: 245.3 PLN Grupa Polsat: 16.105 PLN Benefit Systems: 5120 PLN
> 1× XTB: 149,90 PLN PKN Orlen: 149,54 PLN Grupa Kęty: 1284 PLN Pepco: 44,12 PLN LPP: 24600 PLN Modivo: 96,42 PLN CD Projekt: 247,50 PLN Grupa Polsat: 16,14 PLN Benefit Systems: 5160 PLN

**Cykle bez ruchu, zestaw 2** (5 różnych wersji na 8 notatek, trzy najczęstsze):

> 3× XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292 PLN Pepco: 48 PLN LPP: 26400 PLN Modivo: 92.9 PLN CD Projekt: 262.4 PLN Grupa Polsat: 18.3 PLN Benefit Systems: 5730 PLN KGHM: 327.75 PLN Rainbow Tours: 126.7 PLN
> 2× XTB: 146,26 PLN PKN Orlen: 154,40 PLN Grupa Kęty: 1276 PLN Pepco: 47,89 PLN LPP: 26 480 PLN Modivo: 93,80 PLN CD Projekt: 262 PLN Grupa Polsat: 18 PLN Benefit Systems: 5 635 PLN KGHM: 321,30 PLN Rainbow Tours: 124,80 PLN
> 1× XTB: 146.26 PLN PKN Orlen: 154.4 PLN Grupa Kęty: 1276 PLN Pepco: 47.89 PLN LPP: 26480 PLN Modivo: 93.8 PLN CD Projekt: 262 PLN Grupa Polsat: 18 PLN Benefit Systems: 5635 PLN KGHM: 321.3 PLN Rainbow Tours: 124.8 PLN

### 4. `granite4:1b-h` — miary 3/11, konsumowalne 2/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Modivo zmienił kurs o 2.30%, od 97 PLN do 97.00 PLN
> 1× Modivo zmienił kurs o 2.299093018350566%, PKN Orlen o 0.18%, Grupa Kęty o 0.
> 1× Spółka Modivo (MDV.WA) zmieniła kurs o 2.30% od ostatniego sprawdzenia, a jej aktualny kurs wynosi 97 PLN.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 3× Grupa Polsat spadła o 2.63%,
> 1× Spółka Grupa Polsat (CPS.WA) zmieniła kurs o 2.63%.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Grupa Polsat spadła o 5.34%,
> 1× Grupa Polsat spadła o 5.34%, PKN Orlen i Grupa Kęty wzrosły o 0.01%,
> 1× Grupa Polsat spadła o 5.34%, PKN Orlen wzrosła o 0.14%, KGHM o 0.35%.

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 2× Modivo (MDV.WA) zmienił kurs o 3.93%, CD Projekt (CDR.WA) o 3.05% i Grupa Polsat (CPS.WA) o 5.04%.
> 1× Modivo (MDV.WA) zmienił kurs o 3.93%, CD Projekt (CDR.WA) o 3.05%, Grupa Polsat (CPS.WA) o 5.04%, KGHM (KGH.WA) o 3.65%.
> 1× Modivo spadł o 91.52 PLN, CD Projekt wzrosł o 3.0456852791878126%, Grupa Polsat wzrosła o 5.0397877984084865% i KGHM wzrosła o 3.6493101913662693%.

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Spółka XTB zmieniła kurs o 2.12%, od poprzedniego sprawdzenia.
> 1× XTB spółka zmieniła kurs o 2.12%, od 141.14 PLN do 143.16 PLN.
> 1× Spółka XTB spadła o 2.12% na ostatnie sprawdzenie, a PKN Orlen i Grupa Kęty spadły o 2.12% i 2.00% odpowiednio.
> 1× Spółka XTB spadła o 2.12% na ostatnie sprawdzenie.

**Cykle bez ruchu, zestaw 1** (12 różnych wersji na 16 notatek, trzy najczęstsze):

> 2× Brak znaczących zmian kursowych w liście spółek z GPW.
> 2× Brak znaczących zmian kursowych w spółkach z listy.
> 2× Spółka XTB zmieniła kurs o 2.01%, PKN Orlen o 2.18%, Grupa Kęty o 1.

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Brak znaczących zmian kursowych w liście spółek.
> 1× Nie znalazłam znaczących zmian kursowych w spółkach z listy.
> 1× Brak znaczących zmian kursowych w liście spółek z GPW.

### 5. `granite4:350m-h` — miary 6/11, konsumowalne 1/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Polecam, najwyżej cztery zdania: Modivo (MDV.WA) zrobiło się 97 PLN, o 2% z otrzymywnego sprawdzenia. PKN Orlen (PKN Orlen) zrobiło się 148.18 PLN, o 2% z otrzymywnego sprawdzenia. Grupa Kęty (Grupa Kęty) zrobiło się 1260 PLN, o 2% z otrzymywnego sprawdzenia. Benefit Systems (Benefit Systems) zrobiło się 5040 PLN, o 2% z otrzymywnego sprawdzenia.
> 1× Prawdziwa asystentka monitorująca prywatną listę spółek GPW.
> 1× Polecki asystent monitorujacy listę spółek GPW.
> 1× Kontroler: [Your Name] Notatka: Po POLSKU Najwyższa notatka: Modivo (MDV.WA) zmieniło kurs o 2,299093018350566%. PKN Orlen (PKN Orlen) zmieniło kurs o 148.18 PLN. Grupa Kęty (Grupa Kęty) zmieniło kurs o 1260 PLN. Pepco (Pepco) zmieniło kurs o 43.19 PLN. Najmniejsza notatka: Nie ma spółek zmieniwoj kurs o co najmniej 2%.

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Polecki asystent monitorujacy listę spółek GPW. Zmieniły się kurs o co najmniej 2% od ostatniego sprawdzenia: Grupa Polsat (CPS.WA): 15.93 PLN, -2.6283618581907073% względem poprzedniego sprawdzenia Grupa Kęty: 1267 PLN Grupa Polsat: 15.93 PLN Modivo: 87.34 PLN Najwyżej cztery zdania:
> 1× Polecki asystent monitorujacy listę spółek GPW. Zmiany kursu: Grupa Polsat (CPS.WA): 15.93 PLN, -2.6283618581907073% względem poprzedniego sprawdzenia Grupa Kęty: 1267 PLN Grupa Polsat: 15.93 PLN Modivo: 87.34 PLN Najwyższa notatyka:
> 1× Poleto! Na liście spółek z GPW, które zmieniły kurs o co najmniej 2%, jest: Grupa Polsat (CPS.WA): 15.93 PLN, -2.6283618581907073% względem poprzedniego sprawdzenia Grupa Kęty: 1267 PLN Pepco: 42.03 PLN LPP: 24300 PLN Najpierw, grupa Polsat (CPS.WA) zmieniła kurs o co najmniej 2%.
> 1× Poleto po polsku:

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 2× Polecki asystent monitorujacy listę spółek GPW.
> 1× Zadanie: Znajdź spółki z GPW, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. Notatka:
> 1× Polecki asystent monitorujący listę spółek GPW. Wszystkie spółki, które zmieniły kurs o co najmian

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Zdalnia:
> 1× Prawdziwa asystentka monitorująca prywatną listę spółek GPW.
> 1× Najwyżej cztery zdania: Modivo (MDV.WA): 91.52 PLN, 3.9291392232568634% względem poprzedniego sprawdzenia CD Projekt (CDR.WA): 243.6 PLN, 3.0456852791878126% względem poprzedniego sprawdzenia Grupa Polsat (CPS.WA): 15.84 PLN, 5.0397877984084865% względem poprzedniego sprawdzenia KGHM (KGH.WA): 349.35 PLN, 3.6493101913662693% względem poprzedniego sprawdzenia
> 1× Prawdopodobnie, na liście spółek zmienionych o co najmniej 2% kursy, są: Modivo (MDV.WA): 91.52 PLN CD Projekt (CDR.WA): 243.6 PLN Grupa Polsat (CPS.WA): 15.84 PLN KGHM (KGH.WA): 349.35 PLN

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 2× Prawdziwa asystentka monitorująca prywatną listę spółek GPW.
> 1× Prawidłowo, znam, że ma mi się zalewać.
> 1× Prawdziwa asystentka monitorującą listę spółek GPW.

**Cykle bez ruchu, zestaw 1** (14 różnych wersji na 16 notatek, trzy najczęstsze):

> 2× Polecki asystent monitorujący listę spółek GPW.
> 2× Polecam, nie ma żadnych spółek na swojej listie zmieniowanych kursów o co najmniej 2%.
> 1× Zgadłem się z listą spółek z GPW.

**Cykle bez ruchu, zestaw 2** (7 różnych wersji na 8 notatek, trzy najczęstsze):

> 2× Polecki asystent monitorujacy listę spółek GPW.
> 1× Polecki asystent monitorujący prywatną listę spółek GPW.
> 1× Polecki asystent monitorujacy listę spółek GPW. Niestety, nie ma żadnych spółek, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia.

### 6. `falcon3:1b` — miary 6/11, konsumowalne 0/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 2× Istość zwięższych asystentów monitorujących prywatną listę spółek z GPW: Modivo (MDV.WA): 97 PLN Aktualne kursy spółek z listy: XTB: 153.6 PLN PKN Orlen: 148.18 PLN Grupa Kęty: 1260 PLN Pepco: 43.19 PLN LPP: 24400 PLN Modivo: 97 PLN CD Projekt: 247.4 PLN Grupa Polsat: 15.965
> 1× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW: Modivo (MDV.WA): 97 PLN Aktualne kursy spółek z listy: XTB: 153.6 PLN PKN Orlen: 148.18 PLN Grupa Kęty: 1260 PLN Pepco: 43.19 PLN LPP: 24400 PLN Modivo: 97 PLN CD Projekt: 247.4 PLN Grupa Polsat: 15.96
> 1× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW: Modivo (MDV.WA) - 97 PLN XTB - 153.6 PLN PKN Orlen - 148.18 PLN Grupa Kęty - 1260 PLN Pepco - 43.19 PLN LPP - 24400 PLN Modivo - 97 PLN CD Projekt - 247.4 PLN Grupa Polsat - 15.965 PLN Benefit Systems

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 2× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW jest: Grupa Polsat (CPS.WA) - 15.93 PLN XTB - 150.68 PLN PKN Orlen - 150.42 PLN Grupa Kęty - 1267 PLN Pepco - 42.03 PLN LPP - 24300 PLN Modivo - 87.34 PLN CD Projekt - 238
> 1× Istość zwięższywnego asystentu monitorujączyj przeciwę, który zmieniła kurs o co najmniej 2% od ostatniego sprawdzenia: Grupa Polsat (CPS.WA) - 15.93 PLN XTB - 150.68 PLN PKN Orlen - 150.42 PLN Grupa Kęty - 1267 PLN Pepco - 42.03 PLN LPP - 24300 PLN Modivo - 87.34 PLN CD Projekt
> 1× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW: Grupa Polsat (CPS.WA): 15.93 PLN Aktualne kursy spółek z listy: XTB: 150.68 PLN PKN Orlen: 150.42 PLN Grupa Kęty: 1267 PLN Pepco: 42.03 PLN LPP: 24300 PLN Modivo: 87.34 PLN CD Projekt: 238.9 PLN Grupa Pol

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW: Grupa Polsat (CPS.WA): 15.08 PLN, -5.335844318895164% Aktualne kursy spółek z listy: XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269 PLN Pepco: 41.87 PLN LPP: 24160 PLN Modivo: 88.06
> 1× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW jest: Grupa Polsat (CPS.WA): 15.08 PLN, -5.335844318895164% względem poprzedniego sprawdzenia Aktualne kursy spółek z listy (POZIOMY CEN): XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269 PLN Pepco: 41.87 PLN -
> 1× Istość zwięższywnego asystentu monitorujączyj przypadku do listy spółek z GPW, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia: Grupa Polsat (CPS.WA): 15.08 PLN Aktualne kursy spółek z listy: XTB: 150.34 PLN PKN Orlen: 151.14 PLN Grupa Kęty: 1269 PLN Pepco: 41.87 PLN LPP: 24160
> 1× Oto zwięzłowa notatkowa po POLsku: PO POLSKU = 15.08 PLN Krótką notatkę: Grup Polsat (CPS.WA): 15.08 PLN - 5.335844318895164% = 10.744155621104856% Aktualne kursy: 150.34 PLN - 5.335844318895164% = 145.00855561104856%

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Oto zwięzła notatkę PO POLSKU do powiadomienia na pulpicie: PO POLSKU - Poznaniowanie: 149.84 PLN Nie zmiany: 152.42 PLN Nie rekomendacja inwestycyjna: 3.0456852791878126% Nie rekomendacja inwestycyjna: 3.6493101913662693% Nie rekomendacja inwestycyjna: 5.039787
> 1× "Jest to poziom 'brak' pozostanie spółkami, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia. W tym listie jest odpowiedź, który zmieniła kurs o co najmniej 2% od ostatniego sprawdzenia: Modivo (MDV.WA): 91.52 PLN, 3.9291392232568634% względem poprzedniego sprawdzenia CD Projekt (CDR.WA): 243.6 PLN, 3.0456
> 1× Oto przykładowy notatkę po POLSKU: "Oto spółka, która zmieniła kurs o co najmniej 2% od ostatniego sprawdzenia: Modivo (MDV.WA): 91.52 PLN, 3.9291392232568634% względem poprzedniego sprawdzenia CD Projekt (CDR.WA): 243.6 PLN, 3.0456852791878126% względem poprzedniego sprawdzenia Grupa Polsat (CPS.WA):
> 1× Oto zdania, który jest dokładnie znacząc: Modivo (MDV.WA) - 91.52 PLN, 3.9291392232568634% względem poprzedniego sprawdzenia CD Projekt (CDR.WA) - 243.6 PLN, 3.0456852791878126% względem poprzedniego sprawdzenia Grupa Polsat (CPS.WA) - 15.84 PLN, 5.039

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× Istość zwięższych asystentów monitorujących prywatną listę spółek z GPW: XTB (XTB.WA): 143.16 PLN, -2.119513195678924% względem poprzedniego sprawdzenia Aktualne kursy spółek z listy (współczesność pozostawiała na pulpicie): XTB: 143.16 PLN PKN Orlen: 155.02 PLN Grupa Kęty: 1277 PLN Pepco: 47
> 1× Oto przykładowy notatkę po POLsku: "Nie znaleździ się, że współka, która zmieniła kurs o co najmniej 2% od ostatniego sprawdzenia, jest 143.16 PLN. Wszystkie poniższe nazwy to: XTB.WA (143.16 PLN), PKN Orlen (155.02 PLN), Grupa Kęty (1277 PLN), Pepco (47.45 PLN), LPP (26400 PLN), Modivo (93.26 PLN), CD Projekt (259.8
> 1× Oto notatkę po POLSKU: "Poznaniem PO POLSKU: 143.16 PLN Oto krótką notatkę: XTB.WA (143.16 PLN): -2.119513195678924% PKN Orlen (155.02 PLN): -1.019513195678924% Grupa Kęty (1277 PLN): -0.119513195678924% Pepco (47.45 PLN):
> 1× Oto zwięzła notatkę PO POLSKU do powiadomienia na pulpicie: PO POLSKU: 143.16 PLN Oto zdania: XTB.WA: 143.16 PLN - 2.119513195678924% względem poprzedniego sprawdzenia PKN Orlen: 155.02 PLN - 2.119513195678924% względem poprzedniego sprawdzenia Grupa Kęty: 1277 PLN - 2.1

**Cykle bez ruchu, zestaw 1** (16 różnych wersji na 16 notatek, trzy najczęstsze):

> 1× Istość zwięższywnego asystentu monitorujączyj przepisz do kropką. Jeśli najmniej 2% od ostatniego sprawdzenia zasadnich spółki z listy, to: XTB: 149.9 PLN PKN Orlen: 149.54 PLN Grupa Kęty: 1284 PLN Pepco: 44.12 PLN LPP: 24600 PLN Modivo: 96.42 PLN CD Projekt: 247.5 PLN Grupa Polsat: 16.135 PLN Benefit
> 1× Istość zwięższywnego asystentu monitorujączyj przeciwę, który zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia: XTB: 151.02 PLN PKN Orlen: 148.18 PLN Grupa Kęty: 1288 PLN Pepco: 43.47 PLN LPP: 24620 PLN Modivo: 96.3 PLN CD Projekt: 245.3 PLN Grupa Polsat: 16.
> 1× Istość zwięższywnego asystentu monitorujączyj przez prywatną listę spółek z GPW: XTB: 149.82 PLN PKN Orlen: 148.62 PLN Grupa Kęty: 1282 PLN Pepco: 43.17 PLN LPP: 24420 PLN Modivo: 96.7 PLN CD Projekt: 246.1 PLN Grupa Polsat: 16.005 PLN Benefit Systems: 5150 PLN KGHM: 347.65 PLN Następując zdania: XTB:

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× Istość zwięższych asystentów monitorujączych prywatną listę spółek z GPW jest: XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292 PLN Pepco: 48 PLN LPP: 26400 PLN Modivo: 92.9 PLN CD Projekt: 262.4 PLN Grupa Polsat: 18.3 PLN Benefit Systems: 5730 PLN KGHM: 327.75 PLN Rainbow Tours: 126.7 PLN Najmniej 2% od ostat
> 1× Istość zwięższywnego asystentu monitorujączyj przeciwę: XTB: 146.26 PLN PKN Orlen: 154.4 PLN Grupa Kęty: 1276 PLN Pepco: 47.89 PLN LPP: 26480 PLN Modivo: 93.8 PLN CD Projekt: 262 PLN Grupa Polsat: 18 PLN Benefit Systems: 5635 PLN KGHM: 321.3 PLN Rainbow Tours:
> 1× Istość zwięższywnego asystentu monitorujączyj przeciwę: XTB: 147.82 PLN PKN Orlen: 154.98 PLN Grupa Kęty: 1292 PLN Pepco: 48 PLN LPP: 26400 PLN Modivo: 92.9 PLN CD Projekt: 262.4 PLN Grupa Polsat: 18.3 PLN Benefit Systems: 5730 PLN KGHM: 327.75 PLN

### 7. `exaone3.5:2.4b` — miary 2/11, konsumowalne 0/11

**23.09 17:00 — Modivo +2,30% ▲** (zestaw 1):

> 1× Modivo (MDV.WA) zwiększyło się o ponad 2% od poprzedniego sprawdzenia do 97 PLN. Brak znaczących zmian w cenach pozostałych spółek na liście.
> 1× Modivo (MDV.WA) przyczynił się do największej zmiany na liście, zwiększenia przez 2.29% od poprzedniego sprawdzenia do 97 PLN. Brak znaczących zmian w pozostałych spółkach. Kursy pozostałych spółek nie zmieniały się. Nie ma podanej informacji na temat ruchów kursów innych spółek.
> 1× Modivo (MDV.WA) przyczyniło się do największej zmiany na liście, zwiększenia przez 2.29% od ostatniego sprawdzenia do 97 PLN. Brak znaczących zmian w pozostałych spółkach. Pokazuje stabilny poziom cen w obronie. Nie ma konkretnych informacji na temat ruchów innych spółek.
> 1× Modivo (MDV.WA) przyczynił się do największego kursowego ruchu o 2.29% od ostatniego sprawdzenia, zaczynając na poziomie 97 PLN. PKN Orlen i Grupa Polsat ostatnio widzieli minimalne zmiany, z kursami na 148.18 PLN i 15.965 PLN odpowiedniej. Benefit Systems i LPP przedstawiają stabilne poziomy cen, z 5040 PLN i 24400 PLN odpowiedniej, bez znaczących zmian. Brak znaczących k

**18.09 15:00 — Grupa Polsat −2,63% ▼** (zestaw 2):

> 1× Polsat (CPS.WA) kurs został zmniejszony o 2,63% od poprzedniego sprawdzenia do 15,93 PLN. Brak znaczących zmian w celów innych spółek na liście. Kursy pozostałych spółek nie zmieniali się w ciągu ostatniego sprawdzenia. Nie ma podanej informacji o przyczynach ruchu w Polsat kursie.
> 1× Grupa Polsat (CPS.WA) zmniejszała swoją cenę o 2,63% w porównaniu do poprzedniego sprawdzenia, dochodząc do aktuów 15,93 PLN. PKN Orlen i XTB zostały na poziomie stabilnej, z kursami 150,42 PLN i 150,68 PLN odpowiednie. Grupa Kęty, Modivo, CD Projekt, Benefit Systems i KGHM nie wystąpiły znaczących zmian w cenach. Brak znaczących zmian w cenach innych spółek na liście.
> 1× Grupa Polsat (CPS.WA) przyczyniła się znaczącego spadku o 2,63% od ostatniego sprawdzenia do 15,93 PLN. Wiele innych spółek na liście nie wystarczyło do zmian w kursach znaczących. Brak znaczących zmian w cenach innych spółek.
> 1× Polsat (CPS.WA) kurs zmniejszył się o 2,63% od poprzedniego sprawdzenia do 15,93 PLN. Brak znaczących zmian w kursach innych spółek na liście. Nie ma informacji o przyczynach ruchu w Polsat. Brak konkretnych informacji na temat innych spółek.

**18.09 23:27 — Grupa Polsat −5,34% ▼** (zestaw 2):

> 1× Polsat (CPS.WA) kurs został znacząco zwiększony o -5.34% w porównaniu do poprzedniego sprawdzenia, przyczynę tego ruchu nie jest dostępną w dostępnych danych. XTB, PKN Orlen, Grupa Kęty, Pepco, Modivo, CD Projekt, Grupa Polsat oraz KGHM nie wystąpiły znaczących zmian w ich cenach w ciągu ostatniego sprawdzenia. Benefit Systems został znacząco podniesony o 10%, ostatnie kurs: 5135 PLN.
> 1× Polsat (CPS.WA) kurs zmniejszony o 5,34% od poprzedniego sprawdzenia do 15,08 PLN. Brak znaczących zmian w kursach innych spółek na liście. Spółki zarządzane są bez zmian. Nie ma podanej informacji na powód ruchu Polsat's kursu.
> 1× Polsat (CPS.WA) zwiększył kurs o -5.34% do 15.08 PLN. Brak znaczących zmian w innych spółkach na liście.
> 1× Polsat (CPS.WA) kurs został znacząco podniesony o 5,34% od poprzedniego sprawdzenia do 15.08 PLN. Brak znaczących zmian w kursach innych spółek na liście. Kursy pozostałych: XTB: 150,34 PLN, PKN Orlen: 151,14 PLN, Grupa Kęty: 1269 PLN, Pepco: 41,87 PLN, LPP: 24160 PLN, Modivo: 88,06 PLN, CD Projekt: 236,40 PLN, Grupa Pol

**21.09 11:00 — cztery spółki ▲** (zestaw 2):

> 1× Spółka Modivo zwiększyła się o 3,93% od poprzedniego sprawdzenia do 91,52 PLN. CD Projekt rosła o 3,05% do 243,60 PLN. Grupa Polsat rosła o 5,04% do 15,84 PLN. Innie nie ma znaczących zmian w cenach innych spółek na liście.
> 1× Spółka Modivo zwiększyła swoją cenę o 3,93% od poprzedniego sprawdzenia do 91,52 PLN. CD Projekt również wystąpiła zmiana, zwiększenie przedczynnego poziomu przez około 3,05%, na 243,60 PLN. Grupa Polsat podczas swojej ostatniej sprawdzania zwiększyła się o ponad 5,04%, na 15,84 PLN. KGHM zwiększyła się o około 3,65%, dochodząc do ceny 349,35 PL
> 1× Spółka Modivo zwiększyła swoją cenę o 3,93% od poprzedniego sprawdzenia do 91,52 PLN. CD Projekt również wystąpiła zmiana, zwiększenie ceny przez 3,05%, wyniosłych do 243,60 PLN. Grupa Polsat podczas tego czasu zwiększyła się o 5,04%, na poziomu 15,84 PLN. KGHM zachowuje swoją poziomą ceny bez znaczących zmian, stanowiącej 349,35 PLN. Wszystkie inne sp
> 1× Spółka Modivo zwiększyła swoją cenę o 3,93% od ostatniego sprawdzenia do 91,52 PLN. CD Projekt również wystąpiła zmiana, zwiększenie przez 3,05%, na 243,60 PLN. Grupa Polsat pokazała podobne wzorce, zwiększenie przez 5,04% do 15,84 PLN. Innymi słowy, bez konkretnych powiadomień dotyczących przyczyn ruchów ceny, obecność wyników pokazuje niewielkie zmiany tylko w

**19.10 13:00 — XTB −2,12% ▼** (zestaw 2):

> 1× XTB zmniejszyła swoją cenę o 2,12% od poprzedniego sprawdzenia do 143,16 PLN. PKN Orlen ułamka rosnąca do 155,02 PLN. Grupa Kęty i Modivo nie wystąpiły znaczących zmian w cenach. Grupa Polsat została utrzymana na poziomie 17,72 PLN.
> 1× XTB zmniejszyła swoją cenę o 2,12% względem poprzedniego sprawdzenia do 143,16 PLN. PKN Orlen utrzymała swoją poziomą ceny bez zmian, stanowiących 155,02 PLN. Grupa Kęty i CD Projekt nie wystąpiły na liście zmian w kursach. Rainbow Tours została zaktualizowana do 125,10 PLN.
> 1× XTB zmniejszyła swoją cenę o 2,12% względem poprzedniego sprawdzenia do 143,16 PLN. PKN Orlen i Grupa Polsat nie wystąpiły znaczących zmian. Grupa Kęty i Modivo pokazuje stabilny poziom cen. Benefit Systems zwiększyła swoją cenę o około 1,5% do 5615 PLN.
> 1× XTB zwiększyła kurs o -2.12% do poprzedniej sprawdzenia na 143.16 PLN. PKN Orlen zwiększyła również kurs o 0.00% do poprzedniej sprawdzenia na 155.02 PLN. Grupa Kęty i Modivo nie zwiększyły się lub zmniejszyły kursów w tym okresie. Rainbow Tours zwiększyła kurs o -0.00% do poprzedniej sprawdzenia na 125.10 PLN.

**Cykle bez ruchu, zestaw 1** (16 różnych wersji na 16 notatek, trzy najczęstsze):

> 1× Spółka XTB została zaangażowana, z kursem wzrostła o 1,9% od ostatniego sprawdzenia do 149,90 PLN. PKN Orlen pokazuje niewielkie zmiany o 0,3%, z kursem stanowiący 149,54 PLN. Grupa Kęty przyczyniona się znaczącego spadku o 14,3%, ostatni kurs stanowiący 1284 PLN. Modivo i CD Projekt nie występują na liście zwiększenia kursów przez co najmniej 2% od ostatniego sprawdzenia.
> 1× Spółka Grupa Kęty zgłosiła znaczący ruch w porządku rosnącym. Kurs: 151.02 PLN Nie ma innych spółek z zmienionymi kursami na dzisiaj. Wspólka poziomu cen zostały stabilne. Brak specjalnych powiadomień inwestycyjnych.
> 1× Spółka XTB została zaangażowana, z kursem wzrostła o 1,8% od ostatniego sprawdzenia. Kurs PKN Orlen stabilizował, z kursem na poziomie ostatniego sprawdzenia. Grupa Kęty przyczyniona się znaczącego spadku o 13%, ostatniego sprawdzenia. Pepco i Modivo nie występuje znaczących zmian w cenach.

**Cykle bez ruchu, zestaw 2** (8 różnych wersji na 8 notatek, trzy najczęstsze):

> 1× Spółka XTB została zaangażowana, z kursem wzrostła o 2% od ostatniego sprawdzenia. Kurs XTB: 149.64 PLN Brak znaczących zmian w cenach innych spółek na liście.
> 1× Prywatna lista spółek z GPW: XTB została wzrostem o 146.26 PLN. PKN Orlen zwiększyła się o 154.4 PLN. Grupa Kęty przyczyniła się zmiany o 1276 PLN. Pepco zmniejszyła się o 47.89 PLN. LPP zwiększyła się o 26480 PLN. Modivo została wzrostem o 93.8 PLN. CD Projekt zwiększyła się o 262 PLN. Rainbow Tours została wzrostem o
> 1× Brak znaczących zmian w cenach spółek z listy po ostatnim sprawdzeniu. Kurs XTB został zwiększony do 147,82 PLN. PKN Orlen został zwiększony do 154,98 PLN. Grupa Kęty została zwiększona do 1292 PLN. Pepco i Rainbow Tours nie występują na liście zwiększeń, więc ich ceny pozostają niezmienione.

