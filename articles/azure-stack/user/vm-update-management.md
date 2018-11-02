---
title: Update und Verwaltung virtueller Computer mit Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Lösungen für Updateverwaltung, Änderungs- und Bestandsnachverfolgung in Azure Automation zur Verwaltung von virtuellen Windows-Computern verwenden, die in Azure Stack bereitgestellt werden.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: rtiberiu
ms.openlocfilehash: 1ef20dc35b069c5f12c2f31d0979949be27271e0
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323632"
---
# <a name="azure-stack-vm-update-and-management"></a>Update und Verwaltung virtueller Computer mit Azure Stack
Sie können die folgenden Features der Azure Automation-Lösung verwenden, um virtuelle Windows-Computer zu verwalten, die mit Azure Stack bereitgestellt werden:

- **[Updateverwaltung](https://docs.microsoft.com/azure/automation/automation-update-management)**. Mit der Updateverwaltungslösung können Sie den Status der verfügbaren Updates auf allen Agent-Computern schnell auswerten und den Prozess der Installation der erforderlichen Updates für diese virtuellen Windows-Computer verwalten.

- **[Änderungsnachverfolgung](https://docs.microsoft.com/azure/automation/automation-change-tracking)**. Änderungen an installierter Software, an Windows-Diensten, an der Windows-Registrierung und an Windows-Dateien auf den überwachten Servern werden zur Verarbeitung an den Log Analytics-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Mithilfe der Informationen im Change Tracking-Dashboard können Sie ganz leicht die Änderungen erkennen, die in Ihrer Serverinfrastruktur vorgenommen wurden.

- **[Bestand](https://docs.microsoft.com/azure/automation/automation-vm-inventory)**. Die Bestandsnachverfolgung für einen virtuellen Azure Stack Windows-Computer bietet eine browserbasierte Benutzeroberfläche zum Einrichten und Konfigurieren der Bestandssammlung. 

> [!IMPORTANT]
> Diese Lösungen sind identisch mit den Lösungen, die zum Verwalten von virtuellen Azure-Computern verwendet werden. Sowohl virtuelle Azure- als auch virtuelle Azure Stack Windows-Computer werden auf die gleiche Weise, über die gleiche Benutzeroberfläche und mit den gleichen Tools verwaltet. Für virtuelle Azure Stack-Computer gelten die gleichen Preise wie für virtuelle Azure-Computer bei Verwendung der Updateverwaltungs-, Änderungs- und Bestandnachverfolgungslösungen mit Azure Stack.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie diese Features zum Aktualisieren und Verwalten von virtuellen Azure Stack Windows-Computern verwenden können, müssen mehrere Voraussetzungen erfüllt sein. Dazu gehören Schritte, die im Azure-Portal und im Azure Stack-Verwaltungsportal ausgeführt werden müssen.

### <a name="in-the-azure-portal"></a>Im Azure-Portal
Um die Azure Automation-Funktionen für die Bestands- und Änderungsnachverfolgung sowie die Updateverwaltung für virtuelle Azure Stack Windows-Computer zu nutzen, müssen Sie diese Lösungen zunächst in Azure aktivieren.

> [!TIP]
> Wenn Sie diese Features bereits für virtuelle Azure-Computer aktiviert haben, können Sie die bereits vorhandenen Anmeldeinformationen für den LogAnalytics-Arbeitsbereich verwenden. Wenn Sie bereits über eine LogAnalytics-WorkspaceID und einen Primärschlüssel verfügen, die bzw. den Sie verwenden möchten, fahren Sie [mit dem nächsten Abschnitt fort](.\vm-update-management.md#in-the-azure-stack-administration-portal). Andernfalls fahren Sie mit diesem Abschnitt fort, um einen neuen LogAnalytics-Arbeitsbereich und ein Automation-Konto zu erstellen.

Der erste Schritt zur Aktivierung dieser Lösungen besteht darin, [einen LogAnalytics-Arbeitsbereich](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace) in Ihrem Azure-Abonnement zu erstellen. Ein Log Analytics-Arbeitsbereich ist eine eigene Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen. Nachdem Sie einen Arbeitsbereich erstellt haben, notieren Sie sich die WorkspaceID und den Schlüssel. Um diese Informationen anzuzeigen, wechseln Sie zum Blatt „Arbeitsbereich“, klicken Sie auf **Erweiterte Einstellungen**, und überprüfen Sie dann die Werte für die **Arbeitsbereich-ID** und den **Primärschlüssel**. 

Im nächsten Schritt müssen Sie ein [Automation-Konto erstellen](https://docs.microsoft.com/azure/automation/automation-create-standalone-account). Ein Automation-Konto ist ein Container für Ihre Azure Automation-Ressourcen. Es bietet eine Möglichkeit, Ihre Umgebungen zu trennen oder Ihre Automation-Workflows und -Ressourcen besser zu organisieren. Nachdem das Automation-Konto erstellt wurde, müssen Sie das Bestands- und Änderungsnachverfolgungs- sowie das Updateverwaltungsfeature aktivieren. Führen Sie dazu die folgenden Schritte aus, um die einzelnen Features zu aktivieren:

1. Navigieren Sie im Azure-Portal zu dem Automation-Konto, das Sie verwenden möchten.

2. Wählen Sie die zu aktivierende Lösung aus (**Bestand**, **Änderungsnachverfolgung** oder **Updateverwaltung**).

3. Verwenden Sie die Dropdownliste **Arbeitsbereich auswählen**, um den zu verwendenden Log Analytics-Arbeitsbereich auszuwählen.

4. Überprüfen Sie, ob alle übrigen Informationen richtig sind, und klicken Sie dann auf **Aktivieren**, um die Lösung zu aktivieren.

5. Wiederholen Sie die Schritte 2 bis 4, um alle drei Lösungen zu aktivieren. 

   [![](media/vm-update-management/1-sm.PNG "Aktivieren der Features des Automation-Kontos")](media/vm-update-management/1-lg.PNG#lightbox)

### <a name="in-the-azure-stack-administration-portal"></a>Im Azure Stack-Verwaltungsportal
Nachdem Sie die Azure Automation-Lösungen im Azure-Portal aktiviert haben, müssen Sie sich als nächstes als Cloudadministrator im Azure Stack-Verwaltungsportal anmelden und die Erweiterung **Azure Update- und Konfigurationsverwaltung** aus Azure Stack Marketplace herunterladen. 

   ![Marketplace-Erweiterung „Azure Update- und Konfigurationsverwaltung“](media/vm-update-management/2.PNG) 

## <a name="enable-update-management-for-azure-stack-virtual-machines"></a>Aktivieren der Updateverwaltung für virtuelle Azure Stack-Computer
Führen Sie die folgenden Schritte aus, um die Updateverwaltung für virtuelle Azure Stack Windows-Computer zu aktivieren.

1. Melden Sie sich am Azure Stack-Benutzerportal an.

2. Navigieren Sie im Azure Stack-Benutzerportal zum Blatt „Erweiterungen“ der virtuellen Windows-Computer, für die Sie diese Lösungen aktivieren möchten, klicken Sie auf **+ Hinzufügen**, wählen Sie die Erweiterung **Azure Update- und Konfigurationsverwaltung** aus, und klicken Sie auf **Erstellen**:

   [![](media/vm-update-management/3-sm.PNG "Blatt „Erweiterungen“ für virtuelle Windows-Computer")](media/vm-update-management/3-lg.PNG#lightbox)

3. Geben Sie die zuvor erstellte WorkspaceID und den Primärschlüssel ein, um den Agent mit dem LogAnalytics-Arbeitsbereich zu verknüpfen, und klicken Sie auf **OK**, um die Erweiterung bereitzustellen.

   [![](media/vm-update-management/4-sm.PNG "Bereitstellen der WorkspaceID und des Schlüssels")](media/vm-update-management/4-lg.PNG#lightbox) 

4. Wie in der [Dokumentation zur Automation-Updateverwaltung](https://docs.microsoft.com/azure/automation/automation-update-management) beschrieben, müssen Sie die Updateverwaltungslösung für jeden virtuellen Computer aktivieren, den Sie verwalten möchten. Um die Lösung für alle virtuellen Computer zu aktivieren, die den Arbeitsbereich verwenden, wählen Sie **Updateverwaltung** aus, klicken Sie auf **Computer verwalten**, und wählen Sie dann die Option **Auf allen verfügbaren und zukünftigen Computern aktivieren** aus.

   [![](media/vm-update-management/5-sm.PNG "Bereitstellen der WorkspaceID und des Schlüssels")](media/vm-update-management/5-lg.PNG#lightbox) 

   > [!TIP]
   > Wiederholen Sie diesen Schritt, um jede Lösung für die virtuellen Azure Stack Windows-Computer zu aktivieren, die den Arbeitsbereich verwenden. 
  
Nachdem die Erweiterung „Azure Update- und Konfigurationsverwaltung“ aktiviert wurde, wird zwei Mal täglich ein Scan für jeden verwalteten virtuellen Windows-Computer durchgeführt. Die Windows-API wird alle 15 Minuten aufgerufen, um die letzte Aktualisierungszeit abzufragen und festzustellen, ob sich der Status geändert hat. Wenn sich der Status geändert hat, wird eine Konformitätsprüfung eingeleitet.

Nachdem die virtuellen Computer überprüft wurden, werden sie im Azure Automation-Konto in der Updateverwaltungslösung angezeigt: 

   [![](media/vm-update-management/6-sm.PNG "Bereitstellen der WorkspaceID und des Schlüssels")](media/vm-update-management/6-lg.PNG#lightbox) 

> [!IMPORTANT]
> Es kann zwischen 30 Minuten und sechs Stunden dauern, bis im Dashboard aktualisierte Daten von verwalteten Computern angezeigt werden.

Die virtuellen Azure Stack Windows-Computer können nun zusammen mit den virtuellen Azure-Computern in geplante Updatebereitstellungen integriert werden.

## <a name="enable-update-management-using-a-resource-manager-template"></a>Aktivieren der Updateverwaltung mithilfe einer Resource Manager-Vorlage
Wenn Sie eine große Anzahl von virtuellen Azure Stack Windows-Computern verwenden, können Sie [diese Azure Resource Manager-Vorlage](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) verwenden, um die Lösung einfacher auf virtuellen Windows-Computern bereitzustellen. Die Vorlage stellt die Microsoft Monitoring Agent-Erweiterung auf einem vorhandenen virtuellen Windows-Computer bereit und fügt sie einem vorhandenen Azure LogAnalytics-Arbeitsbereich hinzu.
 
## <a name="next-steps"></a>Nächste Schritte
[Optimierung der Leistung von SQL Server](azure-stack-sql-server-vm-considerations.md)
