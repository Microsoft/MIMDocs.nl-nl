---
title: Aanpassingen aan de Microsoft Identity Manager 2016-Portal | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 144ce2344327f16f60269b4f505c30fd470e46da
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758905"
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Aanpassing van Microsoft Identity Manager 2016-Portal

In Microsoft Identity Manager 2016 (MIM) kunt u geselecteerde elementen van de wachtwoord portals aanpassen, met inbegrip van het banner logo, de teken reeks resources en de trapsgewijze opmaak modellen.

>[!WARNING]
>De browser cache altijd wissen wanneer CSS-aanpassingen worden aangebracht.

De volgende items worden gebruikt voor het aanpassen van de MIM 2016-wachtwoord registratie en het opnieuw instellen van portals:

- Aangepaste map: dit is de map die door MIM 2016 wordt gecontroleerd voordat de standaard instellingen worden gebruikt. Voor elke portal die moet worden aangepast, is een map aanpassingen vereist. Aanpassingen moeten alleen worden uitgevoerd in deze map als het installatie proces deze map niet overschrijft op upgrades, de installatie van gewijzigde modus of de herstel modus wordt geïnstalleerd.

- Teken reeksen. resources: dit is een XML-bestand waarmee u de teken reeksen kunt wijzigen die in de portal worden weer gegeven. Dit bestand moet zich bevinden in de map met aanpassingen.

- Style. CSS: dit is het trapsgewijze opmaak model dat wordt gebruikt door de portals voor aanpassing. Maak en wijzig dit opmaak model om het logo te wijzigen. U kunt ook de volledige inhoud van het opmaak model vervangen door uw eigen aanpassingen.

Voor gedetailleerde stapsgewijze instructies voor het aanpassen van de wachtwoord registratie en portals voor het opnieuw instellen van wacht woorden, zie test lab Guide: Demonstring van MIM 2016-wachtwoord registratie en het opnieuw instellen van het aanpassings programma.

>[!WARNING]
>Voor MIM om aangepaste wijzigingen te herkennen, moet u IIS opnieuw starten door uit te voeren `iisreset` .


## <a name="customizations-folder"></a>Aangepaste map

Bij het opstarten zoekt MIM naar het bestand strings. resources in de map aanpassingen voordat de standaard instellingen worden gebruikt. Maak een map aanpassingen in de map voor de portal die u wilt aanpassen (dat wil zeggen, de portal voor wachtwoord registratie of het opnieuw instellen van wacht woorden). Als u beide portals wilt aanpassen, maakt u een map aanpassingen op elk van de volgende locaties:

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`

Een aangepaste map maken voor de portal voor wachtwoord registratie:

1. Blader naar de map `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`.
   
2. Maak een map met de naam **aanpassingen** .

Een aangepaste map maken voor de portal voor het opnieuw instellen van wacht woorden:

1. Blader naar de map `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`.
   
2. Maak een map met de naam **aanpassingen** .


## <a name="custom-strings-in-the-stringsresources-file"></a>Aangepaste teken reeksen in het bestand strings. resources

Veel van de teken reeksen in de gebruikers interface van de portal kunnen worden aangepast door het maken van een teken reeks. resources-bestand en het toevoegen van dit bestand aan de map aanpassingen. Maak een strings. resources-bestand voor elke portal die u wilt aanpassen.

Aangepaste teken reeksen maken in het teken reeks. resources-bestand:

1. Kopieer de volgende code in Klad blok en plak deze in het bestand strings. resources. Sla het bestand op in de map aanpassingen voor de portal.

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

2.  `<!-- Customizations begin here -->`Wijzig de naam van de gegevens in de sectie in de code zodat deze overeenkomt met de teken reeksen die u wilt aanpassen. Voer de waarde voor de teken reeks tussen de `<value></value>` Tags in. Zie de volgende secties voor de teken reeksen die kunnen worden aangepast en hun standaard waarden.

>[!NOTE]
>Het bestand strings. resources is taal onafhankelijk. Als u taalspecifieke aangepaste teken reeksen wilt maken, moet u dat taal pakket hebben geïnstalleerd en het bestand opslaan in de teken reeks indeling `<language>-<culture>.resources` . Bijvoorbeeld: voor de Nederlandse taal cultuur is de bestands naam **teken reeksen. nl-nl. resources** .


## <a name="portal-strings"></a>Portal teken reeksen
In de volgende tabel ziet u de portal teken reeksen die kunnen worden aangepast:

<!-- The default values are actual UI strings and should not copy edited. -->

| Teken reeks naam van portal | Standaardwaarde |
|---|---|
| AboutLinkText                                            | Info  |
| ButtonCancel                                             | Annuleren |
| ButtonFinish                                             | Voltooien |
| ButtonNext                                               | Volgende   |
| ButtonOk                                                 | OK     |
| CancelFinishedMessage                                    | Uw sessie is niet meer actief. U kunt het venster sluiten of opnieuw starten door op de onderstaande koppeling te klikken. |
| CancelFinishedTitle                                      | Sessie is beëindigd |
| ErrorDescription_3000                                    | Er is een fout opgetreden. Probeer het opnieuw. Neem contact op met de Help Desk of systeem beheerder als het probleem zich blijft voordoen. (Fout 3000) |
| ErrorDescription_3001                                    | Zorg ervoor dat u de juiste gebruikers naam opgeeft. Als u uw wacht woord nog steeds niet opnieuw kunt instellen, neemt u contact op met de Help Desk voor ondersteuning. (Fout 3001) |
| ErrorDescription_3002                                    | Uw sessie is beëindigd. Ga terug naar de start pagina om opnieuw te starten. (Fout 3002) |
| ErrorDescription_3003                                    | Het huidige gebruikers account wordt niet herkend door Forefront Identity Manager. Neem contact op met de Help Desk of systeem beheerder. (Fout 3003) |
| ErrorDescription_3004                                    | U bent niet gemachtigd om u te registreren voor het opnieuw instellen van een wacht woord. Neem contact op met de Help Desk of systeem beheerder. (Fout 3004) |
| ErrorDescription_3005                                    | Een of meer antwoorden die u hebt opgegeven, komen niet overeen met de antwoorden die u hebt opgegeven tijdens de wachtwoord registratie. Als u uw wacht woord opnieuw wilt instellen, moeten de antwoorden die u nu opgeeft, overeenkomen met de antwoorden die u hebt opgegeven tijdens de registratie. U kunt opnieuw starten vanaf de start pagina of contact opnemen met de Help Desk of systeem beheerder. (Fout 3005) |
| ErrorDescription_3006                                    | Het ingevoerde wacht woord is onjuist. U moet het juiste wacht woord invoeren om u te registreren voor het opnieuw instellen van het wacht woord. (Fout 3006) |
| ErrorDescription_3007                                    | Het is tijdelijk niet toegestaan om uw wacht woord opnieuw in te stellen. Probeer het later opnieuw of neem contact op met de Help Desk of systeem beheerder voor hulp. (Fout 3007) |
| ErrorDescription_3008                                    | Er is een fout opgetreden. Probeer het opnieuw. Neem contact op met de Help Desk of systeem beheerder als het probleem zich blijft voordoen. (Fout 3008) |
| ErrorDescription_3009                                    | De invoer bevat tekst in een indeling die niet is toegestaan. Probeer het opnieuw met een andere invoer, of neem contact op met de Help Desk of systeem beheerder. (Fout 3009) |
| ErrorDescription_3010_Registration                       | Scripting is niet ingeschakeld in uw browser. Schakel scripting in en ga terug naar de start pagina voor wachtwoord registratie of neem contact op met de Help Desk voor ondersteuning. |
| ErrorDescription_3010_Reset                              | Scripting is niet ingeschakeld in uw browser. Schakel scripting in en ga terug naar de start pagina voor het opnieuw instellen van het wacht woord of neem contact op met de Help Desk voor ondersteuning. |
| ErrorDescription_3011                                    | Deze site maakt gebruik van cookies. Configureer uw browser zo dat cookies worden geaccepteerd en probeer het opnieuw, of neem contact op met de Help Desk voor ondersteuning. |
| ErrorDescription_3012                                    | De gegevens die u hebt ingevoerd, komen niet overeen met de beveiligings code die u hebt ontvangen. U kunt proberen om uw wacht woord opnieuw in te stellen of neem contact op met de Help Desk voor ondersteuning.  |
| ErrorDescription_3013                                    | Kan geen beveiligings code verzenden. Neem contact op met de Help Desk voor ondersteuning. |
| ErrorMessageDomainUsernameFormat                         | Voer uw gebruikers naam in met de juiste indeling. |
| ErrorMessageDomainUsernameRequired                       | Voer een gebruikers naam in om door te gaan. |
| ErrorMessagePasswordRequired                             | Voer een wachtwoord in. |
| ErrorMessagePasswordsDoNotMatch                          | Zorg ervoor dat beide wacht woorden overeenkomen. |
| ErrorPageDefaultHeading                                  | Toepassings fout <br/>**Opmerking** : de kop wordt gevolgd door ' = ' en het fout bericht.
| ErrorPageServerTime                                      | Server tijd: {0:T} <br/>**Opmerking** : {0} is het tijdstip waarop de uitzonde ring is aangetroffen. 'T ' zorgt ervoor dat de door gegeven tijd wordt opgemaakt als een lange tijd, waarbij het uur, de minuut en de seconde worden weer gegeven. De AM/PM-aanduiding wordt ook weer gegeven, afhankelijk van de huidige cultuur. |
| ErrorPageTitle                                           | Forefront Identity Management-wachtwoord fout | 
| ErrorTitle_3000                                          | Fout |
| ErrorTitle_3001                                          | Toegang is geweigerd |
| ErrorTitle_3002                                          | Sessie is beëindigd |
| ErrorTitle_3003                                          | Niet-herkende gebruiker |
| ErrorTitle_3004                                          | Niet-geautoriseerde gebruiker |
| ErrorTitle_3005                                          | Antwoorden komen niet overeen |
| ErrorTitle_3006                                          | Onjuist wacht woord |
| ErrorTitle_3007                                          | Toegang tijdelijk geweigerd |
| ErrorTitle_3008                                          | Communicatie fout |
| ErrorTitle_3009                                          | Verboden invoer |
| ErrorTitle_3010                                          | Fout in browser configuratie |
| ErrorTitle_3011                                          | Fout in browser configuratie |
| ErrorTitle_3012                                          | Verificatie is mislukt |
| ErrorTitle_3013                                          | Kan de beveiligings code niet verzenden |
| FinalizeRegistrationHeading1                             | Als u ooit uw wacht woord opnieuw moet instellen: |
| FinalizeRegistrationSubHeading1                          | Ga naar de portal voor het opnieuw instellen van het wacht woord |
| FinalizeRegistrationSubHeading2                          | Uw identiteit verifiëren |
| FinalizeRegistrationSubHeading3                          | Uw nieuwe wacht woord kiezen |
| FinishingDescription                                     | Uw nieuwe wacht woord kiezen |
| FinishingTitle                                           | Wacht woord opnieuw instellen: |
| GotoPortalPrefix                                         | Ga naar |
| GotoPortalSuffix                                         | Start pagina |
| LabelTroubleshootingLinkText                             | Details weer geven |
| LoadingText                                              | Laden... |
| NoScriptTagErrorMessage                                  | Scripting is niet ingeschakeld in uw browser. Schakel scripting in en ga terug naar de start pagina of neem contact op met de Help Desk voor ondersteuning.  |
| PasswordResetOperationGeneralErrorMessage                | Fout bij het opnieuw instellen van het wacht woord. |
| PasswordResetOperationPolicyViolationErrorMessage        | Het wacht woord voldoet niet aan het wachtwoord beleid van uw organisatie. |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Fout bij het opnieuw instellen van het wacht woord, de gebruiker kan het wacht woord niet wijzigen. |
| PrivacyStatement                                         | Privacyverklaring |
| RegistrationDescription                                  | Self-Service wachtwoord registratie |
| RegistrationMission                                      | Als u ooit uw wacht woord vergeet, kunt u het zelf opnieuw instellen zonder uw Help Desk aan te roepen. |
| RegistrationPageTitle                                    | Forefront Identity Management-wachtwoord registratie |
| RegistrationSteps                                        | Klik op volgende om het registratie proces te starten. |
| RegistrationSuccessDescription                           | U bent nu geregistreerd |
| RegistrationSuccessTitle                                 | Voltooid: |
| RegistrationWelcomeTitle                                 | Wachtwoord registratie: |
| ResetDescription                                         | Selfservice voor wachtwoordherstel |
| ResetEnterNamePrompt                                     | Voer hieronder uw gebruikers naam in |
| ResetEnterPassword                                       | Voer een nieuw wacht woord in: |
| ResetExample1                                            | `contoso\mmeyers` |
| ResetExample2                                            | `mmeyers\@contoso.com` |
| ResetExamples                                            | Voorbeelden: |
| ResetPageTitle                                           | Forefront Identity Management-wacht woord opnieuw instellen |
| ResetReenterPassword                                     | Voer het wacht woord opnieuw in: |
| ResetSuccessDescription                                  | Uw wachtwoord is opnieuw ingesteld |
| ResetSuccessTitle                                        | Geleverd |
| ResetUseNewPassword                                      | U kunt nu uw nieuwe wacht woord gebruiken om u aan te melden. |
| ResetUsernameTextFormat                                  | (Wacht woord opnieuw instellen voor {0} ) <br/>**Opmerking** : {0} is de aanmelding van de gebruiker. |
| ResetWelcomeTitle                                        | Wacht woord opnieuw instellen: |
| TroubleshootingEmailSubject                              | Fout Details van FIM-aanvraag verwerking |
| TroubleshootingLabelAttributes                           | Kenmerken: |
| TroubleshootingLabelCloseButton                          | Sluiten |
| TroubleshootingLabelCopyToClipboard                      | Naar klembord kopiëren |
| TroubleshootingLabelCorrelationId                        | Correlatie-id: |
| TroubleshootingLabelDetails                              | Details: |
| TroubleshootingLabelPostCopyClipboardMessage             | De gegevens zijn gekopieerd naar het klem bord. |
| TroubleshootingLabelRequestId                            | Aanvraag-id: |
| TroubleshootingLabelSendEmail                            | Informatie per E-mail verzenden |
| TroubleshootingLabelSource                               | Reden: |
| TroubleshootingLabelViewRequestDetails                   | Details van aanvraag weer geven |
| TroubleshootingLinkText                                  | Probleemoplossings informatie |


## <a name="authentication-gate-strings"></a>Poort teken reeksen voor verificatie
In de volgende tabel ziet u de verificatie poort teken reeksen die kunnen worden aangepast:

<!-- The default values are actual UI strings and should not copy edited. -->

| Teken reeks naam van verificatie poort | Standaardwaarde |
|---|---|
| OTPEmailRegistraionEmailTextboxLabel                  | E-mail adres: |
| OTPEmailRegistrationEmailRequiredErrorMessage         | Het veld voor het e-mail adres mag niet leeg zijn. |
| OTPEmailRegistrationFooterReadOnly                    | Als u uw e-mail adres wilt bijwerken, volgt u het proces dat door uw organisatie is gedefinieerd of roept u de Help Desk aan. |
| OTPEmailRegistrationFooterReadWrite                   | Het e-mail adres wordt opgeslagen door uw organisatie in Forefront Identity Manager. |
| OTPEmailRegistrationGateTitle                         | Verificatie van e-mail adres |
| OTPEmailRegistrationHeaderReadOnly                    | Als u ooit uw wacht woord opnieuw moet instellen, wordt een beveiligings code voor verificatie verzonden naar uw e-mail adres. Als het onderstaande e-mail adres niet correct is, moet u deze bijwerken om de selfservice voor het opnieuw instellen van het wacht woord te kunnen gebruiken. |
| OTPEmailRegistrationHeaderReadWrite                   | Voer hieronder uw e-mail adres in. Als u ooit uw wacht woord opnieuw moet instellen, wordt er een verificatie code verzonden naar uw e-mail adres. |
| OTPEmailResetGateTitle                                | Uw identiteit verifiëren: E-mail |
| OTPEmailResetHeader                                   | Voer uw beveiligings code hieronder in. Er is een beveiligings code verzonden naar het e-mail adres dat is geregistreerd bij deze organisatie. |
| OTPRegularExpressionErrorMessage                      | De opgegeven waarde komt niet overeen met de verwachte indeling. |
| OTPResetOneTimePasswordRequiredErrorMessage           | Het veld voor de beveiligings code mag niet leeg zijn. |
| OTPResetVerificationLabel                             | Beveiligings code: |
| OTPSmsRegistrationFooterReadOnly                      | Als u uw mobiele telefoon nummer wilt bijwerken, volgt u het proces dat door uw organisatie is gedefinieerd of roept u de Help Desk aan. |
| OTPSmsRegistrationFooterReadWrite                     | Het mobiele telefoon nummer wordt door uw organisatie opgeslagen in Forefront Identity Manager. |
| OTPSmsRegistrationGateTitle                           | Verificatie van mobiele telefoon |
| OTPSmsRegistrationHeaderReadOnly                      | Als u ooit uw wacht woord opnieuw moet instellen, wordt een beveiligings code voor verificatie verzonden naar uw mobiele telefoon. Als het onderstaande mobiele telefoon nummer onjuist is, moet u deze bijwerken om de selfservice voor het opnieuw instellen van wacht woorden te kunnen gebruiken. |
| OTPSmsRegistrationHeaderReadWrite                     | Voer hieronder uw mobiele telefoon nummer in. Als u ooit uw wacht woord opnieuw moet instellen, wordt er een verificatie code verzonden naar uw mobiele telefoon. |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Het veld voor het mobiele telefoon nummer mag niet leeg zijn. |
| OTPSmsRegistrationSMSTextBoxLabel                     | Mobiele telefoon: |
| OTPSmsResetGateTitle                                  | Uw identiteit verifiëren: mobiele telefoon |
| OTPSmsResetHeader                                     | Voer uw beveiligings code hieronder in. Er is een beveiligings code verzonden naar de mobiele telefoon die bij deze organisatie is geregistreerd. |
| PasswordGateDescriptionText                           | Voer hieronder uw huidige wacht woord in en klik vervolgens op volgende. |
| PasswordGateErrorMessagePasswordRequired              | Voer uw huidige wacht woord in. |
| PasswordGateGateTitle                                 | Uw huidige wacht woord |
| PasswordGatePasswordLabelText                         | Wachtwoord: |
| PasswordGateUsernameTextFormat                        | `<i>` (aangemeld als: `<b>{0}</b>` ) `</i>` |
| QAGateErrorNotEnoughQuestionsAnswered                 | U moet ten minste {0} vragen beantwoorden. |
| QAGateIncorrectAnswer                                 | Uw antwoorden zijn onjuist. |
| QAGatePrivacyNotice                                   | De antwoorden die u opgeeft, worden door uw organisatie opgeslagen in Forefront Identity Manager. |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | U moet ten minste {0} vragen beantwoorden om te registreren. |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Een of meer antwoorden voldoen niet aan het beleid. |
| QAGateRegistrationThisAnswerValidationFailed          | Dit antwoord voldoet niet aan het beleid. |
| QAGateRegistrationTitle                               | Uw antwoorden registreren |
| QAGateResetNumberOfQuestionsExplanation_Format        | U moet {0} de volgende vragen beantwoorden {1} . |
| QAGateResetTitle                                      | Uw identiteit verifiëren: uw antwoorden verzenden  |


## <a name="custom-logo-banners"></a>Aangepaste logo banners

De standaard banner op de portal pagina's kunnen worden aangepast voor uw organisatie.

De logo banner aanpassen:

1. Maak uw aangepaste banners en sla ze op als PNG-bestanden. De bestanden moeten voldoen aan de volgende aanbevelingen:

   - Grootte: 490 x 50 pixels.
   - Bitdiepte: 32 pixels.

2. Kopieer de bestanden naar de map aanpassingen in elke portal die u wilt aanpassen.

3. Maak een bestand Style. CSS in elke map. Wijs het bestand naar de map aanpassingen voor de portal en het nieuwe logo. U kunt de naam van het logo zo nodig wijzigen, bijvoorbeeld `/Customizations/contosologo.png` . De CSS moet er als volgt uitzien:

   `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4. Als u Internet Explorer 6,0 gebruikt, moet u een alternatief niet-transparant logo opgeven en de volgende code toevoegen aan style. CSS:

   `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

   De CSS moet er als volgt uitzien:
   
   `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`


## <a name="custom-images-for-smartphones"></a>Aangepaste installatie kopieën voor smartphones

U kunt de logo afbeelding voor smartphones aanpassen. 

Een installatie kopie voor een smartphone aanpassen:

1. Maak uw installatie kopieën en sla ze op als PNG-bestanden. De bestanden moeten voldoen aan de volgende aanbevelingen:

   - Grootte: 190 X 50 pixels.
   - Bitdiepte: 32 pixels.

2. Kopieer de bestanden naar de map aanpassingen in elke portal die u wilt aanpassen.

3. Maak een bestand Style. CSS in elke map. Wijs het bestand naar de map aanpassingen voor de portal en het nieuwe logo. U kunt de naam van het logo zo nodig wijzigen, bijvoorbeeld `/Customizations/contosologo.png` . De CSS moet er als volgt uitzien:

   ```css
   @media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="custom-style-sheets"></a>Aangepaste opmaak modellen

U kunt de indeling en de stijl van de wachtwoord portals wijzigen met behulp van een aangepast trapsgewijs opmaak model (CSS).

Een aangepaste CSS gebruiken:

1. Maak uw aangepaste CSS-bestanden en sla deze op als style. CSS.

2. Kopieer de bestanden naar de map aanpassingen in elke portal die u wilt aanpassen.

De volgende code is een eenvoudig voor beeld van een Style. CSS-bestand:

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
>Voor MIM om aangepaste wijzigingen te herkennen, moet u IIS opnieuw starten door uit te voeren `iisreset` .

De volgende code is een geavanceerd voor beeld van een Style. CSS-bestand. Dit bestand bevat informatie die specifiek is voor een smartphone of Apple iPad voor het weer geven van de portals op deze apparaten.

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


## <a name="common-customization-issues"></a>Veelvoorkomende problemen met aanpassing

De volgende tabel bevat algemene problemen die kunnen optreden bij het upgraden van de FIM-service en de MIM-Portal.

| Probleem | Oplossing |
|---|---|
| Ik heb een teken reeks aanpassing gemaakt, maar deze is niet in de gebruikers interface weer gegeven.                       | Teken reeks aanpassingen in teken reeksen. voor resources moet IIS opnieuw worden gestart door te worden uitgevoerd `iisreset` . |
| Na het maken van een teken reeks. resources worden niet gewijzigd.          | De indeling van teken reeksen. resources is waarschijnlijk misvormd en kan door de portal worden genegeerd. Raadpleeg het gebeurtenis logboek onder **Windows logboeken**  >  **toepassing en services logboeken**  >  **Forefront Identity Manager** . |
| De eerste keer dat ik style. CSS heb toegevoegd, zie ik geen wijzigingen in de stijl in de portal.     | De eerste keer dat u een Style. CSS-bestand introduceert, moet u uitvoeren `iisreset` . |
| Nieuwe stijlen worden toegevoegd of gewijzigd in style. CSS, maar wijzigingen worden niet weer gegeven in de browser. | Wis de cache van de browser en vernieuw de pagina. Controleer de CSS-syntaxis. |
| Ik heb de inhoud van de CSS-map `<path_to_sspr_portal>\css\*.css` of het banner logo direct gewijzigd `<path_to_sspr_portal>\images\fimlogo.png` . Ik ben deze wijzigingen kwijt geraakt tijdens de upgrade. | Gebruikers wordt aangeraden deze bestanden niet rechtstreeks te wijzigen. Gebruik de aanpassings map alleen om een banner logo op te geven en pas CSS-stijl aanpassingen toe in style. CSS. De map met aanpassingen wordt opzettelijk niet overschreven door grote upgrades. Gebruik hulpprogram ma's zoals ILSpy en reflector om teken reeksen in de portal-assembly's te wijzigen. Gebruik teken reeksen. resources voor het overschrijven van standaard teken reeksen. De assembly's worden vervangen tijdens de upgrade.  |
| Het banner logo wordt niet weer gegeven in de portals. Ik zie nog steeds het FIM-logo.                  | De naam/het pad van de installatie kopie in style. CSS is ongeldig of de cache van de browser is niet gewist.  |
| Banner logo ziet rommelige in Internet Explorer 6.                                          | Geef een niet-transparante afbeelding voor Internet Explorer 6 met een bijbehorende stijl voor de afbeelding op in style. CSS. |
