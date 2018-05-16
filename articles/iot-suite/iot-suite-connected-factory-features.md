---
title: 'Features der Connected Factory-Lösung: Azure | Microsoft-Dokumentation'
description: Hier erhalten Sie einen Überblick über die Features der vorkonfigurierten Connected Factory-Lösung.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: dobett
ms.openlocfilehash: e0bd023e73088897303fe0f9de47641cd6ea0389
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-azure-iot-suite-connected-factory"></a>Was ist Connected Factory von Azure IoT Suite?

Connected Factory ist eine Open Source-Lösung und dient zur Implementierung der Azure IoT-Referenz von Microsoft in der Industrie. Sie können sie als Grundlage für ein kommerzielles Produkt verwenden. Sie können über [Azure IoT Suite](https://www.azureiotsuite.com/#solutions/types/CF) eine vorgefertigte Version der Connected Factory-Lösung in Ihrem Azure-Abonnement bereitstellen.

![Dashboard der Connected Factory-Lösung](media/iot-suite-connected-factory-features/dashboard.png)

Connected Factory beinhaltet die folgenden Features:

## <a name="industrial-device-interoperability"></a>Interoperabilität von Industriegeräten

- Herstellen einer Verbindung mit Industrieanlagen über eine OPC UA-Schnittstelle
- Anzeigen von Livetelemetriedaten mithilfe der simulierten Produktionslinien (mit OPC UA-Servern in Docker-Containern)
- Durchsuchen des OPC UA-Informationsmodells der OPC UA-Server über ein Clouddashboard

## <a name="remote-management"></a>Remoteverwaltung

- Konfigurieren von OPC UA-Anlagen über das Clouddashboard (Aufrufen von Methoden, Lesen und Schreiben von Daten)
- Veröffentlichen und Aufheben der Veröffentlichung von Telemetriedaten aus Ihren OPC UA-Ressourcen über das Clouddashboard

## <a name="cloud-dashboard"></a>Clouddashboard

- Anzeigen einer Vorschau von Telemetriedaten direkt auf einem Clouddashboard
- Anzeigen von Trends bei Telemetriedaten und Erstellen von Korrelationen mithilfe des Time Series Insights-Explorer-Dashboards
- Anzeigen der berechneten gesamten Geräteeffizienz (Overall Equipment Efficiency, OEE) und Key Performance Indicators (KPIs) über ein Clouddashboard
- Anzeigen der Hierarchien von Industrieanlagen in einer Baumtopologie und auf einer interaktiven Karte
- Anzeigen, Bestätigen und Schließen von Warnungen über ein Clouddashboard

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

- [Azure Time Series Insights](../time-series-insights/time-series-insights-overview.md) ist zum Speichern, Visualisieren und Abfragen großer Mengen an Zeitreihendaten ausgelegt. Connected Factory nutzt diesen Dienst.
- Connected Factory kann in diesen Dienst integriert werden und ermöglicht Ihnen dadurch die umfassende Echtzeitanalyse Ihrer Gerätedaten.

## <a name="rules-and-alerts"></a>Regeln und Warnungen

[Konfigurieren von schwellenwertbasierten Regeln für Warnungen](iot-suite-connected-factory-configure.md)

## <a name="end-to-end-security"></a>End-to-End-Sicherheit

- Konfigurieren von Sicherheitsberechtigungen für Benutzer mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)
- Implementierung von End-to-End-Verschlüsselung mithilfe von OPC UA-Authentifizierung (mit X.509-Zertifikaten) sowie Sicherheitstoken

## <a name="customizability"></a>Anpassbarkeit

- [Anpassen](iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md) der Lösung an bestimmte Unternehmensanforderungen.
- Der vollständige Quellcode der Lösung ist auf GitHub verfügbar. Informationen finden Sie im Repository zur [vorkonfigurierten Connected Factory-Lösung](https://github.com/Azure/azure-iot-connected-factory).

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die vorkonfigurierte Connected Factory-Lösung:

* [Vorkonfigurierte Connected Factory-Lösung – Exemplarische Vorgehensweise](iot-suite-connected-factory-sample-walkthrough.md)
* [Bereitstellen eines Edge-Gateways für die vorkonfigurierte Connected Factory-Lösung unter Windows oder Linux]( iot-suite-connected-factory-gateway-deployment.md)
