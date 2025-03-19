Tutte le operazioni possono essere eseguite direttamente sul Dispositivo Android stesso, ma digitare i comandi Linux ed eseguire l'editing del testo su di un TouchScreen è quasi impossibile. Personalmente vi suggerirei di usare un **metodo alternativo** per inserire i comandi. 

## [Metodo Alternativo]

Utilizza uno dei metodi alternativi qui elencati o combinali tra loro (ad esempio, io utilizzo la combinazione tra **SCRCPY** e **OpenSSH** )

- Utilizzare [SCRCPY](https://github.com/Genymobile/scrcpy) per controllare il Dispositivo Android da PC (Windows, Linux, MacOS)

- Collegamento SSH tra Dispositivo Android e Computer (Windows, Linux, MacOS)

- Utilizzare una Tastiera BT con il Dispositivo Android

# <mark>Cosa fare al primo avvio di Termux</mark>

**Cosa fare al primo avvio di Termux**, lo ritengo un passaggio necessario dato che poi questi pacchetti quasi sicuramente ci serviranno. Percui perchè non installarli sin da subito?  

- Aggiornare i Repository ed il Sistema base minimale di Termux 

- Installare i Repository Extra

- Installare pacchetti aggiuntivi Termux e Nala (fronted di APT-PKG)

- Installare OpenSSH, inserire una nuova password per il (fake) root

- Avviare OpenSSH, per connessioni sicure tra Dispositivi

- Installare i binari per Git, Wget e Curl

- Installare alcuni pacchetti extra (opzionale)

- Abilitare la condivisione della memoria interna del Dispositivo Android con l'ambiente Termux

- Installare Train e Neofetch

```
pkg update && pkg upgrade 
```

```
pkg install root-repo x11-repo && pkg install tur-repo && pkg install nala
```

**Nala (fronted di APT-PKG)** - A volte può risultare difficile capire cosa sta facendo APT-PKG mentre installa o aggiorna un pacchetto. L’obiettivo di Nala è semplificare l’uso di APT-PKG rimuovendo alcuni messaggi ridondanti, migliorando la formattazione del pacchetto e utilizzando i colori per illustrare cosa accade ad un pacchetto durante l’installazione, la rimozione o l’aggiornamento.

```
nala install termux-tools termux-services termux-auth
```

```
nala install openssh && passwd
```

```
sshd
```

```
nala install git wget curl 
```

```
nala install nmap mc bc ncurses-utils binutils
```

```
termux-setup-storage
```

```
nala install sl neofetch
```

<a href="https://www.youtube.com/watch?v=vJiY9_Ca0wg">Video Preview di quello descritto in questa pagina</a>
