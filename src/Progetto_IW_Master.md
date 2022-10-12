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

I progetti di fine corso si ispirano sempre ad esigenze
reali, e fanno solitamente riferimento a tipologie di sito già presenti sulla
rete. Nello svolgere il progetto, gli studenti dovranno attenersi alla
specifica data in questo documento, ma potranno raffinarla tramite l'interazione
col docente e l'analisi di siti web analoghi. In ogni caso, la realizzazione
finale dovrà essere completamente originale. Le informazioni pubblicate
dovranno essere sempre ben organizzate ed accessibili, date le varie tipologie
di utenza associate alle applicazioni web pubbliche.  

### Specifiche del Sito

<section class="descrizione">${body_specifiche?no_esc}</section>

Di seguito sono illustrati schematicamente i contenuti e le
funzionalità minime che dovrebbero essere inseriti nel sito. Ovviamente, ogni
ulteriore raffinamento o arricchimento di queste specifiche aumenterà il valore
del progetto.

<#if (body_operazioni??)>

<section class="operazioni">${body_operazioni?no_esc}</section>

</#if>

</section>

<#if !(escludi_indicazioni??)>

<section class="indicazioni break">${indicazioni?no_esc}</section>

</#if>


<#if includi_scheda??>

<section class="scheda break">${scheda?no_esc}</section>

</#if>


</body>
</html>