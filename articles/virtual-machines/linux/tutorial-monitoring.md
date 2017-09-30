---
title: "Überwachen und Aktualisieren virtueller Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie auf einem virtuellen Linux-Computer in Azure Startdiagnose und Leistungsmetriken überwachen und Paketupdates verwalten."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 70c17d9a8f7bf6d9106efcb56eee7cd996460c18
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Überwachen und Aktualisieren eines virtuellen Linux-Computers in Azure

Um sicherzustellen, dass die virtuellen Computer (VMs) in Azure ordnungsgemäß ausgeführt werden, können Sie Startdiagnose und Leistungsmetriken überprüfen sowie Paketupdates verwalten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren der Startdiagnose auf dem virtuellen Computer
> * Anzeigen der Startdiagnose
> * Anzeigen von Hostmetriken
> * Aktivieren der Diagnoseerweiterung auf dem virtuellen Computer
> * Anzeigen von Metriken des virtuellen Computers
> * Erstellen von Benachrichtigungen basierend auf Diagnosemetriken
> * Verwalten von Paketupdates
> * Einrichten der erweiterten Überwachung


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-vm"></a>Erstellen eines virtuellen Computers

Um die Diagnose und die Metriken in Aktion anzuzeigen, benötigen Sie einen virtuellen Computer. Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt die Ressourcengruppe *myResourceGroupMonitor* am Standort *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Jetzt können Sie mit [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) einen virtuellen Computer erstellen. Im folgenden Beispiel wird ein virtueller Computer namens *myVM* erstellt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Aktivieren der Startdiagnose

Wenn virtuelle Linux-Computer starten, erfasst die Startdiagnoseerweiterung die Startausgabe und speichert sie in Azure Storage. Diese Daten können zum Beheben von Startproblemen bei virtuellen Computern verwendet werden. Die Startdiagnose wird nicht automatisch aktiviert, wenn Sie eine Linux-VM mithilfe der Azure-Befehlszeilenschnittstelle erstellen.

Vor dem Aktivieren der Startdiagnose muss ein Speicherkonto zum Speichern der Startprotokolle erstellt werden. Speicherkonten benötigen einen global eindeutigen Namen, der zwischen 3 und 24 Zeichen lang sein muss und nur Ziffern und Kleinbuchstaben enthalten darf. Sie erstellen ein Speicherkonto mit dem Befehl [az storage account create](/cli/azure/storage/account#create). In diesem Beispiel wird eine zufällige Zeichenfolge verwendet, um einen eindeutigen Speicherkontonamen zu erstellen. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Bei der Aktivierung der Startdiagnose wird der URI zum Blob Storage-Container benötigt. Der folgende Befehl fragt das Speicherkonto ab, um diesen URI zurückzugeben. Der URI-Wert wird in der Variable *bloburi* gespeichert, die im nächsten Schritt verwendet wird.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Aktivieren Sie nun die Startdiagnose mit [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). Der `--storage`-Wert ist der Blob-URI, der im vorherigen Schritt erfasst wurde.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Anzeigen der Startdiagnose

Wenn die Startdiagnose aktiviert ist, werden bei jedem Beenden und Starten des virtuellen Computers Informationen zum Startvorgang in eine Protokolldatei geschrieben. Heben Sie in diesem Beispiel zunächst die Zuordnung des virtuellen Computers mit dem Befehl [az vm deallocate](/cli/azure/vm#deallocate) wie folgt auf:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Starten Sie nun den virtuellen Computer mit dem Befehl [az vm start]( /cli/azure/vm#stop) wie folgt:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Sie erhalten die Startdiagnosedaten für *myVM* mithilfe des Befehls [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) wie folgt:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Anzeigen von Hostmetriken

Eine Linux-VM verfügt über einen dedizierten Host in Azure, mit dem sie interagiert. Es werden automatisch Metriken für den Host gesammelt, die folgendermaßen im Azure-Portal angezeigt werden können:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroupMonitor** und dann in der Ressourcenliste **myVM** aus.
1. Um die Leistung der Host-VM anzuzeigen, klicken Sie dem Blatt der VM auf **Metriken**, und wählen Sie dann eine der *[Host]*-Metriken unter **Verfügbare Metriken** aus.

    ![Anzeigen von Hostmetriken](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Installieren der Diagnoseerweiterung

> [!IMPORTANT]
> Dieses Dokument beschreibt Version 2.3 der Linux-Diagnoseerweiterung, die veraltet ist. Version 2.3 wird bis zum 30. Juni 2018 unterstützt.
>
> Stattdessen kann Version 3.0 der Linux-Diagnoseerweiterung aktiviert werden. Weitere Informationen finden Sie in der [Dokumentation](./diagnostic-extension.md).

Die grundlegenden Hostmetriken sind direkt verfügbar. Wenn Sie aber detailliertere und VM-spezifische Metriken anzeigen möchten, müssen Sie die Azure-Diagnose-Erweiterung auf dem virtuellen Computer installieren. Die Azure-Diagnose-Erweiterung ermöglicht das Abrufen zusätzlicher Überwachungs- und Diagnosedaten von virtuellen Computern. Sie können diese Leistungsmetriken anzeigen und basierend auf der VM-Leistung Benachrichtigungen erstellen. Die Diagnose-Erweiterung wird wie folgt über das Azure-Portal installiert:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
1. Klicken Sie auf **Diagnoseeinstellungen**. Die Liste zeigt, dass die *Startdiagnose* bereits im vorherigen Abschnitt aktiviert wurde. Klicken Sie auf das Kontrollkästchen für *Grundlegende Metriken*.
1. Wählen Sie im Abschnitt *Speicherkonto* das Konto *mydiagdata[1234]* aus, das im vorherigen Abschnitt erstellt wurde.
1. Klicken Sie auf die Schaltfläche **Save** .

    ![Anzeigen von Diagnosemetriken](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Anzeigen von Metriken des virtuellen Computers

Sie können die VM-Metriken auf die gleiche Weise anzeigen, wie Sie die Metriken zur Host-VM angezeigt haben:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
1. Um die Leistung des virtuellen Computers anzuzeigen, klicken Sie dem Blatt der VM auf **Metriken**, und wählen Sie dann eine der Diagnosemetriken unter **Verfügbare Metriken** aus.

    ![Anzeigen von Metriken des virtuellen Computers](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Erstellen von Warnungen

Sie können Warnungen auf Grundlage von bestimmten Leistungsmetriken erstellen. Warnungen können für Benachrichtigungen verwendet werden, wenn z.B. die durchschnittliche CPU-Auslastung einen bestimmten Schwellenwert überschreitet oder wenn der verfügbare freie Speicherplatz unter einen bestimmten Wert fällt. Warnungen werden im Azure-Portal angezeigt oder können per E-Mail gesendet werden. Sie können auch als Reaktion auf die generierten Warnungen Azure Automation-Runbooks oder Azure Logic Apps auslösen.

Das folgende Beispiel erstellt eine Warnung für die durchschnittliche CPU-Auslastung.

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen**, und wählen Sie **myResourceGroup** und dann in der Ressourcenliste **myVM** aus.
2. Klicken Sie auf dem Blatt des virtuellen Computers auf **Warnungsregeln** und dann am oberen Rand des Warnungsblatts auf **Metrikwarnung hinzufügen**.
4. Geben Sie einen **Namen** für die Warnung ein, z.B. *myAlertRule*.
5. Um eine Warnung auszulösen, wenn der CPU-Prozentsatz 1.0 für fünf Minuten überschreitet, belassen Sie alle anderen Standardeinstellungen ausgewählt.
6. Aktivieren Sie optional das Kontrollkästchen *E-Mail-Besitzer, Mitwirkende und Leser*, um E-Mail-Benachrichtigungen zu senden. Als Standardaktion wird im Portal eine Benachrichtigung angezeigt.
7. Klicken Sie auf die Schaltfläche **OK**.

## <a name="manage-package-updates"></a>Verwalten von Paketupdates

Mithilfe der Updateverwaltung können Sie Paketupdates und Patches für Ihre Azure-Linux-VMs verwalten. Direkt von Ihrem virtuellen Computer aus können Sie schnell den Status der verfügbaren Updates bewerten, die Installation der erforderlichen Updates planen und Bereitstellungsergebnisse, überprüfen, um sicherzustellen, dass Updates erfolgreich auf den virtuellen Computer angewendet wurden.

Informationen zur Preisgestaltung finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).

### <a name="enable-update-management-preview"></a>Aktivieren der Updateverwaltung (Vorschau)

Aktivieren der Updateverwaltung für Ihre VM

1. Wählen Sie auf der linken Seite des Bildschirms **Virtuelle Computer**.
1. Wählen Sie einen virtuellen Computer in der Liste aus.
1. Klicken Sie auf dem VM-Bildschirm im Abschnitt **Vorgänge** auf **Updateverwaltung**. Der Bildschirm **Updateverwaltung aktivieren** wird geöffnet.

Eine Überprüfung wird ausgeführt, um festzustellen, ob die Updateverwaltung für diesen virtuellen Computer aktiviert ist. Die Überprüfung umfasst Prüfungen für einen Log Analytics-Arbeitsbereich und ein verknüpftes Automation-Konto, und ob die Lösung im Arbeitsbereich vorhanden ist.

Mit einem Log Analytics-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Updateverwaltung generiert werden. Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen. Um weitere Aktionen auf virtuellen Computern auszuführen, die Updates erfordern, können Sie mit Azure Automation Skripts für virtuelle Computer ausführen, um z.B. Updates herunterzuladen und anzuwenden.

Der Überprüfungsprozess prüft auch, ob der virtuelle Computer mit dem Microsoft Monitoring Agent (MMA) und Hybrid Worker bereitgestellt wird. Dieser Agent wird verwendet, um mit dem virtuellen Computer zu kommunizieren und Informationen zum Updatestatus abzurufen. 

Wenn diese Voraussetzungen nicht erfüllt sind, wird ein Banner angezeigt, das Ihnen die Möglichkeit bietet, die Lösung zu aktivieren.

![Integriertes Banner zur Konfiguration der Updateverwaltung](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Klicken Sie auf das Banner, um die Lösung zu aktivieren. Wenn bei der Überprüfung festgestellt wird, dass eine der folgenden Voraussetzungen fehlt, wird sie automatisch hinzugefügt:

* [Log Analytics](../../log-analytics/log-analytics-overview.md)-Arbeitsbereich
* [Automation](../../automation/automation-offering-get-started.md)
* Ein [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) ist auf dem virtuellen Computer aktiviert.

Der Bildschirm **Updateverwaltung aktivieren** wird geöffnet. Konfigurieren Sie die Einstellungen, und klicken Sie auf **Aktivieren**.

![Aktivieren der Updateverwaltungslösung](./media/tutorial-monitoring/manage-updates-update-enable.png)

Das Aktivieren der Lösung kann bis zu 15 Minuten dauern, und während dieser Zeit sollten Sie das Browserfenster nicht schließen. Nachdem die Lösung aktiviert wurde, fließen Informationen zu fehlenden Updates vom Paketmanager auf dem virtuellen Computer zu Log Analytics.
Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

### <a name="view-update-assessment"></a>Anzeigen der Updatebewertung

Sobald die Lösung **Updateverwaltung** aktiviert ist, wird der Bildschirm **Updateverwaltung** angezeigt. Auf der Registerkarte **Fehlende Updates** wird eine Liste der fehlenden Updates angezeigt.

![Anzeigen des Updatestatus](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Planen einer Updatebereitstellung

Planen Sie zum Installieren von Updates eine Bereitstellung, die Ihrem Releasezeitplan und Wartungsfenster entspricht.

Um eine neue Updatebereitstellung für den virtuellen Computer zu planen, klicken Sie auf **Updatebereitstellung planen** am oberen Rand des Bildschirms **Updateverwaltung**. Geben Sie im Bildschirm **Neue Updatebereitstellung** die folgenden Informationen ein:

* **Name**: eindeutiger Name zum Identifizieren der Updatebereitstellung.
* **Auszuschließende Updates**: Wählen Sie diese Option, um die Namen von Paketen einzugeben, die vom Update ausgeschlossen werden sollen.
* **Zeitplaneinstellungen**: Sie können entweder Standarddatum und Standarduhrzeit (30 Minuten nach der aktuellen Zeit) akzeptieren oder einen anderen Zeitpunkt angeben. Sie können auch angeben, ob die Bereitstellung einmalig erfolgt, oder einen sich wiederholenden Zeitplan einrichten. Klicken Sie unter „Wiederholung“ auf die Option „Wiederholt“, um einen sich wiederholenden Zeitplan einzurichten.

  ![Bildschirm für Updatezeitplan-Einstellungen](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Wartungsfenster (Minuten)**: Geben Sie den Zeitraum an, in dem die Updatebereitstellung stattfinden soll.  So wird sichergestellt, dass Änderungen in dem von Ihnen festgelegten Wartungsfenster ausgeführt werden. 

Nachdem Sie die Konfiguration des Zeitplans abgeschlossen haben, klicken Sie auf die Schaltfläche **Erstellen**, sodass Sie zum Statusdashboard zurückkehren.
Beachten Sie, dass die Tabelle **Geplant** den von Ihnen erstellten Bereitstellungszeitplan anzeigt.

> [!WARNING]
> Der virtuelle Computer wird nach Installation der Updates automatisch neu gestartet, wenn ausreichend Zeit im Wartungsfenster vorhanden ist.

Die Updateverwaltung verwendet den auf Ihrem virtuellen Computer vorhandenen Paket-Manager, um Pakete zu installieren.

### <a name="view-results-of-an-update-deployment"></a>Anzeigen der Ergebnisse einer Updatebereitstellung

Nach dem Start der geplanten Bereitstellung sehen Sie den Status der Bereitstellung auf der Registerkarte **Updatebereitstellungen** auf dem Bildschirm **Updateverwaltung**.
Wenn sie aktuell ausgeführt wird, wird der Status **Vorgang wird ausgeführt** angezeigt. Nach erfolgreichem Abschluss ändert sich die Anzeige in **Erfolgreich**.
Wenn bei einem oder mehreren Updates in der Bereitstellung ein Fehler auftritt, wird der Status **Fehler** angezeigt.
Klicken Sie auf die abgeschlossene Updatebereitstellung, um das Dashboard für diese Updatebereitstellung anzuzeigen.

![Updatebereitstellungs-Statusdashboard für bestimmte Bereitstellung](./media/tutorial-monitoring/manage-updates-view-results.png)

Die Kachel **Updateergebnisse** enthält eine Zusammenfassung der Gesamtzahl der Updates und die Ergebnisse der Bereitstellung auf dem virtuellen Computer.
Die Tabelle rechts enthält eine detaillierte Analyse der einzelnen Updates und die Installationsergebnisse, wobei jeweils einer der folgenden Werte infrage kommt:

* **Kein Versuch erfolgt**: Das Update wurde nicht installiert, da aufgrund des definierten Wartungsfensters nicht genügend Zeit zur Verfügung stand.
* **Erfolgreich**: Das Update wurde erfolgreich heruntergeladen und auf dem virtuellen Computer installiert.
* **Fehler**: Das Update konnte nicht heruntergeladen oder nicht auf dem virtuellen Computer installiert werden.

Klicken Sie auf **Alle Protokolle**, um alle von der Bereitstellung erstellten Protokolleinträge anzuzeigen.

Klicken Sie auf die Kachel **Ausgabe**, um den Auftragsdatenstrom des Runbooks anzuzeigen, dass für die Verwaltung der Updatebereitstellung auf der Ziel-VM verantwortlich ist.

Klicken Sie auf **Fehler**, um ausführliche Informationen zu Fehlern aus der Bereitstellung anzuzeigen.

## <a name="advanced-monitoring"></a>Erweiterte Überwachung 

Mithilfe der [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) können Sie eine noch umfassendere Überwachung Ihrer virtuellen Computer umsetzen. Wenn Sie dies noch nicht getan haben, können Sie sich für eine [kostenlose Testversion](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) von Operations Management Suite registrieren.

Wenn Sie auf das OMS-Portal zugreifen, finden Sie den Arbeitsbereichsschlüssel und die Arbeitsbereichs-ID auf dem Blatt „Einstellungen“. Ersetzen Sie <workspace-key> und <workspace-id> durch die Werten aus Ihrem OMS-Arbeitsbereich, und verwenden Sie dann **az vm extension set**, um dem virtuellen Computer die OMS-Erweiterung hinzuzufügen:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

Auf dem Blatt „Protokollsuche“ im OMS-Portal sehen Sie *myVM* wie in der folgenden Abbildung dargestellt:

![OMS-Blatt](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Updates für einen virtuellen Computer konfiguriert, überprüft und verwaltet. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Aktivieren der Startdiagnose auf dem virtuellen Computer
> * Anzeigen der Startdiagnose
> * Anzeigen von Hostmetriken
> * Aktivieren der Diagnoseerweiterung auf dem virtuellen Computer
> * Anzeigen von Metriken des virtuellen Computers
> * Erstellen von Benachrichtigungen basierend auf Diagnosemetriken
> * Verwalten von Paketupdates
> * Einrichten der erweiterten Überwachung

Im nächsten Tutorial erhalten Sie Informationen zu Azure Security Center.

> [!div class="nextstepaction"]
> [Verwalten der VM-Sicherheit](./tutorial-azure-security.md)

