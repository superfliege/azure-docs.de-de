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
ms.openlocfilehash: dc816bb6e95d2800d79124dfac60b55e88eaa500
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="guidance-on-deploying-web-apps-by-using-azure-resource-manager-templates"></a>Anleitung zum Bereitstellen von Web-Apps mit Azure Resource Manager-Vorlagen

Dieser Artikel enthält Empfehlungen zum Erstellen von Azure Resource Manager-Vorlagen für die Bereitstellung von Azure App Service-Lösungen. Diese Empfehlungen helfen Ihnen dabei, häufige Probleme zu vermeiden.

## <a name="define-dependencies"></a>Definieren von Abhängigkeiten

Das Definieren von Abhängigkeiten für Web-Apps erfordert Kenntnisse in Bezug auf die Interaktion der Ressourcen innerhalb einer Web-App. Wenn Sie die Abhängigkeiten in der falschen Reihenfolge angeben, treten unter Umständen Bereitstellungsfehler auf, oder es entsteht eine Racebedingung, die die Bereitstellung stoppt.

> [!WARNING]
> Wenn Sie eine MSDeploy-Websiteerweiterung in die Vorlage einfügen, müssen Sie alle Konfigurationsressourcen als von der MSDeploy-Ressource abhängig festlegen. Änderungen an der Konfiguration führen zu einem asynchronen Neustart der Website. Indem Sie die Konfigurationsressourcen von MSDeploy abhängig machen, stellen Sie sicher, dass MSDeploy abgeschlossen wird, bevor die Website neu gestartet wird. Ohne diese Abhängigkeiten wird die Website eventuell während des Bereitstellungsprozesses von MSDeploy neu gestartet. Eine Beispielvorlage finden Sie unter [WordPress-Vorlage mit Web Deploy-Abhängigkeit](https://github.com/davidebbo/AzureWebsitesSamples/blob/master/ARMTemplates/WordpressTemplateWebDeployDependency.json).

Die folgende Abbildung zeigt die Reihenfolge der Abhängigkeiten für verschiedene App Service-Ressourcen:

![Web-App-Abhängigkeiten](media/web-sites-rm-template-guidance/web-dependencies.png)

Sie stellen Ressourcen in der folgenden Reihenfolge bereit:

**Ebene 1**
* App Service-Plan.
* Alle anderen verwandten Ressourcen, z.B. Datenbanken oder Speicherkonten.

**Ebene 2**
* Web-App: Hängt vom App Service-Plan ab.
* Azure Application Insights-Instanz, die auf die Serverfarm ausgerichtet ist: Hängt vom App Service-Plan ab.

**Ebene 3**
* Quellcodeverwaltung: Hängt von der Web-App ab.
* MSDeploy-Websiteerweiterung: Hängt von der Web-App ab.
* Application Insights-Instanz, die auf die Serverfarm ausgerichtet ist: Hängt von der Web-App ab.

**Ebene 4**
* App Service Certificate: Hängt von der Quellcodeverwaltung oder MSDeploy ab, falls vorhanden. Hängt andernfalls von der Web-App ab.
* Konfigurationseinstellungen (Verbindungszeichenfolgen, Werte in „web.config“, App-Einstellungen): Hängen von der Quellcodeverwaltung oder von MSDeploy ab, falls vorhanden. Hängen andernfalls von der Web-App ab.

**Ebene 5**
* Hostnamenbindungen: Hängen vom Zertifikat ab, falls vorhanden. Hängen andernfalls von einer Ressource auf höherer Ebene ab.
* Websiteerweiterungen: Hängen von den Konfigurationseinstellungen ab, falls vorhanden. Hängen andernfalls von einer Ressource auf höherer Ebene ab.

In der Regel enthält Ihre Lösung nur einige dieser Ressourcen und Ebenen. Bei fehlenden Ebenen ordnen Sie die Ressourcen auf niedrigerer Ebene der nächsthöheren Ebene zu.

Das folgende Beispiel zeigt einen Teil einer Vorlage. Der Wert der Verbindungszeichenfolgen-Konfiguration hängt von der MSDeploy-Erweiterung ab. Die MS Deploy-Erweiterung hängt von der Web-App und der Datenbank ab.

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
2. Navigieren Sie zum Ordner „D:\home\LogFiles\SiteExtensions\MSDeploy“.
3. Suchen Sie nach den Dateien „appManagerStatus.xml“ und „appManagerLog.xml“. Die erste Datei protokolliert den Status. Die zweite Datei protokolliert Informationen zum Fehler. Wenn Ihnen der Fehler nicht klar ist, können Sie die Informationen bei Nachfragen im Forum einfügen.

## <a name="choose-a-unique-web-app-name"></a>Auswählen eines eindeutigen Web-App-Namens

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