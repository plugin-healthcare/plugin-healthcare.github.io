---
icon: lucide/library-big
title: Informatie
---

Het effectief en efficient hergebruik van gezondheidsgegevens staat of valt met het standaardiseren van de manier hoe datahouders informatie vastleggen. Hiertoe zal op twee niveaus interoperabiliteit moeten worden gerealiseerd.

<div class="grid cards" markdown>

-   :lucide-message-circle-code:{ .lg .middle } __Syntactische interoperabiliteit__

    ---
    
    
    Ten eerste is zogenaamde **syntactische interoperabiliteit** nodig. Dit gaat over de **vorm** en de **structuur** van het bericht, zoals bijvoorbeeld een brief.
    Syntactische interoperabiliteit betekent dat de ontvanger de brief fysiek kan openen, herkent dat het een brief is, en dat zij de letters kan lezen (bijvoorbeeld het Latijnse alfabet).

    !!! quote "Analogie"

        Ik stuur jou een zin die grammaticaal perfect klopt: "De blerf schrobt de grakker." De ontvanger kan de zin lezen (syntax is correct), maar heeft geen idee wat de verzender ermee bedoelt te zeggen.

-   :lucide-book-open-check:{ .lg .middle } __Semantische interoperabiliteit__

    ---

    Dit gaat over de **inhoud** en het **begrip**. Als de brief eenmaal is geopend, willen we begrijpen wat er staat.
    We moeten dezelfde taal spreken en dezelfde definities gebruiken. Als ik "bank" schrijf, moet de ontvanger weten of ik een zitmeubel bedoel of een geldinstelling.
    In de zorg maken we gebruik van landelijke codestelsels, zoals de DHD diagnose- en verrichtingenthesaurus, en internationale codestelsels zoals ICD-10, SNOMED CT en LOINC.

    !!! quote "Analogie"

        Om _"De blerf schrobt de grakker"_ te begrijpen, hebben we een woordenboek nodig dat uitlegt wat een _'blerf'_ is. Semantische interoperabiliteit zorgt ervoor dat de computer niet alleen "180" ziet staan, maar begrijpt dat dit een "bloeddruk" is in "mmHg".

</div>

!!! abstract "Leeswijzer informatie"

    === "Syntactische interoperabiliteit"

        | Onderdeel | Waar toegelicht |
        |:-----------|:----------------|
        | Gebruik van FHIR als logisch model | Op deze pagina onder [...]() |
        | Gebruik van OMOP als logisch model | Op deze pagina onder [...]() |
        | Transformaties tussen logische modellen | Op deze pagina onder [PLUGIN Lake]() |

    === "Semantische interoperabiliteit"

        | Onderdeel | Waar toegelicht |
        |:-----------|:----------------|
        | Gebruik van project-specifieke aanleveringen | Op deze pagina onder [AIOC aanlevering]() |
        | Gebruik van DHD diagnose- en verrichtingenthesaurus | Op deze pagina onder [DHD thesauri]() |
        | Gebruik van (inter)nationale codestelsels | Op deze pagina onder [codestelsels]() |
        | Transformaties tussen codestelsels | Op deze pagina onder [PLUGIN Lake]() |
