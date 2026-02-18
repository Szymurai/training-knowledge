## Jak skonfigurować konto OpenAI do integracji z n8n?

Na wstępie: OpenAI to firma stojąca za modelami z rodziny GPT (m.in. GPT-4o, GPT-4.1, o3, o4-mini) oraz popularnymi narzędziami takimi jak ChatGPT, DALL·E czy Whisper. Platforma OpenAI udostępnia rozbudowane API, które pozwala programistom i narzędziom automatyzacyjnym (takim jak n8n) korzystać z modeli językowych, generowania obrazów, transkrypcji mowy oraz wielu innych funkcjonalności.

Na potrzeby warsztatów z n8n wykorzystamy API OpenAI do integracji z węzłami (nodes) obsługującymi modele językowe. Konfiguracja obejmuje trzy kroki: założenie konta na platformie, doładowanie kredytów oraz wygenerowanie klucza API, który następnie zapiszemy w banku poświadczeń (Credentials) na platformie n8n.

**Wymagania wstępne:**
- Dostęp do przeglądarki internetowej
- Konto Google, Microsoft, Apple bądź adres e-mail (do rejestracji na platformie OpenAI)
- Karta płatnicza Visa, Mastercard bądź American Express (do doładowania kredytów)
- Działająca instancja n8n (jak ją uruchomić, opisuję w: [[Konfiguracja platformy n8n]])

---

## Krok 1: Rejestracja konta na platformie OpenAI

Całą konfigurację rozpoczynamy od założenia konta na platformie OpenAI, dostępnej pod adresem:
- https://platform.openai.com/

Na stronie głównej klikamy przycisk **Sign up**. OpenAI umożliwia założenie konta na kilka sposobów:
- **Google** – logowanie za pomocą konta Google
- **Microsoft** – logowanie za pomocą konta Microsoft
- **Apple** – logowanie za pomocą konta Apple
- **E-mail** – rejestracja klasyczna za pomocą adresu e-mail i hasła

![[screenshots-openai/openai-screenshot-placeholder-01.png]]

Wybieramy preferowaną metodę rejestracji i postępujemy zgodnie z wyświetlanymi instrukcjami. Po pomyślnej rejestracji zostaniemy przekierowani do głównego dashboardu platformy OpenAI:
![[screenshots-openai/openai-screenshot-placeholder-02.png]]

> **Wskazówka:** OpenAI rozróżnia dwa osobne produkty, tj. **ChatGPT** (chatgpt.com) oraz **API Platform** (platform.openai.com). Na potrzeby integracji z n8n interesuje nas wyłącznie platforma API. Konto założone w ChatGPT daje dostęp również do platformy API, natomiast kredyty i płatności są rozliczane osobno.

---

## Krok 2: Doładowanie konta (Credits)

OpenAI API działa w modelu prepaid, tj. przed rozpoczęciem korzystania z API musimy doładować konto odpowiednią kwotą kredytów (denominowanych w dolarach amerykańskich). Koszt każdego zapytania do modelu AI jest automatycznie odliczany od naszego salda.

Przechodzimy do ustawień rozliczeniowych w panelu użytkownika:
- https://platform.openai.com/settings/organization/billing/overview

![[screenshots-openai/openai-screenshot-placeholder-03.png]]

**Dodanie metody płatności:**
1. Na stronie Billing klikamy przycisk **Add payment details**.
2. Wybieramy typ konta: **Individual** (konto indywidualne) bądź **Company** (konto firmowe).
3. Uzupełniamy dane karty płatniczej i adres rozliczeniowy.

![[screenshots-openai/openai-screenshot-placeholder-04.png]]

**Doładowanie kredytów:**
1. Po dodaniu metody płatności klikamy przycisk **Add to credit balance**.
2. Wpisujemy kwotę, którą chcemy wpłacić (minimalna kwota to **$5**).
3. Potwierdzamy transakcję.

![[screenshots-openai/openai-screenshot-placeholder-05.png]]

**Dostępne metody płatności:**

| Metoda | Uwagi |
|---|---|
| Karty płatnicze (Visa, Mastercard, American Express) | Obsługiwane przez Stripe |
| Karty debetowe | Obsługiwane przez Stripe |

> **Warto wiedzieć:** OpenAI stosuje własny cennik za użycie poszczególnych modeli, rozliczany w tokenach (jednostkach tekstu). Aktualne stawki dla każdego modelu są dostępne na stronie: https://platform.openai.com/docs/pricing. Przykładowo: model GPT-4o kosztuje $2.50 za 1 milion tokenów wejściowych i $10.00 za 1 milion tokenów wyjściowych, co w praktyce przekłada się na grosze za pojedyncze zapytanie.

**Automatyczne doładowanie (Auto recharge):**

OpenAI oferuje możliwość skonfigurowania automatycznego doładowania konta. Po włączeniu tej opcji platforma automatycznie zasili nasze saldo, gdy spadnie ono poniżej ustalonego progu, tj. unikniemy sytuacji, w której nasze automatyzacje w n8n przestaną działać z powodu wyczerpania kredytów. Konfiguracja automatycznego doładowania jest dostępna bezpośrednio na stronie Billing.

![[screenshots-openai/openai-screenshot-placeholder-06.png]]

> **Ważne:** Kredyty API OpenAI wygasają po upływie jednego roku od daty zakupu. Niewykorzystane środki po tym okresie przepadają.

**Limity wydatków (Usage limits):**

Konfiguracja limitów wydatków to kluczowy krok, którego nie należy pomijać. Rozwiązania oparte o LLM (Large Language Models) potrafią wpaść w nieskończoną pętlę, co skutkuje wyzerowaniem dostępnych środków w ciągu minut. Aby się przed tym zabezpieczyć, przechodzimy do ustawień limitów:
- https://platform.openai.com/settings/organization/limits

![[screenshots-openai/openai-screenshot-placeholder-06b.png]]

Na tej stronie konfigurujemy dwa progi:
1. **Monthly budget** – miesięczny limit wydatków. Po jego osiągnięciu platforma zablokuje dalsze zapytania do API. Na potrzeby warsztatów rekomendujemy ustawienie limitu na **$20**.
2. **Email notification threshold** – próg, po przekroczeniu którego otrzymamy powiadomienie e-mail. Warto ustawić go na niższą wartość niż miesięczny limit, np. **$10**, aby mieć czas na reakcję.

> **Ważne:** Ustawienie limitu wydatków jest szczególnie istotne przy automatyzacjach w n8n. Źle skonfigurowany przepływ (np. pętla wywołująca model GPT w nieskończoność) może w krótkim czasie wygenerować setki bądź tysiące zapytań, co przełoży się na znaczne koszty. Limit wydatków stanowi zabezpieczenie przed takim scenariuszem.

---

## Krok 3: Wygenerowanie klucza API

Mając doładowane konto, możemy przystąpić do wygenerowania klucza API, który posłuży nam do uwierzytelniania zapytań z poziomu n8n.

Przechodzimy na stronę zarządzania kluczami API:
- https://platform.openai.com/api-keys

![[screenshots-openai/openai-screenshot-placeholder-07.png]]

**Tworzenie nowego klucza:**
1. Klikamy przycisk **Create new secret key**.
2. Nadajemy kluczowi opisową nazwę, np. `n8n-integration` (ułatwi to identyfikację klucza w przyszłości, szczególnie gdy będziemy posiadać ich więcej).
3. Wybieramy **projekt** (Project), do którego klucz ma być przypisany. Domyślnie dostępny jest projekt **Default project**, co jest wystarczające na potrzeby warsztatów.
4. Ustawiamy **Permissions**, tj. uprawnienia klucza:
   - **All** – pełen dostęp do wszystkich endpointów API (zalecane na potrzeby warsztatów)
   - **Restricted** – ograniczony dostęp, pozwalający precyzyjnie kontrolować, do których usług klucz ma uprawnienia
5. Klikamy **Create secret key** w celu wygenerowania klucza.

![[screenshots-openai/openai-screenshot-placeholder-08.png]]

Po utworzeniu klucza zostanie on wyświetlony na ekranie:
![[screenshots-openai/openai-screenshot-placeholder-09.png]]

> **Ważne:** Klucz API wyświetlany jest **TYLKO RAZ** – po zamknięciu okna nie będziemy mogli go ponownie odczytać. Należy go natychmiast skopiować i zapisać w bezpiecznym miejscu, np. w aplikacji [1password](https://1password.com). W razie utraty klucza jedyną opcją jest wygenerowanie nowego.

---

## Krok 4: Konfiguracja poświadczeń OpenAI w n8n

Jak uruchomić własną instancję n8n lub założyć darmowe konto na oficjalnej stronie producenta, szczegółowo ilustruję w tym wątku:
[[Konfiguracja platformy n8n]]

Platforma n8n oferuje natywny typ poświadczeń **OpenAi API**, co sprawia, że konfiguracja jest szybka i bezproblemowa, tj. wystarczy wkleić klucz API.

**Tworzenie poświadczenia:**
1. W n8n przechodzimy do zakładki **Credentials** i klikamy **Create credential** (bądź **Add first credential**, jeśli uruchamiamy świeżą instancję).
2. W wyświetlonym oknie wyszukujemy typ poświadczeń: **OpenAi API** i klikamy **Continue**.

![[screenshots-openai/openai-screenshot-placeholder-10.png]]

3. Uzupełniamy pole w formularzu:
   - **API Key** – klucz API, który wygenerowaliśmy w poprzednim kroku

![[screenshots-openai/openai-screenshot-placeholder-11.png]]

4. Klikamy **Save** w celu zapisania poświadczenia.

Warto w tym miejscu zwrócić uwagę, iż w wersji Cloud platformy n8n (dostępnej na https://n8n.io/) możemy posłużyć się wbudowanym asystentem AI (przycisk **n8n AI**) w celu uzyskania instrukcji dotyczących poprawnego uzupełnienia pól formularza. Funkcja ta nie jest dostępna w instancjach self-hosted.

> **Wskazówka:** Formularz poświadczeń OpenAi w n8n zawiera również opcjonalne pole **Organization ID**. Jeśli korzystamy z konta firmowego przypisanego do organizacji, warto uzupełnić to pole identyfikatorem organizacji (dostępnym w ustawieniach konta: https://platform.openai.com/settings/organization/general). Dla kont indywidualnych pole to można pozostawić puste.

Po poprawnej konfiguracji powinniśmy móc korzystać z modeli OpenAI bezpośrednio z poziomu węzłów (nodes) n8n, tj. w węźle **AI Agent**, **Chat Model**, **OpenAI** bądź dowolnym innym węźle obsługującym modele językowe.

> **Warto wiedzieć:** Nowe konta na platformie n8n Cloud mogą otrzymać **100 darmowych kredytów od OpenAI**, tj. stosowny komunikat z przyciskiem **Claim credits** pojawi się w górnej części formularza poświadczeń OpenAi.

---

## Bezpieczeństwo kluczy API

Klucz API OpenAI pełni rolę poświadczenia uwierzytelniającego, tj. każdy, kto go posiada, może wykonywać zapytania do modeli AI na koszt naszego konta. Dlatego warto przestrzegać kilku zasad:

- **Nigdy nie commitujemy kluczy do publicznych repozytoriów** – OpenAI automatycznie skanuje popularne platformy (m.in. GitHub) pod kątem wycieków kluczy API. W przypadku wykrycia ujawnionego klucza zostanie on automatycznie unieważniony, a my otrzymamy powiadomienie e-mail.
- **Przechowujemy klucze w menedżerze haseł** – np. [1password](https://1password.com), Bitwarden bądź KeePass.
- **Stosujemy zmienne środowiskowe** – jeśli używamy klucza w kodzie bądź skryptach, przechowujemy go w pliku `.env` (dodanym do `.gitignore`), a nie bezpośrednio w kodzie źródłowym.
- **Konfigurujemy limity wydatków** – w ustawieniach Billing na platformie OpenAI możemy ustawić miesięczny limit wydatków (Usage limits), aby zabezpieczyć się przed nieoczekiwanym wyczerpaniem środków. Jest to szczególnie istotne w kontekście automatyzacji, które mogą generować dużą liczbę zapytań.
- **W razie podejrzenia wycieku danych** – natychmiast usuwamy klucz na stronie https://platform.openai.com/api-keys i generujemy nowy.

---

## Podsumowanie

W ramach tego poradnika wykonaliśmy kompletną konfigurację konta OpenAI na potrzeby integracji z platformą n8n:

1. **Założyliśmy konto** na platformie OpenAI – rejestrując się za pomocą konta Google, Microsoft, Apple bądź adresu e-mail.
2. **Doładowaliśmy kredyty** – dodając metodę płatności i zasilając konto odpowiednią kwotą, niezbędną do korzystania z API modeli językowych.
3. **Wygenerowaliśmy klucz API** – unikalny token uwierzytelniający, który skopiowaliśmy i zapisaliśmy w bezpiecznym miejscu.
4. **Skonfigurowaliśmy poświadczenie w n8n** – wykorzystując natywny typ OpenAi API i podając wygenerowany klucz.

W odróżnieniu od OpenRouter, który oferuje dostęp do modeli wielu dostawców przez jedno API, konto OpenAI daje nam bezpośredni dostęp wyłącznie do modeli OpenAI (GPT-4o, GPT-4.1, o3, o4-mini, DALL·E, Whisper itd.). Zaletą bezpośredniej integracji jest brak pośredników, niższe opóźnienia (latency) oraz natychmiastowy dostęp do najnowszych modeli i funkcjonalności udostępnianych przez OpenAI.

Tak przygotowane poświadczenie stanowi fundament dalszej pracy z n8n – będziemy z niego korzystać przy tworzeniu przepływów automatyzujących zadania z wykorzystaniem modeli językowych, agentów AI oraz narzędzi generatywnych.