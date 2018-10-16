---
title: Conversie van specifieke MIM-Services naar gMSA | Microsoft Docs
description: Dit onderwerp wordt de basisstappen voor het configureren van gMSA te beschrijven.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: d719b771e8db514d45b15c21b60460cc821e872e
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333221"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversie van specifieke MIM-Services naar gMSA

Deze handleiding wordt stapsgewijs door de basisstappen voor het configureren van gMSA voor ondersteunde services. Het proces moet worden geconverteerd naar gMSA is eenvoudig uw omgeving vooraf zijn geconfigureerd.

De hotfix is vereist: \<koppeling naar de meest recente KB\>

Ondersteund:

-   MIM-synchronisatieservice service(FIMSynchronizationService)
-   MIM Service(FIMService)
-   MIM-Wachtwoordregistratie
-   MIM-wachtwoord opnieuw instellen
-   PAM-bewaking Service(PamMonitoringService)
-   PAM Component-Service (PrivilegeManagementComponentService)

Niet ondersteund:

-   MIM-Portal wordt niet ondersteund. het deel uitmaakt van de sharepoint-omgeving en moet u implementeren in de farmmodus en [automatisch veranderen van wachtwoorden in SharePointServer configureren](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Alle Beheeragents
-   Microsoft Certificate Management
-   BHOLD

<a name="general-information"></a>Algemene gegevens 
--------------------

Lezen die nodig zijn voor installatie is voltooid en begrijpen

-   [Overzicht van de groep beheerde Service-Accounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Eerste stap op uw windows-domeincontroller

1.  Maak de basissleutel van sleutel distributie Services(KDS) (slechts één keer per domein), indien nodig. Hoofd-sleutel wordt gebruikt door de KDS-service op DC's (samen met andere informatie) voor het genereren van wachtwoorden.

    -   Toevoegen-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately ' betekent dat wacht tot \~10 uur / wilt repliceren naar alle DC. Dit is ongeveer 1 uur voor twee domeincontrollers.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronisatieservice
-----------------------

1.  Maak een groep met de naam 'MIMSync_Servers' en alle synchronisatie-servers toevoegen aan deze groep.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Van windows PowerShell, Voer onderstaande opdracht als domeinadministrator met computeraccount al toegevoegd aan het domein

    -   Nieuwe-ADServiceAccount-naam MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Details van de GSMA ophalen voor synchronisatie:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Als de PCNS-Service uitgevoerd, moet u de overdracht bijwerken

    -   Set-ADServiceAccount-identiteit MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Vervolgens bij de synchronisatie moet services u back-up van de versleutelingssleutel als wordt gevraagd bij installatie in de modus wijzigen

    -   Zoek op de Server waarop de synchronisatieservice is geïnstalleerd op het hulpprogramma voor synchronisatie-Service-Sleutelbeheer

    -   Standaard de ** Export sleutel instellen ** is al geselecteerd

    -   Klik op **volgende**

    -   U wordt nu gevraagd om in te voeren van de bestaande synchronisatie-accountgegevens

    -   Geef en controleer of de FIM-Sync-accountgegevens

        -   Accountnaam - accountnaam van de synchronisatie-serviceaccount gebruikt tijdens de initiële installatie

        -   Wachtwoord - wachtwoord van serviceaccount voor synchronisatie

        -   Domein - domein dat de synchronisatie-serviceaccount gesplitst van is

    -   Klik op **volgende**

    -   Als u iets niet juist ingevoerd, ontvangt u de volgende fout

    -   Nu u de accountgegevens correct hebt ingevoerd, u krijgt een optie voor het wijzigen van de bestemming (bestandslocatie exporteren) van de versleutelingssleutel voor de back-up

        -   De locatie van de export is standaard **C:\\Windows\\system32**\\miiskeys 1.bin.

4. Installeer Microsoft Identity Manager SP1-synchronisatieservice build 4.4.1302.0. u kunt u vinden op het Volume License-downloaden Center of MSDN-Site downloaden. Nadat u de installatie voltooid Zorg ervoor dat, u sleutelset miiskeys.bin opslaan.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installeer de meest recente [hotfix 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) of hoger.

- Zodra een patch uitgevoerd stoppen FIM-synchronisatieservice.
- Besturingselement van het Configuratiescherm en functies Microsoft Identity Manager
- Synchronisatieservice wijzigen -\> volgende -\> configureren -\> volgende

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  De naam van het wissen
-  Naam van het type serviceaccount **MIMSyncGMSA** met \$ symbool zoals op de
- schermopname. Wachtwoord leeg laten.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Volgende -\> volgende -\> installeren
- Sleutelset herstellen vanuit het .bin-bestand opgeslagen.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> SQL-machtiging toegevoegd is-account is gemaakt voor de aanmelding dus u moet de gebruiker toestaan toepassen modus wijzigingsmachtiging-account en dbo toevoegen op de database van de synchronisatie-service

## <a name="mim-service"></a>MIM-service
-----------

>[!IMPORTANT]
>Het volgende proces moet worden gebruikt bij het converteren van de MIM-Service eerst accounts dat moet worden gMSA-accounts. De PowerShell-cmdlets die u hebt genoteerd in de bijlage kan alleen worden gebruikt om de accountgegevens wijzigen nadat de eerste configuratie done.* is

1.  Maken van de groep beheerde Accounts voor MIM-Service, PAM Rest API, PAM-controleservice, PAM Component-Service, SSPR-Registratieportal, SSPR-Portal opnieuw instellen.

    -   Zorg ervoor dat u gMSA-overdracht en SPN bijwerken
        -   Set-ADServiceAccount-identiteit \<account\> - ServicePrincipalNames \@{toevoegen = "\<SPN\>"}
        -   Overdracht
            -   Set-ADServiceAccount-identiteit \<gsmaaccount\> - TrustedForDelegation \$true
        -   Beperkte delegering
            -   \$delspns = ' http/mim', 'http/mim.contoso.com'
            -   Nieuwe-ADServiceAccount-naam \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<groep\> - ServicePrincipalNames \$SPN's - OtherAttributes \@{'msDS-AllowedToDelegateTo' =\$delspns}

2.  Account voor MIM-Service in synchronisatiegroepen toevoegen. Dit is nodig voor self-service voor Wachtwoordherstel.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **HOUD ER REKENING MEE**.  Bekend probleem waarbij de services die gebruikmaken van beheerd account vastlopen na het opnieuw opstarten-server vanwege Microsoft Key Distribution-Service is niet gestart nadat de Windows opnieuw is opgestart. Service kan niet worden gestart en kan Windows niet te worden gestart. Het probleem is ten minste reproduceerbare op Windows Server 2012 R2. Tijdelijke oplossing voor dit probleem wordt opdracht uitgevoerd 

-   **SC triggerinfo kdssvc start/networkon**

    de Microsoft Key Distribution-Service starten wanneer het netwerk is op (meestal vroeg in de opstartcyclus).

    Zie de beschrijving over soortgelijk probleem: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Uitvoeren met verhoogde bevoegdheid MSI-bestand van de MIM-Service en selecteer wijzigen.

5.  Schakel selectievakje 'Ander account gebruiken voor Exchange (voor beheerde accounts)' op 'Hoofdserver-verbindingspagina configureren'. Hier hebt u een optie voor het gebruik van het oude account dat een postvak is of gebruik van cloud postvak.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Op 'Configureren MIM-Service-account' pagina type serviceaccount met \$ symbool aan het einde. Wachtwoord van serviceaccount e-mailadres ook typen. Wachtwoord van serviceaccount moet worden uitgeschakeld.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Als de functie LogonUser voor beheerde accounts niet werkt, worden volgende pagina waarschuwing "Controleer of Service-Account beveiligd in de huidige configuratie is".

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Typ op de pagina 'Configureren van Privileged Access Management REST API', naam groep van toepassingen rekening met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Typ op de pagina 'Configureren van PAM Component-Service' naam van het serviceaccount met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Typ op de pagina 'Configureren Privileged Access Management Monitoring Service' naam van het serviceaccount met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Typ op de pagina "Configureren van MIM-Portal voor Wachtwoordregistratie" accountnaam met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Typ op de pagina configureren van MIM wachtwoord opnieuw instellen-Portal' accountnaam met \$ symbool aan het einde en wachtwoordveld leeg laten.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Installatie voltooid.

Opmerking:

-  Na de installatie worden twee nieuwe sleutels gemaakt in het register door pad
    - "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service ' voor het opslaan van versleutelde Exchange-wachtwoord. Een voor
    - Exchange Online en andere voor Exchange on-premises (een van deze moet
    - leeg).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Voor het bijwerken van het wachtwoord, zijn er een script beschikbaar [download hier](microsoft-identity-manager-2016-gmsascript.md) , zodat klanten geen om uit te voeren modus wijzigen

- Voor het versleutelen van de Exchange-wachtwoord het installatieprogramma maakt extra service en
    - uitgevoerd onder het beheerde account. Worden volgende berichten toegevoegd
    - Gebeurtenislogboek van toepassing tijdens de installatie.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
