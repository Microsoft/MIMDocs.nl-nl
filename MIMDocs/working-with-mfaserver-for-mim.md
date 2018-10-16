---
title: SDK voor Azure multi-factor Authentication-Server gebruiken om PAM of scenario's voor self-service voor Wachtwoordherstel te activeren | Microsoft Docs
description: Instellen van de SDK van Azure multi-factor Authentication-Server als een tweede beveiligingslaag wanneer uw gebruikers rollen in Privileged Access Management en selfservice voor wachtwoordherstel activeren.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 868e5c543fb55257f7de903eade69529d90ec895
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334360"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Azure multi-factor Authentication-Server gebruiken om PAM of self-service voor Wachtwoordherstel te activeren
Het volgende document wordt beschreven hoe u de Azure MFA-server instellen als een tweede beveiligingslaag wanneer uw gebruikers rollen in de bevoegdheid Access Management of selfservice voor wachtwoordherstel activeren.

> [!IMPORTANT]
> Vanwege de aankondiging van de afschaffing van Azure multi-factor Authentication Software Development Kit. De Azure MFA-SDK wordt ondersteund voor bestaande klanten tot de vervaldatum van 14 November 2018. Nieuwe klanten en huidige klanten zich niet kunnen downloaden van de SDK niet meer via de klassieke Azure portal. Als u wilt downloaden dat u moet contact opnemen met ondersteuning voor Azure-klant voor het ontvangen van de gegenereerde Servicereferenties voor de MFA-pakket. <br> Het Microsoft-ontwikkelteam werkt op wijzigingen in MFA door te integreren met de SDK van Azure multi-factor Authentication-Server.

Het onderstaande artikel wordt de configuratie-update en de stappen om in te schakelen voor een eenvoudige switch een overzicht van Azure MFA-SDK voor Azure multi-factor Authentication-Server SDK wanneer zoals dit zal worden opgenomen in een toekomstige hotfix uitgebracht Zie [versiegeschiedenis ](/reference/version-history.md) voor meldingen. 

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken de Azure multi-factor Authentication-Server met MIM, hebt u het volgende nodig:

- Toegang tot Internet vanaf elke MIM-Service of de MFA-Server bieden PAM en SSPR, neem contact op met de Azure MFA-service
- Een Azure-abonnement
- Installatie maakt al gebruik van Azure MFA-SDK
- Azure Active Directory Premium-licenties voor kandidaatgebruikers of een alternatieve methode voor Azure MFA-licentieverlening
- Telefoonnummers voor alle kandidaatgebruikers
- MIM-hotfix 4.5. of hoger Zie [versiegeschiedenis](/reference/version-history.md) voor aankondigingen

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuratie van de Azure multi-factor Authentication-Server 
> [!NOTE] 
> In de configuratie moet u een geldig SSL-certificaat voor de SDK is geïnstalleerd. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Stap 1: Azure multi-factor Authentication-Server downloaden via de azure portal 
Aanmelden bij de [Azure-portal](https://portal.azure.com/) en de Azure MFA-server te downloaden.
![werkmap-met-mfaserver-voor-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Stap 2: Activeringsreferenties genereren
Gebruik de **activeringsreferenties om gebruik te starten genereren** koppeling naar de activeringsreferenties genereren. Eenmaal gegenereerd opslaan voor later gebruik.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Stap 3: De Azure multi-factor Authentication-Server installeren
Nadat u de server hebt gedownload [installeren](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) deze.  Uw activeringsreferenties is vereist. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Stap 4: Uw IIS-webtoepassing die als voor de SDK host maken
1. Open IIS-beheer ![werken-met-mfaserver-voor-mim_iis. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Nieuwe Website aanroep 'MIM MFASDK' maken, koppelen aan een lege map ![werken-met-mfaserver-voor-mim_sdkweb. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Multi-factor Authentication-Console openen en klik op de Web Service SDK ![werken-met-mfaserver-voor-mim_sdkinstall. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Zodra de wizards is klik door config, selecteer "MIM MFASDK' en app-pool

> [!NOTE] 
> Wizard moet een beheerdersgroep moet worden gemaakt. Meer informatie vindt u in de Azure >> MFA Azure multi-factor Authentication-Server-documentatie.

5. Vervolgens moeten we voor het importeren van de MIM-Service-account openen multi-factor Authentication-Console selecteren 'Gebruikers' een. Klik op 'Importeren uit Active Directory' b. Ga naar serviceaccount, ook wel 'contoso\mimservice' c. Klik op "Importeren" en "Afsluiten" ![werken-met-mfaserver-voor-mim_importmimserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Bewerk het account van de MIM-Service inschakelen in multi-factor Authentication-Console voor ![werken-met-mfaserver-voor-mim_enableserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Werk de IIS-verificatie op de website 'MIM MFASDK'. Eerst wordt uitgeschakeld. de 'Anonieme verificatie' vervolgens naar het Windows-verificatie inschakelen' ![werken-met-mfaserver-voor-mim_iisconfig. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Laatste stap: De MIM-serviceaccount toevoegen aan de 'PhoneFactor Admins' ![werken-met-mfaserver-voor-mim_addservicetomfaadmin. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>De MIM-Service voor Azure multi-factor Authentication-Server configureren 

### <a name="step-1-patch-server-to-452020"></a>Stap 1: Patch Server 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Stap 2: Back-up en Open de MfaSettings.xml zich in de 'C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Stap 3: De volgende regels bijwerken
1. De volgende regels van de configuratie-items verwijderen/wissen <br>
&LT; LICENTIECODE &GT;&LT; / LICENTIECODE &GT;<br>
&LT; GROUP_KEY &GT;&LT; / GROUP_KEY &GT;<br>
&LT; CERT_PASSWORD &GT;&LT; / CERT_PASSWORD &GT;<br>
<CertFilePath></CertFilePath><br>

2. Bijwerken of de volgende regels toevoegen aan de volgende MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. MIM-Service opnieuw starten en test-functionaliteit met Azure multi-factor Authentication-Server.

> [!NOTE] 
> Als u wilt terugkeren instelling MfaSettings.xml vervangen door uw back-upbestand in stap 2


## <a name="next-steps"></a>Volgende stappen

-    [Aan de slag met de Azure multi-factor Authentication-Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Wat is Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Aangepaste multi-factor Authentication-API gebruiken voor PAM of SSPR activeren](Working-with-custommfaserver-for-mim.md)
- [Versiegeschiedenis van MIM](./reference/version-history.md)