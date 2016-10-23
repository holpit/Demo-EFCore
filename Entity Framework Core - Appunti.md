# Entity Framework Core

## Agenda

Da 5 a 10 minuti in slides, per spiegare che cos'è EF Core

Il resto della sessione a scrivere codice in demo!

Avevo già presentato EF7 in più di un occasione, ma queste demo sono completamente nuove e mostrerò cose nuove.

## Introduzione a EF Core

### Naming history

- Entity Framework Everywhere
  - era l'idea di prendere EF e farlo girare ovunque fosse possibile scrivere codice .NET
  - hanno deciso che quello che stavano costruendo sembrava più l'evoluzione di Entity Framework e quindi hanno buttato via questo nome
- Entity Framework 7
  - per 2 anni è stato EF7
  - questo nome ha portato confusione, perchè giustamente si pensava fosse la naturale evoluzione di EF6, quindi che avrebbe incluso tutte le funzionalità di EF6, ne avrebbe aggiunto delle nuove, fosse stato più robusto, ecc.... ma vedremo nella presentazione che non è così e quindi hanno buttato via anche questo nome
- Entity Framework Core

### Stato del progetto

Alle origini EF era distribuito con il .NET framework e quindi seguiva la nomenclatura dei rilasci .NET EF 3.5 SP1 - EF 4, aveva il vantaggio di installare il framework e avere tutto sulla propria macchina pronta all'uso, ma il "piccolo" svantaggio che se si trovava un bug o si voleva aggiungere un feature il ciclo di rilascio era di circa 1 o 2 anni!

Dalla versione 4.1 fino alla 5 hanno iniziato ha distribuire alcune funzionalità e alcuni bug fix con pacchetti nuget, ma le parti core erano ancora distribuite con il framework, quindi per alcune funzionalità il processo era rapido, ma per altre il processo rimaneva molto lento, legato alla distribuzione del framework.

Alla fine dalla versione 6 hanno iniziato a distribuire tutto EF con pacchetti nuget, spostando anche tutti gli strumenti integrati nell'installer di Visual Studio in estensioni separate scaricabili dal download center di Microsoft.

Dalla versione 6 EF è anche diventato open-source, e come al solito per confondere un po' gli sviluppatori del mondo Microsoft, la fuori esisteva già GitHub dove la maggior parte dell'open-source risiedeva, e ovviamente Microsoft ha pensato bene di creare la loro cosa chiamata "CodePlex" e quindi sono andati open-source li sopra.

La versione stabile è ad oggi la 6.1.3, e stanno lavorando sulla versione 6.2 che dovrebbe essere rilasciata entro la fine dell'anno anche se sono solo al 8% della milestone (ah si sono spostati su GitHub nel frattempo!!)

Quindi il succo è che EF 6 è ancora in sviluppo e supportato, e allo stesso tempo lo stesso team sta lavorando su EF Core ovviamente su GitHub.

## Nuove piattaforme

### Unified platform

Con EF 6 è possibile solo sviluppare attraverso il full .NET framework, mentre con EF Core è possibile sviluppare sul full framework, su .net core e su xamarin.

Unico appunto è che questa visione è ancora in sviluppo, quindi alcune parti potrebbero non essere ancora perfette su tutte le piattaforme, come UWP che si, usa le .NET Core, ma dove EF Core non gira ancora perfettamente, ha ancora qualche limitazione, quindi va bene per fare prototipi, ma non ancora per fare applicazioni reali. Anche su Xamarin si riesce già a fare qualcosa con EF Core, ma ci stanno ancora lavorando.

## Nuovi data store

Relazionali e non-relazionali

- con questo non vuol dire che stanno sviluppando una magica astrazione che mascheri completamente il tipo di data store che si sta utilizzando, perchè sarebbe fico in demo, ma inutilizzabile nel codice reale
- quindi stanno creando un set di componenti core che hanno senso con tutti i datastore, come utilizzare linq per portare i dati in memoria e abilitare le funzionalità di tracking per capire se un oggetto e stato modificato, ed eventualmente passarlo al provider del data store per salvare le informazioni.
- tutte le cose che sono specifiche per un data store sono codificate in librerie separate attivabili attraverso una serie di extension methods, come ad esempio il nome di una tabella o di una colonna, oppure se sono su azure table storage è possibile settare il RowKey e il PartitionKey per l'entità.
- Su EF Core v1.0.0 hanno rilasciato solo i data store relazionali: SQL Server, SQLite, Postgres, MySQL, SQL Compact, Oracle (presto...)

## Nuove funzionalità

Ci sono tante funzionalità che gli sviluppatori hanno richiesto nel tempo, ma la verità è che in EF 6 alcune funzionalità sono molto difficili e molto costose da implementare, EF Core invece è stato costruito con in mente la possibilità di estenderlo in ogni minimo dettaglio in modo da potere implementare anche le funzionalità più richieste.

Questa è una lista di funzionalità richieste da tanto tempo su EF 6, che sono già nella versione v1 su EF Core:

- Salvataggio in batch durante la SaveChanges
- Valutazione lato client nelle LINQ query
- Shadow state properties (proprietà che non esistono nel modello a classi, ma che sono gestite dal ChangeTracker, es.: DataUltimoAggiornamento)
- SQL Server sequences
- Alternate keys (permette di definire una relazione senza usare necessariamente la primary key, es.: BlogUrl invece che la classica chiave su BlogId)

## Leggero ed estensibile

**API di primo livello costruite su servizi modulari**

Questo vuol dire che esiste un set di api di primo livello, DBContext, DBSet, ecc... come siete abituati ad utilizzare con le precedenti versioni di EF, ma costruito su un set di piccoli servizi, servizi per la generazione di sql, servizi per la generazione dei valori per le proprietà, e questi servizi possono essere riutilizzati, oppure possono essere sostituiti.

**Uso ottimizzato della memoria e della CPU**

Costruito per girare su piattaforme Mobile e Azure, quindi meno consumo di memoria e meno consumo di CPU.

**Pay-per-play components**

L'applicazione paga solo per i moduli che carica, e dato che i provider su EF Core sono sviluppati in moduli separati, se ho bisogno di utilizzare solo SQLite, caricherò solo il modulo SQLite e non avrò il peso di caricare il modulo SQL Sever.

## EF Core & EF6.x

### EF Core

Microsoft sta cercando di mantenere le cose che agli sviluppatori sono piaciute, DBContext e DBSet sono ancora al loro posto, LINQ query, ecc... ma allo stesso tempo non vogliono essere vincolati dal passato. Infatti le parti interne sono state completamente riscritte per facilitare le estensioni, e non tutte le funzionalità di EF 6 sono state implementate, alcune le saranno con il tempo, altre probabilmente abbandonate, ma con il fatto che EF Core è completamente estensibile, posso immaginare che qualsiasi cosa potrà essere introdotta di nuovo, magari con estensioni dalla community open-source.

### EF Core & EF6.x

EF6 è un prodotto molto maturo, li da più di 8 anni, ci sono centinaia di database provider, e continueranno a fare rilasci di patch e piccole funzionalità, accetteranno contributi dalla community, ma non hanno intenzione di sviluppare grosse funzionalità, perchè ovviamente lascieranno la maggiorparte delle risorse su EF Core.

EF Core è un prodotto nuovo, è davvero alla versione 1, e come tale va trattata. Non è stabile al 100%, non ha ancora tutte le funzionalità, e non ha ancora tutti i provider per tutti i database esistenti, ci sono più o meno 10 provider al momento.

EF6 rimane la giusta opzione per molte applicazioni.

Fate attenzione a controllare i vostri requisiti con cura se state considerando di utilizzare EF Core, facendo particolare attenzione alla traduzione delle LINQ query in SQL, perchè è la parte di stack più complicata ed è facile possano esserci bug o limitazioni.

Convertire applicazioni esistenti da EF6.x a EF Core non è un upgrade, ma un porting completo, questo vuol dire che codice molto semplice sarà convertibile facilmente, ma applicazioni che fanno largo uso di API EF avranno bisogno di parecchio lavoro per essere convertite, inoltre alcune API che si chiamano in modo simile hanno comportamenti diversi tra le due versioni di EF. Quindi fatelo solo se avete un enorme vantaggio in termini di performance o funzionalità.

Controllate il sito docs.efproject.net alla sezione "EF Core vs EF6.x" per tutti i dettagli sulle differenze.

----

**Fino a qui devo metterci più o meno 10-11 minuti!!**

----

## Demos

Quanti di voi hanno utilizzato almeno una delle versione precedenti di EF?

Quanti di voi hanno giocato con EF Core?

### Demo1 - EF Core 101

Quando parliamo di Entity Framework, sia per EF6 che per EF Core, abbiamo un modello, e il modello è un qualcosa che dice, queste sono le classi con cui andrò ad interagire, e questo è come si traducono e parlano con il database.

Quindi abbiamo una serie di classi che definiscono le entità, e poi un DbContext, che è una sessione dove è possibile fare query e salvare dati. Da notare che la classe che definisce un contesto in EF Core deriva da DbContext che è lo stesso nome utilizzato in EF6.

EF6 usa un sacco di magia per stabilire a quale database si deve collegare, e questo era fico nelle demo, forse interessante nei prototipi, ma di certo con il tempo è diventato un qualcosa che ha solo confuso chi approcciava a EF. Quindi in EF Core hanno introdotto il metodo `OnConfiguring` che ti permette di specificare in modo esplicito il tipo di provider da utilizzare, e quale connection string utilizzare.

----

**Demo1 in 6-7 minuti**

----

### Demo2 - Performance improvements

Molti degli scenari sono stati migliorati in EF Core, ma ancora non tutti sono stati migliorati rispetto EF6, anzi alcuni potrebbero essere anche peggio in questa fase, e sarebbe bene aprire una issue su GitHub se dovessimo trovare un caso riproducibile da potere sottoporre al team EF.

I modelli sia per EF6 che per EF Core sono stati generati da un database esistente.

EF6 e EF Core possono essere utilizzati side-by-side, avendo cura di separare le classi in namespace diversi per evitare collisioni di classi/namespace.

----

**Demo2 in 6-7 minuti**

----

### Demo3 - Model building pipeline

Questo è il processo dove definisco le mie entità e dove eventualmente specifico alcuni attributi o configurazioni con la fluent api.

In EF6.x questo è il processo per la creazione del modello EF:

1. Vengono esaminate le classi che definiscono le entità contro un set di convenzioni, cercando di capire quali sono le chiavi primarie, quali proprietà devono essere mappate a colonne a db, le relazioni, ecc...
2. Viene controllato il `DbContext` alla ricerca di proprietà `DbSet` in modo da stabilire quali entità faranno parte del modello EF.
3. Viene eseguito il nostro codice specificato in `OnModelCreating` e viene salvato tutto nelle configurazioni di EF.
4. Infine viene eseguito un processo che combina i tre punti precedenti per creare il modello finale.

La cosa interessante da notare in EF6, è che quando viene eseguito `OnModelCreating` il modello non è ancora stato creato, e quindi ad esempio per potere impostare un algoritmo diverso per la selezione della chiave primaria di tutte le mie entità, è stato necessario creare un ulteriore api in `OnModelCreating` per potere accettare la nostra richiesta di configurazione e successivamente tenerla in considerazione quando il modello viene creato.

In EF Core hanno seguito un approccio differente:

1. Vengono comunque esaminate le classi contro un set di convenzioni
2. Viene comunque controllato il `DbContext` alla ricerca di `DbSet`
3. A questo punto **il modello viene creato**
4. Il modello viene passato al metodo `OnModelCreating` dove è possibile fare le solite configurazioni con la fluent api, ma inoltre è possibile aggiungere le proprie convenzioni per stravolgere ad esempio la selezione della chiave primaria.

----

**Demo3 in 5-6 minuti**

----

### Demo4 - Simplified metadata API

Una della cose più complicate in EF6 sono le API per l'accesso ai metadati, API dove posso vedere quali classi ho a disposizione nel model e come sono mappate con il database.

La differenza con le API viste nel metodo `OnModelCreating`, è che il `Model` restituito dal `DbContextOptionsBuilder` è modificabile, mentre quello restituito dal `DbContext` è read-only.

EF6 tra l'altro quando crea il modello si collega al database per controllare la versione di SQL Server, che rallenta il processo, tra l'altro se gli impedisci di connettersi a sql, la creazione del modello diventa ancora più lenta, EF Core invece crea il modello molto più velocemente.

----

**Demo4 in 4-5 minuti**

----

### Demo5 - Same model, multiple platforms

Non è solo che è possibile utilizzare EF Core ovunque, ma che il vostro codice per l'accesso ai dati è lo stesso su diverse piattaforme.

Non tutti i database provider sono disponibili per tutte le piattaforme, ad esempio se sono su un device non posso connettermi ad un istanza MySql o SQL Server, ma posso utilizzare SQLite.

Per questa demo si deve eseguire l'applicazione in `Debug` mode, perchè in `Release` l'applicazione viene compilata passando dal .NET Native Toolchain, e dato che EF Core usa molto le generics, tantissimo caching, e cose di questo tipo, non funziona molto bene in release. Con la prossima release di .net native anche EF funzionerà molto meglio in applicazioni UWP.

----

**Demo5 in 6 minuti**

----

### Demo5 (part2) - Same model, multiple databases

Quindi l'idea nella demo precedente è di potere scrivere lo stesso **identico codice** di accesso ai dati e poterlo utilizzare su piattaforme differenti, allo stesso modo possiamo utilizzare del **codice simile** per potere accedere ai dati su differenti database.

----

**Demo5 (part2) in 5 minuti**

----

### Demo6 - Consuming low level services

Diciamo che in poche parole in EF Core finalmente hanno scritto il codice interno rispettando la single responsibility principle, e ogni singolo componente di basso livello risolve solo uno specifico problema, ad esempio la generazione dello script SQL. Questo permette agli sviluppatori di sostituire o estendere ogni singolo componente utilizzato internamente da EF Core, dando praticamente infinite possibilità di estensione.

----

**Demo6 in 4 minuti**

----

## EF Core 1.1.0

Prima di rimetterci sulle demo, parliamo di cosa ci sarà in EF Core 1.1 e poi riprenderemo con le demo mostrando come è possibile sostituire i servizi a basso livello in EF Core 1.1

Questa versione è incentrata a semplificare il passaggio a EF Core da parte degli sviluppatori, in pratica ci sono alcune funzionalità che mancano, e alcune sono molto importanti per permettere l'utilizzo di EF Core in molto realtà.

Uno dei punti più dolenti è il LINQ provider per SQL Server, che ad oggi non è ancora in grado di convertire molte delle query che ci si aspetta siano fatte su SQL mentre vengono eseguite lato client, ecco la v1.1 cercherà di colmare questa mancanza nella generazione degli statement sql.

Un altro dei punti invece è quello di semplificare il modo in cui si sostituiscono i servizi di basso livello.

Detto questo anche in EF Core 1.0 è possibile sostituire i servizi di basso livello, ma è molto più complicato, e nella versione 1.1 hanno semplificato tantissimo questo processo.

Nella prossime versioni >1.1 aggiungeranno il supporto per i Complex/value types (la possibilità di avere una classe che non necessariamente viene mappata ad una tabella, ma a un set di colonne già presenti in una tabella), poi ci sarà quello che chiamano "Simple type conversions" cioè la possibilità di avere un oggetto json in memoria che però è salvato come stringa su database, e quindi avere un qualcosa che traduca la stringa in un oggetto c# per noi nel momento in cui andiamo a leggere la colonna a db.

## Demos

### Demo7 - Replacing low level services

----

**Demo7 in 6 minuti**

----

### Demo8 - Mapping to fields

In EF Core 1.0 era indispensabile avere proprietà `get; set;` e nelle prossime versioni cercheranno di togliere queste restrizioni il più possibile, ma nel frattempo, dalla versione 1.1, sarà possibile mappare anche i membri delle classi (AKA fields).

Esiste una convenzione, che quando forzo una proprietà senza setter ad essere inclusa, EF controlla se esiste un membro della classe che si chiama come la proprietà preceduto da un `_` quello deve essere il *backing field* della proprietà mappata su EF. Se non si segue questa regola è possibile specificare il *backing field* sul *model builder*, se non si segue la convenzione e non si configura il builder, EF ritorna un errore durante la creazione del modello.

----

**Demo8 in 6 minuti**

----

### Demo9 - SQL Server memory-optimized tables

Semplicemente in EF Core a partire dalla v1.1 sulla top level api, esisterà il supporto nativo per le in-memory table di SQL Server.

----

**Demo9 in 6 minuti**

----

### DemoExtra1 - Batching

----

**DemoExtra1 in 5 minuti**

----

### DemoExtra2 - FromSql

In EF6 utilizzare il metodo `SqlQuery` vuole dire che poi qualsiasi altra operazione, come ordinare, filtrare, ecc... deve essere fatto nello script SQL.

In EF Core invece è possibile concatenare altri comandi LINQ dopo un operazione `FromSql`.

----

***DemoExtra2 in 6 minuti***

----

