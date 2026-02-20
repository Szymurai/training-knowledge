Features:
- Notatki
- Węzły (automatycznie łączą się z poprzednimi węzłami)
	- Węzły są konfigurowalne 
	- Węzły dzielą się na:
		- Węzły wyzwalaczy
		- Węzły akcji
			- Łączenia się z zewnętrznymi serwisami
		- Węzły transformacji danych
W kontekście platformy n8n przepływ pracy to szereg węzłoów polaczonych ze sobe ktore jeden po drugim wukonują jakas prace albo przyjmuja jakies dane, tak jak wezel poczatkowy, albo przetwarzaja w jakis sposob te dane dane zeby przygotowac je do wezlow nastpenych albo wyslaja te dane do aplikacji zewnetrznych albo w niektorych wypadkach steruja przeplywem i decyduzja czy po przetworzeniu poprzednich wezlow naklezy cos jeszcze zrobic np. wywolaac kolejna aplikacje badx zakonczyc prace, taki zestaw wezlow wykonujacych okreslany zestaw czynnosci nazywamy wlasnie przeplywem – takich obiektow mozemy miec na swoim koncie wiele przy czym niektore moga byc aktywne i czekac na uruchomienie czyli wystapienie jakiegos zdarzenia w aplikacjch zenwtrznych np. nadejscie maiala, pojawiania sie pliku na google drive, co w efekcie wywoluje cala sekwencje czynnosci, nawet jesli nie jestemy zalogowaniu doi naszego komputera czy konta n8n. Uruchasmiaja sie tam gdzie zostaly zdefiniowane i jesli nsa dobrze skofngurowane mozemy tam nie zalgadac przez wiele dnbi, a one beda wykonywac swoaja robote calkowiec ie niezalenie od tego czym my sie w tym momencie zajmujemy. 