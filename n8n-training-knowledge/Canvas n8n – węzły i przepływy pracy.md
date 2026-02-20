## Canvas – wizualny edytor przepływów n8n

Canvas (pl. *kanwa*) to serce interfejsu n8n: wizualny obszar roboczy, na którym projektujesz przepływy pracy. Zamiast pisać kod, łączysz na nim bloki funkcjonalne, tj. **węzły**, w sekwencje tworzące gotową automatyzację.

---

## Elementy interfejsu Canvas

Canvas oferuje dwa typy obiektów:

- **Węzły** – podstawowe elementy konstrukcyjne każdego przepływu. Każdy nowo dodany węzeł pojawia się na kanwie i automatycznie łączy się z poprzedzającym go węzłem.
- **Notatki** – swobodnie pozycjonowane adnotacje, które możesz dodać w dowolnym miejscu kanwy, aby dokumentować przepływ bądź zostawić wskazówki dla innych.

---

## Węzły i ich typy

Węzły są konfigurowalne: każdy posiada własny panel ustawień, w którym definiujesz, co ma zrobić i z jakimi danymi ma operować. Ze względu na rolę w przepływie węzły dzielą się na trzy kategorie:

### Węzły wyzwalaczy (trigger nodes)

Wyzwalacz to zawsze pierwszy węzeł przepływu. Jego zadaniem jest nasłuchiwanie na zdarzenie i uruchomienie sekwencji pozostałych węzłów, gdy to zdarzenie nastąpi. Przykłady zdarzeń:
- nadejście wiadomości e-mail,
- pojawienie się nowego pliku w Google Drive,
- wywołanie webhooka przez zewnętrzną aplikację,
- nastanie określonej godziny (harmonogram).

### Węzły akcji (action nodes)

Węzły akcji łączą się z zewnętrznymi serwisami i wykonują na nich konkretne operacje: wysyłają e-mail, tworzą zadanie w Jirze, dodają wiersz do arkusza Google Sheets bądź wywołują dowolne zewnętrzne API przez węzeł **HTTP Request**.

### Węzły transformacji danych

Węzły transformacji danych operują wyłącznie na danych przepływających przez przepływ, nie komunikując się z zewnętrznymi serwisami. Filtrują, agregują, mapują, przekształcają bądź łączą dane, aby przygotować je w formacie oczekiwanym przez kolejne węzły.

---

## Czym jest przepływ pracy (workflow)?

Przepływ pracy to zestaw węzłów połączonych ze sobą, które wykonują określoną sekwencję czynności. W n8n możesz przechowywać wiele przepływów na swoim koncie, a każdy z nich może być niezależnie aktywowany bądź dezaktywowany.

**Jak działa przepływ w praktyce?**

1. **Węzeł wyzwalacza** przyjmuje dane inicjujące (np. treść nadchodzącej wiadomości e-mail).
2. Kolejne węzły **przetwarzają te dane**: filtrują, transformują bądź wzbogacają je o informacje z innych źródeł.
3. Węzły końcowe **wysyłają dane** do aplikacji zewnętrznych: bazy danych, systemu CRM, kanału Slacka.
4. Węzły logiczne (np. **If**, **Switch**) **sterują przepływem**: na podstawie wyników poprzednich kroków decydują, którą gałęzią sekwencja ma podążyć, bądź zatrzymują jej wykonanie.

---

## Przepływy aktywne i nieaktywne

Przepływ może być w jednym z dwóch stanów:

- **Aktywny**: nasłuchuje na zdarzenia i uruchamia się automatycznie, gdy wyzwalacz wykryje zdarzenie – niezależnie od tego, czy jesteś zalogowany do platformy. Przepływ działa tam, gdzie został wdrożony: na serwerze, w chmurze n8n bądź na lokalnej maszynie.
- **Nieaktywny**: można go uruchamiać wyłącznie ręcznie, np. podczas testowania bądź budowania.

Dobrze skonfigurowany, aktywny przepływ może działać przez wiele dni bez żadnej interwencji, wykonując swoją pracę całkowicie niezależnie od tego, czym zajmujesz się w danej chwili.

---

## Podsumowanie

Canvas to wizualny obszar roboczy n8n, w którym projektujesz automatyzacje, łącząc ze sobą węzły trzech typów: wyzwalacze, akcje i transformacje danych. Połączone węzły tworzą przepływ pracy, który po aktywacji działa autonomicznie i uruchamia się w odpowiedzi na zdarzenia w zewnętrznych aplikacjach.

W kontekście platformy n8n przepływ pracy to szereg węzłów połączonych ze sobą, które kolejno wykonują określone zadania: węzły początkowe przyjmują dane, węzły pośrednie przetwarzają je i przygotowują dla kolejnych etapów, węzły wyjściowe wysyłają dane do aplikacji zewnętrznych, a węzły sterujące decydują, czy po przetworzeniu należy wykonać kolejne kroki – na przykład wywołać kolejną aplikację lub zakończyć pracę. Taki zestaw węzłów realizujących określony ciąg czynności nazywamy właśnie przepływem. Na swoim koncie możemy mieć wiele takich obiektów, przy czym niektóre mogą być aktywne i czekać na uruchomienie tj. na wystąpienie określonego zdarzenia w aplikacjach zewnętrznych, np. nadejście e-maila czy pojawienie się pliku na Google Drive – co wyzwala całą sekwencję działań, nawet jeśli nie jesteśmy zalogowani na komputerze ani w koncie n8n. Przepływy uruchamiają się tam, gdzie zostały zdefiniowane, i jeśli są poprawnie skonfigurowane, możemy nie zaglądać do nich przez wiele dni, a one będą wykonywać swoją pracę całkowicie niezależnie od tego, czym w danym momencie się zajmujemy.