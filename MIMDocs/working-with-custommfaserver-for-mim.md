---
title: Een alternatieve multi-factor Authentication-provider via een API om PAM te activeren of in scenario SSPR | Microsoft Docs
description: Instellen van aangepaste MFA-API als een tweede beveiligingslaag wanneer uw gebruikers rollen in Privileged Access Management activeren en gebruiken van de selfservice voor wachtwoordherstel.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 7fb111520f94541672fc56d0fd2ee95bfcd3a49e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690749"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Een aangepaste multi-factor Authentication-provider via een API te gebruiken tijdens de activering van PAM-rol of in selfservice voor Wachtwoordherstel

Klanten van Azure AD Premium of Azure MFA kunnen Azure MFA integreren in twee MIM-scenario's - activering van rollen voor Privileged Access Management (PAM) en selfservice wachtwoord opnieuw instellen (SSPR).

MIM-klanten hebben twee extra opties:

 - Gebruik een aangepaste levering van een eenmalig wachtwoord-provider die van toepassing alleen in het scenario voor MIM SSPR en gedocumenteerd in de handleiding voor het is [configureren selfservice voor wachtwoordherstel met OTP-SMS-Gate](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - Gebruik een aangepaste multi-factor authentication-provider telephony. Dit is van toepassing in de MIM SSPR en de PAM-scenario's die worden beschreven in dit artikel

In dit artikel bevat een overzicht over het gebruik van MIM met een aangepaste multi-factor authentication-provider, via een API en een integratie SDK die zijn ontwikkeld door de klant.  

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken een aangepaste API van de multi-factor Authentication-provider met MIM, hebt u het volgende nodig:

- Telefoonnummers voor alle kandidaatgebruikers
- MIM-hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) of hoger - Zie [versiegeschiedenis](reference/version-history.md) voor aankondigingen
- MIM-Service is geconfigureerd voor de self-service voor Wachtwoordherstel of PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Benadering met behulp van aangepaste multi-factor authenticatiecode

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Stap 1: Zorg ervoor dat de MIM-Service is versie 4.5.202.0 of hoger

Download en installeer de hotfix MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) of een latere versie.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Stap 2: Maken van een DLL-bestand waarmee de IPhoneServiceProvider-interface wordt geïmplementeerd

Het DLL-bestand moet bevatten een klasse, waarmee de drie methoden geïmplementeerd:

- `InitiateCall`: De MIM-Service wordt deze methode worden aangeroepen. De service doorgegeven de telefoon aantal en de aanvraag-ID als parameters.  De methode moet retourneren een `PhoneCallStatus` waarde van `Pending`, `Success` of `Failed`.
- `GetCallStatus`: Als een eerdere aanroep `initiateCall` geretourneerd `Pending`, de MIM-Service met deze methode wordt aangeroepen. Deze methode retourneert ook `PhoneCallStatus` waarde van `Pending`, `Success` of `Failed`.
- `GetFailureMessage`: Als een eerdere aanroep van `InitiateCall` of `GetCallStatus` geretourneerd `Failed`, de MIM-Service met deze methode wordt aangeroepen. Deze methode retourneert een diagnostisch bericht.

De implementaties van deze methoden moet thread-veilig is, en daarnaast de uitvoering van de `GetCallStatus` en `GetFailureMessage` moet niet vanuit gegaan dat ze worden aangeroepen door de dezelfde thread als een eerdere aanroep `InitiateCall`.

Store van het DLL-bestand in de `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` directory.

Voorbeeld van code, die worden kan gecompileerd met behulp van Visual Studio 2010 of hoger.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Stap 3: Back-up de MfaSettings.xml zich in de 'C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Stap 4: Bewerk het bestand MfaSettings.xml

Bijwerken of schakelt u de volgende regels:

- Alle regels voor configuratie-items verwijderen/wissen 

- Bijwerken of de volgende regels toevoegen aan de volgende MfaSettings.xml met uw provider aangepast telefoonnummer <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Stap 5: MIM-Service opnieuw starten

Nadat de service opnieuw is opgestart, gebruikt u SSPR en/of de PAM-functionaliteit met de aangepaste id-provider te valideren.

> [!NOTE] 
> Als u wilt terugkeren instelling MfaSettings.xml vervangen door uw back-upbestand in stap 3


## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met de Azure multi-factor Authentication-Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Wat is Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Versiegeschiedenis van MIM](./reference/version-history.md)
