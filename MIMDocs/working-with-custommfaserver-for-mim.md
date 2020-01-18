---
title: Gebruik een alternatieve Multi-Factor Authentication provider via een API om PAM of het SSPR-scenario te activeren | Microsoft Docs
description: Stel aangepaste MFA API in als een tweede beveiligingslaag wanneer uw gebruikers rollen activeren in Privileged Access Management en self-service voor wachtwoord herstel gebruiken.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 9ce531fb3f6f9c831ecdb716f006f947611871e6
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256594"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Een aangepaste Multi-Factor Authentication provider gebruiken via een API tijdens de activering van de PAM-rol of in SSPR

Klanten van Azure AD Premium of Azure MFA kunnen Azure MFA integreren in twee MIM-scenario's-Privileged Access Management (PAM)-rol activering en self-service voor wachtwoord herstel (SSPR).

MIM-klanten hebben twee extra opties:

 - Gebruik een aangepaste provider voor eenmalige wacht woorden, die alleen van toepassing is op het MIM SSPR-scenario en wordt beschreven in hand leiding voor het [configureren van self-service voor wachtwoord herstel met otp SMS gate](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10))
 - Gebruik een aangepaste telefoonservice provider voor multi-factor Authentication. Dit is van toepassing in zowel de MIM SSPR-als de PAM-scenario's, zoals beschreven in dit artikel

In dit artikel wordt uitgelegd hoe u MIM kunt gebruiken met een aangepaste multi-factor Authentication-provider, via een API en een integratie-SDK die is ontwikkeld door de klant.  

## <a name="prerequisites"></a>Vereisten

Als u een aangepaste provider-API voor Multi-Factor Authentication met MIM wilt gebruiken, hebt u het volgende nodig:

- Telefoonnummers voor alle kandidaatgebruikers
- MIM-hotfix 4.5.202.0 of hoger-Zie [versie geschiedenis](reference/version-history.md) voor aankondigingen
- MIM-service geconfigureerd voor SSPR of PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Benadering van het gebruik van aangepaste multi-factor Authentication-Code

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Stap 1: Zorg ervoor dat de MIM-service de versie 4.5.202.0 of hoger heeft

Down load en installeer de MIM-hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) of een latere versie.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Stap 2: Maak een DLL-bestand dat de IPhoneServiceProvider-interface implementeert

Het DLL-bestand moet een klasse bevatten die drie methoden implementeert:

- `InitiateCall`: de MIM-service roept deze methode aan. Met de service worden het telefoon nummer en de aanvraag-ID als para meters door gegeven.  De-methode moet een `PhoneCallStatus` waarde van `Pending`, `Success` of `Failed`retour neren.
- `GetCallStatus`: als een eerdere aanroep naar `initiateCall` geretourneerd `Pending`, wordt deze methode door de MIM-service aangeroepen. Deze methode retourneert ook `PhoneCallStatus` waarde van `Pending`, `Success` of `Failed`.
- `GetFailureMessage`: als er een eerdere aanroep van `InitiateCall` of `GetCallStatus` retourneert `Failed`, wordt deze methode door de MIM-service aangeroepen. Met deze methode wordt een diagnostisch bericht geretourneerd.

De implementaties van deze methoden moeten thread-safe zijn en daarnaast moet de implementatie van de `GetCallStatus` en `GetFailureMessage` niet aannemen dat ze worden aangeroepen door dezelfde thread als een eerdere aanroep van `InitiateCall`.

Sla het DLL-bestand op in de `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` Directory.

Voorbeeld code, die kan worden gecompileerd met Visual Studio 2010 of hoger.

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
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Stap 3: back-up maken van bestand mfasettings. XML, dat zich bevindt in de map C:\Program Files\Microsoft Forefront Identity Manager\2010\Service

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Stap 4: het bestand bestand mfasettings. xml bewerken

De volgende regels bijwerken of wissen:

- Alle regels voor configuratie vermeldingen verwijderen/wissen 

- Update of Voeg de volgende regels aan het volgende toe voor bestand mfasettings. XML met uw aangepaste telefoon provider <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Stap 5: de MIM-service opnieuw starten

Nadat de service opnieuw is opgestart, gebruikt u SSPR en/of PAM om de functionaliteit te valideren met de aangepaste ID-provider.

> [!NOTE] 
> Als u de instelling wilt herstellen, vervangt u bestand mfasettings. XML door het back-upbestand in stap 3


## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met de Azure Multi-Factor Authentication-server](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Wat is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Release geschiedenis van MIM-versie](./reference/version-history.md)
