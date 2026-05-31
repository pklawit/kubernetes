Krok 1: Sprawdzenie stanu początkowego (Ruch działa)
Zanim wdrożysz politykę, upewnij się (lub przypomnij widzom), że aplikacja echo wciąż może pisać do bazy danych.
Wejdź do podu klienta i wyślij wiadomość:
kubectl exec -it -n echo client-pod -- /bin/sh
# Wewnątrz podu:
echo "Wiadomosc przed blokada" | nc echo-service 8080
exit

Sprawdź logi bazy danych – wpis Log: Wiadomosc przed blokada powinien się pojawić.
Krok 2: Wdrożenie blokady sieciowej
Teraz aplikujesz przygotowaną politykę sieciową, która odcina ruch do bazy danych:
kubectl apply -f networkpolicy1.yaml

Krok 3: Test po zablokowaniu ruchu
Ponownie wchodzisz do podu klienta i próbujesz wysłać kolejną wiadomość:
kubectl exec -it -n echo client-pod -- /bin/sh
# Wewnątrz podu:
echo "Wiadomosc w trakcie blokady" | nc echo-service 8080

Co się stanie na ekranie (Wynik)?
Komenda w terminalu klienta "zawiesi się" na dłuższą chwilę (nastąpi tzw. timeout).
Dlaczego tak się dzieje? (Wyjaśnienie dla widzów):
	1.	Klient wysyła wiadomość do echo-service. To połączenie nadal działa, ponieważ nie zablokowaliśmy ruchu do podu Echo.
	2.	Pod Echo odbiera tekst, a następnie próbuje wykonać linijkę kodu: echo "Log: ..." | nc db-service 5432.
	3.	W tym momencie NetworkPolicy na poziomie warstwy sieciowej klastra przechwytuje pakiety idące do bazy i po cichu je porzuca (drop).
	4.	Pod Echo czeka na potwierdzenie nawiązania połączenia TCP z bazą, a klient czeka na odpowiedź od podu Echo. Cały łańcuch zostaje wstrzymany.
Jeśli sprawdzisz logi w podzie bazy danych (kubectl exec -n echo db-pod -- tail /tmp/chat.log), zobaczysz, że nowa wiadomość już tam nie trafiła.
Krok 4 (Opcjonalny): Jak to naprawić? (Zezwolenie konkretnemu podowi)
Jeśli chcesz pokazać pełnię możliwości, możesz zmodyfikować politykę, aby pozwalała na ruch do bazy danych tylko i wyłącznie podowi echo-app, nadal blokując wszystkich innych.
Zastąp zawartość polityki poniższym YAML-em (kubectl apply -f ...):
a

Po zaaplikowaniu tej poprawki, wysłanie wiadomości z poziomu klienta do serwisu Echo znów zacznie działać, ale gdyby ktoś spróbował włamać się i połączyć z bazy bezpośrednio z poziomu client-pod do db-service (omijając aplikację Echo), ruch zostanie zablokowany.
