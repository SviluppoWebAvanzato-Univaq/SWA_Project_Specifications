# Corso di Sviluppo Web Avanzato<br/>*Progetti A.A. 2024/2025* - Specifica 1

<section class="specifica">

## Progetto "SoccorsoWeb Services"
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

A partire dalle strutture dati del progetto *SoccorsoWeb*
(che non dovete necessariamente implementare: siete liberi di usare il codice e il database
realizzato da qualche vostro collega, oppure di abbozzare della logica con i soli elementi 
necessari a supportare le operazioni specificate), si 
costruisca il servizio RESTful specificato di seguito. 

<section class="operazioni">



1. Login/logout con username e password.
2. Inserimento di una *richiesta di soccorso*.
3. Convalida di una richiesta di soccorso (questo endpoint deve ricevere le richieste provenienti dai click sui link di convalida descritti nella specifica).
4. Lista (paginata) delle richieste di soccorso, filtrata in base alla tipologia (attive, in corso, chiuse, ignorate).
5. Lista delle richieste di soccorso chiuse con risultato non totalmente positivo (livello di successo minore di 5).
6. Lista degli operatori attualmente liberi.
7. Creazione di una missione.
8. Chiusura di una missione in corso.
9. Annullamento di una richiesta di soccorso (da parte dell'amministratore).
10. Dettagli di una missione.
11. Dettagli una richiesta di soccorso.
12. Dettagli di un operatore.
13. Lista delle missioni in cui un operatore è stato coinvolto.


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
  


In particolare, il client dovrà permettere di eseguire opportunamente le operazioni 1, 2, 4 e 6.  
  
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

### Consegna del Progetto

La *documentazione* del progetto (e in particolare *la specifica OpeAPI*), redatta come indicato nelle sezioni precedenti,
dovrà essere consegnata al docente **almeno due giorni prima** della data d'esame, inviandola semplicemente per email.   
*Non verranno in alcun caso concesse proroghe a questo termine*, perchè concedere 
un giorno in più a uno studente vorrebbe dire svantaggiare tutti gli altri che hanno 
consegnato per tempo o hanno organizzato i loro impegni per farlo, e che avrebbero 
anch'essi sicuramente beneficiato di un po' di tempo aggiuntivo, quantomeno per 
migliorare o rifinire il proprio lavoro.

*Non è necessario consegnare il codice*, che verrà analizzato e provato in sede d'esame (portate quindi con voi una copia funzionante
del progetto sul vostro PC portatile), ma è utile (se possibile) allegare alla documentazione la URL di un repository pubblico da 
cui questo può essere scaricato e ispezionato se necessario.

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

