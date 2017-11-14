---
title: De installatie van BHOLD access management-connector | Microsoft Docs
description: "De module BHOLD-connector ondersteunt de initiële en lopende synchronisatie van gegevens"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2017
---
# <a name="access-management-connector-installation"></a>De installatie van een Access Management-Connector

De module BHOLD-Suite Access Management-Connector ondersteunt de initiële en lopende synchronisatie van gegevens in BHOLD. De Access Management-Connector werkt met de synchronisatieservice van Microsoft Identity Manager (MIM) om gegevens tussen de Core BHOLD-database, de FIM 2010-metaverse en doeltoepassingen en identiteitsopslag te verplaatsen. Na de installatie van de module Access Management-Connector, zich u voor het maken van Beheeragents FIM waarmee de gegevensoverdracht tussen BHOLD en MIM.

## <a name="access-management-connector-software-requirements"></a>Softwarevereisten voor Access Management-Connector

Voordat u de module Access Management-Connector installeren kunt, moet u Microsoft .NET Framework 4 installeren. Zie voor meer informatie over Microsoft .NET Framework 4 en installatie-instructies de [Microsoft .NET-startpagina](http://www.microsoft.com/net).
U moet de Access Management-Connector installeren op een computer met de Service MIM van FIM-synchronisatie.

## <a name="access-management-connector-setup"></a>Instellen van bedrijfstoegang Management-Connector

Meld u aan als lid van de groep Domeinadministrators voor het installeren van de module Beheer van toegangsbeheer, downloadt u het volgende bestand en als administrator uitvoeren op de server die u wilt de integratie van BHOLD FIM-module installeren op:

- AccessManagementConnector.msi

Als u wilt het programmabestand uitvoeren als beheerder, met de rechtermuisknop op het bestand en klik vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Installatie van BHOLD FIM-integratie](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) zodat eindgebruikers selfservice van rollen kunt u de module BHOLD FIM-integratie installeren
- [BHOLD-installatiehandleiding](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)