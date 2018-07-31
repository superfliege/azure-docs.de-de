---
title: 'Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub (Vorschauversion) | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Azure-Diagnose einrichten, um Azure Active Directory-Protokolle per Pushvorgang in einen Event Hub zu übertragen (Vorschauversion).
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c4f5bf1b49d7f59b2bb938a7ddc53b96c1d354ef
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240217"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub-preview"></a>Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie Azure Monitor-Diagnoseeinstellungen einrichten, um Azure Active Directory-Protokolle an einen Azure Event Hub zu leiten. Verwenden Sie diesen Mechanismus, um Ihre Protokolle in SIEM-Tools von Drittanbietern zu integrieren, z.B. Splunk und QRadar.

## <a name="prerequisites"></a>Voraussetzungen 

Erforderlich:

* Ein Azure-Abonnement. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/).
* Azure Active Directory-Mandant
* Ein Benutzer, der als globaler Administrator oder Sicherheitsadministrator für den Mandanten fungiert.
* Ein Event Hubs-Namespace und ein Event Hub in Ihrem Azure-Abonnement. [Hier ist die Erstellung beschrieben.](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md)

## <a name="archive-logs-to-event-hub"></a>Archivieren von Protokollen in einem Event Hub

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Klicken Sie auf **Azure Active Directory** -> **Aktivität** -> **Überwachungsprotokolle**. 
3. Klicken Sie auf **Einstellungen exportieren**, um das Blatt mit den Diagnoseeinstellungen zu öffnen. Klicken Sie auf **Einstellung bearbeiten**, wenn Sie vorhandene Einstellungen ändern möchten, oder auf **Diagnoseeinstellung hinzufügen**, um eine neue hinzuzufügen. Sie können maximal drei Einstellungen verwenden. 
    ![Einstellungen exportieren](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png "Einstellungen exportieren")

4. Aktivieren Sie das Kontrollkästchen **An einen Event Hub streamen**, und klicken Sie auf **Event Hub/Konfigurieren**.
5. Wählen Sie ein Azure-Abonnement und einen Event Hubs-Namespace als Ziel für die Weiterleitung der Protokolle aus. Das Abonnement und der Event Hubs-Namespace müssen jeweils dem Active Directory-Mandanten zugeordnet sein, von dem die Protokolle gestreamt werden. Sie können einen Event Hub auch in dem Event Hubs-Namespace angeben, an den Protokolle gesendet werden sollen. Wenn kein Event Hub angegeben ist, wird im Namespace ein Event Hub mit dem Standardnamen **insights-logs-audit** erstellt.
6. Klicken Sie auf **OK**, um die Event Hub-Konfiguration zu beenden.
7. Aktivieren Sie das Kontrollkästchen **AuditLogs**, um Überwachungsprotokolle an das Speicherkonto zu senden. 
8. Aktivieren Sie das Kontrollkästchen **SignInLogs**, um Anmeldeprotokolle an das Speicherkonto zu senden.
9. Klicken Sie auf **Speichern**, um die Einstellung zu speichern.
    ![Diagnoseeinstellungen](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png "Diagnoseeinstellungen")

10. Nach ca. 15 Minuten sollten Sie überprüfen, ob die Ereignisse in Ihrem Event Hub angezeigt werden. Navigieren Sie hierzu über das Portal zum Event Hub, und vergewissern Sie sich, dass die Anzahl von **eingehenden Nachrichten** größer als null ist. 
    ![Überwachungsprotokolle](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png "Überwachungsprotokolle")


## <a name="access-data-from-event-hubs"></a>Zugreifen auf Daten von Event Hubs

Nachdem Daten im Event Hub angezeigt wurden, können Sie auf zwei Arten darauf zugreifen.

* **Konfigurieren eines unterstützten SIEM-Tools zum Lesen der Daten**: Die meisten Tools benötigen die Event Hub-Verbindungszeichenfolge und bestimmte Berechtigungen für Ihr Azure-Abonnement zum Lesen von Daten aus dem Event Hub. Im Folgenden wird eine nicht erschöpfende Liste von Tools mit der Azure Monitor-Integration aufgeführt:
    1. **Splunk**: Weitere Informationen zur Integration von Azure AD-Protokollen in Splunk finden Sie unter [How to integrate Azure Active Directory logs with Splunk using Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md) (Integrieren von Azure Active Directory-Protokollen in Splunk mit Azure Monitor)
    
    2. **IBM QRadar**: Das DSM und Event Hub-Protokoll von Microsoft Azure sind zum Download auf der [Website des IBM-Supports](http://www.ibm.com/support) erhältlich. Weitere Informationen zur Integration in Azure finden Sie [hier](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    3. **SumoLogic**: Befolgen Sie [diese Anleitung](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub), um SumoLogic zum Nutzen der Daten einer Event Hub-Instanz einzurichten. 

* **Einrichten von benutzerdefinierten Tools zum Lesen der Daten**: Wenn Ihr derzeitiges SIEM-Tool in der Azure Monitor-Diagnose noch nicht unterstützt wird, können Sie mit den Event Hub-APIs benutzerdefinierte Tools einrichten. Weitere Informationen finden Sie im Artikel zu [Event Hub-APIs](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Nächste Schritte

* [Integrate Azure Active Directory logs with Splunk using Azure Monitor Diagnostics](reporting-azure-monitor-diagnostics-splunk-integration.md) (Integrieren von Azure Active Directory-Protokollen in Splunk mit der Azure Monitor-Diagnose)
* [Interpret audit logs schema in Azure monitor diagnostics](reporting-azure-monitor-diagnostics-audit-log-schema.md) (Interpretieren des Überwachungsprotokollschemas in der Azure Monitor-Diagnose)
* [Interpret sign-in logs schema in Azure monitor diagnostics](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) (Interpretieren des Anmeldeprotokollschemas in der Azure Monitor-Diagnose)