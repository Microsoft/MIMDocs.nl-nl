---
title: PAM implementeren - Stap 7 - Gebruikerstoegang | Microsoft Docs
description: Als laatste stap moet u tijdelijke bevoorrechte gebruikerstoegang opgeven om aan te tonen dat de Privileged Access Management-implementatie is gelukt.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---

# Stap 7 – De toegangsrechten van een gebruiker uitbreiden
<a id="step-7--elevate-a-users-access" class="xliff"></a>

>[!div class="step-by-step"]
[« Stap 6 ](step-6-transition-group-to-pam.md)


In deze stap wordt gedemonstreerd dat een gebruiker via MIM toegang tot een functie kan aanvragen.

## Controleren of Jen geen toegang heeft tot de bevoorrechte resource
<a id="verify-that-jen-cannot-access-the-privileged-resource" class="xliff"></a>
Zonder verhoogde bevoegdheden heeft Jen geen toegang tot de bevoorrechte resource in het CORP-forest.

1. Meld u af bij CORPWKSTN om alle geopende verbindingen in de cache te verwijderen.
2. Meld u aan bij CORPWKSTN als *CONTOSO\Jen* en schakel over naar de **bureaubladweergave**.
3. Open een DOS-opdrachtprompt.
4. Typ de opdracht `dir \\corpwkstn\corpfs`. Als het goed is, wordt het foutbericht **De toegang is geweigerd** weergegeven.
5. Houd het opdrachtpromptvenster geopend.

## Bevoorrechte toegang aanvragen bij MIM
<a id="request-privileged-access-from-mim" class="xliff"></a>
1. Typ op CORPWKSTN de volgende opdracht terwijl u nog steeds als CONTOSO\Jen bent aangemeld.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Typ het wachtwoord voor het account PRIV.Jen als u hierom wordt gevraagd. Er wordt een nieuw opdrachtpromptvenster weergegeven.
3. Typ de volgende opdrachten wanneer het PowerShell-venster wordt weergegeven.

    > [!NOTE]
    > Nadat u deze opdrachten uitgevoerd, zijn de volgende stappen tijdgebonden.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Sluit het PowerShell-venster nadat de stappen zijn voltooid.
5. Typ de volgende opdracht in het DOS-opdrachtvenster.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Typ het wachtwoord voor het account PRIV.Jen. Er wordt een nieuw opdrachtpromptvenster weergegeven.

## Valideer de uitgebreide toegangsrechten.
<a id="validate-the-elevated-access" class="xliff"></a>
Typ de volgende opdrachten in het nieuwe venster dat wordt geopend.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Als de opdracht dir mislukt met het foutbericht **De toegang is geweigerd**, moet u de vertrouwensrelatie opnieuw controleren.

## De bevoorrechte rol activeren
<a id="activate-the-privileged-role" class="xliff"></a>
U kunt de activering uitvoeren door bevoorrechte toegang aan te vragen via de PAM-voorbeeldportal.

1. Controleer of u op CORPWKSTN bent aangemeld als CORP\Jen.
2. Typ de volgende opdracht in een DOS-opdrachtvenster.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Typ het wachtwoord voor het account PRIV.Jen als u hierom wordt gevraagd. Er wordt een nieuw browservenster weergegeven.
4. Navigeer naar http://pamsrv.priv.contoso.local:8090 en controleer of een webpagina op de voorbeeldportal zichtbaar is.
5. Selecteer in Internet Explorer **Extra** > **Internetopties** en klik op het tabblad **Beveiliging**.
6. Klik op **Lokale intranetzone** > **Sites** > **Geavanceerd** en voeg vervolgens de website toe aan de zone.
7. Sluit de dialoogvensters van **Internetopties**.
8. Klik op het linkertabblad op **Activeren**. Selecteer de **PAM-rol** en klik vervolgens op **Activeren**.

> [!Note]
> In deze omgeving kunt u ook ontdekken hoe u toepassingen kunt ontwikkelen die gebruikmaken van de PAM REST API, zoals is beschreven in het [referentiemateriaal voor de Privileged Access Management REST API](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## Samenvatting
<a id="summary" class="xliff"></a>
Nadat u de stappen in dit overzicht hebt voltooid, hebt u een scenario voor Privileged Access Management gedemonstreerd waarin gebruikersbevoegdheden tijdelijk worden uitgebreid zodat de gebruiker toegang heeft tot beveiligde resources met een afzonderlijk bevoorrecht account. Zodra de sessie voor uitbreiding van de toegangsrechten is verlopen, is de beveiligde resource niet meer toegankelijk met het bevoorrechte account. De beslissing over welke beveiligingsgroepen de bevoorrechte rollen vertegenwoordigen, wordt gecoördineerd door de PAM-beheerder. Nadat de toegangsrechten zijn gemigreerd naar het Privileged Access Management-systeem, kan er geen toegang meer worden verkregen met het oorspronkelijke gebruikersaccount, maar moeten gebruikers zich aanmelden met een speciaal bevoorrecht account dat alleen op aanvraag beschikbaar wordt gesteld. Als gevolg hiervan zijn groepslidmaatschappen voor maximaal bevoorrechte accounts slechts tijdelijk van kracht.

>[!div class="step-by-step"]
[« Stap 6 ](step-6-transition-group-to-pam.md)

