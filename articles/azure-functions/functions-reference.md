---
title: Leitfaden zur Entwicklung von Azure Functions | Microsoft-Dokumentation
description: Lernen Sie die Konzepte und Techniken der Azure Functions kennen, die Sie benötigen, um alle Programmiersprachen und Bindungen übergreifend Funktionen in Azure zu entwickeln.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Entwicklerhandbuch, Azure Functions, Funktionen, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 42635852bb5c6e7b388d4dc58b9d5bfaa6212438
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725852"
---
# <a name="azure-functions-developers-guide"></a>Azure Functions: Entwicklerhandbuch
In Azure Functions nutzen bestimmte Funktionen einige wichtige technische Konzepte und Komponenten gemeinsam, unabhängig von der verwendeten Sprache oder Bindung. Bevor Sie sich mit den spezifischen Details einer bestimmten Sprache oder Bindung beschäftigen, sollten Sie diese Übersicht lesen, die für alle Funktionen gilt.

In diesem Artikel wird davon ausgegangen, dass Sie die [Übersicht zu Azure Functions](functions-overview.md) gelesen haben und mit [WebJobs SDK-Konzepten wie Triggern, Bindungen und der JobHost-Laufzeit](https://github.com/Azure/azure-webjobs-sdk/wiki) vertraut sind. Azure Functions basiert auf dem WebJobs SDK 

## <a name="function-code"></a>Funktionscode
Ein *Funktion* ist das primäre Konzept in Azure Functions. Sie schreiben Code für eine Funktion in der Sprache Ihrer Wahl und speichern die Code- und Konfigurationsdateien im gleichen Ordner. Die Konfigurationsdatei heißt `function.json` und enthält JSON-Konfigurationsdaten. Verschiedene Sprachen werden unterstützt, und für jede Sprache gibt es leicht voneinander abweichende Optionen, die sich für die jeweilige Sprache am besten eignen. 

Die Datei „function.json“ definiert die Funktionsbindungen und weitere Konfigurationseinstellungen. Die Laufzeit verwendet diese Datei, um zu ermitteln, welche Ereignisse überwacht werden sollen und wie Daten in die Funktionsausführung übergeben und aus dieser zurückgegeben werden. Im Folgenden finden Sie eine function.json-Beispieldatei.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Legen Sie die `disabled`-Eigenschaft auf `true` fest, um die Ausführung der Funktion zu verhindern.

In der Eigenschaft `bindings` werden sowohl Trigger als auch Bindungen konfiguriert. Einige Einstellungen gelten für alle Bindungen, einige Einstellungen sind spezifisch für einen bestimmten Bindungstyp. Jede Bindung erfordert folgende Einstellungen:

| Eigenschaft | Werte/Typen | Kommentare |
| --- | --- | --- |
| `type` |Zeichenfolge |Bindungstyp. Beispiel: `queueTrigger`. |
| `direction` |'in', 'out' |Gibt an, ob die Bindung zum Empfangen von Daten in der Funktion oder zum Senden von Daten aus der Funktion dient. |
| `name` |Zeichenfolge |Der Name, der für die gebundenen Daten in der Funktion verwendet wird. In C# ist dies ein Argumentname, in JavaScript ist es der Schlüssel in einer Schlüssel-Wert-Liste. |

## <a name="function-app"></a>Funktionen-App
Eine Funktions-App bietet einen Ausführungskontext in Azure, in dem Ihre Funktionen ausgeführt werden. Eine Funktionen-App besteht aus einer oder mehreren individuellen Funktionen, die zusammen vom Azure App Service verwaltet werden. Der Tarif, die kontinuierliche Bereitstellung und die Laufzeitversion sind für alle Funktionen in einer Funktionen-App gleich. Eine Funktionen-App ist somit eine Möglichkeit, mit der Sie Ihre Funktionen organisieren und kollektiv verwalten können. 

> [!NOTE]
> Ab der [Version 2.x](functions-versions.md) von Azure Functions-Runtime müssen alle Funktionen in einer Funktions-App in der gleichen Sprache geschrieben sein.

## <a name="runtime"></a>Laufzeit
Azure Functions-Runtime bzw. der Skripthost ist der zugrunde liegende Host, der auf Ereignisse lauscht, Daten erfasst und sendet und schließlich den Code ausführt. Dieser gleiche Host wird vom WebJobs SDK verwendet.

Es gibt auch ein Webhost, der HTTP-Triggeranforderungen für die Runtime verarbeitet. Durch die Verwendung von zwei Hosts kann die Runtime vom Front-End-Datenverkehr isoliert werden, der vom Webhost verwaltet wird.

## <a name="folder-structure"></a>Ordnerstruktur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Wenn Sie ein Projekt einrichten, um Funktionen für eine Funktions-App in Azure bereitzustellen, können Sie diese Ordnerstruktur als Sitecode behandeln. Es wird empfohlen, [Paketbereitstellung](deployment-zip-push.md) zum Bereitstellen des Projekts für Ihre Funktions-App in Azure zu verwenden. Sie können auch vorhandene Tools wie [Continuous Integration und Continuous Deployment](functions-continuous-deployment.md) und Azure DevOps verwenden.

> [!NOTE]
> Stellen Sie sicher, dass Sie Ihre `host.json`-Datei und die Funktionsordner direkt im Ordner `wwwroot` bereitstellen. Schließen Sie den Ordner `wwwroot` nicht in Ihre Bereitstellungen ein. Andernfalls erhalten Sie `wwwroot\wwwroot`-Ordner.

## <a id="fileupdate"></a> Aktualisieren von Funktionen-App-Dateien
Der im Azure-Portal integrierte Funktionen-Editor ermöglicht das Aktualisieren der Datei *function.json* und der Codedatei für eine Funktion. Zum Hochladen oder Aktualisieren anderer Dateien wie *package.json* oder *project.json* oder von Abhängigkeiten müssen Sie andere Bereitstellungsmethoden nutzen.

Funktionen-Apps basieren auf App Service. Daher stehen alle [für Standard-Web-Apps verfügbaren Bereitstellungsoptionen](../app-service/deploy-local-git.md) auch für Funktionen-Apps zur Verfügung. Es folgen einige Methoden, die Sie zum Hochladen oder Aktualisieren von Funktionen-App-Dateien befolgen können. 

#### <a name="use-local-tools-and-publishing"></a>Verwenden von lokalen Tools und Veröffentlichung
Funktions-Apps können mit verschiedenen Tools erstellt und veröffentlicht werden, darunter [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](functions-create-first-function-vs-code.md), [IntelliJ](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md) und die [Azure Functions-Kerntools](./functions-develop-local.md). Weitere Informationen finden Sie unter [Lokales Codieren und Testen von Azure Functions](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="continuous-deployment"></a>Kontinuierliche Bereitstellung
Befolgen Sie die Anweisungen im Thema [Continuous deployment for Azure Functions](functions-continuous-deployment.md)(Kontinuierliche Bereitstellung für Azure Functions).

## <a name="parallel-execution"></a>Parallele Ausführung
Wenn die Auslösung mehrerer Ereignisse schneller erfolgt als die Runtime einer Singlethreadfunktion sie verarbeiten kann, kann die Runtime die Funktion mehrmals parallel aufrufen.  Wenn eine Funktionen-App den [verbrauchsbasierten Hostingplan](functions-scale.md#how-the-consumption-plan-works) verwendet, kann die App automatisch horizontal hochskaliert werden.  Jede Instanz der Funktionen-App – unabhängig davon, ob die App im verbrauchsbasierten Hostingplan oder einem regulären [App Service-Hostingplan](../app-service/overview-hosting-plans.md) ausgeführt wird – kann gleichzeitige Funktionsaufrufe über mehrere Threads parallel verarbeiten.  Die maximale Anzahl gleichzeitiger Funktionsaufrufe in jeder Funktionen-App-Instanz variiert je nach Art des verwendeten Triggers sowie je nach den Ressourcen, die von anderen Funktionen innerhalb der Funktionen-App verwendet werden.

## <a name="functions-runtime-versioning"></a>Versionsverwaltung der Functions-Runtime

Sie können die Version der Functions-Runtime mit der App-Einstellung `FUNCTIONS_EXTENSION_VERSION` konfigurieren. Der Wert „~2“ bedeutet beispielsweise, dass für Ihre Funktions-App „2.x“ als deren Hauptversion verwendet wird. Funktionen-Apps werden auf jede neue Nebenversion aktualisiert, wenn sie freigegeben werden. Weitere Informationen finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](set-runtime-version.md), einschließlich der Informationen zum Anzeigen der genauen Version Ihrer Funktions-App.

## <a name="repositories"></a>Repositorys
Der Code für Azure Functions ist Open Source und in GitHub-Repositorys gespeichert:

* [Azure Functions-Laufzeit](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure Functions-Portal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure Functions-Vorlagen](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-Erweiterungen](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindungen
Hier finden Sie eine Tabelle aller unterstützten Bindungen.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Haben Sie Probleme mit Fehlern, die aus den Bindungen stammen? Lesen Sie die Dokumentation [Fehlerbehandlung in Azure Functions](functions-bindings-error-pages.md).

## <a name="reporting-issues"></a>Melden von Problemen
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-reference-csharp.md)
* [F#-Entwicklerreferenz zu Azure Functions](functions-reference-fsharp.md)
* [NodeJS-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Azure Functions:  The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) im Blog des Azure App Service-Teams. Überblick, wie Azure Functions entwickelt wurde.

