---
title: Überwachen der Azure-Dateisynchronisierung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Azure-Dateisynchronisierung überwachen.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 01/31/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 5a0d02768b0fbd23e33d13c5e5c3fe84a41cdc52
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243653"
---
# <a name="monitor-azure-file-sync"></a>Überwachen der Azure-Dateisynchronisierung

Mit der Azure-Dateisynchronisierung können Sie die Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit der Azure-Dateisynchronisierung werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen, z.B. SMB, NFS und FTPS. Sie können weltweit so viele Caches wie nötig nutzen.

In diesem Artikel wird beschrieben, wie Sie Ihre Bereitstellung der Azure-Dateisynchronisierung mit dem Azure-Portal und Windows Server überwachen.

Aktuell stehen die folgenden Überwachungsoptionen zur Verfügung:

## <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal können Sie die Integrität registrierter Server und Serverendpunkte (Synchronisierungsintegrität) sowie Metriken anzeigen.

### <a name="storage-sync-service"></a>Speichersynchronisierungsdienst

Um die Integrität registrierter Server, die Integrität der Serverendpunkte und Metriken anzuzeigen, wechseln Sie im Azure-Portal zum Speichersynchronisierungsdienst. Die Integrität registrierter Server kann auf dem Blatt „Registrierte Server“ angezeigt werden. Die Integrität für Serverendpunkte kann auf dem Blatt „Synchronisierungsgruppen“ angezeigt werden.

Integrität registrierter Server
- Wenn sich ein registrierter Server im Zustand „Online“ befindet, kommuniziert er erfolgreich mit dem Dienst.
- Wenn der Zustand für einen registrierten Server „Offline“ lautet, überprüfen Sie, ob der Überwachungsprozess für die Speichersynchronisierung (AzureStorageSyncMonitor.exe) auf dem Server ausgeführt wird. Wenn sich der Server hinter einer Firewall oder einem Proxy befindet, konfigurieren Sie die Firewall und den Proxy gemäß [Dokumentation](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy).

Integrität der Serverendpunkte
- Die Integrität der Serverendpunkte im Portal basiert auf den Synchronisierungsereignissen, die im Protokoll für Telemetrieereignisse auf dem Server protokolliert werden (ID 9102 und 9302). Wenn eine Synchronisierungssitzung aufgrund eines vorübergehenden Fehlers (z.B. Abbruchfehler) nicht erfolgreich ist, wird die Synchronisierung im Portal möglicherweise weiterhin als fehlerfrei angezeigt, solange ein Fortschritt bei der Synchronisierungssitzung verzeichnet wird (anhand von Ereignis-ID 9302 wird bestimmt, ob Dateien angewendet werden). Weitere Informationen finden Sie in der folgenden Dokumentation: [Integrität der Synchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [Fortschritt der Synchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Wenn das Portal einen Synchronisierungsfehler aufgrund eines fehlenden Fortschritts anzeigt, lesen Sie die [Dokumentation zur Problembehandlung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors).

Metriken
- Die folgenden Metriken können im Speichersynchronisierungsdienst-Portal angezeigt werden:

  | Metrikname | BESCHREIBUNG | Portal-Blatt/-Blätter | 
  |-|-|-|
  | Bytes synchronisiert | Größe der übertragenen Daten (Upload und Download) | Synchronisierungsgruppe, Serverendpunkt |
  | Cloudtieringrückruf | Größe der zurückgerufenen Daten | Registrierte Server |
  | Dateien ohne Synchronisierung | Anzahl der Dateien, die nicht synchronisiert werden können | Serverendpunkt |
  | Dateien synchronisiert | Anzahl der übertragenen Dateien (Upload und Download) | Synchronisierungsgruppe, Serverendpunkt |
  | Onlinestatus des Servers | Anzahl der Taktsignale, die vom Server empfangen wurden | Registrierte Server |

- Weitere Informationen finden Sie im Abschnitt [Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor). 

  > [!Note]  
  > Die Diagramme im Speichersynchronisierungsdienst-Portal haben einen Zeitbereich von 24 Stunden. Die verschiedenen Zeitbereiche oder -dimensionen können Sie mit Azure Monitor anzeigen.


### <a name="azure-monitor"></a>Azure Monitor

Verwenden Sie [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview), um Synchronisierung, Cloudtiering und Serverkonnektivität zu überwachen. Metriken für die Azure-Dateisynchronisierung werden standardmäßig aktiviert und alle 15 Minuten an Azure Monitor gesendet.

Um Metriken der Azure-Dateisynchronisierung in Azure Monitor anzuzeigen, wählen Sie den Ressourcentyp „Speichersynchronisierungsdienste“ aus.

In Azure Monitor sind die folgenden Metriken für die Azure-Dateisynchronisierung verfügbar:

| Metrikname | BESCHREIBUNG |
|-|-|
| Bytes synchronisiert | Größe der übertragenen Daten (Upload und Download).<br><br>Einheit: Byte<br>Aggregationstyp: Summe<br>Verfügbare Dimensionen: Name des Serverendpunkts, Synchronisierungsrichtung, Name der Synchronisierungsgruppe |
| Cloudtieringrückruf | Größe der zurückgerufenen Daten.<br><br>Einheit: Byte<br>Aggregationstyp: Summe<br>Verfügbare Dimension: Servername |
| Dateien ohne Synchronisierung | Anzahl von Dateien, für die keine Synchronisierung möglich ist.<br><br>Einheit: Count<br>Aggregationstyp: Summe<br>Verfügbare Dimensionen: Name des Serverendpunkts, Synchronisierungsrichtung, Name der Synchronisierungsgruppe |
| Dateien synchronisiert | Anzahl der übertragenen Dateien (Upload und Download)<br><br>Einheit: Count<br>Aggregationstyp: Summe<br>Verfügbare Dimensionen: Name des Serverendpunkts, Synchronisierungsrichtung, Name der Synchronisierungsgruppe |
| Onlinestatus des Servers | Anzahl von Taktsignalen, die vom Server empfangen wurden.<br><br>Einheit: Count<br>Aggregationstyp: Maximum<br>Verfügbare Dimension: Servername |
| Ergebnis der Synchronisierungssitzung | Ergebnis der Synchronisierungssitzung (1 = erfolgreiche Synchronisierungssitzung, 0 = fehlerhafte Synchronisierungssitzung)<br><br>Einheit: Count<br>Aggregationstypen: Maximum<br>Verfügbare Dimensionen: Name des Serverendpunkts, Synchronisierungsrichtung, Name der Synchronisierungsgruppe |

## <a name="windows-server"></a>Windows Server

Unter Windows Server können Sie die Integrität für Cloudtiering, registrierte Server und Synchronisierung anzeigen.

### <a name="event-logs"></a>Ereignisprotokolle

Verwenden Sie das Telemetrieereignisprotokoll auf dem Server, um die Integrität für registrierte Server, Synchronisierung und Cloudtiering zu überwachen. Überprüfen Sie in der Ereignisanzeige das Telemetrieereignisprotokoll unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“.

Synchronisierungsintegrität
- Nach Abschluss einer Synchronisierungssitzung wird ein Ereignis mit der ID 9102 protokolliert. Anhand dieses Ereignisses können Sie bestimmen, ob Synchronisierungssitzungen erfolgreich abgeschlossen wurden (HResult = 0) und ob Synchronisierungsfehler auf Elementebene vorliegen. Weitere Informationen finden Sie in der folgenden Dokumentation: [Integrität der Synchronisierung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [Fehler nach Element](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

  > [!Note]  
  > Gelegentlich tritt bei allen Synchronisierungssitzungen ein Fehler auf, oder PerItemErrorCount ist ungleich NULL, obwohl trotzdem ein Fortschritt angezeigt wird und einige Dateien dabei erfolgreich synchronisiert werden. Dies kann bei den Feldern beobachtet werden, die mit „Applied“ beginnen (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount und AppliedSizeBytes). Die Werte dieser Felder geben an, bis zu welchem Anteil die Sitzung zum jeweiligen Zeitpunkt erfolgreich durchgeführt wurde. Wenn für mehrere aufeinanderfolgende Synchronisierungssitzungen Fehler angezeigt werden, die Sitzungen jedoch steigende Applied-Werte aufweisen, sollten Sie der Synchronisierung Zeit für einen erneuten Versuch geben, bevor Sie ein Supportticket öffnen.

- Die Ereignis-ID 9302 wird alle 5 bis 10 Minuten protokolliert, wenn eine aktive Synchronisierungssitzung vorliegt. Anhand dieses Ereignisses sollte bestimmt werden, ob für die aktuelle Synchronisierungssitzung ein Fortschritt verzeichnet wird (AppliedItemCount > 0). Wenn die Synchronisierung keinen Fortschritt zeigt, sollte für die Synchronisierungssitzung irgendwann ein Fehler auftreten, und es wird die Ereignis-ID 9102 mit dem Fehler protokolliert. Weitere Informationen finden Sie in der folgenden Dokumentation: [Synchronisierungsfortschritt](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)

Integrität registrierter Server
- Die Ereignis-ID 9301 wird alle 30 Sekunden protokolliert, wenn ein Server Aufträge im Dienst abfragt. Wenn GetNextJob mit dem Status = 0 abgeschlossen wird, ist eine Kommunikation mit dem Dienst möglich. Wenn GetNextJob mit einem Fehler abgeschlossen wird, überprüfen Sie die [Dokumentation zur Problembehandlung](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors), um weitere Informationen zu erhalten.

Cloudtieringintegrität
- Um die Tieringaktivität auf einem Server zu überwachen, verwenden Sie die Ereignis-IDs 9003, 9016 und 9029 im Telemetrieereignisprotokoll (in der Ereignisanzeige unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“).

  - Die Ereignis-ID 9003 ermöglicht die Fehlerverteilung für einen Serverendpunkt. Beispiele: Gesamtfehlerzahl, ErrorCode usw. Beachten Sie, dass ein Ereignis pro Fehlercode protokolliert wird.
  - Die Ereignis-ID 9016 stellt Ghostingergebnisse für ein Volume bereit. Beispiele: Freier Speicherplatz in Prozent, Anzahl der Dateien in der Sitzung, für die ein Ghosting durchgeführt wurde, Anzahl von Dateien, bei denen beim Ghosting ein Fehler aufgetreten ist usw.
  - Die Ereignis-ID 9029 bietet Informationen zu Ghostingsitzungen für einen Serverendpunkt. Beispiele: Anzahl der in der Sitzung herangezogenen Dateien, Anzahl der Dateien in der Sitzung, für die ein Tiering durchgeführt wurde, Anzahl der Dateien, für die bereits ein Tiering durchgeführt ist usw.
  
- Um die Rückrufaktivität auf einem Server zu überwachen, verwenden Sie die Ereignis-IDs 9005, 9006, 9009 und 9059 im Telemetrieereignisprotokoll (in der Ereignisanzeige unter „Anwendungen und Dienste\Microsoft\FileSync\Agent“).

  - Die Ereignis-ID 9005 bietet Zuverlässigkeit beim Rückruf für einen Serverendpunkt. Beispiele: Gesamtanzahl eindeutiger Dateien, auf die zugegriffen wird, und Gesamtanzahl eindeutiger Dateien, bei denen beim Zugriff ein Fehler aufgetreten ist.
  - Die Ereignis-ID 9006 ermöglicht die Rückruffehlerverteilung für einen Serverendpunkt. Beispiele: Fehlerhafte Anforderungen insgesamt, ErrorCode usw. Beachten Sie, dass ein Ereignis pro Fehlercode protokolliert wird.
  - Die Ereignis-ID 9009 bietet Informationen zu Rückrufsitzungen für einen Serverendpunkt. Hierzu zählen beispielsweise DurationSeconds, CountFilesRecallSucceeded, CountFilesRecallFailed usw.
  - Die Ereignis-ID 9059 bietet Informationen zur Anwendungsrückrufverteilung für einen Serverendpunkt. Hierzu zählen beispielsweise ShareId, Anwendungsname und TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Leistungsindikatoren

Verwenden Sie die Leistungsindikatoren der Azure-Dateisynchronisierung auf dem Server, um die Synchronisierungsaktivität zu überwachen.

Um die Leistungsindikatoren der Azure-Dateisynchronisierung auf dem Server zu überwachen, starten Sie den Systemmonitor (Perfmon.exe). Die Leistungsindikatoren befinden sich unter den Objekten „Übertragene AFS-Bytes“ und „AFS-Synchronisierungsvorgänge“.

Im Systemmonitor stehen die folgenden Leistungsindikatoren für die Azure-Dateisynchronisierung zur Verfügung:

| Name des Leistungsobjekts\Leistungsindikators | BESCHREIBUNG |
|-|-|
| Übertragene AFS-Bytes\Heruntergeladene Bytes/s | Die Anzahl von pro Sekunde heruntergeladenen Bytes. |
| Übertragene AFS-Bytes\Hochgeladene Bytes/s | Die Anzahl von pro Sekunde hochgeladenen Bytes. |
| Übertragene AFS-Bytes\Bytes gesamt/s | Die Gesamtanzahl von Bytes pro Sekunde (Upload und Download). |
| AFS-Synchronisierungsvorgänge\Heruntergeladene Synchronisierungsdateien/s | Die Anzahl von pro Sekunde heruntergeladenen Synchronisierungsdateien. |
| AFS-Synchronisierungsvorgänge\Hochgeladene Synchronisierungsdateien/s | Die Anzahl von pro Sekunde hochgeladenen Synchronisierungsdateien. |
| AFS-Synchronisierungsvorgänge\Synchronisierungsdateivorgänge gesamt/s | Die Gesamtanzahl von synchronisierten Dateien (Upload und Download). |

## <a name="next-steps"></a>Nächste Schritte
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
- [Berücksichtigen von Firewall- und Proxyeinstellungen](storage-sync-files-firewall-and-proxy.md)
- [Bereitstellen der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md)
- [Problembehandlung für die Azure-Dateisynchronisierung](storage-sync-files-troubleshoot.md)
- [Häufig gestellte Fragen zu Azure Files](storage-files-faq.md)
