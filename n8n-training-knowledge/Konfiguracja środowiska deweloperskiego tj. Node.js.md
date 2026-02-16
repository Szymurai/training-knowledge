## Konfiguracja środowiska — Node.js

Aby uruchomić n8n bezpośrednio w systemie operacyjnym (bez konteneryzacji), potrzebujemy środowiska **Node.js** w wersji 18 lub nowszej. Node.js to środowisko uruchomieniowe JavaScript, które umożliwia wykonywanie kodu poza przeglądarką, a to właśnie na nim opiera się n8n.

Poniżej znajdziesz instrukcję instalacji dla systemu Windows:

---

###  Instalacja Node.js na Windows

Proces instalacji składa się z kilku kroków:

1. **Pobranie instalatora** – przejdź na stronę [nodejs.org/en/download](https://nodejs.org/en/download/) i pobierz instalator Node.js dla systemu Windows.
2. **Uruchomienie instalatora** – po zakończeniu pobierania uruchom pobrany plik. Wybierz preferowane opcje instalacji (zazwyczaj wystarczą ustawienia domyślne).
3. **Weryfikacja instalacji** – otwórz wiersz poleceń (Command Prompt lub PowerShell) i wpisz polecenie `node -v`. Jeśli w odpowiedzi zobaczysz numer wersji, instalacja przebiegła pomyślnie.

---

## Podsumowanie

W ramach tego poradnika zainstalowaliśmy środowisko Node.js na systemie Windows:

1. **Pobraliśmy instalator** ze strony nodejs.org.
2. **Przeprowadziliśmy instalację** z domyślnymi ustawieniami.
3. **Zweryfikowaliśmy poprawność instalacji** poleceniem `node -v`.

Node.js jest wymagany do uruchomienia platformy n8n w wersji self-hosted (bez konteneryzacji). Po zainstalowaniu Node.js możemy przejść do instalacji n8n: szczegóły w poradniku: [[Konfiguracja platformy n8n]].
