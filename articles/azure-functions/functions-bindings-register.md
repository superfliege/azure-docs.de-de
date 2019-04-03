---
title: Registrieren von Bindungserweiterungen von Azure Functions
description: Lernen Sie, wie Sie eine Azure Functions-Bindungserweiterung abhängig von Ihrer Umgebung registrieren.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 5534086d5754691f650370e465fa2c63210e0dc7
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56740274"
---
# <a name="register-azure-functions-binding-extensions"></a>Registrieren von Bindungserweiterungen von Azure Functions

Azure Functions unterstützt HTTP und Timer ohne weitere Konfiguration. Um mit anderen Diensten zu arbeiten, müssen Sie eine [Bindungserweiterung](./functions-triggers-bindings.md) installieren oder *registrieren*. Bindungserweiterungen werden über Azure Core Tools oder in Form von NuGet-Paketen bereitgestellt. 

In der folgenden Tabelle ist angegeben, wann und wie Sie Bindungen registrieren.

| Entwicklungsumgebung |Registrierung<br/> in Functions 1.x  |Registrierung<br/> in Functions 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Azure-Portal|Automatisch|[Automatisch mit Eingabeaufforderung](#azure-portal-development)|
|Nicht zu .NET gehörende Sprachen oder lokale Azure Core Tools-Entwicklung|Automatisch|[Verwendung von Core Tools-CLI-Befehlen](#local-development-azure-functions-core-tools)|
|C#-Klassenbibliothek mit Visual Studio 2017|[Verwendung von NuGet-Tools](#c-class-library-with-visual-studio-2017)|[Verwendung von NuGet-Tools](#c-class-library-with-visual-studio-2017)|
|C#-Klassenbibliothek mit Visual Studio Code|–|[Verwendung der .NET Core-CLI](#c-class-library-with-visual-studio-code)|

Die folgenden Bindungstypen sind Ausnahmen, für die keine explizite Registrierung erforderlich ist, da sie in allen Versionen und Umgebungen automatisch registriert werden: HTTP und Timer.

> [!IMPORTANT]
> Diese Inhalte gelten für den Rest dieses Artikels nur für Functions 2.x. Bindungserweiterungen werden in Functions 1.x nicht explizit registriert, ausgenommen beim [Erstellen einer C#-Klassenbibliothek mithilfe von Visual Studio 2017](#local-csharp).

## <a name="azure-portal-development"></a>Entwicklung im Azure-Portal

Wenn Sie eine Funktion erstellen oder eine Bindung hinzufügen, erhalten Sie eine Meldung, wenn die Erweiterung für den Trigger bzw. die Bindung eine Registrierung erfordert. Reagieren Sie auf die Meldung, indem Sie auf **Installieren** klicken, um die Erweiterung zu registrieren. Die Installation kann bis zu 10 Minuten eines Verbrauchstarifs aufbrauchen.

Sie müssen jede Erweiterung nur ein Mal für eine bestimmte Funktions-App installieren. Für unterstützte Bindungen, die im Portal nicht verfügbar sind, oder zum Aktualisieren einer installierten Erweiterung können Sie ferner [Azure Functions-Bindungserweiterungen aus dem Portal manuell installieren](install-update-binding-extensions-manual.md).  

## <a name="local-development-azure-functions-core-tools"></a>Lokale Entwicklung mit Azure Functions Core Tools

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
## <a name="c-class-library-with-visual-studio-2017"></a>C#-Klassenbibliothek mit Visual Studio 2017

In **Visual Studio 2017** können Sie Pakete mithilfe des Befehls [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aus der Paket-Manager-Konsole installieren, wie es im folgenden Beispiel gezeigt wird:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <target_version>
```

Der Name des Pakets, das für eine bestimmte Bindung verwendet werden muss, ist im Referenzartikel für diese Bindung angegeben. Ein Beispiel finden Sie im [Referenzartikel zur Service Bus-Bindung im Abschnitt „Pakete“](functions-bindings-service-bus.md#packages---functions-1x).

Ersetzen Sie `<target_version>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

## <a name="c-class-library-with-visual-studio-code"></a>C#-Klassenbibliothek mit Visual Studio Code

In **Visual Studio Code** können Sie Pakete über die Eingabeaufforderung installieren. Verwenden Sie dazu den Befehl [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) in der .NET Core-CLI, wie es im folgenden Beispiel gezeigt wird:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <target_version>
```

Die .NET Core-CLI kann nur für die Entwicklung in Azure Functions 2.x verwendet werden.

Der Name des Pakets, das für eine bestimmte Bindung verwendet werden muss, ist im Referenzartikel für diese Bindung angegeben. Ein Beispiel finden Sie im [Referenzartikel zur Service Bus-Bindung im Abschnitt „Pakete“](functions-bindings-service-bus.md#packages---functions-1x).

Ersetzen Sie `<target_version>` im Beispiel durch eine bestimmte Version des Pakets, z.B. `3.0.0-beta5`. Gültige Versionen sind auf den Seiten der einzelnen Pakete auf [NuGet.org](https://nuget.org) aufgeführt. Die Hauptversionen, die Functions Runtime 1.x oder 2.x entsprechen, sind im Referenzartikel für die Bindung angegeben.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Trigger- und Bindungsbeispiel für Azure Functions](./functions-bindings-example.md)

