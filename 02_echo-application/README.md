# Demo 1

Krok 1: Wdrożenie konfiguracji
Uruchom wszystkie komponenty w klastrze:
kubectl apply -f demo.yaml

Poczekaj chwilę, aż wszystkie PODy będą miały status Running:
kubectl get pods -n echo -w

Krok 2: Podgląd bazy danych "na żywo"
Aby pokazać, że dane naprawdę się zapisują, otwórz drugi terminal w O'Reilly (lub uruchom komendę w tle) i "podglądaj" plik logu w podzie bazy danych:
kubectl exec -n echo db-pod -- tail -f /tmp/chat.log

(Na razie będzie tu pusto).
Krok 3: Interakcja z poziomu PODa użytkownika (Test)
Teraz wejdź "do środka" podu klienckiego, aby zasymulować ruch użytkownika:
kubectl exec -it -n echo client-pod -- /bin/sh

Jesteś teraz wewnątrz kontenera klienta. Wyślijmy wiadomość tekstową przez TCP przy użyciu netcat bezpośrednio do serwisu echo-service:
echo "Czesc, to wiadomosc z pierwszego podu!" | nc echo-service 8080

Wynik w terminalu klienta:
Powinieneś natychmiast otrzymać odpowiedź zwrotną od serwisu Echo:
ECHO: Czesc, to wiadomosc z pierwszego podu!

(Uwaga: Ponieważ nasz uproszczony skrypt w echo-podzie przetwarza jedną linię tekstu i zamyka połączenie, curl wysyłający nagłówki HTTP mógłby zawiesić prostego netcata, dlatego do czystego TCP komenda nc jest w tym scenariuszu bezbłędna i niezawodna).
Krok 4: Weryfikacja zapisu w bazie danych
Wróć do terminala, w którym masz podgląd na pod bazy danych (db-pod). Zobaczysz, że pojawił się tam wpis:
Log: Czesc, to wiadomosc z pierwszego podu!

Punkty do omówienia:
	1.	Service Discovery: Pod kliencki nie musi znać adresu IP podu Echo. Używa nazwy DNS echo-service, a K8s sam wie, gdzie to przekierować.
	2.	Komunikacja wewnątrz klastra: Pod Echo bez problemu rozmawia z db-service na innym porcie.
	3.	Izolacja: Wszystko dzieje się wewnątrz dedykowanego namespace: echo, nie zakłócając pracy innych aplikacji w klastrze.
