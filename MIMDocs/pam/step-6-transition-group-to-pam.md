---
title: PAM implementeren - Stap 6 - Groep verplaatsen | Microsoft Docs
description: Een groep migreren naar het PRIV-forest, zodat de groep kan worden beheerd met Privilege Access Management.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: aeffca2c4e5467ec039c2077a88f36a652493e90
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---

# Stap 6 – Een groep overzetten naar Privileged Access Management
<a id="step-6--transition-a-group-to-privileged-access-management" class="xliff"></a>

>[!div class="step-by-step"]
[«Stap 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[stap 7 »](step-7-elevate-user-access.md)

U moet PowerShell-cmdlets gebruiken om bevoorrechte accounts in het PRIV-forest te maken. Met deze cmdlets worden de volgende functies uitgevoerd:

- Maak een nieuwe groep in het PRIV-forest met de dezelfde beveiligings-id (SID) als een groep in het CORP-forest.  
- Maak een object in de database van de MIM-service die overeenkomt met de groep in het PRIV-forest.  
- Maak voor elk gebruikersaccount twee objecten in de database van de MIM-service die overeenkomen met de gebruiker in het CORP-forest en het nieuwe gebruikersaccount in het PRIV-forest.  
- Maak een PAM-functieobject in de database van de MIM-service.  

De cmdlets moet eenmaal voor elke groep worden uitgevoerd en eenmaal voor elk lid van een groep. Met de cmdlets voor migratie worden gebruikers of groepen in het CORP-forest niet aangepast. Dit wordt later handmatig gedaan door de PAM-beheerder.

1. Meld u rechtstreeks of vanaf een PRIV-werkstation aan bij PAMSRV als *PRIV\MIMAdmin*.

2.  Start PowerShell en typ de volgende opdrachten.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Maak voor demonstratiedoeleinden een overeenkomend gebruikersaccount in PRIV voor een gebruikersaccount in een bestaand forest.

    Typ in PowerShell de volgende opdrachten.  Als u niet de naam *Jen* hebt gebruikt om eerder de gebruiker in contoso.local te maken, wijzigt u waar nodig de parameters van de opdracht. Het wachtwoord 'Pass@word1' is slechts een voorbeeld en moet worden gewijzigd in een unieke wachtwoordwaarde.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Kopieer voor demonstratiedoeleinden een groep en het bijbehorende lid (Jen) van het CONTOSO- naar het PRIV-domein.

    Voer de volgende opdrachten uit en geef het wachtwoord van de CORP-domeinbeheerder (CONTOSO\Administrator) op als u hierom wordt gevraagd:

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Ter referentie moeten voor de opdracht **New-PAMGroup** de volgende parameters worden gebruikt:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Optioneel) Verwijder op CORPDC het account van Jen uit de groep **CONTOSO CorpAdmins** als het account nog aanwezig is in deze groep.  Dit is alleen nodig voor demonstratiedoeleinden om duidelijk te maken hoe machtigingen kunnen worden gekoppeld met accounts die zijn gemaakt in het PRIV-forest.

    1.  Meld u aan bij CORPDC als *CONTOSO\Administrator*.

    2.  Start PowerShell, voer de volgende opdracht uit en bevestig de wijziging.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Als u wilt demonstreren dat forest-overschrijdende toegangsrechten geldig zijn voor het beheerdersaccount van de gebruiker, gaat u door met de volgende stap.

>[!div class="step-by-step"]
[«Stap 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[stap 7 »](step-7-elevate-user-access.md)

