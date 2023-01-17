# Corso di Sviluppo Web Avanzato
*Progetti A.A. 2016/2017* - Specifica 2

## Progetto "SocialDevelopServices"

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

A partire dalla base di dati del progetto *SocialDevelop*
(che non dovete necessariamente realizzare ex novo se non siete studenti del
corso di Web Engineering: siete liberi di usare il codice e il database
realizzato da qualche vostro collega), si
costruisca il servizio RESTful
specificato di seguito. La logica del servizio potrà, opzionalmente, utilizzare
una libreria ORM per accedere ai dati.
Note: nella specifica che segue gli elementi tra parentesi graffe sono *placeholder* , la parentesi quadre indicano elementi opzionali e le parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per indicare delle alternative. Se non altrimenti specificato, il formato dati per l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere al meglio i dati richiesti in questo formato.

#### URL:/auth/login

##### VERBO: POST

Prende in input un oggetto contenente username e password di un utente e restituisce un oggetto contenente il relativo *token* di sessione/autenticazione.

#### URL:/auth/logout

##### VERBO: POST

Prende in input un token di sessione/autenticazione e chiude la relativa sessione, invalidando il token.

#### URL:/auth/{SID}/developers\[?skills={SKILLS}\]

##### VERBO: GET

Restituisce la lista degli utenti noti al sistema, opzionalmente filtrati in base ai loro {SKILLS}.

Esempio di output:

`[{"name":"Pinco Pallino","url":"http://socialdevelop.it/developers/21"},{"name":"Paolino Paperino","url":" http://socialdevelop.it/developers/141"}]`

L'operazione viene eseguita nel contesto della sessione con token {SID}. L'operazione è ammessa anche per gli utenti anonimi (cioè con la sola URL/developers\[?skills={SKILLS}\]).

##### VERBO: POST

Inserisce un nuovo utente nel sistema. L'oggetto passato come payload conterrà tutti i dati del profilo utente, ad esempio

{"username": "pinco", "password": "in_chiaro!"}

L'operazione viene eseguita nel contesto della sessione con token {SID} e solo se l'utente associato è un amministratore (altrimenti potreste restituire un errore 403 - *Forbidden*).

#### URL:/auth/{SID}/developers/{ID}

##### VERBO: GET

Restituisce il profilo dell'utente {ID} *Dovrete progettare voi un formato JSON adeguato per questo output.*

L'operazione viene eseguita nel contesto della sessione con token {SID}. L'operazione è ammessa anche per gli utenti anonimi (cioè con la sola URL/developers/{ID}\]).

##### VERBO: PUT

Aggiorna il profilo dell'utente {ID}. Il payload è lo stesso oggetto utilizzato per l'inserimento (POST) tramite la URL auth/{SID}/users.

L'operazione viene eseguita nel contesto della sessione con token {SID} e solo se l'utente associato è un amministratore (altrimenti potreste restituire un errore 403 - *Forbidden*).

##### VERBO: DELETE

Elimina il profilo dell'utente {ID}.

L'operazione viene eseguita nel contesto della sessione con token {SID} e solo se l'utente associato è un amministratore (altrimenti potreste restituire un errore 403 - *Forbidden*).

**Ognuna delle URL che seguono potrà essere preceduta dal prefisso auth/{SID}/, in cui {SID} è un token di sessione restituito da auth/login. Se presente, questo segmento farà sì che le operazioni richieste siano eseguite nel contesto della sessione con token {SID} e quindi** **con i permessi dell'utente associato. In assenza di questo prefisso, il sistema eseguirà le operazioni come se fossero richieste da un utente anonimo (con le relative restrizioni!).** *Suggerimento: se strutturate bene il servizio, sarà semplice rispondere alle URL seguenti con o senza il prefisso di autenticazione usando lo spesso codice.*

#### URL:/projects\[?name={NAME}\&skills={SKILLS}\]

##### VERBO: GET

Restituisce la lista delle URI che rappresentano tutti i progetti presenti nel sistema, opzionalmente filtrati in base a parte del nome {NAME} e/o a una lista di skill associate ai suoi tasks ({SKILLS}, della forma skill1,skill2).

##### VERBO: POST

Inserisce un nuovo progetto. Il payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL projects/{ID}. I filtri , in questo caso, non sono ammessi (se presenti potreste restituire un errore 400 -- *Bad Request*).

#### URL: /projects/{ID}

##### VERBO: GET

Restituisce tutte le informazioni inerenti il progetto {ID} *Dovrete progettare voi un formato JSON adeguato per questo output.*

##### VERBO: PUT

Aggiorna le informazioni inerenti il progetto {ID}. Il payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL projects/{ID}. E' ammesso anche un payload parziale, nel qual caso solo le informazioni specificate nel payload verranno aggiornate nel progetto.

##### VERBO: DELETE

Cancella il progetto {ID} dal catalogo.

#### URL: /projects/{ID}/tasks

##### VERBO: GET

Restituisce la lista dei task associati al progetto {ID}.

Esempio di output:

`[{"name":"Task1","url":" http://socialdevelop.it/projects/1/tasks/1"}, {"name":"Task2","url":" http://socialdevelop.it/projects/1/tasks/245"}]`

##### VERBO: POST

Aggiunge un task al progetto {ID}. Il payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL projects/{ID}/tasks/{TASKID}.

#### URL: /projects/{ID}/tasks/{TASKID}

##### VERBO: GET

Restituisce tutte le informazioni inerenti il task {TASKID} del progetto {ID} *Dovrete progettare voi un formato JSON adeguato per questo output.*

##### VERBO: PUT

Aggiorna le informazioni inerenti il task {TASKID} del progetto {ID}. Il payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL projects/{ID}/tasks/{TASKID}. E' ammesso anche un payload parziale, nel qual caso solo le informazioni specificate nel payload verranno aggiornate nel task.

##### VERBO: DELETE

Cancella il task {TASKID} dal progetto {ID}.

#### URL: /projects/{ID}/tasks/{TASKID}/collaborators

##### VERBO: GET

Restituisce la lista dei collaboratori associati al task {TASKID} del progetto {ID}.

Esempio di output:

`[{"name":"Pinco Pallino","url":"http://socialdevelop.it/developers/21"},{"name":"Paolino Paperino","url":" http://socialdevelop.it/developers/141"}]`

##### VERBO: POST

* Se l'utente che effettua la richiesta *è il proprietario* del progetto {ID}, crea una *proposta* di collaborazione sul task {TASKID} indirizzata allo sviluppatore la cui URI è specificata nel payload ({"developer":"http://socialdevelop.it/developers/21"}).

* Se l'utente che effettua la richiesta *non è il proprietario* del progetto {ID}, crea una *domanda* di collaborazione sul task {TASKID} da parte dell'utente corrente.

#### URL: /projects/{ID}/tasks/{TASKID}/collaborators/{DEVELOPERID}

##### VERBO: PUT

* Se l'utente che effettua la richiesta *è il proprietario* del progetto {ID}

  * Se la collaborazione con {DEVELOPERID} sul task {TASKID} del progetto {ID} è *attiva* , *aggiorna la valutazione* dello sviluppatore con un payload del tipo {"evaluation":5}

  * Se la collaborazione con {DEVELOPERID} sul task {TASKID} del progetto {ID} è una *domanda*, ne aggiorna lo stato con un payload del tipo {"accepted":true} o {"accepted":false}

  * Se l'utente che effettua la richiesta *è lo sviluppatore* {DEVELOPERID} e la sua collaborazione sul progetto {ID} è una *proposta*, ne aggiorna lo stato con un payload del tipo {"accepted":true} o {"accepted":false}

##### VERBO: DELETE

* Se l'utente che effettua la richiesta *è il* *proprietario* del progetto {ID}, cancella la collaborazione, o la proposta di collaborazione con {DEVELOPERID} sul task {TASKID} del progetto {ID}.

* Se l'utente che effettua la richiesta *è lo sviluppatore* {DEVELOPERID}, cancella la sua domanda di collaborazione sul task {TASKID} del progetto {ID}.

#### URL: /projects/{ID}/messages\[?from={FROMDATE}\&to={TODATE}\]

##### VERBO: GET

Restituisce la lista dei messaggi inseriti nel progetto {ID}, opzionalmente filtrati per data iniziale e/o finale. In base al livello di accesso dell'utente potranno essere restituiti solo i messaggi pubblici o anche quelli privati.

Esempio di output:

`[{"title":"Pinco Pallino", "date":"2017-05-05", "url":" http://socialdevelop.it/projects/1/messages/1"},{"title":"Paolino Paperino", "date":"2017-05-05","url":" http://socialdevelop.it/projects/1/messages/2"}]`

##### VERBO: POST

Posta un messaggio nel progetto {ID}. Il payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL projects/{ID}/messages/{MESSAGEID}.

#### URL: /projects/{ID}/messages/{MESSAGEID}

##### VERBO: GET

Restituisce il messaggio {MESSAGEID} del progetto {ID} nella sua forma completa. *Dovrete progettare voi un formato JSON adeguato per questo output.*

##### VERBO: DELETE

Cancella il messaggio {MESSAGEID} dal progetto {ID}.

#### URL: /developers/{DEVELOPERID}/projects

##### VERBO: GET

Restituisce la lista dei progetti su cui lo sviluppatore {DEVELOPERID} ha lavorato o lavora, con eventuale valutazione finale.

Esempio di output:

`[{"project":" http://socialdevelop.it/projects/1/tasks/1"}, {"project":" http://socialdevelop.it/projects/1/tasks/2","evaluation":4}]`

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
