<html><head>
<meta charset="UTF-8"/>
<title>${nomecorso}: Progetti di Fine Corso</title>
<style>${css?no_esc}</style>
</head>

<body>

# Corso di ${nomecorso}<br/>*Progetti A.A. ${anno}/${(anno?number)+1}* - Specifica ${numero!""}


<section class="specifica">

## Progetto "${titolo}"
> Versione ${versione!"1.0"}

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

<#if (we_project_name??)>
A partire dalla base di dati del progetto *${we_project_name}*
(che non dovete necessariamente realizzare ex novo se non siete studenti del
corso di Web Engineering: siete liberi di usare il codice e il database
realizzato da qualche vostro collega), si 
<#else>
Si 
</#if>
costruisca il servizio RESTful
specificato di seguito. La logica del servizio potrà, opzionalmente, utilizzare
una libreria ORM per accedere ai dati.


<section class="operazioni">${body_operazioni?no_esc}</section>

Prestate attenzione alla *struttura delle URL*, che
deve seguire per quanto possibile il paradigma collection-item, evidenziando le
relazioni tra gli oggetti tramite nidificazione.

Se alcune operazioni necessitano di accesso con
autenticazione, potrete usare uno qualsiasi degli approcci visti a lezione per
trasmettere il *token di sessione/autenticazione* , precedentemente
ottenuto tramite la procedura (API) di login: ad esempio inserire un prefisso *auth/{SID}*
sulla URL o passare tale token tramite *cookie* o con l'header *Authentication*.  

### Il Client

Si crei un client HTML5/Javascript con il supporto di JQuery
per il servizio appena descritto, sotto forma di una semplice *single page
application* che utilizza AJAX per interagire col servizio mostrandone i
dati opportunamente sulla pagina.

<#if (body_client??)>

<section class="client">${body_client?no_esc}</section>

Questa è una specifica di base. Potete aggiungere altre
funzionalità a vostra scelta che sfruttino le altre API del servizio, se
volete, aumentando così il valore del progetto.

</#if>

Nota: normalmente, una SPA di una certa complessità andrebbe
realizzata sfruttando framework di più alto livello come Angular o Vue.
Tuttavia, per applicazioni semplici, l'uso di JQuery a basso livello risulta
essere più pratico, in quanto l'uso di framework come AngularJS aggiunge un
notevole overhead al progetto.  

<#if !(escludi_indicazioni??)>

<section class="indicazioni break">${indicazioni?no_esc}</section>

</#if>


<#if includi_scheda??>

<section class="scheda break">${scheda?no_esc}</section>

</#if>


</body>
</html>