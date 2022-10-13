---
titolo: Collectors Site Services
numero: 1
versione: 1.0
lingua: IT
anno: 2020
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: Collectors Site
---

-------
{#operazioni}

- Login/logout con username e password
- Elenco collezioni (personali) dell'utente (qui e successivamente
con "utente" intendiamo quello autenticato)
- Elenco collezioni condivise con un utente
- Elenco dei dischi in una collezione (accessibile dall'utente)
- Dettagli singolo disco contenuto in una collezione
- Inserimento di un nuovo disco in una delle collezioni dell'utente
 Ricerca di un disco (in base a vari criteri) tra le collezioni
personali, tra le collezioni condivise o tra quelle pubbliche (tre *endpoint*
diversi)
- Elenco di tutti gli autori presenti nel sistema
- Elenco di tutti i dischi di un certo autore presenti nelle
collezioni pubbliche
- Aggiornamento di un disco in una collezione personale dell'utente
- Estrazione di alcune statistiche (si veda la specifica del
progetto Collectors Site per alcuni suggerimenti) dalla collezione di un utente


-------
{#client}

In particolare, il client dovrà permettere di navigare tra
le collezioni personali di un utente: dalla lista delle collezioni alla lista
dei dischi nella collezione fino ad arrivare ai dettagli di un singolo disco.