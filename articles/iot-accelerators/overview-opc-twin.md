---
title: Was ist die OPC UA-Geräteverwaltung von Azure IoT? | Microsoft-Dokumentation
description: Übersicht über OPC Twin
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 3bbc20aa61faafb1405fa756b334bae05ec4129b
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759716"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-device-management"></a>Was ist die OPC-Geräteverwaltung (Open Platform Communications) von Azure IoT?
Die OPC UA-Geräteverwaltung von Azure IoT wird auch als OPC Twin bezeichnet und umfasst Microservices, die über Azure IoT Edge und IoT Hub eine Verbindung zwischen Cloud und Fabriknetzwerk herstellen. OPC Twin bietet Erkennungs- und Registrierungsfunktionen und ermöglicht die Fernsteuerung von Industriegeräten über Rest-APIs. OPC Twin erfordert kein OPC UA SDK (Unified Architecture), ist unabhängig von der Programmiersprache und kann in einen serverlosen Workflow integriert werden. In diesem Artikel werden einige Anwendungsfälle für OPC Twin beschrieben.

## <a name="discovery-and-control"></a>Ermittlung und Steuerung
OPC Twin ermöglicht eine einfache Ermittlung und Registrierung.

### <a name="simple-discovery-and-registration"></a>Einfache Ermittlung und Registrierung
Mit OPC Twin können Bediener das Fabriknetzwerk scannen, um die Ermittlung und Registrierung von OPC UA-Servern zu ermöglichen. Alternativ können OPC UA-Geräte von Bedienern auch manuell unter Verwendung einer bekannten Ermittlungs-URL registriert werden. Ein Bediener kann also beispielsweise remote einen Scanvorgang des Netzwerks initiieren und alle OPC UA-Server anzeigen, um eine Verbindung mit allen OPC UA-Geräten herzustellen, nachdem das IoT Edge-Gateway mit einem OPC Twin-Modul im Fertigungsbereich installiert wurde. 

### <a name="simple-control"></a>Einfache Steuerung
Mit OPC Twin können Bediener auf Ereignisse reagieren und ihre Computer im Fertigungsbereich über die Cloud neu konfigurieren (entweder automatisch oder manuell bei Bedarf). OPC Twin bietet REST-APIs, mit denen Sie Dienste auf dem OPC UA-Server aufrufen, seinen Adressraums durchsuchen sowie Variablen lesen/schreiben und Methoden ausführen können. Ein Beispiel: Eine Fertigungslinie wird auf der Grundlage der Temperatur eines Heizkessels gesteuert. Der Temperatursensor veröffentlicht eine Änderung der Daten mithilfe von OPC Publisher. Der Bediener erhält die Warnung, dass die Temperatur den Schwellenwert erreicht hat. Die Fertigungslinie wird über OPC Twin automatisch abgekühlt. Der Bediener wird über die Abkühlung informiert.

## <a name="authentication"></a>Authentication
OPC Twin ermöglicht eine einfache Authentifizierung und Entwicklung.

### <a name="simple-authentication"></a>Einfache Authentifizierung 
OPC Twin nutzt eine auf Azure Active Directory (AAD) basierende End-to-End-Authentifizierung und -Überwachung. So können Sie beispielsweise eine Anwendung auf der Grundlage von OPC Twin erstellen, um zu ermitteln, welche Aktionen ein Bediener auf einem Computer ausgeführt hat. Aufseiten des Computers wird dazu die OPC UA-Überwachung verwendet. Aufseiten der Cloud wird ein unveränderliches Clientüberwachungsprotokoll gespeichert und eine AAD-Authentifizierung über die REST-API verwendet.

### <a name="simple-developer-experience"></a>Einfache Entwicklung 
OPC Twin kann über REST-APIs mit Anwendungen in einer beliebigen Programmiersprache verwendet werden. Da Entwickler einen OPC UA-Client in eine Lösung integrieren, müssen sie nicht mit dem OPC UA SDK vertraut sein. OPC Twin lässt sich nahtlos in zustands- und serverlose Architekturen integrieren. So kann beispielsweise ein Full-Stack-Webentwickler, der eine Anwendung für ein Alarm- und Ereignisdashboard entwickelt, die Logik für die Reaktion auf Ereignisse in JavaScript oder TypeScript schreiben und dabei OPC Twin verwenden, ohne mit C, C# oder der Implementierung des vollständigen OPC UA-Stapels vertraut zu sein. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich hier über OPC Twin und entsprechende Verwendungsmöglichkeiten informiert haben, können Sie mit dem nächsten Thema fortfahren:

> [!div class="nextstepaction"]
> [OPC Twin architecture](overview-opc-twin-architecture.md) (OPC Twin-Architektur)