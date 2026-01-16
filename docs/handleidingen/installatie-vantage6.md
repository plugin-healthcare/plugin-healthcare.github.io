---
icon: lucide/wrench
title: Installatiegids vantage6
---

# Installatie vantage6

## Randvoorwaarden

* Docker Engine geïnstalleerd
    - [Ubuntu 22.04](https://docs.docker.com/engine/install/ubuntu/)
    - [debian 12](https://docs.docker.com/engine/install/debian/)
    - [RHEL 8](https://docs.docker.com/engine/install/rhel/)
    > Om de PLUGIN infrastructuur te gebruiken is het noodzakelijk om Docker Engine en niet Docker Desktop te installeren. Docker desktop maakt gebruik van een virtuele machine die niet compatibel is met de PLUGIN infrastructuur.
* Huidige gebruiker heeft voldoende rechten Docker te gebruiken.
* Python >= 3.10 *vantage6 is gebouwd en getest met python 3.10, nieuwere versies leiden mogelijk tot dependency issues*
    > De installatie van python wordt aangeraden via een python version manager zoals [pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#installation) of een volledige python project manager zoals [uv](https://docs.astral.sh/uv/). Hierdoor wordt migratie en onderhoud van python versies en packages eenvoudiger.
* Indien gebruik gemaakt wordt van een NVIDIA GPU, moet naast de standaard NVIDIA drivers ook de [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) worden geïnstalleerd.

## Installatie van vantage6 software

De installatie van een vantage6 node op een server loopt via de vantage6 command line interface (CLI). Via deze CLI kan men een nieuwe node configureren, starten, stoppen en monitoren ([zie vantage6 node documentatie voor CLI overzicht](https://docs.vantage6.ai/en/main/node/use.html)).

Na het opzetten en activeren van de gewenste python omgeving, kan de vantage6 CLI worden geïnstalleerd:

``` bash
pip install vantage6
```

Test of de installatie gelukt is, kan met het commando:

``` bash
vnode --help
```

Om een bestaande versie van vantage6 te updaten, kan het volgende commando worden gebruikt:

``` bash
pip install --upgrade vantage6
```

## Vantage6 node configuratie

Voor het configureren van een vantage6 node is het volgende nodig vanuit de vantage6 server/netwerk beheerder (bijvoorbeeld DHD of IKNL):

* Een *organization* voor de node
* Inloggegevens voor een *user* binnen deze organisatie met *organization administrator* rechten
* Een *collaboration* waarin de node deelneemt
* Een API key per *node* per *collaboration*

Al deze gegevens worden verstrekt door de beheerder van het betreffende vantage6 netwerk.

Met deze gegevens kan de node worden geconfigureerd. De node configuratie bepaalt samen met de collaboration configuratie welke algoritmes door welke organisaties mogen worden uitgevoerd. De node configuratie bepaalt daarnaast ook de toegang tot data en hardware binnen de organisatie.

Een node wordt standaard via een host gebruiker geinstalleerd. Dit betekent dat alle vantage6 (meta)data zoals configuratie bestanden en logs worden opgeslagen in de home directory van de gebruiker waarop de node is geconfigureerd. #TODO INSERT LINK NAAR UITLEG SHARED USERS.

De node configuratie kan op twee manieren worden uitgevoerd: via een interactieve wizard of handmatig door het toevoegen van een YAML configuratiebestand. Voor de volledige configuratie opties zie de [vantage6 node config documentatie](https://docs.vantage6.ai/en/main/node/configure.html#all-configuration-options).

## Configuratie via wizard

1. Activeer de python omgeving waarin vantage6 is geïnstalleerd.
2. Voer het volgende commando uit om de configuratie wizard te starten:

    ``` bash
    v6 node new
    ```

3. Beantwoord de vragen in de wizard (zie toelichting hieronder)

    | # | Question | Beschrijving | Default | Logica |
    |---|----------|-------------|---------|--------|
    | 1 | Enter given api-key: | API key verstrekt door je server administrator | - | - |
    | 2 | The base-URL of the server: | Basis URL van de vantage6 server | `http://localhost` | - |
    | 3 | Enter port to which the server listens: | Poortnummer waarop de server luistert | `443` (HTTPS) of `5000` (HTTP) | - |
    | 4 | Path of the api: | Pad naar het API endpoint | `/api` | - |
    | 5 | Task directory path: | Pad waar task data wordt opgeslagen | Systeem data directory | - |
    | 6 | Do you want to add a database? | Database configuratie toevoegen | - | Bij Nee → #10 |
    | 7 | Enter unique label for the database: | Unieke identifier voor deze database | `default` | - |
    | 8 | Database URI: | Verbindingsstring naar de database | - | - |
    | 9 | Database type: | Type database | Selecteer uit lijst | Herhaal #6-9 bij meer databases |
    | 10 | Do you want to connect to a VPN server? | VPN server verbinding inschakelen | No | Bij Nee → #12 |
    | 11 | Subnet of the VPN server you want to connect to: | Netwerk subnet voor VPN | `10.76.0.0/16` | - |
    | 12 | Do you want to limit the algorithms allowed to run on your node? | Beperk welke algoritmes kunnen worden uitgevoerd (aanbevolen voor productie) | Yes | Bij Nee → #21 |
    | 13 | Do you want to enter a list of allowed algorithms? | Whitelist specifieke algoritmes met namen of regex | - | Bij Nee → #16 |
    | 14 | Enter your algorithm expression: | Algoritme naam of regex patroon (bijv. `^harbor2\.vantage6\.ai/demo/average$`) | - | - |
    | 15 | Do you want to add another algorithm expression? | Nog meer algoritme expressies toevoegen | Yes | Bij Nee → #16, anders herhaal #14-15 |
    | 16 | Do you want to allow algorithms from specific algorithm stores? | Whitelist hele algoritme stores | - | Bij Nee → #21 |
    | 17 | Enter the URL of the algorithm store: | Store URL of regex patroon (bijv. `https://store.cotopaxi.vantage6.ai`) | - | - |
    | 18 | Do you want to add another algorithm store? | Nog meer algoritme stores toevoegen | Yes | Bij Nee → #19, anders herhaal #17-18 |
    | 19 | Do you want to allow only algorithms that are both in the list of allowed algorithms AND are part of one of the allowed algorithm stores? | Algoritmes moeten aan beide whitelists voldoen (AND) vs één van beide (OR) | Yes (AND) | Alleen gevraagd als zowel #13 als #16 Ja waren |
    | 20 | *Informatie bericht over allowing either whitelist or store* | Legt de logica uit bij Nee kiezen in #19 | - | - |
    | 21 | Which level of logging would you like? | Verbosity van logging output | Selecteer uit lijst | - |
    | 22 | Encryption is enabled/disabled for this collaboration. Accept? | Bevestig encryptie instelling gedetecteerd van server | Yes | Als server bereikbaar, ga naar #24; anders verder naar #23 |
    | 23 | Enable encryption? | Encryptie in-/uitschakelen (als server niet bereikbaar) | Yes | Bij Nee → Einde, anders #24 |
    | 24 | Path to private key file: | Locatie van private key bestand* (als encryptie is ingeschakeld) | - | Alleen gevraagd als encryptie is ingeschakeld |

!!! info "*Private key aanmaken"
    Elke organisatie heeft een eigen private key (.pem bestand). Indien deze nog niet is aangemaakt, kan dit volgens de instructies in de volgende sectie "Aanmaken private key". Laat in dat geval het pad naar de private key leeg. Indien er al eerder een key is aangemaakt voor deze organisatie, vul dan hier het pad naar dat bestand in.

!!! warning "connection errors"
    De wizard probeert na stap 4 te authenticeren met de server om automatisch collaboratie instellingen te detecteren. Als authenticatie mislukt, wordt je gevraagd door te gaan met handmatige configuratie of het setup proces af te breken.

## Configuratie via configuratie YAML bestand

Vantage6 nodes kunnen ook handmatig worden geconfigureerd door een YAML configuratiebestand aan te maken. Dit bestand moet worden opgeslagen in de config directory van vantage6. Standaard is dit op een linux systeem `.config/vantage6/node/node-config.yaml`. De volledige lijst met configuratie opties is te vinden in de [vantage6 node config documentatie](https://docs.vantage6.ai/en/main/node/configure.html#all-configuration-options)

!!! Info "locatie configuratie bestanden"
    De locatie van de configuratie bestanden kan worden opgevraagd met het commando:
    ```bash
    v6 node files
    ```

### Aanmaken private key

Vanuit de collaboratie kan end-to-end encryptie worden afgedwongen. Elke organisatie binnen de collaboratie heeft een eigen key pair (private en public key) voor het versleutelen en ontsleutelen van data. Aangezien de private key gekoppeld is aan een organisatie zal deze key een keer aangemaakt worden en hergebruikt worden voor alle nodes binnen deze organisatie. Lees meer over end-to-end encryption in de [vantage6 encryptie documentatie](https://docs.vantage6.ai/en/main/features/inter-component/encryption.html).

Het aanmaken van een key pair kan met de vantage6 CLI via het volgende commando:

``` bash
v6 node create-private-key
```

Beantwoord de vragen in de wizard:

| # | Question | Beschrijving | Default |
|---|----------|-------------|---------|
| 1 | Select the configuration you want to use: | Kies de node configuratie | Lijst van beschikbare configuraties | 
| 2 | Enter your username: | Gebruikersnaam voor server authenticatie | - |
| 3 | Enter your password: | Wachtwoord voor server authenticatie | - |
| 4 | Enter your MFA code: | Multi-factor authenticatie code | alleen invullen indien mfa is ingesteld |

Na het succesvol doorlopen wordt de public key opgeslagen op de centrale vantage6 server en de private key als `.pem` bestand lokaal opgeslagen. Bewaar dit bestand op een veilige plek, deze is nodig voor het configureren van nieuwe nodes binnen de organisatie.

!!! info "Organization key vervangen"
    Indien de private key verloren is of vervangen moet worden, kan dit via het bovenstaande commando opnieuw worden uitgevoerd. De nieuwe public key zal de oude overschrijven op de server. De nieuwe private key zal dan lokaal op alle servers moeten worden opgeslagen waar nodes geconfigureerd worden.

## Node starten, stoppen en status controleren

De node kan na configuratie gestart worden met het commando:

``` bash
v6 node start --name <node-naam> --attach
```

De node kan gestopt worden met het commando:

``` bash
v6 node stop --name <node-naam>
```

De status van de node kan gecontroleerd worden met het commando:

``` bash
v6 node attach --name <node-naam>
```

Een lijst van de nodes en hun status kan opgevraagd worden met het commando:

``` bash
v6 node list
```

Voor de volledige lijst van `v6 node` commando's en opties, zie de [vantage6 node documentatie](https://docs.vantage6.ai/en/main/node/use.html).

## Overige setup en configuratie aanbevelingen

### Vantage6 node toegang voor meerdere gebruikers

Indien meerdere gebruikers binnen een organisatie een vantage6 node moeten kunnen beheren, is het aan te raden om een gedeelde systeem gebruiker aan te maken waarop de node geconfigureerd en gestart wordt. Deze zogenaamde vantage6 gebruiker kan dan door meerdere systeem gebruikers worden gebruikt om de node te beheren. Om dit in te stellen, volg de volgende stappen:

1. Maak een nieuwe gebruiker aan, bijvoorbeeld `vantage6`:

    ``` bash
    sudo adduser -m vantage6
    ```
    !!! tip Extra beveiliging via blokkeren directe login"
        Voor extra beveiliging kun je de vantage6 gebruiker aanmaken met `--disabled-login`. Dan is alleen toegang via sudo mogelijk, niet via directe login met wachtwoord.

2. Maak eventueel een wachtwoord aan voor de nieuwe gebruiker:
    ``` bash
    sudo passwd vantage6
    ```
    
3. Volg de standaard installatie en configuratie stappen voor het installeren van de vantage6 node.

Indien gewenst kunnen andere gebruikers ofwel via het wachtwoord ofwel via `sudo` toegang krijgen tot de vantage6 gebruiker om de node te beheren.

### Automatisch starten van de vantage6 node bij systeem opstart

De node wordt bij herstart van de server niet automatisch gestart. Om dit te automatiseren kan er een opstart script worden aangemaakt via systemd:

1. Maak een bash script met activatie van de python omgeving en het starten van de node, bijvoorbeeld `/home/vantage6/start-vantage6-node.sh`:

    ``` bash
    #!/bin/bash
    source /path/to/your/python/environment/bin/activate
    v6 node start --name <node-naam>
    ```
2. Maak het script uitvoerbaar:

    ``` bash
    chmod +x /home/vantage6/start-vantage6-node.sh
    ```
3. Maak een systemd service bestand aan, bijvoorbeeld `/etc/systemd/system/vantage6-node.service`:

    ``` ini
    [Unit]
    Description=Startup script for vantage6 node/server named <NAME>
    After=docker.service

    [Service]
    Type=oneshot
    User=<USER>
    WorkingDirectory=/home/<USER>/
    ExecStart=/path/to/start-vantage6-node.sh

    [Install]
    WantedBy=multi-user.target
    ```
    Vervang `<NAME>` door de naam van de node, `<USER>` door de systeem gebruiker waarop de node draait (bijvoorbeeld `vantage6`), en pas het pad naar het start script aan.
4. Herlaad systemd om de nieuwe service te registreren:

    ``` bash
    sudo systemctl daemon-reload
    ```
5. Schakel de service in om automatisch te starten bij systeem opstart:

    ``` bash
    sudo systemctl enable vantage6-node.service
    ```
6. Test de service door deze handmatig te starten:

    ``` bash
    sudo systemctl start vantage6-node.service
    ```
7. Controleer de status van de service:

    ``` bash
    sudo systemctl status vantage6-node.service
    ```
!!! Warning "Service error indien bij actieve node"
    Indien de node al actied bij het starten van de service, zal er een foutmelding verschijnen vanuit vantage6 en de service als mislukt worden gemarkeerd. Dit kan genegeerd worden aangezien de node al draait.


