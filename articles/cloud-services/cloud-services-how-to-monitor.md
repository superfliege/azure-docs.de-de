---
title: "Überwachen eines Azure-Clouddiensts| Microsoft Docs"
description: "Hier wird beschrieben, welche Aspekte die Überwachung eines Azure-Clouddiensts umfasst und welche Optionen verfügbar sind."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: adegeo
ms.openlocfilehash: c63a49c65f2d8261caa534308477888c752a89da
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2018
---
# <a name="introduction-to-cloud-service-monitoring"></a>Einführung in die Überwachung von Clouddiensten

Sie können wichtige Leistungsmetriken für beliebige Clouddienste überwachen. Für jede Clouddienstrolle werden minimale Daten zur CPU-Nutzung, Netzwerkverwendung und Datenträgerverwendung erfasst. Wenn auf eine Rolle des Clouddiensts die Erweiterung `Microsoft.Azure.Diagnostics` angewendet wurde, können für die Rolle zusätzliche Datenpunkte erfasst werden. Dieser Artikel bietet eine Einführung in die Azure-Diagnose für Clouddienste.

Bei der grundlegenden Überwachung werden alle drei Minuten Leistungsindikatordaten aus den Rolleninstanzen abgefragt und erfasst. Daten aus der grundlegenden Überwachung werden nicht unter Ihrem Speicherkonto gespeichert und verursachen keine zusätzlichen Kosten.

Bei der erweiterten Überwachung werden in Intervallen von fünf Minuten, einer Stunde und zwölf Stunden zusätzliche Metriken abgefragt und erfasst. Die aggregierten Daten werden unter einem Speicherkonto in Tabellen gespeichert und nach zehn Tagen gelöscht. Das verwendete Speicherkonto wird nach der Rolle konfiguriert. Sie können verschiedene Speicherkonten für verschiedene Rollen verwenden. Die Konfiguration erfolgt mithilfe einer Verbindungszeichenfolge in der [CSDEF](cloud-services-model-and-package.md#servicedefinitioncsdef)- und [CSCFG](cloud-services-model-and-package.md#serviceconfigurationcscfg)-Datei.


## <a name="basic-monitoring"></a>Grundlegende Überwachung

Wie in der Einführung beschrieben, erfasst ein Clouddienst automatisch grundlegende Überwachungsdaten vom virtuellen Hostcomputer. Zu diesen Daten gehören: CPU-Prozentsatz, Netzwerk-E/A und Lese-/Schreibzugriffe auf Datenträger. Die erfassten Überwachungsdaten werden automatisch auf den Übersichts- und Metrikseiten des Clouddiensts im Azure-Portal angezeigt. 

Für die grundlegende Überwachung wird kein Speicherkonto benötigt. 

![Kacheln der grundlegenden Clouddienstüberwachung](media/cloud-services-how-to-monitor/basic-tiles.png)

## <a name="advanced-monitoring"></a>Erweiterte Überwachung

Bei der erweiterten Überwachung wird die Azure-Diagnoseerweiterung (und optional das Application Insights SDK) für die zu überwachende Rolle verwendet. Die Diagnoseerweiterung verwendet (pro Rolle) eine Konfigurationsdatei mit dem Namen **diagnostics.wadcfgx** zur Konfiguration der überwachten Diagnosemetriken. Die von der Azure-Diagnoseerweiterung erfassten Daten werden unter einem Azure Storage-Konto gespeichert, das in den **WADCFGX**-, [CSDEF](cloud-services-model-and-package.md#servicedefinitioncsdef)- und [CSCFG](cloud-services-model-and-package.md#serviceconfigurationcscfg)-Dateien konfiguriert wird. Dies bedeutet, dass die erweiterte Überwachung mit zusätzlichen Kosten verbunden ist.

Während der Erstellung einer Rolle wird ihr von Visual Studio die Azure-Diagnoseerweiterung hinzugefügt. Diese Erweiterung kann die folgenden Informationstypen erfassen:

* Benutzerdefinierte Leistungsindikatoren
* Anwendungsprotokolle
* Windows-Ereignisprotokolle
* .NET-Ereignisquelle
* IIS-Protokolle
* Manifestbasiertes ETW
* Absturzabbilder
* Benutzerdefinierte Fehlerprotokolle

Die Daten werden zwar vollständig unter dem Speicherkonto aggregiert, allerdings bietet das Portal keine native Möglichkeit, die Daten in einem Diagramm darzustellen. Sie können einen anderen Dienst wie Application Insights nutzen, um die Daten zu korrelieren und darzustellen.

### <a name="use-application-insights"></a>Verwenden von Application Insights

Wenn Sie den Clouddienst über Visual Studio veröffentlichen, können Sie die Diagnosedaten an Application Insights senden. Sie können die Application Insights-Ressource dann erstellen oder die Daten an eine vorhandene Ressource senden. Ihr Clouddienst kann von Application Insights auf Verfügbarkeit, Leistung, Fehler und Auslastung überwacht werden. Application Insights können benutzerdefinierte Diagramme hinzugefügt werden. So haben Sie immer die für Sie relevanten Daten im Blick. Rolleninstanzdaten können mithilfe des Application Insights SDKs in Ihrem Clouddienstprojekt erfasst werden. Weitere Informationen zur Integration von Application Insights finden Sie unter [Application Insights für Azure Cloud Services](../application-insights/app-insights-cloudservices.md).

Hinweis: Sie können die Leistungsindikatoren (und weiteren Einstellungen), die Sie über die Windows Azure-Diagnoseerweiterung festgelegt haben, mithilfe von Application Insights anzeigen, erhalten allerdings noch bessere Darstellungsmöglichkeiten, wenn Sie das Application Insights SDK in die Worker- und Webrollen integrieren.


## <a name="add-advanced-monitoring"></a>Hinzufügen der erweiterten Überwachung

Wenn Sie noch nicht über ein **klassisches** Speicherkonto verfügen, [erstellen Sie zunächst ein Konto](../storage/common/storage-create-storage-account.md#create-a-storage-account). Stellen Sie sicher, dass das Speicherkonto mit dem **klassischen Bereitstellungsmodell** erstellt wird.

Navigieren Sie als Nächstes zur Ressource **Speicherkonto (klassisch)**. Wählen Sie **Einstellungen** > **Zugriffsschlüssel** aus, und kopieren Sie den Wert unter **Primäre Verbindungszeichenfolge**. Sie benötigen diesen Wert für den Clouddienst. 

Es gibt zwei Konfigurationsdateien, die Sie zur Aktivierung der erweiterten Diagnose ändern müssen: **ServiceDefinition.csdef** und **ServiceConfiguration.cscfg**. Wahrscheinlich verfügen Sie über zwei **CSCFG**-Dateien, eine mit dem Namen **ServiceConfiguration.cloud.cscfg** für die Bereitstellung in Azure und eine mit dem Namen **ServiceConfiguration.local.cscfg**, die für lokale Debugbereitstellungen verwendet wird. Ändern Sie beide.

Fügen Sie in der Datei **ServiceDefinition.csdef** für jede Rolle, für die die erweiterte Diagnose verwendet werden soll, eine neue Einstellung mit dem Namen `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` hinzu. Dieser Wert wird der Datei von Visual Studio hinzugefügt, wenn Sie ein neues Projekt erstellen. Falls sie fehlt, können Sie sie jetzt hinzufügen. 

```xml
<ServiceDefinition name="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
```

Dadurch wird eine neue Einstellung definiert, die jeder Datei **ServiceConfiguration.cscfg** hinzugefügt werden muss. Öffnen und ändern Sie jede **CSCFG**-Datei. Fügen Sie eine Einstellung mit dem Namen `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` hinzu. Legen Sie den Wert entweder auf die **Primäre Verbindungszeichenfolge** des klassischen Speicherkontos fest oder auf `UseDevelopmentStorage=true`, wenn Sie den lokalen Speicher auf Ihrem Entwicklungscomputer verwenden möchten.

```xml
<ServiceConfiguration serviceName="AnsurCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="WorkerRoleWithSBQueue1">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=mystorage;AccountKey=KWwkdfmskOIS240jnBOeeXVGHT9QgKS4kIQ3wWVKzOYkfjdsjfkjdsaf+sddfwwfw+sdffsdafda/w==" />

<!-- or use the local development machine for storage
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
-->
```

## <a name="next-steps"></a>Nächste Schritte

- [Informationen zu Application Insights mit Cloud Services](../application-insights/app-insights-cloudservices.md)

