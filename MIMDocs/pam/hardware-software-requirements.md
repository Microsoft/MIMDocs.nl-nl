---
title: Softwarevereisten voor PAM | Microsoft Identity Manager
description: De hardware- en softwarevereisten voor een correcte implementatie van Privileged Access Management
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 75a748f7035cfb10e833e4fdbfdc208b5245d3ea


---

# Hardware- en softwarevereisten

Privileged Access Management heeft geen hardwarevereisten naast de vereisten van de onderliggende softwareplatforms. Zorg ervoor dat u voldoende geheugen of schijfruimte vrij hebt en beschikt over een netwerkverbinding.

In dit artikel worden minimale vereisten vermeld voor een standaardimplementatie. Het is niet bedoeld ter illustratie van de prestaties, schaalbaarheid of hoge beschikbaarheid, en vertegenwoordigt geen aanbevolen implementatietopologie voor grote ondernemingen of productieomgevingen.

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



<!--HONumber=Jul16_HO3-->

