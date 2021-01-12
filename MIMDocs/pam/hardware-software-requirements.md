---
title: Softwarevereisten voor PAM | Microsoft Docs
description: De hardware- en softwarevereisten voor een correcte implementatie van Privileged Access Management
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 417f26b123aec7e8ab1b8cf254e0771f799dcb9d
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98103966"
---
# <a name="hardware-and-software-requirements"></a>Hardware- en softwarevereisten

Privileged Access Management heeft geen hardwarevereisten naast de vereisten van de onderliggende softwareplatforms. Zorg ervoor dat u voldoende geheugen of schijfruimte vrij hebt en beschikt over een netwerkverbinding.

> [!IMPORTANT]
> In dit artikel worden minimale vereisten vermeld voor een standaardimplementatie. Het is niet bedoeld om prestaties, schaal baarheid of hoge Beschik baarheid te demonstreren. Dit vertegenwoordigt geen aanbevolen implementatie topologie voor grote ondernemingen of productie omgevingen.

## <a name="installing-from-software-packages"></a>Installeren vanuit softwarepakketten

De volgende software kan worden gedownload van TechNet Evaluation Center of MSDN:

- Microsoft Identity Manager 2016
  - Service en portal: bevat het installatieprogramma voor de MIM-service, de MIM-portal en het PAM-scenario
  - Invoegtoepassingen en extensies: bevat het installatieprogramma voor de aanvrager van PowerShell-cmdlets

De volgende software kan worden gedownload van GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): bevat een voor beeld-webtoepassing voor de rest API

## <a name="required-software"></a>Vereiste software

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 of SQL Server 2014

## <a name="evaluation-software"></a>Evaluatiesoftware

Als u geen licenties hebt voor Windows, SQL Server of Windows Server, kunt u evaluatieversies downloaden.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- Windows Server 2012 R2 of hoger
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Microsoft Downloadcentrum

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)  
- [SharePoint Foundation 2013 SP1 en de vereisten](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Hardwarevereisten

Raadpleeg de systeemvereisten van de software voor elk onderdeel van PAM.

Voor CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) of eerder

Voor CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Voor PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Voor PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) of [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
