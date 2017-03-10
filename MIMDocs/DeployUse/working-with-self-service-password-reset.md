---
title: Werken met de portal van de self-service voor wachtwoord opnieuw instellen | Microsoft Docs
description: Ontdek wat er nieuw is bij de selfservice voor wachtwoordherstel in MIM 2016, zoals de werking van SSPR met meervoudige verificatie.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 72c773601cd722290b6e7a9d5d13458f0409cfdc


---

# <a name="working-with-self-service-password-reset"></a>Werken met de selfservice voor wachtwoordherstel
Microsoft Identity Manager 2016 biedt aanvullende functionaliteit voor de functie Wachtwoord opnieuw instellen in Selfservice. Deze functionaliteit is uitgebreid met verschillende belangrijke functies:

-   Met de selfservice portal voor het opnieuw instellen van het wachtwoord en het Windows-aanmeldingsscherm kunnen gebruikers nu hun account ontgrendelen zonder dat zij hun wachtwoord hoeven te wijzigen of bij de beheerders om ondersteuning hoeven te vragen. De toegang tot het eigen account kan om diverse geldige redenen voor gebruikers worden geblokkeerd. Zo kunnen de gebruikers een oud wachtwoord hebben ingevoerd, een tweetalige computer gebruiken en het toetsenbord op de verkeerde taal hebben ingesteld of hebben ze geprobeerd om zich bij een gedeeld werkstation aan te melden dat al is geopend voor het account van iemand anders.

-   Er is een nieuwe verificatiepoort, een telefoonpoort, toegevoegd. Hiermee kan de gebruikersverificatie telefonisch plaatsvinden.

-   Er is ondersteuning toegevoegd voor de Microsoft Azure Multi-Factor Authentication-service (MFA). Deze kan worden gebruikt voor de bestaande poort voor verificatie door middel van een eenmalig wachtwoord via sms of de nieuwe telefoonpoort.

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

-   MIM 2016-invoegtoepassingen &amp; -uitbreidingen, zoals de geïntegreerde client voor Windows-aanmelding in SSPR die op de server of op een afzonderlijke clientcomputer wordt geïmplementeerd.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>MIM voorbereiden voor het toepassen van meervoudige verificatie
Configureer MIM Sync voor de ondersteuning van de functies voor het opnieuw instellen van het wachtwoord en het ontgrendelen van het account. Zie [De FIM-invoegtoepassingen en -uitbreidingen installeren](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [FIM SSPR installeren](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR-verificatiepoorten](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) en [de handleiding voor de SSPR-testomgeving](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

In de volgende sectie stelt u de Azure MFA-provider in Microsoft Azure Active Directory in. Als onderdeel hiervan genereert u een bestand met de verificatiegegevens die nodig zijn voor MFA om met Azure MFA verbinding te maken.  U moet over een Azure-abonnement beschikken om verder te gaan.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Uw provider voor meervoudige verificatie in Azure registreren

1.  Ga naar de [klassieke Azure Portal](http://manage.windowsazure.com) en meld u aan als Azure-abonnementbeheerder.

2.  Klik in de hoek linksonder op **Nieuw**.

3.  Klik op **App-services &gt; Active Directory &gt; Provider voor Multi-Factor Authentication&gt; Snelle invoer**.

![Afbeelding voor snelle invoer voor MFA-provider in Azure Portal](media/MIM-SSPR-Azureportal.png)

4.  Typ in het veld **Naam** de tekst **SSPRMFA** en klik vervolgens op **Maken**.

![Afbeelding voor Azure Portal MFA](media/MIM-SSPR-Azureportal-2.png)

5.  Klik op **Active Directory** in het menu van de klassieke Azure Portal en vervolgens op het tabblad **Providers voor meervoudige verificatie**.

6.  Klik op **SSPRMFA** en vervolgens op **Beheren** onder aan het scherm.

    ![Pictogram voor Beheren in Azure Portal](media/MIM-SSPR-ManageButton.png)

7.  Klik in het nieuwe venster in het linkerdeelvenster onder **Configureren** op **Instellingen**.

8.  Schakel onder **Fraudewaarschuwing** het selectievakje **Gebruiker blokkeren wanneer fraude wordt gemeld** uit. Dit wordt gedaan om te voorkomen dat de hele service wordt geblokkeerd.

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

10. Voer in het element `<DefaultCountryCode>` uw standaardlandnummer in. Wanneer telefoonnummers worden geregistreerd voor gebruikers zonder een landnummer, wordt dit landnummer gebruikt. Als een gebruiker over een internationaal landnummer beschikt, moet dit in het geregistreerde telefoonnummer worden opgenomen.

11. Sla het bestand MfaSettings.xml op met dezelfde naam en op dezelfde locatie.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>De poort voor verificatie met een eenmalig wachtwoord via sms of de nieuwe telefoonpoort configureren

1.  Start Internet Explorer en navigeer naar de MIM-portal, verifieer uzelf als de MIM-beheerder en klik vervolgens op de navigatiebalk aan de linkerkant op **Werkstromen**.

    ![Afbeelding van de navigatie in de MIM-portal](media/MIM-SSPR-workflow.jpg)

2.  Schakel het selectievakje **Verificatiewerkstroom voor het opnieuw instellen van het wachtwoord** in.

    ![Afbeelding van de werkstromen in MIM-portal](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Klik op het tabblad **Activiteiten** en schuif vervolgens omlaag naar **Activiteit toevoegen**.

4.  Selecteer **Telefoonpoort** of **SMS-gate voor OTP (eenmalig wachtwoord)**, klik op **Selecteren** en vervolgens op **OK**.

Gebruikers in uw organisatie kunnen zich nu registreren voor het opnieuw instellen van het wachtwoord.  Bij de registratie voeren de gebruikers hun zakelijke telefoonnummer of mobiele telefoonnummer in zodat ze kunnen worden gebeld (of naar hen sms-berichten kunnen worden verzonden).

#### <a name="register-users-for-password-reset"></a>Gebruikers registreren voor het opnieuw instellen van het wachtwoord

1.  De gebruiker opent een webbrowser om naar de MIM-registratieportal voor het opnieuw instellen van het wachtwoord te navigeren.  (Deze portal is meestal geconfigureerd met Windows-verificatie).  In de portal geven ze nogmaals hun gebruikersnaam en wachtwoord op om hun identiteit te bevestigen.

    Ze moeten naar de portal voor wachtwoordregistratie gaan en zich verifiëren met hun gebruikersnaam en wachtwoord.

2.  De gebruikers moeten in het veld **Telefoonnummer** of **Mobiele telefoon** een landnummer, een spatie en het telefoonnummer invoeren en op **Volgende** klikken.

    ![Afbeelding voor de verificatie via de telefoon in MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Afbeelding voor de verificatie via de mobiele telefoon in MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Hoe werkt dit voor de gebruikers?
Nu alles is geconfigureerd en actief is, wilt u misschien wel weten wat de gebruikers te wachten staat wanneer ze hun wachtwoord vlak voor een vakantie opnieuw hebben ingesteld en na de vakantie tot de ontdekking komen dat ze dit compleet zijn vergeten.

De gebruiker kan op twee manieren gebruikmaken van de functies voor het opnieuw instellen van het wachtwoord en het ontgrendelen van het account: in het Windows-aanmeldingsscherm of in de Selfservice portal.

Wanneer de MIM-invoegtoepassingen en -uitbreidingen worden geïnstalleerd op een computer in het domein die via het netwerk van uw organisatie is verbonden met de MIM-service, kunnen gebruikers een vergeten wachtwoord herstellen bij het aanmelden op de computer.  U moet hiervoor de onderstaande stappen uitvoeren.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Geïntegreerde functie voor het opnieuw instellen van het wachtwoord bij de aanmelding bij het Windows-bureaublad

1.  Als de gebruiker meerdere keren het verkeerde wachtwoord in het scherm invoert, kan deze op **Problemen bij het aanmelden?** klikken .

    ![Afbeelding van het aanmeldingsscherm](media/MIM-SSPR-problemsloggingin.JPG)

    Wanneer gebruikers op deze koppeling klikken, worden ze doorgeleid naar het scherm voor het opnieuw instellen van het wachtwoord in MIM. Hier kunnen ze hun wachtwoord wijzigen of hun account ontgrendelen.

    ![Afbeelding voor het wachtwoord opnieuw instellen in MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  De gebruiker wordt gevraagd om zich te verifiëren. Als MFA is geconfigureerd, wordt de gebruiker gebeld.

3.  Op de achtergrond wordt door Azure MFA naar het nummer gebeld dat door de gebruiker is opgegeven toen deze zich voor de service heeft aangemeld.

4.  Wanneer de gebruiker de telefoonoproep beantwoordt, wordt deze gevraagd om de hekjestoets (#) op de telefoon in te toetsen. De gebruiker klikt vervolgens in de portal op **Volgende**.

    Als u ook andere poorten instelt, wordt de gebruiker gevraagd om in opeenvolgende schermen meer gegevens te verstrekken.

    > [!NOTE]
    > Als de gebruiker ongeduldig is en op **Volgende** klikt voordat deze de hekjestoets (#) intoetst, mislukt de verificatie.

5.  Wanneer de gebruiker is geverifieerd, heeft deze twee opties: het account ontgrendelen en het huidige wachtwoord behouden of een nieuw wachtwoord instellen.

6.  De gebruiker moet vervolgens twee keer een nieuw wachtwoord invoeren, waarna het wachtwoord opnieuw is ingesteld.

#### <a name="access-from-the-self-service-portal"></a>Toegang via de selfservice portal

1.  Gebruikers kunnen een webbrowser openen, naar de **portal voor het opnieuw instellen van het wachtwoord** navigeren, hun gebruikersnaam invoeren en op **Volgende** klikken.

    Als MFA is geconfigureerd, wordt de gebruiker gebeld. Op de achtergrond wordt door Azure MFA naar het nummer gebeld dat door de gebruiker is opgegeven toen deze zich voor de service heeft aangemeld.

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



<!--HONumber=Jan17_HO4-->


