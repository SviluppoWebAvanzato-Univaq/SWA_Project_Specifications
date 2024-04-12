# Corso di Sviluppo Web Avanzato<br/>*Progetti A.A. 2023/2024* - Specifica 1

<section class="specifica">

## Progetto "WebMarket Services"
> Versione 1.0

### Premessa

Lo scopo del corso è insegnare a *progettare, specificare formalmente e sviluppare servizi*, 
intesi come Web API RESTful: la logica di business sottostante il servizio, quindi, 
non è di nostro interesse, e potrà essere implementata in maniera molto prototipale. 
Sarà quindi effettivamente necessario solo implementare le strutture dati su cui basare
l'API e la logica con cui manipolarle, anche senza generare risultati reali, come visto a lezione. 

Va notato che il presente progetto si basa sulla specifica di un progetto del corso di Web Engineering. 
Per prima cosa, quindi, potrebbe essere utile leggere questa specifica di base. Tuttavia, come già detto,
non si dovrà realizzare la *web application* descritta da questa specifica base, ma solo la API descritta
nella presente specifica. 

Si dovrà infine implementare un semplicissimo client per
questo servizio, utile anche a testarne la correttezza.

### Il Servizio

A partire dalle strutture dati del progetto *WebMarket*
(che non dovete necessariamente implementare: siete liberi di usare il codice e il database
realizzato da qualche vostro collega, oppure di abbozzare della logica con i soli elementi 
necessari a supportare le operazioni specificate), si 
costruisca il servizio RESTful specificato di seguito. 

<section class="operazioni">



1. Login/logout con username e password.
2. Inserimento di una *richiesta di acquisto* (comprensiva di categoria di prodotto, di tutte le caratteristiche richieste per quel tipo di prodotto e delle eventuali note)
3. Associazione di una richiesta di acquisto a un tecnico incaricato  
4. Inserimento e modifica (da parte del tecnico incaricato) di una *proposta di acquisto* associata a una richiesta 
5. Approvazione (da parte dell'ordinante) di una *proposta di acquisto*
6. Eliminazione di una *richiesta di acquisto* dal sistema
7. Estrazione lista delle richieste di acquisto *in corso* (non chiuse) di un determinato ordinante
8. Estrazione lista delle richieste di acquisto non ancora assegnate ad alcun tecnico
9. Estrazione di tutti i dettagli di una richiesta di acquisto (richiesta iniziale, eventuale prodotto candidato, approvazione/rifiuto dell'ordinante con relativa motivazione)
10. Estrazione lista richieste di acquisto gestite da un determinato tecnico


</section>

Prestate attenzione alla *struttura delle URL*, che
deve seguire per quanto possibile il paradigma *collection-item*, evidenziando le
relazioni tra gli oggetti tramite nidificazione.

Se alcune operazioni necessitano di accesso con
autenticazione, potrete usare uno qualsiasi degli approcci visti a lezione per
trasmettere il *token di sessione/autenticazione*, precedentemente
ottenuto tramite la procedura (API) di login: ad esempio passare tale token 
con l'header *Authentication*.  

Questa è una specifica di base. Potete aggiungere altre
funzionalità a vostra scelta che sfruttino le altre API del servizio, se
volete, aumentando così il valore del progetto. 



### Il Client

Si implementi un semplice client di test (preferibilmente Javascript), 
che interagisce con le API principali del servizio appena descritto.


  <section class="client">
  


In particolare, il client dovrà permettere di effettuare la login come ordinante ed eseguire opportunamente le operazioni 5, 6, 7 e 9 (quest'ultima relativa ovviamente a una richiesta dell'utente connesso).  
  
 Nota bene: il *test client* general-purpose inserito all'interno degli 
esempi del corso non è valido in quest'ambito, in quanto non richiede alcuna 
programmazione per poter essere adattato ad altri servizi!

  </section>

<section class="indicazioni break">

# Indicazioni per lo Sviluppo del Progetto

### Tecnologie da utilizzare

- Per la programmazione del *server*, si potranno utilizzare
JAVA (consigliato) o PHP. Sviluppando in Java, si utilizzi
preferibilmente un'implementazione di *JAX-RS* (come Jersey), che permette
di realizzare servizi con una struttura più modulare. L'uso di framework come *Spring*
per creare il servizio è ammesso, ma solo se il progetto dovrà essere integrato
o esteso, ad esempio con quello di un altro corso. In PHP, invece, è possibile
usare un framework come *Slim* o un altro a scelta.  

- Per la programmazione del *client*, si possono utilizzare Javascript con o senza JQuery (consigliato),
ma anche Java e PHP. Si possono liberamente includere nel progetto altre librerie e plugin, a patto che
nella relazione siano citate e descritte.

### Svolgimento e Documentazione del Progetto

Le specifiche fornite potrebbero non risultare esaustive o
completamente definite. Ogni funzionalità aggiunta o raffinata, anche tramite
l'interazione con il committente o con gli utenti finali del sito, sarà
adeguatamente valutata. Tutte le scelte progettuali vanno comunque discusse e
motivate.

Il progetto, svolto secondo le linee guida date dalle
specifiche, dovrà essere consegnato nella forma di una o più web application completamente
funzionanti, i cui contenuti e le cui caratteristiche saranno valutati in sede
d'esame. Le parti della specifica marcate come *opzionali*, se omesse, non
renderanno il progetto insufficiente ma non gli permetteranno comunque di
raggiungere il massimo dei voti. Nel caso si decida di realizzarle, non sarà
necessario che siano perfette o complete, ma che dimostrino chiaramente il
vostro impegno nell'affrontare una tematica avanzata.

La documentazione (**in formato elettronico**) che
accompagna il progetto **deve** contenere almeno le seguenti informazioni:

1. Indicazione delle **dipendenze** software (di quali librerie avete
bisogno dal lato server e client?).

2. Indicazione delle **funzionalità realizzate** e di quelle
eventualmente non realizzate. Descrizione dettagliata delle eventuali
funzionalità extra o opzionali inserite nel progetto.

3. **Specifica dettagliata dell'API** redatta in formato *OpenAPI*
(ad esempio tramite il tool di *Swagger*, <https://swagger.io/tools>): 
per ogni operazione realizzata dovrete specificare

   - la URL,

   - gli eventuali parametri, dove sono inseriti (nel path o nella query string) e i loro possibili valori,

   - il verbo HTTP usato,

   - il formato del payload di richiesta (se necessario),

   - i codici di stato http ritornati e, per ogni stato, il formato dell'output (se presente).

Per quel che riguarda il formato dei dati scambiati (payload e output) fornite sempre un esempio completo della struttura JSON che intendete usare.

Nel caso di gruppi di lavoro composti da più componenti, *il contributo effettivo offerto da ciascun componente* alla realizzazione
finale **deve** essere descritto nella documentazione. In sede di esame, i
responsabili potranno essere chiamati a riferire sugli aspetti loro delegati.  

### Valutazione del Progetto

Nel valutare il progetto consegnato saranno prese in
considerazione le seguenti caratteristiche (in ordine di importanza):
1. Rispetto delle specifiche.
2. Correttezza tecnica.
3. Uso appropriato delle tecnologie di base descritte nel corso.

A questa valutazione si aggiungerà quella generale derivata
dalla discussione del progetto in sede d'esame.  


### Ulteriori Informazioni

Questa specifica è disponibile nel repository del corso di Sviluppo Web Avanzato, 
all'indirizzo https://github.com/SviluppoWebAvanzato-Univaq/SWA_Project_Specifications. Ulteriori informazioni e chiarimenti
sulle specifiche possono essere richiesti direttamente via email all'indirizzo giuseppe.dellapenna@univaq.it.

Si ricorda che i progetti vanno svolti in *piccoli*
gruppi (uno o due componenti). Eccezioni a questa regola andranno concordate direttamente col docente.

</section>

