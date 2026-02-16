## Jak skonfigurować Google Cloud Console do integracji z n8n? (Gmail, Sheets, Google Drive, Docs)

Na wstępie – aby móc korzystać z usług Google (Gmail, Google Sheets, Google Drive, Google Docs) na platformie n8n, musimy utworzyć projekt w Google Cloud Console, włączyć odpowiednie API tj. (Gmail, Google Sheets, Google Drive, Google Docs) oraz wygenerować poświadczenia OAuth 2.0 (tj. Client ID, jak i Client Secret). Dzięki temu n8n uzyska autoryzowany dostęp do naszego konta Google.

**Wymagania wstępne:**
- Posiadanie konta Google (np. skrzynki Gmail: company-name@gmail.com)
- Dostęp do przeglądarki internetowej
- Działająca instancja n8n (jak ją uruchomić – opisuję w: [[Konfiguracja platformy n8n]])

---

## Krok 1: Utworzenie projektu w Google Cloud Console

Całą konfigurację przeprowadzamy w Google Cloud Console, dostępnej pod adresem:
- https://console.cloud.google.com/

Po zalogowaniu się na konto Google (to, z którym będziemy integrować narzędzia) przechodzimy do Google Cloud Console.
![[n8n-training-knowledge/screenshots-google-console/01-google-login.png]]
Po zalogowaniu zobaczymy główny dashboard Google Cloud Console:
![[n8n-training-knowledge/screenshots-google-console/02-gcc-dashboard.png]]

**Tworzenie nowego projektu:**
1. Na górnym pasku nawigacyjnym klikamy na selektor projektów (obok logo Google Cloud).
![[n8n-training-knowledge/screenshots-google-console/03-gcc-project-selector.png]]
2. W oknie, które się pojawi, klikamy przycisk **Nowy projekt** (ang. *New Project*).
3. Uzupełniamy pola:
   - **Nazwa projektu** – np. `n8n-integration` (nazwa dowolna, ale warto, aby była opisowa)
   - **Organizacja / Lokalizacja** – pozostawiamy domyślne ustawienia (chyba że pracujemy w ramach organizacji Google Workspace)
4. Klikamy **Utwórz** (ang. *Create*).
![[n8n-training-knowledge/screenshots-google-console/04-gcc-new-project.png]]

Po chwili projekt zostanie utworzony. Upewniamy się, że nowo utworzony projekt jest aktywny (widoczny na górnym pasku nawigacyjnym).

---

## Krok 2: Włączenie wymaganych API

Aby n8n mogło komunikować się z usługami Google, musimy włączyć odpowiednie interfejsy API w naszym projekcie.

**Przejście do biblioteki API:**
1. W menu bocznym (hamburger menu) wybieramy: **API i usługi > Biblioteka** (ang. *APIs & Services > Library*).
![[n8n-training-knowledge/screenshots-google-console/05-gcc-api-library.png]]
2. W pasku wyszukiwania wpisujemy nazwę usługi i włączamy kolejno każde z poniższych API:

| Usługa | Nazwa API do wyszukania | Opis |
|---|---|---|
| Gmail | `Gmail API` | Wysyłanie, odczytywanie i zarządzanie wiadomościami e-mail |
| Google Sheets | `Google Sheets API` | Odczyt i zapis danych w arkuszach kalkulacyjnych |
| Google Drive | `Google Drive API` | Zarządzanie plikami i folderami na Dysku Google |
| Google Docs | `Google Docs API` | Tworzenie i edycja dokumentów tekstowych |

3. Po wyszukaniu danego API klikamy na jego kafelek, a następnie przycisk **Włącz** (ang. *Enable*).

Przykładowy widok strony Gmail API z przyciskiem „Enable":
![[n8n-training-knowledge/screenshots-google-console/06-gcc-gmail-api.png]]

Analogicznie postępujemy z pozostałymi API:
![[n8n-training-knowledge/screenshots-google-console/07-gcc-sheets-api.png]]
- Google Drive API:
![[n8n-training-knowledge/screenshots-google-console/08-gcc-drive-api.png]]
- Google Docs API:
![[n8n-training-knowledge/screenshots-google-console/09-gcc-docs-api.png]]

4. Powtarzamy tę czynność dla każdego z czterech API.

> **Wskazówka:** Możemy również skorzystać z bezpośrednich linków:
> - [Gmail API](https://console.cloud.google.com/apis/library/gmail.googleapis.com)
> - [Google Sheets API](https://console.cloud.google.com/apis/library/sheets.googleapis.com)
> - [Google Drive API](https://console.cloud.google.com/apis/library/drive.googleapis.com)
> - [Google Docs API](https://console.cloud.google.com/apis/library/docs.googleapis.com)

---

## Krok 3: Konfiguracja ekranu zgody OAuth (OAuth Consent Screen)

Zanim utworzymy poświadczenia, musimy skonfigurować ekran zgody OAuth. Jest to formularz, który użytkownik widzi podczas autoryzacji dostępu do swojego konta Google.

1. W menu bocznym wybieramy: **API i usługi > Ekran zgody OAuth** (ang. *APIs & Services > OAuth consent screen*).
![[n8n-training-knowledge/screenshots-google-console/10-gcc-oauth-overview.png]]
2. Wybieramy typ użytkownika:
   - **Zewnętrzny** (ang. *External*) – jeśli korzystamy ze zwykłego konta Gmail
   - **Wewnętrzny** (ang. *Internal*) – jeśli korzystamy z konta w ramach Google Workspace (tylko użytkownicy z naszej organizacji będą mieli dostęp)
![[n8n-training-knowledge/screenshots-google-console/12-gcc-oauth-audience.png]]
3. Klikamy **Utwórz** (ang. *Create*).

**Wypełniamy formularz ekranu zgody:**
- **Nazwa aplikacji** – np. `n8n Integration`
- **Adres e-mail pomocy użytkownika** – nasz adres Gmail
- **Logo aplikacji** – opcjonalnie (można pominąć)
- Przewijamy na dół formularza i uzupełniamy:
  - **Dane kontaktowe dewelopera** – nasz adres e-mail
- Klikamy **Zapisz i kontynuuj** (ang. *Save and Continue*).
![[n8n-training-knowledge/screenshots-google-console/11-gcc-oauth-branding.png]]

**Konfiguracja zakresów (Scopes):**
1. Na ekranie „Zakresy" (ang. *Scopes*) klikamy **Dodaj lub usuń zakresy** (ang. *Add or Remove Scopes*).
2. Wyszukujemy i zaznaczamy zakresy odpowiadające włączonym API:
   - `https://www.googleapis.com/auth/gmail.modify` – Gmail (odczyt i wysyłka)
   - `https://www.googleapis.com/auth/spreadsheets` – Google Sheets
   - `https://www.googleapis.com/auth/drive` – Google Drive
   - `https://www.googleapis.com/auth/documents` – Google Docs
3. Klikamy **Aktualizuj** (ang. *Update*), a następnie **Zapisz i kontynuuj**.
![[n8n-training-knowledge/screenshots-google-console/18-gcc-data-access-scopes.png]]

**Użytkownicy testowi (dotyczy typu „Zewnętrzny"):**
1. Na ekranie „Użytkownicy testowi" klikamy **Dodaj użytkowników** (ang. *Add Users*).
2. Wpisujemy adres e-mail konta Google, z którym będziemy integrować n8n.
3. Klikamy **Dodaj**, a następnie **Zapisz i kontynuuj**.

> **Ważne:** Dopóki aplikacja ma status „Testowanie" (ang. *Testing*), dostęp będą mieli wyłącznie użytkownicy dodani na listę testową. Dla zastosowań prywatnych / warsztatowych jest to wystarczające i nie wymaga przechodzenia procesu weryfikacji Google.

---

## Krok 4: Utworzenie poświadczeń OAuth 2.0

Teraz tworzymy klucze poświadczające, które n8n będzie wykorzystywać do autoryzacji.

1. W menu bocznym wybieramy: **API i usługi > Dane logowania** (ang. *APIs & Services > Credentials*).
![[n8n-training-knowledge/screenshots-google-console/13-gcc-oauth-clients.png]]
2. Na górze strony klikamy **Utwórz dane logowania > Identyfikator klienta OAuth** (ang. *Create Credentials > OAuth client ID*).
![[n8n-training-knowledge/screenshots-google-console/14-gcc-create-oauth-client.png]]
3. Uzupełniamy formularz:
   - **Typ aplikacji** – wybieramy **Aplikacja internetowa** (ang. *Web application*)
![[n8n-training-knowledge/screenshots-google-console/15-gcc-application-type-dropdown.png]]
   - **Nazwa** – np. `n8n OAuth Client`
   - **Autoryzowane identyfikatory URI przekierowania** (ang. *Authorized redirect URIs*) – klikamy **Dodaj URI** i wpisujemy adres callback n8n:
     - Dla instancji lokalnej (Docker): `http://localhost:5678/rest/oauth2-credential/callback`
     - Dla instancji n8n Cloud: `https://app.n8n.cloud/rest/oauth2-credential/callback`
     - Dla własnej domeny: `https://twoja-domena.com/rest/oauth2-credential/callback`
![[n8n-training-knowledge/screenshots-google-console/16-gcc-web-app-form.png]]
![[n8n-training-knowledge/screenshots-google-console/17-gcc-redirect-uris.png]]
4. Klikamy **Utwórz** (ang. *Create*).

Po utworzeniu poświadczeń pojawi się okno z danymi:
- **Identyfikator klienta** (ang. *Client ID*) – np. `123456789-abc.apps.googleusercontent.com`
- **Tajny klucz klienta** (ang. *Client Secret*) – np. `GOCSPX-xxxxxxxxxx`

> **Ważne:** Skopiuj oba klucze i zapisz je w bezpiecznym miejscu (np. w menedżerze haseł, takim jak [1Password](https://1password.com)). Tajny klucz klienta wyświetlany jest tylko raz – później można go jedynie zresetować.

> **Warto w tym miejscu zwrócić uwagę, iż** jeśli korzystamy z wersji Cloud platformy n8n (dostępnej na https://n8n.io/), to **nie musimy samodzielnie generować Client ID ani Client Secret**. Wystarczy, że podczas konfiguracji poświadczeń w n8n zalogujemy się swoim kontem Google – platforma n8n Cloud automatycznie obsłuży proces autoryzacji OAuth 2.0 za nas. Ręczne tworzenie poświadczeń w Google Cloud Console jest wymagane jedynie w przypadku instancji self-hosted (np. uruchomionej lokalnie przez Docker).

---

## Krok 5: Konfiguracja poświadczeń Google w n8n

Mając przygotowane Client ID i Client Secret, możemy teraz skonfigurować poświadczenia na platformie n8n. Proces powtarzamy dla każdej usługi Google, z której chcemy korzystać.

### 5a: Gmail

1. W n8n przechodzimy do zakładki **Credentials** i klikamy **Add Credential**.
2. Wyszukujemy typ poświadczenia: **Gmail OAuth2 API** i klikamy **Continue**.
3. Uzupełniamy pola:
   - **Client ID** – identyfikator klienta skopiowany z Google Cloud Console
   - **Client Secret** – tajny klucz klienta
4. Klikamy **Sign in with Google** – zostaniemy przekierowani do ekranu zgody Google.
5. Wybieramy konto Google i zatwierdzamy dostęp.
6. Po powrocie do n8n powinien pojawić się zielony komunikat potwierdzający poprawne połączenie.

### 5b: Google Sheets

1. Tworzymy nowe poświadczenie typu: **Google Sheets OAuth2 API**.
2. Podajemy te same **Client ID** i **Client Secret** (możemy używać jednego zestawu kluczy OAuth dla wszystkich usług Google w ramach tego samego projektu).
3. Klikamy **Sign in with Google** i autoryzujemy dostęp.

### 5c: Google Drive

1. Tworzymy nowe poświadczenie typu: **Google Drive OAuth2 API**.
2. Podajemy **Client ID** i **Client Secret**.
3. Autoryzujemy dostęp przez Google.

### 5d: Google Docs

1. Tworzymy nowe poświadczenie typu: **Google Docs OAuth2 API**.
2. Podajemy **Client ID** i **Client Secret**.
3. Autoryzujemy dostęp przez Google.

> **Wskazówka:** Jeden projekt w Google Cloud Console z włączonymi wszystkimi API i jednym zestawem kluczy OAuth 2.0 wystarczy do obsługi wielu usług Google. Nie trzeba tworzyć osobnych projektów ani kluczy dla każdej usługi.

---

## Rozwiązywanie problemów

### Błąd 403: `access_denied`
- Upewnij się, że Twój adres e-mail został dodany do listy **użytkowników testowych** w ekranie zgody OAuth.
- Sprawdź, czy wszystkie wymagane API zostały włączone w projekcie.

### Błąd: `redirect_uri_mismatch`
- Sprawdź, czy adres URI przekierowania w Google Cloud Console **dokładnie** odpowiada adresowi Twojej instancji n8n (uwaga na `http` vs `https` oraz końcowy ukośnik `/`).

### Token wygasa po 7 dniach
- Dotyczy aplikacji ze statusem „Testowanie". Aby uniknąć wygasania tokenów, można opublikować aplikację (przycisk **Opublikuj aplikację** na ekranie zgody OAuth). Dla aplikacji prywatnych Google zazwyczaj nie wymaga pełnej weryfikacji, jeśli nie przekraczamy 100 użytkowników.

### Błąd: `invalid_client`
- Sprawdź, czy Client ID i Client Secret zostały skopiowane poprawnie (bez zbędnych spacji).
- Upewnij się, że wybrany typ aplikacji to **Aplikacja internetowa** (nie Android, iOS ani Desktop).

---

## Podsumowanie

W ramach tego poradnika wykonaliśmy kompletną konfigurację Google Cloud Console na potrzeby integracji z platformą n8n:

1. **Utworzyliśmy projekt** w Google Cloud Console – stanowi on kontener dla naszych API i poświadczeń.
2. **Włączyliśmy cztery API** – Gmail, Google Sheets, Google Drive oraz Google Docs – umożliwiając n8n komunikację z tymi usługami.
3. **Skonfigurowaliśmy ekran zgody OAuth** – zdefiniowaliśmy nazwę aplikacji, zakresy uprawnień oraz użytkowników testowych.
4. **Wygenerowaliśmy poświadczenia OAuth 2.0** (Client ID + Client Secret) – klucze umożliwiające bezpieczną autoryzację.
5. **Połączyliśmy n8n z usługami Google** – tworząc odpowiednie Credentials dla Gmail, Sheets, Drive i Docs.

Tak przygotowane poświadczenia stanowią fundament do budowania automatyzacji w n8n obejmujących ekosystem Google – od wysyłania e-maili przez Gmail, przez operacje na arkuszach kalkulacyjnych, aż po zarządzanie plikami na Dysku Google i edycję dokumentów.
