---
title: "Konfigurieren Ihres eigenständigen Azure Service Fabric-Clusters | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie einen eigenständigen oder lokalen Azure Service Fabric-Cluster konfigurieren."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: dc17ba7f8cc1326790b0256de277ccb2eaa20949
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2017
---
# <a name="configuration-settings-for-a-standalone-windows-cluster"></a>Konfigurationseinstellungen für einen eigenständigen Windows-Cluster
In diesem Artikel wird beschrieben, wie Sie einen eigenständigen Azure Service Fabric-Cluster mithilfe der Datei „ClusterConfig.JSON“ konfigurieren. Sie können diese Datei verwenden, um Informationen wie die Service Fabric-Knoten und deren IP-Adressen sowie die unterschiedlichen Typen der Knoten auf dem Cluster anzugeben. Sie können auch Sicherheitskonfigurationen sowie die Netzwerktopologie in Bezug auf Fehler-/Upgradedomänen für Ihren eigenständigen Cluster angeben.

Wenn Sie [das eigenständige Service Fabric-Paket herunterladen](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), werden einige Beispiele der Datei „ClusterConfig.JSON“ auf Ihren Arbeitscomputer heruntergeladen. Die Beispiele mit DevCluster im Namen dienen Ihnen als Hilfe beim Erstellen eines Clusters mit allen drei Knoten auf demselben Computer, z.B. logischen Knoten. Davon muss mindestens ein Knoten als primärer Knoten gekennzeichnet sein. Dieser Cluster eignet sich für eine Entwicklungs- oder Testumgebung. Er wird nicht als Produktionscluster unterstützt. Die Beispiele mit „MultiMachine“ im Namen dienen Ihnen als Hilfe beim Erstellen eines Clusters in Produktionsqualität, bei dem sich jeder Knoten auf einem anderen Computer befindet. Die Anzahl von primären Knoten für diese Cluster basiert jeweils auf der [Zuverlässigkeitsstufe](#reliability). Im Release 5.7, API-Version 05-2017 wurde die Eigenschaft für die Zuverlässigkeitsstufe entfernt. Stattdessen wird mit dem Code die optimale Leistungsstufe für Ihren Cluster berechnet. Verwenden Sie diese Eigenschaft nicht in der Codeversion 5.7 und höher.


* Mit „ClusterConfig.Unsecure.DevCluster.JSON“ und „ClusterConfig.Unsecure.MultiMachine.JSON“ wird veranschaulicht, wie Sie einen ungeschützten Test- bzw. Produktionscluster erstellen.

* Mit „ClusterConfig.Windows.DevCluster.JSON“ und „ClusterConfig.Windows.MultiMachine.JSON“ wird veranschaulicht, wie Sie einen Test- oder Produktionscluster erstellen, der per [Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md) geschützt ist.

* Mit „ClusterConfig.X509.DevCluster.JSON“ und „ClusterConfig.X509.MultiMachine.JSON“ wird veranschaulicht, wie Sie einen Test- oder Produktionscluster erstellen, der per [Sicherheit auf Basis des X509-Zertifikats](service-fabric-windows-cluster-x509-security.md) geschützt ist.

Sehen wir uns nun die verschiedenen Abschnitte der Datei „ClusterConfig.JSON“ an.

## <a name="general-cluster-configurations"></a>Allgemeine Clusterkonfigurationen
Allgemeine Clusterkonfigurationen behandeln wie im folgenden JSON-Ausschnitt dargestellt die breiten clusterspezifischen Konfigurationen:

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

Sie können für Ihren Service Fabric-Cluster einen beliebigen Anzeigenamen festlegen, indem sie ihn der Variablen „name“ zuweisen. Die „clusterConfigurationVersion“ ist die Versionsnummer Ihres Clusters. Erhöhen Sie diese Nummer jedes Mal, wenn Sie Ihr Service Fabric-Cluster upgraden. Lassen Sie „apiVersion“ als Standardversion.

    <a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a>Knoten im Cluster
Sie können die Knoten in Ihrem Service Fabric-Cluster, wie im folgenden Codeausschnitt gezeigt, mithilfe des „nodes“-Abschnitts konfigurieren:

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Ein Service Fabric-Cluster muss mindestens drei Knoten enthalten. Wenn es für Ihr Setup erforderlich ist, können Sie diesem Abschnitt weitere Knoten hinzufügen. In der folgenden Tabelle werden die Konfigurationseinstellungen für alle Knoten erläutert:

| **Knotenkonfiguration** | **Beschreibung** |
| --- | --- |
| nodeName |Sie können einen beliebigen Anzeigenamen für den Knoten festlegen. |
| iPAddress |Ermitteln Sie die IP-Adresse Ihres Knotens, indem Sie in einem Befehlsfenster `ipconfig`eingeben. Notieren Sie sich die IPv4-Adresse, und weisen Sie sie der Variablen „iPAddress“ zu. |
| nodeTypeRef |Jedem Knoten kann ein anderer Knotentyp zugewiesen werden. Die [Knotentypen](#node-types) werden im folgenden Abschnitt definiert. |
| faultDomain |Mit Fehlerdomänen können Clusteradministratoren die physischen Knoten definieren, die unter Umständen gleichzeitig ausfallen können, weil gemeinsame physische Abhängigkeiten bestehen. |
| upgradeDomain |Upgradedomänen beschreiben Gruppen von Knoten, die zum Durchführen von Service Fabric-Upgrades zur gleichen Zeit heruntergefahren werden. Sie können selbst auswählen, welche Knoten den Upgradedomänen zugewiesen werden sollen, denn es bestehen hier keine physischen Einschränkungen. |

## <a name="cluster-properties"></a>Clustereigenschaften
Im Abschnitt „properties“ der Datei „ClusterConfig.JSON“ wird der Cluster wie folgt konfiguriert:

    <a id="reliability"></a>

### <a name="reliability"></a>Zuverlässigkeit
Das Konzept „reliabilityLevel“ definiert die Anzahl von Replikaten oder Instanzen der Service Fabric-Systemdienste, die auf den primären Knoten des Clusters ausgeführt werden können. Es bestimmt die Zuverlässigkeit dieser Dienste und damit auch des Clusters. Der Wert wird vom System bei der Clustererstellung und beim Upgraden des Clusters berechnet.

### <a name="diagnostics"></a>Diagnose
Im Abschnitt „diagnosticsStore“ können Sie Parameter konfigurieren, um bei Knoten- und Clusterausfällen die Diagnose und Fehlerbehandlung zu ermöglichen. Dies wird im folgenden Codeausschnitt veranschaulicht: 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

„Metadata“ ist eine Beschreibung Ihres Clusters und kann gemäß Ihrem Setup festgelegt werden. Diese Variablen helfen Ihnen beim Sammeln von ETW-Ablaufprotokollen, Absturzabbildern und Leistungsindikatoren. Weitere Informationen über ETW-Ablaufprotokolle finden Sie unter [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) und [ETW-Ablaufverfolgung](https://msdn.microsoft.com/library/ms751538.aspx). Alle Protokolle einschließlich [Absturzabbildern](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) und [Leistungsindikatoren](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) können zum Ordner „connectionString“ auf Ihrem Computer geleitet werden. Sie können AzureStorage auch zum Speichern von Diagnosen verwenden. Informationen hierzu finden Sie auch im folgenden Beispielcodeausschnitt:

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Sicherheit
Der Abschnitt „security“ wird für einen sicheren eigenständigen Service Fabric-Cluster benötigt. Der folgende Codeausschnitt zeigt einen Teil dieses Abschnitts:

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

„Metadata“ ist eine Beschreibung Ihres sicheren Clusters und kann gemäß Ihrem Setup festgelegt werden. „ClusterCredentialType“ und „ServerCredentialType“ bestimmen den Typ der Sicherheit, der im Cluster und in den Knoten implementiert wird. Sie können *X509* für eine auf Zertifikaten basierende Sicherheit oder *Windows* für eine auf Azure Active Directory basierende Sicherheit festlegen. Die übrigen Festlegungen im Abschnitt „security“ hängen vom Typ der Sicherheit ab. Informationen darüber, wie Sie die restlichen Teile des Abschnitts „security“ ausfüllen, erfahren Sie in den Artikeln [Zertifikatbasierte Sicherheit in eigenständigen Windows-Clustern](service-fabric-windows-cluster-x509-security.md) und [Windows-Sicherheit in eigenständigen Windows-Clustern](service-fabric-windows-cluster-windows-security.md).

    <a id="nodetypes"></a>

### <a name="node-types"></a>Knotentypen
Im Abschnitt „nodeTypes“ werden die Typen der Knoten beschrieben, die in Ihrem Cluster vorhanden sind. Es muss wie im folgenden Codeausschnitt gezeigt mindestens ein Knotentyp für einen Cluster angegeben sein: 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Der Name ist der Anzeigename für diesen bestimmten Knotentyp. Weisen Sie zum Erstellen eines Knotens dieses Typs seinen Anzeigenamen der Variablen „nodeTypeRef“ für den Knoten zu, wie [zuvor beschrieben](#nodes-on-the-cluster). Definieren Sie für jeden Knotentyp die zu verwendenden Verbindungsendpunkte. Für diese Verbindungsendpunkte können Sie jede beliebige Portnummer auswählen, sofern dadurch keine Konflikte mit anderen Endpunkten in diesem Cluster entstehen. In einem Cluster mit mehreren Knoten gibt es mindestens einen primären Knoten („isPrimary“ ist *TRUE*). Dies richtet sich nach der Zuverlässigkeitsstufe ([reliabilityLevel](#reliability)). Informationen zu den primären Knoten und anderen Knotentypen sowie zu „nodeTypes“ und „reliabilityLevel“ erhalten Sie unter [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster](service-fabric-cluster-capacity.md). 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Endpunkte zum Konfigurieren der Knotentypen
* „clientConnectionEndpointPort“ ist der Port, der vom Client zum Herstellen der Verbindung mit dem Cluster verwendet wird, wenn die Client-APIs genutzt werden. 
* „clusterConnectionEndpointPort“ ist der Port, über den die Knoten miteinander kommunizieren.
* „leaseDriverEndpointPort“ ist der Port, über den vom Clusterleasetreiber ermittelt wird, ob die Knoten noch aktiv sind. 
* „serviceConnectionEndpointPort“ ist der Port, der von den auf einem Knoten bereitgestellten Anwendungen und Diensten verwendet wird, um mit dem Service Fabric-Client auf dem jeweiligen Knoten zu kommunizieren.
* „httpGatewayEndpointPort“ ist der Port, der vom Service Fabric Explorer zum Herstellen der Verbindung mit dem Cluster verwendet wird.
* „ephemeralPorts“ setzen die [vom Betriebssystem verwendeten dynamischen Ports](https://support.microsoft.com/kb/929851) außer Kraft. Service Fabric nutzt einen Teil dieser Ports als Anwendungsports, und die restlichen Ports sind für das Betriebssystem verfügbar. Außerdem wird dieser Bereich dem vorhandenen Bereich des Betriebssystems zugeordnet, und Sie können die in den JSON-Beispieldateien angegebenen Bereiche für alle Zwecke verwenden. Stellen Sie sicher, dass der Unterschied zwischen den Start- und Endports mindestens 255 beträgt. Wenn dieser Unterschied zu gering ist, kommt es womöglich zu Konflikten, da dieser Bereich für das Betriebssystem freigegeben ist. Um den konfigurierten dynamischen Portbereich anzuzeigen, führen Sie `netsh int ipv4 show dynamicport tcp` aus.
* „applicationPorts“ sind die Ports, die von den Service Fabric-Anwendungen verwendet werden. Der Portbereich der Anwendung sollte ausreichend sein, um die Endpunktanforderung Ihrer Anwendungen abzudecken. Dieser Bereich sollte vom dynamischen Portbereich auf dem Computer (dem „ephemeralPorts“-Bereich in der Konfiguration) getrennt sein. Service Fabric greift auf diese Ports zurück, wenn neue Ports benötigt werden, und öffnet die Firewall für diese Ports. 
* „reverseProxyEndpointPort“ ist ein optionaler Reverseproxy-Endpunkt. Weitere Informationen finden Sie unter [Service Fabric-Reverseproxy](service-fabric-reverseproxy.md). 

### <a name="log-settings"></a>Protokolleinstellungen
Im Abschnitt „fabricSettings“ können Sie die Stammverzeichnisse für die Service Fabric-Daten und -Protokolle festlegen. Sie können diese Verzeichnisse nur während der erstmaligen Clustererstellung anpassen. Informationen hierzu finden Sie auch im Codeausschnitt dieses Abschnitts, der als Beispiel dient:

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Es wird empfohlen, dass Sie ein Nicht-Betriebssystem-Laufwerk als FabricDataRoot und FabricLogRoot verwenden. Er bietet höhere Zuverlässigkeit, wenn es darum geht, Situationen zu vermeiden, in denen das Betriebssystem nicht mehr reagiert. Wenn Sie nur das Stammverzeichnis für die Daten anpassen, wird das Stammverzeichnis für die Protokolle genau eine Ebene unterhalb des Stammverzeichnisses für die Daten angesiedelt.

### <a name="stateful-reliable-services-settings"></a>Einstellungen für zustandsbehaftete Reliable Services
Im Abschnitt „KtlLogger“ können Sie die globalen Konfigurationseinstellungen für Reliable Services festlegen. Weitere Informationen zu diesen Einstellungen finden Sie unter [Konfigurieren zustandsbehafteter Reliable Services](service-fabric-reliable-services-configuration.md). Im folgenden Beispiel wird gezeigt, wie Sie das freigegebene Transaktionsprotokoll ändern, das als Unterstützung für zuverlässige Sammlungen für zustandsbehaftete Dienste erstellt wird:

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Add-On-Funktionen
Um Add-on-Funktionen zu konfigurieren, konfigurieren Sie die API-Version als „04-2017“ oder höher, und konfigurieren Sie „addonFeatures“, so wie hier gezeigt:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Containerunterstützung
Um bei eigenständigen Clustern die Containerunterstützung sowohl für Windows Server-Container als auch für Hyper-V-Container zu aktivieren, muss das Add-on-Feature von „DnsService“ aktiviert sein.


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine vollständige „ClusterConfig.JSON“-Datei gemäß des Setups Ihres eigenständigen Clusters konfiguriert haben, können Sie Ihren Cluster bereitstellen. Befolgen Sie die Schritte unter [Erstellen eines eigenständigen Azure Service Fabric-Clusters](service-fabric-cluster-creation-for-windows-server.md). Fahren Sie dann mit [Visualisieren Ihres Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) fort, und befolgen Sie die Schritte.

