---
title: Diagnose in Azure Stack
description: "Es wird beschrieben, wie Sie Protokolldateien für die Diagnose in Azure Stack sammeln."
services: azure-stack
author: adshar
manager: byronr
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 10/2/2017
ms.author: adshar
ms.openlocfilehash: 9b1fbbf63ddd8bac2c1a76bbcd5daca69e2513f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack-Diagnosetools

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*
 
Bei Azure Stack handelt es sich um eine große Sammlung von Komponenten, die zusammenarbeiten und miteinander interagieren. Alle diese Komponenten generieren eigene eindeutige Protokolle. Dies kann die Diagnose von Problemen zu einer Herausforderung machen – insbesondere bei Fehlern, die von verschiedenen interagierenden Azure Stack-Komponenten stammen. 

Mit unseren Diagnosetools wird sichergestellt, dass der Mechanismus für die Protokollsammlung einfach und effizient ist. Im folgenden Diagramm ist dargestellt, wie Tools zum Sammeln von Protokollen in Azure Stack funktionieren:

![Tools für die Protokollsammlung](media/azure-stack-diagnostics/image01.png)
 
 
## <a name="trace-collector"></a>Collector der Ablaufverfolgung
 
Der Collector der Ablaufverfolgung ist standardmäßig aktiviert. Er wird fortlaufend im Hintergrund ausgeführt und sammelt von den Komponentendiensten in Azure Stack Protokolle vom Typ „Ereignisablaufverfolgung für Windows“ (Event Tracing for Windows, ETW) und speichert sie auf einer gemeinsamen lokalen Freigabe. 

Hier sind einige wichtige Informationen zum Collector der Ablaufverfolgung aufgeführt:
 
* Der Collector der Ablaufverfolgung wird fortlaufend mit Standardgrößenbeschränkungen ausgeführt. Die zulässige maximale Standardgröße jeder Datei (200 MB) ist **keine** endgültige Größe. Die Größe wird regelmäßig überprüft (derzeit alle 2 Minuten), und wenn die aktuelle Datei größer als 200 MB ist, wird sie gespeichert, und es wird eine neue Datei generiert. Außerdem gilt ein (konfigurierbarer) Grenzwert von 8 GB für die Gesamtdateigröße, die pro Ereignissitzung generiert werden kann. Nachdem dieser Grenzwert erreicht wurde, werden die ältesten Dateien gelöscht, wenn neue Dateien erstellt werden.
* Für die Protokolle gilt ein Altersgrenzwert von fünf Tagen. Dieser Grenzwert kann ebenfalls konfiguriert werden. 
* Jede Komponente definiert die Eigenschaften für die Ablaufverfolgungskonfiguration mit einer JSON-Datei. Die JSON-Dateien werden unter `C:\TraceCollector\Configuration` gespeichert. Bei Bedarf können diese Dateien bearbeitet werden, um die Grenzwerte für Alter und Größe für die gesammelten Protokolle zu ändern. Für Änderungen an diesen Dateien ist ein Neustart des Diensts *Microsoft Azure Stack-Collector für die Ablaufverfolgung* erforderlich, damit die Änderungen wirksam werden.
* Das folgende Beispiel ist eine JSON-Datei für die Konfiguration der Ablaufverfolgung für FabricRingServices Operations über die XRP-VM: 

```
{
    "LogFile": 
    {
        "SessionName": "FabricRingServicesOperationsLogSession",
        "FileName": "\\\\SU1FileServer\\SU1_ManagementLibrary_1\\Diagnostics\\FabricRingServices\\Operations\\AzureStack.Common.Infrastructure.Operations.etl",
        "RollTimeStamp": "00:00:00",
        "MaxDaysOfFiles": "5",
        "MaxSizeInMB": "200",
        "TotalSizeInMB": "5120"
    },
    "EventSources":
    [
        {"Name": "Microsoft-AzureStack-Common-Infrastructure-ResourceManager" },
        {"Name": "Microsoft-OperationManager-EventSource" },
        {"Name": "Microsoft-Operation-EventSource" }
    ]
}
```

* **MaxDaysOfFiles**

    Mit diesem Parameter wird das Alter der beizubehaltenden Dateien gesteuert. Ältere Protokolldateien werden gelöscht.
* **MaxSizeInMB**

    Mit diesem Parameter wird der Größenschwellenwert für eine einzelne Datei gesteuert. Wenn die jeweilige Größe erreicht ist, wird eine neue ETL-Datei erstellt.
* **TotalSizeInMB**

    Mit diesem Parameter wird die Gesamtgröße der ETL-Dateien gesteuert, die für eine Ereignissitzung generiert werden. Wenn die Gesamtdateigröße größer als dieser Parameterwert ist, werden ältere Dateien gelöscht.
  
## <a name="log-collection-tool"></a>Tool für die Protokollsammlung
 
Der PowerShell-Befehl `Get-AzureStackLog` kann verwendet werden, um Protokolle für alle Komponenten einer Azure Stack-Umgebung zu sammeln. Sie werden an einem vom Benutzer angegebenen Ort in ZIP-Dateien gespeichert. Wenn unser Team für den technischen Support Ihre Protokolle für die Behandlung eines Problems benötigt, werden Sie unter Umständen gebeten, dieses Tool auszuführen.

> [!CAUTION]
> Es kann sein, dass diese Protokolldateien personenbezogene Informationen enthalten. Berücksichtigen Sie dies, bevor Sie Protokolldateien öffentlich bereitstellen.
 
Es folgen einige Beispiele für die gesammelten Protokolltypen:
*   **Azure Stack-Bereitstellungsprotokolle**
*   **Windows-Ereignisprotokolle**
*   **Panther-Protokolle**

   Für die Behandlung von Problemen bei der VM-Erstellung:
*   **Clusterprotokolle**
*   **Speicherdiagnoseprotokolle**
*   **ETW-Protokolle**

Diese Dateien werden vom Collector für die Ablaufverfolgung gesammelt und auf einer Freigabe gespeichert, auf der sie von `Get-AzureStackLog` abgerufen werden.
 
**So führen Sie Get-AzureStackLog in einem System mit Azure Stack Development Kit (ASDK) aus**
1.  Melden Sie sich auf dem Host als „AzureStack\AzureStackAdmin“ an.
2.  Öffnen Sie ein PowerShell-Fenster als Administrator.
3.  Führen Sie `Get-AzureStackLog`aus.  

    **Beispiele**

    - Sammeln Sie alle Protokolle für alle Rollen:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs`

    - Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal`

    - Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal, und verwenden Sie die Datumsfilterung für Protokolldateien für die letzten acht Stunden:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)`

    - Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal, und verwenden Sie für die Datumsfilterung der Protokolldateien den Zeitraum zwischen acht und zwei Stunden vor dem aktuellen Zeitpunkt:

      `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)`

**So führen Sie Get-AzureStackLog in einem integrierten Azure Stack-System aus**

Um das Protokollerfassungstool in einem integrierten System auszuführen, benötigen Sie Zugriff auf den privilegierten Endpunkt (PEP). Das folgende Beispielskript können Sie mit dem PEP ausführen, um Protokolle in einem integrierten System zu sammeln:

```
$ip = "<IP OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDREESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- Geben Sie beim Sammeln von Protokollen vom PEP für den `OutputPath`-Parameter einen Speicherort auf dem HLH-Computer an. Stellen Sie außerdem sicher, dass der Speicherort verschlüsselt ist.
- Die Parameter `OutputSharePath` und `OutputShareCredential` sind optional und werden verwendet, wenn Sie Protokolle in einen externen freigegebenen Ordner hochladen. Verwenden Sie diese Parameter *zusätzlich* zu `OutputPath`. Wenn `OutputPath` nicht angegeben ist, verwendet das Protokollerfassungstool das Systemlaufwerk der PEP-VM für die Speicherung. Dies kann zu einem Fehler beim Skript führen, da der Speicherplatz auf dem Laufwerk begrenzt ist.
- Wie im vorherigen Beispiel gezeigt, können die Parameter `FromDate` und `ToDate` zum Sammeln von Protokollen für einen bestimmten Zeitraum verwendet werden. Dies kann für Szenarien wie das Sammeln von Protokollen nach dem Anwenden von Updatepaketen in einem integrierten System hilfreich sein.

**Überlegungen zu den Parametern für das ASDK und integrierte Systeme:**

- Wenn die Parameter `FromDate` und `ToDate` nicht angegeben sind, werden standardmäßig Protokolle für die letzten vier Stunden gesammelt.
- Sie können den `TimeOutInMinutes`-Parameter verwenden, um das Timeout für die Sammlung von Protokollen festzulegen. Es ist standardmäßig auf 150 (2,5 Stunden) festgelegt.

- Derzeit können Sie den Parameter `FilterByRole` verwenden, um die Protokollsammlung nach den folgenden Rollen zu filtern:

   |   |   |   |
   | - | - | - |
   | `ACSMigrationService`     | `ACSMonitoringService`   | `ACSSettingsService` |
   | `ACS`                     | `ACSFabric`              | `ACSFrontEnd`        |
   | `ACSTableMaster`          | `ACSTableServer`         | `ACSWac`             |
   | `ADFS`                    | `ASAppGateway`           | `BareMetal`          |
   | `BRP`                     | `CA`                     | `CPI`                |
   | `CRP`                     | `DeploymentMachine`      | `DHCP`               |
   |`Domain`                   | `ECE`                    | `ECESeedRing`        |        
   | `FabricRing`              | `FabricRingServices`     | `FRP`                |
   |` Gateway`                 | `HealthMonitoring`       | `HRP`                |               
   | `IBC`                     | `InfraServiceController` | `KeyVaultAdminResourceProvider`|
   | `KeyVaultControlPlane`    | `KeyVaultDataPlane`      | `NC`                 |            
   | `NonPrivilegedAppGateway` | `NRP`                    | `SeedRing`           |
   | `SeedRingServices`        | `SLB`                    | `SQL`                |     
   | `SRP`                     | `Storage`                | `StorageController`  |
   | `URP`                     | `UsageBridge`            | `VirtualMachines`    |  
   | `WAS`                     | `WASPUBLIC`              | `WDS`                |


Weitere wichtige Informationen:

* Je nachdem, welche Rollen in den Protokollen gesammelt werden, kann dieser Befehl mehr Zeit beanspruchen. Zu den entscheidenden Faktoren gehören auch der Zeitraum, der für die Protokollsammlung angegeben wurde, und die Anzahl von Knoten in der Azure Stack-Umgebung.
* Überprüfen Sie nach Abschluss der Protokollsammlung den neuen Ordner, der mit dem Parameter `-OutputPath` des Befehls erstellt wird.
* Für jede Rolle sind die Protokolle in separaten ZIP-Dateien enthalten. Abhängig von der Größe der gesammelten Protokolle kann das Protokoll einer Rolle möglicherweise in mehrere ZIP-Dateien aufgeteilt werden. Wenn Sie alle Protokolldateien in denselben Ordner entzippen möchten, verwenden Sie für eine solche Rolle ein Tool, das den Entzipvorgang in einer Sammeloperation durchführen kann (z.B. 7-Zip). Wählen Sie alle ZIP-Dateien für die Rolle und dann **Hier entpacken** aus. Damit werden die Protokolldateien für diese Rolle in einem zusammengeführten Ordner entzippt.
* Eine Datei namens `Get-AzureStackLog_Output.log` wird ebenfalls in dem Ordner mit den gezippten Protokolldateien erstellt. Diese Datei ist ein Protokoll der Befehlsausgabe, die Sie zum Beheben von Problemen während der Protokollsammlung verwenden können.
* Zum Untersuchen eines bestimmten Fehlers werden unter Umständen die Protokolle von mehr als einer Komponente benötigt.
    -   System- und Ereignisprotokolle für alle Infrastruktur-VMs werden unter der Rolle *VirtualMachines* gesammelt.
    -   System- und Ereignisprotokolle für alle Hosts werden unter der Rolle *BareMetal* gesammelt.
    -   Failovercluster- und Hyper-V-Ereignisprotokolle werden unter der Rolle *Storage* gesammelt.
    -   ACS-Protokolle werden unter den Rollen *Storage* und *ACS* gesammelt.

> [!NOTE]
> Größen- und Altersgrenzwerte für die gesammelten Protokolle werden erzwungen, da es sehr wichtig ist, für eine effiziente Nutzung Ihres Speicherplatzes zu sorgen. So wird sichergestellt, dass keine übermäßig hohe Anzahl von Protokollen gespeichert wird. Allerdings werden beim Diagnostizieren von Problemen manchmal Protokolle benötigt, die aufgrund dieser Grenzwerte möglicherweise nicht mehr vorhanden sind. Daher wird **dringend empfohlen**, Ihre Protokolle alle acht bis zwölf Stunden in einen externen Speicher zu verlagern (Speicherkonto in Azure, zusätzliches lokales Speichergerät usw.) und je nach Ihren Anforderungen dort ein bis drei Monate lang aufzubewahren. Stellen Sie außerdem sicher, dass der Speicherort verschlüsselt ist.

## <a name="next-steps"></a>Nächste Schritte
[Microsoft Azure Stack troubleshooting (Problembehandlung für Microsoft Azure Stack)](azure-stack-troubleshooting.md)
