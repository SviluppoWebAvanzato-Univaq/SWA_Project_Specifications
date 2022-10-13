---
titolo: SocialDevelopServices
numero: 2
versione: 1.0
lingua: IT
anno: 2016
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: SocialDevelop
---

-------
{#operazioni}


Note: nella specifica che segue gli elementi tra parentesi graffe
sono *placeholder* , la parentesi quadre indicano elementi opzionali e le
parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per
indicare delle alternative. Se non altrimenti specificato, il formato dati per
l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere
al meglio i dati richiesti in questo formato.

#### URL:/auth/login

##### VERBO: POST

Prende in input un oggetto contenente username e
password di un utente e restituisce un oggetto contenente il relativo *token*
di sessione/autenticazione.

#### URL:/auth/logout

##### VERBO: POST

Prende in input un token di sessione/autenticazione e
chiude la relativa sessione, invalidando il token.

#### URL:/auth/\{SID\}/developers\[?skills=\{SKILLS\}\]

##### VERBO: GET

Restituisce la lista degli utenti noti al sistema,
opzionalmente filtrati in base ai loro \{SKILLS\}.

Esempio di output:

`[{"name":"Pinco Pallino","url":"http://socialdevelop.it/developers/21"},{"name":"Paolino Paperino","url":" http://socialdevelop.it/developers/141"}]`

L'operazione viene eseguita nel contesto della sessione
con token \{SID\}. L'operazione è ammessa anche per gli utenti anonimi (cioè con
la sola URL/developers\[?skills=\{SKILLS\}\]).

##### VERBO: POST

Inserisce un nuovo utente nel sistema. L'oggetto
passato come payload conterrà tutti i dati del profilo utente, ad esempio

{"username": "pinco", "password": "in_chiaro!"}

L'operazione viene eseguita nel contesto della sessione
con token \{SID\} e solo se l'utente associato è un amministratore (altrimenti
potreste restituire un errore 403 - *Forbidden*).

#### URL:/auth/\{SID\}/developers/\{ID\}

##### VERBO: GET

Restituisce il profilo dell'utente \{ID\} *Dovrete
progettare voi un formato JSON adeguato per questo output.*

L'operazione viene eseguita nel contesto della sessione
con token \{SID\}. L'operazione è ammessa anche per gli utenti anonimi (cioè con
la sola URL/developers/\{ID\}\]).

##### VERBO: PUT

Aggiorna il profilo dell'utente \{ID\}. Il payload è lo
stesso oggetto utilizzato per l'inserimento (POST) tramite la URL auth/\{SID\}/users.

L'operazione viene eseguita nel contesto della sessione
con token \{SID\} e solo se l'utente associato è un amministratore (altrimenti
potreste restituire un errore 403 - *Forbidden*).

##### VERBO: DELETE

Elimina il profilo dell'utente \{ID\}.

L'operazione viene eseguita nel contesto della sessione
con token \{SID\} e solo se l'utente associato è un amministratore (altrimenti
potreste restituire un errore 403 - *Forbidden*).

**Ognuna delle URL che seguono potrà essere preceduta dal
prefisso auth/\{SID\}/, in cui \{SID\} è un token di sessione restituito da
auth/login. Se presente, questo segmento farà sì che le operazioni richieste
siano eseguite nel contesto della sessione con token \{SID\} e quindi** **con
i permessi dell'utente associato. In assenza di questo prefisso, il sistema eseguirà
le operazioni come se fossero richieste da un utente anonimo (con le relative restrizioni!).** *Suggerimento: se strutturate bene il servizio, sarà semplice rispondere
alle URL seguenti con o senza il prefisso di autenticazione usando lo spesso
codice.*

#### URL:/projects\[?name=\{NAME\}\&skills=\{SKILLS\}\]

##### VERBO: GET

Restituisce la lista delle URI che rappresentano tutti
i progetti presenti nel sistema, opzionalmente filtrati in base a parte del
nome \{NAME\} e/o a una lista di skill associate ai suoi tasks (\{SKILLS\}, della
forma skill1,skill2).

##### VERBO: POST

Inserisce un nuovo progetto. Il payload è lo stesso
oggetto restituito dalla lettura (GET) tramite la URL projects/\{ID\}. I filtri
{FILTER}, in questo caso, non sono ammessi (se presenti potreste restituire un
errore 400 -- *Bad Request*).

#### URL: /projects/\{ID\}

##### VERBO: GET

Restituisce tutte le informazioni inerenti il progetto
\{ID\} *Dovrete progettare voi un formato JSON adeguato per questo output.*

##### VERBO: PUT

Aggiorna le informazioni inerenti il progetto \{ID\}. Il
payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL projects/\{ID\}.
E' ammesso anche un payload parziale, nel qual caso solo le informazioni
specificate nel payload verranno aggiornate nel progetto.

##### VERBO: DELETE

Cancella il progetto \{ID\} dal catalogo.

#### URL: /projects/\{ID\}/tasks

##### VERBO: GET

Restituisce la lista dei task associati al progetto
\{ID\}.

Esempio di output:

`[{"name":"Task1","url":" http://socialdevelop.it/projects/1/tasks/1"}, {"name":"Task2","url":" http://socialdevelop.it/projects/1/tasks/245"}]`

##### VERBO: POST

Aggiunge un task al progetto \{ID\}. Il payload è lo
stesso oggetto restituito dalla lettura (GET) tramite la URL projects/\{ID\}/tasks/\{TASKID\}.

#### URL: /projects/\{ID\}/tasks/\{TASKID\}

##### VERBO: GET

Restituisce tutte le informazioni inerenti il task
\{TASKID\} del progetto \{ID\} *Dovrete progettare voi un formato JSON adeguato per
questo output.*

##### VERBO: PUT

Aggiorna le informazioni inerenti il task \{TASKID\} del
progetto \{ID\}. Il payload è lo stesso oggetto restituito dalla lettura (GET)
tramite la URL projects/\{ID\}/tasks/\{TASKID\}. E' ammesso anche un payload
parziale, nel qual caso solo le informazioni specificate nel payload verranno
aggiornate nel task.

##### VERBO: DELETE

Cancella il task \{TASKID\} dal progetto \{ID\}.

#### URL: /projects/\{ID\}/tasks/\{TASKID\}/collaborators

##### VERBO: GET

Restituisce la lista dei collaboratori associati al
task \{TASKID\} del progetto \{ID\}.

Esempio di output:

`[{"name":"Pinco Pallino","url":"http://socialdevelop.it/developers/21"},{"name":"Paolino Paperino","url":" http://socialdevelop.it/developers/141"}]`

##### VERBO: POST

- Se l'utente che effettua la richiesta *è il proprietario*
del progetto \{ID\}, crea una *proposta* di collaborazione sul task \{TASKID\}
indirizzata allo sviluppatore la cui URI è specificata nel payload
({"developer":"http://socialdevelop.it/developers/21"}).

- Se l'utente che effettua la richiesta *non è il proprietario*
del progetto \{ID\}, crea una *domanda* di collaborazione sul task \{TASKID\}
da parte dell'utente corrente.

#### URL: /projects/\{ID\}/tasks/\{TASKID\}/collaborators/\{DEVELOPERID\}

##### VERBO: PUT

- Se l'utente che effettua la richiesta *è il proprietario*
del progetto \{ID\}

   - Se la collaborazione con \{DEVELOPERID\} sul task \{TASKID\} del
progetto \{ID\} è *attiva* , *aggiorna la valutazione* dello
sviluppatore con un payload del tipo {"evaluation":5}

   - Se la collaborazione con \{DEVELOPERID\} sul task \{TASKID\} del
progetto \{ID\} è una *domanda*, ne aggiorna lo stato con un payload del
tipo {"accepted":true} o {"accepted":false}

   - Se l'utente che effettua la richiesta *è lo sviluppatore* \{DEVELOPERID\}
e la sua collaborazione sul progetto \{ID\} è una *proposta*, ne aggiorna lo
stato con un payload del tipo {"accepted":true} o {"accepted":false}

##### VERBO: DELETE

- Se l'utente che effettua la richiesta *è il* *proprietario*
del progetto \{ID\}, cancella la collaborazione, o la proposta di collaborazione
con \{DEVELOPERID\} sul task \{TASKID\} del progetto \{ID\}.

- Se l'utente che effettua la richiesta *è lo sviluppatore* \{DEVELOPERID\},
cancella la sua domanda di collaborazione sul task \{TASKID\} del progetto \{ID\}.

#### URL: /projects/\{ID\}/messages\[?from=\{FROMDATE\}\&to=\{TODATE\}\]

##### VERBO: GET

Restituisce la lista dei messaggi inseriti nel progetto
\{ID\}, opzionalmente filtrati per data iniziale e/o finale. In base al livello
di accesso dell'utente potranno essere restituiti solo i messaggi pubblici o
anche quelli privati.

Esempio di output:

`[{"title":"Pinco Pallino", "date":"2017-05-05", "url":" http://socialdevelop.it/projects/1/messages/1"},{"title":"Paolino Paperino", "date":"2017-05-05","url":" http://socialdevelop.it/projects/1/messages/2"}]`

##### VERBO: POST

Posta un messaggio nel progetto \{ID\}. Il payload è lo
stesso oggetto restituito dalla lettura (GET) tramite la URL
projects/\{ID\}/messages/\{MESSAGEID\}.

#### URL: /projects/\{ID\}/messages/\{MESSAGEID\}

##### VERBO: GET

Restituisce il messaggio \{MESSAGEID\} del progetto \{ID\}
nella sua forma completa. *Dovrete progettare voi un formato JSON adeguato per
questo output.*

##### VERBO: DELETE

Cancella il messaggio \{MESSAGEID\} dal progetto \{ID\}.

#### URL: /developers/\{DEVELOPERID\}/projects

##### VERBO: GET

Restituisce la lista dei progetti su cui lo
sviluppatore \{DEVELOPERID\} ha lavorato o lavora, con eventuale valutazione
finale.

Esempio di output:

`[{"project":" http://socialdevelop.it/projects/1/tasks/1"}, {"project":" http://socialdevelop.it/projects/1/tasks/2","evaluation":4}]`

