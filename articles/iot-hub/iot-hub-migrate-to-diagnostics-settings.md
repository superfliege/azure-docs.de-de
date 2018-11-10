---
title: Migrieren von Azure IoT Hub zu Diagnoseeinstellungen | Microsoft-Dokumentation
description: Sie erfahren, wie Sie Azure IoT Hub zur Verwendung von Azure-Diagnoseeinstellungen anstatt zur Vorgangsüberwachung zum Überwachen des Status von Vorgängen auf Ihrem IoT Hub in Echtzeit aktualisieren.
author: kgremban
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: kgremban
ms.openlocfilehash: 3cb0f91f3143e6a4828548f3a15678b3814cba17
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154860"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Migrieren Ihres IoT Hub von der Vorgangsüberwachung zu Diagnoseeinstellungen

Kunden, die mit der [Vorgangsüberwachung](iot-hub-operations-monitoring.md) den Status von Vorgängen in IoT Hub nachverfolgen, können diesen Workflow zu [Azure-Diagnoseeinstellungen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) migrieren, einem Feature von Azure Monitor. Diagnoseeinstellungen liefern Diagnoseinformationen auf Ressourcenebene für viele Azure-Dienste.

Die Vorgangsüberwachungsfunktionalität von IoT Hub ist veraltet und wird in Zukunft entfernt. Dieser Artikel enthält die Schritte zum Verschieben Ihrer Workloads von der Vorgangsüberwachung zu Diagnoseeinstellungen. Weitere Informationen zur Veraltungszeitachse finden Sie unter [Monitor your Azure IoT solutions with Azure Monitor and Azure Resource Health](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/) (Überwachen Ihrer Azure IoT-Lösungen mit Azure Monitor und Azure Resource Health).

## <a name="update-iot-hub"></a>Aktualisieren von IoT Hub

Um Ihren IoT Hub im Azure-Portal zu aktualisieren, aktivieren Sie zuerst die Diagnoseeinstellungen und deaktivieren dann die Vorgangsüberwachung.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>Deaktivieren der Vorgangsüberwachung

Nachdem Sie die neuen Diagnoseeinstellungen in Ihrem Workflow getestet haben, können Sie das Vorgangsüberwachungsfeature deaktivieren. 

1. Wählen Sie in Ihrem IoT Hub-Menü die Option **Vorgangsüberwachung**.

2. Wählen Sie unter jeder Überwachungskategorie die Option **Keine**.

3. Speichern Sie die Änderungen der Vorgangsüberwachung.

## <a name="update-applications-that-use-operations-monitoring"></a>Aktualisieren von Anwendungen, die Vorgangsüberwachung verwenden

Die Schemata für Vorgangsüberwachung und Diagnoseeinstellungen variieren geringfügig. Es ist wichtig, dass Sie die Anwendungen, die derzeit die Vorgangsüberwachung verwenden, dahingehend aktualisieren, dass sie zu dem von den Diagnoseeinstellungen verwendeten Schema wechseln. 

Darüber hinaus bieten die Diagnoseeinstellungen die Nachverfolgung für fünf neue Kategorien. Fügen Sie nach Aktualisieren der Anwendungen für das vorhandene Schema auch die neuen Kategorien hinzu:

* Cloud-zu-Gerät-Zwillingsvorgänge
* Gerät-zu-Cloud-Zwillingsvorgänge
* Zwillingsabfragen
* Auftragsvorgänge
* Direkte Methoden

Die spezifischen Schemastrukturen finden Sie unter [Grundlegendes zu den Protokollen](iot-hub-monitor-resource-health.md#understand-the-logs).

## <a name="next-steps"></a>Nächste Schritte

* [Schnelle Überwachung der Integrität von Azure IoT Hub und Diagnose von Problemen](iot-hub-monitor-resource-health.md)