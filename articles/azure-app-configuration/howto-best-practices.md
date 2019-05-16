---
title: Bewährte Methoden für Azure App Configuration | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure App Configuration optimal verwenden.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: d1275a48de5cad9321186ba20860d853b8ce55ad
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413620"
---
# <a name="azure-app-configuration-best-practices"></a>Bewährte Methoden für Azure App Configuration

Dieser Artikel behandelt gängige Muster und Methoden bei der Verwendung von Azure App Configuration.

## <a name="key-groupings"></a>Schlüsselgruppierungen

App Configuration bietet zwei Optionen zum Organisieren von Schlüsseln: Schlüsselpräfixe oder -bezeichnungen. Sie können einen oder beide Typen verwenden.

Schlüsselpräfixe sind die Anfangsteile von Schlüsseln. Sie können einen Satz von Schlüsseln logisch gruppieren, indem Sie dasselbe Präfix in ihren Namen verwenden. Präfixe können mehrere Komponenten enthalten, die durch ein Trennzeichen wie z. B. `/` miteinander verbunden sind, ähnlich einem URL-Pfad, um einen Namespace zu bilden. Solche Hierarchien sind hilfreich, wenn Sie Schlüssel für viele Anwendungen, Komponentendienste und Umgebungen in einem App Configuration-Speicher speichern. Ein wichtiger, zu berücksichtigender Aspekt ist, dass Schlüssel das sind, worauf Ihr Anwendungscode verweist, um die Werte der entsprechenden Einstellungen abzurufen. Ein Schlüssel sollte nicht geändert werden, da Sie sonst den Code jedes Mal ändern müssen, wenn es hierzu kommt.

Bezeichnungen sind Attribute von Schlüsseln. Sie werden zum Erstellen von Varianten eines Schlüssels verwendet. Beispielsweise können Sie mehrere Versionen eines Schlüssels Bezeichnungen zuweisen. Eine Version kann eine Iteration, eine Umgebung oder andere kontextbezogene Informationen sein. Ihre Anwendung kann einen vollständig anderen Satz von Schlüsselwerten anfordern, indem sie einfach eine andere Bezeichnung angibt. Alle Schlüsselverweise können unverändert bleiben.

## <a name="key-value-compositions"></a>Schlüsselwertkompositionen

App Configuration behandelt alle gespeicherten Schlüssel als unabhängige Entitäten. Es versucht nicht, eine Beziehung zwischen Schlüsseln abzuleiten oder Schlüsselwerte basierend auf der Hierarchie zu erben. Sie können aber mehrere Sätze von Schlüsseln aggregieren, indem Sie Bezeichnungen verwenden, die mit geeigneter Konfigurationsstapelung in Ihrem Anwendungscode gekoppelt sind.

Schauen wir uns ein Beispiel an. Sie verfügen über eine Einstellung **Asset1**, deren Wert für die Umgebung „Development“ (Entwicklung) variieren kann. Sie können einen Schlüssel namens „Asset1“ mit einer leeren Bezeichnung und eine Bezeichnung namens „Development“ erstellen. Sie legen den Standardwert für **Asset1** in der ersten und jeden spezifischen Wert für „Development“ in der letzteren ab. In Ihrem Code rufen Sie zuerst die Schlüsselwerte ohne eine Bezeichnung ab und dann die mit einer „Development“-Bezeichnung, um jeden vorherigen Wert derselben Schlüssel zu überschreiben. Wenn Sie ein modernes Programmierframework wie .NET Core verwenden, erhalten Sie diese Stapelfunktion kostenlos, wenn einen nativen Konfigurationsanbieter für den Zugriff auf App Configuration verwenden. Der folgende Codeausschnitt zeigt, wie Sie Stapelung in eine .NET Core-Anwendung implementieren können.

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Use(KeyFilter.Any, LabelFilter.Null)
           .Use(KeyFilter.Any, "Development");
});
```

## <a name="app-configuration-bootstrap"></a>App Configuration-Bootstrapping

Um auf den App Configuration-Speicher zuzugreifen, können Sie seine Verbindungszeichenfolge verwenden, die im Azure-Portal verfügbar ist. Verbindungszeichenfolgen enthalten Anmeldeinformationen und gelten als Geheimnisse. Sie müssen in einem Key Vault gespeichert werden. Eine bessere Option ist die Verwendung einer verwalteten Azure-Identität. Mit dieser Methode benötigen Sie nur die URL des App Configuration-Endpunkts, um den Zugriff auf Ihren Konfigurationsspeicher zu bootstrappen. Sie können die URL in Ihren Anwendungscode einbetten (z. B. in die Datei *appsettings.json*). Weitere Details finden Sie unter [Integrieren mit verwalteten Azure-Identitäten](howto-integrate-azure-managed-service-identity.md).

## <a name="web-app-or-function-access-to-app-configuration"></a>Web-App- oder Funktionszugriff auf App Configuration

Sie können die Verbindungszeichenfolge zu Ihrem App Configuration-Speicher über das Azure-Portal in den Anwendungseinstellungen des App Service eingeben. Sie können sie außerdem in Key Vault speichern und [aus App Service darauf verweisen](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references). Sie können auch eine verwaltete Azure-Identität für den Zugriff auf den Konfigurationsspeicher verwenden. Weitere Details finden Sie unter [Integrieren mit verwalteten Azure-Identitäten](howto-integrate-azure-managed-service-identity.md).

Alternativ können Sie die Konfiguration aus App Configuration an App Service per Push übertragen. App Configuration bietet eine Exportfunktion (im Azure-Portal und in der CLI), die Daten direkt an App Service sendet. Mit dieser Methode müssen Sie den Anwendungscode gar nicht ändern.

## <a name="next-steps"></a>Nächste Schritte

* [Schlüssel und Werte](./concept-key-value.md)
