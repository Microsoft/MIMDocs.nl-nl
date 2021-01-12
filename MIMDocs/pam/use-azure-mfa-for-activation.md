---
title: Azure MFA gebruiken om PAM te activeren | Microsoft Docs
description: Azure MFA instellen als een tweede beveiligingslaag wanneer uw gebruikers rollen in Privileged Access Management activeren.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 1c26147dc1e192804011ee104989a508acb25974
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104051"
---
# <a name="using-azure-mfa-for-activation-in-mim-pam"></a>Azure MFA gebruiken voor activering in MIM PAM
> [!IMPORTANT]
> In eerdere versies van MIM werd de Software Development Kit (SDK) voor Azure Multi-Factor Authentication (MFA) gebruikt om te integreren met Azure MFA.  Vanwege de afschaffing van de Azure MFA SDK kunnen bestaande en nieuwe MIM-klanten de Azure MFA SDK niet meer downloaden. Voor informatie over het gebruik van Azure MFA-server in plaats van de Azure MFA SDK raadpleegt [u Azure MFA server gebruiken in pam of SSPR](../working-with-mfaserver-for-mim.md).




Wanneer u een PAM-rol configureert, kunt u hoe gebruikers die een aanvragen voor activering van de rol verzenden, moeten worden geautoriseerd. Met de PAM-autorisatieactiviteit worden de volgende keuzen geïmplementeerd:

- Goedkeuring van roleigenaar
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Als geen van beide opties is ingeschakeld, worden kandidaatgebruikers automatisch geactiveerd voor de betreffende rol.

Microsoft Azure Multi-Factor Authentication (MFA) is een verificatieservice waarbij gebruikers zich bij het aanmelden moeten verifiëren via een mobiele app, telefonische oproep of een tekstbericht. De service is beschikbaar voor gebruik met Microsoft Azure Active Directory en kan worden ingezet als service voor bedrijfstoepassingen in de cloud en on-premises. Voor het PAM-scenario biedt Azure MFA een extra verificatie methode. Azure MFA kan worden gebruikt voor autorisatie, ongeacht hoe een gebruiker wordt geverifieerd op het Windows PRIV-domein.

> [!NOTE]
> De PAM-benadering met een bastion-omgeving van MIM is bedoeld om te worden gebruikt in een aangepaste architectuur voor geïsoleerde omgevingen waar geen toegang tot internet beschikbaar is, waarbij deze configuratie vereist is voor de voor Schriften of in een zeer veel impact van geïsoleerde omgevingen zoals offline onderzoek laboratoria en niet-verbonden operationele technologie of omgevingen voor gegevens verzameling.  Omdat Azure MFA een Internet service is, wordt deze richt lijn uitsluitend verstrekt voor bestaande MIM PAM-klanten of in omgevingen waarin deze configuratie is vereist voor de regelgeving. Als uw Active Directory deel uitmaakt van een omgeving met Internet verbinding, raadpleegt u [privileged Access beveiligen](/security/compass/overview) voor meer informatie over waar u moet beginnen.

## <a name="prerequisites"></a>Vereisten

Als u Azure MFA met MIM PAM wilt gebruiken, hebt u het volgende nodig:

- Internettoegang voor elke MIM-service waarmee PAM wordt geleverd voor de communicatie met Azure MFA
- Een Azure-abonnement
- Azure MFA
- Azure Active Directory Premium licenties voor kandidaten gebruikers
- Telefoonnummers voor alle kandidaatgebruikers

## <a name="downloading-the-azure-mfa-service-credentials"></a>De referenties voor de Azure MFA-service downloaden

Voorheen downloadt u een ZIP-bestand van de Azure MFA SDK die het verificatie materiaal voor MIM heeft opgenomen om contact op te nemen met Azure MFA. Als de Azure MFA SDK echter niet meer beschikbaar is, raadpleegt [u de Azure MFA-server gebruiken in pam of SSPR](../working-with-mfaserver-for-mim.md) voor informatie over het gebruik van de Azure MFA-server.


## <a name="configuring-the-mim-service-for-azure-mfa"></a>De MIM-service voor Azure MFA configureren

1.  Meld u op de computer waarop de MIM-service is geïnstalleerd, aan als een beheerder of als de gebruiker die MIM heeft geïnstalleerd.

2.  Maak een nieuwe map in de map waar de MIM-service is geïnstalleerd, bijvoorbeeld ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Navigeer in Windows Verkenner naar de ```pf\certs``` map van het zip-bestand dat u in de vorige sectie hebt gedownload. Kopieer het bestand ```cert\_key.p12``` naar de nieuwe map.

4.  Navigeer in Windows Verkenner naar de ```pf``` map van de zip en open het bestand ```pf\_auth.cs``` in een tekst editor zoals Klad blok.

5. Deze drie para meters zoeken: ```LICENSE\_KEY``` , ```GROUP\_KEY``` , ```CERT\_PASSWORD``` .

![Waarden kopiëren uit het bestand pf\_auth.cs - schermafbeelding](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Open in Kladblok het bestand **MfaSettings.xml** dat zich in ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service``` bevindt.

7. Kopieer de waarden van de parameters LICENSE\_KEY, GROUP\_KEY en CERT\_PASSWORD in het bestand pf\_auth.cs file naar de overeenkomstige XML-elementen in het bestand MfaSettings.xml.

8. Geef in het **<CertFilePath>** XML-element de volledige padnaam op van het certificaat \_ sleutel. P12-bestand dat eerder is geëxtraheerd.

9. Voer in het **<username>** element een gebruikers naam in.

10. Voer in het **<DefaultCountryCode>** element de land code in voor het kiezen van uw gebruikers, zoals 1 voor de Verenigde Staten en Canada. Deze waarde wordt gebruikt als gebruikers zijn geregistreerd met telefoonnummers zonder landcode. Als het telefoonnummer van de gebruiker een internationale landcode heeft die verschilt van de landcode die is geconfigureerd voor de organisatie, moet die landcode worden opgenomen in het telefoonnummer dat wordt geregistreerd.

11. Sla het bestand **MfaSettings.xml** in de map ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service``` van de MIM-service op om het bestand te overschrijven.

> [!NOTE]
> Controleer aan het einde van het proces of het bestand **MfaSettings.xml** of kopieën hiervan, of het ZIP-bestand niet openbaar leesbaar zijn.

## <a name="configure-pam-users-for-azure-mfa"></a>PAM-gebruikers configureren voor Azure MFA

Als een gebruiker een rol wilt activeren waarvoor Azure MFA is vereist, moet het telefoonnummer van de gebruiker zijn opgeslagen in MIM. Dit kenmerk kan op twee manieren worden ingesteld.

Ten eerste kan met de opdracht `New-PAMUser` een telefoonnummerkenmerk in de adreslijstvermelding voor de gebruiker in het CORP-domein naar de database van de MIM-service worden gekopieerd. Dit is een eenmalige bewerking.

Daarnaast kan met de opdracht `Set-PAMUser` het telefoonnummerkenmerk worden bijgewerkt in de database van de MIM-service. Zo wordt met de volgende opdracht het telefoonnummer van een bestaande PAM-gebruiker vervangen in de MIM-service. De adreslijstvermelding blijft ongewijzigd.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>PAM-rollen configureren voor Azure MFA

Nadat de telefoonnummers van alle kandidaatgebruikers voor een PAM-rol zijn opgeslagen in de database van de MIM-service, kan de rol worden geconfigureerd voor het vereisen van Azure MFA. Hiervoor kunt u de opdracht `New-PAMRole` of `Set-PAMRole` gebruiken. Bijvoorbeeld:

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

U kunt Azure MFA uitschakelen voor een rol door de parameter -MFAEnabled 0 op te geven in de opdracht `Set-PAMRole`.

## <a name="troubleshooting"></a>Problemen oplossen

De volgende gebeurtenissen vindt u in het gebeurtenislogboek voor Privileged Access Management:

| Id  | Severity | Gegenereerd door | Beschrijving |
|-----|----------|--------------|-------------|
| 101 | Fout       | MIM-service            | De gebruiker heeft Azure MFA niet voltooid (heeft bijvoorbeeld de telefoonoproep niet beantwoord) |
| 103 | Informatie | MIM-service            | De gebruiker heeft Azure MFA voltooid tijdens de activering                       |
| 825 | Waarschuwing     | Service voor PAM-controle | Het telefoonnummer is gewijzigd                                |

## <a name="next-steps"></a>Volgende stappen

- [Wat is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
