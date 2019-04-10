---
title: Was ist die OPC UA-Zertifikatverwaltung von Azure IoT? | Microsoft-Dokumentation
description: Übersicht über OPC Vault
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ba3313d927ec4317d6c051f6058d75a415b92288
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759772"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-ua-certificate-management"></a>Was ist die OPC UA-Zertifikatverwaltung (Open Platform Communications) von Azure IoT?

Die OPC UA-Zertifikatverwaltung von Azure IoT, auch als OPC Vault bezeichnet, ist ein Microservice, mit dem der Zertifikatlebenszyklus für OPC UA-Server- und -Clientanwendungen in der Cloud konfiguriert, registriert und verwaltet werden kann. In diesem Artikel werden einfache Anwendungsfälle für OPC Vault beschrieben.

## <a name="certificate-management"></a>Zertifikatverwaltung

Es kann beispielsweise sein, dass ein Fertigungsunternehmen seinen OPC UA-Servercomputer mit einer neu entwickelten Clientanwendung verbinden muss. Wenn das Fertigungsunternehmen zum ersten Mal auf den Servercomputer zugreift, wird in der OPC UA-Serveranwendung sofort eine Fehlermeldung angezeigt, um darauf hinzuweisen, dass die Clientanwendung nicht sicher ist. Dieser Mechanismus ist in den OPC UA-Servercomputer integriert, um den unberechtigten Zugriff auf die Anwendung und somit gefährliches Hacking in der Fertigung zu verhindern.

## <a name="application-security-management"></a>Verwaltung der Anwendungssicherheit
Ein Sicherheitsexperte nutzt den OPC Vault-Microservice zum einfachen Aktivieren des OPC UA-Servers für die Kommunikation mit einer beliebigen Clientanwendung, da OPC Vault über alle Funktionen für die Zertifikatregistrierung, Speicherung und Lebenszyklusverwaltung verfügt. Nachdem der OPC UA-Server nun auf sichere Weise verbunden wurde, kann er mit der neu entwickelten Clientanwendung kommunizieren.

## <a name="the-complete-opc-vault-architecture"></a>Vollständige OPC Vault-Architektur
Im folgenden Diagramm ist die gesamte OPC Vault-Architektur dargestellt.

![OPC Vault-Architektur](media/overview-opc-vault-architecture/opc-vault.png)