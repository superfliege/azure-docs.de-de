---
title: Schlüssel-Wert-Speicher von Azure App Configuration | Microsoft-Dokumentation
description: Enthält eine Übersicht über die Speicherung von Konfigurationsdaten in Azure App Configuration.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: f04067358b0b2bae745727a5dd7a1f5554f9f70e
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884980"
---
# <a name="key-value-store"></a>Schlüssel-Wert-Speicher

Azure App Configuration speichert Konfigurationsdaten als Schlüssel-Wert-Paare. Dies ist eine einfache und flexible Methode zur Darstellung verschiedener Arten von Anwendungseinstellungen, mit denen Entwickler vertraut sind.

## <a name="keys"></a>Schlüssel

Schlüssel dienen als Name für Schlüssel-Wert-Paare und werden zum Speichern und Abrufen von entsprechenden Werten verwendet. Es ist eine häufige Vorgehensweise, Schlüssel mit einem Trennzeichen (z. B. `/` oder `:`) in einem hierarchischen Namespace zu organisieren – basierend auf einer Konvention, die für Ihre Anwendung am besten geeignet ist. Bei App Configuration werden Schlüssel als Ganzes behandelt. Schlüssel werden nicht analysiert, um zu ermitteln, wie deren Namen strukturiert sind, und es werden auch keine Regeln dafür erzwungen.

Bei der Verwendung des Konfigurationsspeichers in Anwendungsframeworks werden unter Umständen bestimmte Benennungsschemas für Schlüssel-Wert-Paare vorgegeben. Für das Spring Cloud-Framework von Java werden beispielsweise `Environment`-Ressourcen definiert, die Einstellungen für eine Spring-Anwendung für die Parametrisierung durch Variablen, z. B. *application name* und *profile* bereitstellen. Schlüssel für Spring Cloud-bezogene Konfigurationsdaten beginnen normalerweise mit diesen beiden Elementen, die durch ein Trennzeichen getrennt sind.

Für in App Configuration gespeicherte Schlüssel wird die Groß-/Kleinschreibung beachtet, und es handelt sich um Unicode-basierte Zeichenfolgen. *app1* und *App1* sind in einem App-Konfigurationsspeicher zwei einzelne Schlüssel. Beachten Sie dies, wenn Sie in einer Anwendung Konfigurationseinstellungen verwenden, da für Konfigurationsschlüssel in einigen Frameworks die Groß-/Kleinschreibung nicht beachtet wird. Vom ASP.NET Core-Konfigurationssystem werden Schlüssel beispielsweise als Zeichenfolgen ohne Berücksichtigung der Groß-/Kleinschreibung behandelt. Um beim Abfragen von App Configuration in einer ASP.NET Core-Anwendung unvorhersehbares Verhalten zu vermeiden, sollten keine Schlüssel verwendet werden, die sich nur anhand der Groß-/Kleinschreibung unterscheiden.

Es ist zulässig, in Schlüsselnamen, die in App Configuration eingegeben werden, beliebige Unicode-Zeichen zu verwenden. Eine Ausnahme stellen nur die reservierten Zeichen `*`, `,` und `\` dar. Wenn Sie ein reserviertes Zeichen einfügen möchten, müssen Sie es wie folgt mit Escapezeichen versehen: `\{Reserved Character}`. Für ein Schlüssel-Wert-Paar gilt ein kombiniertes Größenlimit von 10K Zeichen. Dies umfasst alle Zeichen des Schlüssels, seinen Wert sowie alle zugeordneten optionalen Attribute. Innerhalb dieses Limits können Sie für Schlüssel viele hierarchische Ebenen verwenden.

### <a name="designing-key-namespaces"></a>Entwerfen von Schlüsselnamespaces

Es gibt zwei allgemeine Vorgehensweisen beim Benennen von Schlüsseln, die für Konfigurationsdaten verwendet werden: flach und hierarchisch. Aus Sicht der Anwendungsnutzung ähneln sich diese Vorgehensweisen sehr, aber der hierarchische Ansatz hat eine Reihe von Vorteilen:

* Einfachere Lesbarkeit. Um lange Folgen von Zeichen zu vermeiden, fungieren Trennzeichen in einem hierarchischen Schlüsselnamen als Leerstellen in einem Satz und als natürliche Unterbrechungen zwischen Wörtern.
* Einfachere Verwaltung. Eine Schlüsselnamenhierarchie umfasst logische Gruppen mit Konfigurationsdaten.
* Einfachere Nutzung. Es ist einfacher, eine Abfrage zu schreiben, bei der sich für Schlüssel in einer hierarchischen Struktur Musterübereinstimmungen ergeben und nur ein Teil der Konfigurationsdaten abgerufen wird. Darüber hinaus verfügen viele neuere Programmierframeworks über native Unterstützung für hierarchische Konfigurationsdaten, damit Ihre Anwendung spezifische Konfigurationssätze nutzen kann.

Sie können Schlüssel in App Configuration auf viele verschiedene Arten hierarchisch organisieren. Sie können sich diese Schlüssel als [URIs](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) vorstellen. Jeder hierarchische Schlüssel ist ein *Ressourcenpfad* mit einer oder mehreren Komponenten, die durch Trennzeichen verbunden sind. Sie können wählen, welches Zeichen Sie basierend auf den Anforderungen Ihrer Anwendung, Ihrer Programmiersprache oder Ihrem Framework verwenden möchten. Sie können in App Configuration für unterschiedliche Schlüssel verschiedene Trennzeichen verwenden.

Hier sind einige Beispiele dafür angegeben, wie Sie Ihre Schlüsselnamen in einer Hierarchie strukturieren können:

* Basierend auf Umgebungen

        AppName:Test:DB:Endpoint
        AppName:Staging:DB:Endpoint
        AppName:Production:DB:Endpoint

* Basierend auf Komponentendiensten

        AppName:Service1:Test:DB:Endpoint
        AppName:Service1:Staging:DB:Endpoint
        AppName:Service1:Production:DB:Endpoint
        AppName:Service2:Test:DB:Endpoint
        AppName:Service2:Staging:DB:Endpoint
        AppName:Service2:Production:DB:Endpoint

* Basierend auf Bereitstellungsregionen

        AppName:Production:Region1:DB:Endpoint
        AppName:Production:Region2:DB:Endpoint

### <a name="versioning-key-values"></a>Versionsverwaltung für Schlüsselwerte

Schlüsselwerte in App Configuration können optional über das Attribut **label** (Bezeichnung) verfügen. Bezeichnungen werden genutzt, um zwischen Schlüsselwerten mit demselben Schlüssel zu unterscheiden. Ein Schlüssel *app1* mit den Bezeichnungen *v1* und *v2* steht in einem App-Konfigurationsspeicher für zwei separate Schlüsselwerte. Standardmäßig ist die Bezeichnung für einen Schlüsselwert leer (bzw. `null`).

App Configuration versieht Schlüsselwerte nicht automatisch mit Versionsangaben, wenn sie geändert werden. Sie können Bezeichnungen als Möglichkeit zum Erstellen von mehreren Versionen eines Schlüsselwerts verwenden. Beispielsweise können Sie eine Zahl einer Anwendungsversion oder eine Git-Commit-ID in Bezeichnungen eingeben, um Schlüsselwerte zu identifizieren, die einem bestimmten Softwarebuild zugeordnet sind.

Sie können in Bezeichnungen beliebige Unicodezeichen verwenden, mit Ausnahme der reservierten Zeichen `*`, `,` und `\`. Wenn Sie ein reserviertes Zeichen einfügen möchten, müssen Sie es wie folgt mit Escapezeichen versehen: `\{Reserved Character}`.

### <a name="querying-key-values"></a>Abfragen von Schlüsselwerten

Jeder Schlüsselwert wird anhand seines Schlüssels und einer Bezeichnung, die auch `null` lauten kann, eindeutig identifiziert. Sie fragen einen App-Konfigurationsspeicher für Schlüsselwerte ab, indem Sie ein Muster angeben. Der App-Konfigurationsspeicher gibt alle Schlüsselwerte zurück, die mit dem Muster und den entsprechenden Werten und Attributen übereinstimmen. Sie können die folgenden Schlüsselmuster in REST-API-Aufrufen für App Configuration verwenden:

| Schlüssel | |
|---|---|
| `key` wird weggelassen oder lautet `key=*` | Übereinstimmung mit allen Schlüsseln |
| `key=abc` | Exakte Übereinstimmung mit dem Schlüsselnamen **abc** |
| `key=abc*` | Übereinstimmung mit Schlüsselnamen, die mit **abc** beginnen |
| `key=*abc` | Übereinstimmung mit Schlüsselnamen, die auf **abc** enden |
| `key=*abc*` | Übereinstimmung mit Schlüsselnamen, die **abc** enthalten |
| `key=abc,xyz` | Übereinstimmung mit Schlüsselnamen mit **abc** oder **xyz**, auf fünf CSVs beschränkt |

Sie können auch die folgenden Bezeichnungsmuster verwenden:

| Bezeichnung | |
|---|---|
| `label` wird weggelassen oder lautet `label=*` | Übereinstimmung mit einer beliebigen Bezeichnung, einschließlich `null` |
| `label=%00` | Übereinstimmung mit der Bezeichnung `null` |
| `label=1.0.0` | Exakte Übereinstimmung mit der Bezeichnung **1.0.0** |
| `label=1.0.*` | Übereinstimmung mit Bezeichnungen, die mit **1.0.** beginnen |
| `label=*.0.0` | Übereinstimmung mit Bezeichnungen, die auf **.0.0** enden |
| `label=*.0.*` | Übereinstimmung mit Bezeichnungen, die **.0.** enthalten |
| `label=%00,1.0.0` | Übereinstimmung mit den Bezeichnungen `null` oder **1.0.1**, auf fünf CSVs beschränkt |

## <a name="values"></a>Werte

Bei Werten, die Schlüsseln zugewiesen sind, handelt es sich ebenfalls um Unicode-Zeichenfolgen. Sie können alle Unicode-Zeichen für Werte verwenden. Jedem Wert ist ein optionaler benutzerdefinierter **Inhaltstyp** zugeordnet. Sie können dieses Attribut verwenden, um Informationen (z. B. ein Codierungsschema) zu einem Wert zu speichern, die Ihrer Anwendung die richtige Verarbeitung ermöglichen.

In einem App-Konfigurationsspeicher gespeicherte Konfigurationsdaten, einschließlich aller Schlüssel und Werte, werden im ruhenden Zustand und während der Übertragung verschlüsselt. Trotzdem ist App Configuration keine Ersatzlösung für Azure Key Vault. Sie sollten darin keine Anwendungsgeheimnisse speichern.

## <a name="next-steps"></a>Nächste Schritte

* [Konzept: Point-in-Time-Momentaufnahme](concept-point-time-snapshot.md)  
