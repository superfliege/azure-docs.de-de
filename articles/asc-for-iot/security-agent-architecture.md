---
title: Grundlegendes zur ASC für IoT-Sicherheits-Agent-Architektur (Vorschauversion) | Microsoft-Dokumentation
description: Grundlegendes zur Sicherheits-Agent-Architektur für die im ASC für IoT-Dienst verwendeten Agents.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 7c8f256cf91a479c45f21b933efdb6a5e0212796
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541498"
---
# <a name="security-agent-reference-architecture"></a>Sicherheits-Agent-Referenzarchitektur

> [!IMPORTANT]
> ASC für IoT befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


ASC für IoT bietet eine Referenzarchitektur für Sicherheits-Agents, die Sicherheitsdaten über IoT Hub protokollieren, verarbeiten, aggregieren und senden.

Sicherheits-Agents sind für den Einsatz in einer eingeschränkten IoT-Umgebung konzipiert und können in Bezug auf die Werte, die sie im Vergleich zu den von ihnen verbrauchten Ressourcen bereitstellen, sehr flexibel angepasst werden.

Sicherheits-Agents unterstützen die folgenden IoT-Lösungsfeatures:

- Sammeln von Rohdaten zu Sicherheitsereignissen aus dem zugrunde liegenden Betriebssystem (Linux, Windows). Weitere Informationen zu verfügbaren Sicherheitsdatensammlern finden Sie unter [ASC für die IoT-Agent-Konfiguration](concept-agent-configuration.md).

- Aggregieren von Rohdaten zu Sicherheitsereignissen in Nachrichten, die über IoT Hub gesendet werden.

- Authentifizieren mit vorhandener Geräteidentität oder einer dedizierten Modulidentität. Weitere Informationen finden Sie unter [Sicherheits-Agent-Authentifizierungsmethoden](concept-security-agent-authentication-methods.md).

- Remotekonfigurieren durch Verwendung des Modulzwillings **ascforiot**. Weitere Informationen finden Sie unter [Konfigurieren eines ASC für IoT-Agents](concept-agent-configuration.md).

ASC für IoT-Sicherheits-Agents werden als Open Source-Projekte entwickelt und sind über GitHub verfügbar: 

- [IoT-ASC-Agent-C](https://github.com/Azure/IoT-ASC-Agent-C) 
- [IoT-ASC-Agent-CS](https://github.com/Azure/IoT-ASC-Agent-CS)

## <a name="agent-supported-platforms"></a>Vom Agent unterstützte Plattformen

ASC für IoT bietet verschiedene Installer-Agents für 32-Bit- und 64-Bit-Windows sowie für 32-Bit- und 64-Bit-Linux. Stellen Sie anhand der folgenden Tabelle sicher, dass Sie den richtigen Agent-Installer für jedes Ihrer Geräte verwenden:

| 32 oder 64 Bit | Linux | Windows |    Details|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32 Bit  | C  | C#  ||
| 64 Bit  | C# oder C           | C#      | Verwenden Sie den C-Agent für Geräte mit minimalen Ressourcen|

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie die ASC für IoT-Sicherheits-Agent-Architektur und die verfügbaren Installer kennen gelernt.

Um mit der ASC für IoT-Bereitstellung fortzufahren, lesen Sie die folgenden Artikel:


- Lesen Sie die ASC für IoT-[Dienstvoraussetzungen](service-prerequisites.md).
- Erfahren Sie, wie Sie [den ASC für IoT-Dienst in Ihrer IoT Hub-Instanz aktivieren](quickstart-onboard-iot-hub.md).
- Verwenden Sie den Schnellstart zum [Konfigurieren Ihrer Lösung](quickstart-configure-your-solution.md).
- Machen Sie sich mit den [Methoden der Sicherheits-Agent-Authentifizierung](concept-security-agent-authentication-methods.md) vertraut.
- Wählen Sie einen [Sicherheits-Agent](select-deploy-agent.md) aus, und stellen Sie ihn bereit.
- Unter [ASC für IoT – Häufig gestellte Fragen](resources-frequently-asked-questions.md) erfahren Sie mehr über den Dienst.