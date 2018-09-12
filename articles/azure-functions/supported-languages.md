---
title: In Azure Functions unterstützte Sprachen
description: Erfahren Sie, welche Sprachen für allgemeine Verfügbarkeit und welche experimentell und in Vorschauversionen unterstützt werden.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/02/2018
ms.author: glenga
ms.openlocfilehash: b735f93b2d7ad093ef752fd5f26be729a1157b37
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44090677"
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

Die experimentellen Sprachen in Version 1.x sind nicht gut skalierbar und unterstützen nicht alle Bindungen. Beispielsweise ist Python langsam, da die Functions-Runtime bei jedem Funktionsaufruf *python.exe* ausführt. Und obwohl Python HTTP-Bindungen unterstützt, kann es nicht auf das Anforderungsobjekt zugreifen.

Die experimentelle Unterstützung für PowerShell ist auf Version 5.1 eingeschränkt, weil diese Version standardmäßig auf den virtuellen Computern installiert ist, auf denen Funktions-Apps ausgeführt werden. Wenn Sie PowerShell-Skripts ausführen möchten, ziehen Sie [Azure Automation](https://azure.microsoft.com/services/automation/) in Erwägung.

Wenn Sie eine der Sprachen verwenden möchten, die nur in 1.x zur Verfügung stehen, bleiben Sie bei der 1.x-Runtime. Verwenden Sie jedoch keine experimentellen Sprachen für irgendetwas, worauf Sie aufbauen, da es für sie keine offizielle Unterstützung gibt. Sie können Hilfe durch [Erstellen von GitHub-Problemen](https://github.com/Azure/azure-webjobs-sdk-script/issues) anfordern, jedoch sollten für Probleme mit experimentellen Sprachen keine Supportfälle geöffnet werden. 

Für Version 2.x der Runtime werden keine experimentellen Sprachen unterstützt. Die Unterstützung für neue Sprachen wird nur hinzugefügt, wenn die Sprache in der Produktion unterstützt werden kann. 

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
