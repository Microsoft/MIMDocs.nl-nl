---
title: Azure Multi-Factor Authentication-server gebruiken om PAM-of SSPR-Scenario's te activeren | Microsoft Docs
description: Stel Azure Multi-Factor Authentication-server in als een tweede beveiligingslaag wanneer uw gebruikers rollen activeren in Privileged Access Management en self-service voor het opnieuw instellen van wacht woorden.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 39ebec3002f488077cfda28a5780b0c78c19f363
ms.sourcegitcommit: 28a20aaa1f08b428cc1ae0eae43ae47de4d9d22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684088"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Azure Multi-Factor Authentication-server gebruiken om PAM-of SSPR te activeren
In het volgende document wordt beschreven hoe u de Azure MFA-server instelt als een tweede beveiligingslaag wanneer uw gebruikers rollen activeren in Privileged Access Management of selfservice voor het opnieuw instellen van wacht woorden.

> [!IMPORTANT]
> Vanwege de aankondiging van de afschaffing van Azure Multi-Factor Authentication Software Development Kit, wordt de Azure MFA SDK ondersteund voor bestaande klanten tot de datum van uittreding van 14 november 2018. Nieuwe klanten en huidige klanten kunnen SDK niet meer downloaden via de klassieke Azure-Portal. Als u wilt downloaden, moet u contact op met de klanten service van Azure om uw gegenereerde pakket met MFA-service referenties te ontvangen.

In het onderstaande artikel vindt u een overzicht van de configuratie-updates en de stappen voor het verplaatsen van de Azure MFA-SDK naar Azure Multi-Factor Authentication-server.

## <a name="prerequisites"></a>Vereisten

Als u Azure Multi-Factor Authentication-server met MIM wilt gebruiken, hebt u het volgende nodig:

- Internet toegang van elke MIM-service of MFA-server die PAM-en SSPR biedt om contact op te nemen met de Azure MFA-service
- Een Azure-abonnement
- De Azure MFA-SDK wordt al gebruikt voor de installatie
- Azure Active Directory Premium-licenties voor kandidaatgebruikers of een alternatieve methode voor Azure MFA-licentieverlening
- Telefoonnummers voor alle kandidaatgebruikers
- MIM-hotfix 4,5. of hoger Zie de [versie geschiedenis](./reference/version-history.md) voor aankondigingen

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuratie van Azure Multi-Factor Authentication-server 
> [!NOTE] 
> In de configuratie hebt u een geldig SSL-certificaat nodig dat is geïnstalleerd voor de SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Stap 1: down load Azure Multi-Factor Authentication-server vanuit Azure Portal 
Meld u aan bij het [Azure Portal](https://portal.azure.com/) en down load de Azure MFA-server.
![Working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Stap 2: activerings referenties genereren
Gebruik de koppeling **activerings referenties genereren voor het initiëren** van het maken van activerings referenties. Zodra de opslag is gegenereerd voor later gebruik.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Stap 3: de Azure-Multi-Factor Authentication-server installeren
Nadat u de server hebt gedownload, [installeert](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) u deze.  U moet referenties voor activering opgeven. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Stap 4: de IIS-webtoepassing maken die als host fungeert voor de SDK
1. Open IIS-beheer ![Working-with-mfaserver-for-mim_iis. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Maak een nieuwe website ' MIM MFASDK ' en koppel deze aan een lege directory ![Working-with-mfaserver-for-mim_sdkweb. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Open Multi-Factor Authentication-console en klik op Web Service SDK ![werken-met-mfaserver-for-mim_sdkinstall. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Zodra de wizards zijn ingeklikt, klikt u op configuratie en selecteert u ' MIM MFASDK ' en de groep van toepassingen

> [!NOTE] 
> De wizard vereist dat er een beheerders groep wordt gemaakt. Meer informatie vindt u op de Azure > > MFA Azure Multi-Factor Authentication-server-documentatie.

5. Vervolgens moet u het MIM-service account importeren open Multi-Factor Authentication-console Selecteer ' gebruikers ' a. Klik op importeren uit Active Directory b. Navigeer naar Service account ook wel "contoso\mimservice" c. Klik op importeren en sluiten ![werken-met-mfaserver-for-mim_importmimserviceaccount. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Bewerk het MIM-service account om in Multi-Factor Authentication console in te scha kelen ![Working-with-mfaserver-for-mim_enableserviceaccount. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Werk de IIS-verificatie op de website ' MIM MFASDK ' bij. Eerst wordt de ' anonieme verificatie ' uitgeschakeld en vervolgens Windows-verificatie inschakelen ![Working-with-mfaserver-for-mim_iisconfig. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Laatste stap: Voeg het MIM-service account toe aan de Phone factor-beheerders ![Working-with-mfaserver-for-mim_addservicetomfaadmin. PNG-](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>De MIM-service voor Azure Multi-Factor Authentication-server configureren 

### <a name="step-1-patch-server-to-452020"></a>Stap 1: patch server to 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Stap 2: back-up maken en de bestand mfasettings. XML openen die zich bevindt in de map C:\Program Files\Microsoft Forefront Identity Manager\2010\Service

### <a name="step-3-update-the-following-lines"></a>Stap 3: de volgende regels bijwerken
1. De volgende regels voor configuratie vermeldingen verwijderen/wissen <br>
< LICENSE_KEY > </LICENSE_KEY ><br>
< GROUP_KEY > </GROUP_KEY ><br>
< CERT_PASSWORD > </CERT_PASSWORD ><br>
<CertFilePath></CertFilePath><br>

2. Werk de volgende regels bij of voeg ze toe aan bestand mfasettings. XML: <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. De MIM-service opnieuw starten en de functionaliteit testen met Azure Multi-Factor Authentication-server.

> [!NOTE] 
> Als u de instelling wilt herstellen, vervangt u bestand mfasettings. XML door het back-upbestand in stap 2


## <a name="see-also"></a>Zie tevens

-    [Aan de slag met de Azure Multi-Factor Authentication-server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Wat is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Aangepaste Multi-Factor Authentication-API gebruiken om PAM-of SSPR te activeren](Working-with-custommfaserver-for-mim.md)
- [Release geschiedenis van MIM-versie](./reference/version-history.md)
