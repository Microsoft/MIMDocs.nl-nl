---
title: Handleiding voor topologie van uw implementatie | Microsoft Docs
description: Krijg inzicht in de MIM 2016-onderdelen en profiteer van tips voor het implementeren ervan in uw omgeving.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/21/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: f7e4dc737444df70de3a8a78eb518e9e6f26aadc
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---


# Aandachtspunten voor topologie
<a id="topology-considerations" class="xliff"></a>
U kunt Microsoft Identity Manager-onderdelen (MIM) op dezelfde server of op meerdere servers in verschillende configuraties implementeren. De topologie die u hebt geselecteerd voor uw implementatie, is van invloed op de prestaties van MIM. Dit artikel bevat meerdere implementatietopologieën die u kunt overwegen voor uw implementatie.

## MIM-onderdelen
<a id="mim-components" class="xliff"></a>
Bij het ontwerpen van uw implementatietopologie is het belangrijk dat u weet wat elk onderdeel doet en hoe de onderdelen samenwerken.

- **MIM-portal**: een interface voor het opnieuw instellen van wachtwoorden, groepsbeheer en beheerbewerkingen.
<a id="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations" class="xliff"></a>
    -
- **MIM-service**: een webservice voor de implementatie van de functionaliteit voor identiteitsbeheer in MIM 2016.
- **MIM-synchronisatieservice**: voor synchronisatie van gegevens met andere identiteitssystemen.
- **Microsoft SQL Server**: gegevens van de MIM-service en de MIM-synchronisatieservice worden opgeslagen in SQL-databases.

De volgende tabel bevat de opties voor het hosten van elk van de MIM-onderdelen. De onderdelen worden gehost op dezelfde computer of worden verdeeld over meerdere servers en clusters.

| | MIM-portal | MIM-service | MIM-synchronisatieservice | SQL Server |
| --- | --- | --- | --- | --- |
| Dezelfde computer | Ja | Ja | Ja | Yes |
| Afzonderlijke server | Ja | Ja | Ja | Yes |
| Netwerktaakverdelingscluster | Yes | Ja | | |
| Servercluster | | | | Yes |


## Topologie met meerdere lagen
<a id="multitier-topology" class="xliff"></a>
De topologie met meerdere lagen is de meestgebruikte topologie. Deze topologie biedt de grootste flexibiliteit. De MIM-portal, de MIM-service en de databases worden onderverdeeld in lagen en geïmplementeerd op meerdere computers. Met deze topologie voegt u flexibiliteit toe voor het schalen van de verschillende MIM-onderdelen. U kunt de MIM-portal bijvoorbeeld horizontaal schalen door extra servers toe te voegen in een netwerktaakverdelingscluster. Op dezelfde manier kunt u de MIM-service schalen met een netwerktaakverdelingscluster en door het aantal computers (knooppunten) in het cluster naar behoefte te verhogen.

In de topologie met meerdere lagen wordt een specifieke computer toegewezen voor het hosten van elke SQL-database (een voor de MIM-Service en een andere voor de MIM-synchronisatieservice). De schaalbaarheid van de prestaties van de computers die de SQL-databases hosten, kan worden verhoogd met het toevoegen of upgraden van hardware, bijvoorbeeld door het upgraden van de CPU's, het toevoegen van extra CPU's, het uitbreiden of upgraden van het RAM-geheugen, of het upgraden van de hardeschijfconfiguraties om de lees- en schrijftoegang te verhogen en de latentie te verlagen.

![Diagram van een MIM-topologie met meerdere lagen](media/MIM-topo-multitier.png)

In deze configuratie worden de MIM-synchronisatieservice en de bijbehorende database gehost op dezelfde computer. Met een speciale 1-gigabitnetwerkverbinding tussen de MIM-synchronisatieservice en de bijbehorende database kunt u echter soortgelijke prestaties bereiken wanneer ze worden gehost op afzonderlijke computers.


## Topologie met meerdere lagen met meerdere FIM-services
<a id="multitier-topology-with-multiple-mim-services" class="xliff"></a>
Synchronisatie van gegevens met externe systemen kan lang duren en gedurende deze periode een aanzienlijke belasting vormen voor het systeem. Als de synchronisatieconfiguratie resulteert in het activeren van beleidsregels met werkstromen, concurreren deze beleidsregels voor bronnen met werkstromen door eindgebruikers. Deze problemen kunnen worden verergerd met verificatiewerkstromen, zoals het opnieuw instellen van wachtwoorden, die in realtime worden uitgevoerd en waarbij een eindgebruiker wacht tot het proces is voltooid. Als u één instantie van de MIM-service aanbiedt voor bewerkingen van eindgebruikers en een afzonderlijke portal voor de synchronisatie van beheergegevens, kunt u eindgebruikers een betere reactiesnelheid bieden.

![Diagram van een meervoudige MIM-topologie met meerdere lagen](media/MIM-topo-multitier-multiservice.png)

Net zoals bij de standaardtopologie met meerdere lagen, kunt u de prestaties van de MIM-portal verbeteren via een netwerktaakverdelingscluster en door het aantal knooppunten in het cluster naar behoefte te verhogen.

De computers met SQL Server die als host fungeren voor de MIM-synchronisatieservice en de MIM-servicedatabase zullen de algehele prestaties van uw MIM-implementatie aanzienlijk beïnvloeden. Volg daarom de aanbevelingen in de SQL Server-documentatie voor het optimaliseren van databaseprestaties. Zie de volgende documenten voor meer informatie:

## Zie tevens
<a id="see-also" class="xliff"></a>
- De downloadbare [handleiding voor planningscapaciteit van Forefront Identity Manager (FIM) 2010 ](http://go.microsoft.com/fwlink/?LinkId=200180) biedt meer informatie over een testbuild en prestatietestresultaten.

