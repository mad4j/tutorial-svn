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

Questo comando permette di risolvere i conflitti in modo interattivo oppure automatico.
