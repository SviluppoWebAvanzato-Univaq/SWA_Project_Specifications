---
titolo: Web Forms Services
numero: 1
versione: 1.0
lingua: IT
anno: 2021
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: Web of Forms
---

-------
{#operazioni}

1. Login/logout con username e password per gli utenti.
2. Upload template da parte di un utente, con definizione dei relativi
campi (con una singola operazione: il template, sotto forma di stringa, e la
definizione dei campi dovranno essere opportunamente strutturati nel JSON).
3. Approvazione di un template da parte di un amministratore.
4. Elenco dei template disponibili nel sistema (nome e descrizione).
5. Dettaglio di un template (nome, descrizione e lista dei campi con tutti
i loro dettagli).
6. Eliminazione di un template (funzione disponibile solo all'autore del
modulo e agli amministratori).
7. Aggiornamento di un template (funzione disponibile solo all'autore del
modulo e agli amministratori).
8. Compilazione di un template (fornendo i valori dei campi come *payload*
della richiesta) in modalità (cioè con valore di ritorno) HTML e PDF (due *endpoint*
diversi).
9. Invio feedback su un modello (da parte di un utente).
10. Lettura
feedback relativi a un modello (funzione disponibile solo all'autore del modulo
e agli amministratori).


-------
{#client}

In particolare, il client dovrà permettere di navigare tra i
moduli, vederne i dettagli, inserire i valori dei campi e scaricare quantomeno
la versione HTML compilata.
