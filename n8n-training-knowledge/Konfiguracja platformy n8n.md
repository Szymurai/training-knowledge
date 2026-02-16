## Czym jest n8n i co potrafi?
Jest to platforma low-code do automatyzacji przepływów pracy, w szczególności powtarzalnych zadań w przedsiębiorstwie. Pozwala w przystępny sposób zautomatyzować procesy obejmujące wiele zewnętrznych serwisów.

---
## Jak poprawnie skonfigurować platformę automatyzującą n8n?
Aplikację n8n, ze względu na sposób uruchamiania, możemy podzielić na:
- Cloud – wersję uruchamianą przez producenta (n8n GmbH). W tej wersji nie tracisz czasu na instalację i aktualizację oprogramowania ani na konfigurację środowiska programistycznego. Usługa płatna posiadająca 14-dniowy okres próbny, za pomocą którego możesz zapoznać się z platformą.
- Self-hosted – wersję uruchamianą na własnym komputerze bądź infrastrukturze albo u zewnętrznych dostawców usług chmurowych (np. AWS, Azure, GCP). Wówczas potrzebujesz repozytorium zawierający kod źródłowy oprogramowania: https://github.com/n8n-io/n8n. Za pomocą powyższego repozytorium możesz zainstalować platformę na własnym środowisku.

Aplikację n8n możemy uruchomić na kilka sposobów, w zależności od wersji, którą wybierzemy:
- Zakładamy konto na oficjalnej stronie n8n (wersja Cloud).
- Wybieramy hosting u zewnętrznego dostawcy, np. https://railway.com.
- Uruchamiamy wersję self-hosted lokalnie tj. bezpośrednio w systemie operacyjnym (np. przez npm).
- Uruchamiamy wersję self-hosted lokalnie: w środowisku kontenerowym, np. za pomocą Docker Desktop (dostępne na macOS, Linuksie oraz Windowsie).

W tym poradniku zilustrujemy trzy sposoby uruchomienia n8n: przez oficjalną stronę (wersja Cloud), lokalnie w systemie operacyjnym (self-hosted) oraz za pomocą Dockera. Uruchamianie wersji self-hosted u zewnętrznych dostawców różni się w zależności od wybranego usługodawcy, dlatego ten wariant omawiamy indywidualnie – na życzenie kursantów.

---
## Konfiguracja konta na platformie https://n8n.io (wersja Cloud):
![[screenshots-n8n/n8n-screenshot-20260209183411.png]]
W pierwszym kroku klikamy przycisk „Get Started", a następnie po przeniesieniu na kolejną stronę – wpisujemy swój adres e-mail, na który zostanie zarejestrowane konto. Po upewnieniu się, że adres jest poprawny, klikamy „Submit".
![[screenshots-n8n/n8n-screenshot-20260209183758.png]]
W celu weryfikacji adresu e-mail na podany adres zostanie wysłany kod weryfikacyjny. Wpisujemy go i klikamy „Submit":
![[screenshots-n8n/n8n-screenshot-20260209183857.png]]
Po założeniu konta platforma przekieruje nas do widoku Dashboard:
![[screenshots-n8n/n8n-screenshot-20260209184636.png]]
Aby przejść do nowo utworzonej instancji, klikamy „Open Instance". Naszym oczom ukaże się serce n8n tj. miejsce, w którym zaprojektujemy pierwszy przepływ automatyzujący pracę.

---

## Konfiguracja n8n bezpośrednio w systemie operacyjnym (bez konteneryzacji).
Aby uruchomić n8n bezpośrednio w systemie operacyjnym, musimy najpierw zainstalować środowisko Node.js (w wersji 18 lub nowszej).

Instalacja środowiska Node.js w systemie Windows:
[[Konfiguracja środowiska deweloperskiego tj. Node.js]]

Następnie otwieramy terminal i instalujemy n8n globalnie za pomocą polecenia:

`npm install n8n -g`

Po zakończeniu instalacji uruchamiamy aplikację poleceniem:

`n8n start`

Aplikacja wystartuje na domyślnym porcie `5678`. Aby uzyskać do niej dostęp, otwieramy przeglądarkę i wpisujemy adres http://localhost:5678. Przy pierwszym uruchomieniu zostaniemy poproszeni o utworzenie konta administratora.

Ta metoda instalacji jest najprostsza, ale wymaga samodzielnego zarządzania aktualizacjami —
każdorazowo za pomocą polecenia `npm update n8n -g`.

---
## Uruchomienie wersji self-hosted za pomocą Docker Desktop

Aplikacja działa w kontenerze, odizolowana od systemu operacyjnego hosta. Aby rozpocząć, należy mieć zainstalowanego Dockera. Więcej o konfiguracji Docker Desktop znajdziesz w poradniku: [[Konfiguracja oprogramowania Docker Desktop]].

#### Krok 1: Utworzenie katalogu na dane lokalne

Utwórz katalog, w którym n8n będzie przechowywał dane (przepływy, poświadczenia, ustawienia):

```bash
mkdir ~/.n8n
```

#### Krok 2: Nadanie odpowiednich uprawnień

Zmień uprawnienia katalogu, aby kontener miał do niego dostęp:

```bash
chmod 755 ~/.n8n
```

> `chmod 755` daje właścicielowi pełne uprawnienia, a pozostałym użytkownikom jedynie odczyt i wykonywanie. Unikaj `chmod 777` — nadaje ono pełne uprawnienia wszystkim, co stanowi zagrożenie bezpieczeństwa.

#### Krok 3: Uruchomienie kontenera

Uruchom n8n w tle, wpisując w terminalu:

```bash
docker run -d --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n -e N8N_SECURE_COOKIE=false n8nio/n8n:latest
```

Oczekiwany wynik: Docker pobierze obraz i zwróci identyfikator kontenera:

```
Unable to find image 'n8nio/n8n:latest' locally
latest: Pulling from n8nio/n8n
...
Status: Downloaded newer image for n8nio/n8n:latest
a1b2c3d4e5f6...
```

Omówienie flag:

- `-d` — uruchamia kontener w tle ("tryb daemonowy"),
- `--name n8n` — nadaje kontenerowi nazwę `n8n` (ułatwia późniejsze zarządzanie),
- `-p 5678:5678` — mapuje port kontenera na port hosta,
- `-v ~/.n8n:/home/node/.n8n` — montuje lokalny katalog jako wolumen, dzięki czemu dane przetrwają restart kontenera,
- `-e N8N_SECURE_COOKIE=false` — wyłącza wymóg HTTPS dla ciasteczek, co upraszcza lokalne testowanie,
- `n8nio/n8n:latest` — obraz n8n z repozytorium Docker Hub.

#### Krok 4: Weryfikacja uruchomienia

Sprawdź, czy kontener działa:

```bash
docker ps
```

Oczekiwany wynik:

```
CONTAINER ID   IMAGE              STATUS         PORTS                    NAMES
a1b2c3d4e5f6   n8nio/n8n:latest   Up 2 minutes   0.0.0.0:5678->5678/tcp   n8n
```

Jeśli kontener `n8n` widnieje na liście ze statusem `Up`, serwer działa poprawnie.

#### Krok 5: Dostęp do aplikacji

Otwórz przeglądarkę i przejdź pod adres:

```
http://localhost:5678
```

Przy pierwszym uruchomieniu zostaniesz poproszony o utworzenie konta administratora. Ankietę wstępną można pominąć, aby od razu przejść do tworzenia przepływów.

#### Krok 6: Zatrzymanie i ponowne uruchomienie kontenera

Zatrzymanie:

```bash
docker stop n8n
```

Ponowne uruchomienie (dane zostają zachowane dzięki zamontowanemu wolumenowi):

```bash
docker start n8n
```

Podgląd logów (przydatny przy diagnozowaniu problemów):

```bash
docker logs -f n8n
```

---

## Podsumowanie

W ramach tego poradnika przedstawiliśmy trzy sposoby uruchomienia platformy n8n:

1. **Wersja Cloud (n8n.io)**: najprostszy wariant, nie wymagający instalacji. Wystarczy założyć konto na oficjalnej stronie, zweryfikować adres e-mail i otworzyć gotową instancję.
2. **Wersja self-hosted (npm)**: instalacja bezpośrednio w systemie operacyjnym za pomocą `npm install n8n -g`. Wymaga środowiska Node.js w wersji 18 lub nowszej.
3. **Wersja self-hosted (Docker)**: uruchomienie n8n w kontenerze Docker z zamontowanym wolumenem na dane. Zapewnia izolację od systemu hosta i łatwość aktualizacji.

Każdy z wariantów prowadzi do tego samego rezultatu tj. działającej instancji n8n dostępnej pod adresem `http://localhost:5678` (w przypadku wersji self-hosted) lub w panelu n8n Cloud. Wybór zależy od preferencji i wymagań dotyczących infrastruktury.
