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
ms.date: 07/29/2018
ms.author: glenga
ms.openlocfilehash: 6bf6621d650ad590cd1134bc79fcdecdc3fd0963
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622635"
---
# <a name="azure-functions-runtime-versions-overview"></a>Übersicht über die Runtimeversionen von Azure Functions

 Es gibt zwei Hauptversionen der Azure Functions-Runtime: 1.x und 2.x. Nur 1.x ist für den Produktionseinsatz genehmigt. Dieser Artikel erläutert die Neuerungen in 2.x, die sich im Vorschaustadium befindet.

| Laufzeit | Status |
|---------|---------|
|1.x|Allgemein verfügbar (Generally Available, GA)|
|2.x|Vorschauversion<sup>*</sup>|

<sup>*</sup>Wichtige Updates zu Version 2.x, z.B. Ankündigungen zu wichtigen Änderungen, erhalten Sie im Repository mit den [Azure App Service-Ankündigungen](https://github.com/Azure/app-service-announcements/issues).

> [!NOTE] 
> Dieser Artikel bezieht sich auf den Clouddienst Azure Functions. Informationen über das Produkt, das die lokale Ausführung von Azure Functions ermöglicht, finden Sie in der [Übersicht zu Azure Functions-Runtime](functions-runtime-overview.md).

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

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Lokales Codieren und Testen von Azure Functions](functions-run-local.md)
* [Einstellen von Runtimeversionen von Azure Functions als Ziel](set-runtime-version.md)
* [Anmerkungen zu dieser Version](https://github.com/Azure/azure-functions-host/releases)