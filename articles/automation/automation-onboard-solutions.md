---
title: "Integrieren von Update- und Änderungsnachverfolgungslösungen in Azure Automation | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Update- und Änderungsnachverfolgungslösungen in Azure Automation integrieren."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: edae1156-2dc7-4dab-9e5c-bf253d3971d0
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: eamono
ms.openlocfilehash: 9ae5e9ebcbcf3af61318eac98defc6b249417f41
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Integrieren von Update- und Änderungsnachverfolgungslösungen in Azure Automation

In diesem Tutorial wird beschrieben, wie Sie Update-, Änderungsnachverfolgungs- und Inventarlösungen für VMs in Azure Automation integrieren:

> [!div class="checklist"]
> * Manuelles Integrieren eines virtuellen Azure-Computers
> * Installieren und Aktualisieren der erforderlichen Azure-Module
> * Importieren eines Runbooks für die Integration von Azure-VMs
> * Starten des Runbooks für die automatische Integration von Azure-VMs

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Tutorials müssen folgende Voraussetzungen erfüllt sein.
+ Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
+ [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.

## <a name="onboard-an-azure-virtual-machine-manually"></a>Manuelles Integrieren eines virtuellen Azure-Computers

1.  Öffnen Sie das Automation-Konto.
2.  Klicken Sie auf die Seite „Inventory“ (Inventar).
![Integrieren der Inventarlösung](media/automation-onboard-solutions/inventory-onboard.png)
3.  Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich, oder erstellen Sie einen neuen. Klicken Sie auf die Schaltfläche „Aktivieren“.
4.  Klicken Sie auf die Seite „Updateverwaltung“, nachdem die Benachrichtigung über die abgeschlossene Integration der Änderungsnachverfolgungs- und Inventarlösung angezeigt wird.
![Integrieren der Updatelösung](media/automation-onboard-solutions/update-onboard.png)
4.  Klicken Sie auf die Schaltfläche „Aktivieren“, um die Updatelösung zu integrieren.
5.  Klicken Sie auf die Seite „Änderungsnachverfolgung“, nachdem die Benachrichtigung über die abgeschlossene Integration der Updatelösung angezeigt wird.
![Integrieren der Änderungsnachverfolgung](media/automation-onboard-solutions/change-tracking-onboard-vm.png)
6.  Klicken Sie auf die Schaltfläche „Azure-VM hinzufügen“.
![Auswählen der VM für die Änderungsnachverfolgung](media/automation-onboard-solutions/enable-change-tracking.png)
7.  Wählen Sie eine Azure-VM aus, und klicken Sie auf die Schaltfläche „Aktivieren“, um die Änderungsnachverfolgungs- und Inventarlösung zu integrieren.
8.  Klicken Sie auf die Seite „Updateverwaltung“, nachdem die Benachrichtigung über die abgeschlossene VM-Integration angezeigt wird.
![Integrieren der VM für die Updateverwaltung](media/automation-onboard-solutions/update-onboard-vm.png)
9.  Klicken Sie auf die Schaltfläche „Azure-VM hinzufügen“.
![Auswählen der VM für die Updateverwaltung](media/automation-onboard-solutions/enable-update.png)
10.  Wählen Sie eine Azure-VM aus, und klicken Sie auf die Schaltfläche „Aktivieren“, um die Updateverwaltungslösung zu integrieren.

## <a name="install-and-update-required-azure-modules"></a>Installieren und Aktualisieren der erforderlichen Azure-Module

Es ist erforderlich, dass Sie das Update auf die aktuellen Azure-Module durchführen und AzureRM.OperationalInsights importieren, damit das Automatisieren der Lösungsintegration erfolgreich ist.
1.  Klicken Sie auf die Seite „Module“.
![Aktualisieren von Modulen](media/automation-onboard-solutions/update-modules.png)
2.  Klicken Sie auf die Schaltfläche „Azure-Module aktualisieren“, um das Update auf die aktuelle Version durchzuführen. Warten Sie, bis die Updates abgeschlossen sind.
3.  Klicken Sie auf die Schaltfläche „Katalog durchsuchen“, um den Modulkatalog zu öffnen.
![Importieren des OperationalInsights-Moduls](media/automation-onboard-solutions/import-operational-insights-module.png)
4.  Suchen Sie nach AzureRM.OperationalInsights, und importieren Sie dieses Modul in das Automation-Konto.

## <a name="import-a-runbook-that-onboards-solutions-to-azure-vms"></a>Importieren eines Runbooks zum Integrieren von Lösungen in Azure-VMs

1.  Klicken Sie unter der Kategorie „PROZESSAUTOMATISIERUNG“ auf die Seite „Runbooks“.
2.  Klicken Sie auf die Schaltfläche „Katalog durchsuchen“.
3.  Suchen Sie nach „Update und Änderungsnachverfolgung“, und importieren Sie das Runbook in das Automation-Konto.
![Importieren des Runbooks für die Integration](media/automation-onboard-solutions/import-from-gallery.png)
4.  Klicken Sie auf „Bearbeiten“, um die Runbookquelle anzuzeigen, und klicken Sie dann auf die Schaltfläche „Veröffentlichen“.
![Importieren des Runbooks für die Integration](media/automation-onboard-solutions/publish-runbook.png)

## <a name="start-the-runbook-that-onboards-azure-vms-automatically"></a>Starten des Runbooks für die automatische Integration von Azure-VMs

Sie müssen entweder eine Änderungsnachverfolgungs- oder Updatelösung in eine Azure-VM integriert haben, um dieses Runbook starten zu können. Hierfür sind ein vorhandener virtueller Computer und eine Ressourcengruppe sowie die integrierte Lösung für Parameter erforderlich.
1.  Öffnen Sie das Runbook „Enable-MultipleSolution“.
![Mehrere Lösungsrunbooks](media/automation-onboard-solutions/runbook-overview.png)
2.  Klicken Sie auf die Schaltfläche „Start“, und geben Sie die folgenden Werte für die Parameter ein:
    *   VMNAME: Der Name einer vorhandenen VM für die Integration der Update- oder Änderungsnachverfolgungslösung. Wenn Sie dieses Feld leer lassen, werden alle VMs der Ressourcengruppe integriert.
    *   VMRESOURCEGROUP: Der Name der Ressourcengruppe, deren Mitglied die VM ist.
    *   SUBSCRIPTIONID: Die Abonnement-ID der neuen VM, die integriert werden soll. Wenn Sie dieses Feld leer lassen, wird das Abonnement des Arbeitsbereichs verwendet. Wenn eine andere Abonnement-ID angegeben wird, sollte das RunAs-Konto für das Automation-Konto auch als Mitwirkender für das Abonnement hinzugefügt werden.
    *   ALREADYONBOARDEDVM: Der Name der VM, die für die Update- oder Änderungsnachverfolgungslösung manuell integriert wurde.
    *   ALREADYONBOARDEDVMRESOURCEGROUP: Der Name der Ressourcengruppe, deren Mitglied die VM ist.
    *   SOLUTIONTYPE: Geben Sie „Updates“ oder „ChangeTracking“ ein.
    
    ![Mehrere Runbookparameter für Lösungen](media/automation-onboard-solutions/runbook-parameters.png)
3.  Klicken Sie auf „OK“, um den Runbookauftrag zu starten.
4.  Überwachen Sie den Status und etwaige Fehler auf der Seite für den Runbookauftrag.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Planen von Runbooks](automation-schedules.md).