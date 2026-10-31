# Część 2. Kryteria wyboru modeli językowych

Niniejszy tekst tłumaczy różne kryteria wyboru modeli językowych do mojego testu. A są to:
- kryterium producenta
- kryterium obecności na Ollamie
- kryterium licencyjne
- kryterium konwersacyjne po polsku
- kryterium trybu instruct (lub wyłączalnego rozumowania)
- kryterium wieku modelu
- kryterium rozmiaru pliku modelu
- kryterium względnej łatwości obsłużenia w Node.js

## 2.1. Omówienie kryteriów
### 2.1.1. Kryterium producenta
Moja pierwsza zasada jest następująca:
> **Testuję wyłącznie modele publikowane przez ich producentów.**

Nie „konwersje społecznościowe", nie warianty „abliterated", nie przerobione prompty systemowe podszyte pod cudze wagi. Powód jest prozaiczny: przy modelu od producenta wiem, co uruchamiam. Przy konwersji od osoby postronnej mogę nawet nie wiedzieć z którego dokładnie checkpointu powstała.

Nie chodzi tutaj też o żadne zabieganie o popularność ("fame"), ponieważ testuję tylko modele Google'a czy OpenAI czy IBM. Liczę się z prawdopodobieństwem, że użytkownik w ogóle zetknie się z takim modelem również, czy to w domu, czy szkole, na uczelni czy w pracy. Popularne modele są również statystycznie lepiej wspierane, czy to przez samego producenta, czy społeczność oraz mogą liczyć w przyszłości na dodatkowe aktualizacje. Być może będzie to kryterium zatem krzywdzące dla niektórych modeli, które na moją listę nie trafiły, ale też nie jest to artykuł zajmujący się bezwzględnym przeglądem rynku wszystkich dostępnych modeli. Nie oceniam również, czy dana publikacja jest godna zaufania w ogóle. Sprawdzam jedynie trzy konkretne rzeczy: czy wagi pochodzą z konta wydawcy, czy licencja jest ustalona, i czy model robi to, co deklaruje jego opis.

### 2.1.2 Kryterium: Obecność na Ollamie
Druga zasada, jaką przyjąłem, jest następująca i nie przewiduję wyjątków:

> **Model wchodzi do badania, jeśli jego oficjalny wydawca publikuje go w bibliotece Ollamy.**

Bez dostępnych gdziekolwiek indziej konwersji społecznościowych, bez wariantów przerobionych przez osoby trzecie, bez ręcznego importu wag. Chciałbym, aby ten tekst był maksymalnie przystępny, a więc **to badanie opisuje drogę, którą przejdzie czytelnik**. 
Jeśli uruchomienie modelu wymaga konwersji formatu i kilkudziesięciu minut pracy, to nie jest tym razem w zakresie mojego zainteresowania.

### 2.1.3. Kryterium licencyjne
Trzecim założeniem jest, aby licencja, na zasadzie której model jest rozprowadzany, pozwalała na swobodne użycie przez osobę prywatną, bez budzenia obaw o potencjalne ograniczenia w przyszłości, w tym użycie komercyjnie. Mogę jednak dopuścić wyjątek, jeśli zbadanie modelu da mi możliwość jakiegoś porównania.

### 2.1.4. Kryterium konwersacyjne po polsku
Czwarta zasada: interesują mnie modele konwersacyjne, które poradzą sobie zarówno z tekstem pisanym, jak i wypowiedzią na zadany temat w języku polskim. Polskiego nie wymagam w deklaracji wydawcy — sprawdzam go w pomiarze. Modele, których wydawca polskiego nie obiecuje, traktuję jako próbę: czy i tak dadzą radę. Zatem:

> **Model wchodzi do badania, jeśli jest konwersacyjny, ale nie musi deklarować wsparcia dla języka polskiego.**

Uwaga: skupiam się na powierzaniu AI wąskich, powtarzalnych zadań. Lokalne LLMy nie są zamiennikiem dla pracy, która wymaga długiego kontekstu, wielu narzędzi i weryfikacji własnych wyników - tutaj pomogą jedynie modele zamknięte i nie są one tematem tego tekstu.
Także modele specjalne, tworzone na potrzeby przetwarzania obrazów, mowy, osadzeń, naukowe itd. nie leżą w moim spektrum zainteresowania podczas tego testu.
Jeśli model jest jednocześnie konwersacyjny i inny (czyli multimodalny), to jest to ok. 

### 2.1.5. Kryterium trybu instruct (lub wyłączalnego rozumowania)
Zadanie jest transkrypcyjne: model ma przepisać kilka liczb i ułożyć z nich zdanie. Rozumowanie przed odpowiedzią niczego nie wnosi, a będzie kosztować czas i budżet tokenów. Dlatego

**do badania wchodzą modele instrukcyjne oraz hybrydowe — te, którym rozumowanie da się wyłączyć.**

Wyłączalność sprawdziłem dla upewnienia się poprzez testowe uruchomienie, nie deklaracją: model z flagą think: false musi odpowiedzieć bez brudnopisu. Deklaracja w /api/show bywa pomocna, ale nie zawsze jest — część modeli wyłączalnych jej nie ma. Zrobię wyjątek, jeśli rozumowania nie da się wyłączyć ale ograniczyć w inny sposób. 

### 2.1.6. Kryterium wieku modelu
Sytuacja na arenie generatywnej sztucznej inteligencji jest bardzo dynamiczna i praktycznie co kwartał, a nawet co miesiąc można usłyszeć nowe, fascynujące doniesienia o dodatkowych funkcjach i możliwościach modeli językowych. Intuicyjnie darzę pewną nieufnością zupełnie nowe rozwiązania, zapewne wyniesioną z pracy i podpartą latami doświadczeń w informatyce i dziesiątkami rozczarowań. Nie uważam za stosowne skreślać modelu, który został wydany rok temu, tylko dlatego, że "tak wiele się zmieniło". 
Przynajmniej nie dla mojego niniejszego testu. 
Ale zgadzam się z założeniem, że modele dwu- i więcej-letnie są prawdopodobnie już przestarzałe. Dlatego 
> **do testów wybieram modele z obecnej i maksymalnie jednej poprzedniej generacji.** 

Ale jeśli model mnie interesujący nie ma młodszego odpowiednika, mogę w drodze wyjątku również go wypróbować. Zobaczymy. Proszę mnie również nie rozliczać z precyzji wypowiedzi czy generacja poprzednia wobec 4.2 to 3.x czy już 4.0.

### 2.1.7. Kryterium rozmiaru pliku modelu
Modele wybieram również według tego, na jakim sprzęcie da się je uruchomić. To najważniejszy podział dla kogoś, kto ma dopiero kupić komputer. 
Dlatego
> **Model ma dać się uruchomić na przeciętnym komputerze domowym, czyli PC z zainstalowanym systemem Windows, z max 32 GB RAM i bez specjalnego GPU lub NPU.**

Wyróżniam cztery grupy wielkości:

- **Grupa 1 — poniżej 2 GB.** Zmieszczą się wszędzie, także na sprzęcie słabszym niż zakładany. Istnieją, więc zbadam je i sprawdzę, gdzie leży dolna granica sensu.
- **Grupa 2 — od 2 do 6 GB.** Serce badania. Mieszczą się na maszynie obecnie najpopularniejszej z 16 GB bez specjalistycznej karty graficznej, a przy karcie z 8 GB pamięci mieszczą się w niej w całości. To prawdopodobnie ta grupa zdecyduje o odpowiedzi na główne pytanie z tego wpisu.
- **Grupa 3 — od 6 do 14 GB.** Raczej dla konfiguracji 32 GB RAM z kontenerem w Podman na Windows z WSL-em z dostępnymi 16 GB pamięci RAM. Poza zasięgiem karty z 8 GB, np. laptopowej Nvidii RTX 5050.
- **Grupa 4 — od 14 do 22 GB.** Wyłącznie stanowisko hostowe, nie kontenerowe i wyłącznie dla konfiguracji 32 GB RAM. Domknięcie luki: sprawdzę, co traci ten, kto zatrzymuje się na tańszym sprzęcie np. 16 GB RAM host. Sufit w postaci 22 GB zajętej pamięci przy sprzęcie 32 GB RAM zakłada pozostawienie ok 10 GB wolnej pamięci dla funkcjonowania samego systemu operacyjnego oraz innych koniecznych procesów na komputerze. 

## 2.2. Dwa polskie modele, dwie różne sytuacje

Polszczyzna jest w tym zadaniu jednym z obszarów zainteresowań, więc interesują mnie modele tworzone w Polsce. Są dwa główne i różnią się właśnie tym, czego dotyczą powyższe kryteria.
- **Bielik** (SpeakLeash i ACK Cyfronet AGH) spełnia je w całości. Zespół prowadzi w Ollamie własne konto z dziewięcioma repozytoriami, a cztery warianty najnowszej generacji v3.0 — 1,5B, 4,5B, Minitron 7B i 11B — pobiera się jednym poleceniem.
- **PLLuM** (konsorcjum pod egidą Ministerstwa Cyfryzacji) **do badania nie wchodzi** — i warto powiedzieć wprost dlaczego, bo powód nie leży po stronie jakości modelu.

PLLuM jest oficjalnie obecny na HuggingFace, można znaleźć cztery rozmiary w najnowszej generacji `-2512` (4B, 8B, 12B, 70B), czytelny podział licencyjny z wariantami badawczymi oznaczonymi członem `nc`, opisane architektury bazowe. Wariant 4B po kwantyzacji waży około 2,5 GB, czyli trafia dokładnie w tę klasę sprzętu, o której jest ten artykuł.

Ale **konsorcjum nie prowadzi konta w Ollamie**. Nie zapytałem konsorcjum, ale wszystko, co tam znalazłem to pięć repozytoriów od czterech osób prywatnych. Są to konwersje społecznościowe, w większości bez readme, bez licencji w pliku i bez wskazania checkpointu źródłowego. 

Taka sytuacja zaistniała przynajmniej w momencie pisania tego tekstu. Dzieje się tak zapewne dlatego, że PLLuM celuje w zastosowania instytucjonalne i badawcze, gdzie wagi pobiera się z HuggingFace i wdraża własnym łańcuchem narzędzi. Z drugiej strony Bielik celuje również w użytkownika, który chce napisać `ollama pull` i mieć działający model. To dwie różne strategie i obie są uprawnione — ale tylko jedna z nich mieści się dla mnie w definicji "AI pod strzechy", o których jest ten tekst. Stąd tylko Bielik.

Zostawiam to jako temat na osobny artykuł: co da się wycisnąć z modeli, których nie ma w gotowych bibliotekach, a są warte uwagi, i ile to kosztuje pracy.

### 2.2.1 Wsparcie Unii Europejskiej

W internecie można przeczytać również o ciekawym projekcie europejskim — EuroLLM. Tworzy go konsorcjum ośmiu instytucji (Instituto Superior Técnico, University of Edinburgh, Instituto de Telecomunicações, Université Paris-Saclay, Unbabel, Sorbonne, Naver Labs i University of Amsterdam), finansowane przez Horizon Europe w ramach projektu UTTER, Europejską Radę ds. Badań Naukowych i EuroHPC; model trenowano na superkomputerze MareNostrum 5. Licencja to Apache 2.0.

EuroLLM obsługuje wszystkie 24 języki urzędowe UE, w tym polski, a do treningu użyto danych w 35 językach. W najnowszym wydaniu dostępne są trzy modele: EuroLLM-22B, EuroLLM-9B oraz mieszanka ekspertów EuroMoE-2.6B-A0.6B; starszy EuroLLM-1.7B pochodzi z poprzedniej generacji.

Jednak w oficjalnej bibliotece Ollamy EuroLLM-a nie ma, a konsorcjum nie publikuje tam niczego. Wydawca nie udostępnia nawet plików GGUF — w żadnym repozytorium utter-project na HuggingFace nie ma ani jednego. Istnieją tylko konwersje osób trzecich. Dlatego, podobnie jak PLLuM, EuroLLM nie znalazł się w moim zestawieniu.

Uwaga na nazwy: w Ollamie pod hasłem „OpenEuroLLM" znajdziemy serię modeli w wielu językach, w tym OpenEuroLLM-Polish — to jednak Gemma 3 dostrojona przez anonimowe konto, bez związku z EuroLLM. Tych modeli również nie uwzględniłem.

## 2.3. Co z największymi

Swoje modele publikują w rozmiarach domowych wszyscy znani i duzi producenci — IBM, Microsoft, Meta, Google, Alibaba, Mistral, OpenAI (gpt-oss), NVIDIA, LG. 
DeepSeek publikuje swoje modele w rozmiarach serwerowni, ale z wariantami domowymi do pobrania, tzw. destylaty. 
Niektórzy publikują swoje modele wyłącznie w rozmiarach serwerowni, bez wariantów "domowych" np. xAI (Elon Musk). 
Swoich modeli nie publikują do pobrania wcale — Anthropic, Amazon, Google w linii Gemini. Nie publikują i/lub nie zajmują się tworzeniem modeli AI open-source wcale Netflix, Oracle, Dell, Broadcom, SAP, Siemens, Sony czy Hitachi.

Zatem 
> porównanie nie ma sensu dla dużych modeli znanych producentów, które nie są dostępne dla pobrania lub są za duże.

## 2.4. Wybór modeli

Do testów wybrałem zatem następujące **44 modele językowe** 12 producentów. Dane przedstawione poniżej są odczytane z `/api/show` i `/api/tags` obu instalacji Ollamy, nie z kart modeli.
Kolumna **Gr.** oznacza grupę wielkości: **1** — poniżej 2 GB, **2** — od 2 do 6 GB,
**3** — od 6 do 14 GB (wszystkie trzy na stanowisku kontenerowym), **4** — od 14 do 22 GB, stanowisko hostowe.
Kolumna **Dodany** to data utworzenia repozytorium wag na koncie wydawcy w HuggingFace. Publiczna premiera bywa do miesiąca późniejsza — repozytorium Gemmy 4 powstało 2 marca, premiera była 2 kwietnia 2026.

| Model | Gr. | Producent | Dodany | Param | Aktywne | Kwant. | Plik | Kontekst | Architektura | Typ | Licencja | Komerc. | Ollama |
|---|---|---|---:|---|---|---|---|---|---|---|---|---|---|
| `granite4:350m-h` | 1 | IBM | 2025-10-07 | 340 mln | — | Q8_0 | 366 MB | 1 048 576 | *granitehybrid* | hybrydowy | Apache 2.0 | tak | biblioteka |
| `qwen3.5:0.8b` | 1 | Alibaba | 2026-02-28 | 873 mln | — | Q8_0 | 1,04 GB | 262 144 | *qwen35* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `llama3.2:1b` | 1 | Meta | 2024-09-18 | 1,2 mld | — | Q8_0 | 1,32 GB | 131 072 | *llama* | gęsty | **Llama 3.2 Community** | **warunkowo** | biblioteka |
| `granite4:1b-h` | 1 | IBM | 2025-10-07 | 1,5 mld | — | Q8_0 | 1,56 GB | 1 048 576 | *granitehybrid* | hybrydowy | Apache 2.0 | tak | biblioteka |
| `exaone3.5:2.4b` | 1 | LG AI Research | 2024-12-01 | 2,7 mld | — | Q4_K_M | 1,64 GB | 32 768 | *exaone* | gęsty, **bez narzędzi** | **EXAONE 1.1 – NC** | **nie** | biblioteka |
| `falcon3:1b` | 1 | TII | 2024-12-14 | 1,7 mld | — | Q8_0 | 1,78 GB | 8192 | *llama* | gęsty, **bez narzędzi** | **TII Falcon** | **warunkowo** | biblioteka |
| `granite4:micro-h` | 1 | IBM | 2025-09-16 | 3,2 mld | — | Q4_K_M | 1,94 GB | 1 048 576 | *granitehybrid* | hybrydowy | Apache 2.0 | tak | biblioteka |
| `falcon3:3b` | 2 | TII | 2024-12-14 | 3,2 mld | — | Q4_K_M | 2,01 GB | 32 768 | *llama* | gęsty, **bez narzędzi** | **TII Falcon** | **warunkowo** | biblioteka |
| `llama3.2:3b` | 2 | Meta | 2024-09-18 | 3,2 mld | — | Q4_K_M | 2,02 GB | 131 072 | *llama* | gęsty | **Llama 3.2 Community** | **warunkowo** | biblioteka |
| `granite4.1:3b` | 2 | IBM | 2026-04-06 | 3,4 mld | — | Q4_K_M | 2,10 GB | 131 072 | *granite* | gęsty | Apache 2.0 | tak | biblioteka |
| `granite4.2:3b` | 2 | IBM | 2026-08-07 | 3,7 mld | — | Q4_K_M | 2,24 GB | 131 072 | *granite* | gęsty, rozumowanie wyłączalne | Apache 2.0 | tak | biblioteka |
| `phi4-mini` | 2 | Microsoft | 2025-02-19 | 3,8 mld | — | Q4_K_M | 2,49 GB | 131 072 | *phi3* | gęsty | MIT | tak | biblioteka |
| `qwen3.5:2b` | 2 | Alibaba | 2026-02-28 | 2,3 mld | — | Q8_0 | 2,74 GB | 262 144 | *qwen35* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `nemotron-3-nano:4b` | 2 | NVIDIA | 2026-03-07 | 4,0 mld | — | Q4_K_M | 2,84 GB | 262 144 | *nemotron_h* | hybrydowy, rozumowanie wyłączalne | NVIDIA Open Model | tak | biblioteka |
| `ministral-3:3b` | 2 | Mistral AI | 2025-10-31 | 3,8 mld | — | Q4_K_M | 2,95 GB | 262 144 | *mistral3* | gęsty, multimodalny | Apache 2.0 | tak | biblioteka |
| `qwen3.5:4b` | 2 | Alibaba | 2026-02-27 | 4,7 mld | — | Q4_K_M | 3,39 GB | 262 144 | *qwen35* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `granite4:7b-a1b-h` | 2 | IBM | 2025-09-16 | 6,9 mld | **~1 mld** | Q4_K_M | 4,23 GB | 1 048 576 | *granitehybrid* | **MoE**, hybrydowy | Apache 2.0 | tak | biblioteka |
| `gemma4:e2b-it-qat` | 2 | Google | 2026-03-02 | 4,6 mld | — | Q4_0 | 4,34 GB | 131 072 | *gemma4* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `bielik-minitron-7B-v3.0-instruct:Q4_K_M` | 2 | SpeakLeash / ACK Cyfronet AGH | 2026-01-22 | 7,5 mld | — | Q4_K_M | 4,50 GB | 32 768 | *llama* | gęsty, polski | Apache 2.0\* | tak | konto wydawcy |
| `falcon3:7b` | 2 | TII | 2024-11-29 | 7,5 mld | — | Q4_K_M | 4,57 GB | 32 768 | *llama* | gęsty, **bez narzędzi** | **TII Falcon** | **warunkowo** | biblioteka |
| `exaone3.5:7.8b` | 2 | LG AI Research | 2024-12-01 | 7,8 mld | — | Q4_K_M | 4,77 GB | 32 768 | *exaone* | gęsty, **bez narzędzi** | **EXAONE 1.1 – NC** | **nie** | biblioteka |
| `llama3.1:8b` | 2 | Meta | 2024-07-18 | 8,0 mld | — | Q4_K_M | 4,92 GB | 131 072 | *llama* | gęsty | **Llama 3.1 Community** | **warunkowo** | biblioteka |
| `granite4.2:8b` | 2 | IBM | 2026-08-07 | 8,8 mld | — | Q4_K_M | 5,35 GB | 131 072 | *granite* | gęsty, rozumowanie wyłączalne | Apache 2.0 | tak | biblioteka |
| `granite4.1:8b` | 2 | IBM | 2026-04-06 | 8,8 mld | — | Q4_K_M | 5,35 GB | 131 072 | *granite* | gęsty | Apache 2.0 | tak | biblioteka |
| `ministral-3:8b` | 3 | Mistral AI | 2025-10-31 | 8,9 mld | — | Q4_K_M | 6,02 GB | 262 144 | *mistral3* | gęsty, multimodalny | Apache 2.0 | tak | biblioteka |
| `falcon3:10b` | 3 | TII | 2024-12-14 | 10,3 mld | — | Q4_K_M | 6,29 GB | 32 768 | *llama* | gęsty, **bez narzędzi** | **TII Falcon** | **warunkowo** | biblioteka |
| `qwen3.5:9b` | 3 | Alibaba | 2026-02-27 | 9,0 mld | — | Q4_K_M | 6,55 GB | 262 144 | *qwen35* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `bielik-11b-v3.0-instruct:Q4_K_M` | 3 | SpeakLeash / ACK Cyfronet AGH | 2025-11-07 | 11,2 mld | — | Q4_K_M | 6,72 GB | 32 768 | *llama* | gęsty, polski | Apache 2.0\* | tak | konto wydawcy |
| `gemma4:12b` | 3 | Google | 2026-05-23 | 11,9 mld | — | Q4_K_M | 8,02 GB | 262 144 | *gemma4* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `phi4` | 3 | Microsoft | 2024-12-11 | 14,7 mld | — | Q4_K_M | 9,05 GB | 16 384 | *phi3* | gęsty, **bez narzędzi** | MIT | tak | biblioteka |
| `ministral-3:14b` | 3 | Mistral AI | 2025-10-31 | 13,9 mld | — | Q4_K_M | 9,08 GB | 262 144 | *mistral3* | gęsty, multimodalny | Apache 2.0 | tak | biblioteka |
| `qwen3:14b` | 3 | Alibaba | 2025-04-27 | 14,8 mld | — | Q4_K_M | 9,28 GB | 40 960 | *qwen3* | gęsty, rozumowanie wyłączalne | Apache 2.0 | tak | biblioteka |
| `gemma4:e4b` | 3 | Google | 2026-03-02 | 8,0 mld | — | Q4_K_M | 9,61 GB | 131 072 | *gemma4* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `gpt-oss:20b` | 4 | OpenAI | 2025-08-04 | 20,9 mld | 4 z 32 ekspertów | MXFP4 | 13,79 GB | 131 072 | *gptoss* | **MoE**, rozumujący, poziom „low"\*\* | Apache 2.0 | tak | biblioteka |
| `mistral-small3.2` | 4 | Mistral AI | 2025-06-19 | 24,0 mld | — | Q4_K_M | 15,18 GB | 131 072 | *mistral3* | gęsty, multimodalny | Apache 2.0 | tak | biblioteka |
| `granite4.1:30b` | 4 | IBM | 2026-04-06 | 28,9 mld | — | Q4_K_M | 17,49 GB | 131 072 | *granite* | gęsty | Apache 2.0 | tak | biblioteka |
| `granite4.2:30b` | 4 | IBM | 2026-08-07 | 29,3 mld | — | Q4_K_M | 17,72 GB | 131 072 | *granite* | gęsty, rozumowanie wyłączalne | Apache 2.0 | tak | biblioteka |
| `qwen3.8:27b` | 4 | Alibaba | 2026-08-05 | 27,3 mld | — | Q4_K_M | 17,74 GB | 262 144 | *qwen35* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `qwen3.6:27b` | 4 | Alibaba | 2026-04-21 | 27,3 mld | — | Q4_K_M | 17,77 GB | 262 144 | *qwen35* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `gemma4:26b` | 4 | Google | 2026-03-11 | 25,2 mld | **~4 mld** | Q4_K_M | 18,73 GB | 262 144 | *gemma4* | **MoE**, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |
| `exaone3.5:32b` | 4 | LG AI Research | 2024-12-01 | 32,0 mld | — | Q4_K_M | 19,34 GB | 32 768 | *exaone* | gęsty, **bez narzędzi** | **EXAONE 1.1 – NC** | **nie** | biblioteka |
| `granite4:32b-a9b-h` | 4 | IBM | 2025-09-16 | 32,2 mld | **~9 mld** | Q4_K_M | 19,48 GB | 1 048 576 | *granitehybrid* | **MoE**, hybrydowy | Apache 2.0 | tak | biblioteka |
| `laguna-xs-2.1:q4_K_M` | 4 | Poolside | 2026-06-20 | 33,4 mld | **~3 mld** | Q4_K_M | 20,27 GB | 262 144 | *laguna* | **MoE**, rozumowanie wyłączalne | OpenMDW-1.1 | tak | biblioteka |
| `gemma4:31b` | 4 | Google | 2026-03-11 | 30,7 mld | — | Q4_K_M | 20,39 GB | 262 144 | *gemma4* | gęsty, rozumowanie wyłączalne, multimodalny | Apache 2.0 | tak | biblioteka |

\* Brak licencji w samym pliku modelu; readme na koncie wydawcy wskazuje Apache 2.0 wraz
z regulaminem na bielik.ai/terms i linkuje do oryginału na HuggingFace. To kanał wydawcy,
nie konwersja osoby postronnej.

\*\* Jedyny wyjątek od kryterium z części 2.1.5: rozumowania `gpt-oss` nie da się wyłączyć,
ale da się je ograniczyć do najniższego poziomu „low" — i na nim go mierzę.

**Razem 44 modele** — grupa 1: 7, grupa 2: 17, grupa 3: 9, grupa 4: 11.
**Razem 12 producentów**. IBM i Alibaba to dwie piąte stawki — 18 z 44. Obaj publikują wiele wariantów rozmiarowych w Ollamie i stąd nadreprezentacja. Dwa ostatnie modele dołożyłem już po pierwszym przebiegu badania: `qwen3.8:27b` stoi obok starszego `qwen3.6:27b` w tym samym rozmiarze — wbrew mojej regule doboru, ale celowo, żeby pokazać, co daje jeden krok wersji — a `laguna-xs-2.1` od Poolside opisuję w części 2.8. Alibaba jako jedyny jest obecny we wszystkich czterech grupach. IBM jest w 1, 2 i 4, ale nie ma nic w przedziale 6–14 GB; tę lukę zamykają u siebie TII, którego rodzina `falcon3` pokrywa grupy 1–3, oraz Mistral z rodziną `ministral-3` w grupach 2 i 3. Google zaczyna się od grupy 2, za to w grupie 4 ma aż dwa modele. LG z rodziną `exaone3.5` jest w grupach 1, 2 i 4. Meta jest tylko w grupach 1 i 2 — `llama3.2:1b`, `llama3.2:3b` i `llama3.1:8b` — bo jej nowsze modele, Llama 3.3 i Llama 4, zaczynają się od 70 mld parametrów i na domową maszynę się nie mieszczą. Microsoft ma po jednym modelu w grupach 2 i 3 (`phi4-mini` i `phi4`), a SpeakLeash dwa Bieliki, także w grupach 2 i 3.
NVIDIA ma w stawce tylko jeden model czterogigowy z grupy 2 — jej większe modele przekraczają 22 GB i opisuję je w części 2.6. OpenAI i Poolside mają po jednym modelu i tylko w grupie 4 — to jedyni wydawcy, którzy nie dają żadnej opcji na przeciętną maszynę domową: `gpt-oss:20b` waży 13,79 GB, a `laguna-xs-2.1` 20,27 GB, i oba mierzę wyłącznie na hoście.
---

Zdolności modeli w całej stawce, odczytane z `/api/show`:

| Zdolność | Modeli | Czego dotyczy |
|---|---|---|
| `completion` | 44 | generowanie tekstu — ma każdy |
| `tools` | **36** | wywoływanie funkcji, czyli warunek bycia agentem |
| `thinking` | **18** | tok rozumowania przed odpowiedzią — tu wyłączalny, poza `gpt-oss` |
| `vision` | 15 | obraz na wejściu |
| `audio` | 3 | dźwięk na wejściu — wyłącznie mniejsze `gemma4` |

---
Kilka rzeczy, które widać w tej tabeli, wartych uwagi:

**Osiem modeli w ogóle nie deklaruje narzędzi.** Nie chodzi o to, że poradzą sobie
z nimi słabo — Ollama nie wystawia im takiego interfejsu i odrzuca zapytanie
z listą funkcji, zanim model cokolwiek zobaczy. Są to cała rodzina `falcon3`, cała
rodzina `exaone3.5` oraz `phi4`. Nie wyklucza ich to z badania, zamyka
im to natomiast drogę do aplikacji, która sama sięga po
dane. Kto planuje pójść dalej niż jedno zapytanie, powinien na tę kolumnę spojrzeć
wcześniej niż na czas odpowiedzi.
Odwrotna zależność, uprzedzę, nie zachodzi: **deklaracja `tools` nie znaczy, że
model naprawdę wywoła funkcję.** Sprawdziłem to osobnym testem i wracam do tego
w części o wynikach, bo rozjazd między deklaracją a zachowaniem okazał się jedną
z ciekawszych rzeczy w całym badaniu.

**Rozumowanie zawiera się w narzędziach.** Wszystkie 18 modeli deklarujących rozumowanie ma też `tools`, więc te dwie kolumny nigdy nie wejdą ze sobą w konflikt. Odwrotnie już nie: osiemnaście modeli narzędziowych rozumowania nie ma.

**Deklaracja rozumowania nie mówi wszystkiego.** Od wersji 0.34.3 Ollama podaje w `/api/show` także, jak rozumowaniem sterować — na przykład `"values": [false, true]` znaczy, że da się je wyłączyć, a `gpt-oss` ma zamiast tego poziomy `low`, `medium` i `high`. Ale trzy modele `granite4.2` deklarują rozumowanie, a tych metadanych nie mają wcale, choć wyłączają je bez kłopotu. Dlatego wyłączalność z części 2.1.5 sprawdzałem uruchomieniem, nie odczytem.

**Nazwy `gemma4` mylą co do rozmiaru.** `e2b` nie znaczy 2 mld parametrów, tylko *effective 2B* —
model ma 4,6 mld parametrów i waży 4,34 GB. Analogicznie `e4b` to 8,0 mld i 9,61 GB, przez co
trafia do grupy 3, a nie 2. Podobnie `qwen3.5:0.8b` ma w rzeczywistości 873 mln, a `qwen3.5:2b`
waży 2,74 GB mimo 2,3 mld parametrów, bo idzie w Q8_0, nie Q4. Z kolei `falcon3:3b` waży 2,01 GB — sześć megabajtów nad progiem — i trafia do grupy 2, nie 1, a `ministral-3:8b` z 6,02 GB do grupy 3.

**Kolumna „Aktywne" pochodzi z manifestu, nie z nazwy.** `granite4:7b-a1b-h` używa 6 z 64
ekspertów, `granite4:32b-a9b-h` — 10 z 72, `gpt-oss:20b` — 4 z 32, `gemma4:26b` — 8 ze 128, a `laguna-xs-2.1` — 8 z 256.

**Rozumowanie doszło między wersjami, i to całą linią.** Wszystkie trzy modele `granite4.2` —
3b, 8b i 30b — mają tryb rozumowania, a żaden z ich odpowiedników z linii 4.1 go nie ma.
Ten sam producent, ta sama architektura, te same rozmiary, jedna różnica — i w tym badaniu
obie linie stoją obok siebie, bo rozumowanie w 4.2 da się wyłączyć.

**Obraz na wejściu przestał być domeną dużych modeli.** Cała linia `qwen3.5` przyjmuje obraz,
łącznie z wariantem 873-milionowym, a Mistral daje go całej rodzinie `ministral-3`, od 3 mld
parametrów. Najdalej idzie `gemma4`: trzy mniejsze rozmiary — `e2b`, `e4b` i `12b` — mają
komplet, czyli narzędzia, rozumowanie, obraz i dźwięk.

**Architektura `granite` to nie `granitehybrid`.** Linie 4.1 i 4.2 porzuciły hybrydę Mamby —
widać to wprost w manifeście, razem ze spadkiem kontekstu z 1 048 576 na 131 072. W drugą
stronę poszła NVIDIA: `nemotron-3-nano` w rozmiarze 4 mld jest hybrydą z oknem 262 144.
Najmniejsze okna w stawce mają `falcon3:1b` (8 192) i `phi4` (16 384).

## 2.5. Słowo o licencjach
Zidentyfikowałem osiem typów licencji na czterdzieści cztery modele, choć nie u wszystkich modeli były one udokumentowane w pliku:

| Licencja | Modeli | Komercyjnie |
|---|---|---|
| Apache 2.0 | **28** | tak |
| Apache 2.0, ale nie w pliku modelu\* | 2 | tak |
| TII Falcon | 4 | warunkowo |
| EXAONE 1.1 – NC | 3 | **nie** |
| MIT | 2 | tak |
| Llama 3.2 Community | 2 | warunkowo |
| Llama 3.1 Community | 1 | warunkowo |
| NVIDIA Open Model | 1 | tak |
| OpenMDW-1.1 | 1 | tak |

**Trzydzieści cztery modele z czterdziestu czterech są na licencji bez zastrzeżeń.** Siedem ma warunki, a trzy — cała rodzina `exaone3.5` — zabraniają użytku komercyjnego.

---
Krótki przegląd.

**Apache 2.0** — w pełni otwarta licencja uznawana przez Open Source Initiative (OSI). Użytek komercyjny, modyfikacja i redystrybucja bez ograniczeń; wymaga zachowania not o prawach autorskich i oznaczenia wprowadzonych zmian. Zawiera wyraźne udzielenie praw patentowych, czego MIT nie ma. Najliczniejsza w tej stawce i najprostsza w zastosowaniu. Tu: cały IBM, cały Alibaba, cały Mistral, Gemma 4 i `gpt-oss`.

**MIT** — otwarta licencja, jeszcze krótsza niż Apache. Pozwala na wszystko przy zachowaniu noty autorskiej. Różnica wobec Apache 2.0 sprowadza się do braku klauzuli patentowej — w praktyce dla małej firmy bez znaczenia. Tu: oba Phi.

**Llama 3.1 i 3.2 Community** — obie wersje mają identyczną konstrukcję i różnią się tylko datą odniesienia. Teksty obu licencji w plikach modeli:

- **próg 700 mln aktywnych użytkowników miesięcznie** — *„on the Llama 3.1 version release date"*, czyli stan na dzień wydania danej wersji, a nie na "dziś". Dla Llamy 3.1 jest to 23 lipca 2024. Kto nie miał wtedy produktu z 700 milionami użytkowników, tego ten warunek **nie obejmie już nigdy** — jest martwy, nie odroczony, ale nie jestem specjalistą w temacie i należałoby to jeszcze sprawdzić. 
- obowiązek widocznego oznaczenia **„Built with Llama"**;
- wymóg, by nazwa modelu pochodnego **zaczynała się od „Llama"** — ale tylko wtedy, gdy się go rozpowszechnia.

To licencje warunkowe, nie otwarte w rozumieniu OSI. Dla rozwiązania domowego lub małej firmy warunki sprowadzają się do jednej linijki w stopce.

**TII Falcon License** — według własnego wstępu oparta w części na Apache 2.0, ale z dołożoną polityką dopuszczalnego użycia (*Acceptable Use Policy*), którą wydawca publikuje na swojej stronie i która *„may be updated from time to time"*. Używać trzeba zgodnie z jej aktualną wersją, a licencjobiorca odpowiada też za to, by przestrzegali jej wszyscy, którzy używają modelu w jego imieniu. Użytek komercyjny jest dozwolony, ale warunki mogą się zmienić bez zmiany samego modelu — stąd „warunkowo".

**EXAONE AI Model License 1.1 – NC** — jedyna w stawce, która komercji zabrania, i to szeroko:

> *The Licensee is expressly prohibited from using the Model, Derivatives, or Output for any commercial purposes, including but not limited to, developing or deploying products, services, or applications that generate revenue, whether directly or indirectly.*

Zakaz obejmuje nie tylko model, ale i to, co model wypisze — notatki z mojej aplikacji też. Badanie i ewaluacja mieszczą się w przyznanym prawie wprost, użytek domowy również. Dotyczy całej rodziny `exaone3.5`; zasięgnąłbym opinii specjalisty, zanim cokolwiek z jej wyników trafiłoby do firmy.

**NVIDIA Open Model License** — mimo długości tekstu stwierdza wprost: *„Models are commercially usable."* Dotyczy `nemotron-3-nano`. Warto wiedzieć, że starsze wydania NVIDII, jak `nemotron-mini`, chodzą na zupełnie innej licencji, ograniczonej do użytku nieprodukcyjnego — u tego samego wydawcy nazwa licencji ma znaczenie.

**OpenMDW-1.1** — licencja Linux Foundation i PyTorch Foundation z 2025 roku, napisana pod modele: obejmuje naraz wagi, dane i kod. Pozwala używać, modyfikować i rozpowszechniać materiały „bez ograniczeń" i zawiera udzielenie praw patentowych — w praktyce tyle co Apache 2.0. Dotyczy `laguna-xs-2.1`. Ten sam wydawca, Poolside, poprzednie wydanie Laguna XS.2 dawał jeszcze na Apache 2.0.

**Brak licencji w pliku modelu** — plik GGUF nie zawiera żadnego pliku licencyjnego, więc `ollama show --license` nic nie zwraca. Dotyczy obu Bielików z badania. **To nie jest to samo co licencja nieustalona**: konto należy do wydawcy (SpeakLeash i ACK Cyfronet AGH), readme wskazuje Apache 2.0 wraz z regulaminem na bielik.ai/terms i linkuje do oryginału na HuggingFace. Pochodzenie da się prześledzić do końca — brakuje tylko jednego pliku w paczce.

## 2.6. Modele, które poznałem odrobinę i nawet spróbowałem, ale nie znalazły się w zestawieniu

Cztery modele uruchomiłem, a mimo to w tabeli ich nie ma. Dwa spełniają wszystkie kryteria z części 2.1, ale wymagają ręcznego budowania szablonu czatu, czego moja aplikacja nie robi. Dwa kolejne przekraczają sufit rozmiaru z części 2.1.7.

Dwa Bieliki w kwantyzacji Q8_0 — bielik-1.5b-v3.0-instruct:Q8_0 i bielik-4.5b-v3.0-instruct:Q8_0. Przy każdych ustawieniach, od 180 do 8000 tokenów, zwracają same znaki końca linii, aż skończy się budżet. Na krótkie pytania odpowiadają poprawnie, a warianty Q4_K_M tego samego wydawcy działają bez zarzutu, więc to nie jest wada modelu, tylko wydania. Trop prowadzi do szablonu czatu: po każdym nagłówku stoi w nim \r\n\r\n zamiast \n\n, którego wymaga format Llamy 3. Ominięcie szablonu i ręczne złożenie promptu przywraca odpowiedzi, ale nie notatki. Model bielik-1.5b-v3.0-instruct:Q8_0 zaczyna wtedy odpowiadać wywołaniem nieistniejącej funkcji w JSON-ie. Za to bielik-4.5b-v3.0-instruct:Q8_0 pisze notatki z przekręconymi nazwami spółek i odpowiada także przy szablonie z błędnymi znakami, więc u niego przyczyna leży gdzie indziej. Moja aplikacja nie składa szablonów czatu ręcznie i nie zamierzam tego robić dla dwóch wydań, które ich wymagają.

Dwa kolejne modele odpadły na kryterium rozmiaru. nemotron-cascade-2:30b i nemotron-3.5-lightning:30b od NVIDII ważą 24,3 i 25,4 GB, czyli więcej niż sufit 22 GB. Przetestowałem je mimo to, ale musiałem w tym celu spreparować specjalne środowisko: zatrzymać całą maszynę Podmana, poczekać, aż Windows odda jej pamięć, i zdjąć jakikolwiek margines bezpieczeństwa. Po załadowaniu modelu zostawało mniej niż gigabajt wolnej pamięci. Był to więc test dla testu, w warunkach, w których nikt nie uruchamia aplikacji na co dzień — i potwierdził, że sufit 22 GB jest postawiony słusznie.

Wyniki były jednak na tyle ciekawe, że nemotron-cascade-2:30b może zainteresować kogoś, kto ma w komputerze więcej pamięci, na przykład 40 GB i więcej. To mieszanka ekspertów — z 128 ekspertów pracuje sześciu, około 3 mld parametrów z 31,6 mld — więc mimo rozmiaru jest szybki: startuje na zimno w 40 sekund, a każdą kolejną notatkę pisze w 12–14 sekund. Przez cztery przebiegi nie pomylił ani jednej liczby, wywoływał narzędzia bezbłędnie i — w przeciwieństwie do mniejszego nemotron-3-nano:4b — nie przekręcał daty zwróconej przez narzędzie. Miał jedną, za to wyrazistą wadę: notatkę o ruchu pisał po angielsku, bo w polskim prompcie podchwytywał angielskie wtrącenie („vs previous check”) z linii danych, które zostało tam po pierwotnej, angielskiej wersji aplikacji. Po poprawie promptu na całkowicie polski pisał po polsku pięć razy na pięć: „LPP (LPP.WA) zmienił kurs o +6,79% do 23900 PLN. Pozostałe spółki z listy nie wykazywały zmian powyżej 2%. Brak informacji o przyczynach ruchu.” Jego bliźniak nemotron-3.5-lightning:30b był równie szybki, ale w każdym przebiegu dopisywał ruch wszystkim spółkom z listy.

Bez tych dwóch modeli NVIDIA nie ma w moim zestawieniu nic powyżej grupy 2 — wszystkie jej duże modele z linii Nemotron 3 ważą po kwantyzacji ponad 22 GB.

### 2.6.1. Dlaczego modele rozumujące byłyby złym pomysłem

Modele, które rozumują przed każdą odpowiedzią i nie pozwalają tego wyłączyć, w tym zadaniu naprawdę nie mają sensu. Po pierwsze, zadanie nie wymaga użycia modelu rozumującego a po drugie, jest bardzo trudno zmusić modele rozumujące do działania w takim środowisku jak moje. Przy nieodpowiednim ustawieniu model rozumujący nie zgłasza błędu — po prostu nie odpowiada i wygląda na zepsuty. Modelom hybrydowym wysyłam `think: false`, jednak modele wyłącznie rozumujące tej flagi nie obsługują i wysyłają wtedy wewnętrzny tok myślenia, czyli *thinking* — zwykle po angielsku — wprost do pola odpowiedzi, zamiast notatki. Żeby doprowadzić je do działania, musiałem zmieniać trzy rzeczy naraz: **przestać wysyłać flagę think**, **zwiększyć limit tokenów** oraz stopniowo zwiększać **okno kontekstowe**. Nawet jeśli udało mi się doprowadzić model rozumujący do działania w moim scenariuszu, jego uruchomienie na zimno trwało nawet powyżej 10 minut.

Najlepiej widać to na przykładzie modelu exaone-deep:32b od LG, który z ciekawości uruchomiłem. Zanim napisze cztery zdania o jednym ruchu kursu, układa po angielsku brudnopis na 1700–3600 tokenów, a na procesorze generuje około dwóch tokenów na sekundę — jedna notatka zajmuje mu od 13 do 30 minut. Wynik jest poprawny: „LPP wzrosła o 6,79% do 23 900 PLN.” Ale ten sam fakt model instrukcyjny podaje w kilkanaście sekund. Rozumowanie przydaje się tam, gdzie trzeba coś przeliczyć albo zaplanować; tu trzeba przepisać. Dlatego w części 2.1.5 dopuszczam wyłącznie modele instrukcyjne i takie, którym rozumowanie da się wyłączyć lub ograniczyć. Uwaga: z tego powodu w badaniu nie uwzględniłem bardzo popularnego modelu, który jest typu tylko rozumującego: Deepseek-R1. 


## 2.7. Modele, które poznałem odrobinę i dodałem wbrew zdrowemu rozsądkowi

Sprawdziłem i postanowiłem dopuścić do testu dwie rodziny modeli, w których dokumentacji nie znajdzie się otwartej deklaracji co do możliwości użycia w języku polskim: 
- Exaone 3.5 - specjalizujący się w językach angielskim i koreańskim model językowy produkcji giganta elektroniki użytkowej LG z Korei Południowej
- Falcon 3 - instytutu badawczego Technology Innovation Institute z Abu Zabi w Zjednoczonych Emiratach Arabskich.

Seria modeli Falcon 3 jest trenowana przede wszystkim na językach angielskim, hiszpańskim, portugalskim oraz francuskim ale w danych treningowych zawiera też wiele innych języków. Na obsłudze języka polskiego nie skupia się, ale ponieważ utrzymuje podstawowe zdolności wielojęzykowe w wersji bazowej, odziedziczone z wcześniejszych wersji, język polski może udać się użyć. Trochę szkoda, że Falcon-H1 (wydany w maju 2025), który używa architektury hybrydowej łączącej równolegle klasyczny mechanizm uwagi (attention) z architekturą Transformera z warstwami Mamby-2 State Space Model (SSM), na Ollamie oficjalnie dostępny nie jest. 

Podobnie jest z Exaone i LG. LG AI Research dodało język polski jako jeden z czterech nowowspieranych języków w gigantycznym modelu K-EXAONE 2.0 z 750 miliardami parametrów, wydanym w lipcu 2026, który jednak nijak nie udałoby się uruchomić na komputerze z 32 GB pamięci RAM. Natomiast dostępne na Ollamie i będące w zasięgu możliwości mojego komputera modele exaone3.5:2.4b, exaone3.5:7.8b i exaone3.5:32b, deklarują obsługę wyłącznie koreańskiego i angielskiego i próba uruchomienia ich w zetknięciu z językiem polskim właściwie z definicji skazana jest na niepowodzenie. Postanowiłem jednak użyć tych modeli właśnie dlatego, aby porównać moje wyniki z badania modeli obsługujących polski lub zawierających wsparcie dla polskiego pochodzące z wersji wcześniejszych z wynikami modeli, które na pewno i absolutnie niczego takiego nigdy nie oferowały i mieć w ten sposób punkt odniesienia.

## 2.8. Na koniec zupełna nowość i hit 2026 - model Laguna XS (2.1)

Krajobraz modeli LLM zmienia się z miesiąca na miesiąc i ciągle pojawiają się jakieś nowości. W 2026 roku popularność i rozgłos zyskał nowy model językowy Laguna XS, wyprodukowany przez firmę Poolside, której współzałożycielem jest były CTO GitHuba Jason Warner. Model koncentruje się na inżynierii oprogramowania, generowaniu kodu oraz procesach wykorzystujących narzędzia (tool-using), jednak nie na czacie ogólnego przeznaczenia. 
Model jest bardzo wydajny. Zawiera 33 miliardy parametrów ogółem, z czego 3 miliardy są aktywne dzięki architekturze Mixture-of-Experts. Modele Laguna XS zostały zaprojektowane do uruchamiania lokalnego na sprzęcie konsumenckim lub deweloperskim – takim jak procesory Apple Silicon czy integracje z Ollama. To czyni je atrakcyjnymi dla entuzjastów lokalnych modeli LLM i przykuły również moją uwagę. 
Dyskusje na platformach takich jak Hacker News oraz na forach poświęconych lokalnej sztucznej inteligencji wykazują duże zainteresowanie modelem ze względu na jego liberalną licencję OpenMDW-1.1, opcje kwantyzacji (np. pliki GGUF) oraz integrację z systemem agentowym pool od Poolside. Użytkownicy zauważają jednak, że poza kodowaniem jego użyteczność jest znacznie węższa w porównaniu do modeli ogólnego przeznaczenia. Czy tak się okaże w moim badaniu? Przekonam się. 

Profile wydawców modeli ujętych w badaniu znajdziesz [tutaj](./AI%20pod%20strzechy%2007%20Profile%20wydawcow%20modeli.md).

Zapraszam
