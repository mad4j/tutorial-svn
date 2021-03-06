# SVN Tutorial

Breve guida di riferimento all'utilizzo del comando ```svn```.

## Introduzione

> SVN non può sostituire il dialogo tra i membri del gruppo di sviluppo.

## Prima di iniziare

Verifica se il comando svn è installato sul nostro computer

```svn --version --quiet```

Restituisce la versione correntemente installata sul computer ed accessibile di default da linea di comando.

```1.9.7```

## Come ottenere aiuto

Per ottenere informazioni sull'utlizzo del comando svn è possibile utilizzare la seguente opzione

```svn help```

Per ottenere informazioni dettagliate su uno specifico comando è possibile utilizzare

```svn help COMMAND```

dove con **COMMAND** si indica il comando per il quale si vogliono ottenere informazioni (es. ```svn help chekcou```).

## Creare una copia di lavoro locale

Per creare una copia di lavoro sul nostro computer è necessario utilizzare il comando ```checkout```

Questa è la sintassi del comando:

```svn co URL[@REV]... [PATH]```

dove

* ```URL``` indica il path sul repository remoto da copiare in locale
* ```@REV``` indica quale revisione deve essere copiata, se non viene specificato allora si utilizza la revisione HEAD
* ```...```  più URL possono essere indicate nello stesso comando
* ```PATH``` indica in quale cartella locale deve essere effettuata la copia, se non viene specificato viene utilizzato il basename della URL

Il parametro di revisione ```@REV``` può essere specificato nei seguenti modi

* **NUMBER** indica una revsione specifica
* **'{' DATE '}'** indica la revisione inziale ad una specifica data (es. **{2020-03-06}**), se non viene indicta una ora allaro si intende 00:00, quindi il checkout sarà fatto relativo all'ultimo commit del giorno precedente 
* **HEAD** indica l'ultima revisione del repository.
* **BASE** indica la revisione "originale" presente sulla propria copia di lavoro; non si riverisce al server, ma solo alla copia locale.
* **COMMITTED** indica l'ultima versione presente modficata sulla copia locale.
* **PREV** indica la revisione immeditamente precedente a **COMMITTED**

## Gestione dei conflitti
Se viene generato un conflitto, modifiche effettuate da diverse copie locali si sovrappongono, allora ci verrà chiesto di gestirlo.
Nel caso di documenti di testo vegono inseriti nel dei marcatori che indicano le sezioni in conflitto:

```
<<<<<<< .mine
local copy revision
=======
repository revision
>>>>>>> .r1111
```

La sezione marcata come ```.mine``` contiene la porzione di testo che ha generato il conflitto. La sezione sottostante, marcata con il numero di versione (es. ```.r1111```) contiene la porzione di testo originale.

Inoltre vengono creati tre nuovi file temporanei:

* **document.mine** contiene la copia locale del documento che ha generato il conflitto
* **document.rBASE** contiene la copia originale precedente alle modifiche locali
* **document.rHEAD** contiene la copia più aggiornata sul repository

Dove **BASE** e **HEAD** vengono sostituiti con i rispettivi numeri di revisione.

A questo punto è possibile:

* risolvere i conflitti manualmente
* sovrascrivere il documento in conflitto con uno dei tre temporanei
* utilizzare il comando ```revert``` per annullare le modifiche locali

Nei primi due casi è necessario indicare la risoluzione del conflitto con il comando ```svn resolve --accept working```.

I conflitti che non vengono risolti immediatamente (es. durante una operazione di ```update```) possono essere processati in un secondo momento con il comando ```svn resolve```.

```svn resolve [PATH...]```

Questo comando permette di risolvere i conflitti in modo interattivo oppure automatico. In modalità interattiva lo scorrimento delle cartelle è ricorsivo per default. Invece in modalità automatica lo scorrimento ricorsivo deve essere dichiarato esplicitamente tramite l'opzione ```-R [--recursive]```.

Per risolvere automaticamente i conflitti è necessario specificare l'opzione ```--accept ARG``` con uno dei seguenti argomenti:

* **base**
* **working**
* **mine-conflict**, **mine-full**
* **theirs-conflict**, **theirs-full**

Nella modalità interattiva viene richiesto, per ogni file, quale argomento applicare.

## Verificare lo stato della copia locale

Per verificare lo stato delle modifiche effettuate nella copia locale è possibile utilizzare il comando:

```svn status [PATH...] [-u] [--ignore-externals]```

dove

* **PATH...** indica su quali cartelle deve essere eseguito il comando
* **-u** indica che deve essere indicato anche lo stato delle modifiche sul server
* **--ignore-externals** indica che devono essere analizzati tutti i file indipendentemente dalle direttive **ignore**

Se non vengono specificati parametri allora vengono mostrate solo le modifiche locali senza connettersi al server.

Il comando indica lo stato delle risorse utilizzando le seguenti etichette:

* **M** risorsa modificata loacalmente
* **?** risorsa non sotto controllo di configurazione
* **A** risorsa da aggiungere al repository
* **C** risorsa in conflitto
* **!** risorsa cancellata esternamente al controllo di configurazione
* **X** risorsa inclusa da una direttiva **external**
* **\*** risorsa con revisione più recente presente sul server 

E' possibile ottenere informazioni dettagliate utilizzando il comando

```svn help status```

## Consolidare le modifiche locali

Per consolodiare le modifiche locali sul repository è possibile utilizzare il seguente comando:

```svn ci [PATH...] [--include-externals] -m ARG```

dove

* **PATH...** indica le risorse (i.e. documenti o cartelle) che devono essere processate
* **--include-externals** indica di processare anche le risorse che sono state indicate dalle diretteve **externals**
* **-m ARG** indica il messaggio con cui descrivere l'aggiornamento

## Aggiornare la copia locale

Per allineare la copia locale con gli ultimi aggiornamneti sul server è possibile utilizzare il seguente comando:

```svn up [PATH...]```

dove

* **PATH...** indica le risorse (documenti o cartelle) da processare.

Se viene indetificato un conflitto verrà richiesto di risolverlo immediatamente o in un momento successivo (**postpone**). In questo caso il documento verrà modificato in modo da contenere delle sezioni che evidenziano i conflitti e verranno creati tre file temporanei con le diverse versioni del documento che ha creato il conflitto.


