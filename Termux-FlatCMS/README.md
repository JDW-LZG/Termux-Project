Tutte le oprazioni possono essere eseguite direttamente sul Dispositivo Android stesso, ma digitare i comandi Linux ed eseguire l'editing del testo su di un TouchScreen è quasi impossibile. Personalmente vi suggerirei di usare un **metodo alternativo** per inserire i comandi. 

## [Metodo Alternativo]

Utilizza uno dei metodi alternativi qui elencati o combinali tra loro (ad esempio, io utilizzo la combinazione tra **SCRCPY** e **OpenSSH** )

- Utilizzare [SCRCPY](https://github.com/Genymobile/scrcpy) per controllare il Dispositivo Android da PC (Windows, Linux, MacOS)

- Collegamento SSH tra Dispositivo Android e Computer (Windows, Linux, MacOS)

- Utilizzare una Tastiera BT con il Dispositivo Android

  NB. Presumo tu abbia fatto le operazioni iniziali in Termux(quello che chiamo <a href="https://github.com/JDW-LZG/Termux-Project/blob/main/Primo-Avvio.md">Primo Avvio</a>)

# <mark>Termux FlatCMS</mark>

###### Cosa ci serve per Termux FlatCMS

- Installare Apache Server e PHP

- Creare la cartella htdocs e 2 file

- Configurare Apache Server e PHP

- Installare un FlatCMS

```
nala install php php-apache
```

```
mkdir $HOME/storage/shared/htdocs
```

Per la creazione e modifica dei file ci serviremo di nano. Nano è un'editor di testo della Vecchia Scuola, per muoverci in `nano` dobbiamo usare i tasti freccia, per salvare CTRL+O e premere Invio, e CTRL+X per uscire da nano.

- Creare il file `index.html` nella cartella **htdocs**

```
nano $HOME/storage/shared/htdocs/index.html
```

Ed inserisci quanto segue

```
<body color="#000000" align="center"><font size="20" color="#ba372a" size="24.4px">
    <b>Apache Server avviato con successo</b></font>
<table style="border-collapse: collapse; width: 100%;" border="0">
    <colgroup><col style="width: 50%;"><col style="width: 50%;"></colgroup>
    <tbody>
        <tr>
            <td><a href="localhost:8080/phpinfo/" target="_blank" rel="noopener">Informazioni PHP</a></td>
            <td><a href="localhost:8080" target="_blank" rel="noopener">Home</a></td>
        </tr>
    </tbody>
</table>
</body>
```

Salvare il file `index.html` ed uscire da nano. (per salvare CTRL+O e premere Invio, e CTRL+X per uscire)

- Creare il file `phpinfo.php` nella cartella **htdocs**

```
nano $HOME/storage/shared/htdocs/phpinfo.php
```

Ed inserisci quanto segue

```
<?php phpinfo(); ?>
```

Salaver il file `phpinfo.php` ed uscire da nano. (per salvare CTRL+O e premere Invio, e CTRL+X per uscire)

- Configurare Apache e PHP

Modificare il file di configurazione `httpd.conf` di Apache

```
nano $PREFIX/etc/apache2/httpd.conf
```

Scorri la pagina verso il basso fino all'elenco dei comandi di LoadModule. Dobbiamo disabilitarne uno, abilitare una coppia ed aggiungerne uno. 

Per **disabilitare** un comando devi aggiungere all'inizio della riga `#` 

Per **abilitare** un comando devi rimuovere dall'inizio della riga `#`

Abilitare

```
#LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so
```

Disabilitare

```
LoadModule mpm_worker_module libexec/apache2/mod_mpm_worker.so
```

Abilitare

```
#LoadModule rewrite_module libexec/apache2/mod_rewrite.so
```

Aggiungere (subito dopo il comando appena abilitato)

```
LoadModule php_module /data/data/com.termux/files/usr/libexec/apache2/libphp.so
```

Continua a scorrere verso il basso sino a quando non trovi la sezione `DcoumentRoot`

```
DocumentRoot "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs"
<Directory "/data/data/com.termux/files/usr/share/apache2/default-site/htdocs">
```

Avendo creato precedentemente la cartella `htdocs` all'interno della memoria del dispositivo android, dobbiamo scrivere

```
DocumentRoot "/sdcard/htdocs"
<Directory "/sdcard/htdocs">
```

Scorrere qualche riga piu sotto e `AllowOverride none` diventa

```
AllowOverride FileInfo
```

Immediatamente sotto puoi osservare il blocco

```
<IfModule dir_module>
  DirectoryIndex index.html 
</IfModule>
```

Aggiungere index.php accanto ad index.html e immediatamente subito dopo aggiungere questo blocco

```
<FilesMatch \.php$>
    SetHandler application/x-httpd-php
</FilesMatch>
```

Salvare il file  **httpd.conf** ed uscire da nano. (per salvare CTRL+O e premere Invio, e CTRL+X per uscire)

Ora dobbiamo creare il file di configurazione per PHP, accedere alla cartella `lib` e,  creare e modificare il file di configurazione php.ini

```
touch $PREFIX/lib/php.ini && nano $PREFIX/lib/php.ini
```

Qui dobbiamo aggiungere 2 righe

```
[PHP]
engine=On
file_uploads=On
upload_max_filesize=32M
post_max_size=32M
allow_url_fopen=On
allow_url_include=Off

[Date]
date.timezone=Europe/Rome
```

Salvare e uscire da `php.ini` (per salvare CTRL+O e premere Invio, e CTRL+X per uscire)

###### Test Web Server e Info PHP

Avviare Il Web Server HTML/PHP

```
apachectl start
```

Apri il Browser Web (ad esempio: Chrome) sul tuo Dispositivo Android e connettiti all'indirizzo 

```
localhost:8080
```

(se hai seguito alla lettera i passaggi sino a qui, puoi saltare questo passagio, infatti ti basta cliccare sul link nella pagina aperta nel tuo Browser Web per vedere le info relative a PHP)
Per vedere le info su quale versione di PHP è installata nella barra degli indirizzi del Browser Web(ad esempio Chrome) sul tuo Dispositivo Android scrivere

```
localhost:8080/phpinfo/
```

Se tutto funziona possiamo procedere con l'installazione di un FlatCMS sul nostro Web Server flessibile e portatile.

###### Installare un FlatCMS

Per installare un FlatCMS abbiamo 2 metodi

1. Scaricare il FlatCMS che piu ci piace dal sito ufficiale e scompattarlo nella cartella htdocs presente nella memoria del telefono(puoi eseguire tutta l'operazione direttamente dall'applicazione file manager del tuo Dispositivo Android)

2. Usare i comandi `wget` `tar` `unzip` `mv` `rm` in Termux per eseguire la stessa operazione, ma da linea di comando

##### FlatCMS - Creare Siti Web e Blog senza usare DataBase (MySQL o simili)

###### Per capire cosa è un FlatCMS, dobbiamo prima capire cosa è un CMS

Il CMS(Content Management System) è un software che consente di gestire ed organizzare il contenuto di un sito web. E' uno strumento molto utile per tutti coloro che desiderano creare e gestire un sito web senza dover scrivere il codice da zero.

Un CMS ti permette di aggiungere, eliminare o modificare facilmente il contenuto del tuo sito web, come testi, immagini, o video attraverso un'interfaccia semplice ed intuitiva. Alcuni dei CMS piu popolari sono Joomla, Wordpress, Drupal.

Questi sistemi ti consentono di personalizzare il design e le funzionalità del tuo sito web, aggiungendo plugin e/o temi specifici. Un CMS ti offre anche la possibilità di gestire gli utenti e i loro permessi, consentendo a piu persone di collaborare per la creazione e manuntenzione del sito stesso. In conclusione, un CMS **semplifica la creazione e gestione di un sito web**, consentendoti di concentrarti sul contenuto senza doverti preoccupare della parte tecnica.

###### Cosa è un CMS senza DataBase, od anche chiamato FlatCMS

Un FlatCMS è un tipo di sistema di gestione dei contenuti progettato per organizzare ed archiviare il database su file. A differenza di un CMS tradizionale come Joomla, Wordpress, Drupal, che si concentrano sulla gestione di contenuti web, un FlatCMS si concentra sulla gestione dei file. Invece di archiviare i contenuti in un DataBase(MySQL o simili), un FlatCMS memorizza i file direttamente nella struttura del File System.

In parole povere, un FlatCMS è un sistema di gestione dei contenuti che si concentra sulla gestione dei file, offrendo un'organizzazione efficiente e una facile accessibilità ai file arichiviati.

E per i meno esperti è una manna dal cielo, infatti non devi cimentarti nell'installazione e configurazione, e poi anche la gestione del DataBase (MySQL o simili).

**FlatCMS - Elenco qualcuno che ho testato, il metodo di installazzione è il medesimo**.

#### [FlatPress](https://github.com/flatpressblog/flatpress)

Assicurati di essere nella cartella `htdocs`, per sicurezza scrivi

```
cd $HOME/storage/shared/htdocs
```

Scaricare ed installare FlatPress(se usciranno nuove versioni aggiornerò questo parte della guida)

```
wget https://github.com/flatpressblog/flatpress/archive/1.3.1.zip
```

```
unzip 1.3.1.zip && mkdir fp && mv flatpress-1.3.1/* fp && rm -rf flatpress-1.3.1 1.3.1.zip
```

(FlatPress è stato installato automaticamente nell cartella `fp`, connettiti a `localhost:8080/fp/` e seguire le informazioni a monitor per completare l'installazione)

#### [HTMLy](https://github.com/danpros/htmly/?tab=readme-ov-file)

Assicurati di essere nella cartella `htdocs`, per sicurezza scrivi

```
cd $HOME/storage/shared/htdocs
```

Scaricare ed installare HTMLy(se usciranno nuove versioni aggiornerò questo parte della guida)

```
wget https://github.com/danpros/htmly/archive/refs/tags/v3.0.5.tar.gz
```

```
tar xzf v3.0.5.tar.gz && mkdir htmly && mv htmly-3.0.5/* htmly && rm -rf htmly-3.0.5 v3.0.5.tar.gz && ls
```

(HTMLy è stato installato automaticamente nell cartella `htmly`, connettiti a `localhost:8080/htmly/` e seguire le informazioni a monitor per completare l'installazione)

