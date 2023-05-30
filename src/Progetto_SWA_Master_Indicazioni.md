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
ma anche Java e PHP. Si Possono liberamente includere nel progetto altre librerie e plugin, a patto che
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
gruppi. Eccezioni a questa regola andranno concordate direttamente col docente.
