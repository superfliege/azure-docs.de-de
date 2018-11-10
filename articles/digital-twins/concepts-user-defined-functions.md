---
title: Datenverarbeitung und benutzerdefinierte Funktionen mit Azure Digital Twins | Microsoft-Dokumentation
description: Übersicht über Datenverarbeitung, Matcher und benutzerdefinierte Funktionen mit Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: a45f82b142ee4f4c9c88ea755607b88323feaae5
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210124"
---
# <a name="data-processing-and-user-defined-functions"></a>Datenverarbeitung und benutzerdefinierte Funktionen

Azure Digital Twins stellt erweiterte Computefunktionen bereit. Entwickler können benutzerdefinierte Funktionen erstellen und mit diesen Funktionen eingehende Telemetrienachrichten verarbeiten, um Ereignisse an vordefinierte Endpunkte zu senden.

## <a name="data-processing-flow"></a>Datenverarbeitungsfluss

Sobald Geräte Telemetriedaten an Digital Twins senden, können Entwickler Daten in vier Phasen verarbeiten: _Überprüfung_ (Validate), _Abgleich_ (Match), _Berechnung_ (Compute) und _Versendung_ (Dispatch):

![Datenverarbeitungsfluss in Digital Twins][1]

1. In der _Validierungsphase_ wird eine eingehende Telemetrienachricht in ein allgemein verständliches Format eines [**Datenübertragungsobjekts**](https://en.wikipedia.org/wiki/Data_transfer_object) transformiert. In dieser Phase erfolgt auch die Überprüfung von Geräten und Sensoren.
1. In der _Abgleich_-Phase wird nach den entsprechenden benutzerdefinierten Funktionen gesucht, die ausgeführt werden sollen. Vordefinierte Matcher (Abgleicher) finden die benutzerdefinierten Funktionen anhand der Geräte-, Sensor- und Rauminformationen aus der eingehenden Telemetrienachricht.
1. In der _Berechnung_-Phase werden die benutzerdefinierten Funktionen ausgeführt, für die in der vorherigen Phase eine Übereinstimmung gefunden wurde. In diesen Funktionen können berechnete Werte aus Raumgraphknoten gelesen und aktualisiert und können benutzerdefinierte Benachrichtigungen ausgegeben werden.
1. In der _Versendung_-Phase werden alle benutzerdefinierten Benachrichtigungen aus der Berechnungsphase an Endpunkte gesendet, die im Graphen definiert sind.

## <a name="data-processing-objects"></a>Datenverarbeitungsobjekte

Datenverarbeitung in Azure Digital Twins besteht aus dem Definieren von drei Objekten: _Matcher_, _benutzerdefinierte Funktionen_ und _Rollenzuweisungen_:

![Datenverarbeitungsfluss in Digital Twins][2]

### <a name="matchers"></a>Matcher (Abgleicher)

_Matcher_ definieren eine Reihe von Bedingungen, mit denen anhand der eingehenden Telemetriedaten eines Sensors bestimmt wird, welche Aktionen auszuführen sind. In diesen Bedingungen können Eigenschaften des Sensors, des übergeordneten Geräts des Sensors und des übergeordneten Raums des Sensors enthalten sein. Die Bedingungen sind als Vergleiche mit einem jeweiligen [JSON-Pfad](http://jsonpath.com/) (path) formuliert, wie dies im folgenden Beispiel skizziert ist:

- Alle Sensoren, die den Datentyp **Temperature** aufweisen.
- Für die `01` in deren Anschlüssen steht.
- Die zu Geräten gehören, für die der erweiterte Eigenschaftenschlüssel **Manufacturer** auf den Wert `"GoodCorp"` festgelegt ist.
- Die zu Räumen des Typs `"Venue"` gehören.
- Die Nachfolger der übergeordneten **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD` sind.

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
> - In JSON-Pfaden wird die Groß-/Kleinschreibung beachtet.
> - Die JSON-Nutzlast ist identisch mit der Nutzlast, die zurückgegeben würde von:
>   - `/sensors/{id}?includes=properties,types` für den Sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` für das übergeordnete Gerät des Sensors.
>   - `/spaces/{id}?includes=properties,types,location,timezone` für den übergeordneten Raum des Sensors.
> - In den Vergleichen wird die Groß-/Kleinschreibung beachtet.

### <a name="user-defined-functions"></a>Benutzerdefinierte Funktionen

Eine _benutzerdefinierte Funktion_, oder _UDF_ (User-Defined Function), ist eine angepasste Funktion, die in einer isolierten Umgebung in Azure Digital Twins ausgeführt wird. Eine UDF hat sowohl Zugriff auf die unformatierte Telemetrienachricht eines Sensors so, wie sie empfangen wurde, als auch auf den Raumgraphen und den Versendungsdienst. Nachdem die UDF im Graphen registriert wurde, muss ein Matcher (Beschreibung weiter oben) erstellt werden, um anzugeben, wann die UDF ausgeführt werden soll. Wenn Digital Twins neue Telemetriedaten von einem bestimmten Sensor empfängt, kann die übereinstimmende UDF z. B. einen gleitenden Durchschnitt der letzten x Sensormesswerte berechnen.

UDFs können in JavaScript geschrieben werden und ermöglichen es Entwicklern, benutzerdefinierte Codeausschnitte für Telemetrienachrichten von Sensoren auszuführen. Außerdem gibt es Hilfsmethoden, über die in der benutzerdefinierten Ausführungsumgebung auf den Graphen zugegriffen werden kann. Mit einer benutzerdefinierten Funktion hat ein Entwickler folgende Möglichkeiten:

- Festlegen eines Sensormesswerts direkt im Sensorobjekt im Graphen
- Ausführen einer Aktion basierend auf verschiedenen Sensormesswerten in einem Raum im Graphen
- Erstellen einer Benachrichtigung, wenn bestimmte Bedingungen für einen eingehenden Sensormesswert erfüllt sind
- Hinzufügen von Graphmetadaten zu einem Sensormesswert, bevor eine Benachrichtigung gesendet wird

Weitere Informationen finden Sie unter [Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](how-to-user-defined-functions.md).

### <a name="role-assignment"></a>Rollenzuweisung

Die Aktionen einer benutzerdefinierten Funktion unterliegen der rollenbasierten Zugriffssteuerung von Digital Twins, um die Daten im Dienst zu schützen. Rollenzuweisungen stellen sicher, dass eine bestimmte benutzerdefinierte Funktion die entsprechenden Berechtigungen für Zugriffe auf den Raumgraphen hat. Beispielsweise könnte eine benutzerdefinierte Funktion versuchen, Graphdaten unter einem bestimmten Raum zu erstellen, zu lesen, zu aktualisieren oder zu löschen. Die Zugriffsebene einer benutzerdefinierten Funktion wird überprüft, wenn diese Daten aus dem Graphen abrufen möchte oder versucht, eine Aktion auszuführen. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung](security-create-manage-role-assignments.md).

Es ist möglich, dass ein Matcher eine benutzerdefinierte Funktion auslöst, die keine Rollenzuweisungen hat. In diesem Fall kann die benutzerdefinierte Funktion keine Daten aus dem Graphen lesen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Ereignisse und Telemetrienachrichten an andere Azure-Dienste weitergeleitet werden, finden Sie unter [Weiterleiten von Ereignissen und Nachrichten](concepts-events-routing.md).

Weitere Informationen dazu, wie Matcher, benutzerdefinierte Funktionen und Rollenzuweisungen erstellt werden, finden Sie unter [Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
