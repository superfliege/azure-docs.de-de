---
title: Updates mithilfe von SCCM-Sammlungen in der OMS-Updateverwaltung | Microsoft-Dokumentation
description: "Dieser Artikel hilft Ihnen beim Konfigurieren von System Center Configuration Manager mit dieser Lösung, um Updates von über SCCM verwalteten Computern zu verwalten."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: c3ae8da65e03fe9e11b5657a6a40d02de0567da6
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="integrate-system-center-configuration-manager-with-oms-update-management-preview"></a>Integrieren von System Center Configuration Manager und OMS-Updateverwaltung (Vorschau)

Kunden, die in System Center Configuration Manager investiert haben, um PCs, Server und Mobilgeräte zu verwalten, profitieren auch von der Leistungsstärke und dem Funktionsumfang dieser Lösung bei der Verwaltung von Softwareupdates im Rahmen des Softwareupdateverwaltungs-Zyklus.  

Aufbauend auf der bereits bestehenden Integration zwischen OMS und Configuration Manager können Sie Berichte für verwaltete Windows-Server erstellen und die Server verwalten, indem Sie Softwareupdatebereitstellungen in Configuration Manager erstellen und vorab bereitstellen. Sie können auch in der [Lösung für die Updateverwaltung in OMS](../operations-management-suite/oms-solution-update-management.md) den detaillierten Status abgeschlossener Updatebereitstellungen abrufen. Wenn Sie Configuration Manager zum Aktualisieren von Complianceberichten verwenden, aber nicht zum Verwalten von Updatebereitstellungen für Ihre Windows-Server, können Sie für die Berichterstellung weiterhin Configuration Manager verwenden, während Sicherheitsupdates mit der OMS-Updateverwaltungslösung verwaltet werden.

## <a name="prerequisites"></a>Voraussetzungen

* Die [Updateverwaltungslösung](../operations-management-suite/oms-solution-update-management.md) muss zu Ihrem Log Analytics-Arbeitsbereich hinzugefügt und mit Ihrem Automation-Konto in der gleichen Ressourcengruppe und Region verknüpft sein.   
* Windows-Server, die zurzeit von Ihrer System Center Configuration Manager-Umgebung verwaltet werden, müssen auch Berichte an den Log Analytics-Arbeitsbereich übermitteln, in dem die Updateverwaltungslösung aktiviert ist.  
* Dieses Feature unterstützt System Center Configuration Manager Current Branch, Version 1606 und höher.  Informationen zum Integrieren der zentralen Configuration Manager-Verwaltungswebsite oder einer eigenständigen primären Website mit Log Analytics und zum Importieren von Sammlungen finden Sie unter [Herstellen einer Verbindung zwischen Configuration Manager und Log Analytics](../log-analytics/log-analytics-sccm.md).  
* Windows-Agents müssen entweder für die Kommunikation mit einem WSUS-Server (Windows Server Update Services) konfiguriert sein oder über Zugriff auf Microsoft-Update verfügen, wenn sie keine Sicherheitsupdates von Configuration Manager erhalten.   

Wie Sie die in Azure IaaS gehosteten Clients mit Ihrer vorhandenen Configuration Manager-Umgebung verwalten, richtet sich primär nach der Verbindung zwischen Azure-Rechenzentren und Ihrer Infrastruktur. Diese Verbindung wirkt sich auf alle Designänderungen aus, die Sie an Ihrer Configuration Manager-Infrastruktur vornehmen müssen, und auf die Kosten, die anfallen, um diese Änderungen zu unterstützen.  Informationen zu den Planungsüberlegungen, die Sie berücksichtigen müssen, finden Sie unter [Configuration Manager in Azure – häufig gestellte Fragen](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure#networking).    

## <a name="configuration"></a>Konfiguration

### <a name="manage-software-updates-from-configuration-manager"></a>Verwalten von Softwareupdates von Configuration Manager 

Führen Sie folgende Schritte aus, wenn Sie Updatebereitstellungen weiterhin über Configuration Manager verwalten möchten.  OMS stellt eine Verbindung mit Configuration Manager her, um Updates auf den Clientcomputern anzuwenden, die mit Ihrem Log Analytics-Arbeitsbereich verbunden sind. Die Updateinhalte sind im Cache der Clientcomputer verfügbar, so als würde die Bereitstellung von Configuration Manager verwaltet.  

1. Erstellen Sie mithilfe des unter [Bereitstellen von Softwareupdates](https://docs.microsoft.com/en-us/sccm/sum/deploy-use/deploy-software-updates) beschriebenen Prozesses eine Softwareupdatebereitstellung von der Top-Level-Website in Ihrer Configuration Manager-Hierarchie.  Die einzige Einstellung, die anders konfiguriert werden muss als bei einer Standardbereitstellung, ist die Option **Softwareupdates nicht installieren**, um das Downloadverhalten des Bereitstellungspakets zu steuern. Dieses Verhalten wird von der OMS-Updateverwaltungslösung durch Erstellen einer geplanten Updatebereitstellung im nächsten Schritt gesteuert.  
2. Wählen Sie im Azure-Portal im Bildschirm **Automation-Konto** Ihr Automation-Konto aus, und erstellen Sie eine Variable vom Typ „boolesch“ mit dem Namen **UseOMSForSCCMUpdates** und dem Wert **true**. Eine Anleitung finden Sie unter [So erstellen Sie eine neue Variable über das Azure-Portal](../automation/automation-variables.md#to-create-a-new-variable-with-the-azure-portal).
3. Öffnen Sie im OMS-Portal das Dashboard der Updateverwaltung.  Erstellen Sie anhand der Schritte in [Erstellen einer Updatebereitstellung](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment) eine neue Bereitstellung, und wählen Sie aus der Dropdownliste eine geeignete Configuration Manager-Sammlung aus, die als OMS-Computergruppe dargestellt wird.  Berücksichtigen Sie dabei die folgenden wichtigen Punkte:
    1. Wenn in der ausgewählten Configuration Manager-Gerätesammlung ein Wartungsfenster definiert ist, verwenden die Elemente der Sammlung dieses Fenster anstelle der Einstellung **Dauer**, die in der geplanten Bereitstellung in OMS definiert ist.
    2. Elemente der Zielsammlung müssen über eine Verbindung mit dem Internet verfügen (entweder direkt, über einen Proxyserver oder über das OMS-Gateway).  

Nach Abschluss der Updatebereitstellung mit der OMS-Lösung installieren die Zielcomputer, die Mitglieder der ausgewählten Computergruppe sind, die Updates zum geplanten Zeitpunkt aus ihrem lokalen Clientcache.  Sie können den [Status der Updatebereitstellung anzeigen](../operations-management-suite/oms-solution-update-management.md#viewing-update-deployments), um die Ergebnisse Ihrer Bereitstellung zu überwachen.  

### <a name="manage-software-updates-from-oms"></a>Verwalten von Softwareupdates über OMS

Um Updates für virtuelle Windows Server-Computer zu verwalten, die Configuration Manager-Clients sind, müssen Sie die Clientrichtlinie so konfigurieren, dass die Funktion der Softwareupdateverwaltung für alle von dieser Lösung verwalteten Clients deaktiviert wird.  Standardmäßig gelten Clienteinstellungen für alle Geräte in der Hierarchie.  Weitere Informationen zu dieser Richtlinieneinstellung und ihrer Konfiguration finden Sie unter [Konfigurieren von Clienteinstellungen in System Center Configuration Manager](https://docs.microsoft.com/sccm/core/clients/deploy/configure-client-settings).  

Nachdem Sie dieses Konfigurationsänderung vorgenommen haben, erstellen Sie anhand der Schritte in [Erstellen einer Updatebereitstellung](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment) eine neue Bereitstellung, und wählen Sie aus der Dropdownliste eine geeignete Configuration Manager-Sammlung aus, die als OMS-Computergruppe dargestellt wird. 


