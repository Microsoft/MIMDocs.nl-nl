---
title: Werken met de portal van de self-service voor wachtwoord opnieuw instellen | Microsoft Docs
description: Ontdek wat er nieuw is bij de selfservice voor wachtwoordherstel in MIM 2016, zoals de werking van SSPR met meervoudige verificatie.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: davidste
ms.date: 06/26/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: b1b30b744a5f735512f31d98184a561ce3f9b047
ms.sourcegitcommit: 03617b441135a55b664e0d81cce4d17541bee93b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36963372"
---
# <a name="working-with-self-service-password-reset"></a>Werken met de selfservice voor wachtwoordherstel

> [!IMPORTANT]
> Als gevolg van de aankondiging van afschaffing van Azure multi-factor Authentication Software Development Kit. De Azure MFA-SDK worden voor bestaande klanten tot de datum buiten gebruik stellen van 14 November 2018 ondersteund. Huidige en nieuwe klanten kunnen niet meer SDK downloaden via de klassieke Azure portal. U kunt u downloaden moet bereiken Azure klantondersteuning om uw gegenereerde Servicereferenties MFA-pakket wordt ontvangen. <br> Het ontwikkelteam Microsoft werkt op wijzigingen in MFA door te integreren met MFA Server SDK.  Hiermee worden opgenomen in een toekomstige hotfix Zie [versiegeschiedenis](/reference/version-history.md) aankondigingen.

Microsoft Identity Manager 2016 biedt aanvullende functionaliteit voor de functie Wachtwoord opnieuw instellen in Selfservice. Deze functionaliteit is uitgebreid met verschillende belangrijke functies:

-   De selfservice voor wachtwoordherstel portal en de Windows-aanmeldingsscherm nu kunnen gebruikers hun account ontgrendelen zonder hun wachtwoorden wijzigen of beheerders ondersteuning aan te roepen. Gebruikers kunnen ophalen vergrendeld hun account voor diverse geldige redenen, zoals als ze een oud wachtwoord hebben ingevoerd, tweetalige computer gebruiken en het toetsenbord ingesteld op de verkeerde taal hebben of poging om aan te melden bij een gedeeld werkstation aan een account van iemand anders geopend.

-   Er is een nieuwe verificatiepoort, een telefoonpoort, toegevoegd. Deze poort kan de gebruikersverificatie via een telefoongesprek.

-   Is er ondersteuning toegevoegd voor de service Microsoft Azure multi-factor Authentication (MFA). Deze service kan worden gebruikt voor de bestaande SMS-Gate een eenmalig wachtwoord of de nieuwe Telefoonpoort.

## <a name="azure-for-multi-factor-authentication"></a>Azure voor meervoudige verificatie
Microsoft Azure Multi-Factor Authentication is een verificatieservice waarbij gebruikers zich bij het aanmelden moeten verifiëren door middel van een mobiele app, telefonische oproep of een tekstbericht. Het kan met Microsoft Azure Active Directory worden gebruikt en als service voor bedrijfstoepassingen in de cloud en on-premises.

Azure MFA biedt een aanvullende verificatiemethode waarmee bestaande verificatieprocessen kunnen worden uitgebreid, zoals de verificatie die wordt uitgevoerd door MIM voor de aanmeldassistent van de selfservice.

Gebruikers verifiëren zich met Azure MFA bij het systeem om hun identiteit kenbaar te maken en tegelijkertijd om opnieuw toegang te krijgen tot hun account en resources. Verificatie kan plaatsvinden via sms of een telefoonoproep.   Hoe strenger de verificatie, hoe groter het vertrouwen dat de persoon die toegang probeert te krijgen inderdaad de werkelijke gebruiker is die houder is van de identiteit. Wanneer de gebruiker eenmaal is geverifieerd, kan deze een nieuw wachtwoord kiezen ter vervanging van het oude.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Vereisten voor het instellen van de selfservicefuncties voor het ontgrendelen van het account en het opnieuw instellen van het wachtwoord met MFA
In deze sectie wordt ervan uitgegaan dat u Microsoft Identity Manager 2016 hebt gedownload en de implementatie hiervan hebt voltooid, met inbegrip van de volgende onderdelen en services:

-   Een Windows Server 2008 R2-server of hoger is ingesteld als een Active Directory-server, met inbegrip van AD Domain Services en de domeincontroller met een aangewezen domein (een 'bedrijfsdomein')

-   Er is een groepsbeleid gedefinieerd voor de accountvergrendeling

-   De MIM 2016-synchronisatieservice (Sync) is geïnstalleerd en actief op een server die deel uitmaakt van het AD-domein

-   De MIM 2016-service &amp; -portal, met inbegrip van de SSPR-registratieportal en de SSPR-portal voor het opnieuw instellen van het wachtwoord, zijn geïnstalleerd en actief op een server (kan met de synchronisatieservice op dezelfde server worden geplaatst)

-   MIM Sync is geconfigureerd voor AD MIM- identiteitssynchronisatie, met inbegrip van:

    -   De configuratie van ADMA (Active Directory Management Agent) voor de verbinding met AD DS en voor het importeren en exporteren van identiteitsgegevens vanuit en naar Active Directory.

    -   De configuratie van MIM MA (MIM Management Agent) voor de verbinding met de FIM-servicedatabase en voor het importeren en exporteren van identiteitsgegevens vanuit en naar de FIM-database.

    -   De configuratie van synchronisatieregels in de MIM-portal om de synchronisatie van gebruikersgegevens toe te staan en synchronisatieactiviteiten in de MIM-service mogelijk te maken.

-   MIM 2016-invoegtoepassingen &amp; uitbreidingen, zoals de SSPR Windows geïntegreerde aanmelding-client wordt geïmplementeerd op de server of op een afzonderlijke clientcomputer.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>MIM voorbereiden voor het toepassen van meervoudige verificatie
Configureer MIM Sync voor de ondersteuning van de functies voor het opnieuw instellen van het wachtwoord en het ontgrendelen van het account. Zie [De FIM-invoegtoepassingen en -uitbreidingen installeren](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [FIM SSPR installeren](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR-verificatiepoorten](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) en [de handleiding voor de SSPR-testomgeving](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

In de volgende sectie stelt u de Azure MFA-provider in Microsoft Azure Active Directory in. Genereert u een bestand met de verificatiegegevens die nodig zijn voor Azure MFA contact kunnen maken.  U moet over een Azure-abonnement beschikken om verder te gaan.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Uw provider voor meervoudige verificatie in Azure registreren

1.  Maak een [MFA-provider](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Open een ondersteuningsaanvraag en aanvragen van de directe SDK voor ASP.net 2.0 C#. De SDK wordt alleen voor gebruikers van MIM met MFA worden opgegeven omdat de directe SDK is afgeschaft. Nieuwe klanten moeten de volgende versie van MIM die kan worden geïntegreerd met MFA-server vaststellen.

3.  Klik op **App-services &gt; Active Directory &gt; Provider voor Multi-Factor Authentication&gt; Snelle invoer**.

![Azure Portals snelle maken MFA installatiekopie](media/MIM-SSPR-Azureportal.png)

4.  Typ in het veld **Naam** de tekst **SSPRMFA** en klik vervolgens op **Maken**.

![Afbeelding voor Azure Portal MFA](media/MIM-SSPR-Azureportal-2.png)

5.  Klik op **Active Directory** in het menu van de klassieke Azure Portal en vervolgens op het tabblad **Providers voor meervoudige verificatie**.

6.  Klik op **SSPRMFA** en vervolgens op **Beheren** onder aan het scherm.

    ![Pictogram voor Beheren in Azure Portal](media/MIM-SSPR-ManageButton.png)

7.  Klik in het nieuwe venster in het linkerdeelvenster onder **Configureren** op **Instellingen**.

8.  Onder **fraudewaarschuwing**, schakel het selectievakje ** gebruiker blokkeren wanneer fraude wordt gemeld. Als u het selectievakje uitschakelt, is om te voorkomen dat de hele service wordt geblokkeerd.

9. Klik in het venster **Microsoft Azure Multi-Factor Authentication** dat wordt geopend op **SDK** onder **Downloads** in het menu aan de linkerkant.

10. Klik op de koppeling **Downloaden** in de kolom met ZIP-bestanden voor het bestand met de programmeertaal **SDK voor ASP.net 2.0 C**.

    ![Afbeelding voor het downloaden van het ZIP-bestand voor Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Kopieer het betreffende ZIP-bestand naar elk systeem waarop de MIM-service is geïnstalleerd.  Houd er rekening mee dat het ZIP-bestand sleutelmateriaal bevat dat wordt gebruikt bij de verificatie voor de Azure MFA-service.

### <a name="update-the-configuration-file"></a>Het configuratiebestand bijwerken

1. Meld u als de gebruiker die MIM heeft geïnstalleerd aan bij de computer waarop de MIM-service is geïnstalleerd.

2. Maak een nieuwe map onder de map waarin de MIM-service is geïnstalleerd, bijvoorbeeld: **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Navigeer in Windows Verkenner naar de map **\pf\certs** van het ZIP-bestand dat in de vorige sectie is gedownload en kopieer het bestand **cert_key.p12** naar de nieuwe map.

4.  Open het bestand **pf_auth.cs** in de map **\pf** van het ZIP-bestand voor de SDK.

5.  Zoek de volgende drie parameters op: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Afbeelding van de code in pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  Open in **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** het bestand: **MfaSettings**.xml.

7.  Kopieer de waarden van de parameters `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` in het bestand pf_aut.cs naar de overeenkomstige XML-elementen in het bestand MfaSettings.xml.

8.  Pak in het ZIP-bestand voor de SDK, onder \pf\certs, het bestand **cert_key.p12** uit en voer het volledige pad naar dit bestand in het XML-element `<CertFilePath>` in het bestand MfaSettings.xml in.

9. Voer in het element `<username>` een gebruikersnaam in.

10. Voer in het element `<DefaultCountryCode>` uw standaardlandnummer in. In gevallen waar telefoonnummers worden geregistreerd voor gebruikers zonder een landnummer, krijgen gebruikers deze code. In gevallen waarin een gebruiker een internationale landcode heeft, heeft deze in het geregistreerde telefoonnummer worden opgenomen.

11. Sla het bestand MfaSettings.xml op met dezelfde naam en op dezelfde locatie.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>De poort voor verificatie met een eenmalig wachtwoord via sms of de nieuwe telefoonpoort configureren

1.  Start Internet Explorer en navigeer naar de MIM-Portal, verifieer uzelf als de MIM-beheerder en klik vervolgens op **werkstromen** in de linkernavigatiebalk.

    ![Afbeelding van de navigatie in de MIM-portal](media/MIM-SSPR-workflow.jpg)

2.  Schakel het selectievakje **Verificatiewerkstroom voor het opnieuw instellen van het wachtwoord** in.

    ![Afbeelding van de werkstromen in MIM-portal](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Klik op het tabblad **Activiteiten** en schuif vervolgens omlaag naar **Activiteit toevoegen**.

4.  Selecteer **Telefoonpoort** of **SMS-gate voor OTP (eenmalig wachtwoord)**, klik op **Selecteren** en vervolgens op **OK**.

Gebruikers in uw organisatie kunnen zich nu registreren voor het opnieuw instellen van het wachtwoord.  Bij de registratie voeren de gebruikers hun zakelijke telefoonnummer of mobiele telefoonnummer in zodat ze kunnen worden gebeld (of naar hen sms-berichten kunnen worden verzonden).

#### <a name="register-users-for-password-reset"></a>Gebruikers registreren voor het opnieuw instellen van het wachtwoord

1.  Een gebruiker wordt opent een webbrowser en navigeer naar de MIM opnieuw Portal voor Wachtwoordregistratie.  (Deze portal is meestal geconfigureerd met Windows-verificatie).  In de portal geven ze nogmaals hun gebruikersnaam en wachtwoord op om hun identiteit te bevestigen.

    Ze moeten naar de portal voor wachtwoordregistratie gaan en zich verifiëren met hun gebruikersnaam en wachtwoord.

2.  De gebruikers moeten in het veld **Telefoonnummer** of **Mobiele telefoon** een landnummer, een spatie en het telefoonnummer invoeren en op **Volgende** klikken.

    ![Afbeelding voor de verificatie via de telefoon in MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Afbeelding voor de verificatie via de mobiele telefoon in MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Hoe werkt dit voor de gebruikers?
Nu alles is geconfigureerd en actief is, wilt u misschien wel weten wat de gebruikers te wachten staat wanneer ze hun wachtwoord vlak voor een vakantie opnieuw hebben ingesteld en na de vakantie tot de ontdekking komen dat ze dit compleet zijn vergeten.

De gebruiker kan op twee manieren gebruikmaken van de functies voor het opnieuw instellen van het wachtwoord en het ontgrendelen van het account: in het Windows-aanmeldingsscherm of in de Selfservice portal.

Wanneer de MIM-invoegtoepassingen en -uitbreidingen worden geïnstalleerd op een computer in het domein die via het netwerk van uw organisatie is verbonden met de MIM-service, kunnen gebruikers een vergeten wachtwoord herstellen bij het aanmelden op de computer.  U moet hiervoor de onderstaande stappen uitvoeren.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Windows desktop geïntegreerde aanmelding-wachtwoord opnieuw instellen

1.  Als de gebruiker meerdere keren het verkeerde wachtwoord in het scherm invoert, kan deze op **Problemen bij het aanmelden?** klikken .

    ![Afbeelding van het aanmeldingsscherm](media/MIM-SSPR-problemsloggingin.JPG)

    Wanneer gebruikers op deze koppeling klikken, worden ze doorgeleid naar het scherm voor het opnieuw instellen van het wachtwoord in MIM. Hier kunnen ze hun wachtwoord wijzigen of hun account ontgrendelen.

    ![Afbeelding voor het wachtwoord opnieuw instellen in MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  De gebruiker wordt gevraagd om zich te verifiëren. Als MFA is geconfigureerd, wordt de gebruiker gebeld.

3.  Op de achtergrond is wat er gebeurt door Azure MFA een telefonische oproep naar het nummer van de gebruiker is opgegeven toen deze zich aangemeld voor de service.

4.  Wanneer de gebruiker de telefoonoproep beantwoordt, wordt deze gevraagd om de hekjestoets (#) op de telefoon in te toetsen. De gebruiker klikt vervolgens in de portal op **Volgende**.

    Als u ook andere poorten instelt, wordt de gebruiker gevraagd om in opeenvolgende schermen meer gegevens te verstrekken.

    > [!NOTE]
    > Als de gebruiker ongeduldig is en op **Volgende** klikt voordat deze de hekjestoets (#) intoetst, mislukt de verificatie.

5.  Wanneer de gebruiker is geverifieerd, heeft deze twee opties: het account ontgrendelen en het huidige wachtwoord behouden of een nieuw wachtwoord instellen.

6.  De gebruiker moet vervolgens twee keer een nieuw wachtwoord invoeren, waarna het wachtwoord opnieuw is ingesteld.

#### <a name="access-from-the-self-service-portal"></a>Toegang via de selfservice portal

1.  Gebruikers kunnen een webbrowser openen, naar de **portal voor het opnieuw instellen van het wachtwoord** navigeren, hun gebruikersnaam invoeren en op **Volgende** klikken.

    Als MFA is geconfigureerd, wordt de gebruiker gebeld. Op de achtergrond is wat er gebeurt door Azure MFA een telefonische oproep naar het nummer van de gebruiker is opgegeven toen deze zich aangemeld voor de service.

    Wanneer de gebruiker de telefoonoproep beantwoordt, wordt deze gevraagd om de hekjestoets (#) op de telefoon in te toetsen. De gebruiker klikt vervolgens in de portal op **Volgende**.

2.  Als u ook andere poorten instelt, wordt de gebruiker gevraagd om in opeenvolgende schermen meer gegevens te verstrekken.

    > [!NOTE]
    > Als de gebruiker ongeduldig is en op **Volgende** klikt voordat deze de hekjestoets (#) intoetst, mislukt de verificatie.

3.  De gebruikers moeten kiezen of zij hun wachtwoord opnieuw instellen of hun account ontgrendelen. Als gebruikers ervoor kiezen om hun account te ontgrendelen, wordt het account ontgrendeld.

    ![Afbeelding voor het ontgrendelen van het account in de MIM-aanmeldassistent](media/MIM-SSPR-accountUnlock.JPG)

4.  Wanneer de gebruiker is geverifieerd, heeft deze twee opties: het huidige wachtwoord behouden of een nieuw wachtwoord instellen.

5.  ![Afbeelding voor ontgrendeld account in MIM](media/MIM-SSPR-account-unlock.JPG)

6.  Als de gebruiker ervoor kiest om het wachtwoord opnieuw in te stellen, moet deze twee keer een nieuw wachtwoord invoeren en op **Volgende** klikken om het wachtwoord te wijzigen.

    ![Afbeelding voor het opnieuw instellen van het wachtwoord in de MIM-aanmeldassistent](media/MIM-SSPR-PR1.JPG)
