---
title: "Nachverfolgen von Änderungen auf Ihren virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Verwenden Sie die Änderungsnachverfolgung zum Nachverfolgen von Änderungen in Dateien und der Registrierung auf Ihren virtuellen Computern."
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: ba92390b684573818ec9d69fd8c7be6d064316c7
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="track-changes-in-your-azure-virtual-machines"></a>Nachverfolgen von Änderungen auf Ihren virtuellen Azure-Computern

Indem Sie die Änderungsnachverfolgung aktivieren, können Sie Änderungen an Dateien und Windows-Registrierungsschlüsseln auf Ihren virtuellen Computern nachverfolgen. . Durch Ermitteln von Konfigurationsänderungen können Sie Betriebsprobleme präzise bestimmen.

Sie können die Änderungsnachverfolgung direkt auf Ihrem virtuellen Azure-Computer aktivieren.

Wenn Sie keinen virtuellen Azure-Computer haben, können Sie die Erstellung anhand der Informationen im [Windows-Schnellstart](../virtual-machines/windows/quick-create-portal.md) oder [Linux-Schnellstart](../virtual-machines/linux/quick-create-portal.md) durchführen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.

## <a name="enable-change-tracking-for-an-azure-virtual-machine"></a>Aktivieren der Änderungsnachverfolgung für einen virtuellen Azure-Computer

1. Wählen Sie auf der linken Seite des Bildschirms die Option **Virtuelle Computer**.
1. Wählen Sie in der Liste einen virtuellen Computer aus.
1. Klicken Sie auf dem Bildschirm des virtuellen Computers im Abschnitt **Vorgänge** auf **Änderungsnachverfolgung**. Der Bildschirm **Updateverwaltung aktivieren** wird geöffnet.

   ![Änderungsnachverfolgung auf der VM](./media/automation-vm-change-tracking/change-onboard-vm-blade.png)

Es wird eine Überprüfung durchgeführt, um zu ermitteln, ob die Änderungsnachverfolgung für diesen virtuellen Computer aktiviert ist. Wenn dies nicht der Fall ist, wird ein Banner angezeigt, über das Sie die Lösung aktivieren können.

   ![Integriertes Banner für Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-onboard-banner.png)

Klicken Sie auf das Banner, um die Lösung zu aktivieren. Falls Sie nicht über die folgenden Komponenten verfügen, werden diese automatisch hinzugefügt:

* [Log Analytics](../log-analytics/log-analytics-overview.md)-Arbeitsbereich
* [Automation](../automation/automation-offering-get-started.md)-Konto

Wählen Sie einen Log Analytics-Arbeitsbereich zum Speichern der Datenprotokolle für die Änderungsnachverfolgung und ein Automation-Konto zum Nachverfolgen von Änderungen aus, und klicken Sie auf **Aktivieren**.

Sie werden über eine Statusleiste benachrichtigt, dass die Lösung aktiviert wird. Dieser Vorgang kann bis zu 15 Minuten dauern.

## <a name="configure-change-tracking"></a>Konfigurieren der Änderungsnachverfolgung

Nachdem die Änderungsnachverfolgung aktiviert wurde, wird der Bildschirm **Änderungsnachverfolgung** angezeigt. Wählen Sie aus, welche Dateien und Registrierungsschlüssel nachverfolgt werden sollen, indem Sie auf **Einstellungen bearbeiten** klicken.

![Ändern der Einstellungen für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-edit-settings.png)

Der Bildschirm **Arbeitsbereichskonfiguration** wird geöffnet. Klicken Sie auf der entsprechenden Registerkarte auf **Hinzufügen**, um Windows-Registrierungsschlüssel, Windows-Dateien oder Linux-Dateien hinzuzufügen, die nachverfolgt werden sollen.

## <a name="add-a-windows-registry-key"></a>Hinzufügen eines Windows-Registrierungsschlüssels

1. Klicken Sie auf dem Bildschirm **Arbeitsbereichskonfiguration** auf der Registerkarte **Windows-Registrierung** auf **Hinzufügen**. Der Bildschirm **Windows-Registrierung für Änderungsnachverfolgung hinzufügen** wird geöffnet.

   ![Hinzufügen der Registrierung für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-add-registry.png)

1. Wählen Sie unter **Aktiviert** die Option **Wahr**.
1. Fügen Sie im Feld **Elementname** einen Anzeigenamen hinzu.
1. Geben Sie im Feld **Gruppe** einen Gruppennamen ein (optional).
1. Fügen Sie im Feld **Windows-Registrierungsschlüssel** den Namen des Registrierungsschlüssels hinzu, den Sie nachverfolgen möchten.
1. Klicken Sie auf **Speichern**.

## <a name="add-a-windows-file"></a>Hinzufügen einer Windows-Datei

1. Klicken Sie auf dem Bildschirm **Arbeitsbereichskonfiguration** auf der Registerkarte **Windows-Dateien** auf **Hinzufügen**. Der Bildschirm **Windows-Datei für Änderungsnachverfolgung hinzufügen** wird geöffnet.

   ![Hinzufügen einer Windows-Datei für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-add-win-file.png)

1. Wählen Sie unter **Aktiviert** die Option **Wahr**.
1. Fügen Sie im Feld **Elementname** einen Anzeigenamen hinzu.
1. Geben Sie im Feld **Gruppe** einen Gruppennamen ein (optional).
1. Fügen Sie im Feld **Pfad eingeben** den vollständigen Pfad und Dateinamen der Datei ein, die nachverfolgt werden soll.
1. Klicken Sie auf **Speichern**.

## <a name="add-a-linux-file"></a>Hinzufügen einer Linux-Datei

1. Klicken Sie auf dem Bildschirm **Arbeitsbereichskonfiguration** auf der Registerkarte **Linux-Dateien** auf **Hinzufügen**. Der Bildschirm **Linux-Datei für Änderungsnachverfolgung hinzufügen** wird geöffnet.

   ![Hinzufügen einer Linux-Datei für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-add-linux-file.png)

1. Wählen Sie unter **Aktiviert** die Option **Wahr**.
1. Fügen Sie im Feld **Elementname** einen Anzeigenamen hinzu.
1. Geben Sie im Feld **Gruppe** einen Gruppennamen ein (optional).
1. Fügen Sie im Feld **Pfad eingeben** den vollständigen Pfad und Dateinamen der Datei ein, die nachverfolgt werden soll.
1. Wählen Sie im Feld **Pfadtyp** entweder die Option **Datei** oder **Verzeichnis**.
1. Wählen Sie unter **Rekursion** die Option **Ein**, um Änderungen für den angegebenen Pfad und alle Dateien und Pfade darunter nachzuverfolgen. Wählen Sie **Aus**, um nur den ausgewählten Pfad bzw. die Datei nachzuverfolgen.
1. Wählen Sie unter **Sudo** die Option **Ein**, um Dateien nachzuverfolgen, für die zum Zugreifen der Befehl `sudo` erforderlich ist. Wählen Sie andernfalls die Option **Aus**.
1. Klicken Sie auf **Speichern**.

## <a name="view-changes"></a>Anzeigen von Änderungen

Auf dem Bildschirm **Änderungsnachverfolgung** werden für Ihren virtuellen Computer in den unterschiedlichen Kategorien die Änderungen in Abhängigkeit der Zeit angezeigt.

   ![Hinzufügen von Ansichtsänderungen für die Änderungsnachverfolgung](./media/automation-vm-change-tracking/change-view-changes.png)

Sie können die Kategorien und den Zeitbereich auswählen, die für die Änderungen angezeigt werden sollen. Am oberen Bildschirmrand sind die Änderungen in Abhängigkeit der Zeit grafisch dargestellt.
Am unteren Bildschirmrand befindet sich eine Liste mit den letzten Änderungen.

## <a name="view-change-tracking-log-data"></a>Anzeigen von Protokolldaten für die Änderungsnachverfolgung

Bei der Änderungsnachverfolgung werden Protokolldaten generiert, die an Log Analytics gesendet werden. Um die Protokolle per Ausführung von Abfragen zu durchsuchen, klicken Sie oben auf dem Bildschirm **Änderungsnachverfolgung** auf **Log Analytics**.

   ![Änderungsnachverfolgung für Log Analytics](./media/automation-vm-change-tracking/change-log-analytics.png)

Weitere Informationen zur Ausführung von Abfragen zum Durchsuchen von Protokolldateien finden Sie unter [Log Analytics](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Änderungsnachverfolgung finden Sie unter [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md).
* Weitere Informationen zur Verwaltung von Updates für Ihre virtuellen Computer finden Sie unter [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md).

