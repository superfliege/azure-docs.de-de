---
title: Architektonische Konzepte in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Architektur von Azure IoT Central vorgestellt.
author: dominicbetts
ms.author: dobett
ms.date: 03/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 4f4b917808f4973dc83294391f58d7e0e2d01c4c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798809"
---
# <a name="azure-iot-central-architecture"></a>Azure IoT Central-Architektur

Dieser Artikel enthält eine Übersicht über die Architektur von Microsoft Azure IoT Central.

![Architektur im Überblick](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Geräte

Geräte tauschen Daten mit Ihrer Azure IoT Central-Anwendung aus. Ein Gerät kann Folgendes durchführen:

- Senden von Messungen wie Telemetrie
- Synchronisieren von Einstellungen mit Ihrer Anwendung

In Azure IoT Central werden die Daten, die ein Gerät mit Ihrer Anwendung austauschen kann, in einer Gerätevorlage angegeben. Weitere Informationen zu Gerätevorlagen finden Sie unter [Metadatenverwaltung](#metadata-management).

Um mehr darüber zu erfahren, wie Geräte eine Verbindung mit Ihrer Azure IoT Central-Anwendung herstellen können, lesen Sie [Gerätekonnektivität](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Cloudgateway

Azure IoT Central verwendet Azure IoT Hub als Cloudgateway, das Gerätekonnektivität ermöglicht. IoT Hub ermöglicht Folgendes:

- Datenerfassung im großen Maßstab in der Cloud
- Geräteverwaltung
- Sichere Gerätekonnektivität

Weitere Informationen zu IoT Hub finden Sie unter [Dokumentation zu IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

Weitere Informationen zur Gerätekonnektivität in Azure IoT Central finden Sie unter [Gerätekonnektivität](concepts-connectivity.md).

## <a name="data-stores"></a>Datenspeicher

Azure IoT Central speichert Anwendungsdaten in der Cloud. Zu den gespeicherten Anwendungsdaten zählen Folgende:

- Gerätevorlagen
- Geräteidentitäten
- Gerätemetadaten
- Benutzer- und Rollendaten

Azure IoT Central verwendet einen Zeitreihenspeicher für die Messdaten, die von Ihren Geräten gesendet werden. Die Zeitreihendaten von Geräten werden vom Analysedienst verwendet.

## <a name="analytics"></a>Analytics

Der Analysedienst ist für das Generieren von benutzerdefinierten Berichterstellungsdaten zuständig, die die Anwendung anzeigt. Ein Operator kann die [Analysen anpassen](howto-create-analytics.md), die in der Anwendung angezeigt werden. Der Analysedienst basiert auf [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) und verarbeitet die von Ihren Geräten gesendeten Messdaten.

## <a name="rules-and-actions"></a>Regeln und Aktionen

[Regeln und Aktionen](howto-create-telemetry-rules.md) werden zur Automatisierung von Aufgaben in der Anwendung ausgiebig zusammen verwendet. Ein Ersteller kann Regeln basierend auf Gerätetelemetrie definieren, wie etwa Temperaturen, die einen definierten Schwellenwert definieren. Azure IoT Central verwendet einen Datenstromprozessor, um zu bestimmen, wann die Regelbedingungen erfüllt sind. Wenn eine Regelbedingung erfüllt ist, löst diese eine vom Ersteller definierte Aktion aus. Eine Aktion kann z.B. eine E-Mail senden, um einen Techniker darüber zu benachrichtigen, dass die Temperatur in einem Gerät zu hoch ist.

## <a name="metadata-management"></a>Metadatenverwaltung

In einer Azure IoT Central-Anwendung definieren Gerätevorlagen das Verhalten und die Funktion der Gerätetypen. So gibt z.B. die Gerätevorlage eines Kühlschranks die Telemetrie an, die ein Kühlschrank an Ihre Anwendung sendet.

![Vorlagenarchitektur](media/concepts-architecture/template_architecture.png)

Eine Gerätevorlage gibt Folgendes an:

- **Messungen** geben die Telemetrie an, die das Gerät an die Anwendung sendet.
- **Einstellungen** geben die Konfigurationen an, die ein Operator festlegen kann.
- **Eigenschaften** geben Metadaten an, die ein Operator festlegen kann.
- **Regeln** automatisieren das Verhalten in der Anwendung basierend auf Daten, die von einem Gerät gesendet werden.
- **Dashboards** sind anpassbare Ansichten eines Geräts in der Anwendung.

Eine Anwendung kann ein oder mehrere simulierte und echte Geräte basierend auf den einzelnen Gerätevorlagen verwenden.

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Ein Administrator kann mithilfe der vordefinierten Rollen [Zugriffsregeln für eine Azure IoT Central-Anwendung definieren](howto-administer.md). Ein Administrator kann Benutzern Rollen zuweisen, die bestimmen, auf welche Anwendungsbereiche der Benutzer Zugriff hat.

## <a name="security"></a>Sicherheit

Zu den Sicherheitsfeatures in Azure IoT Central gehören Folgende:

- Während der Übertragung und im Ruhezustand verschlüsselte Daten
- Die von Azure Active Directory oder vom Microsoft-Konto bereitgestellte Authentifizierung. Unterstützung für zweistufige Authentifizierung
- Vollständige Mandantenisolation
- Sicherheit auf Geräteebene

## <a name="ui-shell"></a>UI Shell

Die UI Shell ist eine moderne, reaktionsfähige und browserbasierte HTML5-Anwendung.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Architektur von Azure IoT Central erfahren haben, besteht der nächste empfohlene Schritt darin, sich mit der [Gerätekonnektivität](concepts-connectivity.md) in Azure IoT Central vertraut zu machen.