---
title: Übersicht über die Runtimeversionen von Azure Functions
description: Azure Functions unterstützt mehrere Versionen der Runtime. Lernen Sie die Unterschiede kennen, und erfahren Sie, wie Sie die Version auswählen, die sich am besten für Ihre Anforderungen eignet.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 9f916aaa8032ff519709d73a1c1f51195f811686
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
ms.locfileid: "28919354"
---
# <a name="azure-functions-runtime-versions-overview"></a>Übersicht über die Runtimeversionen von Azure Functions

 Es gibt zwei Hauptversionen der Azure Functions-Runtime: 1.x und 2.x. Dieser Artikel erläutert die Kriterien, nach denen Sie die zu verwendende Hauptversion aussuchen sollten.

> [!IMPORTANT] 
> Runtime 1.x ist die einzige Version, die für die Verwendung in der Produktion genehmigt ist.

| Laufzeit | Status |
|---------|---------|
|1.x|Allgemein verfügbar (Generally Available, GA)|
|2.x|Vorschau|

Informationen zum Konfigurieren einer Funktions-App oder Ihrer Entwicklungsumgebung für eine bestimmte Version finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](set-runtime-version.md) und [Lokales Codieren und Testen von Azure Functions](functions-run-local.md).

## <a name="cross-platform-development"></a>Plattformübergreifende Entwicklung

Runtime 1.x unterstützt die Entwicklung und das Hosting nur im Portal oder unter Windows. Runtime 2.x wird auf .NET Core ausgeführt, kann also auf allen Plattformen ausgeführt werden, die von .NET Core unterstützt werden, einschließlich macOS und Linux. Damit können plattformübergreifende Entwicklungs- und Hostingszenarien umgesetzt werden, die mit 1.x nicht möglich sind.

## <a name="languages"></a>Sprachen

Runtime 2.x verwendet ein neues Modell für die Erweiterbarkeit von Sprachen. JavaScript und Java profitieren von Beginn an von diesem neuen Modell. Experimentelle Sprachen von Azure Functions 1.x wurden nicht für die Verwendung des neuen Modells aktualisiert und werden daher in 2.x nicht unterstützt. Die folgende Tabelle zeigt, welche Programmiersprachen in den einzelnen Runtimeversionen unterstützt werden.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Weitere Informationen finden Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="bindings"></a>Bindungen 

Runtime 2.x verwendet ein neues [Modell für die Erweiterbarkeit von Bindungen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview), das folgende Vorteile bietet:

* Unterstützung für Bindungserweiterungen von Drittanbietern.
* Entkoppeln von Runtime und Bindungen. Damit können Bindungserweiterungen versioniert und unabhängig freigegeben werden. Sie können z.B. ein Upgrade auf eine Version einer Erweiterung durchführen, das auf einer neueren Version des zugrunde liegenden SDKs basiert.
* Eine schlankere Ausführungsumgebung, in der nur die tatsächlich verwendeten Bindungen bekannt sind und von der Runtime geladen werden.

Dieses Modell wurde für alle integrierten Azure Functions-Bindungen übernommen, sodass diese mit Ausnahme des Timer- und des HTTP-Triggers nicht mehr standardmäßig inbegriffen sind. Diese Erweiterungen werden automatisch installiert, wenn Sie Funktionen mithilfe von Tools wie Visual Studio oder über das Portal erstellen.

Die folgende Tabelle zeigt, welche Bindungen in den einzelnen Runtimeversionen unterstützt werden.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>Bekannte Probleme in 2.x

Informationen zur Bindungsunterstützung und zu anderen funktionalen Lücken in 2.x finden Sie unter [Bekannte Probleme von Runtime 2.0](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verwenden der 2.0-Runtime als Ziel in Ihrer lokalen Entwicklungsumgebung](functions-run-local.md)

> [!div class="nextstepaction"]
> [Siehe Anmerkungen zu dieser Version für Runtimeversionen](https://github.com/Azure/azure-webjobs-sdk-script/releases)