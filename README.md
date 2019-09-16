# Rinnovo automatico di host noip.com da RPi

[noip.com](https://www.noip.com/) propone un servizio di free hosts che scade (e va quindi rinnovato) ogni mese.
Questo script simula il login alla pagina del servizio e il rinnovo tramite pulsante ad-hoc utilizzando Python/Selenium con Chrome headless mode.

**Progetto originale**: [loblab/noip-renew](https://github.com/loblab/noip-renew) ([articolo ufficiale sul blog (in cinese)](http://www.jianshu.com/p/3c8196175147)), modificato secondo quanto suggerito nel thread [github.com/loblab/noip-renew/issues/4](https://github.com/loblab/noip-renew/issues/4#issuecomment-496487647).

**Testato su RPi 3B+ con Raspbian 9**:

```bash
pi@raspberrypi:~ $ cat /etc/os-release
PRETTY_NAME="Raspbian GNU/Linux 9 (stretch)"
NAME="Raspbian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
ID=raspbian
ID_LIKE=debian
HOME_URL="http://www.raspbian.org/"
SUPPORT_URL="http://www.raspbian.org/RaspbianForums"
BUG_REPORT_URL="http://www.raspbian.org/RaspbianBugs"
```

## Utilizzo

- Modifica il file **noip-renew.sh** inserendo le tue credenziali di accesso a noip.com e specifica il numero di host che hai creato e che vuoi tenere attivi,
- Avvia **setup.sh**,
- Avvia **noip-renew.sh**, apri e verifica il file immagine *result.png* (se lo script termina senza errori) o *error.png* (se lo script termina con errori)

Il mio consiglio è quello di **configurare il tuo Crontab** per eseguire lo script di rinnovo a intervalli regolari, per evitare che il tuo host scada. Questo è l'esempio basato sulla mia attuale installazione:

```bash
# NoIP: Automatic Renewal
45 3 * * 1,3,5 /home/pi/Scripts/noip/noip-renew.sh
```

Verifica l'intervallo di esecuzione: [crontab.guru/#45_3_*_*_1,3,5](https://crontab.guru/#45_3_*_*_1,3,5).

**L'esecuzione dello script va fatta con i permessi dell'utente, non utilizzare `sudo` per scrivere nel Crontab!**

## Ulteriori informazioni

Lo script non si occupa di rinnovare l'indirizzo IP associato al tuo host DNS, per quello dai un'occhiata alla [documentazione su noip.com](https://www.noip.com/integrate). Molti degli attuali router supportano già nativamente noip.com.
Dai un'occhiata anche a [DNS-O-Matic](https://dnsomatic.com/) se hai necessità di aggiornare multipli record DNS ospitati da noip.com.

Non esiste un `chromedriver` nativo su Raspberry Pi. Lo script di installazione si occupa di installarne uno nel caso in cui non dovesse già trovarlo sul sistema.
