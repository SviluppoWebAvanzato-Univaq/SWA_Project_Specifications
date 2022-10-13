---
titolo: Internship tutor services
numero: 1
versione: 1.0
lingua: IT
anno: 2017
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: Internship tutor 
---

-------
{#operazioni}

Nota: nella specifica che segue gli elementi tra parentesi graffe
sono *placeholder* , la parentesi quadre indicano elementi opzionali e le
parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per
indicare delle alternative. Se non altrimenti specificato, il formato dati per
l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere
al meglio i dati richiesti in questo formato.

#### URL: auth

##### VERBO: POST

Prende in input un oggetto contenente username e
password di un utente e restituisce un oggetto contenente il relativo *token*
di sessione/autenticazione.

#### URL: auth/\{SID\}

##### VERBO: DELETE

Chiude la sessione relativa al token di sessione/autenticazione
\{SID\}, invalidandolo.

#### URL: aziende

##### VERBO: GET

Restituisce una lista contenente i nomi di tutte le
aziende presenti nel sistema e la URI necessaria per leggerne i dettagli.

Esempi di output:

`[{nome:"PincoPallino srl", url:"http://internships.it/aziende/1"}, {nome:"ZioPaperone spa", url:"http://internships.it/aziende/2"}]`

##### VERBO: POST

Registra una nuova azienda. Il payload dovrà essere una
struttura contente tutti i dati richiesti in fase di registrazione, come
risulta dalla specifica del progetto *Internship tutor*, inclusi username
e password dell'utenza corrispondente. In output, come da standard, ci sarà la
URL necessaria a leggere i dati dell'azienda appena inserita (credenziali
escluse!).

#### URL: aziende/\{ID\}

##### VERBO: GET

Restituisce tutti i dettagli relativi all'azienda
identificata da \{ID\}. Il formato di output potrà essere lo stesso usato per
l'inserimento (POST) sulla URL aziende.

#### URL: auth/\{SID\}/aziende/\{ID\}

##### VERBO: PUT

Modifica le informazioni relative ai dati dell'azienda
identificata da \{ID\}. Il payload potrà essere lo stesso usato per l'inserimento
(POST) sulla URL aziende. L'operazione viene eseguita nel contesto della
sessione con token \{SID\} e, in base al tipo di utente associato, permetterà di
aggiornare solo un insieme specifico di dettagli: l'azienda potrà modificare
tutti i dati inizialmente inseriti, tranne le credenziali di accesso,
l'amministratore solo il flag di "abilitazione alla pubblicazione", mentre gli
studenti riceveranno un errore 403 - *Forbidden*.

#### URL: studenti

##### VERBO: POST

Registra un nuovo studente. Il payload dovrà essere una
struttura contente tutti i dati richiesti in fase di registrazione, come
risulta dalla specifica del progetto *Internship tutor*, inclusi username
e password dell'utenza corrispondente. In output, come da standard, ci sarà la
URL necessaria a manipolare i dati dell'utente appena inserito (credenziali
escluse!).

#### URL: auth/\{SID\}/studenti/\{ID\}

##### VERBO: GET

Restituisce tutti i dettagli relativi allo studente
indetificato da \{ID\}. Il formato di output potrà essere lo stesso usato per
l'inserimento (POST) sulla URL studenti. L'operazione viene eseguita nel
contesto della sessione con token \{SID\} e, in base al tipo di utente associato,
permetterà di leggere solo un insieme specifico di dettagli: le aziende
potranno leggere tutti i dati dello studente solo nel caso in cui risulti
attivo un tirocinio presso di loro, mentre l'amministratore e l'utente stesso
potranno vedere tutti i dati. Negli altri casi, verrà generato un errore 403 - *Forbidden*.

#### URL: offerte/\{ID\}

##### VERBO: GET

Restituisce i dettagli dell'offerta identificata da
\{ID\}, come definiti nella specifica del progetto *Internship tutor*.

#### URL: offerte? \{FILTER\}\[first=\{m\}\[\&last=\{n\}\]\]

##### VERBO: GET

Restituisce una lista di tutte le offerte di tirocinio
presenti nel sistema. Ciascun elemento della lista avrà la struttura vista per
la GET sulla URL offerte/\{ID\}. La lista potrà essere ulteriormente filtrata
tramite la query string \{FILTER\} che può contenere tutti i parametri previsti
per la ricerca sulla bacheca offerte specificati dal progetto *Internship
tutor* (ad esempio città, durata, ecc.).

Poiché gli elementi della lista potrebbero essere
molti, adotteremo una *strategia di paginazione* . Includendo i parametri *first*
ed eventualmente *last* , la lista conterrà solo gli annunci dal numero *m*
al numero *n* inclusi (supponendo qualche tipo di ordinamento tra gli
annunci, magari per data). Omettendo *last* , verranno restituiti tutti gli
annunci dal numero *m* all'ultimo. Solo omettendo entrambi i parametri
verrà restituita la lista completa.

#### URL: auth/\{SID\}/offerte

##### VERBO: POST

Inserisce un'offerta di tirocinio relativo all'azienda
autenticata nel contesto della sezione \{SID\}. Se la sessione non corrisponde a
un'azienda abilitata alla pubblicazione, genera un errore un errore 403 - *Forbidden*.
Il payload potrà essere lo stesso tipo di struttura usato per la lettura (GET)
sulla URL offerte.

#### URL: aziende/\{ID\}/offerte

##### VERBO: GET

Restituisce una lista contenente tutte le offerte di tirocinio
pubblicate dell'azienda identificata da \{ID\}.

#### URL: auth/\{SID\}/offerte/\{ID\}/candidati

##### VERBO: POST

Inserisce la candidatura per il tirocinio \{ID\} dello
studente autenticato nel contesto della sessione \{SID\}. Se la sessione non
corrisponde a uno studente, genera un errore un errore 403 - *Forbidden* .
Il payload dovrà contenere tutti i dati necessari alla definizione della
candidatura, come risulta dalla specifica del progetto *Internship tutor.*

#### URL: auth/\{SID\}/offerte/\{IDt\}/candidati/\{IDc\}

##### VERBO: PUT

Consente all'azienda autenticata nel contesto della
sessione \{SID\} di accettare la candidatura \{IDc\} per il tirocinio \{IDt\}. Il
payload dovrà contenere tutti i dati necessari alla finalizzazione del progetto
formativo, come risulta dalla specifica del progetto *Internship tutor.* Se
la sessione non corrisponde a un'azienda, o se l'offerta di tirocinio non è
stata pubblicata dall'azienda in sessione, genera un errore un errore 403 - *Forbidden*.

##### VERBO: DELETE

Consente all'azienda autenticata nel contesto della
sessione \{SID\} di respingere la candidatura \{IDc\} per il tirocinio \{IDt\}. Se la
sessione non corrisponde a un'azienda, o se l'offerta di tirocinio non è stata
pubblicata dall'azienda in sessione, genera un errore un errore 403 -- *Forbidden* o*400 -- Bad Request.*

#### URL: auth/\{SID\}/offerte/\{IDt\}/candidati/\{IDc\}/progetto-formativo

##### VERBO: GET

Consente all'azienda o allo studente autenticati nel
contesto della sessione \{SID\} di scaricare il progetto formativo relativo alla
candidatura \{IDc\} per il tirocinio \{IDt\}. Se la sessione non corrisponde
all'azienda che offre il tirocinio specificato o allo studente che ha
presentato la relativa candidatura, che deve essere stata approvata, genera un errore
un errore 403 - *Forbidden*.  


Come vedrete, alcune URL contengono un segmento "*auth/\{SID\}"* .
Questo realizza un semplice schema di autenticazione che useremo per verificare
le autorizzazioni necessarie a eseguire certe operazioni. Tutte le URL senza il
prefisso *auth/\{SID\}* corrispondono a operazioni accessibili agli anonimi.
Ove presente, invece, questo prefisso indica che la URL deve contenere un *token
di sessione/autenticazione* , precedentemente ottenuto tramite un POST sulla
URL *auth*, che simula un vero e proprio login, associando al token
restituito i dati dell'utente autenticato.


-------
{#client}


Il client dovrà essere strutturato come segue:
1. Un
selettore permetterà di scegliere un'azienda
2. Sarà
visualizzata una pagina (o un popup) di dettaglio relativo all'azienda
selezionata dal controllo al punto (1).
3. Selezionando
un apposito controllo presente nel dettaglio dell'azienda visualizzato come al
punto (2), sarà visualizzata la lista delle offerte relative all'azienda stessa.
*Per evitare eccessivo numero di chiamate al servizio, è opportuno paginare
questa lista.*
