---
title: Installatie van BHOLD Access Management-connector | Microsoft Docs
description: De BHOLD-connector module ondersteunt de initiële en actieve synchronisatie van gegevens
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ae9cc0bb4c63089c6733c06b7b035b2b9566fdd0
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041860"
---
# <a name="access-management-connector-installation"></a>Installatie van toegangs beheer connector

De BHOLD Suite Access Management connector-module ondersteunt zowel initiële als actieve synchronisatie van gegevens in BHOLD. De Access Management-connector werkt met de synchronisatie service van Microsoft Identity Manager (MIM) om gegevens te verplaatsen tussen de BHOLD Core-Data Base, de FIM 2010-tekst en de doel toepassingen en identiteits archieven. Na de installatie van de module Access Management connector kunt u FIM-beheer agenten maken waarmee de gegevens stroom tussen BHOLD en MIM wordt beheerd.

## <a name="access-management-connector-software-requirements"></a>Software vereisten voor toegangs beheer connector

Voordat u de module Access Management connector kunt installeren, moet u Microsoft .NET Framework 4 installeren. Ga voor meer informatie over .NET Framework 4 en installatie-instructies naar de [Start pagina van Microsoft .net](https://www.microsoft.com/net).
U moet de Access Management-connector installeren op een computer met de FIM-synchronisatie service van MIM.

## <a name="access-management-connector-setup"></a>Setup van toegangs beheer connector

Als u de Access Management-beheer module wilt installeren, meldt u zich aan als lid van de groep domein Administrators, downloadt u het volgende bestand en voert u het uit als beheerder op de server waarop u de BHOLD FIM-integratie module wilt installeren:

- AccessManagementConnector. msi

Als u het programma bestand als beheerder wilt uitvoeren, klikt u met de rechter muisknop op het bestand en klikt u vervolgens op **als administrator uitvoeren**.

## <a name="next-steps"></a>Volgende stappen

- [Installatie van BHOLD FIM-integratie](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Als u selfservice voor eind gebruikers van rollen wilt inschakelen, kunt u de BHOLD FIM-integratie module installeren
- [Installatie handleiding voor BHOLD](bhold-installation-guide.md)
- [BHOLD-referentie voor ontwikkelaars](../reference/mim2016-bhold-developer-reference.md)
- [Versiegeschiedenis van BHOLD](../reference/version-bhold-history.md)
