---
titolo: CourseWebServices
numero: 1
versione: 1.0
lingua: IT
anno: 2016
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: CourseWeb
---

-------
{#operazioni}


Note: nella specifica che segue gli elementi tra parentesi graffe
sono *placeholder* , la parentesi quadre indicano elementi opzionali e le
parentesi tonde, insieme al carattere *pipe* "\|", sono utilizzate per
indicare delle alternative. Se non altrimenti specificato, il formato dati per
l'input e l'output è JSON, quindi andranno progettate opportune strutture per contenere
al meglio i dati richiesti in questo formato.

#### URL: auth/login

##### VERBO: POST

Prende in input un oggetto contenente username e
password di un utente e restituisce un oggetto contenente il relativo *token*
di sessione/autenticazione.

#### URL: auth/logout

##### VERBO: POST

Prende in input un token di sessione/autenticazione e
chiude la relativa sessione, invalidando il token.

#### URL: auth/\{SID\}/users

##### VERBO: GET

Restituisce la lista delle URI che rappresentano tutti
agli utenti presenti nel sistema.

Esempio di output:

`["http://courseweb.it/auth/123/users/1", "http://courseweb.it/auth/123/users/2", "http://courseweb.it/auth/123/users/3"]`

L'operazione viene eseguita nel contesto della sessione
con token \{SID\} e solo se l'utente associato è un amministratore (altrimenti
potreste restituire un errore 403 - *Forbidden*).

##### VERBO: POST

Inserisce un nuovo utente nel sistema. L'oggetto
passato come payload conterrà tutti i dati del profilo utente, ad esempio

`{"username": "pinco", "password": "in_chiaro!"}`

L'operazione viene eseguita nel contesto della sessione
con token \{SID\} e solo se l'utente associato è un amministratore (altrimenti
potreste restituire un errore 403 - *Forbidden*).

#### URL: auth/\{SID\}/users/\{ID\}

##### VERBO: GET

Restituisce il profilo dell'utente \{ID\} sotto forma
dello stesso oggetto utilizzato come payload per l'inserimento (POST) tramite
la URL auth/\{SID\}/users, ad esclusione della password.

L'operazione viene eseguita nel contesto della sessione
con token \{SID\} e solo se l'utente associato è un amministratore (altrimenti
potreste restituire un errore 403 - *Forbidden*).

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
le operazioni come se fossero richieste da un utente anonimo (con le relative
restrizioni!).** *Suggerimento: se strutturate bene il servizio, sarà
semplice rispondere alle URL seguenti con o senza il prefisso di autenticazione
usando lo spesso codice.*

#### URL: courses/(\{YEAR\}\|current)?{FILTER}

##### VERBO: GET

Restituisce la lista delle URI che rappresentano tutti
i corsi presenti nel sistema, includendo solo quelli con informazioni associate
all'anno accademico corrente (*current*) o a quello specificato con \{YEAR\}.
La lista potrà essere ulteriormente filtrata tramite la query string {FILTER}
che può contenere tutti i parametri previsti per la ricerca sul catalogo dei
corsi nel progetto CourseWeb (ad esempio nome, codice, ecc.).

Esempi di output:

Per la URL *courses/current*

`["http://courseweb.it/courses/current/1", "http://courseweb.it/courses/current/2"]`

Per la URL *courses/2015*

`["http://courseweb.it/courses/2015/1", "http://courseweb.it/courses/2015/3"]`

Per la URL *auth/123/courses/2015*

`["http://courseweb.it/auth/123/courses/2015/1", "http://courseweb.it/auth/123/courses/2015/3"]`

##### VERBO: POST

Inserisce un nuovo corso nel catalogo dell'anno
accademico \{YEAR\} o in quello corrente (*current* ). Il payload è lo stesso
oggetto restituito dalla lettura (GET) tramite la URL courses/(\{YEAR\}\|current)/\{ID\}.
I filtri {FILTER}, in questo caso, non sono ammessi (se presenti potreste
restituire un errore 400 -- *Bad Request*).

#### URL: courses/history/\{ID\}

##### VERBO: GET

Restituisce la lista degli anni accademici per cui
esistono informazioni relative al corso \{ID\} insieme alle URL necessarie ad
accedervi, sotto forma di un oggetto JSON del tipo

`[{"year":2015,url:"http://courseweb.it/courses/2015/1"},{"year":2016,url:"http://courseweb.it/courses/2016/1"}]`

#### URL: courses/(\{YEAR\}\|current)/\{ID\}\[/it\|/en\]

##### VERBO: GET

Restituisce tutte le informazioni inerenti il corso
\{ID\} relative all'anno accademico corrente (*current* ) o a quello
specificato con \{YEAR\}. Aggiungendo uno dei due suffissi /it o /en verranno
restituite solo le informazioni presenti nella lingua corrispondente,
altrimenti l'oggetto conterrà i dati in entrambe le lingue. *Dovrete
progettare voi un formato JSON adeguato per entrambi i casi.*

#### URL: courses/(\{YEAR\}\|current)/\{ID\}

##### VERBO: PUT

Aggiorna le informazioni inerenti il corso \{ID\} relative
all'anno accademico \{YEAR\} o a quello corrente (*current*). Il payload è
lo stesso oggetto restituito dalla lettura (GET) tramite la URL courses/(\{YEAR\}\|current)/\{ID\}.

##### VERBO: DELETE

Cancella il corso \{ID\} dal catalogo dell'anno
accademico \{YEAR\} oppure da quello dell'anno accademico corrente (*current*).

#### URL: courses/(\{YEAR\}\|current)/\{ID\}/basic\[/it\|/en\]

##### VERBO: GET

Restituisce un oggetto contenente le sole informazioni *di
base* (così come descritte nella specifica del progetto CourseWeb), estratte
dall'oggetto restituito dalla GET su courses/(\{YEAR\}\|current)/\{ID\}\[/it\|/en\]

#### URL: courses/(\{YEAR\}\|current)/\{ID\}/syllabus\[/it\|/en\]

##### VERBO: GET

Restituisce un oggetto con il solo sillabo/programma estratto
dall'oggetto restituito dalla GET su courses/(\{YEAR\}\|current)/\{ID\}\[/it\|/en\]

#### URL: courses/(\{YEAR\}\|current)/\{ID\}/teachers

##### VERBO: GET

Restituisce la lista dei docenti presenti nell'oggetto
restituito dalla GET su courses/(\{YEAR\}\|current)/\{ID\}.

Esempio di output:

`["Pinco Pallino","Paolino Paperino"]`

##### VERBO: PUT

Aggiorna la lista dei docenti per il corso \{ID\} relativa
all'anno accademico \{YEAR\} o a quello corrente (*current*). Il payload è
lo stesso oggetto restituito dalla lettura (GET) tramite la URL courses/(\{YEAR\}\|current)/\{ID\}/teachers.

#### URL: courses/(\{YEAR\}\|current)/\{ID\}/relations

##### VERBO: GET

Restituisce la lista dei corsi in relazione con il
corso \{ID\}, estratta dall'oggetto restituito dalla GET su courses/(\{YEAR\}\|current)/\{ID\}.
L'oggetto restituito dovrà contenere tre liste di URI, rispettivamente per i
corsi propedeutici, quelli mutuati e i moduli:

`{"propedeutics": ["http://courseweb.it/courses/current/1", "http://courseweb.it/courses/current/2"\], "same": ["http://courseweb.it/courses/current/3"], "modules":[] }`

##### VERBO: PUT

Aggiorna la lista dei corsi in relazione con il corso
\{ID\} relativa all'anno accademico \{YEAR\} o a quello corrente (*current*).
Il payload è lo stesso oggetto restituito dalla lettura (GET) tramite la URL courses/(\{YEAR\}\|current)/\{ID\}/relations.

#### URL: courses/(\{YEAR\}\|current)/\{ID\}/links

##### VERBO: GET

Restituisce i link relativi al corso \{ID\}, estratti
dall'oggetto restituito dalla GET su courses/(\{YEAR\}\|current)/\{ID\}. L'oggetto
restituito dovrà contenere tante chiavi quanti sono i tipi di link previsti
dalla specifica:

`{"homepage":"...", "forum":"...", "elearning":"...", resources:...""}`

##### VERBO: PUT

Aggiorna i link relativi al corso \{ID\} per all'anno
accademico \{YEAR\} o per quello corrente (*current*). Il payload è lo
stesso oggetto restituito dalla lettura (GET) tramite la URL courses/(\{YEAR\}\|current)/\{ID\}/links.  

-------
{#client}

Il client dovrà essere strutturato come segue:

1. Un selettore permetterà di scegliere un anno accademico o l'opzione "corrente".
2. Un selettore permetterà di scegliere una lingua tra italiano e inglese.
3. Sarà visualizzata la lista dei corsi relativi all'anno accademico selezionato dal
controllo al punto (1) con nome, codice, settore e docenti. I dettagli saranno
mostrati nella lingua selezionata dal controllo al punto (2). *Per evitare
eccessivo numero di chiamate al servizio, è opportuno paginare questa lista e
caricare le informazioni su un corso (mettendole poi in una cache) solo quando
viene richiesta la corrispondente pagina.*
4. Selezionando un corso dalla lista al punto (3), in un'area specifica della pagina (o in un *dialog
box modale*, ci sono ottimi plugin di JQuery per gestirli) verrà mostrato il
sillabo corrispondente, sempre nella lingua selezionata dal controllo al punto
(2).
