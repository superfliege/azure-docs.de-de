---
title: Azure Front Door Service – Sicherheit für die Anwendungsschicht | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Azure Front Door Service das Schützen und Sichern Ihrer Anwendungs-Back-Ends ermöglicht.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e5714f60b7fdd790f3af8e31250c41038110fd08
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47046156"
---
# <a name="application-layer-security-with-front-door"></a>Sicherheit für die Anwendungsschicht mit Front Door
Azure Front Door Service bietet Funktionen für den Schutz von Webanwendungen, um Ihre Webanwendungen vor Netzwerkangriffen und verbreiteten Sicherheitsrisiken und Exploits im Internet wie Einschleusung von SQL-Befehlen oder Cross-Site Scripting (XSS) zu schützen. Die Sicherheit für die Anwendungsschicht mit Front Door ist für HTTP(S)-Front-Ends aktiviert, global verteilt und immer aktiv. Sie beendet böswillige Angriffe im Randbereich des Azure-Netzwerks – weit entfernt von Ihren Back-Ends. Durch Erhöhung der Sicherheit und Leistungsoptimierungen ermöglicht Front Door schnelle und sichere Weberlebnisse für Ihre Endbenutzer.

## <a name="application-protection"></a>Anwendungsschutz
Der Anwendungsschutz von Front Door ist weltweit in jeder Edge-Umgebung in Übereinstimmung mit Anwendungen konfiguriert und blockiert Nicht-HTTP(S)-Datenverkehr automatisch, sodass er Ihre Webanwendungen nicht erreicht. Unsere verteilte Architektur mit mehreren Mandanten ermöglicht umfangreichen globalen Schutz ohne Leistungseinbußen. Für HTTP(S)-Workloads bietet der Schutzdienst für Webanwendungen von Front Door ein umfassendes Regelmodul für benutzerdefinierte Regeln, einen vorkonfigurierten Regelsatz für verbreitete Angriffe und eine ausführliche Protokollierung für alle Anforderungen, die mit einer Regel übereinstimmen. Flexible Aktionen wie „Zulassen“, „Blockieren“ oder „Nur Protokollieren“ werden unterstützt.

## <a name="custom-access-control-rules"></a>Benutzerdefinierte Regeln für die Zugriffssteuerung
- **IP-Blacklists und -Whitelists:** Sie können benutzerdefinierte Regeln zum Steuern des Zugriffs auf Ihre Webanwendungen basierend auf einer Liste von Client-IP-Adressen konfigurieren. IPv4 und IPv6 werden unterstützt.
- **Geografische Zugriffssteuerung:** Sie können benutzerdefinierte Regeln zum Steuern des Zugriffs auf Ihre Webanwendungen basierend auf dem Ländercode, zu dem eine Client-IP-Adresse gehört, konfigurieren.
- **Filtern von HTTP-Parametern:** Sie können benutzerdefinierte Zugriffsregeln basierend auf übereinstimmenden HTTP(S)-Anforderungsparametern, einschließlich URL, Header und Abfragezeichenfolgen, konfigurieren.

## <a name="azure-managed-rules"></a>Von Azure verwaltete Regeln
- Ein vorkonfigurierter Satz von Regeln für die wichtigsten OWASP-Sicherheitsrisiken ist standardmäßig aktiviert. In der Vorschauversion umfasst der Satz von Regeln die Überprüfung von sqli- und xss-Anforderungen. Weitere Regeln werden hinzugefügt. Sie können auch mit der Aktion „Nur Protokollieren“ beginnen, um zu überprüfen, ob die vorkonfigurierten Regeln wie erwartet für Ihre Anwendungen funktionieren. 

## <a name="rate-limiting"></a>Ratenbegrenzung
- Eine Regel für die Ratensteuerung dient dazu, ungewöhnlich umfangreichen Datenverkehr von beliebigen Client-IP-Adressen zu beschränken.  Sie können einen Schwellenwert für die Anzahl von Webanforderungen festlegen, die von einer Client-IP-Adresse während einer Dauer von einer Minute zulässig sind.

## <a name="centralized-protection-policy"></a>Zentralisierte Schutzrichtlinie
- Sie können mehrere Schutzregeln definieren und sie einer Richtlinie in der Reihenfolge ihrer Priorität hinzufügen. Benutzerdefinierte Regeln haben höheren Priorität als der verwaltete Regelsatz, um Ausnahmen zu ermöglichen. Eine einzige Richtlinie wird Ihrer Webanwendung zugeordnet.  Identische Schutzrichtlinien für Webanwendungen werden auf allen Edgeservern an allen Standorten repliziert, um konsistente Sicherheitsrichtlinien in allen Regionen sicherzustellen.

## <a name="configuration"></a>Konfiguration
- In der Vorschauversion können Sie REST-APIs, PowerShell oder CLI verwenden, um Anwendungschutzregeln und -richtlinien zu erstellen und bereitzustellen. Portalzugriff wird unterstützt, bevor der Dienst allgemein verfügbar ist. 


## <a name="monitoring"></a>Überwachung
Front Door bietet die Möglichkeit zur Überwachung von Webanwendungen auf Angriffe. Dazu werden Echtzeitmetriken herangezogen, die in Azure Monitor integriert sind, um Warnungen nachzuverfolgen und Trends leicht zu überwachen.

## <a name="pricing"></a>Preise
Die Sicherheit für die Anwendungsschicht von Front Door ist in der Vorschauversion kostenlos.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
