---
title: Microsoft Identity Manager 2016 Portal aanpassingen | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016 Portal aanpassen


>[!Warning]
Zorg ervoor dat de browser-cache wissen wanneer CSS aanpassingen worden gemaakt.

In Microsoft Identity Manager 2016 (MIM), kunt u de geselecteerde elementen van de portals wachtwoord aanpassen zoals het logo in banner, tekenreeksresources en trapsgewijze opmaakmodellen.

Hiervoor zijn een aantal zaken vereist, afhankelijk van het niveau van aanpassing. Hier volgt een lijst met items die zijn betrokken bij het aanpassen van de MIM 2016-wachtwoordregistratie en portals opnieuw instellen.

-   Aanpassingen map - dit is de map die MIM 2016 wordt gecontroleerd voordat de standaardwaarden. Elke portal die zal worden aangepast vereist een map aanpassingen. Aanpassingen moeten alleen worden uitgevoerd in deze map omdat setup wordt niet op upgrades overschrijven, modus wordt geïnstalleerd te wijzigen of te modus wordt geïnstalleerd herstellen.

-   Strings.resources - dit is een XML-bestand waarmee u de tekenreeksen die worden weergegeven in de portal wijzigen. Dit bestand moet zich bevinden in de map aanpassingen.

-   Style.CSS – dit is het opmaakmodel dat door de portals worden gebruikt om aan te passen. Dit opmaakmodel moet worden gemaakt en gewijzigd als het logo wilt wijzigen of volledig kan worden vervangen door uw eigen aanpassingen.

Portals Zie voor gedetailleerde stapsgewijze instructies over het aanpassen van de registratie en het wachtwoord opnieuw instellen van wachtwoorden Test Lab Guide: MIM 2016-Wachtwoordregistratie te demonstreren en aanpassing van de Portal opnieuw instellen.

>[! WARGNING] om ervoor te zorgen dat MIM aangepaste wijzigingen kan herkennen, moet u IIS opnieuw iisreset uitvoert.


## <a name="creating-the-customizations-folder"></a>Maken van de map aanpassingen

Bij het opstarten zoekt MIM naar het bestand Strings.resources in de map voor aanpassingen voordat u de standaardinstellingen gebruikt. U moet een map aanpassingen onder de map voor de portal die u aanpassen wilt (dat wil zeggen de Portal voor Wachtwoordregistratie of de Portal voor wachtwoord opnieuw instellen). Als u wilt aanpassen, beide portals moet u een map aanpassingen onder elk van de volgende maken:

-   C:\\programmabestanden\\Microsoft Forefront Identity Manager\\2010\\-Portal voor Wachtwoordregistratie

-   C:\\programmabestanden\\Microsoft Forefront Identity Manager\\2010\\Portal voor wachtwoordherstel

### <a name="to-create-the-customization-folder"></a>Maak de map voor aanpassing

1.  Navigeer naar station C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\map van de Portal voor Wachtwoordregistratie.

2.  Maak een map met de naam aanpassingen.

3.  Navigeer terug één niveau naar de map van de Portal voor wachtwoord opnieuw instellen en maak een map met de naam aanpassingen.

## <a name="customizing-strings"></a>Tekenreeksen aanpassen

Veel van de tekenreeksen in de gebruikersinterface van de portal kunnen worden aangepast door het maken van een bestand Strings.resources en dit bestand naar de map aanpassingen toe te voegen. U moet een bestand Strings.resources voor elke portal die u wilt aanpassen.

### <a name="to-customize-strings"></a>Tekenreeksen aanpassen

1.  Met Kladblok, Kopieer de volgende code hieronder in de App en sla het op de map aanpassingen als Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Onder de `<!-- Customizations begin here -->` sectie Wijzig de Gegevensnaam van de overeenkomen met de tekenreeksen die u wilt aanpassen en voer de waarde voor deze reeks tussen de <value> </value> labels. Zie de onderstaande lijst voor de tekenreeksen die kunnen worden aangepast en hun standaardwaarden.

>[!NOTE]
De **Strings.resources** bestand is taalonafhankelijk. Voor het maken van taal specifieke aangepaste tekenreeksen, moet u die language pack is geïnstalleerd en sla het bestand in de indeling tekenreeksen. `<language>-<culture>.resources`, bijvoorbeeld Strings.en worden geconverteerd in MyResources.

Hier volgt een lijst met portal tekenreeksen die kan worden aangepast.

<a name="portal-strings"></a>Portal tekenreeksen
--------------

| Naam                                                     | Standaardwaarde                                                                                                                                                                                                                                                                                                                                         | Opmerking                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Over         |       |
| ButtonCancel                                             | Annuleren                                                                                 |     |
| ButtonFinish                                             | Voltooien    |    |
| ButtonNext                                               | Volgende    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | Uw sessie is niet meer actief. U kunt het venster sluiten of u kunt opnieuw starten door te klikken op de onderstaande koppeling.         |      |
| CancelFinishedTitle                                      | Sessie is beëindigd                                                              |      |
| ErrorDescription_3000                                    | Er is een fout opgetreden. Probeer het opnieuw, en als het probleem zich blijft voordoen, neem dan contact op met uw helpdesk of systeembeheerder. (Fout 3000)       |          |
| ErrorDescription_3001                                    | Zorg ervoor dat u uw gebruikersnaam correct invoert. Als u het wachtwoord niet herstellen, contact op met uw      |           |
|                                                          | de helpdesk voor ondersteuning. (Fout 3001)   |                   |
| ErrorDescription_3002                                    | Uw sessie is beëindigd. Ga terug naar de startpagina opnieuw te starten. (Fout 3002)                                     |                |
| ErrorDescription_3003                                    | Het huidige gebruikersaccount wordt niet herkend door de Forefront Identity Manager.Please Neem contact op met uw helpdesk of systeembeheerder. (Fout 3003)           |               |
| ErrorDescription_3004                                    | U bent niet gemachtigd om te registreren voor wachtwoordherstel. Neem contact op met uw helpdesk of systeembeheerder. (Fout 3004)     |              |
| ErrorDescription_3005                                    | Een of meer opgegeven antwoorden komen niet overeen met de antwoorden die u hebt opgegeven tijdens wachtwoord Registration.In om uw wachtwoord opnieuw instellen, de antwoorden die u nu opgeeft, moeten overeenkomen met de antwoorden die u hebt opgegeven bij de registratie. U kunt opnieuw beginnen vanaf de startpagina of neem contact op met uw helpdesk of systeembeheerder. (Fout 3005) |           |
| ErrorDescription_3006                                    | Het ingevoerde wachtwoord is onjuist. U moet het juiste wachtwoord invoeren om te registreren voor wachtwoord opnieuw instellen. (Fout 3006)            |              |
| ErrorDescription_3007                                    | U tijdelijk mogen niet uw wachtwoord opnieuw instellen. Probeer het later opnieuw of neem contact op met uw helpdesk of systeembeheerder voor ondersteuning. (Fout 3007)     |         |
| ErrorDescription_3008                                    | Er is een fout opgetreden. Probeer het opnieuw, en als het probleem zich blijft voordoen, neem dan contact op met uw helpdesk of systeembeheerder. (Fout 3008)        |          |
| ErrorDescription_3009                                    | Uw invoer bevat tekst in een indeling die niet is toegestaan. Probeer het opnieuw met andere invoer of neem contact op met uw helpdesk of systeembeheerder. (Fout 3009)     |             |
| ErrorDescription_3010_Registration                       | Uitvoeren van scripts is niet ingeschakeld in uw browser. Schakel scripting en terug naar de startpagina voor Wachtwoordregistratie of neem contact op met uw helpdesk voor hulp.      |               |
| ErrorDescription_3010_Reset                              | Uitvoeren van scripts is niet ingeschakeld in uw browser. Scripts inschakelen en terugkeren naar de startpagina voor wachtwoord opnieuw instellen, of neem contact op met uw helpdesk voor hulp.     |            |
| ErrorDescription_3011                                    | Deze site worden cookies gebruikt. Configureer de browser cookies accepteert en probeer het opnieuw of neem contact op met uw helpdesk voor hulp.          |                                           |
| ErrorDescription_3012                                    | De gegevens die u hebt ingevoerd komt niet overeen met de beveiligingscode die aan u is toegezonden. U kunt proberen uw wachtwoord opnieuw instellen of neem contact op met uw helpdesk voor hulp.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Kan geen beveiligingscode verzenden. Neem contact op met uw helpdesk voor hulp.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Voer uw gebruikersnaam in de juiste indeling.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Geef een gebruikersnaam op om door te gaan.                                              |                         |
| ErrorMessagePasswordRequired                             | Voer een wachtwoord.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Zorg ervoor dat beide wachtwoorden overeen.                                |           |
| ErrorPageDefaultHeading                                  | Toepassingsfout                                            |               |
| ErrorPageServerTime                                      | Servertijd: {0: t}                     | {0} is de tijd waarop de uitzondering is opgetreden. De t ' zorgt ervoor dat de tijd doorgegeven worden ingedeeld als een 'lange tijd'. Deze uiteindelijk wordt weergegeven met het uur, minuut en seconde en mogelijk de aanwijzing AM/PM (afhankelijk van de huidige cultuur). |
| ErrorPageTitle                                           | Forefront Identity Management - Wachtwoordfout                     |   |
| ErrorTitle_3000                                          | Fout                                  |  |
| ErrorTitle_3001                                          | Toegang geweigerd                          |  |
| ErrorTitle_3002                                          | Sessie is beëindigd                          |  |
| ErrorTitle_3003                                          | Niet-herkende gebruiker                      |  |
| ErrorTitle_3004                                          | Niet-gemachtigde gebruiker                      |  |
| ErrorTitle_3005                                          | Antwoorden komen niet overeen                    |  |
| ErrorTitle_3006                                          | Onjuist wachtwoord                     |  |  
| ErrorTitle_3007                                          | Toegang is tijdelijk geweigerd              |  |
| ErrorTitle_3008                                          | Fout met agentcommunicatie                    |  |
| ErrorTitle_3009                                          | Niet-toegestane invoer                       |  |
| ErrorTitle_3010                                          | Configuratiefout van de browser            |  |
| ErrorTitle_3011                                          | Configuratiefout van de browser            |  |
| ErrorTitle_3012                                          | Verificatie is mislukt                    |  |
| ErrorTitle_3013                                          | Kan geen beveiligingscode verzenden   |  |
| FinalizeRegistrationHeading1                             | Als u ooit moet uw wachtwoord opnieuw instellen:        |   |
| FinalizeRegistrationSubHeading1                          | Ga naar de portal voor wachtwoordherstel   |   |
| FinalizeRegistrationSubHeading2                          | Uw identiteit verifiëren   |   |
| FinalizeRegistrationSubHeading3                          | Een nieuw wachtwoord kiezen    |     |
| FinishingDescription                                     | Een nieuw wachtwoord kiezen        |    |
| FinishingTitle                                           | Wachtwoord opnieuw instellen:      |     |
| GotoPortalPrefix                                         | Ga naar     |        |
| GotoPortalSuffix                                         | startpagina     |      |
| LabelTroubleshootingLinkText                             | Details weergeven           |    |
| LoadingText                                              | Laden...     |  |
| NoScriptTagErrorMessage                                  | Uitvoeren van scripts is niet ingeschakeld in uw browser. Schakel scripting en terug naar de startpagina of neem contact op met uw helpdesk voor hulp.      |   |
| PasswordResetOperationGeneralErrorMessage                | Fout bij poging wachtwoord opnieuw instellen.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | Het wachtwoord voldoet niet aan het wachtwoordbeleid van uw organisatie.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Fout bij het wachtwoord opnieuw instellen, de gebruiker kan het wachtwoord niet wijzigen.    |   |
| PrivacyStatement                                         | Privacyverklaring                                                      |    |
| RegistrationDescription                                  | Registratie van de selfservice voor wachtwoordherstel     |    |
| RegistrationMission                                      | Als u ooit uw wachtwoord vergeet, u kunt het zelf opnieuw instellen zonder het aanroepen van de helpdesk.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - Wachtwoordregistratie                                          |   |
| RegistrationSteps                                        | Klik op 'Volgende' om te beginnen met het registratieproces.   |      |
| RegistrationSuccessDescription                           | U bent nu geregistreerd        |   |
| RegistrationSuccessTitle                                 | Voltooid:   |    |
| RegistrationWelcomeTitle                                 | Wachtwoordregistratie van:    | |
| ResetDescription                                         | Selfservice voor wachtwoordherstel  |    |
| ResetEnterNamePrompt                                     | Voer uw gebruikersnaam hieronder     |     |
| ResetEnterPassword                                       | Een nieuw wachtwoord invoeren:                                                  |   |
| ResetExample1                                            | Contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Voorbeelden:  |    |
| ResetPageTitle                                           | Forefront Identity Management - wachtwoord opnieuw instellen       |     |
| ResetReenterPassword                                     | Voer het wachtwoord:    | |
| ResetSuccessDescription                                  | Uw wachtwoord is opnieuw ingesteld    |    |
| ResetSuccessTitle                                        | Voltooid:                                |    |
| ResetUseNewPassword                                      | U kunt nu uw nieuwe wachtwoord gebruiken om aan te melden.     |      |
| ResetUsernameTextFormat                                  | (Wachtwoord opnieuw instellen voor {0})       | {0} is aanmelding van de gebruiker             |
| ResetWelcomeTitle                                        | Wachtwoord opnieuw instellen:     |      |
| TroubleshootingEmailSubject                              | Details voor verwerkingsfout voor FIM-aanvraag     |       |
| TroubleshootingLabelAttributes                           | Kenmerken:    |    |
| TroubleshootingLabelCloseButton                          | Sluiten       |    |
| TroubleshootingLabelCopyToClipboard                      | Naar klembord kopiëren     |     |
| TroubleshootingLabelCorrelationId                        | Correlatie-Id:      |          |
| TroubleshootingLabelDetails                              | Details:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | De informatie is naar het Klembord gekopieerd.           |    |
| TroubleshootingLabelRequestId                            | Aanvraag-Id:                  |    |
| TroubleshootingLabelSendEmail                            | Informatie per E-mail verzenden                            |    |
| TroubleshootingLabelSource                               | Reden:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Details van de aanvraag weergeven        |              |                                                    
| TroubleshootingLinkText                                  | Informatie over probleemoplossing          |                  | |



Hier volgt een lijst met tekenreeksen verificatiepoort die kan worden aangepast.

<a name="authentication-gate-strings"></a>Verificatie-Gate tekenreeksen
---------------------------

| Naam                                          | Standaardwaarde                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | E-mailadres:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | Het veld voor e-mailadres mag niet leeg zijn.     |          |
| OTPEmailRegistrationFooterReadOnly            | Volg het door uw organisatie bepaalde proces voor het bijwerken van uw e-mailadres of de helpdesk bellen.                   |          |
| OTPEmailRegistrationFooterReadWrite           | Het e-mailadres wordt opgeslagen door uw organisatie in Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | E-mailadres verificatie   |          |
| OTPEmailRegistrationHeaderReadOnly            | Als u ooit moet uw wachtwoord opnieuw instellen, wordt ter verificatie een beveiligingscode verzonden naar uw e-mail. Als het e-mailadres hieronder niet juist is, moet u dit bijwerken om te kunnen gebruiken van de selfservice voor wachtwoordherstel. |          |
| OTPEmailRegistrationHeaderReadWrite           | Voer hieronder uw e-mailadres. Als u ooit moet uw wachtwoord opnieuw instellen, wordt een verificatiecode naar uw e-mail worden verzonden.                 |          |
| OTPEmailResetGateTitle                        | Uw identiteit verifiëren: e-mailadres         |          |
| OTPEmailResetHeader                           | Voer uw beveiligingscode hieronder. Een beveiligingscode verzonden naar e-mailadres voor deze organisatie is geregistreerd.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | De opgegeven waarde komt niet overeen met de verwachte indeling.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | Veld voor de beveiligingscode mag niet leeg zijn.        |          |
| OTPResetVerificationLabel                     | Beveiligingscode:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Volg het door uw organisatie bepaalde proces voor het bijwerken van uw mobiele telefoonnummer of de helpdesk bellen.   ||
| OTPSmsRegistrationFooterReadWrite                     | Het mobiele telefoonnummer is door uw organisatie in Forefront Identity Manager opgeslagen.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Verificatie van mobiele telefoon                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Als u ooit moet uw wachtwoord opnieuw instellen, wordt ter verificatie een beveiligingscode verzonden naar uw mobiele telefoon. Als het mobiele telefoonnummer hieronder niet juist is, moet u dit bijwerken om te kunnen gebruiken van de selfservice voor wachtwoordherstel. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Voer uw mobiele telefoonnummer-hieronder. Als u ooit moet uw wachtwoord opnieuw instellen, wordt een verificatiecode naar uw mobiele telefoon worden verzonden.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Veld voor het mobiele telefoonnummer mag niet leeg zijn.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobiele telefoon:                    |   |
| OTPSmsResetGateTitle                                  | Uw identiteit verifiëren: mobiele telefoon         |   |
| OTPSmsResetHeader                                     | Voer uw beveiligingscode hieronder. Een beveiligingscode verzonden naar de mobiele telefoon voor deze organisatie is geregistreerd.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Voer hieronder uw huidige wachtwoord en klik op 'Volgende'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Voer uw huidige wachtwoord.                  |   |
| PasswordGateGateTitle                                 | Uw huidige wachtwoord               |   |
| PasswordGatePasswordLabelText                         | Wachtwoord:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(aangemeld als:`<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | U moet ten minste beantwoorden {0} vragen.        |   |
| QAGateIncorrectAnswer                                 | Uw antwoorden zijn niet juist.       |   |
| QAGatePrivacyNotice                                   | Antwoorden worden opgeslagen door uw organisatie in Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | U moet ten minste beantwoorden {0} vragen om te registreren.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Een of meer antwoorden voldoen niet aan het beleid.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Dit antwoord voldoet niet aan het beleid.      |   |
| QAGateRegistrationTitle                               | Registreren van uw antwoorden         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | U moet {0} van de volgende {1} vragen beantwoorden.   |   |
| QAGateResetTitle                                      | Uw identiteit verifiëren: Uw antwoorden verzenden                                  |   |



## <a name="customizing-the-logo-banner"></a>De banner logo aanpassen

De banner standaard op de portal-pagina's kan worden aangepast voor uw organisatie.
De banner logo aanpassen:

1.  Uw aangepaste banner maken en deze opslaan als PNG-bestanden. De bestanden moeten voldoen aan de volgende aanbevelingen:

 - Grootte: 490 X 50 pixels.

 - Bitdiepte: 32

2.  Kopieer de bestanden naar de \\aanpassingen map in elke portal die u wilt aanpassen.

3.  Maak een bestand Style.css in elke map. Deze zijn verwijst naar de map aanpassingen en het nieuwe logo... U kunt de naam van het logo wijzigen indien van toepassing (dat wil zeggen /Customizations/contosologo.png). De code dient er als volgt uit:

   **Voorbeeld 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Als u Internet Explorer 6.0 gebruikt, moet u een alternatieve ondoorzichtige logo bieden en voeg de volgende code toe Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Voorbeeld 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Als u Internet Explorer 6.0 gebruikt, moet u een alternatieve ondoorzichtige logo bieden en voeg de volgende code toe Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Afbeelding aanpassen voor SmartPhones

U kunt de installatiekopie voor SmartPhones aanpassen met behulp van de volgende. Voor het aanpassen van de installatiekopie van het SmartPhone:

1.  Uw afbeelding maken en deze opslaan als PNG-bestanden. De bestanden moeten voldoen aan de volgende aanbevelingen:

    - Grootte: 190 X 50 pixels.
    - Bitdiepte: 32

2.  Kopieer de bestanden naar de \\aanpassingen map in elke portal die u wilt aanpassen.

2.  Maak een bestand Style.css in elke map. Deze zijn verwijst naar de map aanpassingen en het nieuwe logo... U kunt de naam van het logo wijzigen indien van toepassing (dat wil zeggen /Customizations/contosologo.png). De code dient er als volgt uit:

  **Voorbeeld 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>StyleSheets aanpassen

U kunt de indeling en de stijl van de portals voor wachtwoord wijzigen met behulp van een aangepaste opmaakmodel (CSS). Een aangepaste CSS gebruiken:

1.  Uw aangepaste CSS-bestanden maken en deze opslaan als Style.css.

2.  Kopieer de bestanden naar de \\aanpassingen map in elke portal die u wilt aanpassen.

Hier volgt een voorbeeld van een basis van een bestand Style.css:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Om ervoor te zorgen dat MIM aangepaste wijzigingen kan herkennen, moet u IIS opnieuw starten door iisreset uitvoert.                                                                                                                                                                                       

Hier volgt een voorbeeld van een meer geavanceerde van een bestand Style.css. Dit bestand bevat smartphone en ipad specifieke informatie voor het weergeven van de portals op deze apparaten.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Veelvoorkomende problemen voor aanpassing

De volgende tabel wordt een lijst met bekende problemen die zich voordoen kunnen met het bijwerken van de FIM-Service en de Portal. Deze tabel bevat ook de oplossingen voor dit probleem

|Probleem |Oplossing                                                                    |
|------|------------------------------|
| Ik heb een string-aanpassing maar het is niet doorgevoerd in de gebruikersinterface         | Tekenreeks aanpassingen in strings.resources moeten altijd iisreset         |
| Nadat u een wijziging strings.resources, zie ik mijn tekenreeks wijzigingen meer     | Strings.resources indeling is waarschijnlijk niet goed gevormd en daarom wordt genegeerd door de portal. Controleer het gebeurtenislogboek onder Windows Logboeken-Logboeken voor toepassingen en Services – Forefront Identity Manager                        |
| De eerste keer dat ik toegevoegd Style.css, heeft geen Zie ik deze stijlwijzigingen in de portal            | De eerste keer dat u een bestand Style.css introduceren die u wilt een iisreset uitvoeren     |
| Nieuwe stijlen worden toegevoegd/gewijzigd in Style.css maar wijzigingen zijn niet zichtbaar in de browser      | De browser-cache wissen en vernieuw de pagina <br/> Controleer de syntaxis van CSS    |
| Ik heb de inhoud van de map CSS in path_to_sspr_portal rechtstreeks gewijzigd\\css\\\*CSS of het logo in banner in path_to_sspr_portal\\installatiekopieën\\fimlogo.png en deze wijzigingen bij een upgrade verloren | U moet deze bestanden nooit beginnen wijzigen. De map aanpassing als een methode alleen gebruiken voor dat een logo in banner en CSS-stijl aanpassingen in Style.css. Aanpassingen map opzettelijk niet wordt overschreven door belangrijke upgrades <br/><br/>Probeer niet gebruikmaken van hulpprogramma's zoals ILSpy en Routereflector tekenreeksen in de portal assembly's te wijzigen. Gebruik strings.resources standaardtekenreeksen overschrijven. De assembly's worden vervangen bij een upgrade  |
| Logo in banner wordt niet weergegeven in de portals / nog steeds het FIM-logo     | Naam-/ installatiekopiepad in Style.css is niet geldig of de browser-cache is niet gewist          |
| Logo in banner lijkt lelijke in IE6       | U moet een niet-transparante afbeelding bieden voor IE6 en een speciaal bijbehorend opmaakprofiel in style.css        |
