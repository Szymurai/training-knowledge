# Jak poprawnie skonfigurować dane poświadczające (Credentials) w aplikacji Jira?

Na wstępie – w zależności od typu instancji Jiry, którą dysponujemy (w swoim przedsiębiorstwie bądź prywatnie) powinniśmy zalogować się na własne konto w celu wygenerowania tokenu API, który następnie zapiszemy w banku poświadczeń (Credentials) na platformie n8n.

Proces generowania tokenu API zilustruję na przykładzie wersji Cloud, ponieważ wersja Server w n8n wymaga podania jedynie nazwy użytkownika i hasła (token API nie jest w niej wymagany).

**Logowanie się do Jira Cloud Account**:
- Przechodzimy na stronę https://id.atlassian.com/login – po czym logujemy się odpowiednim adresem e-mail (powiązanym z naszym kontem).
![[ScreenShot Tool -20260209094449.png]]
Następnie, po zalogowaniu się, przechodzimy do widoku generowania tokenów API tj. `Zarządzaj ustawieniami konta > Bezpieczeństwo > Tokeny API`:
![[ScreenShot Tool -20260209100346.png]]
W celu wygenerowania nowego tokenu klikamy na:
- [Utwórz tokeny API i nimi zarządzaj](https://id.atlassian.com/manage-profile/security/api-tokens)
Prawdopodobnie zostaniemy poproszeniu o ponowną weryfikację tj.
![[ScreenShot Tool -20260209100713.png]]
Wówczas powinniśmy zalogować się na naszą skrzynkę pocztową i wpisać wymagany kod weryfikacyjny. 
Po poprawnej weryfikacji zostaniemy przeniesieni na stronę, gdzie za pomocą przycisku:
- Utwórz token API
możemy wygenerować nowy klucz poświadczający:
![[ScreenShot Tool -20260209101156.png]]
Ważne żeby w tym miejscu skopiować nowo utworzony token, a następnie zapisać go w bezpieczne miejsce np. do aplikacji [1password](https://1password.com).

---
## Jak skonfigurować poświadczenia Jira na platformie n8n?
Jak uruchomić własną instancję n8n lub założyć darmowe konto na oficjalnej stronie producenta – szczegółowo ilustruję w tym wątku:
[[Konfiguracja platformy n8n]]
Aby poprawnie połączyć się z Jirą z poziomu n8n, należy utworzyć nowe poświadczenia (Credentials). W tym celu przechodzimy na zakładkę „Credentials", klikamy pomarańczowy przycisk „Create Credentials" bądź "Add first credential" – jeśli uruchamiamy świeżą instancję oprogramowania, a następnie w wyświetlonym oknie wyszukujemy typ poświadczeń przeznaczony dla Jiry, tj. Jira Software Cloud API, i klikamy „Continue":
![[ScreenShot Tool -20260209185224.png]]![[ScreenShot Tool -20260209185326.png]]
W tym miejscu pojawi się popup
![[ScreenShot Tool -20260209185431.png]]
Którego pola powinniśmy odpowiednio wypełnić tj.
- Adres e-mail, którym logowaliśmy się do Jira Cloud Account
- API Token – token, który wygenerowaliśmy w poprzednich krokach
- Domain

Warto w tym miejscu zwrócić uwagę, iż w wersji utrzymywanej przez oficjalną stronę https://n8n.io/ Możemy posłużyć się wbudowaną Generatywną AI w celu poprawnego uzupełnienia danych:
![[ScreenShot Tool -20260215173727.png]]