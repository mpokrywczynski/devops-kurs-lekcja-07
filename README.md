# Lekcja 7 - cwiczenie 9: srodowisko z Vagranta

Maszyna z serwerem WWW opisana jednym plikiem `Vagrantfile`.

## Co jest w Vagrantfile

- obraz: `bento/ubuntu-24.04`
- nazwa hosta: `lab-web`
- pamiec: 1 GB, 2 procesory
- siec prywatna: `192.168.56.10`
- port: `127.0.0.1:8081` na moim komputerze -> port 80 w maszynie
- provision: instaluje nginx i wstawia strone z moim imieniem

Port 8081 zamiast 8080, bo u mnie 8080 jest zajety przez Apache.

## Srodowisko

- Windows 11, VirtualBox 7.2.20, Vagrant 2.4.9
- komendy uruchamiane w PowerShell

## Uruchomienie

```powershell
vagrant up
curl.exe http://127.0.0.1:8081
```

Wynik:

```
<meta charset="utf-8"><h1>Maciej - lab-web z Vagranta</h1>
```

## Pomiary

Czas mierzylem tak:

```powershell
$start = Get-Date; vagrant up; (Get-Date) - $start
```

| Uruchomienie | Co bylo wczesniej | Czas |
|---|---|---|
| 1 | `vagrant destroy -f` i `vagrant box remove bento/ubuntu-24.04` | 1 min 52 s |
| 2 | tylko `vagrant destroy -f` | 1 min 27 s |

Po obu uruchomieniach strona wyglada tak samo.

## Dlaczego drugie uruchomienie bylo szybsze

Za pierwszym razem Vagrant musial pobrac box (obraz Ubuntu, ok. 0,6 GB).
`vagrant destroy -f` kasuje tylko maszyne, a box zostaje na dysku
(`vagrant box list`). Za drugim razem Vagrant od razu tworzyl maszyne z boxa,
ktory juz byl.

Reszta trwala tyle samo, bo maszyna jest za kazdym razem nowa:
start systemu i provision (apt-get update, instalacja nginx) wykonuja sie od nowa.

## Wniosek

Cale srodowisko jest w jednym pliku. Moge je skasowac i odtworzyc w poltorej
minuty i dostaje to samo.
