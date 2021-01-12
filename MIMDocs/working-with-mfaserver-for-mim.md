---
title: Azure Multi-Factor Authentication-server gebruiken om PAM-of SSPR-Scenario's te activeren | Microsoft Docs
description: Stel Azure Multi-Factor Authentication-server in als een tweede beveiligingslaag wanneer uw gebruikers rollen activeren in Privileged Access Management en self-service voor het opnieuw instellen van wacht woorden.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 1/5/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 68f8ae0f64e136add96b3abb5083331912a0efe7
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104102"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Azure Multi-Factor Authentication-server gebruiken om PAM-of SSPR te activeren
In het volgende document wordt beschreven hoe u de Azure MFA-server instelt als een tweede beveiligingslaag wanneer uw gebruikers rollen activeren in Privileged Access Management of Self-Service wacht woord opnieuw instellen.

> [!IMPORTANT]
> Als gevolg van de afschaffing van de Software Development Kit (SDK) van Azure Multi-Factor Authentication (MFA), kunnen klanten de Azure MFA SDK niet meer downloaden.

In het onderstaande artikel vindt u een overzicht van de configuratie-updates en de stappen voor het verplaatsen van de Azure MFA-SDK naar de Azure MFA-server.

## <a name="prerequisites"></a>Vereisten

Als u Azure Multi-Factor Authentication-server met MIM wilt gebruiken, hebt u het volgende nodig:

- Internet toegang van elke MIM-service of MFA-server die PAM-en SSPR biedt om contact op te nemen met de Azure MFA-service
- Een Azure-abonnement
- De Azure MFA-SDK wordt al gebruikt voor de installatie
- Azure Active Directory Premium licenties voor kandidaten gebruikers
- Telefoonnummers voor alle kandidaatgebruikers
- MIM-hotfix 4,5. of hoger Zie de [versie geschiedenis](./reference/version-history.md) voor aankondigingen

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuratie van Azure Multi-Factor Authentication-server 
> [!NOTE] 
> In de configuratie hebt u een geldig SSL-certificaat nodig dat is geïnstalleerd voor de SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Stap 1: down load Azure Multi-Factor Authentication-server van de Azure Portal 
Meld u aan bij het [Azure Portal](https://portal.azure.com/) en down load de Azure MFA-server.
![werken-met-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Stap 2: activerings referenties genereren
Gebruik de koppeling **activerings referenties genereren voor het initiëren** van het maken van activerings referenties. Sla het bestand op voor later gebruik zodra het is gegenereerd.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Stap 3: de Azure-Multi-Factor Authentication-server installeren
Nadat u de server hebt gedownload, [installeert](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) u deze.  U moet referenties voor activering opgeven. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Stap 4: de IIS-webtoepassing maken die als host fungeert voor de SDK
1. working-with-mfaserver-for-mim_iis.PNGvoor IIS-beheer openen ![](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Nieuwe website-aanroep ' MIM MFASDK ' maken, deze koppelen aan een lege directory ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Open Multi-Factor Authentication-console en klik op Web Service SDK ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Zodra de wizards zijn ingeklikt, klikt u op configuratie en selecteert u ' MIM MFASDK ' en de groep van toepassingen

> [!NOTE] 
> De wizard vereist dat er een beheerders groep wordt gemaakt. Meer informatie vindt u op de Azure > > MFA Azure Multi-Factor Authentication-server-documentatie.

5. Vervolgens moet u het MIM-service account importeren open Multi-Factor Authentication-console Selecteer ' gebruikers ' a. Klik op importeren uit Active Directory b. Navigeer naar het service account, zoals "contoso\mimservice" c. Klik op importeren en sluiten ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Bewerk het MIM-service account om in te scha kelen in Multi-Factor Authentication-console ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Werk de IIS-verificatie op de website ' MIM MFASDK ' bij. Eerst wordt de optie Anonieme verificatie uitgeschakeld en vervolgens Windows-verificatie inschakelen ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Laatste stap: Voeg het MIM-service account toe aan deworking-with-mfaserver-for-mim_addservicetomfaadmin.PNG' Phone factor admins ' ![](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>De MIM-service voor Azure Multi-Factor Authentication-server configureren 

### <a name="step-1-patch-server-to-452020"></a>Stap 1: patch server to 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Stap 2: back-up maken en de MfaSettings.xml in de map C:\Program Files\Microsoft Forefront Identity Manager\2010\Service openen

### <a name="step-3-update-the-following-lines"></a>Stap 3: de volgende regels bijwerken
1. De volgende regels voor configuratie vermeldingen verwijderen/wissen <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Update of Voeg de volgende regels toe aan het volgende om MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. De MIM-service opnieuw starten en de functionaliteit testen met Azure Multi-Factor Authentication-server.

> [!NOTE] 
> Als u de instelling wilt herstellen, vervangt u MfaSettings.xml door het back-upbestand in stap 2


## <a name="see-also"></a>Zie tevens

-    [Aan de slag met de Azure Multi-Factor Authentication-server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Wat is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Aangepaste Multi-Factor Authentication-API gebruiken om PAM-of SSPR te activeren](Working-with-custommfaserver-for-mim.md)
- [Release geschiedenis van MIM-versie](./reference/version-history.md)
