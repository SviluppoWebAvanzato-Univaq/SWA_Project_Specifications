<html>
<head>
<meta charset="UTF-8"/>
<title>${nomecorso}: Final Course Projects</title>
<style>${css?no_esc}</style>
</head>




<body>

# ${nomecorso} Course<br/>*Final Projects A.Y. ${anno}/${(anno?number)+1}* - Specification #${numero!""}

<section class="specifica">

## Project "${titolo}"
> Version ${versione!"1.0"}

### Preamble

The course projects are inspired by real-world needs, and they usually refer to similar sites already on the web. Students must follow the specifications given by this document, but they can also refine them through an interaction with the teacher and the analysis of similar websites. The final website must be completely original, well organized and easily accessible to all the users.  

### Site Specifications

<section class="descrizione">${body_specifiche?no_esc}</div>

The following list contains a brief description of the contents and functionalities required by this site. Obviously, any further refinement or enrichment of these specifications will increase the value of the project.


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