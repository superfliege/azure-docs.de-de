---
title: Datenverarbeitung und benutzerdefinierte Funktionen mit Azure Digital Twins | Microsoft-Dokumentation
description: Übersicht über die Datenverarbeitung, Matcher und benutzerdefinierte Funktionen in Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 2703778cd2eab582a9e7311aaf2024f100261889
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624521"
---
# <a name="data-processing-and-user-defined-functions"></a>Datenverarbeitung und benutzerdefinierte Funktionen

Azure Digital Twins stellt erweiterte Computefunktionen bereit. Entwickler können benutzerdefinierte Funktionen erstellen und für eingehende Telemetrienachrichten ausführen, um Ereignisse an vordefinierte Endpunkte zu senden.

## <a name="data-processing-flow"></a>Datenverarbeitungsfluss

Nachdem Geräte Telemetriedaten an Azure Digital Twins gesendet haben, können Entwickler Daten in vier Phasen verarbeiten: Überprüfen, Abgleichen, Berechnen und Versenden.

![Datenverarbeitungsfluss in Azure Digital Twins][1]

1. In der Phase „Überprüfen“ wird die eingehende Telemetrienachricht in ein allgemein verständliches Format eines [Datenübertragungsobjekts](https://en.wikipedia.org/wiki/Data_transfer_object) transformiert. In dieser Phase erfolgt auch die Überprüfung von Geräten und Sensoren.
1. In der Phase „Abgleichen“ wird nach den entsprechenden benutzerdefinierten Funktionen (User-Defined Functions, UDFs) gesucht, die ausgeführt werden sollen. Vordefinierte Matcher (Abgleicher) ermitteln die benutzerdefinierten Funktionen anhand der Geräte-, Sensor- und Gebäudebereichsinformationen aus der eingehenden Telemetrienachricht.
1. In der Phase „Berechnen“ werden die benutzerdefinierten Funktionen ausgeführt, für die in der vorherigen Phase eine Übereinstimmung gefunden wurde. Diese Funktionen können berechnete Werte aus Raumgraphknoten lesen und aktualisieren sowie benutzerdefinierte Benachrichtigungen ausgeben.
1. In der Phase „Versenden“ werden alle benutzerdefinierten Benachrichtigungen aus der Berechnungsphase an Endpunkte gesendet, die im Graphen definiert sind.

## <a name="data-processing-objects"></a>Datenverarbeitungsobjekte

Die Datenverarbeitung in Azure Digital Twins umfasst die Definition von drei Objekten: Matcher, benutzerdefinierte Funktionen und Rollenzuweisungen.

![Datenverarbeitungsobjekte in Azure Digital Twins][2]

### <a name="matchers"></a>Matcher (Abgleicher)

Matcher definieren eine Reihe von Bedingungen, mit denen anhand der eingehenden Telemetriedaten eines Sensors bestimmt wird, welche Aktionen ausgeführt werden. In diesen Bedingungen können Eigenschaften des Sensors, des übergeordneten Geräts des Sensors und des übergeordneten Gebäudebereichs des Sensors enthalten sein. Die Bedingungen werden wie im folgenden Beispiel dargestellt als Vergleiche mit einem [JSON-Pfad](http://jsonpath.com/) formuliert:

- Alle Sensoren vom Datentyp **Temperature**
- Die den Wert `01` im Port aufweisen
- Die zu Geräten gehören, für die der erweiterte Eigenschaftenschlüssel **Manufacturer** auf den Wert `"GoodCorp"` festgelegt ist
- Die zu Gebäudebereichen des Typs `"Venue"` gehören
- Die Nachfolger der übergeordneten **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD` sind

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - Bei JSON-Pfaden muss die Groß-/Kleinschreibung beachtet werden.
> - Die JSON-Nutzlast ist identisch mit der Nutzlast, die für Folgendes zurückgegeben wird:
>   - `/sensors/{id}?includes=properties,types` für den Sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` für das übergeordnete Gerät des Sensors.
>   - `/spaces/{id}?includes=properties,types,location,timezone` für den übergeordneten Raum des Sensors.
> - Bei den Vergleichen muss die Groß-/Kleinschreibung nicht beachtet werden.

### <a name="user-defined-functions"></a>Benutzerdefinierte Funktionen

Eine benutzerdefinierte Funktion ist eine angepasste Funktion, die in einer isolierten Umgebung in Azure Digital Twins ausgeführt wird. Benutzerdefinierte Funktionen haben Zugriff auf die empfangene unformatierte Telemetrienachricht eines Sensors. Benutzerdefinierte Funktionen haben auch Zugriff auf den Raumgraphen und den Verteilerdienst. Nachdem die benutzerdefinierte Funktion im Graphen registriert wurde, muss ein Matcher (Beschreibung weiter oben) erstellt werden, um anzugeben, wann die benutzerdefinierte Funktion ausgeführt werden soll. Wenn Azure Digital Twins neue Telemetriedaten von einem bestimmten Sensor empfängt, kann die übereinstimmende benutzerdefinierte Funktion beispielsweise einen gleitenden Durchschnitt der letzten Sensormesswerte berechnen.

Benutzerdefinierte Funktionen können in JavaScript geschrieben werden. Entwickler können benutzerdefinierte Codeausschnitte für Telemetrienachrichten von Sensoren ausführen. Hilfsmethoden interagieren mit dem Graphen in der benutzerdefinierten Ausführungsumgebung. Mit einer benutzerdefinierten Funktion hat ein Entwickler folgende Möglichkeiten:

- Festlegen eines Sensormesswerts direkt im Sensorobjekt im Graphen
- Ausführen einer Aktion basierend auf verschiedenen Sensormesswerten in einem Raum im Graphen
- Erstellen einer Benachrichtigung, wenn bestimmte Bedingungen für einen eingehenden Sensormesswert erfüllt sind
- Hinzufügen von Graphmetadaten zu einem Sensormesswert, bevor eine Benachrichtigung gesendet wird

Weitere Informationen finden Sie unter [Definieren von benutzerdefinierten Funktionen in Azure Digital Twins](how-to-user-defined-functions.md).

### <a name="role-assignment"></a>Rollenzuweisung

Die Aktionen einer benutzerdefinierten Funktion unterliegen der rollenbasierten Zugriffssteuerung von Azure Digital Twins, um die Daten im Dienst zu schützen. Rollenzuweisungen stellen sicher, dass eine bestimmte benutzerdefinierte Funktion über die entsprechenden Berechtigungen für die Interaktion mit dem Raumgraphen verfügt. Beispielsweise könnte eine benutzerdefinierte Funktion versuchen, Graphdaten unter einem bestimmten Raum zu erstellen, zu lesen, zu aktualisieren oder zu löschen. Die Zugriffsebene einer benutzerdefinierten Funktion wird überprüft, wenn diese Daten aus dem Graphen abrufen möchte oder versucht, eine Aktion auszuführen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung](security-create-manage-role-assignments.md).

Es ist möglich, dass ein Matcher eine benutzerdefinierte Funktion auslöst, die keine Rollenzuweisungen hat. In diesem Fall kann die benutzerdefinierte Funktion keine Daten aus dem Graphen lesen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Weiterleiten von Ereignissen und Telemetrienachrichten an andere Azure-Dienste finden Sie unter [Weiterleiten von Ereignissen und Nachrichten](concepts-events-routing.md).

* Weitere Informationen dazu, wie Matcher, benutzerdefinierte Funktionen und Rollenzuweisungen erstellt werden, finden Sie unter [Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
