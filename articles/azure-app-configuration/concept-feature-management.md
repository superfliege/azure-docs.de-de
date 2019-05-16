---
title: Featureverwaltung in Azure App Configuration | Microsoft-Dokumentation
description: Eine Übersicht darüber, wie Azure App Configuration zum bedarfsabhängigen Aktivieren und Deaktivieren von Anwendungsfeatures verwendet werden kann
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 304ca08275ccf4bf32d38e3fc89dd5cf07460fa4
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413766"
---
# <a name="feature-management"></a>Funktionsverwaltung

Traditionell erfordert die Auslieferung eines neuen Anwendungsfeatures eine vollständige erneute Bereitstellung der Anwendung selbst. Zum Testen eines Features müssen Sie Ihre Anwendung wahrscheinlich mehrmals bereitstellen, um zu steuern, wann oder wem das Feature angezeigt wird. Die Featureverwaltung ist ein moderner Softwareentwicklungsansatz, der die Freigabe von Features von der Codebereitstellung entkoppelt und schnelle bedarfsabhängige Änderungen an der Featureverfügbarkeit ermöglicht. Dabei wird eine Technik namens *Featureflag* (auch als *Feature Toggle*, *Featureschalter* usw. bezeichnet) verwendet, um den Lebenszyklus eines Features dynamisch zu verwalten. Diese hilft Entwicklern bei der Lösung der folgenden Probleme:

* **Verwaltung von Codebranches**: Featureflags werden häufig verwendet, um neue Anwendungsfunktionen, die sich in der Entwicklung befinden, zu umschließen. Solche Funktionalität ist standardmäßig ausgeblendet. Sie können sie sicher ausliefern, obwohl sie unvollendet ist, das sie in der Produktion inaktiv bleibt. Mit diesem Ansatz, der als *dunkle Bereitstellung* bezeichnet wird, können Sie Ihren gesamten Code am Ende jedes Entwicklungszyklus freigeben. Sie müssen keinen Codebranch mehr über mehrere Zyklen hinweg pflegen, wenn die Fertigstellung eines Feature mehrere Zeiträume in Anspruch nimmt.
* **Testen in der Produktion**: Sie können Featureflags verwenden, um Vorabzugriff auf neue Features in der Produktion zu ermöglichen. Sie können diesen Zugriff beispielsweise auf Ihre Teammitglieder oder interne Betatester beschränken. Diese Benutzer erhalten die originalgetreue Produktionsoberfläche anstelle einer Simulations- oder Teiloberfläche in einer Testumgebung.
* **Erstellen von Flights**: Sie können Featureflags auch verwenden, um neue Features schrittweise für Endbenutzer einzuführen. Sie können zunächst einen kleinen Prozentsatz Ihrer Benutzer als Zielgruppe verwenden und diesen Prozentsatz im Laufe der Zeit schrittweise erhöhen, nachdem Sie mehr Vertrauen in die Implementierung gewonnen haben.
* **Schalter zum sofortigen Beenden**: Featureflags bieten ein inhärentes Sicherheitsnetz für die Freigabe neuer Features. Damit können Sie die Anwendungsfeatures nicht nur problemlos aktivieren, sondern auch deaktivieren. Sie erhalten die Möglichkeit, ein Feature bei Bedarf schnell zu deaktivieren, ohne dass die Anwendung neu erstellt und bereitgestellt werden muss.
* **Selektive Aktivierung**: Die meisten Featureflags existieren zwar nur, bis ihre zugehörigen Funktionalitäten erfolgreich freigegeben wurden, einige können jedoch lange vorhanden sein. Sie können sie als Methode zum Segmentieren Ihrer Benutzer verwenden und jeder Gruppe einen bestimmten Satz von Features zur Verfügung stellen. Möglicherweise haben Sie ein Feature, das mit einem bestimmten Webbrowser funktioniert. Dann können Sie ein Featureflag so definieren, dass es nur von Benutzer des entsprechenden Browsers gesehen und genutzt werden kann. Der Vorteil dieses Ansatzes ist, dass Sie die Liste der unterstützten Browser später problemlos erweitern können, ohne den Code zu ändern.

## <a name="basic-concepts"></a>Grundlegende Konzepte

Hier sind einige neue Begriffe im Zusammenhang mit der Featureverwaltung.

* **Featureflag**: Ein Featureflag ist eine Variable mit dem Binärzustand *ein* oder *aus* und hat einen zugehörigen Codeblock. Der Zustand bestimmt, ob der Codeblock ausgeführt wird oder nicht.
* **Feature-Manager**: Ein Feature-Manager ist ein Anwendungspaket, das den Lebenszyklus aller Featureflags in einer Anwendung verwaltet. Es bietet in der Regel zusätzliche Funktionen wie das Zwischenspeichern von Flags und die Aktualisierung ihrer Zustände.
* **Filter**: Ein Filter ist eine Regel für das Auswerten des Featureflag-Zustands. Benutzergruppe, Gerät- oder Browsertyp, geografischer Standort und Zeitfenster sind Beispiele dafür, was ein Filter darstellen kann.

Eine effektive Implementierung der Featureverwaltung umfasst mindestens zwei Komponenten, die zusammenwirken: eine Anwendung, die Featureflags verwendet, und ein separates Repository, das Featureflags und deren aktuelle Zustände speichert. Ihre Interaktionen werden nachfolgend veranschaulicht.

## <a name="feature-flag-usage-in-code"></a>Verwendung von Featureflags in Code

Das Grundmuster für die Implementierung von Featureflags in einer Anwendung ist einfach. Sie können es als boolesche Zustandsvariable konzipieren, die mit einer bedingten `if`-Anweisung in Ihrem Code verwendet wird.

```csharp
if (featureFlag) {
    // Run the following code
}
```

Wenn in diesem Fall `featureFlag` auf `True` gesetzt ist, wird der eingeschlossene Codeblock ausgeführt, ansonsten wird er übersprungen. Der Wert von `featureFlag` kann statisch festgelegt werden:

```csharp
bool featureFlag = true;
```

Er kann auch basierend auf bestimmten Regeln ausgewertet werden:

```csharp
bool featureFlag = isBetaUser();
```

Ein etwas komplizierteres Featureflag-Muster beinhaltet auch eine `else`-Anweisung.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
} else {
    // This following code will run if the featureFlag value is false
}
```

Diese kann jedoch im Grundmuster umgeschrieben werden. Unter [Verwenden von Featureflags in einer ASP.NET Core-App](./use-feature-flags-dotnet-core.md) wird der Vorteil der Einführung eines einfachen Codemusters als Standard aufgezeigt.

```csharp
if (featureFlag) {
    // This following code will run if the featureFlag value is true
}

if (!featureFlag) {
    // This following code will run if the featureFlag value is false
}
```

## <a name="feature-flag-repository"></a>Featureflag-Repository

Sie müssen alle Featureflags in einer Anwendung auslagern, um sie effektiv nutzen zu können. So können Sie den Zustand der Featureflags ändern, ohne die Anwendung an sich zu ändern und neu bereitzustellen. Azure App Configuration ist als zentrales Repository für Featureflags konzipiert. Sie können damit verschiedene Arten von Featureflags definieren und deren Zustände schnell und sicher bearbeiten. Sie können dann in Ihrer Anwendung problemlos auf diese Featureflags zugreifen, indem Sie die App Configuration-Bibliotheken für verschiedene Frameworks von Programmiersprachen verwenden. In [Verwenden von Featureflags in einer ASP.NET Core-App](./use-feature-flags-dotnet-core.md) wird veranschaulicht, wie der .NET Core App Configuration-Anbieter und die Bibliotheken der Featureverwaltung zusammen als Komplettlösung zur Implementierung von Featureflags für Ihre ASP.NET-Webanwendung verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Featureflags zu einer ASP.NET Core-Web-App](./quickstart-feature-flag-aspnet-core.md)  
