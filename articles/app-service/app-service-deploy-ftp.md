---
title: Bereitstellen der App in Azure App Service mithilfe von FTP/S | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre App mithilfe von FTP oder FTPS in Azure App Service bereitstellen.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: 561f317cd7afd740b83709efc8a75ed515626192
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
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
2. Wählen Sie im linken Menü **Übersicht** aus, und notieren Sie die Werte für **FTP/Bereitstellungsbenutzer**, **FTP-Hostname** und **FTPS-Hostname**. 

    ![FTP-Verbindungsinformationen](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Der im Azure-Portal angezeigte Wert **FTP/Bereitstellungsbenutzer** schließt den App-Namen ein, um den richtigen Kontext für den FTP-Server bereitzustellen.
    > Die gleichen Informationen finden Sie, indem Sie im linken Navigationsbereich **Eigenschaften** auswählen. 
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

## <a name="enforce-ftps"></a>Erzwingen von FTPS

Aus Sicherheitsgründen sollten Sie nur FTP über SSL zulassen. Sie können auch FTP und FTPS deaktivieren, wenn Sie keine FTP-Bereitstellung verwenden.

Wählen Sie auf der Ressourcenseite Ihrer App im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich **App-Einstellungen** aus.

Um unverschlüsselte FTP-Verbindungen zu deaktivieren, aktivieren Sie **Nur FTPS**. Um FTP und FTPS vollständig zu deaktivieren, wählen Sie **Deaktivieren** aus. Klicken Sie abschließend auf **Speichern**.

![Deaktivieren von FTP/S](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="troubleshoot-ftp-deployment"></a>Problembehandlung bei der FTP-Bereitstellung

- [Wie behebe ich Probleme bei der FTP-Bereitstellung?](#how-can-i-troubleshoot-ftp-deployment)
- [Ich kann meinen Code nicht mit FTP erreichen und veröffentlichen. Wie kann ich das Problem beheben?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Wie kann ich in Azure App Service über den passiven Modus eine Verbindung mit FTP herstellen?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>Wie behebe ich Probleme bei der FTP-Bereitstellung?

Der erste Schritt in der Problembehandlung bei der FTP-Bereitstellung besteht darin, ein Bereitstellungsproblem von einem Laufzeitproblem der Anwendung zu trennen.

Bereitstellungsprobleme führen in der Regel dazu, dass für Ihre Anwendung keine oder die falschen Dateien bereitgestellt werden. Dies kann durch Untersuchen Ihrer FTP-Bereitstellung oder Auswählen einer alternativen Bereitstellungsmethode (z.B. Quellcodeverwaltung) behoben werden.

Laufzeitprobleme der Anwendung führen in der Regel dazu, dass die richtigen Dateien für Ihre App bereitgestellt werden, die App jedoch ein falsches Verhalten aufweist. Dies kann behoben werden, indem Sie sich auf das Codeverhalten zur Laufzeit konzentrieren und spezifische Fehlerpfade untersuchen.

Weitere Informationen zur Ermittlung, ob es sich um ein Bereitstellungs- oder Laufzeitproblem handelt, finden Sie unter [Probleme mit Bereitstellung oder Laufzeit](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

 
### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>Ich kann meinen Code nicht mit FTP erreichen und veröffentlichen. Wie kann ich das Problem beheben?
Vergewissern Sie sich, dass Sie den richtigen Hostnamen und die richtigen [Anmeldeinformationen](#step-1--set-deployment-credentials) eingegeben haben. Stellen Sie auch sicher, dass die folgenden FTP-Ports auf dem Computer nicht durch eine Firewall blockiert werden:

- Steuerungsport für FTP-Verbindung: 21
- Port für FTP-Datenverbindung: 989, 10001-10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Wie kann ich in Azure App Service über den passiven Modus eine Verbindung mit FTP herstellen?
Azure App Service unterstützt das Herstellen einer Verbindung im aktiven und passiven Modus. Der passive Modus wird bevorzugt, da sich Bereitstellungscomputer in der Regel hinter einer Firewall (im Betriebssystem oder als Teil eines Heim- oder Firmennetzwerks) befinden. Weitere Informationen finden Sie in einem [Beispiel in der WinSCP-Dokumentation](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Nächste Schritte

Für komplexere Bereitstellungsszenarien versuchen Sie die [Bereitstellung in Azure mit Git](app-service-deploy-local-git.md). Die Git-basierte Bereitstellung in Azure ermöglicht Versionskontrolle, Paketwiederherstellung, MSBuild und mehr.

## <a name="more-resources"></a>Weitere Ressourcen

* [Anmeldeinformationen für die Azure App Service-Bereitstellung](app-service-deploy-ftp.md)
