# Uwierzytelnianie klienta SSH za pomocą kluczy prywatnych

## Konfiguracja serwera


## Generowanie kluczy
### PuTTY
1. Otwórz program PuTTYgen
2. Kliknij przycisk 'generate'
3. Poruszaj kursorem myszki nad pustym obszarem podpisanym 'Key' w celu wygenerowania pary kluczy
4. Kliknij przycisk 'Save public key', wybierz nazwę i lokalizację pliku, żeby zapisać klucz publiczny
5. Kliknij przycisk 'Save private key', wybierz passphrase* oraz nazwę i lokalizację pliku, żeby zapisać klucz prywatny
*passphrase jest polem opcjonalnym zapewniającym większe bezpieczneństwo klucza prywatnego poprzez szyfrowanie, ale wymaga podania przy każdym jego użyciu


## Instalacja kluczy dla wybranych klientów usług ssh
#### Instalacja klucza publicznego
1. Sprawdź czy na serwerze z którym chcesz się połączyć w katalogu home znajduje się ukryty katalog '.ssh' z plikiem 'authorized_key' w środku. Jeśli tak, postępuj dalej.

W przeciwnym wypadku wykonaj w katalogu home:
```
mkdir .ssh
cd .ssh
touch authorized_keys
```
2. Przekopiuj klucz publiczny do środka pliku authorized_keys*
*w pliku może się znajdować więcej niż jeden klucz publiczny

Klucz publiczny można przekopiować do środka authorized_keys na kilka sposobów:
1. Ręcznie wprowadzając klucz do pliku
2. Korzystając z komendy scp od strony użytkownika przekopiowując cały plik na stronę serwera
3. Korzystając z komendy ssh-copy-id na systemie Linux
4. Korzystając ze [skryptu-substytutu dla ssh-copy-id](https://gist.github.com/ceilfors/fb6908dc8ac96e8fc983) na systemie Windows
#### Instalacja klucza prywatnego
##### PuTTY
1. Otwórz program PuTTY
2. Z menu po lewej stronie wybierz Connection>SSH>Auth
3. Obok pustego pola 'Private key file for authentication' kliknij przycisk 'Browse' i wybierz klucz prywatny
4. W pole 'Host Name (or IP address)' wpisz adres IP serwera z jakim chcesz się połączyć
5. Kliknij przycisk 'Open'
##### Windows SSH
Klucz prywatny zlokalizowany w domyślnym katalogu:
```
ssh user@host
```
Klucz prywatny lokalizowany w innym katalogu:
```
ssh -i <lokalizacja klucza> user@host
```
user - nazwa użytkownika z jakim się łączymy

host - adres IP serwera z jakim się łączymy


## Bezpieczny transfer plików z wykorzystaniem ssh

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
scp -i C:\Users\<winuser>\.ssh\id_rsa <ścieżka/nazwa_wysyłanego pliku> <user>@<ip_serwera>:/home/<user>/<nazwa_przeslanego_pliku>

lub jeśli klucz został wygenerowany w domyślnym katalogu:
scp <ścieżka/nazwa_wysyłanego pliku> <user>@<ip_serwera>:/home/<user>/<nazwa_przeslanego_pliku>

Bezpieczne pobranie pliku z serwera:
scp -i C:\Users\<winuser>\.ssh\id_rsa <user>@<ip_serwera>:/home/<user>/<ścieżka/nazwa_pobieranego_pliku> <ścieżka/nazwa_pobranego_pliku>

lub jeśli klucz został wygenerowany w domyślnym katalogu:
scp <user>@<ip_serwera>:/home/<user>/<ścieżka/nazwa_pobieranego_pliku> <ścieżka/nazwa_pobranego_pliku>

Dlaczego warto przeprowadzić bezpieczny transfer plików przez scp?
SCP do transferu danych używa SSH. W przeciwieństwie do np. FTP, SCP szyfruje połączenie, dzięki czemu jeśli ktoś będzie podsłuchiwał połączenie, nie będzie w stanie odczytać przechwyconych danych.
  
## Ciekawostki
  Jak zdjąć hasło z klucza?
  
  Wystarczy użyć polecenia:
  ssh-keygen -p
  
  Następnie należy podać hasło do klucza, a nowe hasło zostawić puste.
  
  Jak to wygląda w konsoli:
  
  C:\Users\vip>ssh-keygen -p
  Enter file in which the key is (C:\Users\<user>/.ssh/id_rsa):
  Enter old passphrase:
  Key has comment '<user>@DESKTOP-17P39QL'
  Enter new passphrase (empty for no passphrase):
  Enter same passphrase again:
  Your identification has been saved with the new passphrase.
  
