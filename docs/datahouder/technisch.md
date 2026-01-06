---
icon: lucide/hard-hat
title: Technisch personeel
---

# PLUGIN voor voor technisch personeel

## Organisatie

Ziekenhuizen en andere datahouders moeten steeds meer de data die zijn vastgelegd tijdens het zorgproces, beschikbaar stellen voor hergebruik. Dit zogenaamde secundair gebruik van gezondheidsgegevens is niet nieuw, maar zal met de komst van de European Health Data Space (EHDS) naar verwachting toenemen. Datahouders zullen daarom zaken proces, informatiestandaarden, applicaties en infrastructuur moeten organiseren om secundair gebruik van gezondheidsgegevens op een effectieve en efficiente manier mogelijk te maken.

De organisatorische inrichting begint bij het onderscheiden van verschillende vormen van secundair gebruik, namelijk

- **Gegevensuitwisseling:** een data aanlevering, waarbij persoonsgegevens worden doorgestuurd aan de datagebruiker. Dit kan bijvoorbeeld een kwaliteitsregistratie zijn.
- **Gegevensverzoek:** verschaffen van geanonimiseerde, geaggregeerde gegevens zonder dat de datagebruiker daarbij direct toegang te krijgen tot gezondheidsgegevens op persoonsniveau. Deze vorm van secundair gebruik is beschreven in [artikel 69](https://eur-lex.europa.eu/legal-content/NL/TXT/HTML/?uri=OJ:L_202500327&qid=1764922416982#art_69) van de EHDS.
- **Gegevensvergunning:**  op grond van [artikel 68](https://eur-lex.europa.eu/legal-content/NL/TXT/HTML/?uri=OJ:L_202500327&qid=1764922416982#art_68) van de EHDS kan een datagebruiker, nadat zij een vergunning heeft gekregen, toegang krijgen tot de gezondheidsgegevens om daarmee onderzoek, beleidsanalyses of product ontwikkeling mee te doen. De gebruiker werkt in een beveiligde verwerkingsomgeving.

Elk van deze drie vormen van secundair gebruik kent zijn eigen juridische kaders en spelregels. Los van dit organisatorische kader biedt PLUGIN een generieke basisinfrastructuur die al deze vormen van secundair gebruik ondersteund. Daarmee houden we het voor datahouders beheersbaar en betaalbaar. Voor meer details over deze verschillende vormen van secundair gebruik verwijzen we naar de [handleiding](). 

##  Proces

Het proces voor een datahouder om deel te nemen aan PLUGIN is als volgt.

=== "Eenmalig"

    | Processtap | Waar toegelicht |
    |:-----------|:----------------|
    | Afsluiten overeenkomsten | Handleiding met o.a. standaardovereenkomsten |
    | Inrichting technische infrastructuur (server, netwerkverbindingen, opslag) | Op deze pagina onder [Infrastructuur](#infrastructuur) |
    | Inrichting applicaties (vantage6 node) | Op deze pagina onder [Applicatie](#applicatie) |

=== "Continu / periodiek"

    | Processtap | Waar toegelicht |
    |:-----------|:----------------|
    | Inrichting data aanlevering | Op deze pagina onder [Informatie](#informatie) |
    | Data validatie | Op deze pagina onder [Informatie](#informatie) |
    | (Optioneel) Inrichting koppeling EPD voor resultaten uit een project | Handleiding project b.v. AI-ondersteund coderen |



## Informatie

(nu nog per project, om termijn gestandaardiseerd)

### Syntactische inteorperabiliteit: 
Toelichten dat we toewerken naar syntactische interoperabiliteit. Mocht dit voor hen onbekend zijn kunnen we doorverwijzen naar de Health-RI datastation specificatie. Op dit moment kan een ziekenhuis de data-aanleveren via een API of puur een lokaal bestand wat op het datastation wordt gezet. Dit mag een:

1.	FHIR-bestand
2.	OMOP-bestand
3.	OpenEHR-bestand
4.	Random bestand, zolang het een datamodel heeft.

### Semantische interoperabiliteit

Dit is het moeilijkst: we moeten naar eenduidige code stelsels. Bijvoorbeeld voor medicatie: G-standaard, ATC code, RxNorm, IDMP. Dit zal een jarenlange ontwikkeling zijn, om toe te werken naar standaardisatie. PLUGIN ondersteund datahouders om hierin mee te groeien en de interne organisatie te ontlasten. Dit doen we b.v. met het project AI-ondersteund coderen.

## Applicatie

In deze paragraaf lichten we de verschillende applicaties die PLUGIN gebruikt op technisch vlak toe. Waarbij we duidelijk moeten aangeven wat decentraal draait bij het ziekenhuis en wat centraal draait bij DHD:

1.	PLUGIN-Lake
2.	Vantage6-node
3.	PLUGIN-ML
4.	PLUGIN-Analytics
5.	PLUGIN-Hub

Aan het eind van deze paragraaf eindigen we met een link naar de vantage6-community. 

## Infrastructuur

Dit is de meest belangrijke paragraaf voor de technicus. In deze paragraaf leggen we alles op zo’n gedetailleerd niveau uit.
-	Linux server specificaties
-	IP whitelisten
-	Blob storage

Aan het eind van deze pagina eindigen we met een link naar de handleidingen.

