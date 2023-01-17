# Corso di Sviluppo Web Avanzato
*Progetti A.A. 2017/2018* - Specifica 1

## Progetto "Internship tutor services"

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

A partire dalla base di dati del progetto \*Internship tutor \*
(che non dovete necessariamente realizzare ex novo se non siete studenti del
corso di Web Engineering: siete liberi di usare il codice e il database
realizzato da qualche vostro collega), si
costruisca il servizio RESTful
specificato di seguito. La logica del servizio potrà, opzionalmente, utilizzare
una libreria ORM per accedere ai dati.
Nota: nella specifica che segue gli elementi tra parentesi graffe sono *placeholder* , la parentesi quadre indicano elementi opzionali e le parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per indicare delle alternative. Se non altrimenti specificato, il formato dati per l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere al meglio i dati richiesti in questo formato.

#### URL: auth

##### VERBO: POST

Prende in input un oggetto contenente username e password di un utente e restituisce un oggetto contenente il relativo *token* di sessione/autenticazione.

#### URL: auth/{SID}

##### VERBO: DELETE

Chiude la sessione relativa al token di sessione/autenticazione {SID}, invalidandolo.

#### URL: aziende

##### VERBO: GET

Restituisce una lista contenente i nomi di tutte le aziende presenti nel sistema e la URI necessaria per leggerne i dettagli.

Esempi di output:

`[{nome:"PincoPallino srl", url:"http://internships.it/aziende/1"}, {nome:"ZioPaperone spa", url:"http://internships.it/aziende/2"}]`

##### VERBO: POST

Registra una nuova azienda. Il payload dovrà essere una struttura contente tutti i dati richiesti in fase di registrazione, come risulta dalla specifica del progetto *Internship tutor*, inclusi username e password dell'utenza corrispondente. In output, come da standard, ci sarà la URL necessaria a leggere i dati dell'azienda appena inserita (credenziali escluse!).

#### URL: aziende/{ID}

##### VERBO: GET

Restituisce tutti i dettagli relativi all'azienda identificata da {ID}. Il formato di output potrà essere lo stesso usato per l'inserimento (POST) sulla URL aziende.

#### URL: auth/{SID}/aziende/{ID}

##### VERBO: PUT

Modifica le informazioni relative ai dati dell'azienda identificata da {ID}. Il payload potrà essere lo stesso usato per l'inserimento (POST) sulla URL aziende. L'operazione viene eseguita nel contesto della sessione con token {SID} e, in base al tipo di utente associato, permetterà di aggiornare solo un insieme specifico di dettagli: l'azienda potrà modificare tutti i dati inizialmente inseriti, tranne le credenziali di accesso, l'amministratore solo il flag di "abilitazione alla pubblicazione", mentre gli studenti riceveranno un errore 403 - *Forbidden*.

#### URL: studenti

##### VERBO: POST

Registra un nuovo studente. Il payload dovrà essere una struttura contente tutti i dati richiesti in fase di registrazione, come risulta dalla specifica del progetto *Internship tutor*, inclusi username e password dell'utenza corrispondente. In output, come da standard, ci sarà la URL necessaria a manipolare i dati dell'utente appena inserito (credenziali escluse!).

#### URL: auth/{SID}/studenti/{ID}

##### VERBO: GET

Restituisce tutti i dettagli relativi allo studente indetificato da {ID}. Il formato di output potrà essere lo stesso usato per l'inserimento (POST) sulla URL studenti. L'operazione viene eseguita nel contesto della sessione con token {SID} e, in base al tipo di utente associato, permetterà di leggere solo un insieme specifico di dettagli: le aziende potranno leggere tutti i dati dello studente solo nel caso in cui risulti attivo un tirocinio presso di loro, mentre l'amministratore e l'utente stesso potranno vedere tutti i dati. Negli altri casi, verrà generato un errore 403 - *Forbidden*.

#### URL: offerte/{ID}

##### VERBO: GET

Restituisce i dettagli dell'offerta identificata da {ID}, come definiti nella specifica del progetto *Internship tutor*.

#### URL: offerte? {FILTER}\[first={m}\[\&last={n}\]\]

##### VERBO: GET

Restituisce una lista di tutte le offerte di tirocinio presenti nel sistema. Ciascun elemento della lista avrà la struttura vista per la GET sulla URL offerte/{ID}. La lista potrà essere ulteriormente filtrata tramite la query string {FILTER} che può contenere tutti i parametri previsti per la ricerca sulla bacheca offerte specificati dal progetto *Internship tutor* (ad esempio città, durata, ecc.).

Poiché gli elementi della lista potrebbero essere molti, adotteremo una *strategia di paginazione* . Includendo i parametri *first* ed eventualmente *last* , la lista conterrà solo gli annunci dal numero *m* al numero *n* inclusi (supponendo qualche tipo di ordinamento tra gli annunci, magari per data). Omettendo *last* , verranno restituiti tutti gli annunci dal numero *m* all'ultimo. Solo omettendo entrambi i parametri verrà restituita la lista completa.

#### URL: auth/{SID}/offerte

##### VERBO: POST

Inserisce un'offerta di tirocinio relativo all'azienda autenticata nel contesto della sezione {SID}. Se la sessione non corrisponde a un'azienda abilitata alla pubblicazione, genera un errore un errore 403 - *Forbidden*. Il payload potrà essere lo stesso tipo di struttura usato per la lettura (GET) sulla URL offerte.

#### URL: aziende/{ID}/offerte

##### VERBO: GET

Restituisce una lista contenente tutte le offerte di tirocinio pubblicate dell'azienda identificata da {ID}.

#### URL: auth/{SID}/offerte/{ID}/candidati

##### VERBO: POST

Inserisce la candidatura per il tirocinio {ID} dello studente autenticato nel contesto della sessione {SID}. Se la sessione non corrisponde a uno studente, genera un errore un errore 403 - *Forbidden* . Il payload dovrà contenere tutti i dati necessari alla definizione della candidatura, come risulta dalla specifica del progetto *Internship tutor.*

#### URL: auth/{SID}/offerte/{IDt}/candidati/{IDc}

##### VERBO: PUT

Consente all'azienda autenticata nel contesto della sessione {SID} di accettare la candidatura {IDc} per il tirocinio {IDt}. Il payload dovrà contenere tutti i dati necessari alla finalizzazione del progetto formativo, come risulta dalla specifica del progetto *Internship tutor.* Se la sessione non corrisponde a un'azienda, o se l'offerta di tirocinio non è stata pubblicata dall'azienda in sessione, genera un errore un errore 403 - *Forbidden*.

##### VERBO: DELETE

Consente all'azienda autenticata nel contesto della sessione {SID} di respingere la candidatura {IDc} per il tirocinio {IDt}. Se la sessione non corrisponde a un'azienda, o se l'offerta di tirocinio non è stata pubblicata dall'azienda in sessione, genera un errore un errore 403 -- *Forbidden* o*400 -- Bad Request.*

#### URL: auth/{SID}/offerte/{IDt}/candidati/{IDc}/progetto-formativo

##### VERBO: GET

Consente all'azienda o allo studente autenticati nel contesto della sessione {SID} di scaricare il progetto formativo relativo alla candidatura {IDc} per il tirocinio {IDt}. Se la sessione non corrisponde all'azienda che offre il tirocinio specificato o allo studente che ha presentato la relativa candidatura, che deve essere stata approvata, genera un errore un errore 403 - *Forbidden*.

Come vedrete, alcune URL contengono un segmento "*auth/{SID}"* . Questo realizza un semplice schema di autenticazione che useremo per verificare le autorizzazioni necessarie a eseguire certe operazioni. Tutte le URL senza il prefisso *auth/{SID}* corrispondono a operazioni accessibili agli anonimi. Ove presente, invece, questo prefisso indica che la URL deve contenere un *token di sessione/autenticazione* , precedentemente ottenuto tramite un POST sulla URL *auth*, che simula un vero e proprio login, associando al token restituito i dati dell'utente autenticato.

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

1. Un selettore permetterà di scegliere un'azienda
2. Sarà visualizzata una pagina (o un popup) di dettaglio relativo all'azienda selezionata dal controllo al punto (1).
3. Selezionando un apposito controllo presente nel dettaglio dell'azienda visualizzato come al punto (2), sarà visualizzata la lista delle offerte relative all'azienda stessa. *Per evitare eccessivo numero di chiamate al servizio, è opportuno paginare questa lista.*

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
