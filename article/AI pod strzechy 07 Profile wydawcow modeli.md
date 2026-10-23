# Część 7. Przypis — kto właściwie wydaje te modele

Czterdzieści cztery modele w badaniu pochodzą od dwunastu wydawców. Ten przypis opisuje każdego z nich z punktu widzenia kogoś, kto chce model po prostu pobrać i uruchomić u siebie: skąd pochodzą wagi, na jakiej licencji, czy wydawca ma coś małego i co z tego wyszło w moim zadaniu. Wyniki pochodzą z części 4, licencje omawiam szczegółowo w części 2.

| Wydawca | Modeli | Komplet 11 scenariuszy (konsumowalnych) | Polszczyzna bez wad | Komplet w teście narzędzi | Licencja |
|---|---:|---:|---:|---:|---|
| IBM | 11 | **2** | 0 | **11** | Apache 2.0 |
| Alibaba | 7 | 1 | 2 | 6 | Apache 2.0 |
| Google | 5 | **2** | **3** | **5** | Apache 2.0 |
| Mistral AI | 4 | 1 | 0 | 1 | Apache 2.0 |
| TII | 4 | 0 | 0 | 0 | TII Falcon (warunkowo) |
| Meta | 3 | 0 | 0 | 2 | Llama Community (warunkowo) |
| LG AI Research | 3 | 0 | 0 | 0 | EXAONE 1.1 – NC (niekomercyjna) |
| Microsoft | 2 | 0 | 0 | 0 | MIT |
| SpeakLeash / ACK Cyfronet AGH | 2 | 0 | 0 | 1 | Apache 2.0\* |
| NVIDIA | 1 | 0 | 0 | 1 | NVIDIA Open Model |
| OpenAI | 1 | 0 | 0 | 1 | Apache 2.0 |
| Poolside | 1 | 0 | 0 | 0 | OpenMDW-1.1 |
| **Razem** | **44** | **6** | **5** | **28** | |

\* Brak licencji w samym pliku modelu; readme na koncie wydawcy wskazuje Apache 2.0 wraz z regulaminem na bielik.ai/terms.

Najmocniej wypadają dwaj wydawcy: **Google** — trzy z pięciu modeli piszących po polsku bez wad, dwaj zwycięzcy grup i komplet w teście narzędzi u wszystkich pięciu Gemm — oraz **IBM** — zwycięzca grupy drugiej, czwarty model grupy czwartej i komplet w teście narzędzi u wszystkich jedenastu Granite. Trzeci jest **Alibaba**, który dzięki najnowszemu `qwen3.8:27b` ma drugi model grupy czwartej.

---

## 7.1. IBM — Granite

**W badaniu:** `granite4:350m-h`, `granite4:1b-h`, `granite4:micro-h`, `granite4:7b-a1b-h`, `granite4.1:3b`, `granite4.1:8b`, `granite4.2:3b`, `granite4.2:8b`, a na hoście `granite4.1:30b`, `granite4.2:30b` i `granite4:32b-a9b-h`.

Najlepiej udokumentowany wydawca w całej stawce i jedyny, który prowadzi w Ollamie własną przestrzeń nazw — `ibm/granite4`. Licencja Apache 2.0 siedzi w samym pliku modelu, więc `ollama show --license` ją zwraca; przy większości pozostałych trzeba szukać na stronie. Najliczniejsza rodzina w badaniu: jedenaście modeli we wszystkich grupach poza trzecią.

**Rzecz, która wyszła dopiero z testów:** Granite 4 to jedyna rodzina w stawce z architekturą hybrydową. Sufiks `-h` oznacza Mamba-2 połączoną z transformerem, a notacja `a1b`/`a9b` mówi, ile parametrów naprawdę pracuje na token. `granite4:7b-a1b-h` ma 6,9 mld parametrów, ale liczy około miliarda — i stąd jego prędkość.

**I rzecz, która mnie zaskoczyła:** kolejne wydania **porzuciły tę architekturę**. Granite 4.1 (kwiecień 2026) i 4.2 (sierpień 2026) to modele wyłącznie gęste, bez wariantów `-h`. W rozmiarze ośmiu miliardów wygląda to tak:

| | `granite4:7b-a1b-h` | `granite4.1:8b` | `granite4.2:8b` |
|---|---|---|---|
| Wydanie | wrzesień 2025 | kwiecień 2026 | sierpień 2026 |
| Architektura | hybrydowa, ~1 mld aktywnych | gęsta, 8,8 mld | gęsta, 8,8 mld |
| Zimny start | **10,2 s** | 37,9 s | 26,8 s |
| Kontekst | 1 048 576 | 131 072 | 131 072 |
| Scenariusze konsumowalne | 9 z 11 | **11 z 11** | 9 z 11 |

Hybryda jest najszybsza, ale procentu nie zaokrągla, tylko przepisuje go z piętnastoma miejscami po przecinku, a przy czterech spółkach naraz gubi jedną z nich. Gęsty Granite 4.1 jest najwolniejszy, ale jako jedyny w grupie drugiej przechodzi wszystkie jedenaście scenariuszy — i wygrywa grupę. Najnowszy Granite 4.2 jest z kolei **gorszy od swojego poprzednika w tym samym rozmiarze**: w teście z rozdziału trzeciego pisze „Modivo zmieniła kurs o co najmniej 2%”, nie podając, o ile. To samo powtarza się na górze: `granite4.2:30b` przegrywa z `granite4.1:30b` czasem (110 s wobec 68 s), a w zestawie drugim obcina procent do „5,33%”. Numer wersji sugeruje postęp; pomiar pokazuje regres — dla tego konkretnego zastosowania.

**Trzeci szczegół, niewidoczny z karty modelu:** Granite 4.2 ma trzy tryby rozumowania, a domyślnym jest pełny. Bez jawnego `think: false` model wysyła angielski tok myślenia wprost do pola odpowiedzi — w testach wstępnych 1082 znaki rozważań zamiast dwóch zdań po polsku. Aplikacja wysyła `think: false` każdemu modelowi, więc w badaniu tego nie widać, ale kto podmieni model w innym narzędziu, ten to zobaczy.

**Narzędzia:** wszystkie jedenaście Granite przeszły test narzędzi w komplecie, od `granite4:350m-h` za 366 MB, który domyka pętlę w 1,3 sekundy. Obok Gemmy 4 to jedyna rodzina w stawce, w której komplet mają wszystkie modele. Jedna rysa: zwycięzca grupy drugiej, `granite4.1:8b`, przepisuje kurs z wyniku narzędzia poprawnie, ale datę notowania za każdym razem wymyśla („z 5 dni temu (2025-08-29)”).

---

## 7.2. Google — Gemma

**W badaniu:** `gemma4:e2b-it-qat`, `gemma4:e4b`, `gemma4:12b`, a na hoście `gemma4:26b` i `gemma4:31b` — cała rodzina Gemma 4, wydana 2 kwietnia 2026.

To rodzina, w której **między wersjami zmieniło się wszystko, co dla mnie istotne** — i to w dobrą stronę, co w tym zestawieniu nie jest regułą.

`gemma3` widziała obrazy, ale **nie deklarowała wywoływania narzędzi w ogóle**. Dyskwalifikowało ją to jako kandydata na silnik agenta, niezależnie od tego, jak dobrze pisała po polsku. `gemma4` deklaruje `tools`, `thinking`, obraz, a w mniejszych wariantach **i dźwięk** — jako jedyna w całej stawce obsługuje wejście audio.

Zmieniła się też licencja, i to jest ważniejsze niż cała reszta:

| | `gemma3` | `gemma4` |
|---|---|---|
| Licencja w pliku modelu | **Gemma Terms of Use** (8432 zn) | **Apache 2.0** (10174 zn) |
| Klauzula Prohibited Use | jest | **brak** |
| Słowo „Gemma” w treści licencji | jest | **nie występuje** |

Sprawdziłem to przez `/api/show` na obu rodzinach i potwierdziłem u źródła: **2 kwietnia 2026 Google wydał Gemmę 4 na Apache 2.0 — jako pierwszą Gemmę na licencji zatwierdzonej przez OSI.** Wcześniejsze wersje szły na własnych warunkach Google'a, z zakazami użycia, które trzeba było przeczytać samemu i samemu ocenić, czy się w nie nie wchodzi.

**W badaniu Gemma 4 jest najmocniejszą rodziną.** Trzy z pięciu modeli, które piszą po polsku bez powtarzalnej wady, to Gemmy: `gemma4:12b`, `gemma4:26b` i `gemma4:31b`. Dwie z nich wygrywają swoje grupy — `gemma4:12b` trzecią, `gemma4:26b` czwartą — i obok `qwen3.8:27b` tylko te dwa modele w całym badaniu mają jednocześnie komplet scenariuszy, polszczyznę bez wad i komplet w teście narzędzi. `gemma4:26b` jest przy tym modelem MoE z około 4 z 25 mld parametrów aktywnych, więc szybszym od mniejszej `gemma4:12b`. Wszystkie pięć Gemm przeszło test narzędzi w komplecie.

Najmniejsza `gemma4:e2b-it-qat` (4,3 GB) jest w grupie drugiej tuż za zwycięzcą: przy czterech spółkach naraz jako najszybsza podaje wszystkie procenty poprawnie, ale w jednym cyklu zapisuje kurs Polsatu 15,965 zł jako „15965.00 PLN”. Nazwy wprowadzają w błąd co do rozmiaru: `e2b` nie znaczy 2 mld parametrów, tylko *effective 2B* (część 2).

Dla mnie praktycznie znaczy to tyle, że rodzina, która wyszła u mnie na pierwsze miejsce, przestała być jednocześnie tą jedyną w czołówce z gwiazdką przy licencji.

---

## 7.3. Alibaba — Qwen

**W badaniu:** `qwen3.5:0.8b`, `qwen3.5:2b`, `qwen3.5:4b`, `qwen3.5:9b`, `qwen3:14b`, a na hoście `qwen3.6:27b` i `qwen3.8:27b`.

Rodzina najczęściej odświeżana z całej stawki: `qwen3.5` wyszła pod koniec lutego 2026, `qwen3.6` w kwietniu, a po niej `qwen3.8`. Skala też jest największa — od 0,8 mld do ponad 100 mld parametrów — i Alibaba jako jedyny wydawca jest obecny we wszystkich czterech grupach.

**Ale to nie znaczy, że świeżość dociera na dół.** Najmniejszy wariant `qwen3.6` waży 17,8 GB, a `qwen3.8` zaczyna się od 18 GB — obie linie są dla maszyny z 16 GB nieosiągalne. Wszystko, co z tej rodziny mieści się w kontenerze, pochodzi z linii `qwen3.5` albo starszej. Na maszynie z 32 GB mieszczą się obie i obie są w badaniu — celowo obok siebie, bo to ten sam model bazowy (rodzina `qwen35`, 27,3 mld parametrów) w dwóch kolejnych wydaniach.

**`qwen3.8:27b` to najlepszy w badaniu przykład, że aktualizacja modelu ma sens.** `qwen3.6:27b` pisze po polsku bez wad, ale przy czterech spółkach naraz raz podaje same kursy, bez procentów. Jego następca ten błąd naprawia: przechodzi wszystkie jedenaście scenariuszy, pisze bez wad, ma komplet w teście narzędzi, startuje szybciej (61 s wobec 73,5 s) i domyka pętlę w 51 s zamiast 125 s — i staje na drugim miejscu grupy czwartej, za `gemma4:26b`. Wyniki reszty rodziny rozkładają się nierówno. `qwen3.5:4b` i `qwen3.5:9b` przechodzą po dziesięć scenariuszy z jedenastu — obaj potykają się na czterech spółkach, podając zmiany w złotych zamiast procentów. Najmniejsze `qwen3.5:0.8b` i `qwen3.5:2b` zmyślają procenty spółek, które się nie ruszyły. `qwen3:14b` wywołuje narzędzia bezbłędnie, ale po otrzymaniu wyniku zwraca pustą odpowiedź — jedyny taki przypadek w badaniu.

Wszystkie Qweny w stawce są na Apache 2.0. Nie zawsze tak było — o licencji Qwen Research piszę w podrozdziale 7.13.

---

## 7.4. Mistral AI — Ministral i Mistral Small

**W badaniu:** `ministral-3:3b`, `ministral-3:8b`, `ministral-3:14b`, a na hoście `mistral-small3.2`.

Licencja **Apache 2.0** bez zastrzeżeń. Mistral to jeden z dwóch europejskich wydawców w stawce, obok polskiego SpeakLeash, więc sprawdziłem osobno, czy to widać w polskim.

**Widać, ale nie tam, gdzie się spodziewałem.** Najczystszą polszczyznę w grupie drugiej daje `ministral-3:3b` — krótkie, poprawne zdania — ale procenty skraca do jednego miejsca po przecinku („−5,3%”), więc w trzech scenariuszach jego notatki nie są konsumowalne. `ministral-3:14b` przechodzi wszystkie jedenaście scenariuszy i jest najwyżej sklasyfikowanym modelem europejskim — trzeci w grupie trzeciej, za `gemma4:12b` i `phi4`, przegrywa polszczyzną: wbrew zakazowi wypisuje kursy pozostałych spółek. Na górze `mistral-small3.2` (15 GB) popełnia błąd przesuniętego przecinka: „LPP 244,00 PLN” zamiast 24 400 zł, w sześciu scenariuszach.

**Narzędzia to słabość tej rodziny.** Linia `ministral-3` deklaruje `tools`, ale żaden z trzech modeli nie ma kompletu: `ministral-3:14b` zapisuje wywołanie jako tekst (`kurs_historyczny[ARGS]{…}`), zamiast je wykonać, a oba mniejsze w pętli podają kurs 24 160 zł jako „241,60 PLN” i wymyślają datę „19 października 2026”. Komplet ma tylko `mistral-small3.2`.

Europejskie pochodzenie wydawcy nie przekłada się więc wprost na polski. Przekłada się rozmiar modelu i to, ile pracy wydawca włożył w dostrojenie do poleceń poza angielskim — a także staranność w liczbach, której ta rodzina miała mniej niż Gemma.

---

## 7.5. Meta — Llama

**W badaniu:** `llama3.2:1b`, `llama3.2:3b`, `llama3.1:8b`.

Najszerzej używana rodzina otwartych wag i zarazem ta, która **od dawna nie wydała nic małego**. Najnowsze to `llama3.3` (70B) i `llama4` (16×17B) — oba poza zasięgiem maszyny z 16 GB. Dla tego tieru Llama zatrzymała się na wersji 3.2 z września 2024 — to najstarsze modele w badaniu.

Licencja **Llama Community** dopuszcza użycie komercyjne warunkowo, z progiem liczby użytkowników i wymogiem oznaczania produktów pochodnych. To licencja własna Mety, nie standard branżowy.

Po polsku żadna Llama nie przechodzi testu z rozdziału trzeciego: `llama3.1:8b` gubi jedyny ruch dnia („Brak znaczących zmian”), `llama3.2:3b` dopisuje zmyśloną kwotę, a `llama3.2:1b` zamiast notatki przepisuje dane. Po angielsku obraz się odwraca — `llama3.2:3b` pisze bez wad i ma najszybciej domkniętą pętlę narzędziową w grupie drugiej (4,8 s). To najczytelniejszy w badaniu przykład modelu dobrego, tylko nie po polsku.

---

## 7.6. Microsoft — Phi

**W badaniu:** `phi4-mini`, `phi4`.

Jedyna rodzina w zestawieniu z licencją **MIT** — najbardziej liberalną z możliwych, bez warunków i bez progów. I jedna z tych, które **nie mają nowszej wersji**: `phi4` pochodzi z grudnia 2024, a po nim przyszły tylko warianty wyłącznie rozumujące, `phi4-reasoning` (14B) i `phi4-mini-reasoning` (3,8B), które do badania nie weszły (kryterium 2.1.5). Nowej generacji, Phi 5, w Ollamie nie ma.

`phi4` (9 GB) przechodzi dziewięć scenariuszy z jedenastu: wypisuje kursy wszystkich spółek wbrew zakazowi i w trzech notatkach wyczerpuje przy tym limit długości, a notatka urywa się w pół słowa. **Agentem jednak nie będzie:** manifest nie deklaruje narzędzi i Ollama odrzuca zapytanie od razu. `phi4-mini` (2,5 GB) jest odwrotnością: narzędzia deklaruje, ale zamiast je wywołać, opisuje wywołanie słowami — zero poprawnych prób na sześć. Po polsku odwraca kierunek ruchu („zmniejszając się do 97 PLN”).

---

## 7.7. SpeakLeash i ACK Cyfronet AGH — Bielik

**W badaniu:** `SpeakLeash/bielik-minitron-7B-v3.0-instruct:Q4_K_M`, `SpeakLeash/bielik-11b-v3.0-instruct:Q4_K_M`.

Polski wyjątek, który wypada przyzwoicie, ale nie zwycięsko. Modele stoją na **koncie wydawcy**, które prowadzi dziewięć repozytoriów całej rodziny. To kanał zespołu, nie konwersja osoby postronnej.

Licencji nie ma wprawdzie w samym pliku, ale readme na koncie wskazuje Apache 2.0 wraz z regulaminem na `bielik.ai/terms` i linkuje do oryginału na HuggingFace. Pochodzenie da się prześledzić do końca.

`bielik-11b` przechodzi dziewięć scenariuszy z jedenastu i ma komplet w teście narzędzi. Dwa cykle bez ruchu traci przez kursy Polsatu zapisane jako „16.105.00 PLN”, a do tego pisze kropkę dziesiętną, wypisuje kursy wbrew zakazowi i zaczyna notatkę od niegramatycznego „Brak spółek zmieniło kurs”. `bielik-minitron-7B` obcina procent do „2,29%” i w scenariuszach ze spadkami gubi minus. Polszczyzna sama w sobie ich nie zawiodła — zawiodło wykonanie polecenia: zapis liczb, zakaz wymieniania kursów i znak liczby.

Dwa mniejsze Bieliki, 1.5B i 4.5B, są dostępne w Ollamie wyłącznie w kwantyzacji Q8_0 i do badania nie weszły: zwracają same znaki końca linii, dopóki nie ominie się dołączonego szablonu czatu, a moja aplikacja z szablonu korzysta. Nie deklarują też narzędzi.

---

## 7.8. OpenAI — gpt-oss

**Na hoście:** `gpt-oss:20b` (13,8 GB). Był też tag `:latest` wskazujący na te same wagi — usunąłem go jako duplikat.

Jedyne otwarte wagi OpenAI, wydane w sierpniu 2025 i od tego czasu bez następcy w tym rozmiarze; to, co widać na liście jako świeższe, to społecznościowe warianty tych samych wag z podmienionym promptem systemowym. Licencja Apache 2.0.

To jedyny model w badaniu, który **nie działa na ustawieniach domyślnych aplikacji**. Nie da się w nim wyłączyć rozumowania — `think: false` jest dla niego nieznaną wartością i Ollama podstawia domyślny poziom „medium”, przy którym model zużywa cały budżet tokenów na rozważania i nie pisze notatki. Wystarczy jednak jeden wpis w konfiguracji, `think: "low"`, i z nim `gpt-oss:20b` ma najkrótszy zimny start w grupie czwartej (24,4 s), komplet w teście narzędzi i dziesięć scenariuszy na jedenaście — przy czterech spółkach naraz raz zwraca pustą notatkę.

---

## 7.9. NVIDIA — Nemotron

**W badaniu:** `nemotron-3-nano:4b` (2,8 GB).

Jedyny model NVIDII w zasięgu domowej maszyny — większe Nemotrony przekraczają 22 GB. Architektura hybrydowa, podobnie jak u Granite 4, z oknem 262 144 tokenów. Licencja **NVIDIA Open Model** mimo długości tekstu stwierdza wprost: *„Models are commercially usable.”* — w odróżnieniu od starszych wydań tego samego wydawcy, jak `nemotron-mini`, które szły na licencji ograniczonej do użytku nieprodukcyjnego.

W zadaniu po polsku wypada słabo: zmyśla ruchy innych spółek i przepisuje do notatki fragment polecenia, więc żaden scenariusz nie jest konsumowalny. Po angielsku przechodzi oba testy, a test narzędzi ma w komplecie.

---

## 7.10. TII — Falcon

**W badaniu:** `falcon3:1b`, `falcon3:3b`, `falcon3:7b`, `falcon3:10b`.

Rodzina z Abu Zabi pokrywa grupy od pierwszej do trzeciej i zamyka w stawce lukę między 6 a 14 GB, w której IBM nie ma nic. Licencja **TII Falcon** jest oparta w części na Apache 2.0, ale z dołożoną polityką dopuszczalnego użycia, którą wydawca publikuje na swojej stronie i może zmieniać — stąd „warunkowo”.

Po polsku żaden Falcon nie przechodzi testu z rozdziału trzeciego: najmniejszy pisze bełkot i przepisuje prompt, większe zmyślają ruchy spółek albo zaokrąglają kursy do całości („Grupa Polsat 16100 PLN”). Po angielsku `falcon3:3b` i `falcon3:10b` przechodzą test z rozdziału trzeciego. **Narzędzi nie ma żaden** — manifest ich nie deklaruje i Ollama odrzuca zapytanie od razu.

---

## 7.11. LG AI Research — EXAONE

**W badaniu:** `exaone3.5:2.4b`, `exaone3.5:7.8b`, a na hoście `exaone3.5:32b`.

Modele, które **deklarują wyłącznie koreański i angielski** — wziąłem je świadomie jako punkt odniesienia: co robi z polskim promptem model, który polskiego nigdy nie obiecywał (część 2). Licencja **EXAONE 1.1 – NC** wyklucza użycie komercyjne. Narzędzi nie deklaruje żaden.

Wynik jest zgodny z deklaracją: po polsku żaden EXAONE nie przechodzi testu z rozdziału trzeciego — obcinają procent do „2,29%”, zmyślają ruchy, a największy pisze „wzrosła o 2,30 PLN” zamiast procentu. Ciekawsze jest co innego: `exaone3.5:32b` po angielsku przechodzi oba testy, a mimo to po polsku osiem scenariuszy na jedenaście ma konsumowalnych — więcej niż kilka modeli, które polski deklarują. Zaskakująco mało dzieli więc model, który polskiego nie zna, od modelu, który go zna słabo.

`exaone3.5:32b` jest też najbardziej pamięciożernym modelem badania: po załadowaniu zajmuje prawie 22 GB.

---

## 7.12. Poolside — Laguna

**Na hoście:** `laguna-xs-2.1:q4_K_M` (20,3 GB).

Najmłodszy wydawca w stawce i jedyny, który nie celuje w czat ogólnego przeznaczenia: Poolside buduje modele dla agentów programistycznych (część 2.8). Laguna XS 2.1 to model MoE z 33,4 mld parametrów, z których na token pracuje około 3 mld (8 z 256 ekspertów), z oknem 262 144 tokenów i wyłączalnym rozumowaniem. Licencja **OpenMDW-1.1**, w samym pliku modelu — permisywna, z wprost dozwolonym użyciem komercyjnym. Poprzednie wydanie, Laguna XS.2, szło jeszcze na Apache 2.0.

**Liczby podaje bezbłędnie, polszczyznę ma słabą.** W miarach przechodzi wszystkie jedenaście scenariuszy i jest po `gemma4:26b` najszybszym modelem grupy czwartej: 38,7 s zimnego startu, 15,7 s ciepłej notatki. Pisze jednak rozwlekle i w czterech notatkach wyczerpuje limit stu osiemdziesięciu tokenów — urwana notatka nie jest konsumowalna, więc z notatkami konsumowalnymi przechodzi siedem scenariuszy — dopisuje rzeczy spoza danych („Dane z dnia dzisiejszego”, „Dane giełdowe są aktualne na dzień dzisiejszy”), twierdzi, że kursy „pozostają bez zmian”, choć zmieniły się poniżej progu, i robi błędy gramatyczne („z kursowym ruchem się większym niż 2%”, „z zmianą”). W rankingu grupy czwartej jest przez to dopiero dziewiąta, tuż przed `exaone3.5:32b` i `mistral-small3.2`.

**Model dla agentów oblewa pętlę narzędziową.** Wywołania ma prawie w komplecie (5 z 6), ale w pętli za każdym razem przekazuje kurs 24 160 zł jako „24,16 PLN” — błąd rzędu wielkości, który stara kontrola liczby, porównująca same cyfry, by przepuściła.

---

## 7.13. Czego nie ma w badaniu i dlaczego

Część modeli wydawców z tej listy pobrałem i sprawdziłem na etapie doboru, ale do ostatecznej stawki nie weszły. Kryteria opisuje część 2; tu zostawiam to, co przy okazji wyszło o samych wydawcach.

**DeepSeek.** Wszystkie trzy warianty `deepseek-r1` (7B, 14B, 32B, licencja MIT) to modele wyłącznie rozumujące: każdy przebieg zaczynał się od `<think>`, którego model nie zamykał przed limitem tokenów — 680–900 znaków angielskiego rozumowania i ani jednego zdania po polsku. Wyłączyłem je z badania zgodnie z kryterium 2.1.5. **Najważniejsze ustalenie dotyczy jednak ich następców.** `deepseek-v4-flash` i `deepseek-v4.1-flash` figurują w bibliotece jako najnowsze, ale mają wyłącznie tagi `:cloud`. Próba pobrania kończy się błędem:

```
Error: pull model manifest: file does not exist
```

**Najnowszy DeepSeek nie działa lokalnie.** Jest dostępny jako usługa w chmurze Ollamy, czyli w modelu, od którego cała ta praca miała uciec. Dla kogoś, kto trzyma dane na własnym dysku, ta rodzina zatrzymała się na wersji R1.

**Qwen — rozumujące i starsze.** `qwen3:4b` i `qwen3:30b` zignorowały `think: false` — w dziesięciu przebiegach na dziesięć wysypały surowe rozumowanie po angielsku („Okay, let's tackle this query…”), i to bez znaczników `<think>`, więc sanitizer nie miał czego uciąć. Ten sam przełącznik działał przy `qwen3:8b` i `qwen3:14b` — niespójność szablonów w obrębie jednej rodziny, którą trzeba sprawdzać tag po tagu. `qwen2.5:3b` i `qwen3:8b` zastąpiłem młodszymi modelami tego samego wydawcy w tym samym rozmiarze (`qwen3.5`).

**Gemma 3 i starsze Mistrale.** `gemma3:4b` i `gemma3:12b`, a także `mistral:latest` i `mistral-nemo`, zastąpiłem młodszymi modelami tych samych wydawców — Gemmą 4 i linią `ministral-3`.

**PLLuM — polski model bez oficjalnego kanału.** Jedyny model, przy którym **nie umiem ustalić licencji**. Sprawa nie leży po stronie konsorcjum: na HuggingFace PLLuM jest opublikowany wzorowo, warianty badawcze mają w nazwie człon `nc` i licencję CC-BY-NC-4.0, pozostałe są na Apache 2.0. Problem powstaje w kanale dystrybucji. **Konsorcjum nie prowadzi konta w Ollamie.** Jest tam pięć repozytoriów od czterech osób prywatnych, z czego cztery nie mają readme ani licencji w pliku, a piąte, najpopularniejsze, **pod jedną nazwą trzyma zarówno wariant otwarty, jak i wprost niekomercyjny**, a domyślnie pobierany tag nie mówi, który to. Do tego jedyna konwersja z udokumentowanym pochodzeniem waży 13 GB. **Wersja, która się mieści, jest zarazem jedyną bez ustalonego pochodzenia** — i dlatego do badania nie weszła. To nie jest zarzut wobec PLLuM, tylko pokazanie, ile kosztuje brak oficjalnego kanału dystrybucji.

Przy okazji zapytałem siedem modeli z ówczesnej stawki, czym jest PLLuM. Sześć zmyśliło: ligę lekarską, system łączności radiowej, model od Google DeepMind, listę lekarzy umysłowych, akronim od „Programowanie Logika i Języki Maszynowe”. `gemma3:12b` odpowiedziała, że to *„model językowy opracowany przez Google DeepMind, otwarty i dostępny do użytku komercyjnego”* — najgroźniejsza odpowiedź, bo brzmi prawdopodobnie, myli autora i dopowiada zdanie o licencji, czyli dokładnie to, czego ustalenie zajęło mi pół dnia. `granite4.2:3b` odpowiedział, że to *„nie jest znane w języku polskim pojęcie; prawdopodobnie chodzi o błąd lub literówkę”*. A `mistral-small:24b`:

> **„Nie mam informacji na temat PLLuM.”**

**Jedyny model, który odpowiedział poprawnie, to ten, który odmówił odpowiedzi.**

---

## 7.14. Tabliczka znamionowa per wydawca

| Wydawca | Licencja | Komercyjnie | Kanał | Najmłodszy model w badaniu | Coś małego? | Narzędzia — deklaracja i test |
|---|---|---|---|---|---|---|
| **IBM** | Apache 2.0 | tak | konto `ibm/` i biblioteka | granite4.2 (sierpień 2026) | tak, od 366 MB | deklaruje cała rodzina; **komplet 11 z 11** |
| **Google** | Apache 2.0 (`gemma4`) | tak | biblioteka | gemma4:12b (maj 2026) | tak, od 4,3 GB | deklaruje cała rodzina; **komplet 5 z 5** |
| **Alibaba** | Apache 2.0 | tak | biblioteka | qwen3.8 (sierpień 2026) | tak, od 1,0 GB — ale tylko w linii 3.5 | deklaruje cała rodzina; komplet 6 z 7 |
| **Mistral AI** | Apache 2.0 | tak | biblioteka | ministral-3 (październik 2025) | tak, od 3 GB | deklaruje cała rodzina; komplet 1 z 4 |
| **Meta** | Llama Community | warunkowo | biblioteka | llama3.2 (wrzesień 2024) | **nic nowszego od 3.2** | deklaruje cała rodzina; komplet 2 z 3 |
| **Microsoft** | MIT | tak | biblioteka | phi4-mini (luty 2025) | brak nowszego | `phi4-mini` deklaruje, ale 0 z 6; **`phi4` nie deklaruje** |
| **SpeakLeash** | Apache 2.0\* | tak | konto wydawcy | bielik-minitron-7B (styczeń 2026) | 1.5B i 4.5B tylko w Q8_0 | deklarują 7B i 11B; komplet 1 z 2 |
| **NVIDIA** | NVIDIA Open Model | tak | biblioteka | nemotron-3-nano (marzec 2026) | tylko 4B, reszta ponad 22 GB | deklaruje; komplet |
| **OpenAI** | Apache 2.0 | tak | biblioteka | gpt-oss (sierpień 2025) | nie, min. 13,8 GB | deklaruje; komplet |
| **Poolside** | OpenMDW-1.1 | tak | biblioteka | laguna-xs-2.1 (czerwiec 2026) | nie, min. 20 GB | deklaruje; wywołania 5 z 6, pętla 0 z 3 |
| **TII** | TII Falcon | warunkowo | biblioteka | falcon3 (grudzień 2024) | tak, od 1,8 GB | **nie deklaruje żaden** |
| **LG AI Research** | EXAONE 1.1 – NC | **nie** | biblioteka | exaone3.5 (grudzień 2024) | tak, od 1,6 GB | **nie deklaruje żaden** |
| *DeepSeek* | *MIT* | *tak* | *biblioteka* | *poza badaniem* | ***najnowszy tylko w chmurze*** | — |
| *Konsorcjum PLLuM* | ***nieustalona***\*\* | ***nie wiadomo*** | *brak konta* | *poza badaniem* | — | — |

\* Brak licencji w pliku modelu; readme na koncie wydawcy wskazuje Apache 2.0 i regulamin na bielik.ai/terms.

\*\* Konsorcjum publikuje czytelnie na HuggingFace; w Ollamie istnieją wyłącznie konwersje osób postronnych, a ta mieszcząca się w tierze nie deklaruje ani licencji, ani źródła.

Daty to daty utworzenia repozytorium wag na koncie wydawcy w HuggingFace, jak w części 2.

### Co z kolumny narzędzi wynika dla agenta

Deklaracja z manifestu (`/api/show`, pole `capabilities`) mówi tylko, że model ma szablon czatu z miejscem na definicje narzędzi, nie że sensownie z nich korzysta. Dlatego test narzędzi z rozdziału 4.6 objął wszystkie czterdzieści cztery modele. Układa się to w trzy rzeczy:

**Narzędzia przestały być cechą dużych modeli.** Granite ma komplet już w wariancie `350m-h`, który waży **366 MB**, Qwen przy 1,0 GB, Llama przy 2 GB. Próg wejścia do budowania agenta zszedł poniżej pół gigabajta.

**Deklaracja to nie dowód.** Trzy modele narzędzia deklarują, a nie potrafią ich użyć: `phi4-mini` opisuje wywołanie słowami, `ministral-3:14b` zapisuje je jako tekst, a `qwen3:14b` po otrzymaniu wyniku milknie. Tego nie wykryje nic poza uruchomieniem testu.

**Bywa odwrotnie, niż podpowiada intuicja.** `phi4-mini` (2,5 GB) narzędzia deklaruje, a jego ponad trzy i pół raza większy brat `phi4` (9,1 GB) **nie deklaruje ich wcale** — choć notatki pisze lepiej od niego. U Bielika `bielik-minitron-7B` waży 4,5 GB i narzędzia ma, a mniejszy nominalnie `bielik-4.5b` waży 5,1 GB i **nie ma ich wcale**. Z samej nazwy ani z samego rozmiaru nie da się tego zgadnąć — trzeba zapytać `/api/show`, a potem i tak sprawdzić.

### Co z kolumny „Komercyjnie” wynika dla tego, co będzie za rok

To jedyny wymiar doboru, którego **nie widać w żadnym pomiarze**. Model z zakazem komercyjnym chodzi dokładnie tak samo szybko i pisze dokładnie tak samo po polsku jak model na Apache 2.0. Ollama pobierze i uruchomi jeden i drugi bez słowa ostrzeżenia. Różnica ujawnia się dopiero wtedy, gdy ktoś chce na tym zarobić — czyli zwykle długo po tym, jak decyzja o wyborze modelu zapadła i obrosła kodem.

Rozkładam to na cztery stany, bo każdy wymaga czego innego. Zastrzegam od razu: czytam pliki licencji i strony wydawców, nie jestem prawnikiem i to nie jest porada prawna.

**„Nie” — tu stawiam sprzeciw, ale precyzyjnie.** Sprzeciw należy się wtedy, gdy ktoś **dostaje rozwiązanie komercyjne** zbudowane na modelu, który komercyjny nie jest. To wada dostarczonego produktu, nie drobiazg do uporządkowania później. Natomiast samo uruchomienie takiego modelu u siebie wadą nie jest i nie ma sensu straszyć na zapas. W tej stawce dotyczy to trzech modeli EXAONE.

Najlepszym przykładem pułapki jest jednak model, którego w stawce już nie ma: `qwen2.5:3b` na licencji **Qwen Research**, gdzie „non-commercial” jest zdefiniowane jako *„for research or evaluation purposes only”*:

> *You are granted a non-exclusive, worldwide, non-transferable and royalty-free limited license […] to use, reproduce, distribute, copy, create derivative works of, and make modifications to the Materials **for non-commercial purposes only**. If you are commercially using the Materials, you shall request a license from us.*

To nie jest zakaz uruchamiania modelu — to zakaz zarabiania na nim bez osobnej umowy. Ewaluacja mieści się w tym bez naciągania, więc opisuję to nie jako naruszenie, tylko jako **pułapkę czasową**: model jest dziś w porządku, a przestaje nim być dokładnie w dniu, w którym rozwiązanie miałoby zacząć zarabiać. **Dowiedziałem się o tym ograniczeniu dopiero przy tym badaniu**, choć model chodził u mnie od miesięcy — Ollama pobrała go i uruchomiła dokładnie tak samo jak każdy inny, bez słowa o licencji.

**„Nie wiadomo” — w praktyce to samo co „nie”.** Dotyczy konwersji PLLuM-a od osoby postronnej (podrozdział 7.13). Nie da się oprzeć uprawnienia na dokumencie, którego nie ma. Z punktu widzenia kogoś, kto planuje komercjalizację, „nie wiadomo” i „nie” znaczą to samo — różnią się tylko tym, że przy „nie wiadomo” dowiesz się później i drożej.

**„Warunkowo” — czujność przy wyborze, zwykle zero działań później.** Dotyczy Llamy i Falcona, i to z różnych powodów. W licencji Llamy próg **700 mln aktywnych użytkowników miesięcznie odnosi się do produktów samego licencjobiorcy**, nie do liczby użytkowników modelu — i jest mierzony na dzień wydania danej wersji, czyli dla Llamy 3.1 na 23 lipca 2024. Dla kogokolwiek, kto to czyta, ten warunek jest martwy. Zostają rzeczy tanie i jednorazowe: oznaczenie „Built with Llama” i zgodność z polityką dopuszczalnego użycia. U Falcona warunek jest inny: polityka dopuszczalnego użycia może się zmienić bez zmiany samego modelu, a używać trzeba zgodnie z jej aktualną wersją. Podobnie wyglądały Gemma Terms przy `gemma3` — tam warunek dotyczył **tego, co robisz**, a nie tego, jak jesteś duży, a takiego warunku nie da się przeczekać wzrostem ani spadkiem skali.

**Licencje otwarte — temat nie istnieje.** Apache 2.0, MIT, NVIDIA Open Model i OpenMDW-1.1 nie stawiają progów ani zakazów użycia komercyjnego. To dziewięć rodzin z dwunastu w tym zestawieniu.

#### Dlaczego to trzeba rozstrzygnąć na początku, a nie przy komercjalizacji

Tu jest sedno. Wybór licencyjny na starcie **nie kosztuje nic**, bo wtedy wszystkie warianty są jeszcze równie dobre. Zmiana modelu po dwóch latach kosztuje tyle, ile przeprowadzenie całego badania od nowa — i to jest koszt, który da się z tej pracy wyliczyć, bo właśnie go poniosłem.

Podmiana modelu nie jest zmianą jednej linijki w `.env`. Z pomiarów w tym badaniu:

- **Prompt przestaje działać tak samo.** `think: false` jest honorowany przez jedne modele, a inne go ignorują albo, jak `gpt-oss:20b`, nie znają w ogóle — także w obrębie jednej rodziny. Po zmianie modelu trzeba to sprawdzić od zera.
- **Kod obróbki odpowiedzi przestaje pasować.** Jedne modele zamykają rozumowanie w znacznikach, które sanitizer obcina, inne wysyłają je bez znaczników jako zwykły tekst. To różnica w kodzie, nie w konfiguracji.
- **Architektura agenta może się zawalić.** `phi4`, cała rodzina Falcon i cała rodzina EXAONE nie deklarują narzędzi wcale. Migracja na taki model to nie strojenie, tylko przepisanie założeń.
- **Charakter pracy zmienia się wymiernie.** Zimny start w tej stawce rozciąga się od 2,5 s do 138,5 s na tę samą notatkę, a polszczyzna od bezbłędnej do bełkotu. Model dobrany pod jedno zadanie po podmianie może to zadanie wykonywać inaczej, nawet jeśli formalnie „działa”.

W moim przypadku zamiana `qwen2.5:3b` na młodszy `qwen3.5:2b` wydawała się na początku dobra — następca był szybszy i na pierwszym zestawie danych pisał lepiej. Pełne badanie pokazało coś innego: `qwen3.5:2b` przechodzi dwa scenariusze z jedenastu, bo w cyklach bez ruchu zmyśla procenty wszystkich spółek, i robi to do dziś w aplikacji. Wyszło to w praniu, a nie z planu. Gdyby wyszło **w dniu komercjalizacji, a nie dziś**, wybór byłby już tylko między pogorszeniem produktu a dogadywaniem się z wydawcą. Tego się unika jednym spojrzeniem na licencję i jednym przebiegiem pomiarowym na samym początku — kiedy nie kosztują one nic.

**Praktycznie: w tej stawce czystość licencyjna nie kosztuje dziś ani grama wydajności.** Wszystkie sześć modeli, które przeszły jedenaście scenariuszy z notatkami konsumowalnymi — `granite4.1:8b`, `gemma4:12b`, `ministral-3:14b`, `gemma4:26b`, `qwen3.8:27b` i `granite4:32b-a9b-h` — są na Apache 2.0. Modele na licencjach warunkowych i niekomercyjnych są w tym zadaniu jednocześnie najsłabsze. Nie ma więc żadnego kompromisu do rozważenia: wybór najlepszych modeli i wybór modeli bez zobowiązań to w tej chwili ten sam wybór.

---

## 7.15. Trzy wnioski

**Kolejny numer wersji nie gwarantuje poprawy w każdym wymiarze z osobna — ale bywa, że daje ją od razu.** Najczystszy przykład w badaniu to `qwen3.8:27b`: ten sam model bazowy co `qwen3.6:27b`, a lepszy w każdym mierzonym wymiarze. Odwrotny przykład to IBM. Granite 4.2 przegrywa z Granite 4.1 w tym samym rozmiarze — w ośmiu miliardach nie podaje, o ile zmienił się kurs, w trzydziestu jest wolniejszy i obcina procent — a obie gęste wersje porzuciły hybrydową architekturę, która dawała prędkość i milionowe okno kontekstu. Żadnej z tych rzeczy nie dało się odczytać z karty modelu; trzeba było zmierzyć.

Nie wyciągam z tego wniosku, że stare modele są równie dobre — bo nie są. Między `gemma3` a `gemma4` doszły narzędzia, których wcześniej nie było **wcale**, licencja zmieniła się na Apache 2.0, a Gemma 4 wygrała dwie grupy z czterech. Modele z 2024 roku — Llamy, Falcony i EXAONE — po polsku nie przechodzą testu z rozdziału trzeciego ani razu; jedynym wyjątkiem z tego rocznika jest `phi4` z grudnia 2024. Postęp między generacjami jest realny i wyraźny. Zawodzi dopiero rozdzielczość jednego kroku wersji: na tym dystansie numer mówi, że coś się zmieniło, ale nie mówi, w którą stronę i w którym wymiarze. Dlatego aktualizacji nie robię w ciemno — robię przebieg pomiarowy na swoim zadaniu i dopiero wtedy decyduję.

**Licencję trzeba sprawdzić samodzielnie, przed wdrożeniem — i sprawdzić ponownie.** W stawce są modele z zakazem użycia komercyjnego i modele z warunkiem, który wydawca może zmienić; poza stawką — model, którego licencji nie da się ustalić. Wszystkie pobierają się i uruchamiają dokładnie tak samo jak reszta; Ollama nie ostrzega. Ale ruch bywa też w drugą stronę i równie cicho: `gemma3` szła na własnych warunkach Google'a z klauzulą zakazanych zastosowań, a `gemma4` jest już na czystym Apache 2.0. Rodzina odrzucona rok temu z powodu licencji może dziś być najczystsza w stawce, i nikt o tym nie zawiadomi — trzeba zajrzeć do `ollama show --license`. To najtańszy wymiar doboru: sprawdzenie zajmuje minutę na starcie, a pominięte potrafi kosztować przepisanie rozwiązania wtedy, gdy zacznie zarabiać.

**Najnowsze nie znaczy dostępne.** Najświeższy DeepSeek działa wyłącznie w chmurze. Najnowsza Llama zaczyna się od 70 mld parametrów, najnowsze Qweny od 17 GB, a większe Nemotrony przekraczają 22 GB. Dla maszyny z 16 GB bez karty graficznej „najnowsze” bywa po prostu nieosiągalne — i to jest wynik sam w sobie.
