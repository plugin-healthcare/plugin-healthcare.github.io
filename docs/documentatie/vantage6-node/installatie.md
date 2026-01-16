---
icon: lucide/wrench
title: Installatie vantage6 node
---
## Randvoorwaarden

* Docker geïnstalleerd
    - [Ubuntu 22.04](/_static/scripts/install-docker-ubuntu.sh)
    - [debian 12](/_static/scripts/install-docker-debian.sh)
* Huidige gebruiker heeft voldoende rechten Docker te gebruiken.
* Python >= 3.10

!!! tip 
    Voor de installatie van Python packages raden we een virtual environment aan. Zie de documentatie van de gebruikte Python distributie voor meer informatie over hoe dit op te zetten. Het gebruik van [uv](https://docs.astral.sh/uv/) wordt ten zeerste aanbevolen.

    | Distributie | Documentatie |
    |:---|:---|
    | Python3 (vanilla) | [Using Python environments with uv](https://docs.astral.sh/uv/pip/environments/) |
    | Anaconda | [Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) |



## Installatie van vantage6 software
Vantage6 nodes draaien als Docker container op de Linux server van de datahouder. Om interactie met deze containers te vergemakkelijken, is er een command line interface (CLI) beschikbaar. Deze CLI is geschreven in Python3 en is als package beschikbaar via [PyPI](https://pypi.org).


Het volgende commando installeert de laatste versie van CLI:

```bash
pip install --upgrade vantage6
```

!!! tip
    Het volgende commando verifieert of installatie van de software succesvol was:
    ```bash
    vnode --help
    ```


## Configuratie van de node(s)
Binnen een vantage6 netwerk is het mogelijk om verschillende samenwerkingsverbanden te definieren. Per samenwerkingsverband wordt bepaald

1. Welke data wordt gebruikt, en 
2. Wat er precies mee gedaan mag worden.


PLUGIN beoogt een of meerdere landelijke samenwerkingsverbanden (bijv. voor AI-ondersteund-coderen of aanlevering aan de NKR), maar faciliteert ook andere samenwerkingen. Zo is het, bijvoorbeeld, mogelijk om voor de [mProve](https://www.mprove.nu) of de [Santeon](https://santeon.nl) ziekenhuizen onderling afspraken te maken.

Per samenwerkingsverband, wordt een node geconfigureerd: de node heeft toegang tot de (juiste) data Ã©n houdt in de gaten dat alleen de toegestane algoritmes op de data worden toegepast.

Om een configuratiebestand voor een nieuwe node aan te maken wordt vanaf de command line  het commando `vnode new` gebruikt.

```bash
vnode new
```

Dit start een "wizard" die een aantal vragen stelt. Deze worden hieronder nader toegelicht.

!!! note "Configuratie wizard"

    | Prompt | Antwoord / Omschrijving |
    |:---|:---|
    | Please enter a configuration-name | Naam van de node (en het configuratiebestand).<br/><br/>Dit is enkel voor intern gebruik. Vaak wordt hier vaak een combinatie van naam van de organisatie en samenwerking voor gebruikt. Bijvoorbeeld LUMC-PLUGIN voor een node die geïnstalleerd wordt door het LUMC en gebruikt wordt voor PLUGIN.|
    | Please select the environment you want to configure | Kies `application`. Deze vraag verdwijnt in een toekomstige versie van vantage6. |
    | Enter given api-key |De API-key wordt gebruikt voor authenticatie van de node bij de server. Deze ontvangt u van DHD, IKNL, of EZA. |
    | The base-URL of the server | Kies een van<br> Voor IKNL: [https://cotopaxi.vantage6.ai](https://cotopaxi.vantage6.ai)<br>Voor DHD: [https://plugin-a.dhd.nl](https://plugin-a.dhd.nl) |
    | Enter port to which the server listens | Kies/typ `443` (https).|
    | Path of the api | Haal de standaardwaarde `/api` weg.|
    | Task directory path | Accepteer de voorgesteld waarde.|
    | Task directory path | Accepteer de voorgesteld waarde.|
    | Do you want to add a database? | Kies `Yes`.|
    | Enter unique label for the database | Accepteer de voorgestelde waarde `default`.|
    | Database URI | Voer de URI voor de database in. Dit is _of_ een URI in de vorm `dialect+driver://username:password@host:port/database` _of_ een absoluut pad naar een databestand (bijv. `/home/melle/database.csv`).|
    | Database type | Kies het juiste database/bestands type.|
    | Do you want to add a database? | Kies `No`.|
    | Which level of logging would you like? | Kies `DEBUG`.|
    | Do you want to connect to a VPN server? | Kies `Yes`.|
    | Subnet of the VPN server you want to connect to | Kies een van:<br>IKNL: `10.76.0.0/16`<br>DHD: `10.76.0.0/16`|
    | Enable encryption? | Kies `Yes`.|
    | Path to private key file | Deze key maken we in de vervolgstap [Aanmaken private key](#aanmaken-private-key) aan. Dit veld mag voor nu leegblijven.|


Na afronding van de wizard, wordt de locatie van het aangemaakte configuratiebestand op het scherm getoond. Dit bestand kan m.b.v. een tekst-editor (bijv. Notepad of Visual Studio Code) worden geopend en aangepast.

De locatie van dit configuratiebestand kan ook worden teruggevonden met behulp van het volgende commando:

```bash
vnode files
```

## Aanmaken private key
Het handmatig aanmaken van een private key kan via het volgende commando:

```bash
vnode create-private-key
```

Hiervoor zijn de gebruikersnaam en password, verstrekt door DHD, IKNL, of EZA, noodzakelijk. Het programma past automatisch de configuratie aan.


## Starten en stoppen van een node
Om een node te starten, wordt het volgende commando gebruikt:
```bash
vnode start
```

Om een idee te krijgen van wat er op de achtergrond gebeurt, is het mogelijk om de output van de node naar het scherm te sturen. Dit kan op twee manieren:

```bash
vnode start --attach
```

Of op een later moment via het commando:

```bash
vnode attach
```


Voor het stoppen van een node bestaat een vergelijkbaar commando:
```bash
vnode stop
```

## Docker registry
!!! tip
    Indien  de docker registry die binnen de samenwerking niet publiek (leesbaar) is, is het nodig om in te loggen.



## Meer informatie
Zie voor meer informatie de documentatie van [vantage6](https://docs.vantage6.ai/en/main/node/install.html)

!!! warning
    De specificaties in de officiële vantage6 documentatie kunnen afwijken van de specificaties in dit document.
