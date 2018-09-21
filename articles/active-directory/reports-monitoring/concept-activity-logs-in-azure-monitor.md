---
title: Azure Active Directory-Aktivitätsprotokolle in Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Übersicht über Azure Active Directory-Aktivitätsprotokolle in Azure Monitor (Vorschauversion)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: concept
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 0c59cac666dec00e02f21ba20c0608b6b8142ba4
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45740645"
---
# <a name="azure-ad-activity-logs-in-azure-monitor-preview"></a>Azure AD-Aktivitätsprotokolle in Azure Monitor (Vorschauversion)

Sie können Azure Active Directory (Azure AD)-Aktivitätsprotokolle jetzt mithilfe von Azure Monitor an Ihr eigenes Speicherkonto oder Ihren Event Hub weiterleiten. Mit der öffentlichen Vorschauversion von Azure Active Directory-Protokollen in Azure Monitor haben Sie die folgenden Möglichkeiten:

* Archivieren Ihrer Überwachungsprotokolle für ein Azure-Speicherkonto, damit Sie die Daten über längere Zeiträume aufbewahren können.
* Streamen Ihrer Überwachungsprotokolle zum Analysieren an einen Azure Event Hub mithilfe beliebter Security Information & Event Management (SIEM)-Tools wie etwa Splunk und QRadar.
* Integrieren Ihrer Überwachungsprotokolle in Ihre eigenen benutzerdefinierten Protokolllösungen, indem Sie sie an einen Event Hub streamen.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Unterstützte Berichte

Mit diesem Feature können Sie Aktivitätsprotokolle zu Überwachungen und Anmeldungen an Ihr Azure-Speicherkonto, einen Event Hub oder eine benutzerdefinierte Lösung weiterleiten. 

* **Überwachungsprotokolle**: Mit dem [Aktivitätsbericht zu Überwachungsprotokollen](concept-audit-logs.md) erhalten Sie Zugriff auf den Verlauf aller Aufgaben, die in Ihrem Mandanten durchgeführt werden.
* **Anmeldeprotokolle**: Mit dem [Aktivitätsbericht zu Anmeldungen](concept-sign-ins.md) können Sie ermitteln, von wem die Aufgaben durchgeführt wurden, die in den Überwachungsprotokollen aufgeführt sind.

> [!NOTE]
> B2C-bezogene Aktivitätsprotokolle für Überwachungen und Anmeldungen werden derzeit nicht unterstützt.
>

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um dieses Feature verwenden zu können:

* Ein Azure-Abonnement. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/).
* Eine Azure AD-[Lizenz](https://azure.microsoft.com/pricing/details/active-directory/) vom Typ „Free“, „Basic“, „Premium 1“ oder „Premium 2“, um auf die Azure AD-Überwachungsprotokolle im Azure-Portal zuzugreifen. 
* Eine Azure AD-[Lizenz](https://azure.microsoft.com/pricing/details/active-directory/) vom Typ „Premium 1“ oder „Premium 2“, um auf die Azure AD-Anmeldeprotokolle im Azure-Portal zuzugreifen. 

Je nachdem, wohin die Überwachungsprotokolldaten weitergeleitet werden, benötigen Sie Folgendes:

* Ein Azure-Speicherkonto, für das Sie über *ListKeys*-Berechtigungen verfügen. Wir empfehlen Ihnen, anstelle eines Blobspeicherkontos ein allgemeines Speicherkonto zu verwenden. Preisinformationen zur Speicherung erhalten Sie über den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Einen Azure Event Hubs-Namespace, um die Integration in Drittanbieterlösungen zu ermöglichen.

> [!NOTE]
> Sie müssen im Azure AD-Mandanten ein *globaler Administrator* oder *Sicherheitsadministrator* sein, um auf die Azure AD-Aktivitätsprotokolle zugreifen zu können.
>

## <a name="cost-considerations"></a>Kostenbetrachtung

Falls Sie bereits über eine Azure AD-Lizenz verfügen, benötigen Sie ein Azure-Abonnement, um das Speicherkonto und den Event Hub einzurichten. Für das Azure-Abonnement fallen keine Kosten an, aber Sie müssen für die Nutzung der Azure-Ressourcen zahlen, z.B. das Speicherkonto, das Sie für die Archivierung verwenden, und den Event Hub, den Sie zum Streamen nutzen. Die Datenmenge und somit auch die anfallenden Kosten variieren je nach Mandantengröße erheblich. 

### <a name="storage-size-for-activity-logs"></a>Speichergröße für Aktivitätsprotokolle

Für jedes Überwachungsprotokollereignis werden ca. 2 KB an Datenspeicher verbraucht. Für einen Mandanten mit 100.000 Benutzern, für den pro Tag ca. 1,5 Millionen Ereignisse anfallen, benötigen Sie also ungefähr 3 GB an Datenspeicher pro Tag. Da Schreibvorgänge in etwa 5-Minuten-Batches erfolgen, können Sie mit ca. 9.000 Schreibvorgängen pro Monat rechnen. 

Die folgende Tabelle enthält eine Schätzung der Kosten in Abhängigkeit von der Mandantengröße für ein universelles Speicherkonto (v2) in der Region „USA, Westen“ und einer Aufbewahrungsdauer von mindestens einem Jahr. Verwenden Sie den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/details/storage/blobs/), um eine genauere Schätzung für die Datenmenge zu erstellen, mit der Sie für Ihre Anwendung rechnen. 

| Protokollkategorie | Anzahl an Benutzern | Ereignisse pro Tag | Datenmenge pro Monat (Schätzung) | Kosten pro Monat (Schätzung) | Kosten pro Jahr (Schätzung) |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Audit | 100.000 | 1,5&nbsp;Millionen | 90 GB | 1,93 $ | 23,12 $ |
| Audit | 1.000 | 15.000 | 900 MB | 0,02 $ | 0,24 $ |
| Anmeldungen | 1.000 | 34.800 | 4 GB | 0,13 $ | 1,56 $ |
| Anmeldungen | 100.000 | 15&nbsp;Millionen | 1,7 TB | 35,41 $ | 424,92 $ | 


### <a name="event-hub-messages-for-activity-logs"></a>Event Hub-Nachrichten für Aktivitätsprotokolle

Ereignisse werden zu Batchintervallen mit einer Länge von ca. fünf Minuten zusammengefasst und als einzelne Nachricht gesendet, die alle Ereignisse innerhalb dieses Zeitraums enthält. Eine Nachricht im Event Hub hat eine maximale Größe von 256 KB. Wenn die Gesamtgröße aller Nachrichten innerhalb des Zeitraums diese Menge übersteigt, werden mehrere Nachrichten gesendet. 

Für einen großen Mandanten mit mehr als 100.000 Benutzern treten pro Sekunde beispielsweise im Schnitt 18 Ereignisse auf. Dies entspricht einer Rate von 5.400 Ereignissen alle fünf Minuten. Da Überwachungsprotokolle ca. 2 KB pro Ereignis umfassen, entspricht dies 10,8 MB an Daten. Daher werden im Intervall von fünf Minuten 43 Nachrichten an den Event Hub gesendet. 

Die folgende Tabelle enthält die ungefähren monatlichen Kosten für einen einfachen Event Hub in „USA, Westen“ in Abhängigkeit der Ereignisdatenmenge. Verwenden Sie den [Event Hubs-Preisrechner](https://azure.microsoft.com/pricing/details/event-hubs/), um eine genaue Schätzung für die Datenmenge zu berechnen, die für Ihre Anwendung zu erwarten ist.

| Protokollkategorie | Anzahl an Benutzern | Ereignisse pro Sekunde | Ereignisse pro Fünf-Minuten-Intervall | Menge pro Intervall | Nachrichten pro Intervall | Nachrichten pro Monat | Kosten pro Monat (Schätzung) |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Audit | 100.000 | 18 | 5.400 | 10,8 MB | 43 | 371.520 | 10,83 $ |
| Audit | 1.000 | 0,1 | 52 | 104 KB | 1 | 8.640 | 10,80 $ |
| Anmeldungen | 1.000 | 178 | 53.400 | 106,8&nbsp;MB | 418 | 3.611.520 | 11,06 $ |  


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Dieser Abschnitt beantwortet häufig gestellte Fragen und bespricht bekannte Probleme zu Azure AD-Protokollen in Azure Monitor.

**F: Wo soll ich beginnen?** 

**A**: In diesem Artikel wird erläutert, was Sie für die Bereitstellung dieses Features benötigen. Wenn die Voraussetzungen erfüllt sind, erhalten Sie in den Tutorials weitere Informationen zum Konfigurieren und Weiterleiten Ihrer Protokolle an einen Event Hub.

---

**F: Welche Protokolle sind enthalten?**

**A:** Sowohl Anmelde- als auch Überwachungsprotokolle sind für das Weiterleiten über dieses Feature verfügbar, aber B2C-bezogene Überwachungsereignisse sind derzeit noch nicht enthalten. Lesen Sie die Informationen zum [Überwachungsprotokollschema](reference-azure-monitor-audit-log-schema.md) und [Anmeldeprotokollschema](reference-azure-monitor-sign-ins-log-schema.md), um zu ermitteln, welche Arten von Protokollen und featurebasierten Protokolle derzeit unterstützt werden. 

---

**F: Wie lange nach einer Aktion werden die entsprechenden Protokolle in Event Hubs angezeigt?**

**A**: Die Protokolle werden innerhalb von zwei bis fünf Minuten nachdem die Aktion ausgeführt wurde, in Ihrem Event Hub angezeigt. Weitere Informationen zu Event Hubs finden Sie unter [Was ist Azure Event Hubs?](../../event-hubs/event-hubs-about.md).

---

**F: Wie lange nach einer Aktion werden die entsprechenden Protokolle in Speicherkonten angezeigt?**

**A:** Für Azure-Speicherkonten beträgt die Latenz zwischen 5 und 15 Minuten nach Durchführung der Aktion.

---

**F: Was kostet das Speichern meiner Daten?**

**A:** Die Speicherkosten richten sich nach der Größe Ihrer Protokolle und nach der gewählten Aufbewahrungsdauer. Eine Liste mit geschätzten Kosten pro Mandant, die von der Menge der erstellten Protokolle abhängen, finden Sie im Abschnitt [Speichergröße für Aktivitätsprotokolle](#storage-size-for-activity-logs).

---

**F: Was kostet es, meine Daten an Event Hubs zu streamen?**

**A:** Die Streamingkosten richten sich nach der Anzahl von Nachrichten, die Sie pro Minute empfangen. In diesem Artikel wird erläutert, wie die Kosten berechnet werden. Außerdem umfasst er Kostenschätzungen ausgehend von der Anzahl von Nachrichten. 

---

**F: Wie integriere ich Azure AD-Aktivitätsprotokolle in mein SIEM-System?**

**A:** Hierzu stehen zwei Möglichkeiten zur Verfügung:

- Verwenden Sie Azure Monitor mit Event Hubs zum Streamen von Protokollen an Ihr SIEM-System. [Streamen Sie die Protokolle zunächst an einen Event Hub](tutorial-azure-monitor-stream-logs-to-event-hub.md), und [richten Sie anschließend Ihr SIEM-Tool mit dem konfigurierten Event Hub ein](tutorial-azure-monitor-stream-logs-to-event-hub.md#access-data-from-your-event-hub). 

- Greifen Sie mit der [Reporting-Graph-API](concept-reporting-api.md) auf die Daten zu, und übertragen Sie diese dann mit Ihren eigenen Skripts in Ihr SIEM-System.

---

**F: Welche SIEM-Tools werden derzeit unterstützt?** 

**A:** Derzeit wird Azure Monitor von [Splunk](tutorial-integrate-activity-logs-with-splunk.md), QRadar und [Sumo Logic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory) unterstützt. Weitere Informationen zur Funktionsweise von Connectors finden Sie unter [Streamen von Azure-Überwachungsdaten an einen Event Hub für die Verwendung durch ein externes Tool](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

---

**F: Wie integriere ich Azure AD-Aktivitätsprotokolle in meine Splunk-Instanz?**

**A:** [Leiten Sie die Azure AD-Aktivitätsprotokolle zunächst an einen Event Hub weiter](quickstart-azure-monitor-stream-logs-to-event-hub.md), und führen Sie dann die Schritte zum [Integrieren von Aktivitätsprotokollen in Splunk](tutorial-integrate-activity-logs-with-splunk.md) aus.

---

**F: Wie integriere ich Azure AD-Aktivitätsprotokolle in Sumo Logic?** 

**A:** [Leiten Sie die Azure AD-Aktivitätsprotokolle zunächst an einen Event Hub weiter](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory), und führen Sie dann die Schritte zum [Installieren der Azure AD-Anwendung und Anzeigen der Dashboards in SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) aus.

---

**F: Kann ich über Event Hub auf die Daten zugreifen, ohne ein externes SIEM-Tool zu verwenden?** 

**A:** Ja. Sie können die [Event Hubs-API](../../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) nutzen, um über Ihre benutzerdefinierte Anwendung auf die Protokolle zuzugreifen. 

---


## <a name="next-steps"></a>Nächste Schritte

* [Archivieren von Aktivitätsprotokollen in einem Speicherkonto](quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Weiterleiten von Aktivitätsprotokollen an Event Hub](quickstart-azure-monitor-stream-logs-to-event-hub.md)
* [Weitere Informationen zu Azure-Diagnoseprotokollen](../../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
