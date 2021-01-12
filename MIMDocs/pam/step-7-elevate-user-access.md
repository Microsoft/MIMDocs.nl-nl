---
title: PAM implementeren - Stap 7 - Gebruikerstoegang | Microsoft Docs
description: Als laatste stap moet u tijdelijke bevoorrechte gebruikerstoegang opgeven om aan te tonen dat de Privileged Access Management-implementatie is gelukt.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 0d7e9a1111e41008a989ff2bfd52c9d79debf104
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104034"
---
# <a name="step-7--elevate-a-users-access"></a>Stap 7 – De toegangsrechten van een gebruiker uitbreiden

> [!div class="step-by-step"]
> [«Stap 6 ](step-6-transition-group-to-pam.md)


In deze stap wordt gedemonstreerd dat een gebruiker via MIM toegang tot een functie kan aanvragen.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Controleren of Jen geen toegang heeft tot de bevoorrechte resource

Zonder verhoogde bevoegdheden heeft Jen geen toegang tot de bevoorrechte resource in het CORP-forest.

1. Meld u af bij CORPWKSTN om alle geopende verbindingen in de cache te verwijderen.
2. Meld u aan bij CORPWKSTN als *CONTOSO\Jen* en schakel over naar de **bureaubladweergave**.
3. Open een DOS-opdrachtprompt.
4. Typ de opdracht `dir \\corpwkstn\corpfs`. Als het goed is, wordt het foutbericht **De toegang is geweigerd** weergegeven.
5. Houd het opdrachtpromptvenster geopend.

## <a name="request-privileged-access-from-mim"></a>Bevoorrechte toegang aanvragen bij MIM

> [!NOTE]
> Het is raadzaam dat het werk station een beschermd werk station (PAW) is.  Zie [apparaten beveiligen](/security/compass/privileged-access-devices)voor meer informatie.

1. Meld u op PRIVWKSTN aan als PRIV\priv.jen.
2. Klik op **Start**, **uitvoeren** en voer **PowerShell.exe** in.
3. Typ de volgende opdracht.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Typ het wachtwoord voor het account PRIV.Jen als u hierom wordt gevraagd. Er wordt een nieuw opdrachtpromptvenster weergegeven.
3. Typ de volgende opdrachten wanneer het PowerShell-venster wordt weergegeven.

    > [!NOTE]
    > Nadat u deze opdrachten uitgevoerd, zijn de volgende stappen tijdgebonden.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Sluit het PowerShell-venster nadat de stappen zijn voltooid.
5. Typ de volgende opdracht in het DOS-opdrachtvenster.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Typ het wachtwoord voor het account PRIV.Jen. Er wordt een nieuw opdrachtpromptvenster weergegeven.

## <a name="validate-the-elevated-access"></a>Valideer de uitgebreide toegangsrechten.
Typ de volgende opdrachten in het nieuwe venster dat wordt geopend.

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

Als de opdracht dir mislukt met het foutbericht **De toegang is geweigerd**, moet u de vertrouwensrelatie opnieuw controleren.

## <a name="activate-the-privileged-role"></a>De bevoorrechte rol activeren

U kunt de activering uitvoeren door bevoorrechte toegang aan te vragen via de PAM-voorbeeldportal.

1. Controleer of u op CORPWKSTN bent aangemeld als CORP\Jen.
2. Typ de volgende opdracht in een DOS-opdrachtvenster.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Typ het wachtwoord voor het account PRIV.Jen als u hierom wordt gevraagd. Er wordt een nieuw browservenster weergegeven.
4. Ga naar `http://pamsrv.priv.contoso.local:8090` en controleer of een webpagina van de voorbeeld Portal zichtbaar is.
5. Selecteer in Internet Explorer **extra**  >  **Internet opties** en klik op het tabblad **beveiliging** .
6. Klik op de **lokale intranet zone**  >  **sites**  >  **Geavanceerd** en voeg vervolgens de website toe aan de zone.
7. Sluit de dialoogvensters van **Internetopties**.
8. Klik op het linkertabblad op **Activeren**. Selecteer de **PAM-rol** en klik vervolgens op **Activeren**.

> [!Note]
> In deze omgeving kunt u ook ontdekken hoe u toepassingen kunt ontwikkelen die gebruikmaken van de PAM REST API, zoals is beschreven in het [referentiemateriaal voor de Privileged Access Management REST API](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## <a name="summary"></a>Samenvatting

Nadat u de stappen in dit overzicht hebt voltooid, hebt u een scenario voor Privileged Access Management gedemonstreerd waarin gebruikersbevoegdheden tijdelijk worden uitgebreid zodat de gebruiker toegang heeft tot beveiligde resources met een afzonderlijk bevoorrecht account. Zodra de sessie voor uitbreiding van de toegangsrechten is verlopen, is de beveiligde resource niet meer toegankelijk met het bevoorrechte account. De beslissing over welke beveiligingsgroepen de bevoorrechte rollen vertegenwoordigen, wordt gecoördineerd door de PAM-beheerder. Nadat de toegangsrechten zijn gemigreerd naar het Privileged Access Management-systeem, kan er geen toegang meer worden verkregen met het oorspronkelijke gebruikersaccount, maar moeten gebruikers zich aanmelden met een speciaal bevoorrecht account dat alleen op aanvraag beschikbaar wordt gesteld. Als gevolg hiervan zijn groepslidmaatschappen voor maximaal bevoorrechte accounts slechts tijdelijk van kracht.

> [!div class="step-by-step"]
> [«Stap 6 ](step-6-transition-group-to-pam.md)
