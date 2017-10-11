---
title: "Scheduler – hohe Verfügbarkeit und Zuverlässigkeit"
description: "Scheduler – hohe Verfügbarkeit und Zuverlässigkeit"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 7e7fe49de7814b6058468d630f8638720e5864f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="scheduler-high-availability-and-reliability"></a>Scheduler – hohe Verfügbarkeit und Zuverlässigkeit
## <a name="azure-scheduler-high-availability"></a>Azure Scheduler – hohe Verfügbarkeit
Als ein Kern Azure Platform-Dienst Azure Scheduler ist hoch verfügbar und features sowohl geografisch redundante dienstbereitstellung als auch geografisch regionale auftragsreplikation.

### <a name="geo-redundant-service-deployment"></a>Geografisch redundante dienstbereitstellung
Azure Scheduler ist über die Benutzeroberfläche in fast allen geografischen Regionen, die in Azure heutzutage ist verfügbar. Ist die Liste der Regionen, die Azure Scheduler verfügbar ist [hier aufgeführten](https://azure.microsoft.com/regions/#services). Wenn ein Rechenzentrum in einer gehosteten Region nicht verfügbar dargestellt wird, sind die Failoverfunktionen von Azure Scheduler, dass der Dienst von einem anderen Rechenzentrum verfügbar ist.

### <a name="geo-regional-job-replication"></a>Geografische regionale auftragsreplikation
Es ist nicht nur Azure Scheduler-Front-End für Management Anforderungen verfügbar, aber Ihr jeweils eigener Auftrag auch geografisch repliziert ist. Bei ein Ausfall in einer Region, wird Azure Scheduler ein Failover und stellt sicher, dass der Auftrag von einem anderen Rechenzentrum in der komplementären geografischen Region ausgeführt wird.

Wenn Sie einen Auftrag in Süd/Mitte erstellt haben, repliziert Azure Scheduler beispielsweise automatisch diesen Auftrag in NORD/Mitte. Wenn ein Fehler im Süd/Mitte vorhanden ist, stellt Azure Scheduler sicher, dass der Auftrag von NORD/Mitte ausgeführt wird. 

![][1]

Daher wird sichergestellt, dass Azure Scheduler, dass Ihre Daten innerhalb der gleichen größeren geografischen Region bei einem Azure-Ausfall bleibt. Daher müssen Sie Ihren Auftrag nur um hohe Verfügbarkeit hinzuzufügen nicht duplizieren – Azure Scheduler stellt hohe Verfügbarkeit von Funktionen für Ihre Aufträge automatisch bereit.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler-Zuverlässigkeit
Azure Scheduler garantiert seine eigene hohe Verfügbarkeit und bietet einen anderen Ansatz für benutzererstellte Aufträge. Der Auftrag kann z. B. einen HTTP-Endpunkt aufrufen, der nicht verfügbar ist. Azure Scheduler versucht dennoch, Ihren Auftrag erfolgreich ausgeführt wird, durch die Vergabe Sie alternativen für den Umgang mit einem Fehler. Dazu bietet Azure Scheduler auf zwei Arten:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurierbare Wiederholungsrichtlinie über "RetryPolicy"
Azure Scheduler können Sie eine wiederholungsrichtlinie konfigurieren. Standardmäßig, wenn ein Auftrag fehlschlägt, versucht Scheduler den Auftrag erneut vier weitere Male in Intervallen von 30 Sekunden. Sie können diese wiederholungsrichtlinie aggressiver (z. B. zehn Mal in Intervallen von 30 Sekunden) werden neu konfigurieren oder großzügiger ist (z. B. zwei Mal täglich.)

Als Beispiel für die dies nützlich ist können Sie einen Auftrag erstellen, der einmal pro Woche ausgeführt und ruft einen HTTP-Endpunkt. Wenn der HTTP-Endpunkt für ein paar Stunden Wenn Ihr Auftrag ausgeführt wird wird, können Sie nicht warten möchten eine weitere Woche für den Auftrag erneut ausführen, da auch die standardmäßige wiederholungsrichtlinie schlägt fehl. In solchen Fällen können Sie die standardmäßige wiederholungsrichtlinie (z. B.) alle drei Stunden wiederholt statt alle 30 Sekunden erneut konfigurieren.

Informationen zum Konfigurieren einer wiederholungsrichtlinie, finden Sie unter [RetryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternative Endpunktkonfigurierbarkeit über "ErrorAction"
Bleibt der Zielendpunkt für Ihren Azure Scheduler-Auftrag nicht erreichbar ist, fragt Azure Scheduler alternativen fehlerbehandlungsendpunkt nachdem seine wiederholungsrichtlinie. Wenn ein alternativer fehlerbehandlungsendpunkt konfiguriert ist, wird Sie von Azure Scheduler aufgerufen. Mit einem alternativen Endpunkt haben Ihre eigenen Aufträge Fehlerfall hoch verfügbar.

Beispielsweise können in der folgenden Abbildung folgt Azure Scheduler seiner wiederholungsrichtlinie, um einen Webdienst in New York zu erreichen. Nachdem die Wiederholungen fehlschlagen, prüft er, ob eine alternative vorhanden ist. Klicken Sie dann Azure, und startet, die an den alternativen mit derselben wiederholungsrichtlinie Anforderungen.

![][2]

Beachten Sie, dass dieselbe wiederholungsrichtlinie sowohl die ursprünglichen Aktion als auch die alternative Fehleraktion gilt. Es ist auch möglich, dass die alternative Fehleraktion Aktionstyp von Aktionstyp der Hauptaktion unterscheiden. Z. B. während der Hauptaktion einen HTTP-Endpunkt aufgerufen werden kann, die Fehleraktion stattdessen möglicherweise eine Speicherwarteschlange, Servicebus-Warteschlange oder Servicebus Thema-Aktion, die Protokollierung von Anzeigefehlern ausführt.

Weitere Informationen zum Konfigurieren eines alternativen Endpunkts finden Sie unter [ErrorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Siehe auch
 [Was ist Scheduler?](scheduler-intro.md)

 [Azure Scheduler-Konzepte,-Terminologie und-Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Gewusst wie: Erstellen von komplexen Zeitpläne und erweiterte Wiederholung mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler-REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Referenz zur Azure Scheduler-PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler – Einschränkungen, Standardwerte und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung mit Azure Scheduler](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
