---
title: Bereitstellen der App in Azure App Service mithilfe von FTP/S | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre App mithilfe von FTP oder FTPS in Azure App Service bereitstellen.
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: fcd079306a8968505349bb3f4a805f203a5c9999
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Bereitstellen der App in Azure App Service mithilfe von FTP/S

In diesem Artikel erfahren Sie, wie Sie eine Web-App, das mobile App-Back-End oder eine API-App mithilfe von FTP oder FTPS in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) bereitstellen.

Der FTP/S-Endpunkt für Ihre App ist bereits aktiv. Zum Aktivieren der FTP/S-Bereitstellung ist keine Konfiguration erforderlich.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Schritt 1: Festlegen von Anmeldeinformationen für die Bereitstellung

Für den Zugriff auf den FTP-Server für Ihre App benötigen Sie zuerst Anmeldeinformationen für die Bereitstellung. 

Informationen zum Festlegen oder Zurücksetzen Ihrer Anmeldeinformationen für die Bereitstellung finden Sie unter [Anmeldeinformationen für die Azure App Service-Bereitstellung](app-service-deployment-credentials.md). Dieses Tutorial veranschaulicht die Verwendung von Anmeldeinformationen auf Benutzerebene.

## <a name="step-2-get-ftp-connection-information"></a>Schritt 2: Abrufen der FTP-Verbindungsinformationen

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Seite [Ressourcen](../azure-resource-manager/resource-group-portal.md#manage-resources) Ihrer App.
2. Wählen Sie im linken Menü **Übersicht**, und notieren Sie dann die Werte für **FTP/Bereitstellungsbenutzer**, **FTP-Hostname** und **FTPS-Hostname**. 

    ![FTP-Verbindungsinformationen](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Der im Azure-Portal angezeigte Wert **FTP/Bereitstellungsbenutzer** schließt den App-Namen ein, um den richtigen Kontext für den FTP-Server bereitzustellen.
    > Die gleichen Informationen finden Sie, wenn Sie im linken Menü **Eigenschaften** auswählen. 
    >
    > Das Kennwort für die Bereitstellung wird nie angezeigt. Wenn Sie Ihr Kennwort für die Bereitstellung vergessen haben, wechseln Sie zurück zu [Schritt1](#step1), und setzen Sie es zurück.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Schritt 3: Bereitstellen von Dateien in Azure

1. Verwenden Sie in Ihrem FTP-Client (z.B. [Visual Studio](https://www.visualstudio.com/vs/community/) oder [FileZilla](https://filezilla-project.org/download.php?type=client)) die gesammelten Verbindungsinformationen, um eine Verbindung mit Ihrer App herzustellen.
3. Kopieren Sie Ihre Dateien und die entsprechende Verzeichnisstruktur in das Verzeichnis [**/site/wwwroot**](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (bzw. für WebJobs in das Verzeichnis **/site/wwwroot/App_Data/Jobs/**).
4. Navigieren Sie zur URL Ihrer App, um sicherzustellen, dass die Anwendung richtig ausgeführt wird. 

> [!NOTE] 
> Im Gegensatz zu [Git-basierten Bereitstellungen](app-service-deploy-local-git.md) unterstützt die FTP-Bereitstellung keine der folgenden Bereitstellungsautomatisierungen: 
>
> - Wiederherstellungen der Abhängigkeit (z.B. NuGet-, NPM-, PIP- oder Composer-Automatisierungen)
> - Kompilierung von .NET-Binärdateien
> - Generierung von „web.config“ (hier sehen Sie ein [Node.js-Beispiel](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Generieren Sie diese erforderlichen Dateien auf Ihrem lokalen Computer manuell, und stellen Sie sie zusammen mit Ihrer App bereit.
>
>

## <a name="next-steps"></a>Nächste Schritte

Für komplexere Bereitstellungsszenarien versuchen Sie die [Bereitstellung in Azure mit Git](app-service-deploy-local-git.md). Die Git-basierte Bereitstellung in Azure ermöglicht Versionskontrolle, Paketwiederherstellung, MSBuild und mehr.

## <a name="more-resources"></a>Weitere Ressourcen

* [Anmeldeinformationen für die Azure App Service-Bereitstellung](app-service-deploy-ftp.md)
