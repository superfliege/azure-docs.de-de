---
title: Konfigurieren von Anmeldeinformationen für die Azure App Service-Bereitstellung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anmeldeinformationen für die Azure App Service-Bereitstellung verwenden.
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnoc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/22/2018
ms.author: cephalin
ms.openlocfilehash: a04057023308087adbf449b8f91af6a473c11c1e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52312094"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Konfigurieren von Anmeldeinformationen für die Azure App Service-Bereitstellung
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) unterstützt zwei Arten von Anmeldeinformationen für [lokale Git-Bereitstellungen](app-service-deploy-local-git.md) und [FTP/S-Bereitstellungen](app-service-deploy-ftp.md). Diese sind nicht identisch mit Ihren Azure Active Directory-Anmeldeinformationen.

* **Anmeldeinformationen auf Benutzerebene**: ein Satz von Anmeldeinformationen für das gesamte Azure-Konto. Hiermit können Sie App Service für alle Apps in allen Abonnements bereitstellen, für die das Azure-Konto über Zugriffsberechtigungen verfügt. Dies ist der Standardsatz, der auf der Benutzeroberfläche des Portals angezeigt wird (z.B. **Übersicht** und **Eigenschaften** auf der [Ressourcenseite](../azure-resource-manager/resource-group-portal.md#manage-resources) der App). Wenn ein Benutzer App-Zugriff über rollenbasierte Zugriffssteuerung (RBAC) oder Co-Administrator-Berechtigungen erhält, kann er die eigenen Anmeldeinformationen auf Benutzerebene verwenden, bis der Zugriff widerrufen wird. Teilen Sie diese Anmeldeinformationen nicht mit anderen Azure-Benutzern.

* **Anmeldeinformationen auf App-Ebene**: ein Satz von Anmeldeinformationen für jede App. Er kann nur verwendet werden, um diese App bereitzustellen. Die Anmeldeinformationen für eine App werden bei der Erstellung jeder App automatisch generiert. Sie können nicht manuell konfiguriert, jedoch jederzeit zurückgesetzt werden. Damit ein Benutzer Zugriff auf Anmeldeinformationen auf App-Ebene über die rollenbasierte Zugriffssteuerung (RBAC) erhalten kann, muss er in der App mindestens ein Mitwirkender sein. Benutzer mit Leseberechtigung dürfen nicht veröffentlichen und haben keinen Zugriff auf diese Anmeldeinformationen.

## <a name="userscope"></a>Festlegen und Zurücksetzen der Anmeldeinformationen auf Benutzerebene

Sie können die Anmeldeinformationen auf Benutzerebene auf der [Ressourcenseite](../azure-resource-manager/resource-group-portal.md#manage-resources) einer App konfigurieren. Unabhängig von der App, in der Sie diese Anmeldeinformationen konfigurieren, gelten sie für alle Apps und für alle Abonnements in Ihrem Azure-Konto. 

So konfigurieren Sie die Anmeldeinformationen auf Benutzerebene

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Menü auf **App Services** > **&lt;beliebige_App>** > **Bereitstellungscenter** > **Anmeldeinformationen für die Bereitstellung**.

    Im Portal muss mindestens eine App vorhanden sein, bevor Sie auf die Seite mit den Anmeldeinformationen für die Bereitstellung zugreifen können. Mit der [Azure-CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) können Sie die Anmeldeinformationen auf Benutzerebene jedoch ohne eine vorhandene App konfigurieren.

2. Klicken Sie auf **Benutzeranmeldeinformationen**, konfigurieren Sie den Benutzernamen und das Kennwort, und klicken Sie dann auf **Anmeldeinformationen speichern**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Wenn Sie Anmeldeinformationen für die Bereitstellung festgelegt haben, finden Sie den *Git*-Benutzernamen für Bereitstellungen in der **Übersicht** Ihrer App.

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Den *FTP*-Benutzernamen für die Bereitstellung finden Sie in den **Eigenschaften** Ihrer App.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Ihr Kennwort auf Benutzerebene für die Bereitstellung wird von Azure nicht angezeigt. Wenn Sie das Kennwort vergessen, können Sie Ihre Anmeldeinformationen über die Schritte in diesem Abschnitt zurücksetzen.
>
>  

## <a name="appscope"></a>Abrufen und Zurücksetzen der Anmeldeinformationen auf App-Ebene
So rufen Sie die Anmeldeinformationen auf App-Ebene ab

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im linken Menü auf **App Services** > **&lt;beliebige_App>** > **Bereitstellungscenter** > **Anmeldeinformationen für die Bereitstellung**.

2. Klicken Sie auf **App-Anmeldeinformationen** und dann auf den Link **Kopieren**, um den Benutzernamen oder das Kennwort zu kopieren.

    ![](./media/app-service-deployment-credentials/deployment_credentials_app_level.png)

Um die Anmeldeinformationen auf App-Ebene zurückzusetzen, klicken Sie im selben Dialogfeld auf **Anmeldeinformationen zurücksetzen**.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie diese Anmeldeinformationen zum Bereitstellen Ihrer App über [lokales Git](app-service-deploy-local-git.md) oder [FTP/S](app-service-deploy-ftp.md) verwenden.
