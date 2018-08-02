---
title: Integrieren von Azure Active Directory-Protokollen in Splunk mit Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Active Directory-Protokolle in Splunk mit Azure Monitor (Vorschauversion) integrieren.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d1dd62d06c7e3ed634795604ce9660694ea073ca
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240449"
---
# <a name="integrate-azure-active-directory-logs-with-splunk-using-azure-monitor-preview"></a>Integrieren von Azure Active Directory-Protokollen in Splunk mit Azure Monitor (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie Azure Active Directory-Protokolle in Splunk mit Azure Monitor integrieren. Leiten Sie die Protokolle zunächst an Azure Event Hub weiter, und integrieren Sie sie dann in Splunk.

## <a name="prerequisites"></a>Voraussetzungen

1. Azure Event Hub mit Azure AD-Aktivitätsprotokollen. [Dieser Artikel erläutert, wie Sie Aktivitätsprotokolle an Event Hub streamen.](reporting-azure-monitor-diagnostics-azure-event-hub.md) 
2. Verwenden Sie [diese Anleitung](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md), um das Azure Monitor-Add-On für Splunk herunterzuladen und Ihre Splunk-Instanz zu konfigurieren.

## <a name="tutorial"></a>Tutorial 

1. Öffnen Sie Ihre Splunk-Instanz, und klicken Sie auf **Data Summary** (Datenzusammenfassung).
    ![Data Summary] (./media/reporting-azure-monitor-diagnostics-splunk-integration/DataSummary.png "(Datenzusammenfassung)")

2. Navigieren Sie zur Registerkarte **Sourcetypes** (Quelltypen), und wählen Sie **amal: aadal:audit** aus. ![Registerkarte „Sourcetypes“] (./media/reporting-azure-monitor-diagnostics-splunk-integration/sourcetypeaadal.png "Registerkarte „Quelltypen“")

3. Daraufhin werden die Azure AD-Aktivitätsprotokolle wie in der folgenden Abbildung angezeigt.
    ![Activity logs] (./media/reporting-azure-monitor-diagnostics-splunk-integration/activitylogs.png "Aktivitätsprotokolle")

> [!NOTE]
> Falls Sie kein Add-On in Ihrer Splunk-Instanz installieren können, z. B. bei Verwendung eines Proxys oder bei Ausführung in Splunk Cloud, können Sie diese Ereignisse an die HTTP-Ereignissammlung von Splunk weiterleiten. Verwenden Sie dazu [diese Azure-Funktion, die durch neue Nachrichten in Event Hub ausgelöst wird](https://github.com/Microsoft/AzureFunctionforSplunkVS). 
>

## <a name="next-steps"></a>Nächste Schritte

* [Interpretieren des Überwachungsprotokollschemas in Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretieren des Anmeldeprotokollschemas in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Häufig gestellte Fragen und bekannte Probleme](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)