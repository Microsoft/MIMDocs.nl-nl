---
title: Hardware- en softwarevereisten | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

Er zijn geen hardwarevereisten buiten die van de onderliggende softwareplatformen; voldoende geheugen of schijfruimte en netwerkverbinding is vereist. In dit artikel worden minimale vereisten vermeld voor een standaardimplementatie. Het is niet bedoeld ter illustratie van de prestaties, schaalbaarheid of hoge beschikbaarheid, en vertegenwoordigt geen aanbevolen implementatietopologie voor grote ondernemingen of productieomgevingen.

## Installeren vanuit softwarepakketten

De volgende software kan worden gedownload van TechNet Evaluation Center of MSDN:  
- Microsoft Identity Manager 2016
  - Service en portal: bevat het installatieprogramma voor de MIM-service, de MIM-portal en het PAM-scenario
  - Invoegtoepassingen en extensies: bevat het installatieprogramma voor de aanvrager van PowerShell-cmdlets

De volgende software kan worden gedownload van GitHub:  
- PAMSamplePortal: bevat een voorbeeldwebtoepassing voor de REST API

## Vereiste software

- Windows Server 2012 R2  
- Windows 8.1 Enterprise of Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 of SQL Server 2014  

## Evaluatiesoftware

Als u geen licenties hebt voor Windows, SQL Server of Windows Server, kunt u evaluatieversies downloaden.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft Downloadcentrum

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 en de vereisten](https://www.microsoft.com/download/details.aspx?id=42039)

## Hardwarevereisten

Raadpleeg de systeemvereisten van de software voor elk onderdeel van PAM.

Voor CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) of eerder

Voor CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Voor PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Voor PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) of [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jun16_HO3-->


