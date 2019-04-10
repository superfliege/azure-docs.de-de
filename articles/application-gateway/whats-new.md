---
title: Neuerungen in Azure Application Gateway
description: Enthält Informationen zu den Neuerungen in Azure Application Gateway, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/1/2019
ms.author: victorh
ms.openlocfilehash: f686c8ac53db2d128cf5bb20f252c547348e5ac7
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58863099"
---
# <a name="whats-new-in-azure-application-gateway"></a>Neuerungen in Azure Application Gateway

Azure Application Gateway wird fortlaufend aktualisiert. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Fehlerbehebungen
- Veraltete Funktionen

## <a name="new-features"></a>Neue Funktionen

|Feature  |BESCHREIBUNG  |Hinzufügedatum  |
|---------|---------|---------|
|CRUD/Umschreibungen von Headern     |Sie können die HTTP-Header jetzt umschreiben. Eine Schritt-für-Schritt-Anleitung finden Sie im [Tutorial: Erstellen eines Application Gateways und erneutes Generieren eines HTTP-Headers](tutorial-http-header-rewrite-powershell.md).|Dezember 2018|
|WAF-Konfiguration und Ausschlussliste     |Wir haben weitere Optionen hinzugefügt, die Ihnen als Hilfe beim Konfigurieren Ihrer WAF und beim Reduzieren von falsch positiven Ergebnissen dienen. Weitere Informationen finden Sie unter [WAF-Anforderungsgrößenlimits und Ausschlusslisten](application-gateway-waf-configuration.md).|Dezember 2018|
|Automatische Skalierung, Zonenredundanz, Unterstützung von statischen VIPs (Vorschauversion)     |Mit der v2 SKU wurden viele Verbesserungen eingeführt, z. B. automatische Skalierung, verbesserte Leistung und mehr. Weitere Informationen finden Sie unter [Was ist Azure Application Gateway?](overview.md#autoscaling-public-preview).|September 2018|
|Verbindungsausgleich     |Der Verbindungsausgleich ermöglicht Ihnen das korrekte Entfernen von Mitgliedern aus Ihren Back-End-Pools. Weitere Informationen finden Sie unter [Verbindungsausgleich](overview.md#connection-draining).|September 2018|
|Benutzerdefinierte Fehlerseiten     |Mit benutzerdefinierten Fehlerseiten können Sie eine Fehlerseite im Format Ihrer restlichen Websites erstellen. Informationen zur Aktivierung finden Sie unter [Erstellen von benutzerdefinierten Application Gateway-Fehlerseiten](custom-error.md).|September 2018|
|Verbesserungen für Metriken     |Dank verbesserter Metriken erhalten Sie einen besseren Einblick in Ihren Application Gateway-Zustand. Informationen zur Aktivierung von Metriken für Application Gateway finden Sie unter [Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](application-gateway-diagnostics.md).|Juni 2018|

## <a name="known-issues"></a>Bekannte Probleme

- [Bekannte Probleme in der v2 SKU](application-gateway-autoscaling-zone-redundant.md#known-issues-and-limitations)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Application Gateway finden Sie unter [Was ist Azure Application Gateway?](overview.md).