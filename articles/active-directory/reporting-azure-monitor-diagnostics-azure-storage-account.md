---
title: 'Tutorial: Archivieren von Azure Active Directory-Protokollen in einem Azure-Speicherkonto (Vorschauversion) | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Azure-Diagnose einrichten, um Azure Active Directory-Protokolle per Pushvorgang in ein Speicherkonto zu übertragen (Vorschauversion).
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
ms.openlocfilehash: ea26da91e2a0b88e8afa54a4210b7a2365e4949b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240243"
---
# <a name="tutorial-archive-azure-active-directory-logs-to-an-azure-storage-account-preview"></a>Tutorial: Archivieren von Azure Active Directory-Protokollen in einem Azure-Speicherkonto (Vorschauversion)

In diesem Tutorial wird beschrieben, wie Sie Azure Monitor-Diagnoseeinstellungen einrichten, um Azure Active Directory-Protokolle an ein Azure-Speicherkonto zu leiten.

## <a name="prerequisites"></a>Voraussetzungen 

Erforderlich:

* Ein Azure-Abonnement mit einem Azure-Speicherkonto. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/).
* Ein Azure Active Directory-Mandant.
* Ein Benutzer, der als globaler Administrator oder Sicherheitsadministrator für den Mandanten fungiert.

## <a name="archive-logs-to-an-azure-storage-account"></a>Archivieren von Protokollen in einem Azure-Speicherkonto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Klicken Sie auf **Azure Active Directory** -> **Aktivität** -> **Überwachungsprotokolle**. 
3. Klicken Sie auf **Einstellungen exportieren**, um das Blatt mit den Diagnoseeinstellungen zu öffnen. Klicken Sie auf **Einstellung bearbeiten**, wenn Sie vorhandene Einstellungen ändern möchten, oder auf **Diagnoseeinstellung hinzufügen**, um eine neue hinzuzufügen. Sie können maximal drei Einstellungen verwenden. 
    ![Einstellungen exportieren](./media/reporting-azure-monitor-diagnostics-azure-storage-account/ExportSettings.png "Einstellungen exportieren")

4. Fügen Sie einen Anzeigenamen für die Einstellung hinzu, damit eine Erinnerung an den Zweck der Einstellung vorhanden ist. Beispiel: „An Azure-Speicherkonto senden“. 
5. Aktivieren Sie das Kontrollkästchen **In ein Speicherkonto archivieren**, und klicken Sie auf **Speicherkonto**, um das Azure-Speicherkonto auszuwählen. 
6. Wählen Sie ein Azure-Abonnement und ein Speicherkonto aus, an das Sie die Protokolle weiterleiten möchten, und klicken Sie auf **OK**, um die Konfiguration zu schließen.
7. Aktivieren Sie das Kontrollkästchen **AuditLogs**, um Überwachungsprotokolle an das Speicherkonto zu senden. 
8. Aktivieren Sie das Kontrollkästchen **SignInLogs**, um Anmeldeprotokolle an das Speicherkonto zu senden.
9. Verwenden Sie den Schieberegler, um für Ihre Protokolldaten die Aufbewahrungsdauer festzulegen. Standardmäßig ist dieser Wert auf „0“ festgelegt, und die Protokolle werden unter dem Speicherkonto unendlich lange aufbewahrt. Andernfalls können Sie einen Wert festlegen, damit die Ereignisse, die älter als die Anzahl von ausgewählten Tagen sind, automatisch bereinigt werden.
10. Klicken Sie auf **Speichern**, um die Einstellung zu speichern.
    ![Diagnoseeinstellungen](./media/reporting-azure-monitor-diagnostics-azure-storage-account/DiagnosticSettings.png "Diagnoseeinstellungen")

11. Nach ca. 15 Minuten sollten Sie überprüfen, ob die Protokolle per Pushvorgang in Ihr Speicherkonto übertragen werden. Navigieren Sie zum Azure-Portal, klicken Sie auf **Speicherkonten**, wählen Sie das Speicherkonto aus, das Sie zuvor verwendet haben, und klicken Sie auf **Blobs**. 
12. Klicken Sie für **Überwachungsprotokolle** auf **insights-log-audit**. Klicken Sie für **Anmeldeprotokolle** auf **insights-logs-signin**.
    ![Speicherkonto](./media/reporting-azure-monitor-diagnostics-azure-storage-account/StorageAccount.png "Speicherkonto")

## <a name="next-steps"></a>Nächste Schritte

* [Interpretieren des Überwachungsprotokollschemas in Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretieren des Anmeldeprotokollschemas in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Häufig gestellte Fragen und bekannte Probleme](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)