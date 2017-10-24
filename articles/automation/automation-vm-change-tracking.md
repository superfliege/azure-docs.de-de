---
title: "Nachverfolgen von Änderungen auf Ihren virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Verwenden Sie die Änderungsnachverfolgung zum Nachverfolgen von Änderungen in den Dateien und der Registrierung auf Ihren virtuellen Computern."
services: automation
documentationcenter: automation
author: eslesar
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: eslesar
ms.custom: 
ms.openlocfilehash: 5c6e8390ec8533fc7ab281c212e47a6982b30f1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="track-changes-in-your-azure-virtual-machines"></a>Nachverfolgen von Änderungen auf Ihren virtuellen Azure-Computern

Indem Sie die Änderungsnachverfolgung aktivieren, können Sie Änderungen an Dateien und Windows-Registrierungsschlüsseln auf Ihren virtuellen Computern nachverfolgen. Durch Ermitteln von Konfigurationsänderungen können Sie Betriebsprobleme präzise bestimmen.

Sie können die Änderungsnachverfolgung direkt auf Ihrem virtuellen Azure-Computer aktivieren.

Wenn Sie keinen virtuellen Azure-Computer haben, können Sie die Erstellung anhand der folgenden Anweisungen im Artikel [Windows-Schnellstart](../virtual-machines/windows/quick-create-portal.md) oder [Linux-Schnellstart](../virtual-machines/linux/quick-create-portal.md) durchführen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.

## <a name="enable-change-tracking-for-an-azure-virtual-machine"></a>Aktivieren der Änderungsnachverfolgung für einen virtuellen Azure-Computer

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Virtuelle Computer**.
2. Wählen Sie einen virtuellen Computer aus der Liste aus.
3. Wählen Sie im Fenster des virtuellen Computers unter **Vorgänge** die Option **Änderungsnachverfolgung**. 

   ![Änderungsnachverfolgung auf der VM](./media/automation-vm-change-tracking/change-onboard-vm-blade.png)  
    Das Fenster **Updateverwaltung aktivieren** wird geöffnet.

    Es wird eine Überprüfung durchgeführt, um zu ermitteln, ob die Änderungsnachverfolgung für diesen virtuellen Computer aktiviert ist. Wenn dies nicht der Fall ist, wird ein Banner angezeigt, über das Sie die Lösung aktivieren können.

   ![Integriertes Banner für Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-onboard-banner.png)

4. Wählen Sie das Banner aus, um die Lösung zu aktivieren. Falls Sie nicht über die folgenden Elemente verfügen, werden sie automatisch hinzugefügt:

   * [Log Analytics](../log-analytics/log-analytics-overview.md)-Arbeitsbereich
   * [Automation](../automation/automation-offering-get-started.md)-Konto

5. Wählen Sie einen Log Analytics-Arbeitsbereich zum Speichern der Datenprotokolle für die Änderungsnachverfolgung und ein Automation-Konto zum Nachverfolgen von Änderungen aus, und wählen Sie dann **Aktivieren**.  
    Sie werden in einer Statusleiste benachrichtigt, dass die Lösung aktiviert wird. Dieser Vorgang kann bis zu 15 Minuten dauern.

## <a name="configure-change-tracking"></a>Konfigurieren der Änderungsnachverfolgung

Nachdem die Änderungsnachverfolgung aktiviert wurde, wird das Fenster **Änderungsnachverfolgung** angezeigt. 

Um zu bestimmen, welche Dateien und Registrierungsschlüssel nachverfolgt werden sollen, wählen Sie **Einstellungen bearbeiten** aus.

   ![Ändern der Einstellungen für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-edit-settings.png)

   Das Fenster **Arbeitsbereichskonfiguration** wird geöffnet. 

Fügen Sie im Fenster **Arbeitsbereichskonfiguration** wie in den nächsten drei Abschnitten erläutert die Windows-Registrierungsschlüssel, Windows- oder Linux-Dateien hinzu, die nachverfolgt werden sollen.

### <a name="add-a-windows-registry-key"></a>Hinzufügen eines Windows-Registrierungsschlüssels

1. Wählen Sie auf der Registerkarte **Windows-Registrierung** die Option **Hinzufügen**.  
    Das Fenster **Windows-Registrierung für Änderungsnachverfolgung hinzufügen** wird geöffnet.

   ![Hinzufügen der Registrierung für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-add-registry.png)

2. Wählen Sie unter **Aktiviert** die Option **Wahr**.
3. Fügen Sie im Feld **Elementname** einen Anzeigenamen hinzu.
4. (Optional) Geben Sie im Feld **Gruppe** einen Gruppennamen ein.
5. Geben Sie im Feld **Windows-Registrierungsschlüssel** den Namen des Registrierungsschlüssels ein, den Sie nachverfolgen möchten.
6. Wählen Sie **Speichern** aus.

### <a name="add-a-windows-file"></a>Hinzufügen einer Windows-Datei

1. Wählen Sie auf der Registerkarte **Windows-Dateien** die Option **Hinzufügen**.  
    Das Fenster **Windows-Datei für Änderungsnachverfolgung hinzufügen** wird geöffnet.

   ![Hinzufügen einer Windows-Datei für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-add-win-file.png)

2. Wählen Sie unter **Aktiviert** die Option **Wahr**.
3. Fügen Sie im Feld **Elementname** einen Anzeigenamen hinzu.
4. (Optional) Geben Sie im Feld **Gruppe** einen Gruppennamen ein.
5. Geben Sie im Feld **Pfad eingeben** den vollständigen Pfad und Dateinamen der Datei ein, die nachverfolgt werden soll.
6. Wählen Sie **Speichern** aus.

### <a name="add-a-linux-file"></a>Hinzufügen einer Linux-Datei

1. Wählen Sie auf der Registerkarte **Linux-Dateien** die Option **Hinzufügen**.  
    Das Fenster **Linux-Datei für Änderungsnachverfolgung hinzufügen** wird geöffnet.

   ![Hinzufügen einer Linux-Datei für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-add-linux-file.png)

2. Wählen Sie unter **Aktiviert** die Option **Wahr**.
3. Fügen Sie im Feld **Elementname** einen Anzeigenamen hinzu.
4. (Optional) Geben Sie im Feld **Gruppe** einen Gruppennamen ein.
5. Geben Sie im Feld **Pfad eingeben** den vollständigen Pfad und Dateinamen der Datei ein, die nachverfolgt werden soll.
6. Wählen Sie im Feld **Pfadtyp** entweder die Option **Datei** oder **Verzeichnis**.
7. Wählen Sie unter **Rekursion** die Option **Ein**, um Änderungen für den angegebenen Pfad und alle Dateien und Pfade darunter nachzuverfolgen. Wählen Sie **Aus**, um nur den ausgewählten Pfad bzw. die Datei nachzuverfolgen.
8. Wählen Sie unter **Sudo verwenden** die Option `sudo`Ein**, um Dateien nachzuverfolgen, die zum Zugreifen den Befehl**  benötigen. Wählen Sie andernfalls die Option **Aus**.
9. Wählen Sie **Speichern** aus.

## <a name="view-changes"></a>Anzeigen von Änderungen

Im Fenster **Änderungsnachverfolgung** werden für Ihren virtuellen Computer in den unterschiedlichen Kategorien die Änderungen in Abhängigkeit von der Zeit angezeigt.

   ![Hinzufügen von Ansichtsänderungen für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-view-changes.png)

Sie können Kategorien und Zeitbereich auswählen, die für die Änderungen angezeigt werden sollen. Am oberen Rand des Fensters wird eine grafische Darstellung der im Laufe der Zeit auftretenden Änderungen angezeigt. Am unteren Rand des Fensters wird eine Liste mit den letzten Änderungen angezeigt.

## <a name="view-change-tracking-log-data"></a>Anzeigen von Protokolldaten für die Änderungsnachverfolgung

Bei der Änderungsnachverfolgung werden Protokolldaten generiert, die an Log Analytics gesendet werden. Um die Protokolle per Ausführung von Abfragen zu durchsuchen, klicken Sie oben im Fenster **Änderungsnachverfolgung** auf **Log Analytics**.

   ![Änderungsnachverfolgung für Log Analytics](./media/automation-vm-change-tracking/change-log-analytics.png)

Weitere Informationen zur Ausführung von Abfragen und zum Durchsuchen von Protokolldateien in Log Analytics finden Sie unter [Log Analytics](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Nachverfolgen von Änderungen erhalten Sie unter [Nachverfolgen von Änderungen an der Software in Ihrer Umgebung mit der Change Tracking-Lösung](../log-analytics/log-analytics-change-tracking.md).
* Weitere Informationen zur Verwaltung von Updates für Ihre virtuellen Computer finden Sie unter [Lösung für die Updateverwaltung in OMS](../operations-management-suite/oms-solution-update-management.md).
