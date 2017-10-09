---
title: "Referenz zu App-Einstellungen für Azure Functions"
description: "Referenzdokumentation für die App-Einstellungen für Azure Functions oder Umgebungsvariablen."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/26/2017
ms.author: tdykstra
ms.translationtype: HT
ms.sourcegitcommit: 8ad98f7ef226fa94b75a8fc6b2885e7f0870483c
ms.openlocfilehash: ce7bf2cf650b0df7e8998766b2d3f5a37c4a1b72
ms.contentlocale: de-de
ms.lasthandoff: 09/29/2017

---

# <a name="app-settings-reference-for-azure-functions"></a>Referenz zu App-Einstellungen für Azure Functions

App-Einstellungen in einer Funktionen-App enthalten globale Konfigurationsoptionen, die sich auf alle Funktionen dieser Funktionen-App auswirken. Bei der lokalen Ausführung befinden sich diese Einstellungen in Umgebungsvariablen. In diesem Artikel werden die in Funktionen-Apps verfügbaren App-Einstellungen aufgelistet.

Es gibt andere globale Konfigurationsoptionen in der Datei [host.json](functions-host-json.md) und in der Datei [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="appinsightsinstrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Der Application Insights-Instrumentierungsschlüssel, wenn Sie Application Insights verwenden. Weitere Informationen finden Sie unter [Überwachen von Azure Functions](functions-monitoring.md).

|Schlüssel|Beispielwert|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Optionale Verbindungszeichenfolge für das Speicherkonto zum Speichern von Protokollen und zu deren Anzeige auf der Registerkarte **Überwachen** im Portal. Das Speicherkonto muss ein allgemeines Konto sein, das BLOBs, Warteschlangen und Tabellen unterstützt. Weitere Informationen finden Sie unter [Speicherkonto](functions-infrastructure-as-code.md#storage-account) und [Anforderungen an das Speicherkonto](functions-create-function-app-portal.md#storage-account-requirements).

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol=https;AccountName=[Name];AccountKey=[Schlüssel]|

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true` bedeutet, dass die standardmäßige Landing Page deaktiviert wird, die für die Stamm-URL einer Funktionen-App angezeigt wird. Der Standardwert ist `false`.

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsDisableHomepage|true|

Wenn diese App-Einstellung ausgelassen oder auf `false` gesetzt wird, erfolgt als Reaktion auf die URL `<functionappname>.azurewebsites.net` die Anzeige einer Seite, die dem folgenden Beispiel ähnelt.

![Landing Page der Funktionen-App](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true` bedeutet, dass beim Kompilieren von .NET-Code der Releasemodus verwendet wird, während `false` bedeutet, dass der Debugmodus verwendet wird. Der Standardwert ist `true`.

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Eine durch Trennzeichen getrennte Liste der zu aktivierenden Features der Betaversion. Durch diese Flags aktivierten Features der Betaversion sind nicht bereit für die Produktion, können aber zur experimentellen Verwendung aktiviert werden, bevor sie live geschaltet werden.

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsFeatureFlags|Feature1,Feature2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

Der Pfad zum Stammverzeichnis, in dem sich die Datei *host.json* sowie Funktionsordner befinden. In einer Funktionen-App ist `%HOME%\site\wwwroot` der Standardwert.

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsScriptRoot|%HOME%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Gibt das Repository oder den Anbieter an, das bzw. der zum Speichern von Schlüsseln verwendet wird. Aktuell sind die unterstützten Repositorys „BLOB“ („Blob“) und „Dateiystem“ („deaktiviert“). Der Standardwert ist „Dateisystem“ („deaktiviert“).

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsSecretStorageType|deaktiviert|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Die Azure Functions-Laufzeit verwendet diese Verbindungszeichenfolge des Speicherkontos für alle Funktionen, mit Ausnahme der per HTTP ausgelösten Funktionen. Das Speicherkonto muss ein allgemeines Konto sein, das BLOBs, Warteschlangen und Tabellen unterstützt. Weitere Informationen finden Sie unter [Speicherkonto](functions-infrastructure-as-code.md#storage-account) und [Anforderungen an das Speicherkonto](functions-create-function-app-portal.md#storage-account-requirements).

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol=https;AccountName=[Name];AccountKey=[Schlüssel]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

Der Pfad zum Compiler, der für TypeScript verwendet wird. Bei Bedarf können Sie den Standardwert außer Kraft setzen.

|Schlüssel|Beispielwert|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="functionappeditmode"></a>FUNCTION\_APP\_EDIT\_MODE

Gültige Werte sind „readwrite“ und „readonly“.

|Schlüssel|Beispielwert|
|---|------------|
|FUNCTION\_APP\_EDIT\_MODE|readonly|

## <a name="functionsextensionversion"></a>FUNCTIONS\_EXTENSION\_VERSION

Die in dieser Funktionen-App zu verwendende Version der Azure Functions-Laufzeit. Eine Tilde mit Hauptversion bedeutet, dass die neueste Version dieser Hauptversion (z. B. „~1“) verwendet wird. Wenn neue Versionen für dieselbe Hauptversion verfügbar sind, werden sie automatisch in der Funktionen-App installiert. Verwenden Sie die vollständige Versionsnummer (z. B. „1.0.12345“), um die App an eine bestimmte Version anzuheften. Der Standardwert ist „~1“.

|Schlüssel|Beispielwert|
|---|------------|
|FUNCTIONS\_EXTENSION\_VERSION|~1|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Nur für Verbrauchstarife. Die Verbindungszeichenfolge für das Speicherkonto, in dem der Code der Funktionen-App und die Konfiguration gespeichert werden. Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App](functions-infrastructure-as-code.md#create-a-function-app).

|Schlüssel|Beispielwert|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol=https;AccountName=[Name];AccountKey=[Schlüssel]|

## <a name="websitecontentshare"></a>WEBSITE_CONTENTSHARE

Nur für Verbrauchstarife. Der Dateipfad zum Code der Funktionen-App und zur Konfiguration. Wird mit WEBSITE_CONTENTAZUREFILECONNECTIONSTRING verwendet. Standardmäßig wird eine eindeutige Zeichenfolge verwendet, die mit dem Namen der Funktionen-App beginnt. Weitere Informationen finden Sie unter [Erstellen einer Funktionen-App](functions-infrastructure-as-code.md#create-a-function-app).

|Schlüssel|Beispielwert|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT

Die maximale Anzahl der Instanzen, auf denen die Funktionen-App horizontal hochskaliert werden kann. Dieser Wert ist standardmäßig unbegrenzt.

> [!NOTE]
> Diese Einstellung ist für ein Vorschaufeature.

|Schlüssel|Beispielwert|
|---|------------|
|WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT|10|

## <a name="websitenodedefaultversion"></a>WEBSITE\_NODE\_DEFAULT_VERSION

Der Standardwert ist „6.5.0“.

|Schlüssel|Beispielwert|
|---|------------|
|WEBSITE\_NODE\_DEFAULT_VERSION|6.5.0|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Aktualisieren von App-Einstellungen](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

> [!div class="nextstepaction"]
> [Weitere Informationen finden Sie unter den globalen Einstellungen in der Datei host.json](functions-host-json.md)

