---
title: Integrieren von Azure Active Directory-Protokollen in Log Analytics mithilfe von Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Active Directory-Protokolle mithilfe von Azure Monitor (Vorschauversion) in Log Analytics integrieren.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 8b9189ef3fff75023316d5272bd93c136106cf94
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54824676"
---
# <a name="integrate-azure-ad-logs-with-log-analytics-using-azure-monitor-preview"></a>Integrieren von Azure AD-Protokollen in Log Analytics mithilfe von Azure Monitor (Vorschauversion)

Mit Log Analytics können Sie Daten abfragen, um bestimmte Ereignisse zu finden, Trends zu analysieren und datenquellenübergreifende Korrelationen vorzunehmen. Durch die Integration von Azure AD-Aktivitätsprotokollen in Log Analytics können Sie nun unter anderem folgende Aufgaben ausführen:

 * Vergleichen Ihrer Azure AD-Anmeldeprotokolle mit vom Azure Security Center veröffentlichten Sicherheitsprotokollen

 * Behandeln von Leistungsengpässen auf der Anmeldeseite Ihrer Anwendung durch Korrelieren von Anwendungsleistungsdaten aus Azure Application Insights  

Das folgende Video einer Ignite-Sitzung veranschaulicht die Vorteile der Verwendung von Log Analytics für Azure AD-Protokolle in praktischen Benutzerszenarien.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

In diesem Artikel erfahren Sie, wie Sie Azure AD-Protokolle (Azure Active Directory) mithilfe von Azure Monitor in Log Analytics integrieren.

## <a name="supported-reports"></a>Unterstützte Berichte

Sie können Überwachungs- und Anmeldeaktivitätsprotokolle zur weiteren Analyse an Log Analytics weiterleiten. 

* **Überwachungsprotokolle**: Mit dem [Aktivitätsbericht zu Überwachungsprotokollen](concept-audit-logs.md) erhalten Sie Zugriff auf den Verlauf aller Aufgaben, die in Ihrem Mandanten durchgeführt werden.
* **Anmeldeprotokolle**: Mit dem [Aktivitätsbericht zu Anmeldungen](concept-sign-ins.md) können Sie ermitteln, von wem die Aufgaben durchgeführt wurden, die in den Überwachungsprotokollen aufgeführt sind.

> [!NOTE]
> B2C-bezogene Aktivitätsprotokolle für Überwachungen und Anmeldungen werden derzeit nicht unterstützt.
>

## <a name="prerequisites"></a>Voraussetzungen 

Sie benötigen Folgendes, um dieses Feature verwenden zu können:

* Ein Azure-Abonnement. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/).
* Einen Azure AD-Mandanten.
* Einen Benutzer, der als *globaler Administrator* oder *Sicherheitsadministrator* für den Azure AD-Mandanten fungiert.
* Einen Log Analytics-Arbeitsbereich in Ihrem Azure-Abonnement. Informationen zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

## <a name="send-logs-to-log-analytics"></a>Senden von Protokollen an Log Analytics

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

2. Klicken Sie auf **Azure Active Directory** > **Diagnoseeinstellungen** -> **Diagnoseeinstellung hinzufügen**. Sie können auch auf der Seite **Überwachungsprotokolle** oder **Anmeldungen** auf **Exporteinstellungen** klicken, um zur Konfigurationsseite für die Diagnoseeinstellungen zu gelangen.  
    
3. Aktivieren Sie im Menü **Diagnoseeinstellungen** das Kontrollkästchen **An Log Analytics senden**, und klicken Sie anschließend auf **Konfigurieren**.

4. Wählen Sie den Log Analytics-Arbeitsbereich aus, an den Sie die Protokolle senden möchten, oder erstellen Sie einen neuen Arbeitsbereich im dafür vorgesehenen Dialogfeld.  

5. Führen Sie einen oder beide der folgenden Schritte aus:
    * Aktivieren Sie das Kontrollkästchen **AuditLogs**, um Überwachungsprotokolle an den Log Analytics-Arbeitsbereich zu senden. 
    * Aktivieren Sie das Kontrollkästchen **SignInLogs**, um Anmeldeprotokolle an den Log Analytics-Arbeitsbereich zu senden.

6. Klicken Sie auf **Speichern**, um die Einstellung zu speichern.

    ![Diagnoseeinstellungen](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Vergewissern Sie sich nach etwa 15 Minuten, dass Ereignisse an Ihren Log Analytics-Arbeitsbereich gestreamt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Analyze Azure AD activity logs with Log Analytics (preview)](howto-analyze-activity-logs-log-analytics.md) (Analysieren von Azure AD-Aktivitätsprotokollen mit Log Analytics (Vorschauversion))
* [Install and use the Log Analytics views for Azure Active Directory](howto-install-use-log-analytics-views.md) (Installieren und Verwenden der Log Analytics-Ansichten für Azure Active Directory)
