## Problem: „n8n is not recognized" po instalacji przez npm

Jeśli po wpisaniu `n8n start` w terminalu pojawia się komunikat:

```
'n8n' is not recognized as an internal or external command
```

najprawdopodobniej instalacja nie powiodła się z powodu braku uprawnień bądź terminal nie widzi jeszcze zainstalowanej paczki. Poniżej znajdziesz kroki, które w większości przypadków rozwiązują problem.

---

## Krok 1: Uruchom CMD jako administrator i powtórz instalację

Zamknij dotychczasowe okno terminala. Następnie wyszukaj **cmd** w menu Start, kliknij prawym przyciskiem myszy i wybierz **Uruchom jako administrator**:

![[screenshots-n8n/n8n-cmd-admin-placeholder.png]]

W oknie terminala z uprawnieniami administratora wpisz ponownie polecenie instalacyjne:

```cmd
npm install n8n -g
```

Poczekaj na zakończenie instalacji, a następnie **zamknij okno terminala**.

---

## Krok 2: Otwórz nowe okno CMD i spróbuj uruchomić n8n

Otwórz nowe okno terminala (tym razem może być zwykłe, bez uprawnień administratora) i wpisz:

```cmd
n8n start
```

Jeśli polecenie zostało rozpoznane i n8n uruchamia się — gotowe. Otwórz przeglądarkę i przejdź pod adres `http://localhost:5678`.

---

## Jeśli nadal nie działa

Jeśli po ponownej instalacji i restarcie terminala problem nie ustąpił, są dwie ścieżki wyjścia:

**Opcja A: npx (szybkie rozwiązanie na teraz)**

Zamiast `n8n start` wpisz:

```cmd
npx n8n start
```

Polecenie `npx` uruchamia paczki zainstalowane globalnie przez npm, omijając problem z PATH. n8n wystartuje dokładnie tak samo.

**Opcja B: dodanie n8n do PATH (rozwiążemy na kolejnych zajęciach)**

Jeśli żadna z powyższych metod nie pomogła bądź chcesz, aby polecenie `n8n` działało bez `npx`, konieczne jest ręczne dodanie folderu npm do zmiennej środowiskowej PATH. Omówimy ten krok na kolejnych zajęciach.

---

## Podsumowanie

| Krok | Działanie |
|---|---|
| 1 | Uruchom CMD jako administrator |
| 2 | Wykonaj `npm install n8n -g` ponownie |
| 3 | Zamknij terminal, otwórz nowe okno |
| 4 | Wpisz `n8n start` |
| Alternatywa | Wpisz `npx n8n start` |
