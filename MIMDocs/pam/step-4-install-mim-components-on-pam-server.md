---
title: PAM installeren - Stap 4 - MIM installeren | Microsoft Docs
description: De MIM-service en -portal op uw Privileged Access Management-server en -werkstations installeren en configureren.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8f81f592beff9ab952d1a42760e06a4f622316e8
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395466"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>Stap 4: MIM-onderdelen installeren op een PAM-server en -werkstation

> [!div class="step-by-step"]
> [«Stap 3](step-3-prepare-pam-server.md) 
>  [Stap 5»](step-5-establish-trust-between-priv-corp-forests.md)

Meld u op PAMSRV aan als PRIV\Administrator om de MIM-service en -portal en de voorbeeldwebtoepassing van de portal te installeren.

  > [!NOTE]
  > U moet een domeinbeheerder zijn. Als u de volgende opdrachten niet uitvoert als een domeinbeheerder, kunnen de controles voor vertrouwensvalidatie in de volgende stap niet worden voltooid.

Als u MIM hebt gedownload, pakt u het MIM-installatiearchief uit naar een nieuwe map.

## <a name="run-the-service-and-portal-install-program"></a>Voer het installatieprogramma voor de service en portal uit.

Volg de richtlijnen van het installatieprogramma en voltooi de installatie.

1. Als u de onderdeelfuncties selecteert, moet u ervoor zorgen dat u de MIM-service (met Privileged Access Management, maar niet met MIM-rapportage) en de MIM-portal selecteert  

   ![Aangepaste configuratie - schermafbeelding](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. Bij het configureren van algemene services en de MIM-databaseverbinding moet u **Een nieuwe database maken** opgeven.

   > [!NOTE]
   > Als u de MIM-service meerdere keren installeert voor maximale beschikbaarheid, geeft **Een bestaande database gebruiken** op voor alle volgende installaties.

3. Bij het configureren van een e-mailserververbinding moet u de e-mailserver instellen op de hostnaam van een Exchange- of SMTP-server voor de CORP-omgeving (gebruik corpdc.contoso.local als u geen e-mailserver hebt) en schakel de selectievakjes **SSL gebruiken** en **E-mailserver is Exchange Server 2007 of Exchange Server 2010** uit.

4. Kies ervoor om een nieuw zelfondertekend certificaat te genereren.

5. Stel de volgende accountreferenties in:
   - Naam van serviceaccount: *MIMService*  
   - Wacht woord van service account: <em>Pass@word1</em> (of het wacht woord dat u hebt gemaakt in stap 2)  
   - Domein van serviceaccount: *PRIV*  
   - E-mail account van de service: <em>MIMService@priv.contoso.local</em>  

6. Accepteer de standaardwaarden voor de hostnaam van de synchronisatieserver en geef *PRIV\MIMMA* op als het MIM-beheeragentaccount. Er wordt een waarschuwing weergegeven dat de MIM-synchronisatieservice niet bestaat. Deze waarschuwing is OK, omdat de MIM-synchronisatie service niet wordt gebruikt in dit scenario.

7. Stel *PAMSRV* in als het serveradres van de MIM-service.

8. Instellen `http://pamsrv.priv.contoso.local:82` als de URL van de share point-site verzameling.

9. Laat de URL voor de registratieportal leeg.

10. Schakel het selectievakje in om de poorten 5725 en 5726 in de firewall te openen en schakel het selectievakje in om alle geverifieerde gebruikers toegang te verlenen tot de MIM-portal-site.

11. Laat de hostnaam van de PAM REST API leeg en stel *8086* in als het poortnummer.

    ![Bindingsgegevens voor de PAM REST API - schermafbeelding](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Configureer het account voor de MIM PAM REST API voor het gebruik van hetzelfde account als SharePoint (omdat de MIM-portal ook op deze server is geplaatst):
    - Naam van account voor groep van toepassingen: *SharePoint*  
    - Wacht woord <em>Pass@word1</em> voor de groep van toepassingen: (of het wacht woord dat u hebt gemaakt in stap 2)  
    - Naam van account voor groep van toepassingen: *PRIV*  

    ![Referentie van account voor groep van toepassingen - schermafbeelding](./media/PAM_GS_Configure_Component_Service.png)

    Er kan een waarschuwing worden weergegeven dat het serviceaccount niet is beveiligd in de huidige configuratie. Dat is niet erg.

13. De MIM-service voor het PAM-onderdeel configureren:
    - Naam van serviceaccount: *MIMComponent*
    - Wacht woord van service account: <em>Pass@word1</em> (of het wacht woord dat u hebt gemaakt in stap 2)  
    - Domein van serviceaccount: *PRIV*

    ![Referenties van het serviceaccount voor het PAM-onderdeel - schermafbeelding](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. De service voor PAM-controle configureren:
    - Naam van serviceaccount: *MIMMonitor*  
    - Wacht woord van service account: <em>Pass@word1</em> (of het wacht woord dat u hebt gemaakt in stap 2)  
    - Domein van serviceaccount: *PRIV*  

    ![Referenties van het serviceaccount voor PAM-controle - schermafbeelding](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Op de portal-pagina voor het invoeren van de gegevens voor het MIM-wachtwoord kunt u de selectievakjes leeg laten en doorgaan. Klik op **Volgende** om door te gaan met de installatie.

Nadat de installatie is voltooid, wordt de server opnieuw opgestart. Controleer of de MIM-portal actief is en stel in dat gebruikers hun eigen objectresource kunnen weergeven in MIM.

## <a name="set-up-mim-portal-management-policy-rules"></a>Beheerbeleidsregels voor de MIM-portal instellen

1. Nadat PAMSRV opnieuw is opgestart, meldt u zich aan als PRIV\Administrator.

2. Start Internet Explorer en maak verbinding met de MIM-Portal op `http://pamsrv.priv.contoso.local:82/identitymanagement` . Er is mogelijk een korte vertraging wanneer voor de eerste keer naar deze pagina wordt gezocht.

3. Meld u zo nodig aan als PRIV\Administrator voor Internet Explorer.

4. Open in Internet Explorer de **Internet opties**, ga naar het tabblad **beveiliging** en voeg de site toe aan de **zone Lokaal intranet** als deze nog niet aanwezig is. Sluit het dialoogvenster Internetopties.

5. Geef in Internet Explorer de MIM-portal weer en klik op **Beheerbeleidsregels**.

6. Zoek naar de beheerbeleidsregel **Gebruikersbeheer: gebruikers kunnen hun eigen kenmerken lezen**.

7. Selecteer deze beheerbeleidsregel, schakel het selectievakje **Beleid is uitgeschakeld** uit, klik op **OK** en klik vervolgens op **Verzenden**.

## <a name="verify-the-firewall-connections"></a>De firewallverbindingen controleren

De firewall moet binnenkomende verbindingen voor TCP-poort 5725, 5726, 8086 en 8090 toestaan.

1.  Start **Windows Firewall met geavanceerde beveiliging** (in Systeembeheer).  
2.  Klik op **Regels voor binnenkomende verbindingen**.  
3.  Controleer of deze twee regels worden weergegeven:  
    - Forefront Identity Manager-service (STS)
    - Forefront Identity Manager-service (webservice)  
4.  Klik op **nieuwe regel**  >  **poort**  >  **TCP** en typ de specifieke lokale poorten *8086* en *8090*. Klik door de wizard en accepteer de standaardwaarden, geef een naam op voor de regel en klik op **Voltooien**.  
5.  Sluit de toepassing Windows Firewall nadat u de wizard hebt voltooid.

6.  Start **Configuratiescherm**.  
7.  Onder netwerk en Internet selecteert u **netwerk status en taken weer geven**.
8.  Controleer of er een actief netwerk is, dat wordt vermeld als Priv. contoso. local en een domein netwerk.
9. Sluit **Configuratiescherm**.

## <a name="optional-set-up-the-sample-web-application"></a>Optioneel: de voor beeld-webtoepassing instellen

In deze sectie kunt u de voor beeld-webtoepassing voor de MIM PAM-REST API installeren en configureren.  Dit onderdeel is alleen nodig als u wilt weten hoe u de MIM PAM-REST API kunt gebruiken. Als u Power shell wilt gebruiken om toegang aan te vragen en goed te keuren, gaat u verder met de volgende sectie om de MIM PAM-aanvrager-cmdlets te installeren.

1. Download de [ identiteitsbeheervoorbeelden](https://github.com/Azure/identity-management-samples) als een zip-bestand uit het archief van de voorbeeldwebtoepassing.

2. Pak de inhoud van de map **identity-management-samples-master\Privileged-Access-Management-Portal\src** uit in een nieuwe map **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3. Maak een nieuwe website in IIS. Gebruik hiervoor de sitenaam MIM Privileged Access Management Example Portal, het fysieke pad C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal en de poort 8090.  Het maken van deze site kan worden uitgevoerd met behulp van de volgende Power shell-opdracht:

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. Stel de voorbeeldwebtoepassing zo in dat gebruikers worden omgeleid naar de MIM PAM REST API. Bewerk het bestand **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config** met behulp van een tekst editor, zoals Klad blok. Voeg in de sectie **<System. webserver>** de volgende regels toe:

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. Configureer de voorbeeldwebtoepassing. Bewerk het bestand **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js** met een teksteditor zoals Kladblok. Stel de waarde van **pamrespapiurl in** in op `http://pamsrv.priv.contoso.local:8086/api/pamresources/` .

6. Start IIS opnieuw met de volgende opdracht om deze wijzigingen door te voeren.

   ```cmd
   iisreset
   ```

7. (Optioneel) Controleer of de gebruiker bij de REST API kan worden geverifieerd. Open een webbrowser als de beheerder op PAMSRV.  Ga naar de website-URL `http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/` , verifieer indien nodig, en zorg ervoor dat er een down load wordt uitgevoerd.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>De aanvrager-cmdlets van MIM PAM installeren

Installeer de aanvrager-cmdlets van MIM PAM op het werkstation dat u hebt geconfigureerd in stap 1.

1.  Meld u bij CORPWKSTN aan als beheerder.

2.  Download de **invoegtoepassingen en extensies** op de CORPWKSTN-computer als deze nog niet aanwezig zijn.

3.  Pak de map met **invoegtoepassingen en extensie** in het archief uit in een nieuwe map.

4.  Voer het installatieprogramma **setup.exe** uit.

5.  Geef bij de aangepaste installatie op dat de **PAM-client** moet worden geïnstalleerd, maar niet de **MIM-invoegtoepassing voor Outlook** of de **MIM-wachtwoord- en -verificatie-extensies**.

6.  Geef op het adres van de PAM-server de hostnaam van de PRIV MIM-server op `pamsrv.priv.contoso.local` .

Start nadat de installatie is voltooid CORPWKSTN opnieuw op om de registratie van de nieuwe PowerShell-module te voltooien.

In de volgende stap gaat u de vertrouwensrelatie tussen het PRIV- en CORP forest instellen.

> [!div class="step-by-step"]
> [«Stap 3](step-3-prepare-pam-server.md) 
>  [Stap 5»](step-5-establish-trust-between-priv-corp-forests.md)
