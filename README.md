# SVN Tutorial

Breve guida di riferimento all'utilizzo del comando ```svn```.

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
