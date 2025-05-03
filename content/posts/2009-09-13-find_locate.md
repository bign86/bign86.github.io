---
title: "Find and locate"
date: 2009-09-13
tags:
  - linux
  - shell
  - base_command
categories:
  - base_commands
author_profile: true
draft: false
comments: false
---

Poter cercare un preciso file nel computer che non sappiamo esattamente dove si trovi è una delle più elementari e fondamentali operazioni che il pc deve eseguire. Linux non è mai stato famoso per la qualità delle applicazioni grafiche create a questo scopo, tuttavia dispone di fantastici tool a linea di comando.\
I due più utilizzati sono `locate` e `find`.

## LOCATE

Il comando più veloce e semplice da usare, ma anche meno potente, è locate. Il suo scopo è trovare nella macchina tutti i file che contengono nel nome la stringa passata come argomento. L'operazione viene compiuta istantaneamente in quanto locate effettua la ricerca all'interno di un apposito database da lui stesso creato che si trova al percorso `/var/lib/mlocate/mlocate.db`.\\
La sintassi è semplicissima:

```bash
locate <stringa>
```

In output il comando restituirà la lista di tutti i file trovati con il loro path. Di default il comando locate è case sensitive. Per effettuare una ricerca case insensitive basta usare l'opzione `-i`:

```bash
locate -i <stringa>
```

Altra utile opzione è `-c`. Con questa il comando locate darà in output solo il conteggio dei record trovati senza darci però la lista completa. Ad esempio sul mio pc:

```bash
locate -c exim4
184
```

ovvero esistomo 184 file contenenti la stringa "exim4".

La velocità di locate nel restituirci i risultati la paghiamo però nel fatto che tutti i file creati, spostati, distrutti, rinominati eccetera dopo l'ultimo aggiornamento del database (che viene effettuato periodicamente in automatico grazie a `cron`) non verranno trovati correttamente. Si può forzare locate a riaggiornare il database, operazione che tra l'altro non richiede molto tempo, con:

```bash
updatedb
```

da eseguire come root.

### Opzioni utili

Vediamo altre opzioni utili per rendere più flessibile locate:

| Option | Meaning                                                 |
|:------:| ------------------------------------------------------- |
| `-b`   | cerca solo il basename                                  |
| `-d`   | specifica un altro file `.db` di database da utilizzare |
| `-e`   | solo esistenti                                          |
| `-L`   | segue i link simbolici controllando l'esistenza         |

Le opzioni non sono finite, per le altre rimando alle pagine man. Anche il comando updatedb possiede alcune opzioni, ma non le riporto perchè esulano dallo scopo di questo breve howto.

```bash
man locate
man updatedb
```

## FIND

Un comando molto più potente e versatile di `locate` è `find`. Con questo comando diventa possibile specificare più dettagliatamente cosa cercare, utilizzando criteri di ricerca non solo basati sul nome del file, ed anche indicare percorsi precisi nei quali effettuare le ricerche. Inoltre è possibile far eseguire comandi sui risultati trovati sempre dalla stessa riga di istruzioni.\
La sintassi di base è la seguente:

```bash
find <cartella> <criterio> <comando>
```

:`<cartella>`: il percorso dal quale cominciare ad effettuare ricorsivamente la ricerca. Se non è specificato si parte dalla cartella corrente: `.`.
:`<criterio>`: su quale base vogliamo effettuare la ricerca. Di default si utilizza il nome del file (come in `locate`).
:`<comando>`: cosa vogliamo fare dei risultati trovati. Di default vengono mostrati a schermo.

Questo è un elenco di alcuni importanti criteri di ricerca utilizzabili.

### Criteri

| Option           | Meaning                                                                                                                         |
|:----------------:| ------------------------------------------------------------------------------------------------------------------------------- |
| `-name stringa`  | cerca file o directory il cui nome contiene la stringa in argomento                                                             |
| `-iname stringa` | come sopra ma case insensitive                                                                                                  |
| `-type x`        | cerca solo un determinato tipo di oggetto. Con `f` trova solo file normali, con `d` solo directory e con `x` solo file speciali |
| `-user utente`   | cerca solo oggetti appartenenti all'utente specificato                                                                          |
| `-amin n`        | cerca file a cui abbiamo acceduto n minuti fa                                                                                   |
| `-atime n`       | cerca file a cui abbiamo acceduto n*24 ore fa                                                                                   |
| `-mmin n`        | cerca file i cui dati sono stati modificati n minuti fa                                                                         |
| `-mtime n`       | cerca file i cui dati sono stati modificati n*24 ore fa                                                                         |
| `-perm n`        | cerca file con certi permessi                                                                                                   |
| `-empty`         | cerca solo file vuoti o directory vuote                                                                                         |

Questa lista sebbene sembri lunga è solo la punta dell'iceberg. Come sempre per ulteriori opzioni le pagine di manuale sono a disposizione:

```bash
man find
```

### Esempi

* **1.** Cerca tutti i file pdf presenti nella cartella `~/download/`:

   ```bash
   find ~ -iname *.pdf -exec mv -v {} ]~/download/ \;
   ```

* **2.** Sposta tutti gli mp3 contenenti nel nome la scritta soundtrack nella cartella `~/soundtrack/`:

   ```bash
   find ~ -type f -iname *soundtrack*.mp3 -exec mv -v {} ]~/soundtrack/ \;
   ```

* **3.** Cerca tutti i file nella cartella `/var/log/` modificati da meno di 10 minuti. Molto utili se cerchiamo qualcosa dentro ad un file di log e vogliamo restringere la ricerca ai file modificati di recente senza doverli controllare tutti (per questa cartella occorrono i privilegi di root).

   ```bash
   find /var/log/ -type f -mmin -10
   ```

* **4.** Cerca nella cartella corrente tutti i file `.txt` modificati da meno di 24 ore:

   ```bash
   find . -type f -mtime 1
   ```

* **5.** Cerca tutti i file di backup nella home e li cancella:

   ```bash
   find ~ -type f -name *~ -mtime 1 -exec rm ’{}’ \;
   ```

* **6.** Cerca nella cartella corrente tutti i file semplici vuoti appartenenti all'utente user:

   ```bash
   find . -type f -empty -user user
   ```

* **7.** Cerca nella cartella corrente tutte le directory con permessi settati a `777`:

   ```bash
   find . -type d -perm 777
   ```

* **8.** Questo comando elenca tutti i moduli installati nel kernel attualmente utilizzato (grazie a volatile int):

   ```bash
   find /lib/modules/$(uname -r)/ -type f -iname '*.o' -or -iname '*.ko'
   ```
