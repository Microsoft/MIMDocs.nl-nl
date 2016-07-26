---
title: Herstel na noodgevallen in PAM | Microsoft Identity Manager
description: Meer informatie over het configureren van Privileged Access Management voor een hoge beschikbaarheid en herstel na noodgevallen.
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 9164e48bf10fa27ff6c87ba3816b586a940dda69


---

# Overwegingen voor hoge beschikbaarheid en herstel na noodgevallen voor de bastionomgeving
In dit artikel worden de overwegingen beschreven voor hoge beschikbaarheid en herstel na noodgevallen bij de implementatie van Active Directory Domain Services (AD DS) en Microsoft Identity Manager 2016 (MIM) voor Privileged Access Management PAM.

Ondernemingen concentreren zich op hoge beschikbaarheid en herstel na noodgevallen voor werkbelastingen in Windows Server, SQL Server en Active Directory. Maar de betrouwbare beschikbaarheid van de bastionomgeving voor Privileged Access Management is ook belangrijk. De bastionomgeving speelt een cruciale rol in de IT-infrastructuur van de onderneming omdat gebruikers communiceren met de onderdelen om beheerdersrollen te kunnen aannemen. U kunt het technische document [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc) (Overzicht van hoge beschikbaarheid van Microsoft) voor meer informatie over maximale beschikbaarheid in het algemeen.

## Scenario’s voor hoge beschikbaarheid en herstel na noodgevallen

Bij het plannen van hoge beschikbaarheid en herstel na noodgevallen, moet u de volgende vragen in overweging nemen:

- Welke functies kunnen worden beïnvloed door een storing?
- Welke functies zijn essentieel voor het bedrijf en/of essentieel voor IT-team?
- Wat zijn de risico's die kunnen leiden tot een storing in deze systemen?

Het bereik van deze overwegingen heeft impact op de totale kosten voor implementatie en bewerkingen, zodat organisaties mogelijk bepaalde functies een hogere prioriteit geven en ook het risico van tijdelijke storingen accepteren voor functies met lagere prioriteit. In de volgende tabel wordt een mogelijk prioriteitsoverzicht gegeven dat een organisatie kan aanhouden:

| **Functie van het bastionforest** | **Relatieve prioriteit tijdens het herstel** | **Migratie als de functie niet beschikbaar is** |
| --------------------------- | --------------------- | -------------- |
| Een vertrouwensrelatie tot stand brengen         | Laag | Wachten totdat de bastionomgeving is hersteld |
| Beperking van gebruikers en groepen   | Laag | Wachten totdat de bastionomgeving is hersteld |
| MIM-beheer          | Laag | Wachten totdat de bastionomgeving is hersteld |
| Activering van bevoorrechte rollen  | Gemiddeld | Toegewezen smartcardaccounts om handmatig gebruikers toe te voegen aan beheergroepen |
| Resourcebeheer         | Hoog | Toegewezen smartcardaccounts om handmatig gebruikers toe te voegen aan beheergroepen |
| Bewaking van gebruikers en groepen in een bestaand forest | Laag | Wachten totdat de bastionomgeving is hersteld |

Laten we nu eens kijken naar elke functie van het bastionforest.

### Een vertrouwensrelatie tot stand brengen

Er moet een forestvertrouwensrelatie zijn tussen de domeinen van het bestaande forest en het forest van de bastionomgeving. Op deze manier kunnen gebruikers die zich verifiëren bij de bastionomgeving resources beheren in de bestaande forests. Er is mogelijk aanvullende configuratie vereist, bijvoorbeeld om de migratie van gebruikers van de bestaande domeinen in eerdere versies van Windows Server toe te staan.

Voor het tot stand brengen van een vertrouwensrelatie moeten de bestaande forestdomeincontrollers online zijn, evenals de MIM- en AD-onderdelen van de bastionomgeving.  Als er een storing optreedt van een van deze onderdelen tijdens het tot stand brengen van de vertrouwensrelatie, kan de beheerder het opnieuw proberen zodra de storing is opgelost.  Wanneer de bestaande forestdomeincontrollers of bastionomgeving zijn hersteld na een storing, bevat MIM ook PowerShell-cmdlets `Test-PAMTrust` en `Test-PAMDomainConfiguration` die kunnen worden gebruikt om te verifiëren of er nog steeds een vertrouwensrelatie is.

### Migratie van gebruikers en groepen

Zodra een vertrouwensrelatie tot stand is gebracht, kunnen schaduwgroepen worden gemaakt in de bastionomgeving, evenals gebruikersaccounts voor leden van deze groepen en fiatteurs. Hierdoor kunnen die gebruikers bevoorrechte rollen activeren en opnieuw effectieve groepslidmaatschappen krijgen.

Voor gebruikers- en groepsmigratie moeten de bestaande forestdomeincontrollers online zijn, evenals de MIM- en AD-onderdelen van de bastionomgeving.   Als de bestaande forestdomeincontrollers niet bereikbaar zijn, kunnen er geen extra gebruikers en groepen worden toegevoegd aan de bastionomgeving, maar bestaande gebruikers en groepen worden niet beïnvloed. Als er een storing optreedt van een van deze onderdelen tijdens de migratie, kan de beheerder het opnieuw proberen zodra de storing is opgelost.

### MIM-beheer
Als gebruikers en groepen zijn gemigreerd, kan een beheerder de roltoewijzingen waarmee gebruikers worden gekoppeld als kandidaten voor activering in rollen verder configureren in MIM.  Ze kunnen ook het MIM-beleid configureren voor goedkeuring en Azure MFA.  

Voor MIM-beheer is vereist dat de MIM- en AD-onderdelen van de bastionomgeving online zijn.

### Activering van bevoorrechte rollen
Wanneer een gebruiker een bevoorrechte rol wil activeren, moet deze zich verifiëren bij de bastionomgeving en een aanvraag indienen bij MIM.  MIM bevat SOAP en REST API's, evenals gebruikersinterfaces in PowerShell en een webpagina.

Voor activering van bevoorrechte rollen is vereist dat de MIM- en AD-onderdelen van de bastionomgeving online zijn.  Als MIM daarnaast is geconfigureerd voor gebruik van [Azure MFA voor activering](use-azure-mfa-for-activation.md) van de geselecteerde rol, is internettoegang vereist om contact op te nemen met de Azure MFA-service.

### Resourcebeheer
Wanneer een gebruiker is geactiveerd voor de rol, kan de domeincontroller een Kerberos-ticket genereren voor deze gebruiker dat kan worden gebruikt door domeincontrollers in de bestaande domeinen. Hiermee worden de nieuwe tijdelijke groepslidmaatschappen van de gebruiker herkend.

Voor resourcebeheer is vereist dat een domeincontroller voor het resourcedomein online is, evenals een domeincontroller in de bastionomgeving.  Wanneer een gebruiker is geactiveerd, hoeven MIM of SQL niet online te zijn in de bastionomgeving om het Kerberos-ticket uit te geven.  (Als Windows Server 2012 R2 het functionele niveau voor de bastionomgeving is, moet MIM online zijn om het tijdelijke lidmaatschap van de groep te beëindigen.)

### Bewaking van gebruikers en groepen in het bestaande forest
MIM bevat ook een PAM-controleservice die regelmatig de gebruikers en groepen in de bestaande domeinen controleert en de MIM-database en AD dienovereenkomstig bijwerkt.  Deze service hoeft niet online te zijn voor rolactivering of tijdens resourcebeheer.

Voor bewaking moeten de bestaande forestdomeincontrollers online zijn, evenals de MIM- en AD-onderdelen van de bastionomgeving.  

## Implementatie-opties
Het [overzicht van de omgeving](environment-overview.md) laat een basistopologie zien die geschikt is voor het leren van de technologie en die niet is bedoeld voor maximale beschikbaarheid. In deze sectie wordt beschreven hoe u die topologie kunt uitbreiden voor hoge beschikbaarheid, voor organisaties met één site evenals met meerdere bestaande sites.

### Netwerken

Het netwerkverkeer tussen de computers in de bastionomgeving moet worden geïsoleerd van de bestaande netwerken, bijvoorbeeld door een ander fysiek of virtueel netwerk te gebruiken.  Afhankelijk van de risico's voor de bastionomgeving, kan ook het nodig zijn om onafhankelijke fysieke verbindingen tussen de computers tot stand te brengen.  Voor bepaalde failoverclustertechnologieën gelden aanvullende vereisten voor netwerkinterfaces.

De computers waarop Active Directory Domain Services worden gehost en de computers die de MIM-services hosten in de bastionomgeving vereisen bidirectionele verbinding met de resources in het bestaande forest zodat:
- gebruikers kunnen worden geverifieerd door de PRIV-forestdomeincontrollers
- gebruikers activering kunnen aanvragen
- gebruikers Kerberos-tickets kunnen ontvangen voor resources in een bestaand forest
- de bestaande forestdomeinen kunnen worden gecontroleerd met MIM
- e-mailberichten van MIM via mailservers in het bestaande forest kunnen worden verzonden.

### Minimale topologieën voor hoge beschikbaarheid
Een organisatie kan selecteren voor welke functies in de bastionomgeving hoge beschikbaarheid is vereist, met de volgende beperkingen:

- Voor hoge beschikbaarheid voor elke functie die wordt geleverd door de bastionomgeving zijn ten minste twee domeincontrollers vereist.  
- Voor hoge beschikbaarheid voor activeringsaanvragen zijn ten minste twee computers vereist die als host fungeren voor de MIM-service en hiervoor is ook hoge beschikbaarheid vereist voor de SQL-server.
- Voor hoge beschikbaarheid van de SQL-server met failoverclusters zijn ten minste twee servers vereist met SQL Server. Deze mogen niet dezelfde zijn als een domeincontroller.
- De MIM-service moet niet worden geïnstalleerd op de domeincontroller om de kwetsbaarheid van elke server te beperken.

De kleinste hoge beschikbaarheidstopologie voor alle functies in een bastionomgeving omvat ten minste vier servers en gedeelde opslag. Twee van de servers moeten zijn geconfigureerd als domeincontrollers om Active Directory Domain Services te leveren. De andere twee servers kunnen worden geconfigureerd als een failovercluster voor SQL Server en om de MIM-service leveren.

Daarnaast bevat een normale implementatie van de bastionomgeving ook een werkstation voor bevoorrecht beheer voor het beheren van deze servers, evenals een bewakingsonderdeel

In het volgende diagram wordt een mogelijke architectuur weergegeven:

![bastiontopologie - diagram](media/bastion1.png)

Er kunnen extra servers worden geconfigureerd voor elk van deze functies om betere prestaties te leveren bij belastingsvoorwaarden of voor geografische redundantie zoals hieronder wordt beschreven.

### Implementaties die meerdere sites ondersteunen
De juiste implementatietopologie kiezen voor resources die zijn geïmplementeerd op meerdere sites is afhankelijke van drie factoren:  
- Doelen en risico’s voor hoge beschikbaarheid en herstel na noodgevallen  
- De hardwaremogelijkheden voor het hosten van de bastionomgeving  
- Het werkmodel voor beheer voor elke site.

Een van de meest eenvoudige manieren is om de bastionomgeving op een bepaalde site te hosten.  Onder normale omstandigheden maken gebruikers verbinding met de MIM-implementatie in de bastionomgeving van die site en vragen activering aan. De activeringen worden toegepast op de resources in alle sites.  Als de netwerkverbinding wordt verbroken of de site die als host fungeert voor de bastionomgeving is niet beschikbaar, kunnen offlinereferenties worden gebruikt op een andere site om tijdelijk beheer uit te voeren totdat de netwerkverbinding weer is hersteld.  Mogelijk is deze benadering geschikt voor situaties waarbij lokaal beheer van een bepaalde site, zoals een filiaal, niet vaak voorkomen en zijn beperkt tot het opnieuw verbinden van die site met de rest van het netwerk van een organisatie.

![Eén bastion voor topologieën met meerdere sites - diagram](media/bastion2.png)

Voor hoge beschikbaarheid en herstel na noodgevallen tussen sites is het ook mogelijk om de onderdelen van de bastionomgeving voor elke site te implementeren waarbij een gemeenschappelijk PRIV-map en algemene SQL-database worden gedeeld.  In deze topologie moet de netwerkkoppeling worden verbroken; gebruikers op elke site kunnen onafhankelijk blijven functioneren.

![Meerdere bastions voor topologieën met meerdere sites - diagram](media/bastion3.png)

Een beperking voor deze implementatiebenadering is dat er voor SQL Server een cluster is vereist dat aanwezig is op beide sites, wat moeilijk te implementeren is. In dit geval kunt u als alternatief alleen de Active Directory (PRIV-forest) van de bastionomgeving repliceren.  Als de netwerkverbinding wordt verbroken tussen sites, kunnen gebruikers op site B die hun bevoorrechte rollen al eerder hebben geactiveerd, blijven doorwerken aan het beheer van resources op site B.

![Gerepliceerd bastion voor topologieën met meerdere sites - diagram](media/bastion4.png)

Als elke site een afzonderlijke beheergrens vertegenwoordigt, is het ook mogelijk om meerdere onafhankelijke bastionomgevingen te implementeren.  Hoewel elke bastionomgeving dezelfde software gebruikt, zijn de domeinnamen in de omgevingen verschillend en zijn er geen overeenkomsten tussen de mappen en databases van elke bastionomgeving. Een gebruiker die resources wil beheren op een bepaalde site moet een gebruikersaccount activeren in de bastionomgeving van die site.

![Onafhankelijke bastions voor topologieën met meerdere sites - diagram](media/bastion5.png)

Ten slotte zijn er complexere implementaties mogelijk waarbij meerdere bastionomgevingen afzonderlijk kunnen worden geconfigureerd om resources in een bepaald domein te beheren.

![Complex bastion voor topologieën met meerdere sites - diagram](media/bastion6.png)

### Gehoste bastionomgeving
Sommige organisaties hebben ook overwogen om de bastionomgeving onafhankelijk van de bestaande sites tot stand te brengen. De software van de bastionomgeving kan worden gehost op een virtualisatieplatform binnen het netwerk van de organisatie of op een externe hostingprovider.  Houd rekening met het volgende wanneer u deze aanpak evalueert:

- Als u bescherming wilt instellen tegen aanvallen van de bestaande domeinen, moet het beheer van de bastionomgeving worden geïsoleerd van de beheerdersaccounts van het bestaande domein.
- Er is een TCP/IP-verbinding met de domeincontrollers in het bestaande domein bereist voor de bastionomgeving.  U kunt een lijst met poorten vinden op [How to configure a firewall for domains and trusts](https://support.microsoft.com/kb/179442) (Een firewall configureren voor domeinen en vertrouwensrelaties).
- Voor een gevirtualiseerde implementatie van Active Directory Domain Services zijn specifieke functies van het virtualisatieplatform vereist zoals beschreven in [Gevirtualiseerde domeindcontrollers implementeren en configureren](https://technet.microsoft.com/library/jj574223.aspx).
- Voor een implementatie met hoge beschikbaarheid van SQL Server voor de MIM-service is een speciale opslagconfiguratie vereist zoals beschreven in de sectie [Database-opslag voor SQL Server](#sql-server-database-storage) hieronder.  Niet alle hostingproviders bieden momenteel Windows Server-hosting met schijfconfiguraties die geschikt zijn voor SQL Server-failoverclusters.

## Procedures voor de implementatie en het herstel van implementaties
Tijdens de voorbereiding voor een implementatie van de bastionomgeving met hoge beschikbaarheid en die gereed is voor herstel na noodgevallen moet worden nagedacht over hoe Windows Server Active Directory, SQL Server en de bijbehorende database in gedeelde opslag en de MIM-service en de bijbehorende PAM-onderdelen moeten worden geïnstalleerd.

### Windows Server
Windows Server bevat een ingebouwde functie voor maximale beschikbaarheid, waardoor meerdere computers kunnen samenwerken als een failovercluster. De geclusterde servers zijn verbonden via fysieke kabels en software. Als een of meer van de clusterknooppunten uitvallen, worden andere knooppunten actief (dit proces wordt failover genoemd).   Zie het [Overzicht van failoverclustering](https://technet.microsoft.com/library/hh831579.aspx) voor meer informatie.

Zorg ervoor dat het besturingssysteem en de toepassingen in de bastionomgeving updates ontvangen voor beveiligingsproblemen. Voor sommige van deze updates moet de server mogelijk opnieuw worden opgestart. U moet de tijdstippen waarop updates worden toegepast op de server goed coördineren om uitgebreide onderbrekingen te voorkomen. Een aanpak is het gebruik van [clusterbewust bijwerken](https://technet.microsoft.com/library/hh831694.aspx) voor de servers in een Windows Server-failovercluster.

De servers in de bastionomgeving zijn toegevoegd aan een domein en zijn afhankelijk van de domeinservices. Zorg ervoor dat ze niet per ongeluk zijn geconfigureerd met een afhankelijkheid op een bepaalde domeincontroller voor services zoals DNS.

### Active Directory in de bastionomgeving
Windows Server Active Directory Domain Services biedt systeemeigen ondersteuning voor hoge beschikbaarheid en herstel na noodgevallen.

#### Voorbereiding
Een standaard productie-implementatie van Privileged Access Management bevat ten minste twee domeincontrollers in de bastionomgeving. Instructies voor het instellen van de eerste domeincontroller in de bastionomgeving vindt u in stap 2 van de artikelen over implementatie: [De PRIV-domeincontroller voorbereiden](step-2-prepare-priv-domain-controller.md).

De procedure voor het toevoegen van een extra domeincontroller kan worden gevonden in [Een replica van een Windows Server 2012-domeincontroller installeren in een bestaand domein (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Lees de waarschuwingen in [Gevirtualiseerde domeincontrollers implementeren en configureren](https://technet.microsoft.com/library/jj574223.aspx) wanneer de domeincontroller moet worden gehost op een virtualisatieplatform zoals Hyper-V.

#### Herstel
Zorg er na een storing voor dat er ten minste één domeincontroller beschikbaar is in de bastionomgeving voordat u de andere servers opnieuw start.

Binnen een domein worden met Active Directory de FSMO-rollen (Flexible Single Master Operation) gedistribueerd tussen domeincontrollers, zoals beschreven in [De werking van Operations-masters](https://technet.microsoft.com/library/cc780487.aspx).  Als er een storing optreedt in een domeincontroller, moet u misschien een of meer van de [domeincontrollerrollen](https://technet.microsoft.com/library/cc786438.aspx) overzetten die aan dat domein zijn toegewezen.

Nadat u hebt vastgesteld dat een domeincontroller niet meer kan worden ingezet voor productie, moet u controleren of er rollen zijn toegewezen aan die domeincontroller en deze zo nodig opnieuw toewijzen. Zie [View the Current Operations Master Role Holders](https://technet.microsoft.com/library/cc816893.aspx) (De huidige Operations Master-roleigenaren weergeven) en verwante artikelen voor instructies.

U kunt het beste ook de DNS-instellingen controleren van computers die zijn toegevoegd aan de bastionomgeving, evenals de domeincontrollers in CORP-domeinen met een vertrouwensrelatie met die domeincontroller om er zeker van te zijn dat er geen domeincontrollers zijn gecodeerd met een afhankelijkheid op het IP-adres van die domeincontroller.

### Database-opslag voor SQL Server
Voor een implementatie met hoge beschikbaarheid zijn SQL Server-failoverclusters vereist. Deze SQL Server-failoverclusterinstanties zijn afhankelijk van gedeelde opslag tussen alle knooppunten voor de opslag van de database en logboekbestanden. De gedeelde opslag kan bestaan uit Windows Server Failover Clustering-clusterschijven, schijven op een Storage Area Network (SAN) of bestandsshares op een SMB-server.  Houd er rekening mee dat deze moeten worden toegewezen aan de bastionomgeving; opslag delen met andere werkbelastingen buiten de bastionomgeving wordt niet aanbevolen omdat dit de integriteit van de bastionomgeving in gevaar kan brengen.

### SQL Server
Voor de MIM-service is een SQL Server-implementatie in de bastionomgeving vereist.   SQL kan worden geïmplementeerd met een failoverclusterinstatie voor hoge beschikbaarheid. In tegenstelling tot in zelfstandige instanties wordt de hoge beschikbaarheid van SQL Server in failoverclusterinstanties beveiligd door de aanwezigheid van redundante knooppunten in de failoverclusterinstanties. Tijdens een storing of een geplande upgrade wordt het eigendom van de resourcegroep verplaatst naar een ander Windows Server-failoverclusterknooppunt.

Als u alleen ondersteuning nodig hebt voor herstel na noodgevallen en geen hoge beschikbaarheid, kunnen de back-upfunctie voor logboekbestanden, transactiereplicatie, momentopnamereplicatie of databasespiegeling worden gebruikt in plaats van failoverclustering.   

#### Voorbereiding
Wanneer u de SQL-server installeert in de bastionomgeving, moet deze onafhankelijk zijn van SQL-servers die al aanwezig in de CORP-forests.  Bovendien wordt het aanbevolen de SQL-server te implementeren op een toegewezen server die niet gelijk is aan die van de domeincontroller.
In de SQL Server-handleiding voor [Exemplaren van AlwaysOn-failoverclusters](https://msdn.microsoft.com/library/ms189134.aspx) kunt u meer informatie vinden.

#### Herstel
Als SQL Server is geconfigureerd voor herstel na noodgevallen met de back-upfunctie voor logboekverzending, moet er actie moet worden ondernomen om SQL Server bij te werken tijdens het herstel.  Bovendien moeten alle instanties van de MIM-service opnieuw worden gestart.

Als er een probleem is met SQL Server of de verbinding tussen SQL Server en de MIM-service is verbroken, kunt het beste alle MIM-services opnieuw starten nadat SQL Server is hersteld.  Dit zorgt ervoor dat de verbinding tussen de MIM-service en SQL Server wordt hersteld.

### MIM-service
De MIM-Service is vereist voor het verwerken van activeringsaanvragen.  Een computer waarop de MIM-service wordt gehost kan alleen worden uitgeschakeld voor onderhoud terwijl er nog activeringsaanvragen worden ontvangen, kunnen er meerdere computers voor de MIM-service worden geïmplementeerd.  De MIM-service wordt niet gebruikt bij Kerberos-bewerking wanneer een gebruiker is toegevoegd aan een groep.  

#### Voorbereiding
U kunt het beste de MIM-service op meerdere servers implementeren die zijn toegevoegd aan het PRIV-domein.
Raadpleeg de documentatie van Windows Server voor [Hardwarevereisten en opslagmoeilijkheden voor failoverclustering](https://technet.microsoft.com/library/jj612869.aspx) en [Failoverclusters maken in Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx) voor hoge beschikbaarheid.

Voor de productie-implementatie op meerdere servers kunt u netwerktaakverdeling gebruiken om de verwerkingsbelasting te verdelen.  U moet ook beschikken over één alias (bijvoorbeeld: A of CNAME-records) zodat de gebruiker een algemene naam te zien krijgt.

>[!IMPORTANT]
> Als u een andere technologie voor taakverdeling dan de netwerktaakverdelingsfunctie van Windows Server 2012 R2 gebruikt, moet u ervoor zorgen dat er met uw oplossing een sessie naar dezelfde server wordt omgeleid en niet naar een willekeurige server.

Elke MIM-implementatie met meerdere servers heeft elke MIM-service een externe hostnaam, een servicenaam en een servicepartitienaam.  De standaardwaarde van de servicenaam is de naam van de computer en de standaardwaarde van de externe hostnaam en servicepartitienaam worden geconfigureerd tijdens de installatie van de MIM-service in het scherm waarin u wordt gevraagd om het serveradres van de MIM-service. Deze drie namen worden opgeslagen in het bestand %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config als kenmerken `externalHostName`, `serviceName` en `servicePartitionName` van het `resourceManagementService`-configuratieknooppunt.  

Als een MIM-service een aanvraag ontvangt, wordt de servicepartitienaam opgeslagen als een kenmerk voor deze aanvraag.   Vervolgens kunnen alleen andere installaties van de MIM-service met dezelfde servicepartitienaam deze aanvraag verwerken.  Als het PAM-scenario meerdere handmatige goedkeuringen bevat of andere langdurige aanvraagverwerkingen, moet u er dus voor zorgen dat elke MIM-service hetzelfde `servicePartitionName`-kenmerk bevat in dat configuratiebestand.

#### Herstel
Zorg ervoor dat er na een storing ten minste één Active Directory-domeincontroller en SQL-server beschikbaar zijn in de bastionomgeving voordat u de MIM-service opnieuw start.  

Een werkstroominstantie kan alleen worden voltooid door een MIM-serviceserver die dezelfde servicepartitienaam en servicenaam heeft als de MIM-serviceserver waarmee deze is gestart.  Als er een storing optreedt op een bepaalde computer tijdens het hosten van een MIM-service waarmee aanvragen werden verwerkt, en die computer wordt niet meer in gebruik genomen, moet de MIM-service op een nieuwe computer worden geïnstalleerd. Nadat de installatie is voltooid, moet u op de nieuwe MIM-service het bestand *resourcemanagementservice.exe.config* aanpassen en de kenmerken `serviceName` en `servicePartitionName` van de nieuwe MIM-implementatie instellen op dezelfde hostnaam en servicepartitienaam als de computer waarop de storing is opgetreden.

### MIM PAM-onderdelen
Het installatieprogramma voor de MIM-service en de portal bevat ook extra PAM-onderdelen, inclusief PowerShell-modules en twee services.

#### Voorbereiding
De Privileged Access Management-onderdelen moeten worden geïnstalleerd op elke computer in de bastionomgeving waar de MIM-service wordt geïnstalleerd.  Ze kunnen niet later worden toegevoegd.

#### Herstel
Zorg ervoor dat de MIM-service wordt uitgevoerd op ten minste één server na het herstel van een storing.  Zorg ervoor dat de MIM PAM-bewakingsservice ook op die server wordt uitgevoerd met behulp van `net start "PAM Monitoring service"`.

Als het functionele forestniveau van de bastionomgeving Windows Server 2012 R2 is, moet u ervoor zorgen dat de MIM PAM-onderdeelservice ook wordt uitgevoerd op die server met de opdracht `net start "PAM Component service"`.



<!--HONumber=Jul16_HO3-->


