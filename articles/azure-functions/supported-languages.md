---
title: In Azure Functions unterstützte Sprachen
description: Erfahren Sie, welche Sprachen für allgemeine Verfügbarkeit und welche experimentell und in Vorschauversionen unterstützt werden.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: glenga
ms.openlocfilehash: 00f291e903948bf43bc997816b6072186cf1f889
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343082"
---
# <a name="supported-languages-in-azure-functions"></a>In Azure Functions unterstützte Sprachen

In diesem Artikel werden die Unterstützungsebenen für Sprachen erläutert, die Sie mit Azure Functions verwenden können.

## <a name="levels-of-support"></a>Unterstützungsebenen

Es gibt drei Unterstützungsebenen:

* **Allgemein verfügbar (generally available, GA)**: Vollständige Unterstützung und Freigabe für die Verwendung in Produktionsumgebungen.
* **Vorschau**: Noch nicht unterstützt, die Erreichung der allgemeinen Verfügbarkeit wird aber für die Zukunft erwartet.
* **Experimentell**: Nicht unterstützt und möglicherweise in der Zukunft wieder aufgegeben; die eventuelle Erreichung des Vorschaustatus oder der allgemeinen Verfügbarkeit ist nicht garantiert.

## <a name="languages-in-runtime-1x-and-2x"></a>Sprachen in der Runtime 1.x und 2.x

Es sind [zwei Versionen der Azure Functions-Runtime](functions-versions.md) verfügbar. Die 1.x-Runtime ist allgemein verfügbar. Sie ist als einzige Runtime für Produktionsanwendungen freigegeben. Die 2.x-Runtime befindet sich aktuell in der Vorschau, daher sind auch die von ihr unterstützten Sprachen im Vorschaustatus. Die folgende Tabelle gibt an, welche Sprachen in den beiden Runtimeversionen unterstützt werden.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Experimentelle Sprachen

Die experimentellen Sprachen in 1.x sind nicht gut skalierbar und unterstützen nicht alle Bindungen. Beispielsweise ist Python langsam, da die Functions-Runtime bei jedem Funktionsaufruf *python.exe* ausführt. Und obwohl Python HTTP-Bindungen unterstützt, kann es nicht auf das Anforderungsobjekt zugreifen.

Die experimentelle Unterstützung für PowerShell ist auf Version 4.0 eingeschränkt, weil diese Version auf den virtuellen Computern installiert ist, auf denen Function-Apps ausgeführt werden. Wenn Sie PowerShell-Skripts ausführen möchten, ziehen Sie [Azure Automation](https://azure.microsoft.com/services/automation/) in Erwägung.

Die 2.x-Runtime unterstützt keine experimentellen Sprachen. In 2.x fügen wir Unterstützung für eine Sprache nur hinzu, wenn sie gut skaliert und erweiterte Trigger unterstützt.

Wenn Sie eine der Sprachen verwenden möchten, die nur in 1.x zur Verfügung stehen, bleiben Sie bei der 1.x-Runtime. Verwenden Sie jedoch keine experimentellen Sprachen für irgendetwas, worauf Sie aufbauen, da es für sie keine offizielle Unterstützung gibt. Sie können Hilfe durch [Erstellen von GitHub-Problemen](https://github.com/Azure/azure-webjobs-sdk-script/issues) anfordern, jedoch sollten für Probleme mit experimentellen Sprachen keine Supportfälle geöffnet werden. 

### <a name="language-extensibility"></a>Spracherweiterbarkeit

Die 2.x-Runtime ist für [Spracherweiterbarkeit](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility) ausgelegt. Unter den ersten Sprachen, die auf diesem Erweiterbarkeitsmodell aufbauen, ist Java, das sich in 2.x im Vorschaustatus befindet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden einer der Sprachen in allgemeiner Verfügbarkeit oder im Vorschaustatus in Azure Functions finden Sie in den folgenden Ressourcen:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)