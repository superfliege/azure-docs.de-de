---
title: Anleitung zur Bereitstellung von Azure-Web-Apps mit Vorlagen | Microsoft-Dokumentation
description: Empfehlungen zum Erstellen von Azure Resource Manager-Vorlagen zum Bereitstellen von Web-Apps
services: app-service
documentationcenter: app-service
author: tfitzmac
manager: timlt
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: tomfitz
ms.openlocfilehash: eff0b0e6258217fa8fbf7f15606f98d70fecd7c5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="guidance-on-deploying-web-apps-with-azure-resource-manager-templates"></a>Anleitung zum Bereitstellen von Web-Apps mit Azure Resource Manager-Vorlagen

Dieser Artikel enthält Empfehlungen zum Erstellen von Azure Resource Manager-Vorlagen zum Bereitstellen von App Service-Lösungen. Diese Empfehlungen helfen Ihnen dabei, häufige Probleme zu vermeiden.

## <a name="define-dependencies"></a>Definieren von Abhängigkeiten

Das Definieren von Abhängigkeiten für Web-Apps erfordert Kenntnisse über die Interaktion der Ressourcen innerhalb einer Web-App. Wenn Sie die Abhängigkeiten in der falschen Reihenfolge angeben, treten möglicherweise Bereitstellungsfehler auf, oder es entsteht eine Racebedingung, die die Bereitstellung stoppt.

> [!WARNING]
> Wenn Sie eine MS Deploy-Websiteerweiterung in die Vorlage einfügen, müssen Sie alle Konfigurationsressourcen als von der MS Deploy-Ressource abhängig festlegen. Änderungen an der Konfiguration führen zu einem asynchronen Neustart der Website. Wenn Sie die Konfigurationsressourcen von MS Deploy abhängig machen, stellen Sie sicher, dass MS Deploy abgeschlossen wird, bevor die Website neu gestartet wird. Ohne diese Abhängigkeiten wird die Website eventuell während des MS Deploy-Bereitstellungsprozesses neu gestartet. Eine Beispielvorlage finden Sie unter [WordPress-Vorlage mit Web Deploy-Abhängigkeit](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

Die folgende Abbildung zeigt die Reihenfolge der Abhängigkeiten für verschiedene App Service-Ressourcen.

![Web-App-Abhängigkeiten](media/web-sites-rm-template-guidance/web-dependencies.png)

Sie stellen Ressourcen in der folgenden Reihenfolge bereit:

**Ebene 1**
* App Service-Plan
* Alle anderen verwandten Ressourcen wie Datenbanken oder Speicherkonten

**Ebene 2**
* Web-App – hängt vom App Service-Plan ab
* Application Insights mit der Serverfarm als Ziel – hängt vom App Service-Plan ab

**Ebene 3**
* Quellcodeverwaltung – hängt von der Web-App ab
* MS Deploy-Websiteerweiterung – hängt von der Web-App ab
* Application Insights mit der Serverfarm als Ziel – hängt von der Web-App ab

**Ebene 4**
* App Service Certificate – hängt von der Quellcodeverwaltung oder MS Deploy ab (falls vorhanden), andernfalls von der Web-App
* Konfigurationseinstellungen (Verbindungszeichenfolgen, Werte in „web.config“, App-Einstellungen) – hängen von der Quellcodeverwaltung oder MS Deploy ab (falls vorhanden), andernfalls von der Web-App

**Ebene 5**
* Hostnamebindungen – hängen vom Zertifikat ab (falls vorhanden), andernfalls von einer Ressource auf höherer Ebene
* Websiteerweiterungen – hängen von den Konfigurationseinstellungen ab (falls vorhanden), andernfalls von einer Ressource auf höherer Ebene

In der Regel enthält Ihre Lösung nur einige dieser Ressourcen und Ebenen. Bei fehlenden Ebenen ordnen Sie die Ressourcen auf niedrigerer Ebene der nächsthöheren Ebene zu.

Das folgende Beispiel zeigt einen Teil einer Vorlage. Der Konfigurationswert der Verbindungszeichenfolge hängt von der MS Deploy-Erweiterung ab. Die MS Deploy-Erweiterung hängt von der Web-App und der Datenbank ab.

```json
{
    "name": "[parameters('name')]",
    "type": "Microsoft.Web/sites",
    "resources": [
      {
          "name": "MSDeploy",
          "type": "Extensions",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'))]",
            "[concat('SuccessBricks.ClearDB/databases/', parameters('databaseName'))]"
          ],
          ...
      },
      {
          "name": "connectionstrings",
          "type": "config",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', parameters('name'), '/Extensions/MSDeploy')]"
          ],
          ...
      }
    ]
}
```

## <a name="find-information-about-msdeploy-errors"></a>Suchen von Informationen zu MS Deploy-Fehlern

Wenn Ihre Resource Manager-Vorlage MS Deploy verwendet, sind die Bereitstellungsfehlermeldungen möglicherweise schwer verständlich. Probieren Sie die folgenden Schritte aus, um nach einer fehlerhaften Bereitstellung weitere Informationen zu erhalten:

1. Wechseln Sie zur [Kudu-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console) der Website.
2. Navigieren Sie zum Ordner `D:\home\LogFiles\SiteExtensions\MSDeploy`.
3. Suchen Sie nach den Dateien *appManagerStatus.xml* und *appManagerLog.xml*. Die erste Datei protokolliert den Status. Die zweite Datei protokolliert Informationen zum Fehler. Wenn Ihnen der Fehler nicht klar ist, können Sie sie bei Nachfragen im Forum einfügen.

## <a name="unique-web-app-name"></a>Eindeutiger Web-App-Name

Der Name Ihrer Web-App muss global eindeutig sein. Sie können eine Namenskonvention verwenden, die höchstwahrscheinlich eindeutig ist, oder Sie können beim Erstellen eines eindeutigen Namens die [uniqueString-Funktion](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) verwenden.

```json
{
  "apiVersion": "2016-08-01",
  "name": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Web/sites",
  ...
}
```

## <a name="next-steps"></a>Nächste Schritte

* Ein Tutorial zum Bereitstellen von Web-Apps mit einer Vorlage finden Sie unter [Vorhersagbares Bereitstellen von Microservices in Azure](app-service-deploy-complex-application-predictably.md).