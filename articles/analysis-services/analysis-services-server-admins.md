---
title: Verwalten von Serveradministratoren in Azure Analysis Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Serveradministratoren für einen Analysis Services-Server verwalten.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: ec1630f4de70f77c13e335c68aff16180e524c12
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307807"
---
# <a name="manage-server-administrators"></a>Verwalten von Serveradministratoren
„Serveradministratoren“ muss eine gültige Benutzer- oder Sicherheitsgruppe in Azure Active Directory (Azure AD) für den Mandanten sein, in dem sich der Server befindet. Zum Verwalten von Serveradministratoren können Sie die Option **Analysis Services-Administratoren** für Ihren Server im Azure-Portal oder die Servereigenschaften in SSMS verwenden. 

> [!NOTE]
> Für Sicherheitsgruppen muss die `MailEnabled`-Eigenschaft auf `True` gesetzt sein.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>So fügen Sie Serveradministratoren über das Azure-Portal hinzu
1. Klicken Sie im Azure-Portal im Bereich für Ihren Server auf **Analysis Services-Administratoren**.
2. Klicken Sie unter **\<Servername> – Analysis Services-Administratoren** auf **Hinzufügen**.
3. Wählen Sie unter **Serveradministratoren hinzufügen** Benutzerkonten aus Ihrem Azure AD-Verzeichnis aus, oder laden Sie externe Benutzer per E-Mail ein.

    ![Serveradministratoren im Azure-Portal](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>So fügen Sie Serveradministratoren über SSMS hinzu
1. Klicken Sie mit der rechten Maustaste auf den Server, und wählen Sie dann **Eigenschaften** aus.
2. Klicken Sie in **Eigenschaften für Analysis-Server** auf **Sicherheit**.
3. Klicken Sie auf **Hinzufügen**, und geben Sie dann die E-Mail-Adresse eines Benutzers oder einer Gruppe in Ihrem Azure AD ein.
   
    ![Hinzufügen von Serveradministratoren in SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Nächste Schritte 
[Authentifizierung und Benutzerberechtigungen](analysis-services-manage-users.md)  
[Verwalten von Datenbankrollen und Benutzern](analysis-services-database-users.md)  
[Rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md)  

