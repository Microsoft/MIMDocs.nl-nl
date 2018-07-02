---
title: Conversie van specifieke MIM-Services naar gMSA | Microsoft Docs
description: Onderwerp beschrijven de basisstappen voor het configureren van gMSA.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: 090e82cac6c734beb9767e2d2e6230320e44c26f
ms.sourcegitcommit: c6cb2556bb9f2256b959a3c95db7ca5bbfc2b437
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37065139"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversie van specifieke MIM-Services naar gMSA

Deze handleiding wordt stapsgewijs door de basisstappen voor het configureren van gMSA voor ondersteunde services. Het proces om te converteren naar gMSA is eenvoudig nadat u uw omgeving voor het vooraf configureren.

De hotfix is vereist: \<koppeling naar de meest recente KB\>

Ondersteund:

-   MIM-synchronisatieserviceaccount service(FIMSynchronizationService)
-   MIM Service(FIMService)
-   MIM-Wachtwoordregistratie
-   MIM-wachtwoord opnieuw instellen
-   PAM-bewaking Service(PamMonitoringService)
-   PAM Component-Service (PrivilegeManagementComponentService)

Niet ondersteund:

-   MIM-Portal wordt niet ondersteund. het deel uitmaakt van de sharepoint-omgeving en u moet implementeren in de farmmodus en [automatische wachtwoordwijziging in SharePointServer configureren](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Alle Beheeragents
-   Microsoft Certificaatbeheer
-   BHOLD

<a name="general-information"></a>Algemene gegevens 
--------------------

Lezen die nodig zijn voor installatie is voltooid en begrijpen

-   [Groep overzicht beheerde serviceaccounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/en-us/library/jj128430(v=ws.11).aspx>

Eerste stap om op uw windows-domeincontroller

1.  Maak de distributie Services(KDS) Root Key (slechts eenmaal per domein), indien nodig. Hoofdsleutel wordt gebruikt door de KDS-service op DC's (samen met andere informatie) voor het genereren van wachtwoorden.

    -   -KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately ' betekent dat wacht tot \~10 uur / wilt repliceren naar alle DC. Dit is ongeveer een uur voor twee domeincontrollers.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronisatieservice
-----------------------

1.  Eerste stap maakt een aanroep van de groep 'MIMSync_Servers' en alle synchronisatie-servers toevoegen aan deze groep.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Via windows PowerShell, vervolgens wordt uitgevoerd onder een opdracht als domeinbeheerder met computeraccount al toegevoegd aan het domein

    -   Nieuwe-ADServiceAccount-naam MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword 'MIMSync_Servers'

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Details van de GSMA ophalen voor synchronisatie:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Als PCNS-Service wordt uitgevoerd, moet u de overdracht bijwerken

    -   Set-ADServiceAccount-identiteit MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Volgende bij de synchronisatie moet services u back-up van de versleutelingssleutel zoals deze wordt aangevraagd bij de installatie in de modus wijzigen

    -   Zoek op de Server die de synchronisatieservice is geïnstalleerd op de synchronisatie Service Key Management-hulpprogramma

    -   Standaard de ** Export sleutel set ** is al geselecteerd

    -   Klik op **volgende**

    -   U wordt nu gevraagd om in te voeren van de bestaande gegevens voor de synchronisatie-account

    -   Geef en controleer of de FIM-Sync-accountgegevens

        -   Accountnaam - accountnaam van de synchronisatie-serviceaccount die wordt gebruikt tijdens de eerste installatie

        -   Wachtwoord - wachtwoord van serviceaccount voor synchronisatie

        -   Domein - domein dat het serviceaccount voor synchronisatie van elkaar

    -   Klik op **volgende**

    -   Als u iets niet juist ingevoerd, ontvangt u de volgende fout

    -   Nu u de accountgegevens correct hebt ingevoerd, u krijgt een optie voor de bestemming (exporteren bestandslocatie) van de back-versleutelingssleutel wijzigen

        -   De locatie van de uitvoer is standaard **C:\\Windows\\system32**\\miiskeys 1.bin.

4. Installeer Microsoft Identity Manager SP1-synchronisatieservice build 4.4.1302.0. u kunt u vinden op het Volume licentie Download Center of MSDN-website downloaden. Nadat u de installatie voltooid Zorg ervoor dat, u sleutelset miiskeys.bin opslaan.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installeer de meest recente [hotfix 4.5.x.x](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history) of hoger.

- Eenmaal een patch uitgevoerd stoppen FIM-synchronisatieservice.
- Besturingselement van het Configuratiescherm en functies Microsoft Identity Manager
- Synchronisatieservice wijzigen -\> volgende -\> configureren -\> volgende

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  De accountnaam wissen
-  Type serviceaccountnaam **MIMSyncGMSA** met \$ symbool zoals op de
- schermopname. Wachtwoord leeg laten.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Volgende -\> volgende -\> installeren
- Sleutelset terugzetten vanuit het .bin-bestand opgeslagen.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> SQL-machtiging toegevoegd wordt het account is gemaakt voor aanmelding dus u moet de gebruiker toestaan modus wijzigingsmachtiging account en dbo toevoegen op de database van de synchronisatie-service toepassen

## <a name="mim-service"></a>MIM-service
-----------

>[!IMPORTANT]
>Het volgende proces moet worden gebruikt bij het converteren van de MIM-Service eerst verwante accounts worden gMSA-accounts. De PowerShell-cmdlets in de bijlage hebt genoteerd kan alleen worden gebruikt om te wijzigen van de accountgegevens als de eerste configuratie is done.*

1.  Maken van de groep beheerde Accounts voor MIM-Service, PAM Rest API, PAM-controleservice, PAM Component-Service, SSPR-Registratieportal, SSPR-Portal opnieuw instellen.

    -   Zorg ervoor dat u een gMSA overdracht en SPN bijwerken
        -   Set-ADServiceAccount-identiteit \<account\> - ServicePrincipalNames \@{toevoegen = "\<SPN\>"}
        -   Overdracht
            -   Set-ADServiceAccount-identiteit \<gsmaaccount\> - TrustedForDelegation \$true
        -   Beperkte overdracht
            -   \$delspns = ' http/mim', 'http/mim.contoso.com'
            -   Nieuwe-ADServiceAccount-naam \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<groep\> - ServicePrincipalNames \$SPN's - OtherAttributes \@{'msDS-AllowedToDelegateTo' =\$delspns}

2.  Account voor MIM-Service in synchronisatiegroepen toevoegen. Het is nodig voor SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **OPMERKING**.  Bekend probleem waarbij de services die gebruikmaken van beheerde account vastlopen na het opnieuw opstarten server omdat de Microsoft Key Distribution-Service is niet gestart nadat Windows opnieuw is opgestart. Service kan niet worden gestart en kan Windows niet te worden gestart. Het probleem is ten minste reproduceerbare op Windows Server 2012 R2. Oplossing voor dit probleem wordt opdracht uitgevoerd 

-   **SC triggerinfo kdssvc start/networkon**

    de Microsoft Key Distribution-Service wordt gestart wanneer het netwerk ingeschakeld is (meestal vroeg in de opstartcyclus).

    Zie discussie over soortgelijk probleem: <https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Uitvoeren met verhoogde bevoegdheid MSI van MIM-Service en selecteer wijzigen.

5.  Schakel selectievakje "Ander account gebruiken voor Exchange (voor beheerde accounts)" op "Pagina configureren hoofdserver verbinding". Hier hebt u een optie voor het gebruik van de oude account met een postvak of cloud postvak gebruiken.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Op de pagina type service-account met 'Configureren MIM-Service-account' \$ symbool aan het einde. Wachtwoord van serviceaccount e-mailadres ook typen. Wachtwoord van serviceaccount moet worden uitgeschakeld.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Als de functie LogonUser voor beheerde accounts niet werkt, worden volgende pagina waarschuwing 'Controleer of als serviceaccount beveiligd in de huidige configuratie is'.

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Typ op de pagina 'Configureren Privileged Access Management REST-API' naam groep van toepassingen Account met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Typ op de pagina 'Configureren PAM Component-Service' serviceaccountnaam met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Typ op de pagina 'Configureren Privileged Access Management Monitoring Service' serviceaccountnaam met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Typ op de pagina 'Configureren MIM-Portal voor Wachtwoordregistratie' accountnaam met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Typ op de pagina configureren van MIM wachtwoord opnieuw instellen-Portal' accountnaam met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Installatie voltooid.

Opmerking:

-  Na de installatie van worden twee nieuwe sleutels gemaakt in het register met het pad
    - ' HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service ' voor het opslaan van versleutelde Exchange-wachtwoord. Één voor
    - Exchange Online en een andere voor Exchange on-premises (een van beide moet
    - leeg).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Voor het bijwerken van het wachtwoord opgegeven we een script [hier downloaden](microsoft-identity-manager-2016-gmsascript.md) zodat de klant heeft geen modus wijzigen uitvoeren

- Voor het versleutelen van de Exchange-wachtwoord het installatieprogramma maakt extra service en
    - uitgevoerd onder het beheerde account. Worden volgende berichten toegevoegd
    - Gebeurtenislogboek van toepassing tijdens de installatie.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
