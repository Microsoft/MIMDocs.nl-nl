---
title: Softwarevereisten voor PAM | Microsoft Docs
description: De hardware- en softwarevereisten voor een correcte implementatie van Privileged Access Management
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/06/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c53d8cc815f792d1a1246a7434350f1cfb087844
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288959"
---
# <a name="hardware-and-software-requirements"></a>Hardware- en softwarevereisten

Privileged Access Management heeft geen hardwarevereisten naast de vereisten van de onderliggende softwareplatforms. Zorg ervoor dat u voldoende geheugen of schijfruimte vrij hebt en beschikt over een netwerkverbinding.

> [!IMPORTANT]
> In dit artikel worden minimale vereisten vermeld voor een standaardimplementatie. Het is niet bedoeld ter illustratie van prestaties, schaalbaarheid of hoge beschikbaarheid. Dit vormt geen geen aanbevolen implementatietopologie voor grote ondernemingen of productieomgevingen.

## <a name="installing-from-software-packages"></a>Installeren vanuit softwarepakketten

De volgende software kan worden gedownload van TechNet Evaluation Center of MSDN:

- Microsoft Identity Manager 2016
  - Service en portal: bevat het installatieprogramma voor de MIM-service, de MIM-portal en het PAM-scenario
  - Invoegtoepassingen en extensies: bevat het installatieprogramma voor de aanvrager van PowerShell-cmdlets

De volgende software kan worden gedownload van GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): Voorbeeldwebtoepassing voor de REST-API bevat

## <a name="required-software"></a>Vereiste software

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 of SQL Server 2014

## <a name="evaluation-software"></a>Evaluatiesoftware

Als u geen licenties hebt voor Windows, SQL Server of Windows Server, kunt u evaluatieversies downloaden.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Microsoft Downloadcentrum

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
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
