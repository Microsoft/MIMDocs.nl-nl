---
title: Handleiding voor topologie van uw implementatie | Microsoft Docs
description: Krijg inzicht in de MIM 2016-onderdelen en profiteer van tips voor het implementeren ervan in uw omgeving.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f59b40f79c58ea5e34fbe090ac9c4a224c561654
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333272"
---
# <a name="topology-considerations"></a>Aandachtspunten voor topologie
U kunt Microsoft Identity Manager-onderdelen (MIM) op dezelfde server of op meerdere servers in verschillende configuraties implementeren. De topologie die u hebt geselecteerd voor uw implementatie, is van invloed op de prestaties van MIM. Dit artikel bevat meerdere implementatietopologieën die u kunt overwegen voor uw implementatie.


> [!NOTE]
> Deze opties zijn van toepassing op implementaties die uitsluitend gebruik maken van MIM-synchronisatie, MIM-Service en MIM-Portal voor identiteitsbeheer.  Implementaties met MIM CM, MIM BHOLD Suite, of voor privileged access management hebben verschillende implementatieopties.


## <a name="mim-components"></a>MIM-onderdelen
Bij het ontwerpen van uw implementatietopologie is het belangrijk dat u weet wat elk onderdeel doet en hoe de onderdelen samenwerken.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**MIM-portal**: een interface voor het opnieuw instellen van wachtwoorden, groepsbeheer en beheerbewerkingen.
    -
- **MIM-service**: een webservice voor de implementatie van de functionaliteit voor identiteitsbeheer in MIM 2016.
- **MIM-synchronisatieservice**: voor synchronisatie van gegevens met andere identiteitssystemen.
- **Microsoft SQL Server**: gegevens van de MIM-service en de MIM-synchronisatieservice worden opgeslagen in SQL-databases.

De volgende tabel bevat de opties voor het hosten van elk van de MIM-onderdelen. De onderdelen worden gehost op dezelfde computer of worden verdeeld over meerdere servers en clusters.

| | MIM-portal | MIM-service | MIM-synchronisatieservice | SQL Server |
| --- | --- | --- | --- | --- |
| Dezelfde computer | Ja | Ja | Ja | Ja |
| Afzonderlijke server | Ja | Ja | Ja | Ja |
| Netwerktaakverdelingscluster | Ja | Ja | | |
| Servercluster | | | | Ja |


## <a name="multitier-topology"></a>Topologie met meerdere lagen
De topologie met meerdere lagen is de meestgebruikte topologie. Deze topologie biedt de grootste flexibiliteit. De MIM-portal, de MIM-service en de databases worden onderverdeeld in lagen en geïmplementeerd op meerdere computers. Met deze topologie voegt u flexibiliteit toe voor het schalen van de verschillende MIM-onderdelen. U kunt de MIM-portal bijvoorbeeld horizontaal schalen door extra servers toe te voegen in een netwerktaakverdelingscluster. Op dezelfde manier kunt u de MIM-service schalen met een netwerktaakverdelingscluster en door het aantal computers (knooppunten) in het cluster naar behoefte te verhogen.

In de topologie met meerdere lagen wordt een specifieke computer toegewezen voor het hosten van elke SQL-database (een voor de MIM-Service en een andere voor de MIM-synchronisatieservice). De schaalbaarheid van de prestaties van de computers die de SQL-databases hosten, kan worden verhoogd met het toevoegen of upgraden van hardware, bijvoorbeeld door het upgraden van de CPU's, het toevoegen van extra CPU's, het uitbreiden of upgraden van het RAM-geheugen, of het upgraden van de hardeschijfconfiguraties om de lees- en schrijftoegang te verhogen en de latentie te verlagen.

![Diagram van een MIM-topologie met meerdere lagen](media/MIM-topo-multitier.png)

In deze configuratie worden de MIM-synchronisatieservice en de bijbehorende database gehost op dezelfde computer. Met een speciale 1-gigabitnetwerkverbinding tussen de MIM-synchronisatieservice en de bijbehorende database kunt u echter soortgelijke prestaties bereiken wanneer ze worden gehost op afzonderlijke computers.


## <a name="multitier-topology-with-multiple-mim-services"></a>Topologie met meerdere lagen met meerdere FIM-services
Synchronisatie van gegevens met externe systemen kan lang duren en gedurende deze periode een aanzienlijke belasting vormen voor het systeem. Als de synchronisatieconfiguratie resulteert in het activeren van beleidsregels met werkstromen, concurreren deze beleidsregels voor bronnen met werkstromen door eindgebruikers. Deze problemen kunnen worden verergerd met verificatiewerkstromen, zoals het opnieuw instellen van wachtwoorden, die in realtime worden uitgevoerd en waarbij een eindgebruiker wacht tot het proces is voltooid. Als u één instantie van de MIM-service aanbiedt voor bewerkingen van eindgebruikers en een afzonderlijke portal voor de synchronisatie van beheergegevens, kunt u eindgebruikers een betere reactiesnelheid bieden.

![Diagram van een meervoudige MIM-topologie met meerdere lagen](media/MIM-topo-multitier-multiservice.png)

Net zoals bij de standaardtopologie met meerdere lagen, kunt u de prestaties van de MIM-portal verbeteren via een netwerktaakverdelingscluster en door het aantal knooppunten in het cluster naar behoefte te verhogen.

De computers met SQL Server die als host fungeren voor de MIM-synchronisatieservice en de MIM-servicedatabase zullen de algehele prestaties van uw MIM-implementatie aanzienlijk beïnvloeden. Volg daarom de aanbevelingen in de SQL Server-documentatie voor het optimaliseren van databaseprestaties. Zie de volgende documenten voor meer informatie:

## <a name="see-also"></a>Zie ook
- De downloadbare [handleiding voor planningscapaciteit van Forefront Identity Manager (FIM) 2010 ](http://go.microsoft.com/fwlink/?LinkId=200180) biedt meer informatie over een testbuild en prestatietestresultaten.
