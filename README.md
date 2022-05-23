# Uwierzytelnianie klienta SSH za pomocą kluczy prywatnych

### Konfiguracja serwera


### Generowanie kluczy


### Instalacja kluczy dla wybranych klientów usług ssh


### Bezpieczny transfer plików z wykorzystaniem ssh

Krok 1: Wygenerowanie klucza na Windowsie:
ssh-keygen -t rsa -b 4096
scp C:\Users\<winuser>\.ssh\id_rsa.pub <user>@<ip_serwera>:/home/<user>

Krok 2: Instalacja klucza na serwerze z systemem Linux:
actual path: /home/user
mkdir ~/.ssh
cat id_rsa.pub >> .ssh/authorized_keys

Krok 3: Przesyłanie plików

SCP (secure copy) to narzędzie pozwalające na bezpieczne kopiowanie plików i katalogów pomiędzy zdalnymi hostami.

Składnia polecenia:
scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2

Bezpieczne przesłanie pliku z hosta na serwer:
scp -i C:\Users\<winuser>\.ssh\id_rsa .\plik2.txt <user>@<ip_serwera>:/home/<user>/przeslany2.txt

lub jeśli klucz został wygenerowany w domyślnym katalogu:
scp .\plik2.txt <user>@<ip_serwera>:/home/<user>/przeslany2.txt

Bezpieczne pobranie pliku z serwera:
scp -i C:\Users\<winuser>\.ssh\id_rsa <user>@<ip_serwera>:/home/<user>/test.txt test.txt

lub jeśli klucz został wygenerowany w domyślnym katalogu:
scp <user>@<ip_serwera>:/home/<user>/test.txt test.txt

Dlaczego warto przeprowadzić bezpieczny transfer plików przez scp?
SCP do transferu danych używa SSH. W przeciwieństwie do np. FTP, SCP szyfruje połączenie, dzięki czemu jeśli ktoś będzie podsłuchiwał połączenie, nie będzie w stanie odczytać przechwyconych danych.
