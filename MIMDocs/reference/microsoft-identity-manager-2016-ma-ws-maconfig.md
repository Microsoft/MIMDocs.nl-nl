---
title: Configuratie opties voor de web service-connector | Microsoft Docs
description: In dit artikel worden de stappen beschreven die nodig zijn voor het installeren van het hulp programma voor configuratie van webservices.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 3/27/2020
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.reviewer: markwahl-msft
ms.assetid: ''
ms.openlocfilehash: 34c83427b6dfb3084976aebf29c019d8228f8247
ms.sourcegitcommit: d21963c1fba6dc908bec5eaadc54e3395a8ef8c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/10/2020
ms.locfileid: "92759010"
---
# <a name="web-service-connector-configuration-options"></a>Configuratieopties van de webserviceconnector
In dit artikel worden de stappen beschreven voor het configureren van een nieuwe webservice-connector of voor het aanbrengen van wijzigingen in een bestaande webservice-connector via de gebruikers interface van de synchronisatie service van Microsoft Identity Manager (MIM).

>[!IMPORTANT]
>Down load en installeer de [webservice-connector](https://www.microsoft.com/download/details.aspx?id=51495) voordat u de stappen in dit artikel uitvoert.

## <a name="configure-the-web-service-connector-in-the-synchronization-service"></a>De web service-connector configureren in de synchronisatie service

U kunt een nieuwe webservice-connector maken met behulp van de ontwerp functie voor beheer agenten. Nadat u de connector hebt gemaakt, kunt u meerdere uitvoerings profielen definiëren voor het uitvoeren van verschillende taken. Wanneer u een bestaande connector configureert, kunt u een taak wijzigen door te klikken op de juiste pagina in Management Agent Designer. Volg de onderstaande stappen om een nieuwe webservice-connector te configureren.

1. Open Microsoft Identity Manager 2016-synchronisatie service. Selecteer **beheer agents** in het menu **extra** .

2. Selecteer **maken** in het menu **acties** . De beheer agent Designer wordt geopend.

3. Selecteer in **beheer agent Designer** onder **beheer agent voor** de optie **Web Service (micro soft)** . Selecteer vervolgens **Volgende** .

    ![Een beheer agent maken](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma.png)

4. Op het scherm **connectiviteit** selecteert u het standaard- **connector project** voor de webservice. Geef waarden op voor de **host** en **poort** . Selecteer vervolgens **Volgende** .

    ![Connectiviteit voor de beheer agent maken](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-connectivity.png)

5. Definieer de **algemene para meters** . Gebruik de aanmeldings referentie die is aangeschaft bij de beheerder van de webservice om verbinding te maken met de host. Selecteer vervolgens **Volgende** .

    ![Algemene para meters voor de beheer agent instellen](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-global-parameters.png)

    - Als de locatie van de gegevens bron wordt gecontroleerd op zomer-en de gegevens bron is geconfigureerd om automatisch aan te passen aan de zomer-en winter instellingen, controleert u of de **gegevens bron zo is geconfigureerd dat de klok automatisch wordt aangepast** aan de zomer-en winter tijd optie.
    - Als u de werk stroom voor het testen van de verbinding vanuit deze connector wilt activeren, controleert u de optie **verbinding testen** .

6. Selecteer op het volgende scherm de optie **standaard** voor het **selecteren van Directory partities** . Selecteer vervolgens **Volgende** .

    ![Partities voor de beheer agent maken](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-partitions.png)

7. Selecteer op het scherm **object typen selecteren** het object type waarmee u wilt werken. Standaard ondersteunt de web service-connector twee object typen: **werk nemer** en **gebruiker** . Selecteer vervolgens **Volgende** .

    ![Selecteer het object type](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-object-types.png)

8. Selecteer op de pagina **kenmerken selecteren** alle verplichte kenmerken voor de geselecteerde objecten en kenmerken waarmee u wilt werken. Selecteer vervolgens **Volgende** .

    ![De kenmerken voor de objecten selecteren](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-attributes.png)

9. Geef op de pagina **ankers configureren** de anker kenmerken op. Selecteer vervolgens **Volgende** .

    ![De ankers configureren](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-anchors.png)

10. Geef op de pagina **connector filter configureren** het **connector filter** op. Selecteer vervolgens **Volgende** .

    ![Het connector filter opgeven](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-connector-filter.png)

11. Geef op de pagina **regels voor samen voegen en projectie configureren** de regels voor samen voegen en projectie op. U kunt een nieuwe regel voor samen voegen en projectie maken door respectievelijk **nieuwe joinlijn** en **nieuwe projectie regel** te selecteren. Selecteer vervolgens **Volgende** .

    ![De regels voor samen voegen en projectie opgeven](media/microsoft-identity-manager-2016-ma-ws-maconfig/join-projection.png)

12. Configureer op de volgende pagina de kenmerk stroom. U moet het **toewijzings type** en de **stroom richting** opgeven voor de kenmerken voor de geselecteerde object typen. Selecteer vervolgens **Volgende** .

    ![De kenmerk stroom configureren](media/microsoft-identity-manager-2016-ma-ws-maconfig/attribute-flow.png)

13. Geef op welk type ongedaan maken van de inrichting moet worden toegepast op de objecten. Selecteer vervolgens **Volgende** .

    ![Geef het type van het ongedaan maken van de inrichting op](media/microsoft-identity-manager-2016-ma-ws-maconfig/deprovisioning.png)

14. In het geval van een import stroom is de pagina **uitbrei dingen configureren** uitgeschakeld. U kunt uitbrei dingen voor export stromen configureren door eerst het type **Geavanceerde** toewijzing op de pagina **kenmerk stroom configureren** te selecteren.

    ![Extensies configureren](media/microsoft-identity-manager-2016-ma-ws-maconfig/extensions.png)

15. Klik op **Voltooien** .

De connector is nu geconfigureerd:

![Configuratie van connector is voltooid](media/microsoft-identity-manager-2016-ma-ws-maconfig/sync-manager.png)

Nadat een connector is geconfigureerd, kunt u de uitvoerings profielen configureren door **Run-profielen configureren** te selecteren.

## <a name="additional-steps"></a>Aanvullende stappen

Wanneer authenticatie op basis van een certificaat wordt gebruikt, is er een aanvullende wijziging nodig nadat het hulp programma voor de configuratie van de webservice een WSConfig-bestand heeft gegenereerd, voordat dit bestand kan worden geïmporteerd in een web service connector-project in MIM-synchronisatie service.

Verificatie op basis van een certificaat inschakelen:

- Uw project configureren voor het gebruik van basis verificatie in het hulp programma voor configuratie van webservices
- Maak een kopie van my_project. wsconfig-bestand en wijzig de naam in my_project.zip
- Open dit archief en wijzig generated.config bestand om basis verificatie met verificatie op basis van certificaten te vervangen (hieronder vindt u een voor beeld)
- Vervang generated.config bestand in my_project.zip en wijzig de naam in my_project_updated. wsconfig
- Selecteer my_project_updated. wsconfig bij het maken van een beheer agent in de MIM-synchronisatie server

Zoek generated.config voorbeeld bestand met verificatie op basis van certificaten hieronder:

```xml
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="SoapAuthenticationType" value="Certificate"/>
        </appSettings>
        <system.serviceModel>
            <bindings>
                <wsHttpBinding>
                    <binding name="binding">
                        <security mode="Transport">
                            <transport clientCredentialType="Certificate"/>
                        </security>
                    </binding>
                </wsHttpBinding>
            </bindings>
            <client>
                <endpoint address="https://myserver.local.net:8011/sap/bc/srt/scs/sap/zsapconnect?sap-client=800"
                    binding="wsHttpBinding" bindingConfiguration="binding"
                    contract="SAPCONNECTOR.ZSAPConnect" name="binding"/>
            </client>
            <behaviors>
                <endpointBehaviors>
                    <behavior name="endpointCredentialBehavior">
                        <clientCredentials>
                            <clientCertificate findValue="my.certificate.name.local.net"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName"/>
                        </clientCredentials>
                    </behavior>
                </endpointBehaviors>
            </behaviors>
        </system.serviceModel>
    </configuration>
```

## <a name="next-steps"></a>Volgende stappen

- [Installeer het hulp programma voor configuratie van webservices](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP-implementatie handleiding](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Hand leiding voor REST-implementatie](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuratie van de web service-MA](microsoft-identity-manager-2016-ma-ws-maconfig.md)
