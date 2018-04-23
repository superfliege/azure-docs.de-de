---
title: Hinzufügen eines Dienstprinzipals zur Azure Analysis Services-Serveradministratorrolle | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Automatisierungsdienstprinzipal zur Serveradministratorrolle hinzufügen.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9c6417e069bbed38b1f6e9317636a10834ce7197
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="add-a-service-principle-to-the-server-administrator-role"></a>Hinzufügen eines Dienstprinzipals zur Serveradministratorrolle 

 Um unbeaufsichtigte PowerShell-Tasks zu automatisieren, muss ein Dienstprinzipal auf dem zu verwaltenden Analysis Services-Server über die Rechte eines **Serveradministrators** verfügen. In diesem Artikel wird beschrieben, wie auf einem Azure AS-Server ein Dienstprinzipal zur Serveradministratorrolle hinzugefügt wird.

## <a name="before-you-begin"></a>Voraussetzungen
Bevor Sie diese Aufgabe durchführen, muss ein Dienstprinzipal in Azure Active Directory registriert sein.

[Erstellen eines Dienstprinzipals – Azure-Portal](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Erstellen eines Dienstprinzipals – PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Für die Durchführung dieser Aufgabe benötigen Sie die Rechte eines [Serveradministrators](analysis-services-server-admins.md) auf dem Azure AS-Server. 

## <a name="add-service-principle-to-server-administrators-role"></a>Hinzufügen eines Dienstprinzipals zur Serveradministratorrolle

1. Stellen Sie in SSMS eine Verbindung mit Ihrem Azure AS-Server her.
2. Klicken Sie unter **Servereigenschaften** > **Sicherheit** auf **Hinzufügen**.
3. Suchen Sie unter **Benutzer oder Gruppe auswählen** anhand des Namens nach Ihrer registrierten App, und klicken Sie dann auf **Hinzufügen**.

    ![Suchen nach dem Dienstprinzipalkonto](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Überprüfen Sie die ID des Dienstprinzipalkontos, und klicken Sie dann auf **OK**.
    
    ![Suchen nach dem Dienstprinzipalkonto](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> Bei Servervorgängen, für die AzureRm-Cmdlets verwendet werden, muss der Dienstprinzipal mit dem Scheduler außerdem zur Rolle **Besitzer** für die Ressource in der [rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, RBAC)](../role-based-access-control/overview.md) gehören. 

## <a name="related-information"></a>Verwandte Informationen

* [Herunterladen des SQL Server PowerShell-Moduls](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Herunterladen von SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


