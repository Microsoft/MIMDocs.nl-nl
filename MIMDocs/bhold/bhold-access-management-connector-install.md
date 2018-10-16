---
title: De installatie van BHOLD access management-connector | Microsoft Docs
description: De module van BHOLD-connector ondersteunt initiële en lopende synchronisatie van gegevens
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: d3a4ab5203e772db80c5345aab8f1c66731c8020
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333697"
---
# <a name="access-management-connector-installation"></a>De installatie van Access Management-Connector

De module BHOLD-Suite Access Management-Connector biedt ondersteuning voor initiële en lopende synchronisatie van gegevens in BHOLD. De Access Management-Connector werkt met de synchronisatieservice van Microsoft Identity Manager (MIM) te verplaatsen van gegevens tussen de kern van BHOLD-database, de FIM 2010-metaverse en doeltoepassingen en identity stores. Na de installatie van de module Access Management-Connector, zich kunt u kunt maken van Beheeragents FIM waarmee de gegevensstroom tussen BHOLD en MIM.

## <a name="access-management-connector-software-requirements"></a>Softwarevereisten voor Access Management-Connector

Voordat u de module Access Management-Connector installeren kunt, moet u Microsoft .NET Framework 4 installeren. Zie voor meer informatie over de .NET Framework 4 en installatie-instructies, de [startpagina van Microsoft .NET](http://www.microsoft.com/net).
U moet de toegang Management-Connector installeren op een computer waarop de FIM-synchronisatie-Service van MIM wordt uitgevoerd.

## <a name="access-management-connector-setup"></a>Instellen van bedrijfstoegang Management-Connector

Meld u aan als een lid van de groep Domeinadministrators voor het installeren van de module Beheer van toegang, het volgende bestand downloaden en als administrator uitvoeren op de server die u van plan bent de integratie van BHOLD FIM-module installeren op:

- AccessManagementConnector.msi

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Installatie van BHOLD FIM-integratie](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) om in te schakelen via self-service door eindgebruikers van rollen, kunt u de integratie van BHOLD FIM-module installeren
- [Handleiding voor BHOLD-installatie](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)