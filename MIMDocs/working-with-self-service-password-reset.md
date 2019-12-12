---
title: Werken met self-service voor wachtwoord herstel | Microsoft Docs
description: Ontdek wat er nieuw is bij de selfservice voor wachtwoordherstel in MIM 2016, zoals de werking van SSPR met meervoudige verificatie.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 90452391170114270765e9a7fe08e98eea0747e4
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690702"
---
# <a name="self-service-password-reset-deployment-options"></a>Implementatie opties voor de selfservice voor wachtwoord herstel

Voor nieuwe klanten die [een licentie voor Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)hebben, kunt u het beste [Azure AD self-service voor wachtwoord herstel](/azure/active-directory/authentication/concept-sspr-howitworks) gebruiken om de gebruikers ervaring te bieden.  De self-service voor wachtwoord herstel van Azure AD biedt zowel een webgebaseerde als Windows-geïntegreerde ervaring voor een gebruiker om hun eigen wacht woord opnieuw in te stellen, en biedt ondersteuning voor veel van de mogelijkheden van MIM, inclusief alternatieve e-mail en Q & een Gates.  Wanneer u de selfservice voor wachtwoord herstel van Azure AD implementeert, ondersteunt Azure AD Connect [het schrijven van de nieuwe wacht woorden naar AD DS](/azure/active-directory/authentication/concept-sspr-writeback)en de MIM- [meldings service voor wachtwoord wijzigingen](deploying-mim-password-change-notification-service-on-domain-controller.md) kan worden gebruikt om de wacht woorden door te sturen naar andere systemen, zoals de Directory server van een andere leverancier.  Voor het implementeren van MIM voor [wachtwoord beheer](infrastructure/mim2016-password-management.md) is het niet vereist dat de MIM-service of de MIM self-service voor wachtwoord herstel of registratie portals worden geïmplementeerd.  In plaats daarvan kunt u deze stappen volgen:

- Als u eerst wacht woorden wilt verzenden naar andere mappen dan Azure AD en AD DS, implementeert u MIM sync with Connect oren to Active Directory Domain Services en eventuele extra doel systemen, configureert u MIM voor [wachtwoord beheer](infrastructure/mim2016-password-management.md) en implementeert u de [meldings service voor wachtwoord wijzigingen](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Als u vervolgens wacht woorden moet verzenden naar andere mappen dan Azure AD, configureert u Azure AD Connect voor [het terugschrijven van de nieuwe wacht woorden naar AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- U kunt [gebruikers eventueel vooraf registreren](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Implementeer ten slotte [Azure AD self-service voor wachtwoord herstel naar uw eind gebruikers](/azure/active-directory/authentication/howto-sspr-deployment).

Voor bestaande klanten die eerder Forefront Identity Manager (FIM) voor selfservice voor het opnieuw instellen van wacht woorden hebben geïmplementeerd en waarvoor een licentie is verleend voor Azure Active Directory Premium, wordt u aangeraden om over te stappen op het opnieuw instellen van een wacht woord voor Azure AD self-service.  U kunt eind gebruikers overstappen naar Azure AD selfservice voor het opnieuw instellen van wacht woorden zonder dat ze opnieuw moeten worden geregistreerd door [Power shell te synchroniseren of in te stellen via het alternatieve e-mail adres of mobiele telefoon nummer van een gebruiker](/azure/active-directory/authentication/howto-sspr-authenticationdata). Nadat gebruikers zijn geregistreerd voor de selfservice voor wachtwoord herstel van Azure AD, kan de FIM-Portal voor het opnieuw instellen van wacht woorden buiten gebruik worden gesteld.

Voor klanten die nog geen Azure AD-selfservice voor wachtwoord herstel voor hun gebruikers hebben geïmplementeerd, biedt MIM ook portals voor het opnieuw instellen van wacht woorden via self-service.  Vergeleken met FIM, MIM 2016 bevat de volgende wijzigingen:

- Met de MIM Self-Service-Portal voor wachtwoord herstel en het Windows-aanmeldings scherm kunnen gebruikers hun accounts ontgrendelen zonder hun wacht woord te wijzigen.
- Er is een nieuwe verificatie poort, een telefoon poort, toegevoegd aan MIM. Hierdoor kan gebruikers verificatie via de telefoon oproep via de service Microsoft Azure Multi-Factor Authentication (MFA).

MIM 2016 release bouwt voort op tot versie 4.5.26.0, afhankelijk van de klant, om de Azure Multi-Factor Authentication Software Development Kit (Azure MFA SDK) te downloaden.  Deze SDK is afgeschaft en de Azure MFA SDK wordt alleen voor bestaande klanten ondersteund tot de datum van beëindiging 14 november 2018. Tot die datum moeten klanten contact opnemen met de klant ondersteuning van Azure om het gegenereerde pakket met de verouderde MFA-service te ontvangen, omdat ze de Azure MFA SDK niet kunnen downloaden. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>NIEUW! Huidige configuratie van Azure MFA bijwerken naar Azure Multi-Factor Authentication-server

In dit [artikel](working-with-mfaserver-for-mim.md) wordt beschreven hoe u uw implementatie MIM self-service password reset Portal en Pam-configuratie kunt bijwerken met behulp van Azure multi-factor Authentication-Server voor multi-factor Authentication.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>De MIM Self-Service-Portal voor wachtwoord herstel implementeren met behulp van Azure MFA voor Multi-Factor Authentication

In de volgende sectie wordt beschreven hoe u de portal selfservice voor wachtwoord herstel implementeert met behulp van Azure MFA voor multi-factor Authentication.  Deze stappen zijn alleen nodig voor klanten die geen gebruik maken van Azure AD selfservice voor wachtwoord herstel voor hun gebruikers.

Microsoft Azure Multi-Factor Authentication is een verificatieservice waarbij gebruikers zich bij het aanmelden moeten verifiëren door middel van een mobiele app, telefonische oproep of een tekstbericht. Het kan met Microsoft Azure Active Directory worden gebruikt en als service voor bedrijfstoepassingen in de cloud en on-premises.

Azure MFA biedt een aanvullende verificatiemethode waarmee bestaande verificatieprocessen kunnen worden uitgebreid, zoals de verificatie die wordt uitgevoerd door MIM voor de aanmeldassistent van de selfservice.

Gebruikers verifiëren zich met Azure MFA bij het systeem om hun identiteit kenbaar te maken en tegelijkertijd om opnieuw toegang te krijgen tot hun account en resources. Verificatie kan plaatsvinden via sms of een telefoonoproep.   Hoe strenger de verificatie, hoe groter het vertrouwen dat de persoon die toegang probeert te krijgen inderdaad de werkelijke gebruiker is die houder is van de identiteit. Wanneer de gebruiker eenmaal is geverifieerd, kan deze een nieuw wachtwoord kiezen ter vervanging van het oude.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Vereisten voor het instellen van de selfservicefuncties voor het ontgrendelen van het account en het opnieuw instellen van het wachtwoord met MFA

In deze sectie wordt ervan uitgegaan dat u de implementatie van de Microsoft Identity Manager 2016 [MIM-synchronisatie, de MIM-service en MIM-Portal onderdelen](microsoft-identity-manager-deploy.md)hebt gedownload en voltooid, met inbegrip van de volgende onderdelen en services:

-   Een Windows Server 2008 R2-server of hoger is ingesteld als een Active Directory-server, met inbegrip van AD Domain Services en de domeincontroller met een aangewezen domein (een 'bedrijfsdomein')

-   Er is een groepsbeleid gedefinieerd voor de accountvergrendeling

-   De MIM 2016-synchronisatieservice (Sync) is geïnstalleerd en actief op een server die deel uitmaakt van het AD-domein

-   De MIM 2016-service &amp; -portal, met inbegrip van de SSPR-registratieportal en de SSPR-portal voor het opnieuw instellen van het wachtwoord, zijn geïnstalleerd en actief op een server (kan met de synchronisatieservice op dezelfde server worden geplaatst)

-   MIM Sync is geconfigureerd voor AD MIM- identiteitssynchronisatie, met inbegrip van:

    -   De configuratie van ADMA (Active Directory Management Agent) voor de verbinding met AD DS en voor het importeren en exporteren van identiteitsgegevens vanuit en naar Active Directory.

    -   De configuratie van MIM MA (MIM Management Agent) voor de verbinding met de FIM-servicedatabase en voor het importeren en exporteren van identiteitsgegevens vanuit en naar de FIM-database.

    -   De configuratie van synchronisatieregels in de MIM-portal om de synchronisatie van gebruikersgegevens toe te staan en synchronisatieactiviteiten in de MIM-service mogelijk te maken.

-   MIM 2016-invoegtoepassingen &amp; -uitbreidingen, zoals de geïntegreerde client voor Windows-aanmelding in SSPR die op de server of op een afzonderlijke clientcomputer wordt geïmplementeerd.

Voor dit scenario moet u MIM-Cal's voor uw gebruikers hebben, evenals een abonnement voor Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>MIM voorbereiden voor het toepassen van meervoudige verificatie
Configureer MIM Sync voor de ondersteuning van de functies voor het opnieuw instellen van het wachtwoord en het ontgrendelen van het account. Zie [De FIM-invoegtoepassingen en -uitbreidingen installeren](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [FIM SSPR installeren](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [SSPR-verificatiepoorten](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) en [de handleiding voor de SSPR-testomgeving](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

In de volgende sectie stelt u de Azure MFA-provider in Microsoft Azure Active Directory in. Als onderdeel hiervan genereert u een bestand met de verificatiegegevens die nodig zijn voor MFA om met Azure MFA verbinding te maken.  U moet over een Azure-abonnement beschikken om verder te gaan.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Uw provider voor meervoudige verificatie in Azure registreren

1.  Maak een [MFA-provider](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Open een ondersteunings aanvraag en vraag de directe SDK voor C#ASP.net 2,0. De SDK wordt alleen aan de huidige gebruikers van MIM met MFA door gegeven omdat de direct SDK is afgeschaft. Nieuwe klanten moeten de volgende versie van MIM aannemen die wordt geïntegreerd met MFA server.

3. Kopieer het betreffende ZIP-bestand naar elk systeem waarop de MIM-service is geïnstalleerd.  Houd er rekening mee dat het ZIP-bestand sleutelmateriaal bevat dat wordt gebruikt bij de verificatie voor de Azure MFA-service.

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

4.  Selecteer **Telefoonpoort** of **SMS-gate voor OTP (eenmalig wachtwoord)** , klik op **Selecteren** en vervolgens op **OK**.

Opmerking: als u een Azure MFA-server gebruikt, of een andere provider die het eenmalige wacht woord zelf genereert, moet u ervoor zorgen dat het veld lengte dat hierboven is geconfigureerd dezelfde lengte heeft als de waarde die is gegenereerd door de MFA-provider.  Deze lengte moet 6 zijn voor de Azure MFA-server.  Azure MFA-server genereert ook een eigen bericht tekst, zodat het SMS-bericht wordt genegeerd.

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

5.  ! [MIM AC
6.  aantal ontgrendelde installatie kopieën voor geslaagde items] (Media/MIM-SSPR-account-unlock. JPG)

6.  Als de gebruiker ervoor kiest om het wachtwoord opnieuw in te stellen, moet deze twee keer een nieuw wachtwoord invoeren en op **Volgende** klikken om het wachtwoord te wijzigen.
