---
title: Übersicht über Azure Digital Twins | Microsoft-Dokumentation
description: Hier finden Sie weitere Informationen zu Azure Digital Twins – einer Azure IoT-Lösung für Raumintelligenz.
author: julieseto
ms.author: jseto
ms.date: 10/10/2018
ms.topic: overview
ms.service: digital-twins
services: digital-twins
manager: bertvanhoof
ms.custom: mvc
ms.openlocfilehash: 85b67683730c8352051b4d6b48f813576b11615b
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322754"
---
# <a name="overview-of-azure-digital-twins"></a>Übersicht über Azure Digital Twins

Azure Digital Twins ist ein Azure IoT-Dienst zur Erstellung umfassender Modelle der physischen Umgebung. Er bietet zahlreiche Features – darunter die Erstellung von Raumintelligenzgraphen, um die Beziehungen und Interaktionen zwischen Personen, Bereichen und Geräten zu modellieren.

Mit Azure Digital Twins lassen sich Daten aus einem physischen Bereich abfragen anstatt von vielen verschiedenen Sensoren. Dieser Dienst hilft Ihnen bei der Erstellung wiederverwendbarer, hochgradig skalierbarer und raumbezogener Umgebungen, die Streamingdaten aus der digitalen und der physischen Welt miteinander verknüpfen. Ganz gleich, ob Sie den Wartungsbedarf für eine Produktionsstätte prognostizieren, in Echtzeit den Strombedarf eines Stromnetzes analysieren oder den Platz in einem Büro optimal nutzen möchten: Ihre Apps werden durch diese äußerst relevanten kontextbezogenen Features noch besser.

Azure Digital Twins kann für alle Arten von Umgebungen verwendet werden. Hierzu zählen beispielsweise Lagerhallen, Büros, Bildungseinrichtungen, Krankenhäuser, Banken, Stadien, Produktionsstätten, Parkplätze, Parks, Smart Grids und Städte. Mögliche Szenarien wären etwa die Überwachung der Tagestemperatur in mehreren Bundesländern, die Überwachung intensiv genutzter Drohnenpfade, die Erkennung autonomer Fahrzeuge, die Analyse der Raumbelegung eines Gebäudes sowie die Ermittlung der am stärksten ausgelasteten Kasse in Ihrem Ladengeschäft. Über Azure Digital Twins kann für jedes geschäftliche Szenario eine entsprechende digitale Instanz bereitgestellt werden.

Das folgende Video enthält ausführlichere Informationen zu Digital Twins:

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Wichtige Funktionen

Azure Digital Twins bietet unter anderem folgende wichtige Funktionen:

### <a name="spatial-intelligence-graph"></a>Raumintelligenzgraph

Der [*Raumintelligenzgraph*](./concepts-objectmodel-spatialgraph.md) oder *Raumgraph* ist eine virtuelle Darstellung der physischen Umgebung, mit der Sie die Beziehungen zwischen Personen, Orten und Geräten modellieren können.

Stellen Sie sich eine intelligente Versorger-App vor, die zahlreiche Stromzähler eines Viertels umfasst. Zur präzisen Überwachung und Prognose des Stromverbrauchs sowie zu Abrechnungszwecken muss das Versorgungsunternehmen jedes Gerät und jeden Sensor im Kontext des Orts und des Kunden modellieren, der die Abrechnung erhält. Diese komplexen Beziehungen können mithilfe des Raumintelligenzgraphen modelliert werden.

### <a name="digital-twin-object-models"></a>Objektmodelle für digitale Zwillinge

[Objektmodelle für digitale Zwillinge](./concepts-objectmodel-spatialgraph.md) sind vordefinierte Geräteprotokolle und Datenschemas, die sich an den domänenspezifischen Anforderungen Ihrer Lösung orientieren, um die Entwicklung zu beschleunigen und zu vereinfachen.

So kann beispielsweise eine Raumbelegungsanwendung vordefinierte Bereichstypen wie Campus, Gebäude, Etage, Raum usw. verwenden.

### <a name="multiple-and-nested-tenants"></a>Mehrere und geschachtelte Mandanten

Sie können Lösungen erstellen, die sicher skaliert und für mehrere Mandanten wiederverwendet werden können. Darüber hinaus können Sie mehrere Untermandanten erstellen, die isoliert und sicher genutzt werden können.

Eine Raumnutzungs-App kann beispielsweise so konfiguriert werden, dass die Daten eines Mandanten von den Daten anderer Mandanten in einem bestimmten Gebäude isoliert oder die Daten für einen einzelnen Mandanten mit mehreren Gebäuden kombiniert werden.

### <a name="advanced-compute-capabilities"></a>Erweiterte Computefunktionen

Dank erweiterter Computefunktionen ([benutzerdefinierte Funktionen](./concepts-user-defined-functions.md)) können Sie angepasste Funktionen für eingehende [Gerätedaten](./concepts-device-ingress.md) definieren und ausführen, um Signale an vordefinierte Endpunkte zu senden. Diese Funktion verbessert die Anpassung und Automatisierung von Geräteaufgaben.

Ein Beispiel wäre etwa eine intelligente Anwendung für die Landwirtschaft mit einer benutzerdefinierten Funktion zur Auswertung der Messwerte von Bodenfeuchtigkeitssensoren und der Wettervorhersage, um daraufhin Signale zum Bewässerungsbedarf zu senden.

### <a name="built-in-access-control"></a>Integrierte Zugriffssteuerung

Zugriffs- und Identitätsverwaltungsfeatures wie [rollenbasierte Zugriffssteuerung](./security-role-based-access-control.md) und [Azure Active Directory](./security-authenticating-apis.md) ermöglichen eine sichere Steuerung des Zugriffs für Personen und Geräte.

Eine Gebäudeverwaltungs-App kann beispielsweise so konfiguriert werden, dass Bewohner die eigene Raumtemperatur nur innerhalb eines bestimmten Bereichs regeln und Gebäudetechniker die Temperatur in jedem Raum auf einen beliebigen Wert festlegen können.

### <a name="ecosystem"></a>Ökosystem

Eine Azure Digital Twins-Instanz kann mit zahlreichen leistungsfähigen Azure-Diensten verknüpft werden. Hierzu zählen unter anderem Azure Stream Analytics, Azure AI und Azure Storage sowie Azure Maps, Microsoft Mixed Reality, Dynamics 365 und Office 365.

Eine intelligente Bürogebäudeanwendung kann mithilfe von Azure Digital Twins beispielsweise Teams und Geräte auf verschiedenen Stockwerken darstellen. Die Livedaten, die Geräte an die bereitgestellte Digital Twins-Instanz streamen, können von Azure Stream Analytics verarbeitet werden, um wichtige praxisrelevante Erkenntnisse zu gewinnen. Anschließend können die Daten in Azure Storage gespeichert, in ein weitergebbares Dateiformat konvertiert und über Office 365 in der gesamten Organisation verteilt werden.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Lösungen, die von Azure Digital Twins profitieren

Azure Digital Twins vereinfacht die IoT-Modellierung, Datenverarbeitung, Ereignisbehandlung und Geräteüberwachung, um die physische Welt darzustellen. Im Anschluss finden Sie eine kleine Auswahl von Szenarien aus verschiedenen Branchen, die von der Verwendung dieser Lösung profitieren:

* Veranschaulichung der Belegungsentwicklung eines Bereichs zur Ermittlung der optimalen Konfiguration des Bürogebäudes einer Immobilienverwaltung
* Auslösung von Arbeitsauftragstickets für eine mobile App zur Entsendung von Wachpersonal, Planung von Hausmeisterdiensten und beliebigen anderen Diensten im Einzelhandel oder an einer Sportstätte
* Anzeige der belegten Räume eines Gebäudes in Echtzeit für einen Interessenten und Unterstützung des Interessenten beim Reservieren geeigneter Arbeitsbereiche
* Nachverfolgung des Orts, an dem sich Ressourcen innerhalb eines Bereichs befinden
* Optimierung der Aufladung von Elektrofahrzeugen durch Modellierung von Benutzerpräferenzen und Einschränkungen des Stromnetzes

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Azure Digital Twins im Kontext anderer IoT-Dienste

Azure Digital Twins nutzt Azure IoT Hub, um eine Verbindung mit den IoT-Geräten und Sensoren herzustellen, die jeweils aktuelle Daten aus der physischen Welt liefern. Das folgende Diagramm veranschaulicht die Beziehung zwischen Azure Digital Twins und anderen Azure IoT-Diensten:

![Azure Digital Twins ist ein auf Azure IoT Hub basierender Dienst.](./media/overview/azure-digital-twins-in-iot-ecosystem.png)

Eine ausführlichere Beschreibung des restlichen IoT-Ökosystems finden Sie unter [Azure IoT-Technologien und -Lösungen](https://docs.microsoft.com/azure/iot-fundamentals/iot-services-and-technologies).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich eine kurze Demonstration der Funktionen von Azure Digital Twins an:

> [!div class="nextstepaction"]
> [Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins](./quickstart-view-occupancy-dotnet.md)

Machen Sie sich ausführlich mit einer Gebäudeverwaltungsanwendung vertraut, die Azure Digital Twins nutzt:

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen von Azure Digital Twins und Konfigurieren eines Raumgraphen](./tutorial-facilities-setup.md)

Machen Sie sich mit zentralen Aspekten von Azure Digital Twins vertraut:

> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](./concepts-objectmodel-spatialgraph.md) (Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen)
