# Uwierzytelnianie klienta SSH za pomocą kluczy prywatnych

## Konfiguracja serwera


## Generowanie kluczy


## Instalacja kluczy dla wybranych klientów usług ssh


## Bezpieczny transfer plików z wykorzystaniem ssh
Wysłanie pliku:
scp -i C:\Users\vip\.ssh\id_rsa .\plik2.txt user@192.168.205.137:/home/user/przeslany2.txt

Pobranie pliku z serwera:
scp -i C:\Users\vip\.ssh\id_rsa user@192.168.205.137:/home/user/test.txt test.txt
