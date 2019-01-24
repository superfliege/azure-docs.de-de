---
title: Verwalten von Azure Automation-Daten
description: Dieser Artikel enthält mehrere Themen zum Verwalten einer Azure Automation-Umgebung.  Er enthält derzeit die Themen "Datenaufbewahrung", "Sichern von Azure Automation" und "Notfallwiederherstellung in Azure Automation".
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b133947a7daf1977f084ee4008599882b51d9e8b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428476"
---
# <a name="managing-azure-automation-data"></a>Verwalten von Azure Automation-Daten
Dieser Artikel enthält mehrere Themen zum Verwalten einer Azure Automation-Umgebung.

## <a name="data-retention"></a>Beibehaltung von Daten
Wenn Sie eine Ressource in Azure Automation löschen, wird diese zu Überwachungszwecken für 90 Tage aufbewahrt, bevor sie dauerhaft gelöscht wird.  In diesem Zeitraum kann die Ressource weder angezeigt noch verwendet werden.  Diese Richtlinie gilt auch für Ressourcen, die zu einem Automation-Konto gehören, das gelöscht wurde.

Aufträge, die älter sind als 90 Tage, werden in Azure Automation automatisch gelöscht und dauerhaft entfernt.

Die folgende Tabelle zeigt die Aufbewahrungsrichtlinie für unterschiedliche Ressourcen.

| Daten | Richtlinie |
|:--- |:--- |
| Konten |Dauerhafte Entfernung 90 Tage nach dem Löschen des Kontos durch einen Benutzer. |
| Objekte |Dauerhafte Entfernung 90 Tage nach dem Löschen des Objekts durch einen Benutzer, oder 90 Tage nach dem Löschen des Kontos mit dem Objekt durch einen Benutzer. |
| Module |Dauerhafte Entfernung 90 Tage nach dem Löschen des Moduls durch einen Benutzer, oder 90 Tage nach dem Löschen des Kontos mit dem Modul durch einen Benutzer. |
| Runbooks |Dauerhafte Entfernung 90 Tage nach dem Löschen der Ressource durch einen Benutzer, oder 90 Tage nach dem Löschen des Kontos mit der Ressource durch einen Benutzer. |
| Aufträge |Löschung und dauerhafte Entfernung 90 Tage nach der letzten Änderung. Dies kann der Fall sein, wenn der Job abgeschlossen, gestoppt oder angehalten wurde. |
| Knotenkonfigurationen/MOF-Dateien |Die alte Knotenkonfiguration wird 90 Tage nach dem Generieren einer neuen Knotenkonfiguration dauerhaft entfernt. |
| DSC-Knoten |Diese werden 90 Tage nach Aufhebung der Registrierung im Automation-Konto über das Azure-Portal oder mit dem Cmdlet [Unregister-AzureRMAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) in Windows PowerShell dauerhaft entfernt. Wenn ein Benutzer das Konto löscht, in dem die Knoten enthalten sind, werden diese ebenfalls nach 90 Tagen dauerhaft entfernt. |
| Knotenberichte |90 Tage nach dem Generieren eines neuen Berichts für diesen Knoten dauerhaft entfernt |

Die Datenaufbewahrungsrichtlinie gilt für alle Benutzer und kann zurzeit nicht angepasst werden.

Wenn Sie jedoch Daten für einen längeren Zeitraum beibehalten möchten, können Sie Runbook-Auftragsprotokolle zu Log Analytics weiterleiten.  Weitere Informationen finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Sichern von Azure Automation
Wenn Sie ein Automation-Konto in Microsoft Azure löschen, werden alle Objekte im Konto gelöscht – darunter Runbooks, Module, Konfigurationen, Einstellungen, Aufträge und Assets. Die Objekte können nicht wiederhergestellt werden, nachdem das Konto gelöscht wurde.  Sie können die Inhalte Ihres Automation-Kontos mithilfe der folgenden Informationen sichern, bevor Sie das Konto löschen. 

### <a name="runbooks"></a>Runbooks
Sie können Ihre Runbooks entweder unter Verwendung des Azure-Portals oder mithilfe des Cmdlets [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) in Windows PowerShell in Skriptdateien exportieren.  Diese Skriptdateien können in ein anderes Automation-Konto importiert werden, wie beschrieben unter [Erstellen oder Importieren eines Runbooks](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Integrationsmodule
Integrationsmodule können nicht aus Azure Automation exportiert werden.  Sie müssen sicherstellen, dass diese außerhalb des Automation-Kontos verfügbar sind.

### <a name="assets"></a>Objekte
[Objekte](https://msdn.microsoft.com/library/dn939988.aspx) können nicht aus Azure Automation exportiert werden.  Sie müssen sich unter Verwendung des Azure-Portals die Details zu Variablen, Anmeldeinformationen, Zertifikaten, Verbindungen und Zeitplänen notieren.  Anschließend müssen Sie alle Objekte, die in Runbooks verwendet und in ein anderes Automation-Konto importiert werden sollen, manuell erstellen.

Sie können mithilfe von [Azure-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.automation#automation) Details aus nicht verschlüsselten Objekten abrufen und diese entweder zu Referenzzwecken speichern, oder Sie können gleichwertige Objekte in einem anderen Automation-Konto erstellen.

Es ist nicht möglich, den Wert verschlüsselter Variablen oder die Kennwortfelder für Anmeldeinformationen mithilfe von Cmdlets abzurufen.  Wenn Sie diese Werte nicht kennen, können Sie sie mithilfe der Aktivitäten [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) und [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) aus einem Runbook abrufen.

Zertifikate können nicht aus Azure Automation exportiert werden.  Sie müssen sicherstellen, dass die erforderlichen Zertifikate außerhalb von Azure zur Verfügung stehen.

### <a name="dsc-configurations"></a>DSC-Konfigurationen
Sie können Ihre Konfigurationen unter Verwendung des Azure-Portals oder mithilfe des Cmdlets [Export-AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/azurerm.automation/export-azurermautomationdscconfiguration) in Windows PowerShell in Skriptdateien exportieren. Diese Konfigurationen können in ein anderes Automation-Konto importiert und darin verwendet werden.

## <a name="geo-replication-in-azure-automation"></a>Georeplikation in Azure Automation
Über die für Azure Automation-Konten standardmäßig eingerichtete Georeplikation werden die Kontodaten für mehr Redundanz an einer geografisch unterschiedlichen Region gespeichert. Hierzu können Sie beim Einrichten des Kontos eine primäre Region auswählen, der dann automatisch eine sekundäre Region zugeordnet wird. Die aus der primären Region kopierten sekundären Daten werden bei einem Datenverlust kontinuierlich aktualisiert.  

In der folgenden Tabelle werden die verfügbaren Paare primärer und sekundärer Regionen gezeigt:

| Primär | Sekundär |
| --- | --- |
| USA Süd Mitte |USA Nord Mitte |
| USA (Ost 2) |USA (Mitte) |
| Europa, Westen |Nordeuropa |
| Südostasien |Ostasien |
| Japan, Osten |Japan, Westen |

Microsoft versucht im unwahrscheinlichen Fall, dass Daten der primären Region verloren gehen, diese wiederherzustellen. Wenn die Daten der primären Region nicht wiederhergestellt werden können, erfolgt ein Geofailover, über das die betroffenen Kunden durch ihr Abonnement benachrichtigt werden.


