---
title: Ausführen Ihrer Azure Functions aus einem Paket | Microsoft-Dokumentation
description: Lassen Sie die Azure Functions-Laufzeit Ihre Funktionen ausführen, indem Sie eine Bereitstellungspaketdatei einbinden, die Ihre Projektdateien für die Funktions-App enthält.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
editor: ''
tags: ''
ms.service: functions
ms.devlang: multiple
ms.topic: conceptual
ms.workload: na
ms.date: 08/22/2018
ms.author: glenga
ms.openlocfilehash: 2b9cc3618bf21eac268e3c25f08b80124d52e6af
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669290"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Ausführen Ihrer Azure Functions aus einem Paket

> [!NOTE]
> Die in diesem Artikel beschriebene Funktionalität befindet sich derzeit in der Vorschau; sie ist für Functions unter Linux nicht verfügbar.

In Azure können Sie Ihre Functions direkt aus der Bereitstellungspaketdatei in Ihrer Funktions-App ausführen. Die andere Option ist die Bereitstellung Ihrer Funktionsprojektdateien im `d:\home\site\wwwroot`-Verzeichnis Ihrer Funktions-App.

Dieser Artikel beschreibt die Vorteile einer Ausführung Ihrer Functions aus einem Paket. Außerdem wir erläutert, wie Sie diese Funktionalität in Ihrer Funktions-App aktivieren.

## <a name="benefits-of-running-from-a-package-file"></a>Vorteile der Ausführung aus einer Paketdatei
  
Es gibt mehrere Vorteile bei der Ausführung aus einer Paketdatei:

+ Reduziert das Risiko von Sperrungen beim Kopieren von Dateien.
+ Kann in einer Produktions-App (mit Neustart) bereitgestellt werden.
+ Sie können sich sicher sein, dass die Dateien, die in Ihrer App ausgeführt werden, sicher sind.
+ Verbessert die Leistung von [Azure Resource Manager-Bereitstellungen](functions-infrastructure-as-code.md).
+ Kann die Kaltstartzeit von JavaScript-Funktionen reduzieren.

Weitere Informationen dazu finden Sie in [dieser Ankündigung](https://github.com/Azure/app-service-announcements/issues/84).

## <a name="enabling-functions-to-run-from-a-package"></a>Aktivieren von Funktionen zum Ausführen aus einem Paket

Damit Ihre Funktions-App aus einem Paket ausgeführt werden kann, müssen Sie nur eine Einstellung `WEBSITE_RUN_FROM_ZIP` zu Ihren Einstellungen in der Funktions-App hinzufügen. Die Einstellung `WEBSITE_RUN_FROM_ZIP` kann einen der folgenden Werte aufweisen:

| Wert  | BESCHREIBUNG  |
|---------|---------|
|**`<url>`**  | Speicherort der spezifischen Paketdatei, die Sie ausführen möchten. Wenn Sie Blob Storage müssen Sie einen privaten Container mit einer [Shared Access Signature (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#attach-a-storage-account-by-using-a-shared-access-signature-sas) verwenden, um die Functions-Laufzeit für den Zugriff auf das Paket zu aktivieren. Sie können den [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) zum Hochladen von Dateien in Ihr Blob Storage-Konto verwenden.         |
| **`1`**  | Ausführen aus einer Paketdatei im Ordner `d:\home\data\SitePackages` der Funktions-App. Für diese Option muss der Ordner zudem eine Datei mit dem Namen `packagename.txt` enthalten. Diese Datei enthält nur den Namen der Paketdatei im Ordner ohne Leerzeichen. |

Das folgende Beispiel zeigt eine Funktions-App, die so konfiguriert ist, dass Sie über eine ZIP-Datei ausgeführt wird, die im Azure Blob Storage gehostet wird:

![WEBSITE_RUN_FROM_ZIP-Appeinstellung](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> Derzeit werden nur ZIP-Paketdateien unterstützt.

## <a name="integration-with-zip-deployment"></a>Integration mit einer ZIP-Bereitstellung

[ZIP-Bereitstellung] [ Zip deployment for Azure Functions] ist eine Funktion von Azure App Service, mit dem Sie Ihr Funktions-App-Projekt im `wwwroot`-Verzeichnis bereitstellen können. Das Projekt ist als ZIP-Bereitstellungsdatei verpackt. Mit derselben API können Sie Ihr Paket im Ordner `d:\home\data\SitePackages` bereitstellen. Mit dem `WEBSITE_RUN_FROM_ZIP`-App-Einstellungswert von `1` kopieren die ZIP-Bereitstellungs-APIs Ihr Paket in den Ordner `d:\home\data\SitePackages`, anstatt die Dateien nach `d:\home\site\wwwroot` zu extrahieren. Außerdem wird die Datei `packagename.txt` erstellt. Die Funktions-App wird dann nach einem Neustart aus dem Paket heraus ausgeführt und `wwwroot` wird schreibgeschützt. Weitere Informationen zur ZIP-Bereitstellung finden Sie unter [ZIP-Bereitstellung für Azure Functions](deployment-zip-push.md).

## <a name="adding-the-websiterunfromzip-setting"></a>Hinzufügen der WEBSITE_RUN_FROM_ZIP-Einstellung

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Kontinuierliche Bereitstellung für Azure Functions](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
