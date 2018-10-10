---
title: Übersicht über die Runtimeversionen von Azure Functions
description: Azure Functions unterstützt mehrere Versionen der Runtime. Lernen Sie die Unterschiede kennen, und erfahren Sie, wie Sie die Version auswählen, die sich am besten für Ihre Anforderungen eignet.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: glenga
ms.openlocfilehash: a601ea42549abad84d6cab5c429cf94147776436
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978623"
---
# <a name="azure-functions-runtime-versions-overview"></a>Übersicht über die Runtimeversionen von Azure Functions

 Es gibt zwei Hauptversionen der Azure Functions-Runtime: 1.x und 2.x. Die aktuelle Version, in der neue Funktionen eingeführt und Verbesserungen vorgenommen werden, ist 2.x. Es werden jedoch beide Versionen für Produktionsszenarien unterstützt.  Im Folgenden werden einige der Unterschiede zwischen den beiden Versionen erläutert. Zudem wird beschrieben, wie Sie Apps in beiden Versionen erstellen und von 1.x auf 2.x aktualisieren.

> [!NOTE] 
> Dieser Artikel bezieht sich auf den Clouddienst Azure Functions. Informationen zum Vorschauprodukt, mit dem Sie Azure Functions lokal ausführen können, finden Sie in der [Übersicht über die Azure Functions-Runtime](functions-runtime-overview.md).

## <a name="creating-1x-apps"></a>Erstellen von 1.x-Apps

Neue Apps, die im Azure-Portal erstellt werden, werden standardmäßig auf 2.x festgelegt, da dies die aktuellste Version ist, der neue Funktionen hinzugefügt werden.  Sie können jedoch wie im Folgenden beschrieben weiterhin v1.x-Apps erstellen.

1. Erstellen einer Azure-Funktions-App über das Azure-Portal
1. Öffnen Sie die erstellte App, und öffnen Sie dann in der leeren App die **Funktionseinstellungen**.
1. Ändern Sie die Version von ~2 in ~1.  *Diese Umschaltfläche ist deaktiviert, wenn die App Funktionen enthält*.
1. Klicken Sie auf „Speichern“, und starten Sie die App neu.  Alle Vorlagen sollten jetzt in 1.x erstellt und ausgeführt werden.

## <a name="cross-platform-development"></a>Plattformübergreifende Entwicklung

Runtime 1.x unterstützt die Entwicklung und das Hosting nur im Portal oder unter Windows. Runtime 2.x wird auf .NET Core 2 ausgeführt, kann also auf allen Plattformen ausgeführt werden, die von .NET Core unterstützt werden, einschließlich macOS und Linux. Dies ermöglicht plattformübergreifende Entwicklungs- und Hostingszenarien.

## <a name="languages"></a>Sprachen

Runtime 2.x verwendet ein neues Modell für die Erweiterbarkeit von Sprachen. Darüber hinaus ist jede App in 2.x zur Verbesserung der Tools und Leistung auf Funktionen in nur einer Sprache beschränkt. Die derzeit in 2.x unterstützten Sprachen sind C#, F#, JavaScript und Java. Experimentelle Sprachen von Azure Functions 1.x wurden nicht für die Verwendung des neuen Modells aktualisiert und werden daher in 2.x nicht unterstützt. Die folgende Tabelle zeigt, welche Programmiersprachen in den einzelnen Runtimeversionen unterstützt werden.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Weitere Informationen finden Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="migrating-from-1x-to-2x"></a>Migrieren von 1.x zu 2.x

Möglicherweise möchten Sie eine vorhandene App, die in 1.x geschrieben wurde, zu 2.x migrieren.  Die meisten der Punkte, die beim Migrieren zwischen Versionen beachtet werden müssen, betreffen die oben genannten Änderungen der Language Runtime (z. B. C# bei der Migration von .NET Framework 4.7 zu .NET Core 2).  Sie müssen sicherstellen, dass Ihr Code und Ihre Bibliotheken mit den verwendeten Language Runtimes kompatibel sind.  Beachten Sie auch die unten genannten Änderungen an Triggern, Bindungen und Funktionen.

### <a name="changes-in-triggers-and-bindings"></a>Änderungen an Triggern und Bindungen

Die meisten Eigenschaften und Konfigurationen für Trigger und Bindungen bleiben zwischen Versionen unverändert. In 2.x müssen Sie jedoch alle Trigger oder Bindungen in der App installieren. Die einzige Ausnahme hiervon sind die HTTP- und Timertrigger.  Weitere Informationen finden Sie unter [Registrieren und Installieren von Bindungserweiterungen](./functions-triggers-bindings.md#register-binding-extensions).  Beachten Sie, dass zwischen Versionen auch Änderungen an der Datei `function.json` oder den Attributen der Funktion vorgenommen werden können (die `connection`-Eigenschaft von CosmosDB ist jetzt beispielsweise `ConnectionStringSetting`).  Links zur Dokumentation für die einzelnen Bindungen finden Sie in der [Tabelle der vorhandenen Bindungen](#bindings).

### <a name="changes-in-features-available"></a>Änderungen an verfügbaren Funktionen

Zusätzlich zu den Änderungen der Programmiersprachen und Bindungen wurden zwischen den Versionen einige Funktionen entfernt, aktualisiert oder ersetzt.  Im Folgenden sind einige der wichtigsten Punkte aufgeführt, die nach dem Wechsel von 1.x zu 2.x zu beachten sind.  In v2.x wurden die folgenden Änderungen vorgenommen:

* Schlüssel zum Aufrufen einer Funktion werden immer im verschlüsselten Blobspeicher gespeichert. In 1.x wurden sie standardmäßig im Dateispeicher gespeichert und konnten in den Blobspeicher verschoben werden, wenn Funktionen wie Slots aktiviert wurden.  Wenn Sie eine 1.x-App auf 2.x aktualisieren und geheime Schlüssel im Dateispeicher vorhanden sind, werden sie zurückgesetzt.
* Zur Verbesserung der Leistung wurden „Webhook“-Trigger entfernt und durch „HTTP“-Trigger ersetzt.
* Die Hostkonfiguration (`host.json`) muss entweder leer sein oder `2.0` als `version` für eine der Eigenschaften enthalten.
* Zur Verbesserung der Überwachung und Beobachtbarkeit wurde das WebJobs-Dashboard (`AzureWebJobsDashboard`) durch [Azure Application Insights](functions-monitoring.md) (`APPINSIGHTS_INSTRUMENTATIONKEY`) ersetzt.
* Anwendungseinstellungen (`local.settings.json`) erfordern einen Wert für die `FUNCTIONS_WORKER_RUNTIME`-Eigenschaft, der der Programmiersprache der App `dotnet | node | java | python` entspricht.
    * Zur Verbesserung des Speicherbedarfs und der Startzeit sind Apps auf eine einzige Programmiersprache beschränkt. Sie können mehrere Apps veröffentlichen, um Funktionen in verschiedenen Sprachen für die gleiche Lösung bereitzustellen.
* Das Standardzeitlimit für Funktionen in einem App Service-Plan beträgt 30 Minuten.  Es kann jedoch manuell auf „Unbegrenzt“ festgelegt werden.
* [Aufgrund der Beschränkungen von .NET Core](https://github.com/Azure/azure-functions-host/issues/3414) `.fsx` wurden Skripts für F#-Funktionen entfernt. Kompilierte F#-Funktionen werden weiterhin unterstützt.
* Das Format von Webhook-basierten Triggern (z. B. Event Grid) wurde in `https://{app}/runtime/webhooks/{triggerName}` geändert.

### <a name="upgrading-a-locally-developed-application"></a>Durchführen eines Upgrades für eine lokal entwickelte Anwendung

Wenn Ihre v1.x-App lokal entwickelt wurde, können Sie Änderungen an der App oder dem Projekt vornehmen, damit die App mit v2 kompatibel ist.  Es wird empfohlen, eine neue App zu erstellen und den Code zur neuen App zu portieren.  An einer vorhandenen App können Änderungen vorgenommen werden, um ein direktes Upgrade auszuführen, allerdings bietet v2 zahlreiche weitere Verbesserungen gegenüber v1, die in Legacycode wahrscheinlich nicht genutzt werden (z. B. in C# die Änderung von `TraceWriter` in `ILogger`).  

Es wird empfohlen, mit einer der v2-Vorlagen zu beginnen und Code in ein neues Projekt oder eine neue App zu verschieben.

#### <a name="visual-studio-runtime-versions"></a>Visual Studio-Runtimeversionen

In Visual Studio wählen Sie die Runtimeversion beim Erstellen eines Projekts aus.  Visual Studio verfügt über die Bits für beide Hauptversionen und kann dynamisch die jeweils richtige Version für das Projekt nutzen.  Diese Einstellungen werden aus der `.csproj`-Datei abgeleitet.  Für 1.x-Apps enthält das Projekt folgende Eigenschaften:

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

In v2 lauten die Projekteigenschaften wie folgt:

```xml
<TargetFramework>netstandard2.0</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Wenn Sie auf „Debuggen“ oder „Veröffentlichen“ klicken, wird die richtige Version für die Projekteinstellungen festgelegt.

#### <a name="vs-code-and-azure-functions-core-tools"></a>Visual Studio Code und Azure Functions Core Tools

Andere lokale Tools basieren auf Azure Functions Core Tools.  Diese Tools werden auf dem Computer installiert, und in der Regel wird jeweils nur eine Version auf einem Entwicklungscomputer installiert.  Lesen Sie hierzu die [Anweisungen zum Installieren bestimmter Versionen von Core Tools](./functions-run-local.md).

Für Visual Studio Code müssen Sie möglicherweise auch die Benutzereinstellung für die `azureFunctions.projectRuntime` entsprechend der installierten Version der Tools aktualisieren.  Dadurch werden auch die Vorlagen und Sprachen aktualisiert, die während der Erstellung von neuen Apps angezeigt werden.

### <a name="changing-version-of-apps-in-azure"></a>Ändern der Version von Apps in Azure

Veröffentlichte App-Versionen werden über die Anwendungseinstellung `FUNCTIONS_RUNTIME_VERSION` festgelegt.  Diese Einstellung wird für v2-Apps auf `~2` festgelegt und für v1-Apps auf `~1`.  Es wird dringend davon abgeraten, die Runtimeversion einer App zu ändern, in der vorhandene Funktionen veröffentlicht wurden, ohne auch den Code für diese Funktionen zu ändern.  Wir empfehlen, eine neue Funktions-App zu erstellen, die entsprechende Version festzulegen, die Änderungen zu testen und anschließend die vorherige App zu deaktivieren oder zu löschen.

## <a name="bindings"></a>Bindungen 

Runtime 2.x verwendet ein neues [Modell für die Erweiterbarkeit von Bindungen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview), das folgende Vorteile bietet:

* Unterstützung für Bindungserweiterungen von Drittanbietern.
* Entkoppeln von Runtime und Bindungen. Damit können Bindungserweiterungen versioniert und unabhängig freigegeben werden. Sie können z.B. ein Upgrade auf eine Version einer Erweiterung durchführen, das auf einer neueren Version des zugrunde liegenden SDKs basiert.
* Eine schlankere Ausführungsumgebung, in der nur die tatsächlich verwendeten Bindungen bekannt sind und von der Runtime geladen werden.

Dieses Modell wurde für alle integrierten Azure Functions-Bindungen übernommen, sodass diese mit Ausnahme des Timer- und des HTTP-Triggers nicht mehr standardmäßig inbegriffen sind. Diese Erweiterungen werden automatisch installiert, wenn Sie Funktionen mithilfe von Tools wie Visual Studio oder über das Portal erstellen.

Die folgende Tabelle zeigt, welche Bindungen in den einzelnen Runtimeversionen unterstützt werden.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Lokales Codieren und Testen von Azure Functions](functions-run-local.md)
* [Einstellen von Runtimeversionen von Azure Functions als Ziel](set-runtime-version.md)
* [Anmerkungen zu dieser Version](https://github.com/Azure/azure-functions-host/releases)
