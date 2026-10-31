# Część 6. Podsumowanie

We wstępie postawiłem kilka pytań: czy przeciętny komputer wystarczy do uruchomienia aplikacji, która lokalnie używa sztucznej inteligencji, jakie są jej możliwości, czy warto dołożyć do mocniejszego sprzętu, czy takie rozwiązanie ma sens i czy można liczyć na odpowiedzi po polsku. Po czterdziestu czterech modelach, jedenastu scenariuszach i blisko dwóch tysiącach notatek odpowiedzi są następujące.

## Czy przeciętny komputer wystarczy?

**Tak — ale na granicy.** Na komputerze z 16 GB RAM bez karty graficznej mieszczą się modele grupy drugiej i jeden z nich, `granite4.1:8b`, przeszedł wszystkie jedenaście scenariuszy z notatkami, które niosą prawdziwe dane. Jest jedyny taki w swojej grupie, a jego notatki mają usterki formy: kropkę dziesiętną zamiast przecinka i kursy wszystkich spółek wypisywane wbrew zakazowi postawionemu w prompcie. Za nim stoją trzy modele z dziesięcioma scenariuszami na jedenaście, każdy z inną słabością. Pierwsza notatka po starcie zajmuje od 10 do 40 sekund — dla aplikacji, która pisze notatkę co dwie godziny, to dużo poniżej jakiegokolwiek progu cierpliwości.

Poniżej 2 GB nie ma czego szukać. Żaden z siedmiu najmniejszych modeli nie przeszedł testów po polsku, a badanie ratunkowe z promptem angielskim niczego nie zmieniło.

## Czy warto dołożyć do mocniejszego sprzętu?

**Tak, jeśli notatka ma być nie tylko prawdziwa, ale i poprawnie napisana.** Komputer z 32 GB RAM to skok od „działa z usterkami” do „działa bez wad”. Już w kontenerze, w grupie trzeciej, `gemma4:12b` za 8 GB przechodzi wszystkie scenariusze i pisze po polsku bez żadnej powtarzalnej wady. Na Ollamie uruchomionej bezpośrednio w systemie `gemma4:26b` przeszedł bez uwag wszystko, co sprawdzałem — i jako model MoE jest przy tym szybszy od mniejszego zwycięzcy grupy trzeciej.

| Sprzęt | Model | Co dostajemy |
|---|---|---|
| 16 GB RAM, bez karty graficznej | `granite4.1:8b` | prawdziwe dane we wszystkich scenariuszach, usterki formy |
| 32 GB RAM, aplikacja w kontenerze | `gemma4:12b` | wszystkie scenariusze, polszczyzna bez wad |
| 32 GB RAM, Ollama w systemie | `gemma4:26b` albo `qwen3.8:27b` | wszystko bez uwag, także narzędzia i prompt angielski |

Wszystkie są na licencji Apache 2.0.

## Czy można liczyć na polski?

**Na liczby tak, na polszczyznę bez zastrzeżeń — rzadko.** Bez powtarzalnej wady językowej pisze pięć modeli na czterdzieści cztery, wszystkie z dwóch górnych grup. Po angielsku ten sam prompt przechodzi wyraźnie więcej modeli, więc dla mniej wymagającego zastosowania angielski wciąż jest bezpieczniejszy. Najlepszymi modelami po polsku okazały się modele zagraniczne — Gemma 4 od Google, Granite od IBM i, w najnowszym wydaniu, Qwen od Alibaby. Polski Bielik podaje liczby poprawnie w większości scenariuszy, ale przegrywa zapisem liczb i wykonaniem polecenia, nie gramatyką samą w sobie.

## Czy da się z tego zbudować agenta?

**Tak, i do tego nie trzeba dużego modelu.** Pełną pętlę narzędziową — model prosi o funkcję, kod ją wykonuje, model przekazuje wynik — domyka 28 modeli na 44, a najszybciej najmniejszy z nich, za 0,37 GB. Halucynacja jednak nie znika: trzy modele przepisały kurs poprawnie, a datę notowania wymyśliły same. Kto przeniesie dane i formatowanie do kodu, a modelowi zostawi jedno zdanie, może użyć modelu małego — byle liczby i daty, które mają trafić do użytkownika, brał z wyniku narzędzia, a nie z odpowiedzi modelu.

## Czy to ma sens?

**Ma, pod jednym warunkiem: wynik trzeba sprawdzić samemu, i to czytając.** Wszystkie jedenaście scenariuszy z poprawnymi danymi przeszło sześć modeli na czterdzieści cztery. Najtrudniejszy okazał się nie spadek ani duża zmiana, tylko cztery spółki naraz — i pojedynczy ruch, którego procent trzeba było zaokrąglić w górę. Zadanie, które wygląda na banalne, bo polega na przepisaniu kilku liczb, rozróżnia modele lepiej niż jakikolwiek ranking z karty modelu.

Sprawdzanie automatyczne nie wystarcza. Dwa błędy mojej własnej miary — kwotę „97,30 PLN” uznaną za kurs 97 zł i „241,60 PLN” uznane za 24 160 zł — znalazła dopiero lektura. Automat zwraca liczbę tak samo pewnie, kiedy działa, jak wtedy, kiedy się myli.

Wiele zależy od tego, czy buduję rozwiązanie dla siebie, czy dla kogoś innego. Jeśli tylko dla siebie, łatwiej mi zostawić model, który podaje liczby poprawnie, a pisze z błędami gramatycznymi albo stylistycznymi — kropka zamiast przecinka czy niezręczne zdanie nie przeszkodzą mi zrozumieć notatki. Jeśli jednak narzędzie ma trafić do innych, choćby bezpłatnie, każdy błąd językowy świadczy o mnie jako autorze i podważa zaufanie do całej reszty, także do liczb. Wtedy błędy językowe przestają być dopuszczalne. W tym badaniu ta granica rozdziela modele wyraźnie: dla siebie wystarczy `granite4.1:8b` na komputerze z 16 GB, dla innych potrzebny jest model, który pisze bez wad — `gemma4:12b`, `gemma4:26b` albo `qwen3.8:27b` — a więc komputer z 32 GB.

I jeszcze jedno: najtańsza poprawa nie wymagała sprzętu. Zdanie w prompcie, które każe zaokrąglać liczby, zamiast pomagać, psuło wyniki — bez niego test po polsku przechodzi więcej modeli. Kto przejmie zapis liczb w kodzie, zdejmie z modelu najtrudniejszą część zadania.

## Ollama też trzeba aktualizować — ale świadomie

Oprócz modeli warto pilnować samej Ollamy, bo to ona decyduje, co w ogóle da się uruchomić. Najlepsze modele tego badania — Gemma 4, Qwen 3.8, Laguna — mają najnowsze architektury i stara wersja silnika może ich po prostu nie wczytać. Nowe wersje dokładają też informacje, które oszczędzają pracę: od wersji 0.34.3 `/api/show` mówi, jak sterować rozumowaniem modelu, zamiast kazać to sprawdzać na próbę.

Aktualizacja nie powinna jednak przychodzić sama. W trakcie tego badania Ollama na Windowsie zaktualizowała się bez pytania do 0.40.6, podczas gdy w kontenerze została 0.34.2 — i część wyników musiałem odłożyć, bo pochodziła z innej wersji silnika. Od tego czasu mam automatyczne aktualizacje wyłączone, a wersję przypiętą na obu stanowiskach. Warto też po każdej aktualizacji sprawdzić rzeczy, które w obecnej wersji działają źle: `/api/ps` zaniża zużycie pamięci przez Gemmę 4 o połowę, przy kilku modelach w pamięci Ollama potrafi przeszacować wolne miejsce i zamiast notatki zwrócić błąd, a `/api/generate` po cichu ignoruje definicje narzędzi. Każda z tych rzeczy może w nowej wersji zniknąć — albo pojawić się inna. Dlatego po zmianie wersji powtarzam pomiar na swoim zadaniu, tak samo jak po zmianie modelu.

## A ten tekst to pisałem sam, na Ollamie czy na Claudzie albo ChatGPT? 

Artykuł, który czytasz, powstał przy pomocy Claude Code i Github Copilot, w formie wtyczek do środowiska programistycznego VS Code, czyli przy pomocy AI na serwerach w chmurze. Cały kod aplikacji, skrypty oraz tabele z danymi wygenerował Claude Code. Zarządzałem aplikacją, uruchomieniami oraz monitorowałem wyniki przy użyciu Claude Code. Cały artykuł i wszystkie jego rozdziały i części zostały przez sztuczną inteligencję przeczytane fragmentami lub całościowo i poprawione, jeśli chodzi o interpunkcję, gramatykę i czasem stylistykę. Github Copilot posłużył mi jedynie na końcu do sprawdzenia raz jeszcze gotowego artykułu pod kątem literówek i innych niespójności, przed przesłaniem na serwer Githuba.

Spędziłem jednak około 3 tygodnie tworząc ten tekst, czytając każdą jego linijkę. Wprowadziłem olbrzymią liczbę zmian ręcznie i wielokrotnie musiałem Claude'a poprawić. Napisałem samodzielnie wiele całych akapitów i ustępów, włącznie z niniejszym. Przejrzałem wszystkie tabele i ręcznie weryfikowałem dane, uruchamiałem przykładowe prompty i wprowadzałem poprawki. Wiele razy zakwestionowałem to, co zobaczyłem, nie tylko dlatego, że pewne liczby nie zgadzały mi się, ale również dlatego, że artykuł poprzez zmiany proponowane przez Claude'a zmierzał nie w tę stronę, w którą chciałem. Claude najczęściej proponował zmiany, które wpierw przeczytałem, po czym osobiście wprowadziłem w miejsce poprzedniego tekstu, czytając całość jeszcze raz pod kątem stylistyki, tempa i zrozumienia przeze mnie samego. 

Przykłady błędów, które wyłapałem ja, a nie Claude Code, i decyzji, które podjąłem sam:
- miara napisana przez Claude Code uznała „97,30 PLN” za kurs 97 zł — tolerancję na zaokrąglanie kwot do całości Claude dodał sam, bez uzgodnienia ze mną, a zapytany, kiedy o niej rozmawialiśmy, przyznał, że wcale;
- Claude przy doborze przeoczył `qwen3.8:27b`, który okazał się drugim modelem grupy czwartej;
- scenariusz opisany jako „18.09 23:27” sugerował notowanie po zamknięciu sesji — w rzeczywistości był to odczyt po przerwie, z kursem zamknięcia;
- w zestawieniach scenariuszy zdarzyło się źle policzyć notatki z ruchem albo połączyć dwa cykle w jeden wiersz;
- zdecydowałem o wprowadzeniu konsumowalności notatek, podczas gdy Claude był gotów wrzucać do jednej kategorii usterki całkiem znośne i notatki zupełnie niepoprawne;
- zauważyłem, że konsumowalność nie uwzględnia innych poważnych błędów, takich jak notatka ucięta przez limit długości;
- pilnowałem, żeby jedna reguła oceny obowiązywała we wszystkich grupach — Claude potrafił tę samą wadę raz wybaczyć, a raz uznać za powód przegranej;
- dodałem scenariusze spoza pięciu cykli z jednego dnia;
- zdecydowałem, że aplikacja nie formatuje liczb, tylko przekazuje je modelowi surowe — dopiero wtedy zadanie sprawdza zaokrąglanie i zapis;
- usunąłem z listy badanych modele, które Claude wcześniej zaproponował, po przeczytaniu wyników i zrozumieniu, że nie pasują z powodu obiektywnych kryteriów, takich jak inne przeznaczenie i architektura. Zatrzymanie ich oznaczałoby tłumaczenie rozmaitych wyjątków i porównywanie wyników tak naprawdę nieporównywalnych. Na przykład na początku Claude zaproponował modele rozumujące, które ostatecznie odrzuciłem jako niepasujące do mojego zadania i mieszające wnioski.

Napisanie tego artykułu przy użyciu Claude'a kojarzy mi się z pracą z asystentem, który wykonuje najbardziej żmudną pracę, ale to ja podejmuję wszystkie decyzje, to ja zatwierdzam tekst i to ja, w razie konieczności, piszę całość od nowa. Na koniec chcę mieć poczucie, że to mój tekst i moja praca i mogę za nią odpowiadać. Fakt, że sztuczna inteligencja pomogła mi stworzyć ten artykuł i na pewno dzięki niej, mimo skali badania (44 modele!), mógł on powstać relatywnie szybko, jest również dobitnym dowodem na to, że AI trafiła już pod strzechy i trafiło bardzo pewnie.

## Co dalej

Sztuczna inteligencja trafia pod strzechy tak, jak trzydzieści lat temu trafiały komputery: najpierw do tych, którzy mają mocniejszy sprzęt i trochę cierpliwości. Ale granica przesuwa się szybko — modele, które wygrały to badanie, mają po kilka miesięcy, a aktualizacja potrafi się opłacić od razu: `qwen3.8:27b`, nowsze wydanie tego samego modelu co `qwen3.6:27b`, naprawił jedyny błąd poprzednika, jest szybszy i wszedł na podium swojej grupy. 

Moja najważniejsza lekcja jest taka, że każdy nowy model warto najpierw samodzielnie uruchomić.

[Rozdział piąty](./AI%20pod%20strzechy%2005%20Sprawdź%20to%20sam.md) pokazuje, jak powtórzyć to badanie u siebie.
