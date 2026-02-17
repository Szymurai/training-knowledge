## Jak skonfigurować konto OpenRouter do integracji z n8n?

Na wstępie – OpenRouter to platforma udostępniająca zunifikowane API do setek modeli AI (m.in. OpenAI, Anthropic, Google, Meta, Mistral) przez jeden endpoint. Dzięki temu nie musimy osobno konfigurować dostępu do każdego dostawcy – wystarczy jedno konto i jeden klucz API, aby korzystać z dowolnego modelu językowego dostępnego w katalogu OpenRouter.

Na potrzeby warsztatów z n8n wykorzystamy OpenRouter jako bramkę dostępu do modeli językowych. Konfiguracja obejmuje trzy kroki: założenie konta, doładowanie kredytów oraz wygenerowanie klucza API, który następnie zapiszemy w banku poświadczeń (Credentials) na platformie n8n.

**Wymagania wstępne:**
- Dostęp do przeglądarki internetowej
- Konto Google, GitHub bądź adres e-mail (do rejestracji na platformie OpenRouter)
- Karta płatnicza, kryptowaluta (USDC) bądź AliPay (do doładowania kredytów)
- Działająca instancja n8n (jak ją uruchomić – opisuję w: [[Konfiguracja platformy n8n]])

---

## Krok 1: Rejestracja konta na platformie OpenRouter

Całą konfigurację rozpoczynamy od założenia konta na platformie OpenRouter, dostępnej pod adresem:
- https://openrouter.ai/

Na stronie głównej klikamy przycisk rejestracji/logowania. OpenRouter umożliwia założenie konta na trzy sposoby:
- **Google** – logowanie za pomocą konta Google
- **GitHub** – logowanie za pomocą konta GitHub
- **E-mail** – rejestracja klasyczna za pomocą adresu e-mail i hasła

![[screenshots-openrouter/openrouter-screenshot-placeholder-01.png]]

Wybieramy preferowaną metodę rejestracji i postępujemy zgodnie z wyświetlanymi instrukcjami. Po pomyślnej rejestracji zostaniemy przekierowani do głównego dashboardu platformy OpenRouter:
![[screenshots-openrouter/openrouter-screenshot-placeholder-02.png]]

> **Wskazówka:** Nowi użytkownicy otrzymują niewielką pulę darmowych kredytów, pozwalającą na przetestowanie platformy bez konieczności natychmiastowego doładowania konta.

---

## Krok 2: Doładowanie konta (Credits)

OpenRouter działa w modelu prepaid tj. przed rozpoczęciem korzystania z API musimy doładować konto odpowiednią kwotą kredytów (denominowanych w dolarach amerykańskich). Koszt każdego zapytania do modelu AI jest automatycznie odliczany od naszego salda.

Przechodzimy do zakładki **Credits** w panelu użytkownika (lub bezpośrednio pod adresem):
- https://openrouter.ai/credits

![[screenshots-openrouter/openrouter-screenshot-placeholder-03.png]]

**Doładowanie kredytów:**
1. Na stronie Credits klikamy przycisk doładowania.
2. Wpisujemy kwotę, którą chcemy wpłacić (minimalna kwota to **$5**).
3. Wybieramy metodę płatności i finalizujemy transakcję.

![[screenshots-openrouter/openrouter-screenshot-placeholder-04.png]]

**Dostępne metody płatności:**

| Metoda | Uwagi |
|---|---|
| Karty płatnicze (Visa, Mastercard itd.) | Obsługiwane przez Stripe |
| Kryptowaluty (USDC) | Dodatkowa opłata transakcyjna |
| AliPay | Dla użytkowników z Azji |

> **Warto wiedzieć:** OpenRouter nie nalicza marży na ceny dostawców modeli – płacimy dokładnie tyle, ile wynosi stawka danego dostawcy (np. OpenAI, Anthropic). Cennik poszczególnych modeli jest dostępny w katalogu: https://openrouter.ai/models

**Automatyczne doładowanie (Auto Top-Up):**

OpenRouter oferuje możliwość skonfigurowania automatycznego doładowania konta. Po włączeniu tej opcji platforma automatycznie zasili nasze saldo, gdy spadnie ono poniżej ustalonego progu – dzięki temu unikniemy sytuacji, w której nasze automatyzacje w n8n przestaną działać z powodu wyczerpania kredytów.

> **Ważne:** Zgodnie z regulaminem platformy – niewykorzystane kredyty mogą wygasnąć po upływie roku od daty zakupu. Zwrot środków jest możliwy wyłącznie w ciągu 24 godzin od dokonania transakcji.

---

## Krok 3: Wygenerowanie klucza API

Mając doładowane konto, możemy przystąpić do wygenerowania klucza API, który posłuży nam do uwierzytelniania zapytań z poziomu n8n.

Przechodzimy na stronę zarządzania kluczami API:
- https://openrouter.ai/keys

![[screenshots-openrouter/openrouter-screenshot-placeholder-05.png]]

**Tworzenie nowego klucza:**
1. Klikamy przycisk **Create Key**.
2. Nadajemy kluczowi opisową nazwę – np. `n8n-integration` (ułatwi to identyfikację klucza w przyszłości, szczególnie gdy będziemy posiadać ich więcej).
3. Opcjonalnie ustawiamy **Credit Limit** – limit kredytowy przypisany do tego konkretnego klucza. Pozwala to kontrolować wydatki per klucz (przydatne, gdy różne aplikacje korzystają z osobnych kluczy). Często rozwiązana oparte o LLM (Large Language Models) wpadają w nieskończoną pętle, co skutkuje wyzerowaniem dostępnych środków, dlatego tak istotne jest ustawienie limitu na np. 20$.
4. Klikamy **Create** w celu wygenerowania klucza.

![[screenshots-openrouter/openrouter-screenshot-placeholder-06.png]]

Po utworzeniu klucza zostanie on wyświetlony na ekranie:
![[screenshots-openrouter/openrouter-screenshot-placeholder-07.png]]

> **Ważne:** Klucz API wyświetlany jest **TYLKO RAZ** – po zamknięciu okna nie będziemy mogli go ponownie odczytać. Należy go natychmiast skopiować i zapisać w bezpiecznym miejscu, np. w aplikacji [1password](https://1password.com). W razie utraty klucza jedyną opcją jest wygenerowanie nowego.

---

## Krok 4: Konfiguracja poświadczeń OpenRouter w n8n

Jak uruchomić własną instancję n8n lub założyć darmowe konto na oficjalnej stronie producenta – szczegółowo ilustruję w tym wątku:
[[Konfiguracja platformy n8n]]

Platforma n8n oferuje natywny typ poświadczeń **OpenRouter**, co sprawia, że konfiguracja jest szybka i bezproblemowa – wystarczy wkleić klucz API.

### Sposób 1: Natywne poświadczenie OpenRouter (zalecany)

1. W n8n przechodzimy do zakładki **Credentials** i klikamy **Create credential** (bądź **Add first credential** – jeśli uruchamiamy świeżą instancję).
2. W wyświetlonym oknie wyszukujemy typ poświadczeń: **OpenRouter** i klikamy **Continue**.

![[screenshots-openrouter/openrouter-screenshot-placeholder-08.png]]

3. Uzupełniamy pole w formularzu:
   - **API Key** – klucz API, który wygenerowaliśmy w poprzednim kroku

![[screenshots-openrouter/openrouter-screenshot-placeholder-09.png]]

4. Klikamy **Save** w celu zapisania poświadczenia.

Warto w tym miejscu zwrócić uwagę, iż w wersji Cloud platformy n8n (dostępnej na https://n8n.io/) możemy posłużyć się wbudowanym asystentem AI (przycisk **n8n AI**) w celu uzyskania instrukcji dotyczących poprawnego uzupełnienia pól formularza. Funkcja ta nie jest dostępna w instancjach self-hosted.

### Sposób 2: Poświadczenie OpenAi API (alternatywny)

OpenRouter udostępnia API w pełni kompatybilne z formatem OpenAI – dzięki temu w n8n możemy również skorzystać z typu poświadczeń **OpenAi**, podmieniając jedynie adres bazowy (Base URL) na endpoint OpenRouter. Podejście to bywa przydatne, gdy chcemy korzystać z węzłów n8n zaprojektowanych stricte pod OpenAI.

1. W oknie tworzenia poświadczeń wyszukujemy typ: **OpenAi** i klikamy **Continue**.

![[screenshots-openrouter/openrouter-screenshot-placeholder-10.png]]

2. Uzupełniamy pola w formularzu:
   - **API Key** – klucz API OpenRouter
   - **Base URL** – zmieniamy domyślny adres na: `https://openrouter.ai/api/v1`

![[screenshots-openrouter/openrouter-screenshot-placeholder-11.png]]

3. Klikamy **Save** w celu zapisania poświadczenia.

> **Warto wiedzieć:** Nowe konta na platformie n8n Cloud mogą otrzymać **100 darmowych kredytów od OpenAI** – stosowny komunikat z przyciskiem **Claim credits** pojawi się w górnej części formularza poświadczeń OpenAi.

> **Wskazówka:** OpenRouter obsługuje opcjonalne nagłówki HTTP, które pozwalają identyfikować naszą aplikację w statystykach platformy:
> - `HTTP-Referer` – adres URL naszej aplikacji
> - `X-Title` – nazwa naszej aplikacji
>
> Dla zastosowań warsztatowych nie są one wymagane.

Po poprawnej konfiguracji (niezależnie od wybranego sposobu) powinniśmy móc korzystać z modeli AI dostępnych w katalogu OpenRouter bezpośrednio z poziomu węzłów (nodes) n8n – np. w węźle **AI Agent**, **Chat Model** czy **OpenAI**.

---

## Bezpieczeństwo kluczy API

Klucz API OpenRouter pełni rolę poświadczenia uwierzytelniającego – każdy, kto go posiada, może wykonywać zapytania do modeli AI na koszt naszego konta. Dlatego warto przestrzegać kilku zasad:

- **Nigdy nie commitujemy kluczy do publicznych repozytoriów** – OpenRouter współpracuje z GitHub w zakresie automatycznego skanowania sekretów. W przypadku wykrycia ujawnionego klucza w publicznym repozytorium otrzymamy powiadomienie e-mail, jednak lepiej zapobiegać niż leczyć.
- **Przechowujemy klucze w menedżerze haseł** – np. [1password](https://1password.com), Bitwarden lub KeePass.
- **Stosujemy zmienne środowiskowe** – jeśli używamy klucza w kodzie bądź skryptach, przechowujemy go w pliku `.env` (dodanym do `.gitignore`), a nie bezpośrednio w kodzie źródłowym.
- **Korzystamy z limitów kredytowych per klucz** – opcja Credit Limit dostępna podczas tworzenia klucza pozwala ograniczyć potencjalne straty w razie wycieku.
- **W razie podejrzenia wycieku danych** – natychmiast usuwamy klucz na stronie https://openrouter.ai/keys i generujemy nowy.

---

## Podsumowanie

W ramach tego poradnika wykonaliśmy kompletną konfigurację konta OpenRouter na potrzeby integracji z platformą n8n:

1. **Założyliśmy konto** na platformie OpenRouter – rejestrując się za pomocą konta Google, GitHub lub adresu e-mail.
2. **Doładowaliśmy kredyty** – zasilając konto odpowiednią kwotą, niezbędną do korzystania z API modeli językowych.
3. **Wygenerowaliśmy klucz API** – unikalny token uwierzytelniający, który skopiowaliśmy i zapisaliśmy w bezpiecznym miejscu.
4. **Skonfigurowaliśmy poświadczenie w n8n** – wykorzystując natywny typ OpenRouter (bądź alternatywnie OpenAi API z podmienionym Base URL na `https://openrouter.ai/api/v1`).

Kluczową zaletą OpenRouter jest dostęp do setek modeli AI przez jedno API – bez konieczności osobnego konfigurowania kont u każdego dostawcy. Tak przygotowane poświadczenie stanowi fundament dalszej pracy z n8n – będziemy z niego korzystać przy tworzeniu przepływów automatyzujących zadania z wykorzystaniem modeli językowych, agentów AI oraz narzędzi generatywnych.