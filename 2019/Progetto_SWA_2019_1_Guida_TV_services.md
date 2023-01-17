# Corso di Sviluppo Web Avanzato
*Progetti A.A. 2019/2020* - Specifica 1

## Progetto "Guida TV services"

> Versione 1.0

### Premessa

Il presente progetto estende un progetto del corso di Web
Engineering. Per prima cosa, quindi, potrebbe essere utile leggere questa
specifica di base. Tuttavia, non si dovrà realizzare la *web application* con
tutte le funzionalità descritte nella specifica base, ma solo la relativa base
di dati, a cui poi affiancare il servizio web RESTful che esporrà questi dati e
alcune delle relative funzionalità di accesso e modifica. Si dovrà infine
implementare, utilizzando HTML5 e JQuery, un semplicissimo client per questo
servizio, utile anche a testarne la correttezza.

Nota: il servizio richiesto realizza, in pratica, un
disaccoppiamento della gestione dati (e di alcune funzionalità correlate), che
risiede necessariamente sul server, dalla logica di business e dal rendering
dell'interfaccia, che potrebbero essere totalmente gestite dal client con
Javascript, come spesso accade nelle SPA (*single page application*,
applicazioni a pagina singola).

### Il Servizio

A partire dalla base di dati del progetto \*Guida TV \*
(che non dovete necessariamente realizzare ex novo se non siete studenti del
corso di Web Engineering: siete liberi di usare il codice e il database
realizzato da qualche vostro collega), si
costruisca il servizio RESTful
specificato di seguito. La logica del servizio potrà, opzionalmente, utilizzare
una libreria ORM per accedere ai dati.
Nota: nella specifica che segue gli elementi tra parentesi graffe sono *placeholder* , le parentesi quadre indicano elementi opzionali e le parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per indicare delle alternative. Se non altrimenti specificato, il formato dati per l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere al meglio i dati richiesti.

#### URL: auth

##### VERBO: POST

Prende in input un oggetto contenente username e password di un utente e restituisce un oggetto contenente il relativo *token* di sessione/autenticazione.

#### URL: auth/{SID}

##### VERBO: DELETE

Chiude la sessione relativa al token di sessione/autenticazione {SID}, invalidandolo.

#### URL: canali

##### VERBO: GET

Restituisce una lista contenente i nomi di tutti i canali presenti nel sistema, la relativa ID e la URI necessaria per leggerne il palinsesto odierno.

Esempi di output:

`[{"nome":"Rai 1", "id":1, "url":"http://miaguidatv.it/palinsesto/20190711/1"},{"nome":"Canale 5", "id":2, "url":" http://miaguidatv.it/palinsesto/20190711/2"}]`

#### URL: auth/{SID}/canali

##### VERBO: POST

Inserisce un nuovo canale nel database. Il payload dovrà essere una struttura contente tutti i dati richiesti dal database per questo tipo di elemento, come risulta dalla specifica del progetto *Guida TV* . In output ci sarà la URL necessaria a leggere i dati del palinsesto del canale appena inserito. L'operazione viene eseguita nel contesto della sessione con token {SID} ed è disponibile solo se l'utente associato è l'amministratore. Tutti gli altri riceveranno un errore 403 - *Forbidden*.

#### URL: canali/{ID}/palinsesto

##### VERBO: GET

E' un alias per palinsesto/\<data odierna\>/{ID}.

#### URL: canali/{ID}/palinsesto/{DATA}

##### VERBO: GET

E' un alias per palinsesto/{DATA}/{ID}.

#### URL: auth/{SID}/canali/{ID}

##### VERBO: PUT

Modifica i dati del canale identificato da {ID}. Il payload potrà essere lo stesso usato per l'inserimento (POST) sulla URL auth/{SID}/canali. L'operazione viene eseguita nel contesto della sessione con token {SID} ed è disponibile solo se l'utente associato è l'amministratore. Tutti gli altri riceveranno un errore 403 - *Forbidden*.

#### URL: palinsesto/{DATA}

##### VERBO: GET

Restituisce il palinsesto di tutti i canali per la data {DATA}. Definite un formato JSON di output che strutturi bene tutte le informazioni necessarie, compresa la URL di dettaglio di ciascun programma: ricordate, infatti, che nel palinsesto non devono essere presenti tutti i dettagli di ciascun programma (si veda la specifica del progetto *Guida TV* ). Se la richiesta riguarda una data non disponibile, si restituisca un errore 404 - *Not Found*.

#### URL: palinsesto/{DATA}/{CANALE}

##### VERBO: GET

Restituisce il palinsesto del canale {CANALE} per la data {DATA}. Se la richiesta riguarda una data o un canale non disponibili, si restituisca un errore 404 - *Not Found*.

#### URL: palinsesto? {FILTER}\[first={m}\[\&last={n}\]\]

##### VERBO: GET

Esegue una ricerca per tutti i programmi nel database che corrispondono ai criteri della query string {FILTER} che può contenere tutti i parametri previsti per la ricerca di programmi specificati dal progetto *Guida TV* (ad esempio titolo, genere, canali, ecc.). Nota: poiché una ricerca senza criteri non ha senso, il servizio dovrà respingere ogni richiesta che non contenga almeno un filtro.

Restituisce la lista delle URL necessarie ad accedere al dettaglio di tutti i programmi corrispondenti ai criteri.

Poiché gli elementi della lista potrebbero essere molti, adotteremo una *strategia di paginazione* . Includendo i parametri *first* ed eventualmente *last* , la lista conterrà solo gli elementi dal numero *m* al numero *n* inclusi (supponendo qualche tipo di ordinamento tra gli elementi, magari per data). Omettendo *last* , verranno restituiti tutti gli elementi dal numero *m* all'ultimo. Omettendo entrambi i parametri verrà restituita la lista completa.

#### URL: programmi/{ID}

##### VERBO: GET

Restituisce tutti i dettagli relativi al programma indentificato da {ID} (titolo, descrizione, data/ora/canale di messa in onda, ecc).

#### URL: auth/{SID}/programmi

##### VERBO: POST

Inserisce un nuovo programma nel database. Il payload dovrà essere una struttura contente tutti i dati richiesti dal database per questo tipo di elemento, come risulta dalla specifica del progetto *Guida TV* . In output, come da standard, ci sarà la URL necessaria a leggere il dettaglio del programma appena inserito. L'operazione viene eseguita nel contesto della sessione con token {SID} ed è disponibile solo se l'utente associato è l'amministratore. Tutti gli altri riceveranno un errore 403 - *Forbidden*.

#### URL: auth/{SID}/programmi/{ID}

##### VERBO: PUT

Modifica i dati del programma identificato da {ID}. Il payload potrà essere lo stesso usato per l'inserimento (POST) sulla URL auth/{SID}/programmi. L'operazione viene eseguita nel contesto della sessione con token {SID} ed è disponibile solo se l'utente associato è l'amministratore. Tutti gli altri riceveranno un errore 403 - *Forbidden*.

#### URL: programmi/{ID}/episodi

##### VERBO: GET

Se il programma indentificato da {ID} è parte di una serie, restituisce una struttura contenente i dati di messa in onda di tutti gli episodi di questa serie in un periodo che va da trenta giorni nel passato a sette giorni nel futuro. Se il programma non ha una serie associata, è possibile restituire un errore 400 - *Bad Request*.

*Opzionalmente* , provate a scrivere una procedura che popoli il database dei palinsesti attingendo ai servizi REST di vere emittenti televisive. Ad esempio, per la RAI, la URL <https://www.raiplay.it/palinsesto/app/old/28-10-2019/canali.json> restituisce il palinsesto di tutte le reti per la data specificata (attenzione: usate una data recente, altrimenti riceverete un *not found*).

Come vedrete, alcune URL contengono un segmento "*auth/{SID}"* : questo realizza un semplice schema di autenticazione che useremo per verificare le autorizzazioni necessarie a eseguire certe operazioni. Tutte le URL senza il prefisso *auth/{SID}* corrispondono a operazioni accessibili agli anonimi. Ove presente, invece, questo prefisso indica che la URL deve contenere un *token di sessione/autenticazione* , precedentemente ottenuto tramite un POST sulla URL *auth*, che simula un vero e proprio login, associando al token restituito i dati dell'utente autenticato.

Prestate attenzione alla *struttura delle URL*, che
deve seguire per quanto possibile il paradigma collection-item, evidenziando le
relazioni tra gli oggetti tramite nidificazione.

Se alcune operazioni necessitano di accesso con
autenticazione, potrete usare uno qualsiasi degli approcci visti a lezione per
trasmettere il *token di sessione/autenticazione* , precedentemente
ottenuto tramite la procedura (API) di login: ad esempio inserire un prefisso *auth/*
sulla URL o passare tale token tramite *cookie* o con l'header *Authentication*.

### Il Client

Si crei un client HTML5/Javascript con il supporto di JQuery
per il servizio appena descritto, sotto forma di una semplice *single page
application* che utilizza AJAX per interagire col servizio mostrandone i
dati opportunamente sulla pagina.
Il client dovrà essere strutturato come segue:

1. Un selettore permetterà di scegliere un canale.
2. Sarà visualizzata una pagina col palinsesto odierno del canale.
3. Cliccando su un programma del palinsesto, si aprirà una pagina (o un popup) con i relativi dettagli.

Questa è una specifica di base. Potete aggiungere altre
funzionalità a vostra scelta che sfruttino le altre API del servizio, se
volete, aumentando così il valore del progetto.

Nota: normalmente, una SPA di una certa complessità andrebbe
realizzata sfruttando framework di più alto livello come Angular o Vue.
Tuttavia, per applicazioni semplici, l'uso di JQuery a basso livello risulta
essere più pratico, in quanto l'uso di framework come AngularJS aggiunge un
notevole overhead al progetto.

# Indicazioni per lo Sviluppo del Progetto

### Tecnologie da utilizzare

* Nel realizzare le pagine web, se necessarie, si utilizzi HTML5.
* Per la programmazione lato *client*, oltre a JQuery, si possono liberamente includere nel progetto altre librerie e plugin, a patto che nella relazione siano citate e descritte.
* Per la programmazione lato *server,* il servizio potrà essere realizzato in *JAVA* o *PHP.* Sviluppando in Java, si utilizzi preferibilmente un'implementazione di *JAX-RS* (come Jersey), che permette di realizzare servizi con una struttura più modulare. L'uso di framework come *Spring* per creare il servizio è ammesso, ma solo se il progetto dovrà essere integrato o esteso, ad esempio con quello di un altro corso. In PHP, invece, è possibile usare un framework come *Slim* o un altro a scelta.

### Svolgimento e Documentazione del Progetto

Le specifiche fornite potrebbero non risultare esaustive o completamente definite. Ogni funzionalità aggiunta o raffinata, anche tramite l'interazione con il committente o con gli utenti finali del sito, sarà adeguatamente valutata. Tutte le scelte progettuali vanno comunque discusse e motivate.

Il progetto, svolto secondo le linee guida date dalle specifiche, dovrà essere consegnato nella forma di una o più web application completamente funzionanti, i cui contenuti e le cui caratteristiche saranno valutati in sede d'esame. Le parti della specifica marcate come *opzionali*, se omesse, non renderanno il progetto insufficiente ma non gli permetteranno comunque di raggiungere il massimo dei voti. Nel caso si decida di realizzarle, non sarà necessario che siano perfette o complete, ma che dimostrino chiaramente il vostro impegno nell'affrontare una tematica avanzata.

La documentazione (**in formato elettronico** ) che accompagna il progetto **deve** contenere almeno le seguenti informazioni:

1. Indicazione delle **dipendenze** software (di quali librerie avete bisogno dal lato server e client?).
2. Indicazione delle **funzionalità realizzate** e di quelle eventualmente non realizzate. Descrizione dettagliata delle eventuali funzionalità extra o opzionali inserite nel progetto.
3. **Specifica dettagliata dell'API** redatta in formato *OpenAPI* (ad esempio tramite il tool di *Swagger* , <https://swagger.io/tools>): per ogni operazione realizzata dovrete specificare
   * la URL,
   * gli eventuali parametri, dove sono inseriti (nel path o nella query string) e i loro possibili valori,
   * il verbo HTTP usato,
   * il formato del payload di richiesta (se necessario),
   * i codici di stato http ritornati e, per ogni stato, il formato dell'output (se presente).

Per quel che riguarda il formato dei dati scambiati (payload e output) fornite sempre un esempio completo della struttura JSON che intendete usare.

Nel caso di gruppi di lavoro composti da più componenti, *il contributo effettivo offerto da ciascun componente* alla realizzazione finale **deve** essere descritto nella documentazione. In sede di esame, i responsabili potranno essere chiamati a riferire sugli aspetti loro delegati.

### Valutazione del Progetto

Nel valutare il progetto consegnato saranno prese in considerazione le seguenti caratteristiche (in ordine di importanza):

1. Rispetto delle specifiche.
2. Correttezza tecnica.
3. Uso appropriato delle tecnologie di base descritte nel corso.

A questa valutazione si aggiungerà quella generale derivata dalla discussione del progetto in sede d'esame.

### Ulteriori Informazioni

Questa specifica è disponibile nel repository del corso di Sviluppo Web Avanzato, all'indirizzo https://github.com/SviluppoWebAvanzato-Univaq/SWA_Project_Specifications. Ulteriori informazioni e chiarimenti sulle specifiche possono essere richiesti direttamente via email all'indirizzo giuseppe.dellapenna@univaq.it.

Si ricorda che i progetti vanno svolti in *piccoli* gruppi. Eccezioni a questa regola andranno concordate direttamente col docente.
