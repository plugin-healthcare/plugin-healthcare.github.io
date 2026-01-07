---
icon: lucide/hard-hat
title: Leeswijzer voor technisch personeel
---

## Organisatie

Ziekenhuizen en andere datahouders moeten steeds meer de data die zijn vastgelegd tijdens het zorgproces, beschikbaar stellen voor hergebruik. Dit zogenaamde secundair gebruik van gezondheidsgegevens is niet nieuw, maar zal naar verwachting met de komst van de European Health Data Space (EHDS) toenemen. Datahouders zullen daarom zaken proces, informatiestandaarden, applicaties en infrastructuur moeten organiseren om secundair gebruik van gezondheidsgegevens op een effectieve en efficiente manier mogelijk te maken.

De organisatorische inrichting begint bij het onderscheiden van verschillende vormen van secundair gebruik, namelijk

- **Gegevensuitwisseling:** een data aanlevering, waarbij persoonsgegevens worden doorgestuurd aan de datagebruiker. Dit kan bijvoorbeeld een kwaliteitsregistratie zijn.
- **Gegevensverzoek:** verschaffen van geanonimiseerde, geaggregeerde gegevens zonder dat de datagebruiker daarbij direct toegang te krijgen tot gezondheidsgegevens op persoonsniveau. Deze vorm van secundair gebruik is beschreven in [artikel 69](https://eur-lex.europa.eu/legal-content/NL/TXT/HTML/?uri=OJ:L_202500327&qid=1764922416982#art_69) van de EHDS.
- **Gegevensvergunning:**  op grond van [artikel 68](https://eur-lex.europa.eu/legal-content/NL/TXT/HTML/?uri=OJ:L_202500327&qid=1764922416982#art_68) van de EHDS kan een datagebruiker, nadat zij een vergunning heeft gekregen, toegang krijgen tot de gezondheidsgegevens om daarmee onderzoek, beleidsanalyses of product ontwikkeling mee te doen. De gebruiker werkt in een beveiligde verwerkingsomgeving.

Alhoewel elk van deze drie vormen van secundair gebruik afzonderlijke juridische kaders en spelregels kent, biedt PLUGIN een generieke basisinfrastructuur die al deze vormen van secundair gebruik ondersteund. Daarmee houden we het voor datahouders beheersbaar en betaalbaar. Op deze pagina wordt uitgelegd hoe dat wordt gedaan.

##  Proces

Het proces voor een datahouders richt zich op de eenmalige inrichting van de basisinfrastructuur, en het beschikbaar stellen van datasets wat een terugkerende activiteit is. Naarmate met de tijd meer data beschikbaar is gesteld, zal deze activiteit afnemen. Onderstaand overzicht verwijst naar de gedetailleerde beschrijving van elke processtap.

!!! abstract "Leeswijzer processtappen"

    === "Eenmalig"

        | Processtap | Waar toegelicht |
        |:-----------|:----------------|
        | Afsluiten overeenkomsten | Handleiding met o.a. standaardovereenkomsten |
        | Inrichting technische infrastructuur (server, netwerkverbindingen, opslag) | Op deze pagina onder [Infrastructuur]() |
        | Inrichting applicaties (vantage6 node) | Op deze pagina onder [Applicatie]() |

    === "Continu / periodiek"

        | Processtap | Waar toegelicht |
        |:-----------|:----------------|
        | Inrichting data aanlevering | Op deze pagina onder [Informatie]() |
        | Data validatie | Op deze pagina onder [Informatie]() |
        | (Optioneel) Inrichting koppeling EPD voor resultaten uit een project | Handleiding project b.v. AI-ondersteund coderen |



## Informatie

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
    In de zorg maken we gebruik van landelijke codestelsels, zoals de DHD diagnose- en verrichtingenthesaurus, en international codestelsels zoals ICD10, SNOMED CT en LOINC.

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


## Applicatie

PLUGIN is modulair opgebouwd en bestaat daarmee uit verschillende componenten die hieronder schematisch zijn weergegeven. Een meer gedetailleerde toelichting staat beschreven in de handleiding en architectuur documentatie.

![](../images/plugin-applicatie-overzicht.drawio.png)

!!! abstract "Leeswijzer applicatie componenten"

    | Component | Omschrijving | Waar toegelicht |
    |:-----------|:------------|:----------------|
    | PLUGIN-Lake | Component waarmee het PLUGIN datastation wordt gevuld | Op deze pagina onder [...]()Handleiding met o.a. standaardovereenkomsten |
    | PLUGIN-Hub | Component waarmee gegevensaanleveringen kunnen worden uitgevoerd | Op deze pagina onder [...]() |
    | PLUGIN-Analytics | Component waarmee gegevensaanleveringen kunnen worden uitgevoerd | Op deze pagina onder [...]() |
    | PLUGIN-ML | Component waarmee federatief machine learning modellen ontwikkeld kunnen worden | Op deze pagina onder [...]() |
    | vantage6 node | Decentrale component van de vantage6 instructuur | Op deze pagina in de [vantage6 documentatie]() |
    | vantage6 server | Centrale processing hub van de vantage6 instructuur | Op deze pagina in de [vantage6 documentatie]() |
    | Docker register | Database van berekeningen die op het datastation mogen worden uitgevoerd | Op deze pagina in de [vantage6 documentatie]() |


## Infrastructuur

De fysieke infrastructuur bestaat uit specificaties voor de Linux server, het IP netwerk en cloud opslag. De details hiervan worden uitgelegd in de [installatiegids]().




