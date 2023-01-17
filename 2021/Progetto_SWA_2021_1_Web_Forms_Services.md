# Corso di Sviluppo Web Avanzato
*Progetti A.A. 2021/2022* - Specifica 1

## Progetto "Web Forms Services"

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

A partire dalla base di dati del progetto *Web of Forms*
(che non dovete necessariamente realizzare ex novo se non siete studenti del
corso di Web Engineering: siete liberi di usare il codice e il database
realizzato da qualche vostro collega), si
costruisca il servizio RESTful
specificato di seguito. La logica del servizio potrà, opzionalmente, utilizzare
una libreria ORM per accedere ai dati.
1. Login/logout con username e password per gli utenti.
2. Upload template da parte di un utente, con definizione dei relativi campi (con una singola operazione: il template, sotto forma di stringa, e la definizione dei campi dovranno essere opportunamente strutturati nel JSON).
3. Approvazione di un template da parte di un amministratore.
4. Elenco dei template disponibili nel sistema (nome e descrizione).
5. Dettaglio di un template (nome, descrizione e lista dei campi con tutti i loro dettagli).
6. Eliminazione di un template (funzione disponibile solo all'autore del modulo e agli amministratori).
7. Aggiornamento di un template (funzione disponibile solo all'autore del modulo e agli amministratori).
8. Compilazione di un template (fornendo i valori dei campi come *payload* della richiesta) in modalità (cioè con valore di ritorno) HTML e PDF (due *endpoint* diversi).
9. Invio feedback su un modello (da parte di un utente).
10. Lettura feedback relativi a un modello (funzione disponibile solo all'autore del modulo e agli amministratori).

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
In particolare, il client dovrà permettere di navigare tra i moduli, vederne i dettagli, inserire i valori dei campi e scaricare quantomeno la versione HTML compilata.

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
