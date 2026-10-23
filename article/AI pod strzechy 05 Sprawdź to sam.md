# Część 5. Sprawdź to sam

Wszystko poniżej można skopiować i wkleić u siebie. Pobierz dowolny model z tej stawki, wyślij mu prompt z jednego z jedenastu scenariuszy i porównaj jego notatkę z wynikiem wzorcowym. Pełne dane wszystkich scenariuszy są w [rozdziale 3a](./AI%20pod%20strzechy%2003a%20Szczegolowe%20dane%20testowe.md).

## 5.1. Zapytanie

Punkt końcowy: **`POST /api/generate`** — ten sam, którego używa moja aplikacja. Ustawienia:

```json
{
  "model": "granite4.1:8b",
  "prompt": "…treść promptu z 5.5…",
  "stream": false,
  "think": false,
  "options": {
    "temperature": 0.2,
    "num_predict": 180,
    "num_gpu": 0
  }
}
```

- **`think: false`** wyłącza rozumowanie. Bez tej flagi część modeli zaczyna od angielskiego toku myślenia i notatka przestaje się mieścić w stu osiemdziesięciu tokenach.
- **`num_predict: 180`** to limit odpowiedzi, ponad dwukrotny zapas na cztery polskie zdania.
- **`num_gpu: 0`** liczy wszystko na procesorze, tak jak w moim badaniu. Jeśli masz kartę graficzną i chcesz po prostu zobaczyć wynik, usuń tę linijkę — notatka będzie ta sama, tylko szybciej.
- Okna kontekstu nie ustawiam — zostaje domyślne dla Ollamy.

**Jeden wyjątek: `gpt-oss:20b`.** Nie pozwala wyłączyć rozumowania — `think: false` jest dla niego nieznaną wartością i Ollama podstawia domyślny poziom „medium”, przy którym model zużywa cały budżet na rozważania. Wyślij mu `"think": "low"`, a resztę bez zmian.

Do pomiarów powtarzalnych dodaj w `options` `"seed": 42`.

## 5.2. Zimny i ciepły start

Mój pomiar zaczyna każdy przebieg od pustej pamięci, więc pierwsza notatka jest zimna — z ładowaniem wag — a kolejne ciepłe. Żeby odtworzyć zimny start, przed zapytaniem zwolnij model:

```json
POST /api/generate
{ "model": "granite4.1:8b", "keep_alive": 0 }
```

Przydatne adresy: `/api/tags` zwraca listę modeli, które masz lokalnie, a `/api/ps` to, co aktualnie siedzi w pamięci. To drugie przydaje się najbardziej — jeśli lista jest pusta, twój następny pomiar będzie zimny. Ollama domyślnie trzyma model w pamięci pięć minut po ostatnim zapytaniu; aplikacja z cyklem co dwie godziny w praktyce zawsze startuje na zimno.

## 5.3. Jak sprawdzić, czy model honoruje `think: false`

Jedno tanie pytanie wystarczy. Wyślij z flagą `think: false` cokolwiek trywialnego i zobacz, gdzie wyląduje rozumowanie, np. (zakładam, że Ollama działa pod `http://127.0.0.1:11434`). Najwygodniej zapisać to do pliku `test.ps1` i uruchomić jako `.\test.ps1` — wklejany wiersz po wierszu blok łatwo przerwać w połowie. PowerShell 7 (`pwsh`) ma też domyślnie kodowanie UTF-8, co oszczędza kłopotów opisanych w 5.4.

```powershell
$body = @{
  model   = 'twoj-model'
  prompt  = 'Odpowiedz jednym slowem: ile to jest dwa plus dwa?'
  stream  = $false
  think   = $false
  options = @{ num_predict = 60 }
} | ConvertTo-Json -Depth 5
$o = Invoke-RestMethod -Uri http://127.0.0.1:11434/api/generate -Method Post `
       -Body ([System.Text.Encoding]::UTF8.GetBytes($body)) `
       -ContentType 'application/json; charset=utf-8'

"response: " + $o.response
"thinking: " + $o.thinking
```

- **`response` zawiera krótką odpowiedź, `thinking` jest puste** → flaga honorowana, użyj ustawień z 5.1.
- **`response` jest puste albo urwane, a `thinking` pełne angielskich rozważań** → model nie zna wartości `false` i rozumuje dalej, tylko Ollama odkłada to do osobnego pola. Budżet tokenów idzie na rozważania i na notatkę nie zostaje nic. Tak zachowuje się `gpt-oss:20b` — wyślij mu `"think": "low"` i powtórz test:

  ```powershell
  $body = @{ model='gpt-oss:20b'; prompt='Odpowiedz jednym slowem: ile to jest dwa plus dwa?'
             stream=$false; think='low'; options=@{ num_predict = 180 } } | ConvertTo-Json -Depth 5
  ```

  Na najniższym poziomie rozważania są krótkie i w `response` pojawia się odpowiedź.
- **`response` zawiera angielski tok myślenia** („Okay, the user asks…”) → flaga ignorowana, a rozumowanie wycieka do treści. Takie modele — wyłącznie rozumujące — do mojego badania nie weszły (kryterium 2.1.5): przy limicie 180 tokenów nie zdążą napisać notatki, a po zdjęciu flagi i podniesieniu budżetu do kilku tysięcy tokenów piszą ją minutami.

Lista dozwolonych wartości jest też w manifeście: `POST /api/show` z nazwą modelu zwraca pole `thinking.values`. Jeśli jest w nim `false`, rozumowanie da się wyłączyć; jeśli tylko poziomy („low”, „medium”…), trzeba wybrać najniższy, jak przy `gpt-oss:20b`.

## 5.4. Jak to wysłać, żeby nie zepsuć polskich znaków

Na Windowsie to jest pułapka, w którą wpadłem sam i która wygląda jak wada modelu. Prompt zawiera „Kęty”, „ó”, „ł”. Jeśli dojdą połamane, **model zacznie zmyślać liczby**: ten sam `granite4:7b-a1b-h` wysłany przez PowerShell bez jawnego kodowania zmyślał procenty w sześciu przebiegach na sześć, a wysłany poprawnie — w żadnym.

Trzeba wysyłać **bajty**, nie łańcuch znaków:

```powershell
$prompt = Get-Content .\prompt.txt -Raw -Encoding UTF8
$body   = @{ model='granite4.1:8b'; prompt=$prompt; stream=$false; think=$false
             options=@{ temperature=0.2; num_predict=180; num_gpu=0 } } | ConvertTo-Json -Depth 5

Invoke-RestMethod -Uri http://127.0.0.1:11434/api/generate -Method Post `
  -Body ([System.Text.Encoding]::UTF8.GetBytes($body)) `
  -ContentType 'application/json; charset=utf-8'
```

Kontrola przed pomiarem: `$prompt.Contains("Kęty")` musi zwrócić `True`.

## 5.5. Prompt

Wszystkie jedenaście scenariuszy dostaje ten sam tekst; zmieniają się tylko dane. Tak wygląda prompt scenariusza z ruchem z 23 września, godz. 17:00:

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

W cyklach bez ruchu zamiast wiersza ze spółką jest słowo **„brak”**, a lista kursów zawiera kursy z danego cyklu — przykład jest w rozdziale 3.1.2. W scenariuszu z czterema spółkami naraz (21 września, 11:00) pod nagłówkiem stoją cztery wiersze. Liczby są przepisane z Yahoo bez obróbki: kropka dziesiętna, procent w pełnej precyzji, bez plusa przy wzroście.

**Test A/B** to ten sam prompt bez zdania „Liczby zaokrąglij do dwóch miejsc po przecinku i zapisz z przecinkiem dziesiętnym.” **Wersja angielska** ma tę samą budowę, z „none” zamiast „brak” i poleceniem napisania notatki IN ENGLISH.

## 5.6. Czego szukać w odpowiedzi

**Wynik wzorcowy dla scenariusza z 17:00** zawiera nazwę **Modivo**, procent **2,30%** — zaokrąglony w górę z 2,2991 — i kurs **97 PLN** albo **97,00 PLN**, nie zawiera żadnej innej liczby procentowej i kończy się kropką. Na przykład tak pisze `gemma4:12b`, w czterech przebiegach na cztery:

> Spółka Modivo zmieniła kurs o 2,30% względem poprzedniego sprawdzenia. Aktualny kurs Modivo wynosi 97,00 PLN. Nie znam przyczyny tego ruchu.

**W cyklu bez ruchu** notatka ma wprost powiedzieć, że znaczących zmian nie było, i nie wymieniać kursów:

> Nie było znaczących zmian w kursach spółek na liście.

Notatkę oceniaj tak jak ja w rozdziale 3.5.1: kropka dziesiętna zamiast przecinka, procent bez zaokrąglenia czy dopisane kursy innych spółek to usterki formy — notatkę da się jeszcze użyć. Zła liczba, zły kierunek albo brak informacji to już notatka, której użyć nie można.

Najczęstsze sposoby, na jakie modele to psują, w kolejności od najczęstszych w moim badaniu:

1. **gubią spółkę albo procent przy kilku ruchach naraz** — w scenariuszu z czterema spółkami podają same kursy albo pomijają KGHM;
2. **zmyślają ruchy spółek, które się nie ruszyły** — także w cyklach bez ruchu, z procentami i kwotami, których nie było na wejściu;
3. **obcinają zamiast zaokrąglać** — „2,29%” zamiast 2,30%, „5,33%” zamiast 5,34%;
4. **skracają procent do jednego miejsca** — „2,3%”, „−5,3%”;
5. **psują zapis kursu** — „16.105.00 PLN” przy kursie 16,105 zł, „LPP 244,00 PLN” zamiast 24 400;
6. **wypisują kursy wszystkich spółek** wbrew zakazowi albo **przepisują do notatki fragment polecenia**;
7. **mylą kierunek** — „wzrosło” przy spadku, zwłaszcza gdy minus przy procencie trzeba przenieść na słowo.

## 5.7. Wolumen

Czterdzieści cztery modele, jedenaście scenariuszy po polsku, każdy w czterech przebiegach — **1936 notatek** w podstawowym teście. Do tego testy A/B i angielskie na pięciu scenariuszach z 23 września oraz zestaw drugi po angielsku dla grupy pierwszej, razem około **4700 notatek**, i test narzędzi: dziewięć prób na model. Na zwykłym procesorze, z pustą pamięcią przed każdym przebiegiem, cała grupa druga — siedemnaście modeli — mieści się w kilku godzinach; pojedynczy model z grupy czwartej to od kilkunastu do kilkudziesięciu minut na jeden zestaw.
