---
title: Wachtwoordbeheer voor Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 86b8b9bdf5c6441d0708cd874742fa48b65177fa
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289360"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Wachtwoordbeheer voor Microsoft Identity Manager 2016

Wachtwoorden beheren voor meerdere gebruikersaccounts is een van de uitdagingen van het beheren van een bedrijfsomgeving met meerdere gegevensbronnen. Microsoft Identity Manager 2016 (MIM) biedt twee oplossingen voor wachtwoordbeheer:

-   Wachtwoordsynchronisatie: hierbij wordt gebruikgemaakt van de meldingsservice voor wachtwoordwijzigingen (PCNS) voor het vastleggen van wachtwoordwijzigingen vanuit Active Directory en het doorgeven ervan aan andere verbonden gegevensbronnen.

-   Op gebruikers gebaseerd beheer voor het wijzigen van wachtwoorden: hierbij wordt gebruikgemaakt van Windows Management Instrumentation (WMI) via een helpdesk op het web en selfservicetoepassingen voor wachtwoordherstel.

Wachtwoordsynchronisatie en op gebruikers gebaseerd beheer voor het wijzigen van wachtwoorden bieden de volgende voordelen:

-   Het verminderen van het aantal verschillende wachtwoorden dat gebruikers moeten onthouden.

-   Gelijktijdig instellen of wijzigen van wachtwoorden voor meerdere accounts van een gebruiker naar hetzelfde wachtwoord.

-   Gebruikers toestaan hun eigen wachtwoorden in Active Directory te wijzigen en de wijziging naar andere systemen te pushen.

-   Elimineren van het risico van het moeten bouwen van een opslag voor aanvullende wachtwoorden of referenties.

-   Wachtwoorden synchroniseren voor meerdere gegevensbronnen met behulp van Active Directory als gezaghebbende bron.

-   Wachtwoordbeheer uitvoeren in real-time, onafhankelijk van MIM-bewerkingen.

## <a name="password-extensions"></a>Wachtwoordextensies

Beheeragents voor adreslijstservers ondersteunen standaard wijzigingen en instelbewerkingen voor wachtwoorden. Voor op bestanden gebaseerde beheeragents, databasebeheeragents en beheeragents voor uitbreidbare connectiviteit die standaard geen wijzigingen en instelbewerkingen voor wachtwoorden ondersteunen, kunt u een DLL-bestand (Dynamic-Link Library ) voor .NET-wachtwoordextensie maken.
De .NET-DLL voor wachtwoordextensie wordt aangeroepen als er een wachtwoord moet worden gewijzigd of ingesteld voor een van deze beheeragents. De instellingen voor wachtwoordextensie worden in Synchronization Service Manager voor deze beheeragents geconfigureerd. Zie FIM Developer Reference (referentiemateriaal voor FIM-ontwikkelaars) voor meer informatie over het configureren van wachtwoordextensies.

| Wachtwoordbeheer wordt standaard ondersteund in beheeragents voor: | Door een wachtwoordextensie te gebruiken, wordt wachtwoordbeheer ook ondersteund in de beheeragents voor: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Kenmerk-waardepaar tekstbestanden                                                                    |
| Active Directory Lightweight Directory Services (ADLDS)                   | Tekstbestanden met scheidingstekens                                                                               |
| IBM Directory Server                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Tekstbestanden met vast breedte                                                                             |
| Sun en Netscape-adreslijstservers                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | LDAP Data Interchange Format (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle Database                                                                                    |

## <a name="password-synchronization"></a>Wachtwoordsynchronisatie


Wachtwoordsynchronisatie werkt met de PCNS voor een Active Directory-domein. Hierbij worden wachtwoordwijzigingen die afkomstig zijn van Active Directory automatisch doorgegeven naar andere verbonden gegevensbronnen. Hiervoor wordt MIM uitgevoerd als een RPC-server (server voor externe procedureaanroepen) die meldingen voor wachtwoordwijzigingen van een Active Directorydomeincontroller detecteert. Als de aanvraag voor een wachtwoordwijziging is ontvangen en geverifieerd, wordt deze door MIM verwerkt en doorgegeven aan de desbetreffende beheeragents.

> [!IMPORTANT]
> Bidirectionele wachtwoordsynchronisatie wordt in MIM niet ondersteund. Het configureren van bidirectionele wachtwoordsynchronisatie kan een loop veroorzaken. Het gevolg hiervan is extra verbruik van serverresources en het kan tevens negatieve gevolgen hebben voor zowel Active Directory als MIM.

De PCNS wordt uitgevoerd op elke Active Directory domeincontroller. De systemen die de wachtwoordmeldingen ontvangen, staan bekend als de doelen. De MIM-server moet worden geconfigureerd als een PCNS-doel in Active Directory voordat er wachtwoordmeldingen worden verzonden. In de PCNS-configuratie moet een opnamegroep en eventueel een uitsluitingsgroep worden gedefinieerd. Deze groepen worden gebruikt om de stroom gevoelige wachtwoorden uit het domein te beperken. Als u bijvoorbeeld wachtwoorden voor alle gebruikers wilt verzenden, maar geen beheerwachtwoorden, kunt u Domeingebruikers als opnamegroep kiezen en Domeinbeheerders als uitsluitingsgroep. Zie [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx) Wachtwoordsynchronisatie gebruiken) voor meer informatie over het configureren van de meldingsservice voor wachtwoordwijzigingen

Bij de wachtwoordsynchronisatie zijn de volgende onderdelen betrokken:

-   **Meldingsservice voor wachtwoordwijzigingen (Pcnssvc.exe)**: de meldingsservice voor wachtwoordwijzigingen wordt uitgevoerd op een domeincontroller en is verantwoordelijk voor het ontvangen van meldingen voor wachtwoordwijzigingen van het lokale wachtwoordfilter, het in de wachtrij plaatsen van de meldingen voor de doelserver waarop MIM wordt uitgevoerd, en het gebruik van RPC voor het bezorgen van de meldingen. De service versleutelt het wachtwoord en zorgt ervoor dat het wachtwoord beveiligd blijft totdat het is bezorgd bij de doelserver waarop MIM wordt uitgevoerd.

-   **SPN (Service Principal Name)**: de SPN is een eigenschap van het accountobject in Active Directory die door het Kerberos-protocol wordt gebruikt om de PCNS en het doel wederzijds te verifiëren. De SPN zorgt ervoor dat de PCNS wordt geverifieerd voor de juiste server waarop MIM wordt uitgevoerd en dat de meldingen voor wachtwoordwijzigingen niet door een andere service worden ontvangen. De SPN wordt gemaakt en toegewezen met behulp van het hulpprogramma setspn.exe. Zie Using Password Synchronization (Wachtwoordsynchronisatie gebruiken) voor meer informatie over het configureren van de SPN.

-   **Filter voor meldingen van wachtwoordwijzigingen (Pcnsflt.dll)**: het wachtwoordfilter wordt gebruikt om wachtwoorden in platte tekst vanuit Active Directory te verkrijgen. Het filter wordt door de LSA (Local Security Authority) geladen op elke Windows Server-domeincontroller die deelneemt aan het distribueren van wachtwoorden naar een doelserver waarop MIM wordt uitgevoerd. Als het filter is geïnstalleerd en de domeincontroller opnieuw is gestart, kan het filter meldingen voor wachtwoordwijzigingen ontvangen die afkomstig zijn van die domeincontroller. Het filter voor wachtwoordwijzigingen wordt gelijktijdig met andere filters op de domeincontrollers uitgevoerd.

-   **Hulpprogramma voor de configuratie van de meldingsservice voor wachtwoordwijzigingen (Pcnscfg.exe)**: het hulpprogramma pcnscfg.exe wordt gebruikt voor het beheren en onderhouden van de configuratieparameters van de meldingsservice voor wachtwoordwijzigingen die in Active Directory zijn opgeslagen. Deze configuratieparameters (bijvoorbeeld voor het definiëren van de doelservers, het interval voor nieuwe pogingen voor de wachtrij met wachtwoorden, en het in- of uitschakelen van een doelserver), worden gebruikt tijdens het verifiëren en verzenden van wachtwoordmeldingen naar de doelserver waarop MIM wordt uitgevoerd.
    De serviceconfiguratie wordt opgeslagen in Active Directory, zodat slechts de configuratie op één domeincontroller hoeft te worden bijgewerkt. In Active Directory wordt de wijziging gerepliceerd naar alle andere domeincontrollers.

-   **RPC-server (server voor externe procedureaanroepen) op de server waarop MIM wordt uitgevoerd**: als wachtwoordsynchronisatie is ingeschakeld, wordt de RPC-server gestart op de server waarop MIM wordt uitgevoerd. Daardoor kan de RPC-server meldingen ontvangen van de meldingsservice voor wachtwoordwijzigingen. Het te gebruiken bereik van poorten wordt dynamisch door RPC geselecteerd. Als MIM via een firewall met de Active Directory-forest dient te communiceren, moet u een bereik van poorten openen.

-   **DLL voor wachtwoordextensie**: De DLL voor wachtwoordextensie biedt een manier om het instellen of wijzigen van wachtwoorden te implementeren door middel van een uitbreiding van regels voor een database, uitbreidbare connectiviteit of een op bestanden gebaseerde beheeragent.
    Hiervoor wordt een alleen-exporteren, versleuteld kenmerk gemaakt met de naam export_password, dat niet feitelijk in de verbonden adreslijst aanwezig is, maar toegankelijk is en kan worden ingesteld in inrichtingsregelextensies of dat tijdens het stromen van exportkenmerken kan worden gebruikt. Zie [FIM Developer Reference](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx) (Referentiemateriaal voor FIM-ontwikkelaars) voor meer informatie over het configureren van wachtwoordextensies.

## <a name="preparing-for-password-synchronization"></a>Voorbereiding voor wachtwoordsynchronisatie

Voordat u wachtwoordsynchronisatie instelt voor uw MIM- en Active Directory-omgeving, dient u het volgende te controleren:

-   MIM is geïnstalleerd volgens de installatie-instructies.

-   De beheeragents voor de verbonden gegevensbronnen die voor wachtwoordsynchronisatie moeten worden beheerd, zijn al gemaakt en de objecten zijn samengevoegd en gesynchroniseerd.

Wachtwoordsynchronisatie instellen:

-   Breid het Active Directory-schema uit door de klassen en kenmerken toe te voegen die noodzakelijk zijn voor het installeren en uitvoeren van de PCNS.

-   Installeer de PCNS op alle domeincontrollers.

-   Configureer de SPN (Service Principal Name) in Active Directory voor het MIM-serviceaccount.

-   Configureer de PCNS zodat de service kan communiceren met de MIM-doelservice.

-   Configureer de beheeragents voor de verbonden gegevensbronnen die voor wachtwoordsynchronisatie moeten worden beheerd.

-   Schakel wachtwoordsynchronisatie in op MIM.

Zie Using Password Synchronization (Wachtwoordsynchronisatie gebruiken) voor meer informatie over het instellen van wachtwoordsynchronisatie.

## <a name="password-synchronization-process"></a>Het wachtwoordsynchronisatieproces

Het proces van het synchroniseren van een aanvraag voor een wachtwoordwijziging vanuit een Active Directory-domeincontroller naar overige verbonden gegevensbronnen wordt in het volgende diagram weergegeven:

1.  De gebruiker start de aanvraag voor het wijzigen van een wachtwoord door op Ctrl+Alt+Del te drukken. De aanvraag wordt samen met het nieuwe wachtwoord naar de dichtstbijzijnde domeincontroller verzonden.

2.  De domeincontroller registreert de aanvraag voor de wachtwoordwijziging en stelt het filter voor meldingen van wachtwoordwijzigingen (Pcnsflt.dll) op de hoogte.

3.  Het filter voor meldingen van wachtwoordwijzigingen stuurt de aanvraag door aan de PCNS.

4.  De PCNS controleert de aanvraag en verifieert vervolgens de SPN met behulp van Kerberos. De aanvraag wordt in versleutelde RPC doorgestuurd naar de MIM-doelserver.

5.  MIM valideert de brondomeincontroller en gebruikt vervolgens de domeinnaam om de beheeragent te lokaliseren die als domein fungeert. Vervolgens worden de gebruikersaccountgegevens in de aanvraag voor de wachtwoordwijziging gebruikt om het overeenkomstige object in het connectorgebied te lokaliseren.

6.  Met behulp van de gegevens uit de join-tabel bepaalt MIM de beheeragents die de wachtwoordwijzigingen ontvangen, en pusht de wachtwoordwijzigingen ernaartoe.

## <a name="password-synchronization-security"></a>Beveiliging wachtwoordsynchronisatie

De volgende problemen met de beveiliging van de wachtwoordsynchronisatie zijn in behandeling genomen:

-   Verificatie van de wachtwoordbron: als de melding voor wachtwoordwijziging is ontvangen, wordt Kerberos-verificatie zowel door MIM als de brondomeincontroller uitgevoerd om te controleren of zowel de ontvanger als de afzender geldig zijn. Na ontvangst van een melding voor wachtwoordwijziging zorgt MIM ervoor dat de aanroepende instantie een account heeft in de container van de domeincontroller voor het bijbehorende domein.

-   Wachtwoordsynchronisatie voor een doelgegevensbron mislukt vanwege een onbeveiligde verbinding: als de beheeragent zodanig is geconfigureerd dat er een beveiligde verbinding is vereist maar deze niet wordt gedetecteerd, mislukt de synchronisatie.
    Er treedt wel synchronisatie op als de beheeragent is geconfigureerd om niet-beveiligde verbindingen toe te staan. Niet-beveiligde verbindingen toestaan mag alleen worden ingeschakeld nadat de mogelijke risico's zijn bestudeerd en begrepen.

-   Beveiligde opslag van wachtwoorden: MIM slaat alleen beveiligde wachtwoorden tijdelijk op. De wachtwoorden die door MIM tijdens een melding voor een wachtwoordwijziging worden ontvangen, worden versleuteld zodra ze het MIM-proces binnengaan.
    Zodra de wachtwoorden naar de met het doel verbonden gegevensbron worden verzonden, worden ze ontsleuteld en wordt het geheugen waarin het wachtwoord is opgeslagen, onmiddellijk gewist. Als er niet naar de met het doel verbonden gegevensbron kan worden geschreven, wordt het versleutelde wachtwoord opgeslagen totdat alle nieuwe pogingen zijn geprobeerd. Het wachtwoord wordt vervolgens uit het geheugen gewist.

-   Beveiligde wachtrijen met wachtwoorden: wachtwoorden die in de PCNS-wachtrijen met wachtwoorden zijn opgeslagen, blijven versleuteld totdat ze worden bezorgd.

## <a name="password-synchronization-error-recovery-scenarios"></a>Scenario's voor wachtwoordherstel bij fouten tijdens de synchronisatie

In het ideale geval wordt een door de gebruiker gewijzigd wachtwoord foutloos gesynchroniseerd. De volgende scenario's beschrijven hoe MIM herstelt na algemene synchronisatiefouten:

-   **Wachtwoordmelding van Active Directory aan MIM is mislukt**: dit kan optreden als het netwerk eruit ligt of als de server waarop MIM wordt uitgevoerd, niet beschikbaar is. De melding voor wachtwoordwijzigingen wordt door de PCNS lokaal in de wachtrij op de domeincontroller gehandhaafd. De PCNS probeert de melding opnieuw uit te voeren volgens de configuratie voor het interval voor nieuwe pogingen.

-   **Wachtwoordsynchronisatie met een doelgegevensbron is mislukt**: dit kan ook optreden als het netwerk eruit ligt of als de doelgegevensbron niet beschikbaar is.
    De melding voor wachtwoordwijzigingen wordt in de wachtrij geplaatst en opnieuw geprobeerd volgens de desbetreffende configuratie van de beheeragent. Alle wachtwoorden worden versleuteld terwijl ze worden opgeslagen om opnieuw te worden geprobeerd. Ze worden verwijderd als de bewerking slaagt of als het maximum aantal pogingen is geprobeerd.

-   **Activeren van een warme stand-byserver waarop MIM wordt uitgevoerd na een fout**: in het geval waarbij een fout optreedt met een primaire server waarop MIM wordt uitgevoerd, kunt u een warme stand-byserver configureren voor wachtwoordsynchronisatie en deze activeren zonder dat er wachtwoordwijzigingen verloren gaan. Zie [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx) (MIISactivate: hulpprogramma voor serveractivering) voor meer informatie

Sommige fouten zijn dermate ernstig dat de bewerking ook na zeer veel nieuwe pogingen niet slaagt. In deze gevallen wordt er een foutgebeurtenis gelogd en wordt het proces beëindigd. De volgende gebeurtenissen worden niet opnieuw geprobeerd:

| Gebeurtenis | Ernst    | Description                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Informatie | Een instelbewerking voor wachtwoordsynchronisatie wordt niet uitgevoerd omdat het tijdstempel verouderd is.                                                                      |
| 6921  | Fout       | De instelbewerking voor wachtwoordsynchronisatie wordt niet verwerkt omdat wachtwoordbeheer niet is ingeschakeld op de doelbeheerserver.                                |
| 6922  | Fout       | De instelbewerking voor wachtwoordsynchronisatie wordt niet verwerkt omdat wachtwoordbeheer niet is geconfigureerd op de doelbeheerserver.                             |
| 6923  | Waarschuwing     | De instelbewerking voor wachtwoordsynchronisatie wordt niet verwerkt omdat het doelobject van het connectorgebied niet in de verbonden adreslijst is gevonden.                  |
| 6927  | Fout       | De instelbewerking voor wachtwoordsynchronisatie is mislukt omdat het wachtwoord niet voldoet aan het wachtwoordbeleid van het doelsysteem.                                      |
| 6928  | Fout       | De instelbewerking voor wachtwoordsynchronisatie is mislukt omdat de wachtwoordextensie voor de doelbeheeragent niet is geconfigureerd voor het ondersteunen van instelbewerkingen voor wachtwoorden. |

## <a name="user-based-password-change-management"></a>Op gebruikers gebaseerd beheer voor wachtwoordwijzigingen

MIM biedt twee webtoepassingen die gebruikmaken van Windows Management Instrumentation (WMI) voor het opnieuw instellen van wachtwoorden. Net als met wachtwoordsynchronisatie, activeert u wachtwoordbeheer als u de beheeragent configureert in Management Agent Designer. Zie MIM Developer Reference (Referentiemateriaal voor MIM-ontwikkelaars) voor informatie over wachtwoordbeheer.

In MIM worden tijdens de installatie twee beveiligingsgroepen gemaakt die specifiek bewerkingen voor wachtwoordbeheer ondersteunen:

-   FIMSyncBrowse: leden van deze groep hebben toestemming informatie over de accounts van een gebruiker te verzamelen bij het uitvoeren van zoekbewerkingen met WMI-query's.

-   FIMSyncPasswordSet: leden van deze groep hebben toestemming in accounts te zoeken, wachtwoorden in te stellen en wachtwoorden te wijzigen met de wachtwoordbeheerinterfaces in combinatie met WMI.
