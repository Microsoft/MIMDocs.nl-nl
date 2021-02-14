---
title: Softwarevereisten voor PAM | Microsoft Docs
description: De hardware- en softwarevereisten voor een correcte implementatie van Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8d3c266c78df21c6ddb24f62618621c55820bd6f
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395449"
---
# <a name="hardware-and-software-requirements"></a>Hardware- en softwarevereisten

Privileged Access Management heeft geen hardwarevereisten naast de vereisten van de onderliggende software platforms. Zorg ervoor dat u voldoende geheugen of schijfruimte vrij hebt en beschikt over een netwerkverbinding.

> [!IMPORTANT]
> Dit artikel bevat de minimale vereisten voor een eenvoudige implementatie op een geïsoleerd netwerk. Het is niet bedoeld ter illustratie van de prestaties, schaalbaarheid of hoge beschikbaarheid, en vertegenwoordigt geen aanbevolen implementatietopologie voor grote ondernemingen of productieomgevingen.  Als uw Active Directory deel uitmaakt van een omgeving met Internet verbinding, raadpleegt u in plaats daarvan de [uitgebreide toegangs richtlijnen beveiligen](/security/compass/overview) voor meer informatie over waar u moet beginnen.

## <a name="installing-from-software-packages"></a>Installeren vanuit softwarepakketten

De volgende software kan worden gedownload van TechNet Evaluation Center of MSDN:

- Microsoft Identity Manager 2016
  - Service en portal: bevat het installatieprogramma voor de MIM-service, de MIM-portal en het PAM-scenario
  - Invoegtoepassingen en extensies: bevat het installatieprogramma voor de aanvrager van PowerShell-cmdlets

De volgende optionele software kan worden gedownload van GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): bevat een voor beeld-webtoepassing voor de rest API

## <a name="required-software"></a>Vereiste software

- Windows Server 2016
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 of SQL Server 2014

## <a name="hardware-requirements"></a>Hardwarevereisten

Raadpleeg de systeemvereisten van de software voor elk onderdeel van PAM.

Voor CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) of eerder

Voor CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Voor PRIVDC:

- Windows Server 2016

Voor PAMSRV:

- Windows Server 2016
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) of [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
