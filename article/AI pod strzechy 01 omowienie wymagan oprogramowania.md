# Część 1. Omówienie wymagań oprogramowania

W tym artykule skupiam się na omówieniu oprogramowania użytego w moim teście, aby przybliżyć jego cechy i uzasadnić niektóre decyzje. Kluczowe komponenty to:
- Ollama
- Podman
- Aplikacja w Node.js

W środowisku aplikacji użyłem oprócz silnika aplikacji Node.js również silnika bazy danych SQL Postgres oraz NoSQL Mongo. PostgreSQL oraz MongoDB nie miały aktywnej roli w testowaniu samych modeli i nie będę ich omawiał.

## 1.1 Dlaczego Ollama

**Ollama powstała w 2023 roku** i założyli ją Jeffrey Morgan oraz Michael Chiang — ci sami, którzy wcześniej zbudowali Kitematic, graficzną nakładkę na Dockera, sprzedaną Dockerowi w 2015 roku. Ollama jest w zasadzie tym samym pomysłem przeniesionym na modele językowe: piszemy **`ollama pull`** podobnie do **`docker pull`**.

Siedziba firmy znajduje się w Palo Alto i dzięki obecności na platformie dla startupów <a href="https://www.ycombinator.com" target="_blank">Y Combinator</a> zebrała **88 milionów dolarów** finansowania, z Theory Ventures na czele oraz Benchmarkiem i YC wśród inwestorów. 

Jak sama podaje, obecnie ma blisko dziewięć milionów użytkowników.

Ollama jest silnikiem, który oferuje najbogatszą bibliotekę modeli, gotowych do pobrania na maszynę lokalną za wywołaniem jednego polecenia w terminalu.
Co najważniejsze, jest darmowa, a więc bardzo przystępna dla budżetu domowego. Wersja chmurowa wiąże się z założeniem konta, natomiast wersja do pobrania na komputer - nie, przynajmniej w chwili pisania tego artykułu.

W moim badaniu wykorzystałem Ollamę 0.40.6 na obu stanowiskach: w kontenerze w WSL/Podman oraz zainstalowaną bezpośrednio na hoście w Windows 11.

### 1.1.1. Co jest w bibliotece

Wprost ze strony Ollamy, oferuje ona **240 modeli w oficjalnej bibliotece** (przestrzeń `ollama.com/library`). To zbiór kurowany przez Ollamę — konwersje wag pochodzących bezpośrednio od producentów, przygotowane i utrzymywane centralnie.

Obok niej są **przestrzenie nazw użytkowników**, i tu przebiega granica, która okazała się dla tego badania kluczowa:

| Rodzaj | Przykład | Kto odpowiada |
|---|---|---|
| Biblioteka oficjalna | `llama3.1`, `gemma4`, `qwen3.5` | Ollama, z wag producenta |
| **Konto wydawcy** | `ibm/granite4`, `SpeakLeash/bielik-11b` | **sam producent** |
| Konwersja społecznościowa | `bbaranow/pllum-12B`, `aiconjured/…` | osoba prywatna |

Ollama **nie publikuje liczby modeli społecznościowych** ani nie oznacza ich w interfejsie. Wyszukiwarka pokazuje wszystkie trzy rodzaje wymieszane, bez etykiety mówiącej, skąd pochodzą wagi. Jedyny sygnał to ukośnik w nazwie. To realna wada i może kosztować pół dnia dochodzenia przy ustalaniu, czyj właściwie jest model (np. PLLuM), jeśli nie zestawić jego nazwy z danymi z innych źródeł, np. HuggingFace.

### 1.1.2. Skala wobec HuggingFace

| | modele |
|---|---|
| HuggingFace | **3 085 046** |
| Ollama, biblioteka oficjalna | **240** |

Różnica jest czterorzędowa, ale **to nie jest od razu wada Ollamy, tylko jej definicja**. HuggingFace to uznane, centralne repozytorium i obsługuje wszystkie modele, w każdym formacie, wraz z wagami treningowymi, adapterami i eksperymentami. Ollama jest **środowiskiem uruchomieniowym** na lokalnym komputerze i w bibliotece znajdziemy to, co da się uruchomić jednym poleceniem, w formacie GGUF, z gotowym szablonem interakcji i domyślnymi parametrami.

Dla tego badania wybrałem Ollamę właśnie z powodu tej selekcji. Trzy miliony modeli to doskonały katalog dla kogoś, kto buduje własny łańcuch narzędzi. Dwieście czterdzieści kurowanych modeli plus konta wydawców z powodzeniem stanowi odpowiedź na prostsze potrzeby użytkowników domowych.

Wybór Ollamy do mojego testu zatem to wybór bardzo ekonomiczny a modele wybrane przeze mnie do testów muszą spełniać warunek opublikowania w katalogu Ollamy.

> Wszystkie pomiary w tym artykule wykonałem na **Ollamie 0.40.6 w kontenerze i na hoście**, w październiku 2026.


## 1.2. Dlaczego Podman

**Podman został opracowany przez firmę Red Hat**. Pierwsze publiczne wydanie (0.2) ukazało się w 2018 roku, a wersja 1.0 — 16 stycznia 2019. Nazwa to skrót od **pod manager**, czyli menedżera podów: grup kontenerów dzielących przestrzeń sieciową, pojęcia zapożyczonego wprost z Kubernetesa. To mówi o intencji więcej niż jakikolwiek opis — Podman od początku był projektowany tak, żeby to, co uruchamiasz na laptopie, miało ten sam kształt co to, co potem może zostać umieszczone na produkcyjnym klastrze.

Dla kogoś, kto zetknął się w przeszłości z Dockerem, podobieństwa Podmana do Dockera, podobnie jak przy Ollamie, dają się momentalnie zauważyć. Zgodność poleceń powoduje, że wyglądają praktycznie identycznie: `podman run`, `podman ps`, `podman build`. 

**Licencja to Apache 2.0**, a od 21 stycznia 2025 Podman i Podman Desktop są projektami [CNCF](https://www.cncf.io/projects/podman-container-tools/) — tej samej fundacji, która opiekuje się Kubernetesem. To istotna różnica wobec Ollamy: Ollama jest startupem po rundzie finansowania, Podman jest projektem fundacyjnym z korporacyjnym rodzicem. 

Z Podmanem zetknąłem się wcześniej, dzięki [IBM WatsonX Data](https://www.ibm.com/products/watsonx-data) i był to mój naturalny wybór, aby zbadać możliwości uruchomienia AI w warunkach domowych. Wydaje mi się łatwy w obsłudze, jego interfejs Desktop jest dla mnie czytelny i jako program jest po prostu darmowy. Docker Desktop również jest darmowy dla użytku osobistego, edukacji i małych firm, ale w przypadku tych ostatnich tylko do 250 pracowników lub 10 mln USD przychodu. Zatem w moim teście sprawdziłby się również, jestem jednak przyzwyczajony do Podmana.

W moim badaniu wykorzystałem Podman 5.7.1 na Windows 11.


## 1.3. Dlaczego Node.js

**Node.js został opracowany przez amerykańskiego programistę, Ryana Dahla**. Pierwsze wydanie pojawiło się w 2009 roku jako próba rozwiązania problemu skalowalności aplikacji internetowych. Nazwa jest prosta i bezpośrednia: „node” oznacza węzeł sieciowy, a cały projekt miał umożliwić budowanie lekkich usług komunikujących się ze sobą przez sieć. Kluczową ideą było wykorzystanie silnika JavaScript V8 stworzonego przez Google oraz odejście od modelu „jeden wątek na połączenie” na rzecz architektury zdarzeniowej i asynchronicznego przetwarzania operacji wejścia i wyjścia.

Dla mnie, jako osoby, która wcześniej pracowała z JavaScriptem w przeglądarce, Node.js jest naturalnym krokiem dalej. Pozwala wykorzystywać ten sam język zarówno po stronie klienta, jak i serwera. Uruchamianie aplikacji sprowadza się często do prostych poleceń: `node app.js`, `npm install`, `npm run start`. Dzięki temu próg wejścia jest niski, a ekosystem niezwykle bogaty i konkurencyjny wobec np. Pythona. 

**Node.js jest projektem open source**, rozwijanym przez globalną społeczność programistów przy wsparciu fundacji OpenJS Foundation. To jeden z najpopularniejszych runtime'ów wykorzystywanych do budowy aplikacji webowych, API, narzędzi CLI oraz rozwiązań czasu rzeczywistego. Jego popularność wynika nie tylko z wydajności, ale także z ogromnego repozytorium pakietów dostępnych przez npm, które obecnie stanowi jeden z największych ekosystemów oprogramowania na świecie.

W moim badaniu wykorzystałem Node.js 24.14.0.
