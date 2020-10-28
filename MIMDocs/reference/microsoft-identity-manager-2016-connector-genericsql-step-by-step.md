---
title: Algemene SQL-connector stap-voor stap | Microsoft Docs
description: Dit artikel doorloopt u stapsgewijs door een eenvoudige stap van het HR-systeem met behulp van de algemene SQL-connector.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 1343fc7604789985c219f6bbae526df72a0baa5b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/10/2020
ms.locfileid: "92758788"
---
# <a name="generic-sql-connector-step-by-step"></a>Algemene SQL Connector, stap voor stap
Dit onderwerp bevat een stapsgewijze hand leiding. Er wordt een eenvoudige HR-data base gemaakt en gebruikt voor het importeren van sommige gebruikers en hun groepslid maatschap.

## <a name="prepare-the-sample-database"></a>De voorbeeld database voorbereiden
Voer het SQL-script dat is gevonden in [bijlage a](#appendix-a)op een server met SQL Server uit. Met dit script maakt u een voorbeeld database met de naam GSQLDEMO. Het object model voor de gemaakte data base ziet er als volgt uit:  
![Object model](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/objectmodel.png)

Maak ook een gebruiker die u wilt gebruiken om verbinding te maken met de data base. In dit scenario wordt de gebruiker FABRIKAM\SQLUser genoemd en bevindt zich in het domein.

## <a name="create-the-odbc-connection-file"></a>Maak het ODBC-verbindings bestand
De algemene SQL-connector gebruikt ODBC om verbinding te maken met de externe server. Eerst moet u een bestand maken met de ODBC-verbindings gegevens.

1. Start het ODBC-beheer programma op uw server:  
   ![ODBC](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc.png)
2. Selecteer de **Bestands-DSN** van het tabblad. Klik op **toevoegen...** .  
   ![ODBC1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc1.png)
3. Het out-of-Box-stuur programma werkt prima, dus Selecteer het en klik op **volgende>** .  
   ![ODBC2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc2.png)
4. Geef het bestand een naam, bijvoorbeeld **GenericSQL** .  
   ![ODBC3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc3.png)
5. Klik op **Voltooien** .  
   ![ODBC4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc4.png)
6. Tijd voor het configureren van de verbinding. Geef de gegevens bron een goede beschrijving en geef de naam van de server met SQL Server op.  
   ![ODBC5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc5.png)
7. Selecteer hoe u met SQL wilt verifiëren. In dit geval gebruiken we Windows-verificatie.  
   ![ODBC6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc6.png)
8. Geef de naam op van de voorbeeld database **GSQLDEMO** .  
   ![ODBC7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc7.png)
9. Bewaar alle standaard instellingen in dit scherm. Klik op **Voltooien** .  
   ![ODBC8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc8.png)
10. Als u wilt controleren of alles werkt zoals verwacht, klikt u op **gegevens bron testen** .  
    ![ODBC9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc9.png)
11. Controleer of de test is geslaagd.  
    ![ODBC10](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc10.png)
12. Het ODBC-configuratie bestand moet nu worden weer gegeven in bestands-DSN.  
    ![ODBC11](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc11.png)

We hebben nu het benodigde bestand en kunnen beginnen met het maken van de connector.

## <a name="create-the-generic-sql-connector"></a>De algemene SQL-connector maken
1. Selecteer in de Synchronization Service Manager-gebruikers interface de optie **connectors** en **maken** . Selecteer **algemene SQL (micro soft)** en geef deze een beschrijvende naam.  
   ![Connector1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector1.png)
2. Zoek het DSN-bestand dat u in de vorige sectie hebt gemaakt en upload het naar de-server. Geef de referenties op om verbinding te maken met de data base.  
   ![Connector2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector2.png)
3. In dit scenario maken we het eenvoudig voor ons en zeggen we dat er twee object typen, **gebruikers** en **groepen** zijn.
   ![Connector3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector3.png)
4. Om de kenmerken te vinden, willen we dat de connector die kenmerken detecteert door de tabel zelf te bekijken. Omdat **gebruikers** een gereserveerd woord in SQL zijn, moeten we dit opgeven tussen vier Kante haken [].  
   ![Connector4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector4.png)
5. Tijd voor het definiëren van het anker kenmerk en het DN-kenmerk. Voor **gebruikers** gebruiken we de combi natie van de twee kenmerken username en EmployeeID. Voor **groep** gebruiken we GroupName (niet realistisch in de levens duur, maar voor dit scenario werkt het niet).
   ![Connector5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector5.png)
6. Niet alle kenmerk typen kunnen worden gedetecteerd in een SQL database. Het verwijzings kenmerk type met name kan niet. Voor het object type groep moeten we de OwnerID en de MemberID wijzigen in verwijzing.  
   ![Connector6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector6.png)
7. Voor de kenmerken die we als referentie kenmerken hebben geselecteerd in de vorige stap is het object type deze waarden een verwijzing naar. In ons geval is het object type gebruiker.  
   ![Connector7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector7.png)
8. Selecteer op de pagina globale para meters **water merk** als de Delta strategie. Typ ook de datum/tijd notatie **jjjj-mm-dd uu: mm: SS** .
   ![Connector8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector8.png)
9. Selecteer beide object typen op de pagina **partities en hiërarchieën configureren** .
   ![Connector9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector9.png)
10. Selecteer in het dialoog **object typen selecteren** en **kenmerken selecteren** beide object typen en alle kenmerken. Klik op de pagina **ankers configureren** op **volt ooien** .

## <a name="create-run-profiles"></a>Uitvoeringsprofielen maken
1. Selecteer in de Synchronization Service Manager-gebruikers interface de optie **connectors** en **Configureer run Profiles** . Klik op **Nieuw profiel** . We beginnen met **volledige import bewerking** .  
   ![Runprofile1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile1.png)
2. Selecteer het type **volledige import (alleen fase)** .  
   ![Runprofile2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile2.png)
3. Selecteer de partitie **object = gebruiker** .  
   ![Runprofile3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile3.png)
4. Selecteer **tabel** en type **[gebruikers]** . Schuif omlaag naar de sectie met het object type met meerdere waarden en voer de gegevens in zoals in de volgende afbeelding. Selecteer **volt ooien** om de stap op te slaan.  
   ![Runprofile4a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4b.png)  
5. Selecteer **nieuwe stap** . Selecteer deze keer **object = groep** . Gebruik op de laatste pagina de configuratie zoals in de volgende afbeelding. Klik op **Voltooien** .  
   ![Runprofile5a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5b.png)  
6. Optioneel: als u wilt, kunt u aanvullende uitvoerings profielen configureren. Voor dit scenario wordt alleen de volledige import gebruikt.
7. Klik op **OK** om het wijzigen van de uitvoerings profielen te volt ooien.

## <a name="add-some-test-data-and-test-the-import"></a>Voeg enkele test gegevens toe en test de import
Vul een aantal test gegevens in de voorbeeld database in. Wanneer u klaar bent, selecteert u **uitvoeren** en **volledige import** .

Dit is een gebruiker met twee telefoon nummers en een groep met een aantal leden.  
![CS1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Bijlage A
**SQL-script voor het maken van de voorbeeld database**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
