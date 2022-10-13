---
titolo: Collectors Site Services
numero: 2
versione: 2.0
lingua: IT
anno: 2021
materia: SWA
mainfile: Progetto_Master.rcp
thisfile: specifica
we_project_name: Collectors Site
---

-------
{#operazioni}

1. Login/logout con username e password
2. Elenco collezioni (personali) dell'utente (qui e successivamente con
"utente" intendiamo quello autenticato)
3. Elenco collezioni condivise con un utente
4. Elenco dei dischi in una collezione (accessibile dall'utente)
5. Dettagli singolo disco contenuto in una collezione
6. Inserimento di un nuovo disco in una delle collezioni dell'utente
7. Ricerca di un disco (in base a vari criteri) tra le collezioni
personali, tra le collezioni condivise o tra quelle pubbliche (tre *endpoint*
diversi)
8. Elenco di tutti gli autori presenti nel sistema
9. Elenco di tutti i dischi di un certo autore presenti nelle collezioni
pubbliche
10. Aggiornamento di un disco in una collezione personale dell'utente
11. Estrazione di alcune statistiche (si veda la specifica del progetto
Collectors Site per alcuni suggerimenti) dalla collezione di un utente


-------
{#client}

In particolare, il client dovrà permettere di navigare tra
le collezioni personali di un utente: dalla lista delle collezioni alla lista
dei dischi nella collezione fino ad arrivare ai dettagli di un singolo disco.
