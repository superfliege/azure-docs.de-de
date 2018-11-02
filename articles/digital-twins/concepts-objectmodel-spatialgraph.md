---
title: Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen | Microsoft-Dokumentation
description: Verwenden von Azure Digital Twins, um Beziehungen zwischen Personen, Orten und Geräten zu modellieren
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 1c2068af510cb3733ce99a6ae7b40487a8c1a015
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323804"
---
# <a name="understanding-digital-twins-object-models-and-spatial-intelligence-graph"></a>Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen

Azure Digital Twins ist ein Azure IoT-Dienst, der umfassende virtuelle Darstellungen von physischen Umgebungen und zugehörigen Geräten, Sensoren und Personen unterstützt. Er verbessert die Entwicklung, indem domänenspezifische Konzepte in nützlichen Modellen strukturiert werden, die wiederum in einem Raumintelligenzgraphen angeordnet sind. In solchen Konzepten werden die Beziehungen und Interaktionen zwischen Menschen, Räumen und Geräten naturgetreu modelliert.

In _Digital Twins-Objektmodellen_ werden domänenspezifische Konzepte, Kategorien und Eigenschaften beschrieben. Modelle werden von Benutzern vordefiniert, die die Lösung an ihre speziellen Anforderungen anpassen möchten. Zusammen bilden diese vordefinierten Digital Twins-Objektmodelle eine _Ontologie_. In der Ontologie eines intelligenten Gebäudes wären Regionen, Orte, Etagen, Büros, Zonen, Konferenzräume und Konzentrationsräume beschrieben. In der Ontologie eines Energienetzes würden verschiedene Kraftwerke, Umspannwerke, Energieressourcen und Kunden beschrieben. Diese Azure Digital Twins-Objektmodelle und Ontologien ermöglichen es, Azure Digital Twins an unterschiedliche Szenarien und Anforderungen anzupassen.

Mit formulierten _Azure Digital Twins-Objektmodellen_ und formulierter _Ontologie_ kann ein _Raumgraph_ bestückt werden. Raumgraphen sind virtuelle Darstellungen der vielen Beziehungen zwischen Räumen, Geräten und Personen, die für eine IoT-Lösung relevant sind. In der folgenden Abbildung ist ein Beispiel eines Raumgraphen gezeigt, für den die Ontologie eines intelligenten Gebäude verwendet wurde.

![Digital Twins-Raumgraph für ein Gebäude][1]

<a id="model" />

In dem Raumgraph werden Räume, Geräte, Sensoren und Benutzer zusammengeführt. Jedes Objekt ist auf eine Weise verknüpft, die die reale Welt modelliert: Veranstaltungsort 43 hat vier Etagen, von denen jede mehrere verschiedene Bereiche hat. Benutzer sind ihren Arbeitsplätzen zugeordnet und erhalten Zugriff auf Teile des Graphen.  Beispielsweise hätte ein Administrator Berechtigungen, Änderungen am Raumgraphen vornehmen zu können, während ein Besucher möglicherweise nur Berechtigungen zum Anzeigen bestimmter Gebäudedaten hätte.

## <a name="digital-twins-object-models"></a>Digitale Twins-Objektmodelle

Für Digital Twins-Objektmodelle werden diese Objekthauptkategorien unterstützt:

- **Räume** sind virtuelle oder physische Orte, z. B. `Tenant`, `Customer`, `Region`, `Venue`.
- **Geräte** sind virtuelle oder physische Geräte, z. B. `AwesomeCompany Device`, `Raspberry Pi 3`.
- **Sensoren** sind Objekte, die Ereignisse erkennen, z. B. `AwesomeCompany Temperature Sensor`, `AwesomeCompany Presence Sensor`.
- **Benutzer** kennzeichnen Personen und deren Merkmale.

Weitere Objektkategorien sind:

- **Ressourcen** sind mit einem Raum verknüpft und entsprechen üblicherweise Azure-Ressourcen, die von Objekten im Raumgraphen verwendet werden, z. B. `IoTHub`.
- **Blobs** sind mit Objekten (z. B. Räume, Geräte, Sensoren und Benutzer) verknüpft und werden als Dateien mit MIME-Typ und Metadaten verwendet, z. B. `maps`, `pictures`, `manuals`.
- **Erweiterte Typen** sind erweiterbare Enumerationen, mit denen Entitäten bestimmte Merkmale hinzugefügt werden, z. B. `SpaceType`, `SpaceSubtype`.
- **Ontologien** entsprechen einem Satz erweiterter Typen, z. B. `Default`, `Building`, `BACnet`, `EnergyGrid`.
- **Eigenschaftenschlüssel und -werte** sind benutzerdefinierte Merkmale von Räumen, Geräten, Sensoren und Benutzern. Sie können zusätzlich zu integrierten Merkmalen verwendet werden, z. B. `DeltaProcessingRefreshTime` als Schlüssel und `10` als Wert.
- **Rollen** sind Sätze von Berechtigungen, die Benutzern und Geräten im Raumgraphen zugeordnet sind, z. B. `Space Administrator`, `User Administrator`, `Device Administrator`.
- **Rollenzuweisungen** sind die Zuordnung zwischen einer Rolle und einem Objekt im Raumgraphen, z. B. einem Benutzer oder Dienstprinzipal die Berechtigung gewähren, einen Raum im Raumgraphen verwalten zu können.
- **Sicherheitsschlüsselspeicher** stellen die Sicherheitsschlüssel für alle Geräte in der Hierarchie unter einem bestimmten Raumobjekt bereit, um es dem Gerät zu ermöglichen, sicher mit dem Digital Twins-Dienst zu kommunizieren.
- **Benutzerdefinierte Funktionen** oder **UDFs** (User-Defined-Functions) ermöglichen anpassbare Verarbeitung von Sensortelemetriedaten im Raumgraphen. Beispielsweise kann eine benutzerdefinierte Funktion einen Sensorwert festlegen, benutzerdefinierte Logik anhand von Sensormesswerten ausführen, die Ausgabe für einen Raum festlegen, Metadaten an einen Raum anfügen und Benachrichtigungen senden, wenn vordefinierte Bedingungen erfüllt sind. Derzeit können benutzerdefinierte Funktionen in JavaScript geschrieben werden.
- **Matcher** (Abgleicher) sind Objekte, die bestimmen, welche benutzerdefinierten Funktionen für eine bestimmte Telemetrienachricht ausgeführt werden sollen.
- **Endpunkte** sind die Punkte, an denen Telemetrienachrichten und Digital Twins-Ereignisse weitergeleitet werden können, z. B. `Event Hub`, `Service Bus`, `Event Grid`.

<a id="graph" />

## <a name="spatial-intelligence-graph"></a>Raumintelligenzgraph

**Raumgraph** ist der hierarchische Graph aus Räumen, Geräten und Personen, die im **Digital Twins-Objektmodell** definiert sind. Für einen Raumgraph werden _Vererbung_, _Filterung_, _Durchlaufen_, _Skalierbarkeit_ und _Erweiterbarkeit_  unterstützt. Benutzer können Raumgraphen über eine Sammlung von REST-APIs (siehe unten) verwalten und nutzen.

Ein Benutzer, der einen Digital Twins-Dienst in seinem Abonnement bereitstellt, wird zum globalen Administrator des Stammknotens und gewährt automatisch vollen Zugriff auf die gesamte Struktur. Dieser Benutzer kann dann über die `Space`-API Räume im Graphen bereitstellen. Geräte können über die `Device`-API, Sensoren können über die `Sensor`-API bereitgestellt werden, usw. Wir bieten auch [Open Source-Tools](https://github.com/Azure-Samples/digital-twins-samples-csharp), um den Graphen in großem Umfang bereitzustellen.

Graph-_Vererbung_ bezieht sich auf die Berechtigungen und Eigenschaften, die von einem übergeordneten Knoten an alle unter ihm befindlichen Knoten weitergegeben werden. Ist beispielsweise einem Benutzer in einem bestimmten Knoten eine Rolle zugewiesen, hat der Benutzer die Berechtigungen dieser Rolle für den bestimmten Knoten und für jeden darunter befindlichen Knoten. Außerdem werden alle Eigenschaftenschlüssel und erweiterte Typen, die für einen bestimmten Knoten definiert sind, an alle Knoten unter diesem Knoten vererbt.

Graph-_Filterung_ ermöglicht es Benutzern, Anforderungsergebnisse nach IDs, Namen, Typen, Untertypen, übergeordnetem Raum, zugeordnetem Raum, Sensordatentypen, Eigenschaftenschlüsseln und -werten, Durchlaufen (traverse), minLevel, maxLevel und anderen OData-Filterparametern einzugrenzen.

Graph-_durchlaufen_ ermöglicht es Benutzern im Raumgraphen in die Tiefe und die Breite zu navigieren. Hinsichtlich Tiefe kann ein Graph von oben nach unten oder von unten nach oben über die Navigationsparameter `traverse`, `minLevel` und `maxLevel` durchlaufen werden. Hinsichtlich Breite kann im Graph navigiert werden, um nebengeordnete Knoten direkt mit einem übergeordneten Raum oder einem ihrer Nachfolgerelemente zu verknüpfen. Wenn Sie ein Objekt abfragen, können Sie alle Objekte, die eine Beziehung mit diesem Objekt haben, ermitteln, indem Sie den `includes`-Parameter der GET-APIs verwenden.

Azure Digital Twins garantiert Graph-_Skalierbarkeit_, sodass Ihre tatsächlichen Workloads verarbeitet werden können. Digital Twins kann verwendet werden, um große Portfolios von Immobilien, Infrastruktur, Geräten, Sensoren, Telemetrie usw. darzustellen.

Graph-_Erweiterbarkeit_ ermöglicht es Benutzern, die zugrunde liegenden Digital Twins-Objektmodelle durch neue Typen und Ontologien anzupassen. Die Daten Ihrer digitalen Zwillinge können auch mit erweiterbaren Eigenschaften und Werten ergänzt werden.

### <a name="spatial-intelligence-graph-management-apis"></a>Verwaltungs-APIs für Raumintelligenzgraph

Sobald Sie Azure Digital Twins über das [Azure-Portal](https://portal.azure.com) bereitgestellt haben, wird die [Swagger](https://swagger.io/tools/swagger-ui/)-URL der Verwaltungs-APIs automatisch generiert und im Azure-Portal in dessen Abschnitt **Übersicht** im folgenden Format angezeigt:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Name des benutzerdefinierten Attributs | Ersetzen durch |
| --- | --- |
| `yourInstanceName` | Den Namen Ihrer Azure Digital Twins-Instanz |
| `yourLocation` | Die Serverregion, in der Ihre Instanz gehostet wird |

 Das vollständige URL-Format, das verwendet wird, ist in der folgenden Abbildung zu sehen:

![Digital Twins-Verwaltungs-API im Portal][2]

Wenn Sie weitere Informationen zur Verwendung von Raumintelligenzgraphen wünschen, sehen Sie sich die Vorschau der Azure Digital Twins-Verwaltungs-APIs an.

> [!TIP]
> Es wird eine Swagger-Vorschau bereitgestellt, um den API-Funktionsumfang zu veranschaulichen.
> Diese Vorschau finden Sie unter [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Erfahren Sie mehr über das [Verwenden von Swagger](how-to-use-swagger.md).

Alle API-Aufrufe müssen mit [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code) authentifiziert werden. Für die APIs gelten die [REST-API-Richtlinien von Microsoft](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Für die meisten APIs, die Auflistungen zurückgeben, werden die [OData](http://www.odata.org/getting-started/basic-tutorial/#queryData)-Systemabfrageoptionen unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Informationen über Gerätekonnektivität und darüber, wie Telemetrienachrichten an Azure Digital Twins gesendet werden, finden Sie unter [Gerätekonnektivität und eingehende Telemetriedaten](concepts-device-ingress.md).

Informationen über Einschränkungen und Drosselungen von Verwaltungs-APIs finden Sie unter [Diensteinschränkungen der öffentlichen Vorschauversion](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
