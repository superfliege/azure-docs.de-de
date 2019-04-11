---
title: Azure Service Fabric-Anwendung für die Patchorchestrierung für Linux | Microsoft-Dokumentation
description: Anwendung zum Automatisieren von Betriebssystempatches in einem Linux-Service Fabric-Cluster.
services: service-fabric
documentationcenter: .net
author: novino
manager: chackdan
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/22/2018
ms.author: nachandr
ms.openlocfilehash: 5efcc92bc2054dfb66b5fe03ae083c49f924d2ce
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58668193"
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Patchen des Linux-Betriebssystems in Ihrem Service Fabric-Cluster

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

Die Anwendung für die Patchorchestrierung ist eine Azure Service Fabric-Anwendung, mit der das Aufspielen von Betriebssystempatches in einem Service Fabric-Cluster ohne Ausfallzeiten automatisiert werden kann.

Die App für die Patchorchestrierung umfasst die folgenden Features:

- **Automatische Installation von Betriebssystemupdates**. Betriebssystemupdates werden automatisch heruntergeladen und installiert. Clusterknoten werden bei Bedarf ohne Ausfall des Clusters neu gestartet.

- **Clusterfähiges Patchen und Integration von Integrität**. Beim Anwenden von Updates überwacht die App für die Patchorchestrierung die Integrität der Clusterknoten. Knoten im Cluster werden einzeln oder in Upgradedomänen aktualisiert. Wenn die Integrität des Clusters aufgrund des Patchingprozesses abnimmt, wird das Patchen angehalten, um eine Verschärfung des Problems zu verhindern.

## <a name="internal-details-of-the-app"></a>Interne Details der App

Die App für die Patchorchestrierung besteht aus den folgenden Teilkomponenten:

- **Koordinatordienst:** Dieser zustandsbehaftete Dienst ist für Folgendes zuständig:
    - Koordinieren der Betriebssystemupdate-Aufträge im gesamten Cluster.
    - Speichern der Ergebnisse der abgeschlossenen Betriebssystemupdate-Vorgänge.
- **Knoten-Agent-Dienst:** Dieser zustandslose Dienst wird auf allen Service Fabric-Clusterknoten ausgeführt. Der Dienst ist für Folgendes zuständig:
    - Bootstrapping des Knoten-Agent-Daemon unter Linux.
    - Überwachen des Daemondiensts.
- **Knoten-Agent-Daemon**: Dieser Linux-Daemondienst wird mit Berechtigungen einer höheren Ebene ausgeführt (root). Im Gegensatz dazu werden der Knoten-Agent-Dienst und der Koordinatordienst mit niedrigeren Berechtigungen ausgeführt. Der Dienst ist für die Ausführung der folgenden Updateaufträge auf allen Clusterknoten zuständig:
    - Deaktivieren automatischer Betriebssystemupdates auf dem Knoten
    - Herunterladen und Installieren des Betriebssystemupdates entsprechend der vom Benutzer bereitgestellten Richtlinie
    - Gegebenenfalls Neustarten des Computers nach der Installation des Betriebssystemupdates
    - Hochladen der Ergebnisse des Betriebssystemupdates in den Koordinatordienst
    - Senden von Integritätsberichten, falls der Vorgang nach allen Wiederholungsversuchen fehlschlägt.

> [!NOTE]
> Die App für die Patchorchestrierung verwendet den Reparatur-Manager-Systemdienst in Service Fabric, um die Knoten zu aktivieren und zu deaktivieren und um Integritätsprüfungen durchzuführen. Der von der App für die Patchorchestrierung erstellte Reparaturtask verfolgt den Updatefortschritt für jeden Knoten nach.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Sicherstellen, dass die virtuellen Azure-Computer unter Ubuntu 16.04 ausgeführt werden
Zum Zeitpunkt der Abfassung dieses Dokuments ist Ubuntu 16.04 (`Xenial Xerus`) die einzige unterstützte Version.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-62x-and-above"></a>Sicherstellen, dass Version 6.2.x oder höher des Service Fabric-Linux-Clusters verwendet wird

Die App für die Patchorchestrierung für Linux verwendet bestimmte Features der Runtime, die nur in der Service Fabric-Runtimeversion 6.2.x und höher verfügbar sind.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Aktivieren des Reparatur-Manager-Diensts (falls dieser nicht bereits ausgeführt wird)

Für die App für die Patchorchestrierung muss der Reparatur-Manager-Systemdienst im Cluster aktiviert sein.

#### <a name="azure-clusters"></a>Azure-Cluster

In Azure-Linux-Clustern auf Dauerhaftigkeitsstufe „Silver“ und „Gold“ ist der Reparatur-Manager-Dienst standardmäßig aktiviert. In Azure-Clustern auf Dauerhaftigkeitsstufe „Bronze“ ist der Reparatur-Manager-Dienst standardmäßig nicht aktiviert. Wenn der Dienst bereits aktiviert ist, wird er im Service Fabric Explorer im Abschnitt mit den Systemdiensten aufgeführt.

##### <a name="azure-portal"></a>Azure-Portal
Sie können den Reparatur-Manager beim Einrichten des Clusters über das Azure-Portal aktivieren. Wählen Sie bei der Clusterkonfiguration unter **Add-On-Features** die Option **Reparatur-Manager einschließen** aus.
![Abbildung zur Aktivierung des Reparatur-Managers über das Azure-Portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager-Bereitstellungsmodell
Alternativ können Sie den Reparatur-Manager-Dienst für neue und vorhandene Service Fabric-Cluster über das [Azure Resource Manager-Bereitstellungsmodell](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) aktivieren. Rufen Sie die Vorlage für den Cluster ab, den Sie bereitstellen möchten. Sie können entweder die Beispielvorlagen verwenden oder eine benutzerdefinierte Vorlage für das Azure Resource Manager-Bereitstellungsmodell erstellen. 

So aktivieren Sie den Reparatur-Manager-Dienst mithilfe einer [Vorlage für das Azure Resource Manager-Bereitstellungsmodell](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm)

1. Überprüfen Sie zunächst, ob `apiversion` für die Ressource `Microsoft.ServiceFabric/clusters` auf `2017-07-01-preview` festgelegt ist. Wenn eine andere Einstellung vorliegt, müssen Sie `apiVersion` auf den Wert `2017-07-01-preview` oder höher aktualisieren:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Aktivieren Sie dann den Reparatur-Manager-Dienst, indem Sie folgenden `addonFeatures`-Abschnitt nach dem Abschnitt `fabricSettings` hinzufügen:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Nachdem Sie die Clustervorlage mit diesen Änderungen aktualisiert haben, wenden Sie die Änderungen an, und warten Sie, bis das Upgrade abgeschlossen wurde. Sie sehen nun, dass der Reparatur-Manager-Systemdienst im Cluster ausgeführt wird. Der Name im Abschnitt für Systemdienste im Service Fabric Explorer lautet`fabric:/System/RepairManagerService`. 

### <a name="standalone-on-premises-clusters"></a>Eigenständige lokale Cluster

Eigenständige Service Fabric-Linux-Cluster werden zum Zeitpunkt der Abfassung dieses Dokuments nicht unterstützt.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Deaktivieren automatischer Betriebssystemupdates auf allen Knoten

Automatische Betriebssystemupdates können zum Verlust der Verfügbarkeit oder einer Änderung des Verhaltens der ausgeführten Anwendungen führen. Die App für die Patchorchestrierung versucht standardmäßig, das automatische Betriebssystemupdate auf jedem Clusterknoten zu deaktivieren, um derartige Szenarien zu verhindern.
Für Ubuntu wird [unattended-upgrades](https://help.ubuntu.com/community/AutomaticSecurityUpdates) von der App für die Patchorchestrierung deaktiviert.

## <a name="download-the-app-package"></a>Herunterladen des App-Pakets

Die Anwendung kann zusammen mit Installationsskripts über den [Archivlink](https://go.microsoft.com/fwlink/?linkid=867984) heruntergeladen werden.

Die Anwendung im SFPKG-Format kann über den [SFPKG-Link](https://aka.ms/POA/POA_v2.0.2.sfpkg) heruntergeladen werden. Dies ist praktisch für die [Azure Resource Manager-basierte Anwendungsbereitstellung](service-fabric-application-arm-resource.md).

## <a name="configure-the-app"></a>Konfigurieren der App

Das Verhalten der App für die Patchorchestrierung kann Ihren Anforderungen entsprechend konfiguriert werden. Überschreiben Sie die Standardwerte, indem Sie während der Erstellung bzw. Aktualisierung einer Anwendung den Anwendungsparameter übergeben. Anwendungsparameter können durch Angeben von `ApplicationParameter` in den Cmdlets `Start-ServiceFabricApplicationUpgrade` oder `New-ServiceFabricApplication` festgelegt werden.

|**Parameter**        |**Typ**                          | **Details**|
|:-|-|-|
|MaxResultsToCache    |Long                              | Maximale Anzahl von Updateergebnissen, die zwischengespeichert werden sollen. <br>Der Standardwert ist 3000, wobei Folgendes angenommen wird: <br> – Es sind 20 Knoten vorhanden. <br> – Jeden Monat können fünf Updates auf einem Knoten erfolgen. <br> – Pro Vorgang können zehn Ergebnisse vorliegen. <br> – Es sollen die Ergebnisse für die letzten drei Monaten gespeichert werden. |
|TaskApprovalPolicy   |Enum <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy gibt die Richtlinie an, die vom Koordinatordienst zum Installieren von Updates auf den Service Fabric-Clusterknoten verwendet werden soll.<br>                         Zulässige Werte sind: <br>                                                           <b>NodeWise</b>. Updates werden immer nur auf jeweils einem Knoten installiert. <br>                                                           <b>UpgradeDomainWise</b>. Updates werden immer nur in jeweils einer Upgradedomäne installiert. (Höchstens alle Knoten in einer Upgradedomäne können ein Update verwenden.)
| UpdateOperationTimeOutInMinutes | Int <br>(Standard: 180)                   | Gibt den Timeoutwert für jeden Updatevorgang an (Herunterladen oder Installieren). Wenn der Vorgang nicht innerhalb des angegebenen Timeoutzeitraums abgeschlossen ist, wird er abgebrochen.       |
| RescheduleCount      | Int <br> (Standard: 5)                  | Gibt an, wie oft der Dienst das Betriebssystemupdate maximal erneut plant, falls bei einem Vorgang wiederholt ein Fehler auftritt.          |
| RescheduleTimeInMinutes  | Int <br>(Standard: 30) | Das Intervall, nach dem der Dienst das Betriebssystemupdate erneut plant, falls der Fehler weiterhin besteht. |
| UpdateFrequency           | Durch Trennzeichen getrennte Zeichenfolge (Standard: „Wöchentlich, Mittwoch, 7:00:00“)     | Die Häufigkeit, mit der Betriebssystemupdates im Cluster installiert werden sollen. Folgende Formate und Werte sind möglich: <br>– Monatlich, TT, HH:MM:SS, z.B. Monatlich, 5, 12:22:32 <br> – Wöchentlich, TAG, HH:MM:SS, z.B. Wöchentlich, Dienstag, 12:22:32  <br> – Täglich, HH:MM:SS, z.B. Täglich, 12:22:32.  <br> – Keine: Gibt an, dass kein Update durchgeführt werden soll.  <br><br> Alle Uhrzeiten werden in UTC angegeben.|
| UpdateClassification | Durch Trennzeichen getrennte Zeichenfolge (Standard: „securityupdates“) | Typ der Updates, die auf den Clusterknoten installiert werden sollen. Zulässige Werte sind „securityupdates“ und „all“. <br> – securityupdates: Es werden nur Sicherheitsupdates installiert. <br> – all: Es werden alle Updates von apt installiert.|
| ApprovedPatches | Durch Trennzeichen getrennte Zeichenfolge (Standard: "") | Dies ist die Liste der genehmigten Updates, die auf den Clusterknoten installiert werden sollen. Die durch Trennzeichen getrennte Liste enthält genehmigte Pakete und optional die gewünschte Zielversion.<br> Beispiel: "apt-utils = 1.2.10ubuntu1, python3-jwt, apt-transport-https < 1.2.194, libsystemd0 >= 229-4ubuntu16" <br> Damit wird Folgendes installiert: <br> – apt-utils mit Version 1.2.10ubuntu1, sofern die Version in apt-cache verfügbar ist. Wenn die betreffende Version nicht verfügbar ist, wird kein Vorgang durchgeführt. <br> – python3-jwt führt Upgrades auf die neueste verfügbare Version durch. Wenn das Paket nicht vorhanden ist, wird kein Vorgang durchgeführt. <br> – apt-transport-https führt Upgrades auf die höchste Version unter 1.2.194 durch. Wenn diese Version nicht vorhanden ist, wird kein Vorgang durchgeführt. <br> – libsystemd0 führt Upgrades auf die höchste Version durch, die größer oder gleich 229-4ubuntu16 ist. Wenn eine solche Version nicht vorhanden ist, wird kein Vorgang durchgeführt.|
| RejectedPatches | Durch Trennzeichen getrennte Zeichenfolge (Standard: "") | Dies ist die Liste der Updates, die auf den Clusterknoten nicht installiert werden sollen. <br> Beispiel: "bash, sudo" <br> In diesem Beispiel wird „bash, sudo“ gefiltert und empfängt keine Updates. |


> [!TIP]
> Wenn das Betriebssystemupdate sofort ausgeführt werden soll, legen Sie `UpdateFrequency` relativ zum Zeitpunkt der Anwendungsbereitstellung fest. Angenommen, Sie haben einen Testcluster mit fünf Knoten und möchten die App ca. um 17:00 Uhr (UTC) bereitstellen. Wenn Sie davon ausgehen, dass das Upgrade oder die Bereitstellung der Anwendung maximal 30 Minuten dauert, legen Sie UpdateFrequency auf „Täglich, 17:30:00“ fest.

## <a name="deploy-the-app"></a>Bereitstellen der App

1. Führen Sie alle erforderlichen Schritte zur Vorbereitung des Clusters aus.
2. Stellen Sie die App für die Patchorchestrierung wie jede andere Service Fabric-App bereit. Sie können die App mit PowerShell oder der Azure Service Fabric-Befehlszeilenschnittstelle bereitstellen. Führen Sie die Schritte unter [Bereitstellen und Entfernen von Anwendungen mit PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) oder [Bereitstellen einer Anwendung mit Azure Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application) aus.
3. Übergeben Sie zum Konfigurieren der Anwendung zum Zeitpunkt der Bereitstellung `ApplicationParameter` an das Cmdlet `New-ServiceFabricApplication` oder die bereitgestellten Skripts. Zur Vereinfachung werden ein PowerShell-Skript („Deploy.ps1“) und ein Bash-Skript („Deploy.sh“) zusammen mit der Anwendung bereitgestellt. So verwenden Sie das Skript

    - Stellen Sie eine Verbindung mit einem Service Fabric-Cluster her.
    - Führen Sie das Deploy-Skript aus. Optional können Sie den Anwendungsparameter an das Skript übergeben. Beispiel: .\Deploy.ps1 -ApplicationParameter @{ UpdateFrequency = "Daily, 11:00:00"} ODER ./Deploy.sh "{\"UpdateFrequency\":\"Daily, 11:00:00\"}" 

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner „PatchOrchestrationApplication“ im gleichen Verzeichnis.

## <a name="upgrade-the-app"></a>Aktualisieren der App

Um eine vorhandene App für die Patchorchestrierung zu aktualisieren, führen Sie die Schritte unter [Service Fabric-Anwendungsupgrade mithilfe von PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) oder [Service Fabric-Anwendungsupgrade mithilfe von Azure Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade) aus.

## <a name="remove-the-app"></a>Entfernen der App

Führen Sie die Schritte unter [Bereitstellen und Entfernen von Anwendungen mit PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) oder [Entfernen einer Anwendung mit Azure Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete) aus, um die Anwendung zu entfernen.

Zur Vereinfachung werden ein PowerShell-Skript („Undeploy.ps1“) und ein Bash-Skript („Undeploy.sh“) zusammen mit der Anwendung bereitgestellt. So verwenden Sie das Skript

  - Stellen Sie eine Verbindung mit einem Service Fabric-Cluster her.
  - Führen Sie das Skript „Undeploy.ps1“ oder „Undeploy.sh“ aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner „PatchOrchestrationApplication“ im gleichen Verzeichnis.

## <a name="view-the-update-results"></a>Anzeigen der Updateergebnisse

Die App für die Patchorchestrierung macht eine REST-API verfügbar, um dem Benutzer die Verlaufsergebnisse anzuzeigen. Im Folgenden ist ein Beispielergebnis aufgeführt: ```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```
```json
[ 
  { 
    "NodeName": "_bronze_0", 
    "UpdateOperationResults": [ 
      { 
        "OperationResult": "succeeded", 
        "NodeName": "_bronze_0", 
        "OperationTime": "2017-11-21T12:39:29.0435917Z", 
        "UpdateDetails": [ 
          { 
            "UpdateId": "linux-cloud-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-headers-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-image-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "python3-apport:amd64=2.20.1-0ubuntu2.13", 
            "ResultCode": "succeeded" 
          }, 
        ], 
        "OperationType": "installation", 
        "UpdateClassification": "securityupdates", 
        "UpdateFrequency": "Daily, 7:00:00", 
        "RebootRequired": true, 
        "ApprovedList": "", 
        "RejectedList": "" 
      } 
    ] 
  } 
] 
```

Felder des JSON werden wie folgt beschrieben:

Feld | Werte | Details
-- | -- | --
OperationResult | 0: Erfolgreich<br> 1: Erfolgreich mit Fehlern<br> 2: Fehlgeschlagen<br> 3 : Abgebrochen<br> 4: Abgebrochen mit Timeout | Gibt das Ergebnis des Gesamtvorgangs an (schließt normalerweise die Installation eines oder mehrerer Updates ein).
ResultCode | Das Gleiche wie bei OperationResult | Diese Feld gibt das Ergebnis eines Installationsvorgangs für ein einzelnes Update an.
OperationType | 1: Installation<br> 0: Suchen und Herunterladen| „Installation“ ist der einzige Wert für OperationType, der standardmäßig in den Ergebnissen angezeigt wird.
UpdateClassification | Standardwert ist „securityupdates“. | Typ der Updates, die während des Updatevorgangs installiert werden.
UpdateFrequency | Standardwert ist „Weekly, Wednesday, 7:00:00“. | Konfigurierte Updatehäufigkeit für das Update.
RebootRequired | TRUE: Neustart war erforderlich<br> FALSE: Neustart war nicht erforderlich | Gibt an, ob ein Neustart erforderlich war, um die Installation des Updates abzuschließen.
ApprovedList | Standardwert ist "". | Liste der genehmigten Patches für das Update
RejectedList | Standardwert ist "". | Liste der abgelehnten Patches für das Update

Wenn noch keine Aktualisierung geplant ist, ist die JSON-Ausgabe leer.

Melden Sie sich beim Cluster an, um Updateergebnisse abzufragen. Ermitteln Sie dann die Replikatadresse des primären Knotens für den Koordinatordienst, und öffnen Sie diese URL im Browser: http://&lt;REPLIKATS-IP&gt;:&lt;Anwendungsport&gt;/PatchOrchestrationApplication/v1/GetResults.

Der REST-Endpunkt für den Koordinatordienst verfügt über einen dynamischen Port. Die genaue URL finden Sie im Service Fabric Explorer. Die Ergebnisse sind beispielsweise unter `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults` verfügbar.

![Abbildung des REST-Endpunkts](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>Diagnose/Integritätsereignisse

### <a name="diagnostic-logs"></a>Diagnoseprotokolle

Protokolle für die App für die Patchorchestrierung werden als Teil der Service Fabric-Laufzeitprotokolle erfasst.

Hinweis für Benutzer, die Protokolle über das gewünschte Diagnosetool/die gewünschte Diagnosepipeline erfassen möchten: Die Anwendung für die Patchorchestrierung protokolliert Ereignisse über [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0) unter Verwendung der folgenden festen Anbieter-IDs:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Integritätsberichte

Die App für die Patchorchestrierung veröffentlicht in folgenden Fällen auch Integritätsberichte für den Koordinatordienst oder den Knoten-Agent-Dienst:

#### <a name="an-update-operation-failed"></a>Fehler bei einem Updatevorgang

Wenn bei einem Updatevorgang auf einem Knoten ein Fehler auftritt, wird ein Integritätsbericht für den Knoten-Agent-Dienst generiert. Der Integritätsbericht enthält den Namen des problematischen Knotens.

Der Bericht wird automatisch gelöscht, sobald das Patchen des problematischen Knotens erfolgreich abgeschlossen wurde.

#### <a name="the-node-agent-daemon-service-is-down"></a>Ausfall des Knoten-Agent-Daemon-Diensts

Wenn der Knoten-Agent-Daemon-Dienst auf einem Knoten ausgefallen ist, wird für den Knoten-Agent-Dienst ein Integritätsbericht mit Warnstufe generiert.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Nicht aktivierter Reparatur-Manager-Dienst

Wenn der Reparatur-Manager-Dienst im Cluster nicht gefunden werden kann, wird für den Koordinatordienst ein Integritätsbericht mit Warnstufe generiert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

F: **Warum befindet sich mein Cluster im Status „Fehler“, wenn die App für die Patchorchestrierung ausgeführt wird?**

A. Während des Installationsprozesses deaktiviert die App für die Patchorchestrierung Knoten oder startet sie neu. Dieser Vorgang kann dazu führen, dass die Integrität des Clusters vorübergehend beeinträchtigt wird.

Basierend auf der Richtlinie für die Anwendung kann während eines Patchvorgangs ein Knoten *oder* eine ganze Upgradedomäne gleichzeitig heruntergefahren werden.

Nach Abschluss der Installation werden die Knoten neu gestartet und erneut aktiviert.

Im folgenden Beispiel ist der Cluster vorübergehend in einen Fehlerzustand gewechselt, da zwei Knoten ausgefallen waren und die Richtlinie MaxPercentageUnhealthyNodes verletzt wurde. Dies ist ein temporärer Fehler, der nur auftritt, solange der Patchvorgang ausgeführt wird.

![Abbildung eines Clusters mit Fehler](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sollte das Problem dauerhaft auftreten, lesen Sie die Informationen im Abschnitt zur Problembehandlung.

F: **Die App für die Patchorchestrierung befindet sich im Status „Warnung“.**

A. Überprüfen Sie, ob der für die Anwendung gesendete Integritätsbericht Informationen zur Ursache enthält. Üblicherweise enthält die Warnung Details zum Problem. Wenn das Problem vorübergehend ist, ist zu erwarten, dass die Anwendung automatisch wiederhergestellt wird.

F: **Was kann ich tun, wenn mein Cluster einen Fehler aufweist und ich ein dringendes Betriebssystemupdate ausführen muss?**

A. Die App für die Patchorchestrierung installiert keine Updates, solange der Cluster sich in einem fehlerhaften Zustand befindet. Versetzen Sie den Cluster in einen fehlerfreien Zustand, um den Workflow der App für die Patchorchestrierung fortzuführen.

F: **Warum dauert das Patchen für den gesamten Cluster so lange?**

A. Die Ausführungsdauer der App für die Patchorchestrierung ist größtenteils von den folgenden Faktoren abhängig:

- Richtlinie des Koordinatordiensts. 
  - Die Standardrichtlinie `NodeWise` führt dazu, dass nur jeweils ein Knoten gepatcht wird. Besonders bei größeren Clustern sollten Sie die Richtlinie `UpgradeDomainWise` verwenden, um ein schnelleres Patchen bei mehreren Clustern zu erreichen.
- Anzahl der zum Herunterladen und Installieren verfügbaren Updates. 
- Durchschnittliche Zeit zum Herunterladen und Installieren eines Updates. Dies sollte nicht länger als einige Stunden dauern.
- Leistung des virtuellen Computers und Netzwerkbandbreite.

F: **Wie ermittelt die App für die Patchorchestrierung, bei welchen Updates es sich um Sicherheitsupdates handelt?**

A. Die App für die Patchorchestrierung verwendet distributionsspezifische Logik zum Ermitteln, bei welchen der verfügbaren Updates es sich um Sicherheitsupdates handelt. Beispiel:  In Ubuntu sucht die App nach Updates aus den Archiven „$RELEASE-security“ und „$RELEASE-updates“ („$RELEASE = xenial“ oder die Linux Standard-Basis-Releaseversion). 

 
F: **Wie kann ich eine Sperre für eine bestimmte Paketversion festlegen?**

A. Verwenden Sie die ApprovedPatches-Einstellungen, um Pakete für eine bestimmte Version zu sperren. 


F: **Wie verhält es sich mit in Ubuntu aktivierten automatischen Updates?**

A. Sobald Sie die App für die Patchorchestrierung im Cluster installieren, wird unattended-upgrades auf dem Clusterknoten deaktiviert. Der gesamte Workflow für regelmäßige Updates wird dann durch die App für die Patchorchestrierung gesteuert.
Für die Konsistenz der Umgebung im Cluster empfiehlt es sich, die Updates ausschließlich über die App für die Patchorchestrierung zu installieren. 
 
F: **Führt die App für die Patchorchestrierung nach dem Upgrade die Bereinigung nicht verwendeter Pakete durch?**

A. Ja, die Bereinigung ist Bestandteil der Schritte nach der Installation. 

F: **Kann die App für die Patchorchestrierung für das Patchen des Entwicklungsclusters (Einzelknotencluster) verwendet werden?**

A. Nein, die App für die Patchorchestrierung kann nicht für Patchvorgänge in Einzelknotenclustern verwendet werden. Diese Einschränkung ist entwurfsbedingt, da es zu Ausfallzeiten bei [Service Fabric-Systemdiensten](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) oder anderen Kunden-Apps kommen kann, sodass ein Reparaturauftrag für einen Patch nie vom Reparatur-Manager genehmigt werden würde.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="a-node-is-not-coming-back-to-up-state"></a>Ein Knoten wird nicht wieder in den betriebsbereiten Zustand versetzt.

**Der Knoten kann sich aus folgenden Gründen im deaktivierten Zustand befinden:**

Eine Sicherheitsprüfung steht aus. Um in dieser Situation Abhilfe zu schaffen, stellen Sie sicher, dass genügend Knoten in fehlerfreiem Zustand verfügbar sind.

**Der deaktivierte Zustand des Knotens kann aus folgenden Gründen nicht aufgehoben werden**:

- Der Knoten wurde manuell deaktiviert.
- Der Knoten wurde aufgrund eines laufenden Azure-Infrastrukturauftrags deaktiviert.
- Der Knoten wurde von der App für die Patchorchestrierung vorübergehend deaktiviert, um Patches auf den Knoten aufzuspielen.

**Der Ausfall des Knoten kann aus folgenden Gründen nicht aufgehoben werden**:

- Der Knoten wurde manuell in den Zustand „Ausgefallen“ versetzt.
- Der Knoten wird gerade neu gestartet (dies kann auch durch die App für die Patchorchestrierung ausgelöst worden sein).
- Der Knoten ist aufgrund eines fehlerhaften virtuellen Computers oder aufgrund von Netzwerkverbindungsproblemen ausgefallen.

### <a name="updates-were-skipped-on-some-nodes"></a>Updates wurden auf einigen Knoten übersprungen

Die App für die Patchorchestrierung versucht, ein Update entsprechend der Neuplanungsrichtlinie zu installieren. Der Dienst versucht gemäß der Anwendungsrichtlinie, den Knoten wiederherzustellen und das Update zu überspringen.

In einem solchen Fall wird ein Integritätsbericht mit Warnstufe für den Knoten-Agent-Dienst generiert. Das Ergebnis des Updates enthält ebenfalls mögliche Fehlerursachen.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Während der Installation eines Updates wird der Cluster in einen Fehlerzustand versetzt.

Ein fehlerhaftes Update kann die Integrität einer Anwendung oder eines Clusters auf einem bestimmten Knoten oder in einer Upgradedomäne verringern. Die App für die Patchorchestrierung reagiert erst wieder auf nachfolgende Updatevorgänge, wenn der Cluster wieder fehlerfrei ist.

Ein Administrator muss eingreifen und ermitteln, weshalb die Integrität der Anwendung oder des Clusters aufgrund eines zuvor installierten Updates beeinträchtigt wurde.

## <a name="disclaimer"></a>Haftungsausschluss

Die App für die Patchorchestrierung sammelt Telemetriedaten zum Nachverfolgen von Nutzung und Leistung. Die Telemetrieeinstellung der Anwendung folgt der Telemetrieeinstellung der Service Fabric-Laufzeit (standardmäßig ist die Einstellung aktiviert).

## <a name="release-notes"></a>Versionsinformationen

### <a name="version-010"></a>Version 0.1.0
- Private Vorschauversion

### <a name="version-200"></a>Version 2.0.0
- Öffentliche Version

### <a name="version-201"></a>Version 2.0.1
- Die App wurde mithilfe des neuesten Service Fabric-SDK erneut kompiliert.

### <a name="version-202-latest"></a>Version 2.0.2 (aktuelle Version)
- Es wurde ein Problem behoben, bei dem Zustandswarnungen während des Neustarts vernachlässigt wurden.
