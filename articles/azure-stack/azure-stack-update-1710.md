---
title: Azure Stack 1710 Update (Build 20171020.1) | Microsoft-Dokumentation
description: "Es wird beschrieben, was in Update 1710 für integrierte Azure Stack-Systeme enthalten ist, welche bekannten Probleme es gibt und wo das Update heruntergeladen werden kann."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: twooley
ms.openlocfilehash: a66c7dddab1a2246dd0a9fa3c7533063140d2f01
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2017
---
# <a name="azure-stack-1710-update-build-201710201"></a>Azure Stack Update 1710 (Build 20171020.1)

*Gilt für: Integrierte Azure Stack-Systeme*

In diesem Artikel wird beschrieben, welche Verbesserungen und Fehlerbehebungen in diesem Updatepaket enthalten sind, welche bekannten Probleme es für dieses Release gibt und wo das Update heruntergeladen werden kann. Die bekannten Probleme sind in Probleme unterteilt, die sich direkt auf den Updateprozess beziehen, und in Probleme mit dem Build (nach der Installation).

> [!IMPORTANT]
> Dieses Updatepaket gilt nur für integrierte Azure Stack-Systeme. Wenden Sie dieses Updatepaket nicht auf das Azure Stack Development Kit an.

## <a name="improvements-and-fixes"></a>Verbesserungen und Fehlerbehebungen

Dieses Update enthält die folgenden Qualitätsverbesserungen und Fehlerbehebungen für Azure Stack.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Windows Server 2016 – Verbesserungen und Fehlerbehebungen

- Updates für Windows Server 2016: 10. Oktober 2017 – KB4041691 (Betriebssystembuild 14393.1770). Weitere Informationen finden Sie unter [https://support.microsoft.com/help/4041691](https://support.microsoft.com/help/4041691).

### <a name="additional-quality-improvements-and-fixes"></a>Zusätzliche Qualitätsverbesserungen und Fehlerbehebungen

- PowerShell-Cmdlets für privilegierte Endpunkte hinzugefügt, um die Problembehandlung und das Aktualisieren des NTP-Servers (Network Time Protocol) zu unterstützen.
- Unterstützung für Aktualisierung des privilegierten Endpunkts, JEA-Endpunktmodule (Just Enough Administration) und Cmdlet-Whitelist hinzugefügt. 
- Fehler in lokalen Sprachen für den privilegierten Endpunkt behoben.
- Möglichkeit zur Rotation von Gatewayanmeldeinformationen hinzugefügt.
- Lokales CBLocalAdmin-Administratorkonto entfernt. 
- Fehler im Inhalt der Heartbeat-Warnungsvorlage behoben, um sicherzustellen, dass Heartbeat-Warnungen nach einem Update richtig funktionieren.
- Fehler im Fabric-Ressourcenanbieter behoben, damit Zeitlimits bei FRU-Vorgängen richtig behandelt werden. 
- Möglichkeit zur Nutzung von Azure Resource Manager-API-Profilen für Cloudentwickler in Azure Stack hinzugefügt.
- Windows Update-Dienst auf dem virtuellen Computer für die Bereitstellung (Deployment Virtual Machine, DVM) deaktiviert. 
- Aktionen für das Ein- und Ausschalten des Knotens aus der Benutzeroberfläche entfernt.
- Verschiedene andere Fehlerbehebungen in Bezug auf die Leistung und Stabilität durchgeführt. 
 
## <a name="known-issues-with-the-update-process"></a>Bekannte Probleme mit dem Updateprozess

Dieser Abschnitt enthält bekannte Probleme, die bei der Installation von Update 1710 auftreten können.

> [!IMPORTANT]
> Wenn das Update fehlschlägt, müssen Sie beim erneuten Durchführen des Updates das Cmdlet `Resume-AzureStackUpdate` vom privilegierten Endpunkt verwenden. Führen Sie das Update nicht über das Administratorportal aus. (Dabei handelt es sich um ein bekanntes Problem dieses Releases.) Weitere Informationen finden Sie unter [Überwachen von Änderungen in Azure Stack mithilfe des privilegierten Endpunkts](azure-stack-monitor-update.md).

| Symptom  | Ursache  | Lösung |
|---------|---------|---------|
|Beim Durchführen eines Updates kann im Schritt „Storage Hosts Restart Storage Node“ des Plans mit den Updateaktionen ein Fehler der folgenden Art auftreten:<br><br>**{"name":"Restart Storage Hosts","description":"Restart Storage Hosts.","errorMessage":"Type 'Restart' of Role 'BareMetal' raised an exception:\n\nThe computer HostName-05 is skipped. Fail to retrieve its LastBootUpTime via the WMI service with the following error message: The RPC server is unavailable. (Exception from HRESULT: 0x800706BA).\nat Restart-Host** | Die Ursache dieses Problems ist ein potenzieller Fehler in einigen Konfigurationen. | 1. Melden Sie sich an der BMC-Weboberfläche (Baseboard Management Controller, Baseboard-Verwaltungscontroller) an, und starten Sie den in der Fehlermeldung angegebenen Host neu.<br><br>2. Führen Sie das Update erneut über den privilegierten Endpunkt durch. |
| Beim Durchführen eines Updates scheint der Updateprozess zu hängen und wird nach dem Schritt „Step: Running step 2.4 - Install update“ des Plans mit den Updateaktionen nicht fortgesetzt.<br><br>Auf diesen Schritt folgt dann eine Reihe von Kopierprozessen für NUPKG-Dateien auf die Dateifreigaben der internen Infrastruktur. Beispiel:<br><br>**Copying 1 files from content\PerfCollector\VirtualMachines to \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\PerfCounterConfiguration**  | Das Problem wird verursacht, wenn zu viele Protokolldateien auf einem virtuellen Computer der Infrastruktur vorhanden sind und ein Problem mit dem SOFS (Scale-Out File Server, Dateiserver mit horizontaler Skalierung) von Windows Server besteht, der in einem nachfolgenden Update bereitgestellt wird. | Wenden Sie sich an den Microsoft-Kundendienst und -Support (CSS), um Hilfe zu erhalten. | 
| Beim Durchführen eines Updates tritt während des Schritts „Step: Running step 2.13.2 - Update *VM-Name*“ des Plans mit den Updateaktionen ein Fehler der folgenden Art auf: (Der Name des virtuellen Computers kann variieren.)<br><br>**ActionPlanInstanceWarning ece/MachineName: WarningMessage:Task: Invocation of interface 'LiveUpdate' of role 'Cloud\Fabric\WAS' failed:<br>Type 'LiveUpdate' of Role 'WAS' raised an exception:<br>ERROR during storage initialization: An error occurred while trying to make an API call to Microsoft Storage service: {"Message":"A timeout occurred while communicating with Service Fabric. Exception Type: TimeoutException. Exception message: Operation timed out."}**  | Das Problem wird durch eine E/A-Zeitüberschreitung in Windows Server verursacht, die in einem nachfolgenden Update behoben sein wird. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.
| Beim Durchführen eines Updates kann während des Schritts „Step 21 Restart SQL server VMs“ ein Fehler der folgenden Art auftreten:<br><br>**Type 'LiveUpdateRestart' of Role 'VirtualMachines' raised an exception:<br>VerboseMessage:[VirtualMachines:LiveUpdateRestart] Querying for VM MachineName-Sql01. - 10/13/2017 5:11:50 PM VerboseMessage:[VirtualMachines:LiveUpdateRestart] VM is marked as HighlyAvailable. - 10/13/2017 5:11:50 PM VerboseMessage:[VirtualMachines:LiveUpdateRestart] at MS.Internal.ServerClusters.ExceptionHelp.Build at MS.Internal.ServerClusters.ClusterResource.BeginTakeOffline(Boolean force) at Microsoft.FailoverClusters.PowerShell.StopClusterResourceCommand.BeginTimedOperation() at Microsoft.FailoverClusters.PowerShell.TimedCmdlet.WrappedProcessRecord() at Microsoft.FailoverClusters.PowerShell.FCCmdlet.ProcessRecord() - 10/13/2017 5:11:50 PM WarningMessage:Task: Invocation of interface 'LiveUpdateRestart' of role 'Cloud\Fabric\VirtualMachines' failed:** | Dieses Problem kann auftreten, wenn der virtuelle Computer nicht neu gestartet werden konnte. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.
| Beim Durchführen eines Updates kann ein Fehler der folgenden Art auftreten:<br><br>**2017-10-22T01:37:37.5369944Z Type 'Shutdown' of Role 'SQL' raised an exception:An error occurred pausing node 's45r1004-Sql01'.at Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_App1\EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Roles\SQL\SQL.psm1: line 542at Shutdown, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_App1\EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Classes\SQL\SQL.psm1: line 50at <ScriptBlock>, <No file>: line 18at <ScriptBlock>, <No file>: line 16** | Dieses Problem kann auftreten, wenn der virtuelle Computer nicht in den angehaltenen Zustand versetzt werden kann, um die Rollen zu beseitigen. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.
| Beim Durchführen eines Updates kann einer der folgenden Fehler auftreten:<br><br>**Type 'Validate' of Role 'ADFS' raised an exception: Validation for ADFS/Graph role failed with error: Error checking ADFS probe endpoint *endpoint_URI*: Exception calling "GetResponse" with "0" argument(s): "The remote server returned an error: (503) Server Unavailable." at Invoke-ADFSGraphValidation**<br><br>**Type 'Validate' of Role 'ADFS' raised an exception: Validation for ADFS/Graph role failed with error: Error fetching ADFS properties: Could not connect to net.tcp://localhost:1500/policy. The connection attempt lasted for a time span of 00:00:02.0498923. TCP error code 10061: No connection could be made because the target machine actively refused it 127.0.0.1:1500. at Invoke-ADFSGraphValidation** | Der Plan mit den Updateaktionen kann die Integrität für Active Directory-Verbunddienste (AD FS) nicht überprüfen. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.

## <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)

Dieser Abschnitt enthält bekannte Probleme, die für Build 20171020.1 nach der Installation auftreten können.

### <a name="portal"></a>Portal

- Unter Umständen ist es nicht möglich, Compute- oder Speicherressourcen im Administratorportal anzuzeigen. Es wird angegeben, dass während der Installation des Updates ein Fehler aufgetreten ist und dass das Update fälschlicherweise als erfolgreich gemeldet wurde. Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten, wenn dieses Problem auftritt.
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.
- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcengruppen zwischen Abonnements wird derzeit nicht unterstützt.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
-  Für alle Workflows, für die Sie in einer Dropdownliste ein Abonnement, eine Ressourcengruppe oder einen Standort auswählen, können unter Umständen folgende Probleme auftreten:

   - Ggf. wird oben in der Liste eine leere Zeile angezeigt. Es sollte trotzdem möglich sein, ein Element auf normale Weise auszuwählen.
   - Wenn die Dropdownliste mit den Elementen kurz ist, ist es ggf. nicht möglich, die Elementnamen anzuzeigen.
   - Wenn Sie über mehrere Benutzerabonnements verfügen, kann die Dropdownliste für die Ressourcengruppen leer sein. 

   Zum Umgehen der beiden letztgenannten Probleme können Sie den Namen des Abonnements oder der Ressourcengruppe eingeben (falls bekannt), oder Sie können stattdessen PowerShell verwenden.
  
### <a name="backup"></a>Backup 

- Belassen Sie die Infrastruktursicherung auf dem Blatt **Infrastructure backup** (Infrastruktursicherung) im deaktivierten Zustand.

### <a name="health-and-monitoring"></a>Integrität und Überwachung

- Wenn Sie eine Infrastrukturrolleninstanz neu starten, wird ggf. eine Meldung mit dem Hinweis angezeigt, dass der Neustart nicht erfolgreich war. Der Neustart wurde aber erfolgreich durchgeführt.

### <a name="services"></a>Dienste

**Marketplace**
- Wenn Sie versuchen, dem Azure Stack-Marketplace mit der Option **Add from Azure** (Aus Azure hinzufügen) Elemente hinzuzufügen, werden unter Umständen nicht alle Elemente zum Herunterladen angezeigt.
- Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden. Sie können eine Skalierungsgruppe erstellen, indem Sie eine Vorlage verwenden.
- Ein Mandant muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen. 

**SQL/MySQL**
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.

**Compute**
- Benutzer haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen. Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt.
- Sie können eine VM-Verfügbarkeitsgruppe nur mit einer Fehlerdomäne und einer Updatedomäne konfigurieren.
 
**Netzwerk**
- Sie können im Portal keinen Lastenausgleich mit einer öffentlichen IP-Adresse erstellen. Zum Umgehen dieses Problems können Sie den Lastenausgleich mit PowerShell erstellen.
- Beim Erstellen eines Netzwerklastenausgleichs müssen Sie eine NAT-Regel (Network Address Translation, Netzwerkadressübersetzung) erstellen. Andernfalls wird ein Fehler ausgegeben, wenn Sie versuchen, eine NAT-Regel hinzufügen, nachdem der Lastenausgleich erstellt wurde.
- Sie können die Zuordnung einer öffentlichen IP-Adresse zu einem virtuellen Computer nicht aufheben, nachdem der virtuelle Computer erstellt und dieser IP-Adresse zugeordnet wurde. Die Aufhebung der Zuordnung war scheinbar erfolgreich, die zuvor zugewiesene öffentliche IP-Adresse bleibt dem ursprünglichen virtuellen Computer jedoch zugeordnet. Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird. Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer virtueller Computer verwenden.
 
### <a name="field-replaceable-unit-fru-procedures"></a>FRU-Verfahren (Field Replaceable Unit)

- Während des Aktualisierungslaufs werden Offlineimages nicht gepatcht. Wenn Sie den Knoten einer Skalierungseinheit ersetzen müssen, sollten Sie zusammen mit Ihrem OEM-Hardwareanbieter sicherstellen, dass der ersetzte Knoten über die aktuelle Patchebene verfügt.

## <a name="download-the-update"></a>Herunterladen des Updates

Sie können das Paket mit Update 1710 [hier](https://aka.ms/azurestackupdatedownload) herunterladen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Updateverwaltung in Azure Stack finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md).
- Informationen zur Anwendung von Updates finden Sie unter [Anwenden von Updates in Azure Stack](azure-stack-apply-updates.md).