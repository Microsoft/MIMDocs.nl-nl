---
title: Azure MFA gebruiken om PAM te activeren | Microsoft Docs
description: Azure MFA instellen als een tweede beveiligingslaag wanneer uw gebruikers rollen in Privileged Access Management activeren.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b937b30da2dff9bbfeabf7dceb43fcaca99a1b63
ms.contentlocale: nl-nl
ms.lasthandoff: 07/10/2017


---

# Azure MFA gebruiken voor activering
<a id="using-azure-mfa-for-activation" class="xliff"></a>
Wanneer u een PAM-rol configureert, kunt u hoe gebruikers die een aanvragen voor activering van de rol verzenden, moeten worden geautoriseerd. Met de PAM-autorisatieactiviteit worden de volgende keuzen geïmplementeerd:

- Goedkeuring van roleigenaar
- Azure Multi-Factor Authentication (MFA)

Als geen van beide opties is ingeschakeld, worden kandidaatgebruikers automatisch geactiveerd voor de betreffende rol.

Microsoft Azure Multi-Factor Authentication (MFA) is een verificatieservice waarbij gebruikers zich bij het aanmelden moeten verifiëren via een mobiele app, telefonische oproep of een tekstbericht. De service is beschikbaar voor gebruik met Microsoft Azure Active Directory en kan worden ingezet als service voor bedrijfstoepassingen in de cloud en on-premises. Voor het PAM-scenario biedt Azure MFA een aanvullende verificatiemethode die kan worden gebruikt voor autorisatie, ongeacht hoe een kandidaatgebruiker eerder is geverifieerd voor Windows PRIV-domein.

## Vereisten
<a id="prerequisites" class="xliff"></a>

Als u Azure MFA met MIM wilt gebruiken, hebt u het volgende nodig:

- Internettoegang voor elke MIM-service waarmee PAM wordt geleverd voor de communicatie met Azure MFA
- Een Azure-abonnement
- Azure Active Directory Premium-licenties voor kandidaatgebruikers of een alternatieve methode voor Azure MFA-licentieverlening
- Telefoonnummers voor alle kandidaatgebruikers

## Een Azure MFA-provider maken
<a id="creating-an-azure-mfa-provider" class="xliff"></a>

In deze sectie stelt u de Azure MFA-provider in Microsoft Azure Active Directory in.  Als u Azure MFA al gebruikt, als zelfstandige functie of geconfigureerd met Azure Active Directory Premium, gaat u door naar de volgende sectie.

1.  Open een webbrowser en maak verbinding met de [klassieke Azure Portal](https://manage.windowsazure.com) als een Azure-abonnementsbeheerder.

2.  Klik in de hoek linksonder op **Nieuw**.

3.  Klik op **App-services > Active Directory > Provider voor Multi-Factor Authentication > Snelle invoer**.

4.  Voer in het veld **Naam** de waarde **PAM** in en selecteer in het veld voor gebruiksmodel de waarde Per ingeschakelde gebruiker. Als u al een Azure AD-adreslijst hebt, selecteert u deze. Klik tot slot op **Maken**.

## De referenties voor de Azure MFA-service downloaden
<a id="downloading-the-azure-mfa-service-credentials" class="xliff"></a>

Vervolgens gaat u een bestand genereren met de verificatiegegevens die nodig zijn voor PAM om met Azure MFA verbinding te maken.

1. Open een webbrowser en maak verbinding met de [klassieke Azure Portal](https://manage.windowsazure.com) als een Azure-abonnementsbeheerder.

2.  Klik op **Active Directory** in het menu van Azure Portal en klik vervolgens op het tabblad **Providers voor Multi-Factor Authentication**.

3.  Klik op de Azure MFA-provider die u gaat gebruiken voor PAM en klik vervolgens op **Beheren**.

4.  Klik in het nieuwe venster in het linkerdeelvenster onder **Configureren** op **Instellingen**.

5.  Klik in het venster **Azure Multi-Factor Authentication** op **SDK** bij **Downloads**.

6.  Klik op de koppeling **Downloaden** in de kolom met ZIP-bestanden voor het bestand met de programmeertaal **SDK voor ASP.net 2.0 C\#**.

![Multi-Factor Authentication SDK downloaden - schermafbeelding](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Kopieer het betreffende ZIP-bestand naar elk systeem waarop de MIM-service is geïnstalleerd. 

>[!NOTE]
> Het ZIP-bestand bevat sleutelmateriaal dat wordt gebruikt bij de verificatie voor de Azure MFA-service.

## De MIM-service voor Azure MFA configureren
<a id="configuring-the-mim-service-for-azure-mfa" class="xliff"></a>

1.  Meld u op de computer waarop de MIM-service is geïnstalleerd, aan als een beheerder of als de gebruiker die MIM heeft geïnstalleerd.

2.  Maak een nieuwe map in de map waar de MIM-service is geïnstalleerd, bijvoorbeeld `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`.

3.  Navigeer in Windows Verkenner naar de map **pf\\certs** van het ZIP-bestand dat in de vorige sectie is gedownload en kopieer het bestand **cert\_key.p12** naar de nieuwe map.

4.  Navigeer in Windows Verkenner naar de map **pf** van het ZIP-bestand en open het bestand **pf\_auth.cs** in een teksteditor zoals Wordpad.

5.  Zoek de volgende drie parameters op: **LICENSE\_KEY**, **GROUP\_KEY**, **CERT\_PASSWORD**.

![Waarden kopiëren uit het bestand pf\_auth.cs - schermafbeelding](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  Open in Kladblok het bestand **MfaSettings.xml** dat zich in `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` bevindt.

7.  Kopieer de waarden van de parameters LICENSE\_KEY, GROUP\_KEY en CERT\_PASSWORD in het bestand pf\_auth.cs file naar de overeenkomstige XML-elementen in het bestand MfaSettings.xml.

8.  Geef in het XML-element **<CertFilePath>** de volledige padnaam op van het bestand cert\_key.p12 dat u eerder hebt uitgepakt.

9.  Voer in het element **<username>** een gebruikersnaam in.

10.  Voer in het element **<DefaultCountryCode>** de landcode in voor het bellen van uw gebruikers, zoals 1 voor de Verenigde Staten en Canada. Deze waarde wordt gebruikt als gebruikers zijn geregistreerd met telefoonnummers zonder landcode. Als het telefoonnummer van de gebruiker een internationale landcode heeft die verschilt van de landcode die is geconfigureerd voor de organisatie, moet die landcode worden opgenomen in het telefoonnummer dat wordt geregistreerd.

11.  Sla het bestand **MfaSettings.xml** in de map `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` van de MIM-service op om het bestand te overschrijven. 

> [!NOTE]
> Controleer aan het einde van het proces of het bestand **MfaSettings.xml** of kopieën hiervan, of het ZIP-bestand niet openbaar leesbaar zijn.

## PAM-gebruikers configureren voor Azure MFA
<a id="configure-pam-users-for-azure-mfa" class="xliff"></a>

Als een gebruiker een rol wilt activeren waarvoor Azure MFA is vereist, moet het telefoonnummer van de gebruiker zijn opgeslagen in MIM. Dit kenmerk kan op twee manieren worden ingesteld.

Ten eerste kan met de opdracht `New-PAMUser` een telefoonnummerkenmerk in de adreslijstvermelding voor de gebruiker in het CORP-domein naar de database van de MIM-service worden gekopieerd. Dit is een eenmalige bewerking.

Daarnaast kan met de opdracht `Set-PAMUser` het telefoonnummerkenmerk worden bijgewerkt in de database van de MIM-service. Zo wordt met de volgende opdracht het telefoonnummer van een bestaande PAM-gebruiker vervangen in de MIM-service. De adreslijstvermelding blijft ongewijzigd.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## PAM-rollen configureren voor Azure MFA
<a id="configure-pam-roles-for-azure-mfa" class="xliff"></a>

Nadat de telefoonnummers van alle kandidaatgebruikers voor een PAM-rol zijn opgeslagen in de database van de MIM-service, kan de rol worden geconfigureerd voor het vereisen van Azure MFA. Hiervoor kunt u de opdracht `New-PAMRole` of `Set-PAMRole` gebruiken. Bijvoorbeeld:

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

U kunt Azure MFA uitschakelen voor een rol door de parameter -MFAEnabled 0 op te geven in de opdracht `Set-PAMRole`.

## Probleemoplossing
<a id="troubleshooting" class="xliff"></a>

De volgende gebeurtenissen vindt u in het gebeurtenislogboek voor Privileged Access Management:

| Id  | Ernst | Gegenereerd door | Beschrijving |
|-----|----------|--------------|-------------|
| 101 | Fout       | MIM-service            | De gebruiker heeft Azure MFA niet voltooid (heeft bijvoorbeeld de telefoonoproep niet beantwoord) |
| 103 | Informatie | MIM-service            | De gebruiker heeft Azure MFA voltooid tijdens de activering                       |
| 825 | Waarschuwing     | Service voor PAM-controle | Het telefoonnummer is gewijzigd                                |

U kunt ook een rapport van Azure MFA weergeven of downloaden voor meer informatie over mislukte telefoonoproepen (gebeurtenis 101).

1.  Open een webbrowser en maak verbinding met de [klassieke Azure Portal](https://manage.windowsazure.com) als een globale beheerder van Azure AD.

2.  Selecteer **Active Directory** in het menu van Azure Portal en selecteer vervolgens het tabblad **Providers voor Multi-Factor Authentication**.

3.  Selecteer de Azure MFA-provider die u gaat gebruiken voor PAM en klik vervolgens op **Beheren**.

4.  Klik in het nieuwe venster op **Gebruik** en vervolgens op **Gebruikersdetails**.

5.  Selecteer het tijdsbereik en schakel het selectievakje in naast **Naam** in de extra rapportkolom. Klik op **Exporteren naar CSV**.

6.  Nadat het rapport is gegenereerd, kunt u het weergeven in de portal of als een CSV-bestand downloaden wanneer het een uitgebreid MFA-rapport is. De waarde voor **SDK** in de kolom **AUTH TYPE** geven de rijen aan die relevant zijn als PAM-activeringsaanvragen: dit zijn de gebeurtenissen die afkomstig zijn uit MIM of andere on-premises software. Het veld **USERNAME** bevat de GUID van het gebruikersobject in de database van de MIM-service. Als een aanroep is mislukt, bevat de kolom **AUTHD** de waarde **No** en worden in de kolom **CALL RESULT** de details van de fout weergegeven.

