---
title: Migrieren von Azure IoT Hub zu Diagnoseeinstellungen | Microsoft-Dokumentation
description: Sie erfahren, wie Sie Azure IoT Hub zur Verwendung von Azure-Diagnoseeinstellungen anstatt zur Vorgangsüberwachung zum Überwachen des Status von Vorgängen auf Ihrem IoT Hub in Echtzeit aktualisieren.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 85bdb4b4802283c591e4d7a9e8b14ae74fa44e8d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633586"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migrieren Ihres IoT Hub von der Vorgangsüberwachung zu Diagnoseeinstellungen

Kunden, die mit der [Vorgangsüberwachung][lnk-opsmon] den Status von Vorgängen in IoT Hub nachverfolgen, können diesen Workflow zu [Azure-Diagnoseeinstellungen][lnk-diagnostics-settings] migrieren, einem Feature von Azure Monitor. Diagnoseeinstellungen liefern Diagnoseinformationen auf Ressourcenebene für viele Azure-Dienste.

Die Vorgangsüberwachungsfunktionalität von IoT Hub ist veraltet und wird in Zukunft entfernt. Dieser Artikel enthält die Schritte zum Verschieben Ihrer Workloads von der Vorgangsüberwachung zu Diagnoseeinstellungen. Weitere Informationen über die Veraltungszeitachse finden Sie unter [Monitor your Azure IoT solutions with Azure Monitor and Azure Resource Health][lnk-blog-announcement] (Überwachen Ihrer Azure IoT-Lösungen mit Azure Monitor und Azure Resource Health).

## <a name="update-iot-hub"></a>Aktualisieren von IoT Hub

Um Ihren IoT Hub im Azure-Portal zu aktualisieren, aktivieren Sie zuerst die Diagnoseeinstellungen und deaktivieren dann die Vorgangsüberwachung.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Deaktivieren der Vorgangsüberwachung

Nachdem Sie die neuen Diagnoseeinstellungen in Ihrem Workflow getestet haben, können Sie das Vorgangsüberwachungsfeature deaktivieren. 

1. Wählen Sie in Ihrem IoT Hub-Menü die Option **Vorgangsüberwachung**.
1. Wählen Sie unter jeder Überwachungskategorie die Option **Keine**.
1. Speichern Sie die Änderungen der Vorgangsüberwachung.

## <a name="update-applications-that-use-operations-monitoring"></a>Aktualisieren von Anwendungen, die Vorgangsüberwachung verwenden

Die Schemata für Vorgangsüberwachung und Diagnoseeinstellungen variieren geringfügig. Es ist wichtig, dass Sie die Anwendungen, die derzeit die Vorgangsüberwachung verwenden, dahingehend aktualisieren, dass sie zu dem von den Diagnoseeinstellungen verwendeten Schema wechseln. 

Darüber hinaus bieten die Diagnoseeinstellungen die Nachverfolgung für fünf neue Kategorien. Fügen Sie nach Aktualisieren der Anwendungen für das vorhandene Schema auch die neuen Kategorien hinzu:

- Cloud-zu-Gerät-Zwillingsvorgänge
- Gerät-zu-Cloud-Zwillingsvorgänge
- Zwillingsabfragen
- Auftragsvorgänge
- Direkte Methoden

Die spezifischen Schemastrukturen finden Sie unter [Understand the logs][lnk-diagnostics-schema] (Grundlegendes zu den Protokollen).

## <a name="next-steps"></a>Nächste Schritte

- [Schnelle Überwachung der Integrität von Azure IoT Hub und Diagnose von Problemen][lnk-monitor]

[lnk-opsmon]: iot-hub-operations-monitoring.md
[lnk-diagnostics-settings]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[lnk-diagnostics-schema]: iot-hub-monitor-resource-health.md#understand-the-logs
[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/
[lnk-monitor]: iot-hub-monitor-resource-health.md
