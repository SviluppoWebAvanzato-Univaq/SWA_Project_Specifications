---
titolo: Guida TV services
numero: 1
versione: 1.0
lingua: IT
anno: 2019
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: Guida TV 
---

-------
{#operazioni}

Nota: nella specifica che segue gli elementi tra parentesi graffe
sono *placeholder* , le parentesi quadre indicano elementi opzionali e le
parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per
indicare delle alternative. Se non altrimenti specificato, il formato dati per
l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere
al meglio i dati richiesti.

#### URL: auth

##### VERBO: POST

Prende in input un oggetto contenente username e
password di un utente e restituisce un oggetto contenente il relativo *token*
di sessione/autenticazione.

#### URL: auth/\{SID\}

##### VERBO: DELETE

Chiude la sessione relativa al token di sessione/autenticazione
\{SID\}, invalidandolo.

#### URL: canali

##### VERBO: GET

Restituisce una lista contenente i nomi di tutti i
canali presenti nel sistema, la relativa ID e la URI necessaria per leggerne il
palinsesto odierno.

Esempi di output:

`[{"nome":"Rai 1", "id":1, "url":"http://miaguidatv.it/palinsesto/20190711/1"},{"nome":"Canale 5", "id":2, "url":" http://miaguidatv.it/palinsesto/20190711/2"}]`

#### URL: auth/\{SID\}/canali

##### VERBO: POST

Inserisce un nuovo canale nel database. Il payload
dovrà essere una struttura contente tutti i dati richiesti dal database per
questo tipo di elemento, come risulta dalla specifica del progetto *Guida TV* .
In output ci sarà la URL necessaria a leggere i dati del palinsesto del canale
appena inserito. L'operazione viene eseguita nel contesto della sessione con
token \{SID\} ed è disponibile solo se l'utente associato è l'amministratore.
Tutti gli altri riceveranno un errore 403 - *Forbidden*.

#### URL: canali/\{ID\}/palinsesto

##### VERBO: GET

E' un alias per palinsesto/\<data odierna\>/\{ID\}.

#### URL: canali/\{ID\}/palinsesto/\{DATA\}

##### VERBO: GET

E' un alias per palinsesto/\{DATA\}/\{ID\}.

#### URL: auth/\{SID\}/canali/\{ID\}

##### VERBO: PUT

Modifica i dati del canale identificato da \{ID\}. Il
payload potrà essere lo stesso usato per l'inserimento (POST) sulla URL auth/\{SID\}/canali.
L'operazione viene eseguita nel contesto della sessione con token \{SID\} ed è
disponibile solo se l'utente associato è l'amministratore. Tutti gli altri
riceveranno un errore 403 - *Forbidden*.

#### URL: palinsesto/\{DATA\}

##### VERBO: GET

Restituisce il palinsesto di tutti i canali per la data
\{DATA\}. Definite un formato JSON di output che strutturi bene tutte le
informazioni necessarie, compresa la URL di dettaglio di ciascun programma:
ricordate, infatti, che nel palinsesto non devono essere presenti tutti i
dettagli di ciascun programma (si veda la specifica del progetto *Guida TV* ).
Se la richiesta riguarda una data non disponibile, si restituisca un errore 404 - *Not Found*.

#### URL: palinsesto/\{DATA\}/\{CANALE\}

##### VERBO: GET

Restituisce il palinsesto del canale \{CANALE\} per la
data \{DATA\}. Se la richiesta riguarda una data o un canale non disponibili, si
restituisca un errore 404 - *Not Found*.

#### URL: palinsesto? \{FILTER\}\[first=\{m\}\[\&last=\{n\}\]\]

##### VERBO: GET

Esegue una ricerca per tutti i programmi nel database
che corrispondono ai criteri della query string \{FILTER\} che può contenere
tutti i parametri previsti per la ricerca di programmi specificati dal progetto
*Guida TV* (ad esempio titolo, genere, canali, ecc.). Nota: poiché una
ricerca senza criteri non ha senso, il servizio dovrà respingere ogni richiesta
che non contenga almeno un filtro.

Restituisce la lista delle URL necessarie ad accedere
al dettaglio di tutti i programmi corrispondenti ai criteri.

Poiché gli elementi della lista potrebbero essere
molti, adotteremo una *strategia di paginazione* . Includendo i parametri *first*
ed eventualmente *last* , la lista conterrà solo gli elementi dal numero *m*
al numero *n* inclusi (supponendo qualche tipo di ordinamento tra gli
elementi, magari per data). Omettendo *last* , verranno restituiti tutti
gli elementi dal numero *m* all'ultimo. Omettendo entrambi i parametri
verrà restituita la lista completa.

#### URL: programmi/\{ID\}

##### VERBO: GET

Restituisce tutti i dettagli relativi al programma
indentificato da \{ID\} (titolo, descrizione, data/ora/canale di messa in onda,
ecc).

#### URL: auth/\{SID\}/programmi

##### VERBO: POST

Inserisce un nuovo programma nel database. Il payload
dovrà essere una struttura contente tutti i dati richiesti dal database per
questo tipo di elemento, come risulta dalla specifica del progetto *Guida TV* .
In output, come da standard, ci sarà la URL necessaria a leggere il dettaglio
del programma appena inserito. L'operazione viene eseguita nel contesto della
sessione con token \{SID\} ed è disponibile solo se l'utente associato è
l'amministratore. Tutti gli altri riceveranno un errore 403 - *Forbidden*.

#### URL: auth/\{SID\}/programmi/\{ID\}

##### VERBO: PUT

Modifica i dati del programma identificato da \{ID\}. Il
payload potrà essere lo stesso usato per l'inserimento (POST) sulla URL auth/\{SID\}/programmi.
L'operazione viene eseguita nel contesto della sessione con token \{SID\} ed è
disponibile solo se l'utente associato è l'amministratore. Tutti gli altri
riceveranno un errore 403 - *Forbidden*.

#### URL: programmi/\{ID\}/episodi

##### VERBO: GET

Se il programma indentificato da \{ID\} è parte di una
serie, restituisce una struttura contenente i dati di messa in onda di tutti
gli episodi di questa serie in un periodo che va da trenta giorni nel passato a
sette giorni nel futuro. Se il programma non ha una serie associata, è
possibile restituire un errore 400 - *Bad Request*.

*Opzionalmente* , provate a scrivere una procedura che popoli il database dei palinsesti attingendo ai
servizi REST di vere emittenti televisive. Ad esempio, per la RAI, la URL <https://www.raiplay.it/palinsesto/app/old/28-10-2019/canali.json>
restituisce il palinsesto di tutte le reti per la data specificata (attenzione:
usate una data recente, altrimenti riceverete un *not found*). 

Come vedrete, alcune URL contengono un segmento "*auth/\{SID\}"* :
questo realizza un semplice schema di autenticazione che useremo per verificare
le autorizzazioni necessarie a eseguire certe operazioni. Tutte le URL senza il
prefisso *auth/\{SID\}* corrispondono a operazioni accessibili agli anonimi.
Ove presente, invece, questo prefisso indica che la URL deve contenere un *token
di sessione/autenticazione*, precedentemente ottenuto tramite un POST sulla
URL *auth*, che simula un vero e proprio login, associando al token
restituito i dati dell'utente autenticato. 

-------
{#client}


Il client dovrà essere strutturato come segue:
1. Un selettore permetterà di scegliere un canale.
2. Sarà visualizzata una pagina col palinsesto odierno del canale.
3. Cliccando su un programma del palinsesto, si aprirà una pagina (o un popup) con i
relativi dettagli.
