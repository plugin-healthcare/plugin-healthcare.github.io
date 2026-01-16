---
icon: lucide/book-open-check
title: Benodigdheden
---

Om deel te nemen aan het project, moet aan een aantal voorwaarden voldaan worden. Deze bevinden zich op drie vlakken:

1. Juridisch
1. Hard- en software
1. Data

Deze punten worden hieronder nader toegelicht.


## Juridische zaken
Er moeten afspraken gemaakt worden over:

* Verwerking van data door DHD/IKNL/EZA
* Aan welke use-cases wel/niet wordt meegewerkt
* Wat wel/niet mag op de infrastructuur

Er moeten afspraken gemaakt worden tussen:

* DHD en het ziekenhuis
* IKNL en het ziekenhuis
* EZA en het ziekenhuis
* In sommige use-cases: deelnemende ziekenhuizen onderling

Een functionaris gegevensbescherming (FG), information security officer (ISO) of jurist zal de overeenkomsten moeten beoordelen. Nadat de FG, ISO of jurist van uw ziekenhuis zijn of haar goedkeuring heeft verleend zullen de dienstverlenings- en verwerkersovereenkomsten door de Raad van Bestuur moeten worden ondertekend.


!!! note
    In het kader van het project AI-ondersteund-coderen, heeft DHD met de meeste ziekenhuizen al afspraken gemaakt over het het gebruik van een federatieve infrastructuur voor het trainen van het model.

    IKNL beheert de Nederlandse Kankerregistratie (NKR). In dit kader hebben data managers van IKNL reeds toegang tot de EPDs in de (meeste) Nederlandse ziekenhuizen en zijn er (verwerkers)overeenkomsten afgesloten waarin is vastgelegd hoe/welke informatie geregistreerd kan worden in de NKR. Er zal onderzocht moeten worden in hoeverre aanvullende afspraken nodig zijn.



## Installatie hard- en software
Voor de vantage6 [node](#vantage6-node-target) software, is een server nodig die (bij voorkeur) continue aanstaat. Dit mag ook een virtual machine zijn. De belangrijkste voorwaarden zijn dat de machine:

* in staat is om [Docker](https://www.docker.com) te draaien.
* toegang heeft tot de data (zie ook [hieronder](#technisch-vlak-target))

### Server/virtual machine
De specificaties voor de server (virtual machine) zijn (bij voorkeur) als volgt:

* \>= 16 cores, x86/x64 CPU
* \>= 56 GB CPU RAM
* \>= 360 GB SSD
* virtualization enabled
* GPU (optioneel, maar aanbevolen):
    - [CUDA compatible](https://developer.nvidia.com/cuda-gpus) NVIDIA kaart
    - 16 GB GPU RAM

Indien voor (iets) lagere specificaties van CPU of RAM gekozen wordt, zal het systeem nog steeds werken, maar duren berekeningen mogelijk wat langer. Qua SSD-opslag wordt minder capaciteit afgeraden.

Of een **GPU** nodig is, is afhankelijk de use cases waaraan deelgenomen wordt. Voor AI-ondersteund-coderen geldt dat **trainings**ziekenhuizen deze _wÃ©l_ nodig hebben, en ziekenhuizen die het model alleen gebruiken, niet.


### Network
* \>= 100Mbit ethernet
* Port 443/TCP (https) open voor _uitgaand_ verkeer naar ...
    * DHD
        * vantage6 server: [https://plugin.dhd.nl](https://plugin.dhd.nl)
        * Docker registry: [https://plugindhd.blob.core.windows.net](https://plugindhd.blob.core.windows.net)
        * Docker registry (voor updates van vantage6): [https://harbor2.vantage6.ai](https://harbor2.vantage6.ai)
        * blob storage: [https://plugindhd.azurecr.io](https://plugindhd.azurecr.io)
    * IKNL
        * vantage6 server: [https://cotopaxi.vantage6.ai](https://cotopaxi.vantage6.ai)
        * Docker registry: [https://harbor2.vantage6.ai](https://harbor2.vantage6.ai)
    * [websites nodig voor installatie/upgrade Python3, Docker, etc.]

### Software
* Operating system: linux of Windows
* [Python](https://python.org) >= 3.10
* [Docker](https://www.docker.com)

!!! warning
    Voor het draaien van Docker is onder Windows [WSL2](https://learn.microsoft.com/en-us/windows/wsl/) en _dus_ nested virtualization nodig.

    Het is echter bekend dat (momenteel) de Azure VMs die nested virtualization gÃ©Ã©n GPU hebben. Dit betekent dat ziekenhuizen een VM mÃ©t GPU nodig hebben, het beste voor een Linux host OS kunnen kiezen.


Installatie van de vantage6 node software staat beschreven op [de volgende pagina](./v6-node.md).

## Beschikbaar stellen van data
Om federatieve toepassingen mogelijk te maken, is het belangrijk dat iedere deelnemer zijn/haar klinische data op dezelfde manier aan het platform aanbiedt. Hiervoor zijn op twee vlakken keuzes noodzakelijk:

1. Op **inhoudelijk vlak**: hoe medische gegevens gestructureerd, gecodeerd, en geïnterpreteerd dienen te worden.
2. Op **technisch vlak**: de wijze van aanlevering en/of het verschaffen van toegang. Bijvoorbeeld, of de data wordt ontsloten via een relationele database, FHIR-server, of via blob storage.

### Het inhoudelijk vlak
Op inhoudelijk vlak is gekozen om gebruik te maken van HL7 [FHIR](https://hl7.org/fhir). FHIR beschrijft een datamodel dat is opgedeeld in modulaire componenten genaamd.

Op basis van eerdere ervaringen, is de verwachting dat begonnen zal worden met de volgende resources:


!!! note "Verwachting initieel benodigde FHIR Resources"

    | Resource | Omschrijving |
    |:---|:---|
    | [Patient](https://hl7.org/fhir/Patient.html) | Patient |
    | [Encounter](https://hl7.org/fhir/Encounter.html) | Bezoeken aan het ziekenhuis: (poli)klinish, dagopnames, etc.|
    | [Account](https://hl7.org/fhir/Account.html) | DBCs |
    | [Condition](https://hl7.org/fhir/Condition.html) | Diagnoses (gecodeerd middels ICD-10) |
    | [Observation](https://hl7.org/fhir/Observation.html) | Labuitslagen (gecodeerd middels LOINC?) |
    | [Questionnaire](https://hl7.org/fhir/Questionnaire.html) | Vragenlijsten |
    | [QuestionnaireResponse](https://hl7.org/fhir/QuestionnaireResponse.html) | Antwoorden op vragenlijsten. Bevatten mogelijk vrije tekst.|
    | [Bundle](https://hl7.org/fhir/Bundle.html) | Klinische brieven (zie [FHIR Documents](http://hl7.org/fhir/documents.html)) |

Aangezien deze standaard relatief flexibel is opgezet, zullen op een aantal punten nog keuzes gemaakt moeten worden. Bijvoorbeeld welke attributen verplicht zijn, welke terminologiestelsels gebruikt zullen worden, of hoe omgegaan wordt met overplaatsingen (tussen (verpleeg)afdelingen binnen het ziekenhuis) tijdens een bezoek. Een eerste aanzet hiervoor staat beschreven in [](informatiestandaard).

!!! info
    Tijdens het project zal de informatiestandaard, samen met de deelnemende ziekenhuizen, verder worden uitgewerkt.



### Het technisch vlak
Op technisch vlak zijn er verschillende manieren om de data te ontsluiten in beeld. Dit zijn:

1. Via een FHIR-server en de bijbehorende [REST API](https://hl7.org/fhir/http.html), zoals [HAPI](https://hapifhir.io/hapi-fhir/) of [Firely](https://fire.ly/products/firely-server/) Server. Voor efficiente data-overdracht, zou gebruik gemaakt kunnen worden van de [bulk data API](https://hl7.org/fhir/uv/bulkdata/) en [nd-json](https://hl7.org/fhir/nd-json.html).
2. Via een relationele database, zoals [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2022) of [PostgreSQL](https://www.postgresql.org), waarbij de FHIR resources Ã³f tabulair worden opgeslagen (met ieder attribuut van een resource in een aparte kolom) Ã³f als document (in een kolom van type JSON-B).
3. Via een data lake / blob storage, waarbij de resources als [nd-json](https://hl7.org/fhir/nd-json.html) bestanden worden opgeslagen en worden ontsloten via een (in process) database management systeem (zoals [DuckDB](https://duckdb.org)).

Ongetwijfeld zijn er nog andere opties mogelijk.

De eerste optie heeft als voordeel dat andere applicaties binnen het ziekenhuis gebruik kunnen maken van dezelfde server. Bijvoorbeeld voor Clinical Decision Support. De tweede optie sluit goed aan bij de technology stack die al in veel ziekenhuizen beschikbaar is, Ã©n wordt reeds toegepast door het LUMC, ErasmusMC en UMCUtrecht. De derde optie is waarschijnlijk (financieel) het voordeligst, omdat alleen data-opslag nodig is; een database of app server is niet noodzakelijk.

!!! info
    Tijdens het project zal, samen met de deelnemende ziekenhuizen, worden onderzocht welke vorm van gegevensopslag/-overdracht het meest geschikt is.
