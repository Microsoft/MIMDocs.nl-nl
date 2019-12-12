---
title: Conversie van specifieke MIM-Services naar gMSA | Microsoft Docs
description: Onderwerp met een beschrijving van de basis stappen voor het configureren van gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "64520552"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversie van specifieke MIM-Services naar gMSA

In deze hand leiding worden de basis stappen beschreven voor het configureren van gMSA voor ondersteunde services. Het proces dat u wilt converteren naar gMSA is eenvoudig nadat u uw omgeving vooraf hebt geconfigureerd.

Vereiste hotfix: \<koppeling naar de meest recente KB-\>

Ondersteund:

-   MIM-synchronisatie service (FIMSynchronizationService)
-   MIM-service (FIMService)
-   MIM-wachtwoord registratie
-   MIM-wacht woord opnieuw instellen
-   PAM-bewakings service (PamMonitoringService)
-   PAM component-service (PrivilegeManagementComponentService)

Niet ondersteund:

-   De MIM-portal wordt niet ondersteund. deze maakt deel uit van de share point-omgeving en u moet in de farm modus implementeren en [Automatische wachtwoord wijziging configureren in SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Alle beheer agenten
-   Micro soft certificaat beheer
-   BHOLD

<a name="general-information"></a>Algemene informatie 
--------------------

Lezen vereist om de installatie te volt ooien en inzicht te krijgen in

-   [Overzicht van door groepen beheerde service accounts](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Eerste stap op uw Windows-domein controller

1.  Maak, indien nodig, de hoofd sleutel distributie Services (KDS) (slechts eenmaal per domein). De basis sleutel wordt door de KDS-service op Dc's (samen met andere informatie) gebruikt voor het genereren van wacht woorden.

    -   Add-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" houdt in dat \~tien uur per seconde moet worden gerepliceerd naar alle domein controllers. Dit was ongeveer 1 uur voor twee domein controllers.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Synchronisatieservice
-----------------------

1.  Maak een groep met de naam MIMSync_Servers en voeg alle synchronisatie servers toe aan deze groep.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Vanuit Windows Power shell, voert u de onderstaande opdracht uit als domein beheerder met computer account dat al is toegevoegd aan het domein

    -   New-ADServiceAccount-name MIMSyncGMSAsvc-DNSHostName MIMSyncGMSAsvc.contoso.com-PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Details van de GSMA voor synchronisatie ophalen:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Als u de PCNS-service uitvoert, moet u de overdracht bijwerken

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc-ServicePrincipalNames \@{add = "PCNSCLNT/mimsync. contoso. com"}

3. Bij de synchronisatie Services moet u een back-up maken van de versleutelings sleutel, zoals deze wordt gevraagd wanneer de modus wijzigen is geïnstalleerd

    -   Op de server waarop de synchronisatie service is geïnstalleerd, zoekt u het hulp programma voor sleutel beheer van synchronisatie service

    -   Standaard is de **sleutel set exporteren** al geselecteerd

    -   Klik op **volgende**

    -   U wordt nu gevraagd om de bestaande synchronisatie-account gegevens in te voeren

    -   De gegevens van het FIM Sync-account invoeren en verifiëren

        -   Account naam-account naam van het synchronisatie service account dat wordt gebruikt tijdens de eerste installatie

        -   Wacht woord-wacht woord van synchronisatie service account

        -   Domein-domein waarvan de synchronisatie service account is opsplitst

    -   Klik op **volgende**

    -   Als u iets onjuist hebt ingevoerd, wordt de volgende fout weer gegeven:

    -   Nu u de account gegevens hebt ingevoerd, krijgt u een optie om de bestemming (locatie van het export bestand) van de back-versleutelings sleutel te wijzigen

        -   De locatie van het export bestand is standaard **C:\\Windows\\system32**\\miiskeys-1. bin.

4. Installeer Microsoft Identity Manager SP1-4.4.1302.0 voor de synchronisatie service. u kunt dit vinden op het Download centrum voor volume licenties of via de MSDN-download site. Nadat u de installatie hebt voltooid, moet u de sleutel miiskeys. bin opslaan.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installeer de meest recente [hotfix 4.5. x. x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) of hoger.

- Zodra patched is de FIM-synchronisatie service gestopt.
- Program Ma's en onderdelen van het configuratie scherm Microsoft Identity Manager
- Synchronisatie service wijzigen-\> volgende\> configureren-\> volgende

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  De account naam wissen
-  Typ de naam van het service account **MIMSyncGMSA** met \$-symbool, zoals op de
- Afdruk. Wacht woord leeg laten.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Volgende\> installatie\>
- Herstel de sleutelset uit het. bin-bestand dat is opgeslagen.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Het account dat is toegevoegd aan de SQL-machtiging is voor aanmelding gemaakt. Daarom moet u toestaan dat de gebruiker de machtiging voor wijzigen van de modus voor het toevoegen van accounts en dbo op de synchronisatie service database toestaat.

## <a name="mim-service"></a>MIM-service
-----------

>[!IMPORTANT]
>Het volgende proces moet worden gebruikt wanneer de aan de MIM-service gerelateerde accounts eerst worden geconverteerd naar gMSA-accounts. De Power shell-cmdlets die in de bijlage worden vermeld, kunnen alleen worden gebruikt om de account gegevens te wijzigen zodra de initiële configuratie is uitgevoerd. *

1.  Groeps-beheerde accounts maken voor de MIM-service, PAM rest API, PAM-bewakings service, PAM component-service, SSPR registratie Portal, SSPR reset Portal.

    -   Zorg ervoor dat u gMSA-overdracht en SPN bijwerkt
        -   Set-ADServiceAccount-Identity \<account\>-ServicePrincipalNames \@{add = "\<SPN-\>"}
        -   Overdracht
            -   Set-ADServiceAccount-Identity \<gsmaaccount\>-TrustedForDelegation \$True
        -   Beperkte overdracht
            -   \$delspns = ' http/Mim ', ' http/mim. contoso. com '
            -   New-ADServiceAccount-name \<gsmaaccount\>-DNSHostName \<gsmaaccount\>. contoso.com-PrincipalsAllowedToRetrieveManagedPassword \<groep\>-ServicePrincipalNames \$spn's-OtherAttributes \@{' msDS-AllowedToDelegateTo ' =\$delspns}

2.  Voeg account toe voor de MIM-service in synchronisatie groepen. Het is vereist voor SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **Opmerking**.  Bekend probleem dat services die gebruikmaken van beheerde accounts, vastlopen na het opnieuw opstarten van de server omdat de micro soft Key Distribution-service niet is gestart na het opnieuw opstarten van de Windows. De service kan niet worden gestart en Windows kan niet opnieuw worden gestart. Het probleem is ten minste reproduceerbaar op Windows Server 2012 R2. Tijdelijke oplossing voor dit probleem is de opdracht uitvoeren 

-   **SC triggerinfo kdssvc start/networkon**

    Als u de micro soft Key Distribution-service wilt starten wanneer het netwerk aan is (doorgaans vroegtijdig in de opstart cyclus).

    Zie de bespreking over soortgelijk probleem: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Voer een verhoogde MSI uit van de MIM-service en selecteer wijzigen.

5.  Schakel het selectie vakje ander account gebruiken voor Exchange (voor beheerde accounts) in op de pagina hoofd server verbinding configureren. Hier hebt u een optie om het oude account te gebruiken dat een postvak heeft of een Cloud postvak gebruikt.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Op het pagina type service account voor MIM-Services configureren met \$ symbool aan het einde. Typ ook wacht woord voor service-e-mail account. Het wacht woord voor het service account moet worden uitgeschakeld.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Omdat de functie LogonUser niet werkt voor beheerde accounts, wordt de volgende pagina waarschuwing weer gegeven: Controleer of het service account is beveiligd in de huidige configuratie.

![Cid: image007. png\@01D36EB 7.562 E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Typ op de pagina Privileged Access Management REST API configureren de naam van het groeps account van de toepassing met \$ symbool aan het einde en laat het veld wacht woord leeg.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Op het pagina type service accountnaam van de PAM-component configureren met \$ symbool aan het einde en wacht woord leeg laten.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![Cid: image010. png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Schakel op het pagina type service accountnaam van de Privileged Access Management monitoring-service configureren met \$ symbool aan het einde in en laat het veld wacht woord leeg.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Op de pagina Type portal voor wachtwoord registratie configureren met \$ symbool aan het einde en laat het veld wacht woord leeg.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Op de pagina Type account voor MIM-wacht woord opnieuw instellen configureren met \$ symbool aan het einde en wacht woord leeg laten.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Voltooi de installatie.

Opmerking:

-  Na de installatie worden twee nieuwe sleutels in het REGI ster gemaakt op basis van het pad
    - "HKEY_LOCAL_MACHINE\\-SOFTWARE\\micro soft\\Forefront Identity
    - Manager\\2010\\-service ' voor het opslaan van een versleuteld Exchange-wacht woord. Een voor
    - Exchange Online en een andere voor Exchange on-premises (een van beide moet
    - leeg).

![Cid: image014. jpg\@01D36F 53.303 D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Om het wacht woord bij te werken, hebben we [hier een down load](microsoft-identity-manager-2016-gmsascript.md) voor het script ontvangen, zodat de gebruiker de wijzigings modus niet hoeft uit te voeren

- Exchange-wacht woord versleutelen het installatie programma maakt aanvullende service en
    - wordt uitgevoerd onder het beheerde account. De volgende berichten worden toegevoegd in
    - Toepassings gebeurtenis logboek tijdens de installatie.

![Cid: image016. jpg\@01D36F 53.303 D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
