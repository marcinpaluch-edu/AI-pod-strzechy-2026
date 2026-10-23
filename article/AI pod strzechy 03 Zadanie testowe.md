# Część 3. Zadanie testowe

Czy na komputerze, który stoi w domu albo w małej firmie, da się uruchomić model językowy robiący coś użytecznego — i czy da się z niego zbudować agenta, który działa sam? Czy **poprawnie skonfigurowany model odpowie poprawnie, dostatecznie szybko i na sprzęcie, który ktoś faktycznie może kupić w przeciętnym sklepie z komputerami?**


## 3.1. Aplikacja

Aplikacją testową jest aplikacja, która monitoruje dziesięć spółek z warszawskiej giełdy. Co dwie godziny pobiera kursy, o 9:00, 11:00, 13:00, 15:00 i 17:00, czyli pięć razy dziennie w dni sesyjne i każdy z nich porównuje z poprzednim odczytem.
Jeśli któraś spółka ruszyła się o 2% lub więcej, model pisze dwa do czterech zdań na pulpit. 
Po polsku, bez rekomendacji inwestycyjnych, bez wymyślania liczb.

To zadanie celowo mało generatywne. Nie chcę oceniać stylu ani myślenia ani tym bardziej pomysłowości modelu, ale sprawdzić, czy model potrafi przepisać kilka liczb i napisać o nich zdanie po polsku. W skromnych warunkach domowych a i przecież w wielu firmach, konkretne zadania, które poddaje się automatyzacji, muszą również być weryfikowalne. Zadanie brzmi więc banalnie, ale zaletą takiego podejścia jest właśnie prosta weryfikowalność. Każda liczba w odpowiedzi albo występuje w danych wejściowych, albo nie. Nie trzeba oceniać „jakości" — wystarczy porównać z gotowym wcześniej wynikiem.

### 3.1.1. Dane wejściowe 

Na wejściu znalazły się prawdziwe dane notowań 10-11 spółkach w jedenastu scenariuszach. Jedenaście scenariuszy można podzielić na dwa zestawy. Pierwszy zestaw to pięć scenariuszy z dnia 23 września 2026 r. ze wszystkich pięciu cykli badania co dwie godziny, od 09:00 do 17:00. Drugi to zbiór różnych cykli z różnych dni, ale reprezentujących odrębne, interesujące przypadki do zbadania możliwości modelu. 

| Scenariusz | Zestaw | Start | Kierunek |
|---|---|---|:---:|
| 23.09 09:00 — bez zmiany o co najmniej 2% | 1 | zimny | — |
| 23.09 11:00 — bez zmiany o co najmniej 2% | 1 | ciepły | — |
| 23.09 13:00 — bez zmiany o co najmniej 2% | 1 | ciepły | — |
| 23.09 15:00 — bez zmiany o co najmniej 2% | 1 | ciepły | — |
| 23.09 17:00 — zmiana dodatnia, Modivo +2,30% | 1 | ciepły | ▲ |
| 18.09 15:00 — zmiana ujemna, Grupa Polsat −2,63% | 2 | zimny | ▼ |
| 18.09 23:27 — zmiana ujemna większa, Grupa Polsat −5,34% | 2 | ciepły | ▼ |
| 21.09 11:00 — wiele zmian naraz: Modivo +3,93%, CD Projekt +3,05%, Grupa Polsat +5,04%, KGHM +3,65% | 2 | ciepły | ▲ |
| 19.10 09:00 — bez zmiany o co najmniej 2%, w inny dzień oraz odniesienie do piątkowego zamknięcia (wszystkie 0,00%) | 2 | ciepły | — |
| 19.10 11:00 — bez zmiany o co najmniej 2%, największa to KGHM −1,97%, tuż pod progiem | 2 | ciepły | — |
| 19.10 13:00 — XTB −2,12%, tuż nad progiem | 2 | ciepły | ▼ |

Kolumna Start: każdy przebieg zaczyna się od pustej pamięci, więc pierwszy scenariusz każdego zestawu model dostaje „na zimno”, z ładowaniem wag, a pozostałe „na ciepło”.

Pełne dane każdego cyklu i scenariusza można znaleźć [tutaj](./AI%20pod%20strzechy%2003a%20Szczegolowe%20dane%20testowe.md).

Wszystkie jedenaście cykli to **jedenaście promptów wysyłanych do modeli** z mojego badania, różniących się tylko kursami i listą spółek, które się ruszyły (movers).


### 3.1.2. Surowe dane wejściowe i prompt

Dane zwracane przez API serwisu Yahoo podane są oryginalnie z użyciem kropki dla oddzielenia części dziesiętnej, bez separatora tysięcy, bez stałej liczby miejsc po przecinku i z zachowaniem takiej ich ilości, jaka wynika z systemu notowań dla danej spółki np. Polsat jest notowany z krokiem pół grosza, więc jego kurs bywa podany z trzema miejscami po przecinku, np. 15.965 PLN. Końcowych zer Yahoo nie przesyła, np. Polsat 15.94, nie 15.940. Nazwy spółek Yahoo nie podaje — aplikacja dobiera je do symboli z własnej listy monitorowanych spółek. 

Przykładowa wiadomość dotycząca kursów spółek z listy, użyta do zbudowania promptu w aplikacji dla ostatniego cyklu z godz. 17:00 w dniu 23.09, wygląda tak:
```json
[
  { "symbol": "XTB.WA", "regularMarketPrice": 153.6 },
  { "symbol": "PKN.WA", "regularMarketPrice": 148.18 },
  { "symbol": "KTY.WA", "regularMarketPrice": 1260 },
  { "symbol": "PCO.WA", "regularMarketPrice": 43.19 },
  { "symbol": "LPP.WA", "regularMarketPrice": 24400 },
  { "symbol": "MDV.WA", "regularMarketPrice": 97 },
  { "symbol": "CDR.WA", "regularMarketPrice": 247.4 },
  { "symbol": "CPS.WA", "regularMarketPrice": 15.965 },
  { "symbol": "BFT.WA", "regularMarketPrice": 5040 },
  { "symbol": "KGH.WA", "regularMarketPrice": 341.55 }
]
```

Aplikacja liczy zmianę procentową względem poprzedniego odczytu: od kursu bieżącego odejmuje kurs poprzedni, różnicę dzieli przez kurs poprzedni i mnoży przez 100. Wynik jest dodatni przy wzroście i ujemny przy spadku.

```javascript
    changePercent = ((q.close - prev.close) / prev.close) * 100;
```

Spółka, której zmiana co do wartości bezwzględnej wynosi co najmniej 2%, trafia do promptu jako ruch.

Wobec powyższego prompt, którego użyłem dla wszystkich modeli w ostatnim cyklu z godz. 17:00, to:

```
Jesteś zwięzłym asystentem monitorującym prywatną listę spółek z GPW. To informacja, nie rekomendacja inwestycyjna.

Spółki, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia:
Modivo (MDV.WA): 97 PLN, 2.299093018350566% względem poprzedniego sprawdzenia

Aktualne kursy spółek z listy (to POZIOMY CEN, nie zmiany; wszystkie poniższe nazwy to spółki, żadna nie jest giełdą ani miejscem notowań):
XTB: 153.6 PLN
PKN Orlen: 148.18 PLN
Grupa Kęty: 1260 PLN
Pepco: 43.19 PLN
LPP: 24400 PLN
Modivo: 97 PLN
CD Projekt: 247.4 PLN
Grupa Polsat: 15.965 PLN
Benefit Systems: 5040 PLN
KGHM: 341.55 PLN

Napisz krótką notatkę PO POLSKU do powiadomienia na pulpicie, najwyżej cztery zdania. Jeśli na liście spółek, które zmieniły kurs o co najmniej 2%, jest choć jedna spółka, napisz, co się ruszyło i o ile, i podaj kursy tylko tych spółek; jeśli nie znasz przyczyny ruchu, napisz to wprost zamiast zgadywać. Jeśli zamiast spółek jest tam słowo „brak”, napisz wprost, że nie było znaczących zmian, nie wymieniaj kursów i pod żadnym pozorem nie opisuj poziomów cen jako wzrostów ani spadków. Nazwy spółek przepisz dokładnie tak, jak podano wyżej. Liczby zaokrąglij do dwóch miejsc po przecinku i zapisz z przecinkiem dziesiętnym. Bez powitania, bez nagłówków, bez wypunktowań, bez formatowania i bez formułek o konsultacji z doradcą. Nie podawaj rekomendacji kupna ani sprzedaży. Nie dopisuj zdań wypełniających i zakończ ostatnie zdanie kropką.
```

Prompt dla pozostałych cykli ma dokładnie ten sam tekst. Zmieniają się tylko dane. W przypadku cykli, w których nie ma żadnej zmiany powyżej progu 2%, zamiast wiersza ze spółką, która się ruszyła, jest słowo „brak”, a lista kursów zawiera kursy z danego cyklu. Dla przykładu prompt dla pierwszego cyklu, o 9:00:

```
Jesteś zwięzłym asystentem monitorującym prywatną listę spółek z GPW. To informacja, nie rekomendacja inwestycyjna.

Spółki, które zmieniły kurs o co najmniej 2% od ostatniego sprawdzenia:
brak

Aktualne kursy spółek z listy (to POZIOMY CEN, nie zmiany; wszystkie poniższe nazwy to spółki, żadna nie jest giełdą ani miejscem notowań):
XTB: 149.9 PLN
PKN Orlen: 149.54 PLN
Grupa Kęty: 1284 PLN
Pepco: 44.12 PLN
LPP: 24600 PLN
Modivo: 96.42 PLN
CD Projekt: 247.5 PLN
Grupa Polsat: 16.135 PLN
Benefit Systems: 5160 PLN
KGHM: 347.25 PLN

Napisz krótką notatkę PO POLSKU do powiadomienia na pulpicie, najwyżej cztery zdania. Jeśli na liście spółek, które zmieniły kurs o co najmniej 2%, jest choć jedna spółka, napisz, co się ruszyło i o ile, i podaj kursy tylko tych spółek; jeśli nie znasz przyczyny ruchu, napisz to wprost zamiast zgadywać. Jeśli zamiast spółek jest tam słowo „brak”, napisz wprost, że nie było znaczących zmian, nie wymieniaj kursów i pod żadnym pozorem nie opisuj poziomów cen jako wzrostów ani spadków. Nazwy spółek przepisz dokładnie tak, jak podano wyżej. Liczby zaokrąglij do dwóch miejsc po przecinku i zapisz z przecinkiem dziesiętnym. Bez powitania, bez nagłówków, bez wypunktowań, bez formatowania i bez formułek o konsultacji z doradcą. Nie podawaj rekomendacji kupna ani sprzedaży. Nie dopisuj zdań wypełniających i zakończ ostatnie zdanie kropką.
```

Zadaniem modelu jest więc nie tylko rozpoznanie, czy w danym cyklu był ruch, i wybranie z promptu tego, co ma trafić do notatki, ale też samodzielna obróbka liczb. Na wejściu dostaje kropkę dziesiętną, a ma napisać przecinek. Procent dostaje z kilkunastoma miejscami po przecinku, a ma go zaokrąglić do dwóch. Kierunek ruchu musi odczytać z samego znaku liczby, bo plusa w prompcie nie ma. Całość ma napisać poprawną polszczyzną.


## 3.2. Stanowiska

Badanie prowadzę na jednej maszynie, ale w dwóch osobnych środowiskach.

**Stanowisko kontenerowe** — cztery kontenery Podmana we wspólnej sieci:

| Kontener | Rola |
|---|---|
| aplikacja Node.js | harmonogram, pobieranie kursów, prompt, panel |
| PostgreSQL | historia notowań |
| MongoDB | zapisane notatki i raporty |
| Ollama | modele językowe, **wyłącznie procesor** |

Kontenery są uruchomione wewnątrz maszyny wirtualnej WSL2, której domyślny limit pamięci to połowa RAM hosta i w ten sposób odtwarza warunki maszyny z 16 GB (moja maszyna ma 32 GB RAM).

**Stanowisko hostowe** — druga instancja Ollamy, uruchomiona natywnie w Windows. Służy wyłącznie do dotestowania modeli, które w kontenerze się nie mieszczą, ale uruchomią się na ew. nieco lepszym komputerze domowym z 32 GB RAM. Nie testuję dodatkowo modeli z kontenera drugi raz na hoście w ogóle.

Na obu stanowiskach pomiar prowadzi ten sam skrypt pomiarowy: w kontenerze uruchamiany jest jednorazowo z obrazu aplikacji, w jej sieci i na jej wersji Node.js, a na hoście na Node.js w tej samej wersji, z tymi samymi funkcjami aplikacji do budowania promptu, wywołania modelu i oczyszczania odpowiedzi.

## 3.3. Protokół pomiaru

**Pięć rzeczywistych cykli z bazy**, nie prompty syntetyczne. To prawdziwe zmiany cen akcji na prawdziwej giełdzie decydują o tym, co model musi umieć.

**Ten sam prompt dla wszystkich**, budowany przez tę samą funkcję, której używa aplikacja.

**Prompt jest w języku polskim.** Przed pomiarem sprawdzam, że dociera do modelu w kodowaniu UTF-8, z polskimi znakami w nienaruszonej postaci.

**Jeden model naraz, z potwierdzonym zwolnieniem pamięci.** Przed każdym modelem wyładowuję wszystko, co rezyduje w pamięci — wysyłając do każdego załadowanego modelu zapytanie z `keep_alive: 0`, czyli poleceniem natychmiastowego zwolnienia — i odpytuję `/api/ps` w pętli aż do pustej listy, dokładając kilka sekund zapasu. Każdy model zaczyna więc pomiar na maszynie w takim stanie, jakby był na niej od początku sam. Przed każdym modelem skrypt sprawdza też, czy wolnej pamięci wystarczy na jego wagi z zapasem — odczytaną z tej maszyny, na której model się ładuje, a nie z tej, na której stoi skrypt. Jeśli nie, model jest odkładany, a nie mierzony w warunkach, w których musiałby korzystać z pliku wymiany.
Same wywołania idą natomiast z `keep_alive: '10m'`, więc w obrębie przebiegu model zostaje w pamięci między cyklami: pierwsza notatka przebiegu powstaje na zimno, kolejne cztery na ciepło.

**Badam jednocześnie powtarzalność**, czyli uruchamiam każdy model 4 razy, aby sprawdzić, czy są stabilne i czy jeśli nie pojawią się w odpowiedzi żadne wady, to czy nie pojawią się one być może przy kolejnym przebiegu. Każdy przebieg to pełne pięć cykli jeszcze raz, z potwierdzonym zwolnieniem pamięci po każdym przebiegu, a więc z zimnym startem dla cyklu pierwszego i ciepłym dla kolejnych czterech w każdym przebiegu.

**Ollama jest do wyłącznej dyspozycji testu**, zatem nie uruchamiam w tym czasie żadnych innych zapytań — ani do okna czatu, ani do API, ani na hoście, ani w kontenerze. Dotyczy to także **samej badanej aplikacji**, która sięga po model o stałych godzinach i weszłaby w pomiar bez ostrzeżenia. Komputer pracuje bez innych zasobożernych programów.

**Uruchamia się jeden proces pomiarowy naraz**, pilnowany blokadą. Zwolnienie pamięci przed pomiarem chroni przed modelami walczącymi o RAM, ale nie chroni przed drugą kopią **tego samego skryptu**, uruchomioną przypadkiem — na przykład po awarii pierwszego procesu i przedwczesnym starcie drugiego. Jeśli plik blokady istnieje i wskazany w nim proces żyje, drugie uruchomienie odmawia startu zamiast dołożyć się do bałaganu. Blokada zdejmuje się sama przy zakończeniu, także przerwanym.

**Dostarczenie prawidłowej notatki do 5 minut od startu "na zimno".** Każdy na nowo załadowany model przechodzi najpierw przez czas ładowania wag po czym chodzi już dalej załadowany w pełni. Jednocześnie moje zadanie jest dość proste i chciałbym zobaczyć jego wynik jak najszybciej po wysłaniu zapytania. Toleruję jednak wynik maksymalnie 5 minut od wysłania zapytania. Po tym czasie zapytanie jest przerywane, a notatka liczy się jako pusta. Ciekawostka dla powtarzającego badanie: **wbudowany w Node `fetch` ma również własny limit 300 sekund** na oczekiwanie na nagłówki odpowiedzi i stosuje go **niezależnie od jakiegokolwiek limitu ustawionego w kodzie**. Przy `stream: false` Ollama milczy, dopóki nie skończy generować, więc każde dłuższe wywołanie ginie jako `fetch failed` i zapisuje się jako pusta odpowiedź modelu. Mój limit jest z nim zbieżny, więc w tej aplikacji to bez znaczenia — ale kto zechce dać modelowi więcej niż pięć minut, musi zmienić sposób transportu, bo samo podniesienie limitu w kodzie nic nie da.

Z drugiej strony **tak właśnie działa badana aplikacja**: odpytanie o kursy odbywa się raz na dwie godziny, a model i tak domyślnie opuszcza pamięć po pięciu minutach. Każdy produkcyjny cykl jest więc zimny i dlatego podaję obie liczby.

**Wszystko liczone wyłącznie na procesorze.** `num_gpu: 0` idzie w każdym zapytaniu na **obu stanowiskach**. W kontenerze jest to bez znaczenia, bo karty i tak nie widać, ale dzięki temu zapytanie ma tam identyczny kształt co na hoście i nie ma miejsca na różnicę, której nie zauważę. Badanie ma odzwierciedlić warunki przeciętnego laptopa bez mocnej karty graficznej.


### 3.3.1. Ustawienia względem Modelfile

**Wywołuję Ollamę przez `/api/generate`.** Jest to punkt końcowy przeznaczony w dokumentacji do generowania odpowiedzi na zadany prompt, a takie właśnie jest moje zadanie. **Wywołania narzędzi muszą za to iść przez `/api/chat`** — `/api/generate` przyjmuje parametr `tools`, zwraca `HTTP 200` i po cichu go ignoruje, bez błędu i bez ostrzeżenia.

**Oczekuję, że każdy model dostaje swój własny szablon czatu.** W związku z tym nie ustawiam parametru `raw`, więc obowiązuje wartość domyślna `false`, a `/api/generate` nakłada szablon zapisany w Modelfile modelu. Tak zachowuje się Ollama u każdego, kto pobierze model i wyśle zapytanie.

**Okno kontekstowe wynosi 4096 tokenów.** Jest to ustawienie domyślne w Ollamie — niezależne od tego, ile deklaruje sam model, a deklaracje sięgają ćwierć miliona. Mój prompt zajmuje **od 454 do około 1100 tokenów zależnie od modelu**: to ten sam tekst, ale każdy tokenizator liczy go inaczej, a szablon czatu dokłada swoje. Na generowanie zostaje więc od 3642 do około 3000 tokenów. 

**Limit 180 tokenów na odpowiedź** — notatka ma mieć najwyżej cztery zdania długości, a cztery polskie zdania to około osiemdziesięciu tokenów. Limit daje więc **ponad dwukrotny zapas na odpowiedź**. 

W aplikacji wymuszam **jedną i tę samą temperaturę 0,2**, ponieważ inaczej mierzenie modeli byłoby niesprawiedliwe. Wszystkie modele są dostarczane z inną temperaturą, od 0,1 to 1,0. Większość nie ma jej zdefiniowanej w pliku Modelfile wcale i otrzymują wówczas przypisaną przez Ollamę domyślną wartość temperatury **0,8**. Sama wartość 0,2 została dobrana, ponieważ zadanie postawione modelom nie jest literacko-konwersacyjne a odpowiedź ma być rzeczowa. 

Postanowiłem przy tym nie wybierać temperatury 0. Przy zerze dekodowanie jest zachłanne: model zawsze bierze szczyt rozkładu. Jeśli na tym szczycie znajduje się halucynacja, dostaje się ją z pewnością, a nie z prawdopodobieństwem. Niezerowa temperatura zostawia modelowi szansę ominięcia takiego szczytu.

**Ziarno jest losowe** czyli takie, jakie domyślnie otrzymałby użytkownik niczego nie ustawiając. Zmienna OLLAMA_SEED nie jest ustawiona nigdzie: ani w kodzie, ani w konfiguracji wdrożeniowej. Klucz seed nie trafia więc do zapytania w ogóle. Ziarna nie definiuje również żaden z plików Modelfile. 
Każde wywołanie startuje więc od innego losowania — co nie znaczy, że odpowiedź będzie inna. Przy zadaniu tak zamkniętym jak to rozkład bywa na tyle ostry, że te same słowa wygrywają niezależnie od ziarna: część modeli zwróciła w trzech powtórzeniach tekst identyczny co do bajtu. Losowe ziarno daje modelowi szansę na inną odpowiedź, ale nie gwarancję.

**Każdy cykl to pojedyncze zapytanie, bez historii rozmowy.** Model nie dostaje ani poprzedniego pytania, ani własnej poprzedniej odpowiedzi. Nie jest to obojętne i **działa na niekorzyść wyników**. Model, który widzi własną poprzednią odpowiedź, nie rozwiązuje zadania od nowa — tylko sprawdza, co się zmieniło. Rezygnuję z tego świadomie, bo **przy monitorowaniu historia jest ryzykiem, nie oszczędnością**. Cykl o trzynastej ma inne dane niż ten o jedenastej, a model, który pamięta „LPP wzrosło o 6,79%", może tę notatkę odtworzyć zamiast przeczytać nowe liczby. Notatka o nieaktualnym kursie jest gorsza niż jej brak.

Ma to jedną konsekwencję praktyczną, o której warto wiedzieć, zanim zacznie się sprawdzać moje liczby w oknie czatu Ollamy: **to, co widać w czacie, nie jest porównywalne z tym, co zwraca API.** W czacie każde kolejne pytanie niesie ze sobą całą poprzednią rozmowę, więc drugi przebieg tego samego promptu jest szybszy i często daje lepszą odpowiedź — nie dlatego, że model „się rozkręcił", tylko dlatego, że ma gotowy wynik przed oczami. 

**Z samplerów wymuszam tylko temperaturę a pozostałe zostają takie, jakie przyszły z modelem**, bo aplikacja ich nie dotyka.
`top_k`, `top_p`, `min_p` i `repeat_penalty` nie trafiają do zapytania w ogóle,
więc obowiązuje to, co producent wpisał w Modelfile, a gdy nie wpisał nic —
wartości Ollamy. Uwaga! **Szesnaście modeli
na czterdzieści cztery przychodzi z własnymi ustawieniami losowania**, dlatego
sprawdziłem osobnym testem A/B, czy to ma znaczenie — obie konfiguracje
naprzemiennie w jednej sesji, przy tym samym ziarnie. **Jakość nie zmieniła się
ani razu.**

## 3.4. Co sprawdza automat

| Miara | Co znaczy | Błąd liczbowy |
|---|---|:---:|
| **movery** | czy procent każdej poruszonej spółki pojawił się w odpowiedzi | tak |
| **kierunek** | czy model opisał wzrost jako wzrost, a spadek jako spadek | tak |
| **zmyślone** | liczby — procenty **i kwoty w złotych** — których nie było na wejściu | tak |
| **puste** | czy cykl w ogóle zwrócił odpowiedź | tak |
| **nazwy** | czy nazwa spółki została przepisana poprawnie | — |
| **urwane** | czy ostatnie zdanie kończy się kropką | — |
| **czas, długość** | sekundy na cykl, znaki odpowiedzi | — |

Dwie z tych miar powstały w trakcie badania, bo czytanie tekstów pokazało błędy, których wcześniejsze miary nie łapały. **Zmyślone** liczyło początkowo tylko procenty i pokazywało zero przy modelu, który zmyślał kwoty w złotych. **Kierunek** nie istniał w ogóle, dopóki nie trafiła się notatka z poprawnym procentem opisanym jako spadek zamiast wzrostu — miara „movery" zaliczyła ją, bo szukała liczby, a nie zdania. Obie przeliczyłem wstecz z zapisanych odpowiedzi, więc obejmują cały materiał, nie tylko pomiary późniejsze.

## 3.5. Co sprawdzić musi człowiek

**Automat sprawdza liczby. Polszczyznę trzeba przeczytać.** Metryka zwraca liczbę tak samo pewnie, kiedy działa, jak wtedy, kiedy się myli. Dlatego przy każdym modelu w tym badaniu są zarówno liczby, jak i pełne teksty odpowiedzi.

### 3.5.1. Konsumowalność

Czytając notatkę, trzeba też ocenić, czy mimo ewentualnych błędów człowiek zrozumie jej przekaz. Mamy do czynienia z modelami generatywnej sztucznej inteligencji, przy których trudno o wynik idealny, a część wykrytych błędów może okazać się akceptowalna. Dlatego obok miar automatu każdą notatkę oceniam jeszcze jednym pytaniem: **czy dane są prawdziwe i kompletne, nawet jeśli źle sformatowane?** Notatkę, która przechodzi tę próbę, nazywam konsumowalną. Oceniam tutaj formę, nie treść.

| Konsumowalna — lżejsza przewina | Niekonsumowalna |
|---|---|
| kropka zamiast przecinka dziesiętnego | jedno miejsce po przecinku albo liczba całkowita zamiast dwóch miejsc |
| brak zaokrąglenia, czyli procent w pełnej precyzji | obcięcie zamiast zaokrąglenia (2,29% zamiast 2,30%) |
| separator tysięcy („1.288,00 PLN”, „24 400 PLN”) | zmyślony procent, kwota albo zmiana („XTB: +1.82”) |
| dopisana waluta | zły kierunek albo zły znak |
| kursy pozostałych spółek wbrew zakazowi, zdania wypełniające | pominięty ruch, ruch bez procentu, przekręcona nazwa spółki z ruchem |
| | ruch przypisany spółce, która się nie ruszyła |
| | przepisane polecenie zamiast notatki |
| | pusta notatka; w cyklu bez ruchu brak informacji, że zmian nie było |
| | notatka urwana na limicie długości — nie wiadomo, co zostało ucięte |

Granica przebiega tam, gdzie czytelnik notatki zostałby wprowadzony w błąd albo musiałby sięgnąć do danych źródłowych. Kropkę dziesiętną każdy odczyta poprawnie; „2,3%” zamiast 2,30% gubi już precyzję, którą dane miały, a „Grupa Polsat 16.105.00 PLN” przy kursie 16,105 zł czytelnik odczyta jako szesnaście tysięcy. Scenariusz jest dla modelu konsumowalny, jeśli konsumowalne są wszystkie cztery notatki z czterech przebiegów.

## 3.6. Cztery grupy wielkości

Przypominam, modele dzielę na grupy wg rozmiaru:

**Grupa 1 — poniżej 2 GB** (7 modeli). Zmieszczą się wszędzie, także na sprzęcie słabszym niż zakładany. Badam je dla porządku i po to, żeby pokazać, gdzie leży dolna granica sensu.

**Grupa 2 — od 2 do 6 GB** (17 modeli). Serce badania. Mieszczą się na maszynie z 16 GB bez karty graficznej, a przy karcie z 8 GB pamięci mieszczą się w niej w całości.

**Grupa 3 — od 6 do 14 GB** (9 modeli). Poza zasięgiem karty z 8 GB RAM, możliwe do uruchomienia w kontenerze wyłącznie przy konfiguracji 32 GB RAM z WSL-em na 16.

**Grupa 4 — powyżej 14 GB** (11 modeli). Domknięcie luki: sprawdzam, co traci ten, kto zatrzymuje się na tańszym sprzęcie.

W sumie **44 modele od dwunastu wydawców**. 

Granice podaję w gigabajtach.
Uwaga, `gpt-oss:20b` waży 13,79 GB, więc nominalnie należy do grupy trzeciej, lecz z półtoragigabajtowym marginesem potrzebuje 15,3 GB i pod sufitem szesnastu się nie mieści. Mierzony był wyłącznie na hoście i dlatego liczę go do grupy czwartej.

## 3.7. Kryteria oceny

W każdej grupie wyłaniam zwycięzcę według trzech ocen, w tej kolejności, a czas odpowiedzi rozstrzyga remisy:

**Poprawność liczbowa we wszystkich jedenastu scenariuszach i czterech przebiegach.** Model, który gubi ruchy albo wymyśla procenty, odpada niezależnie od reszty. Poprawność liczbową liczy automat (rozdział 3.4) na wszystkich notatkach. Jedno zmyślenie na dwadzieścia notatek oznacza, że przy prawdziwym uruchomieniu, pięć razy dziennie w dni sesyjne, wyjdzie mniej więcej raz w tygodniu.

**Poprawna polszczyzna.** Oceniana z pełnych tekstów, nie z metryk. Notatka trafia na pulpit użytkownika, więc gramatyka jest moim kryterium. Błąd, który wraca w co najmniej dwóch przebiegach, jest właściwością modelu. Błąd z jednego przebiegu odnotowuję w komentarzu i rozstrzyga on dopiero przy równej ocenie.

**Konsumowalność notatek.** Czy notatka niesie prawdziwe i kompletne dane, nawet jeśli ma usterki formy — kropkę dziesiętną, separator tysięcy, procent bez zaokrąglenia. Konsumowalność przepuszcza takie usterki, ale jest ostrzejsza od automatu tam, gdzie gubi się treść: procent skrócony do jednego miejsca, zła kwota czy notatka, która w cyklu bez ruchu nie mówi, że zmian nie było, nie są konsumowalne (rozdział 3.5.1).

**Czas odpowiedzi.** Przy równych ocenach decyduje zimny start, bo tak w praktyce pracuje aplikacja — każdy cykl co dwie godziny zaczyna się od załadowania modelu.


### 3.7.1. Kwalifikacja do pracy jako agent

**Kwalifikacja do pracy jako agent** nie wchodzi do tego rankingu. Sprawdzam ją rzeczywistym wywołaniem narzędzia, nie deklaracją z karty modelu, ale wątek narzędziowy i agentowy opisuję osobno — jest na tyle obszerny, że nie mieści się jako kolumna w tabeli.


Zapraszam do lektury wyników [tutaj.](./AI%20pod%20strzechy%2004%20Wyniki.md)