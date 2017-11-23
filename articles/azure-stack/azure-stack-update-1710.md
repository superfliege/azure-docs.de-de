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
ms.date: 11/09/2017
ms.author: twooley
ms.openlocfilehash: 32ca25998046caa6cf29410644f3cd86868b679d
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
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
|Beim Durchführen eines Updates<br>kann während des Schritts „Storage Hosts Restart Storage Node“<br> des Plans mit den Updateaktionen ein Fehler der folgenden Art auftreten:<br><br>**{"name":"Restart Storage Hosts","description":"Restart<br> Storage Hosts.","errorMessage":"Type 'Restart' of Role<br> 'BareMetal' raised an exception:\n\n The computer<br> HostName-05 is skipped. Fail to retrieve its LastBootUpTime<br> via the WMI service with the following error message:<br> The RPC server is unavailable.<br> (Exception from HRESULT: 0x800706BA).\nat Restart-Host** | Die Ursache dieses Problems ist ein potenzieller Fehler in einigen Konfigurationen. | 1. Melden Sie sich an der BMC-Weboberfläche (Baseboard Management Controller, Baseboard-Verwaltungscontroller) an, und starten Sie den in der Fehlermeldung angegebenen Host neu.<br><br>2. Führen Sie das Update erneut über den privilegierten Endpunkt durch. |
| Beim Durchführen eines Updates scheint der Updateprozess zu hängen und wird nach dem<br> Schritt „Step: Running step 2.4 – Install update“<br> des Plans mit den Updateaktionen nicht fortgesetzt.<br><br>Auf diesen Schritt folgt dann eine Reihe von Kopierprozessen für NUPKG-Dateien<br> auf die Dateifreigaben der internen Infrastruktur. Beispiel:<br><br>**Copying 1 files from content\PerfCollector\VirtualMachines to <br> \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\ <br>PerfCounterConfiguration**  | Das Problem wird verursacht, wenn zu viele Protokolldateien auf einem virtuellen Computer der Infrastruktur vorhanden sind und ein Problem mit dem SOFS (Scale-Out File Server, Dateiserver mit horizontaler Skalierung) von Windows Server besteht, der in einem nachfolgenden Update bereitgestellt wird. | Wenden Sie sich an den Microsoft-Kundendienst und -Support (CSS), um Hilfe zu erhalten. | 
| Beim Durchführen eines Updates<br> kann während des Schritts „Step: Running step 2.13.2 – Update<br> *VM-Name*“ des Plans mit den Updateaktionen ein Fehler der folgenden Art auftreten: (Der Name des virtuellen Computers<br> kann variieren.)<br><br>**ActionPlanInstanceWarning ece/MachineName:<br> WarningMessage:Task: Invocation of interface 'LiveUpdate' of<br> role 'Cloud\Fabric\WAS'failed: Type 'LiveUpdate' of Role<br> 'WAS' raised an exception: ERROR during storage<br> initialization: An error occurred while trying to make an API<br> call to Microsoft Storage service: {"Message": "A timeout<br> occurred while communicating with Service Fabric.<br> Exception Type: TimeoutException.<br> Exception message: Operation timed out."}**  | Das Problem wird durch eine E/A-Zeitüberschreitung in Windows Server verursacht, die in einem nachfolgenden Update behoben sein wird. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.
| Beim Durchführen eines Updates<br> kann während des Schritts „Step 21 Restart SQL server VMs“ ein Fehler der folgenden Art auftreten:<br><br>**Type 'LiveUpdateRestart' of Role 'VirtualMachines' raised an<br> exception: VerboseMessage:[VirtualMachines:LiveUpdateRestart]<br> Querying for VM MachineName-Sql01. - 10/13/2017 5:11:50 PM VerboseMessage:[VirtualMachines:LiveUpdateRestart]<br> VM is marked as HighlyAvailable. - 10/13/2017 5:11:50 PM<br> VerboseMessage:[VirtualMachines:LiveUpdateRestart] at<br>MS.Internal.ServerClusters.ExceptionHelp.Build at<br>MS.Internal.ServerClusters.ClusterResource.BeginTakeOffline<br>(Boolean force) at Microsoft.FailoverClusters.PowerShell.<br>StopClusterResourceCommand.BeginTimedOperation() at <br>Microsoft.FailoverClusters.PowerShell.TimedCmdlet.Wrapped<br>ProcessRecord() at Microsoft.FailoverClusters.PowerShell.<br>FCCmdlet.ProcessRecord() - 10/13/2017 5:11:50 PM Warning<br>Message: Task: Invocation of interface 'LiveUpdateRestart' of<br> role 'Cloud\Fabric\VirtualMachines' failed:** | Dieses Problem kann auftreten, wenn der virtuelle Computer nicht neu gestartet werden konnte. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.
| Beim Durchführen eines Updates kann ein Fehler der folgenden Art auftreten:<br><br>**2017-10-22T01:37:37.5369944Z Type 'Shutdown' of Role 'SQL'<br> raised an exception: An error occurred pausing node<br> 's45r1004-Sql01'.at Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2 <br>\Fabric\work\Applications\ EnterpriseCloud <br>EngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\ <br> CloudDeployment\Roles\SQL\SQL.psm1:line 542 at<br> Shutdown,C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\ <br>Applications \EnterpriseCloudEngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\Cloud<br>Deployment\Classes\SQL\SQL.psm1: line 50 at &#60;ScriptBlock&#62;,<br> &#60;No file>: line 18 at &#60;ScriptBlock&#62;, &#60;No file&#62;: line 16** | Dieses Problem kann auftreten, wenn der virtuelle Computer nicht in den angehaltenen Zustand versetzt werden kann, um die Rollen zu beseitigen. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.
| Beim Durchführen eines Updates kann einer der folgenden Fehler auftreten:<br><br>**Type 'Validate' of Role 'ADFS' raised an exception: Validation<br> for ADFS/Graph role failed with error: Error checking ADFS<br> probe endpoint *endpoint_URI*: Exception calling<br> "GetResponse" with "0" argument(s): "The remote server<br> returned an error: (503) Server Unavailable." at Invoke-<br>ADFSGraphValidation**<br><br>**Type 'Validate' of Role 'ADFS' raised an exception: Validation<br> for ADFS/Graph role failed with error: Error fetching<br> ADFS properties: Could not connect to <br>net.tcp://localhost:1500/policy. The connection attempt lasted<br> for a time span of 00:00:02.0498923. TCP error code<br> 10061: No connection could be made because the target<br> machine actively refused it 127.0.0.1:1500.<br> at Invoke-ADFSGraphValidation** | Der Plan mit den Updateaktionen kann die Integrität für Active Directory-Verbunddienste (AD FS) nicht überprüfen. | Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten.

## <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)

Dieser Abschnitt enthält bekannte Probleme, die für Build 20171020.1 nach der Installation auftreten können.

### <a name="portal"></a>Portal

- Unter Umständen ist es nicht möglich, Compute- oder Speicherressourcen im Administratorportal anzuzeigen. Es wird angegeben, dass während der Installation des Updates ein Fehler aufgetreten ist und dass das Update fälschlicherweise als erfolgreich gemeldet wurde. Wenden Sie sich an Microsoft CSS, um Hilfe zu erhalten, wenn dieses Problem auftritt.
- Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird. Wählen Sie zum Wiederherstellen des Dashboards oben rechts im Portal das Zahnradsymbol und dann die Option **Standardeinstellungen wiederherstellen**.
- Die Schaltfläche **Verschieben** ist deaktiviert, wenn Sie die Eigenschaften einer Ressourcengruppe anzeigen. Dies ist das erwartete Verhalten. Das Verschieben von Ressourcengruppen zwischen Abonnements wird derzeit nicht unterstützt.
- Für alle Workflows, für die Sie in einer Dropdownliste ein Abonnement, eine Ressourcengruppe oder einen Standort auswählen, können unter Umständen folgende Probleme auftreten:

   - Ggf. wird oben in der Liste eine leere Zeile angezeigt. Es sollte trotzdem möglich sein, ein Element auf normale Weise auszuwählen.
   - Wenn die Dropdownliste mit den Elementen kurz ist, ist es ggf. nicht möglich, die Elementnamen anzuzeigen.
   - Wenn Sie über mehrere Benutzerabonnements verfügen, kann die Dropdownliste für die Ressourcengruppen leer sein. 

   Zum Umgehen der beiden letztgenannten Probleme können Sie den Namen des Abonnements oder der Ressourcengruppe eingeben (falls bekannt), oder Sie können stattdessen PowerShell verwenden.
- Das Löschen von Benutzerabonnements führt zu verwaisten Ressourcen. Eine Problemumgehung besteht darin, zuerst Benutzerressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Benutzerabonnements zu löschen.
- Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen. Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
  
### <a name="backup"></a>Backup 

- Belassen Sie die Infrastruktursicherung auf dem Blatt **Infrastructure backup** (Infrastruktursicherung) im deaktivierten Zustand.

### <a name="health-and-monitoring"></a>Integrität und Überwachung

- Wenn Sie eine Infrastrukturrolleninstanz neu starten, wird ggf. eine Meldung mit dem Hinweis angezeigt, dass der Neustart nicht erfolgreich war. Der Neustart wurde aber erfolgreich durchgeführt.

### <a name="marketplace"></a>Marketplace
- Wenn Sie versuchen, dem Azure Stack-Marketplace mit der Option **Add from Azure** (Aus Azure hinzufügen) Elemente hinzuzufügen, werden unter Umständen nicht alle Elemente zum Herunterladen angezeigt.
- Benutzer können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt. Diese Elemente sind für Benutzer nicht funktional.

### <a name="compute"></a>Compute
- Benutzer haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen. Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt.
- Sie können eine VM-Verfügbarkeitsgruppe nur mit einer Fehlerdomäne und einer Updatedomäne konfigurieren.
- Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden. Sie können eine Skalierungsgruppe erstellen, indem Sie eine Vorlage verwenden.
- Die Skalierungseinstellungen für Skalierungsgruppen für virtuelle Computer sind im Portal nicht verfügbar. Dieses Problem können Sie umgehen, indem Sie [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set) verwenden. Aufgrund der Versionsunterschiede bei PowerShell müssen Sie den `-Name`-Parameter statt des `-VMScaleSetName`-Parameters verwenden.
 
### <a name="networking"></a>Netzwerk
- Sie können im Portal keinen Lastenausgleich mit einer öffentlichen IP-Adresse erstellen. Zum Umgehen dieses Problems können Sie den Lastenausgleich mit PowerShell erstellen.
- Beim Erstellen eines Netzwerklastenausgleichs müssen Sie eine NAT-Regel (Network Address Translation, Netzwerkadressübersetzung) erstellen. Andernfalls wird ein Fehler ausgegeben, wenn Sie versuchen, eine NAT-Regel hinzufügen, nachdem der Lastenausgleich erstellt wurde.
- Sie können die Zuordnung einer öffentlichen IP-Adresse zu einem virtuellen Computer nicht aufheben, nachdem der virtuelle Computer erstellt und dieser IP-Adresse zugeordnet wurde. Die Aufhebung der Zuordnung war scheinbar erfolgreich, die zuvor zugewiesene öffentliche IP-Adresse bleibt dem ursprünglichen virtuellen Computer jedoch zugeordnet. Dieses Verhalten tritt auch dann auf, wenn Sie die IP-Adresse einem neuen virtuellen Computer neu zuordnen (gewöhnlich als *VIP-Austausch* bezeichnet). Alle künftigen Versuche, eine Verbindung über diese IP-Adresse herzustellen, führen dazu, dass eine Verbindung mit dem ursprünglich zugeordneten virtuellen Computer (und nicht mit dem neuen) hergestellt wird. Derzeit müssen Sie ausschließlich neue öffentliche IP-Adressen für die Erstellung neuer virtueller Computer verwenden.
 
### <a name="sqlmysql"></a>SQL/MySQL
- Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
- Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.
 
### <a name="app-service"></a>App Service
- Ein Benutzer muss den Speicherressourcenanbieter vor dem Erstellen seiner ersten Azure-Funktion im Abonnement registrieren.
 
### <a name="field-replaceable-unit-fru-procedures"></a>FRU-Verfahren (Field Replaceable Unit)

- Während des Aktualisierungslaufs werden Offlineimages nicht gepatcht. Wenn Sie den Knoten einer Skalierungseinheit ersetzen müssen, sollten Sie zusammen mit Ihrem OEM-Hardwareanbieter sicherstellen, dass der ersetzte Knoten über die aktuelle Patchebene verfügt.

## <a name="download-the-update"></a>Herunterladen des Updates

Sie können das Paket mit Update 1710 [hier](https://aka.ms/azurestackupdatedownload) herunterladen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Updateverwaltung in Azure Stack finden Sie unter [Übersicht zum Verwalten von Updates in Azure Stack](azure-stack-updates.md).
- Informationen zur Anwendung von Updates finden Sie unter [Anwenden von Updates in Azure Stack](azure-stack-apply-updates.md).