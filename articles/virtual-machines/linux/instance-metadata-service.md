---
title: Azure-Instanzmetadatendienst | Microsoft-Dokumentation
description: "RESTful-Schnittstelle zum Abrufen von Informationen über Compute-, Netzwerk- und anstehende Wartungsereignisse einer Linux-VM."
services: virtual-machines-linux
documentationcenter: 
author: harijayms
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: harijayms
ms.openlocfilehash: 1ed64ece4d05dea93fd15e24aaf9921d8614277e
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2017
---
# <a name="azure-instance-metadata-service"></a>Azure-Instanzmetadatendienst


Der Azure-Instanzmetadatendienst stellt Informationen zum Ausführen von Instanzen virtueller Computer bereit, die zum Verwalten und Konfigurieren Ihrer virtuellen Computer verwendet werden können.
Hierzu gehören Informationen wie die SKU, die Netzwerkkonfiguration und bevorstehende Wartungsereignisse. Weitere Informationen zu der Art der verfügbaren Informationen finden Sie unter [Metadatenkategorien](#instance-metadata-data-categories).

Der Instanzmetadatendienst von Azure ist ein REST-Endpunkt, der für alle IaaS-VMs verfügbar ist, die mit dem neuen [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) erstellt wurden. Der Endpunkt steht unter einer bekannten, nicht routingfähigen IP-Adresse zur Verfügung (`169.254.169.254`), auf die nur innerhalb der VM zugegriffen werden kann.

> [!IMPORTANT]
> Dieser Dienst ist in allen Azure-Regionen **allgemein verfügbar**.  Er empfängt regelmäßig Updates, um neue Informationen zu Instanzen virtueller Computer verfügbar zu machen. Auf dieser Seite werden die neuesten [Datenkategorien](#instance-metadata-data-categories) angezeigt.

## <a name="service-availability"></a>Dienstverfügbarkeit
Der Dienst ist in allen Azure-Regionen allgemein verfügbar. Unter Umständen sind nicht alle API-Versionen in allen Azure-Regionen verfügbar.

Regionen                                        | Verfügbarkeit?                                 | Unterstützte Versionen
-----------------------------------------------|-----------------------------------------------|-----------------
[Allgemein verfügbar in globalen Azure-Regionen](https://azure.microsoft.com/regions/)     | Allgemein verfügbar   | 2017-04-02, 2017-08-01
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Allgemein verfügbar | 2017-04-02
[Azure China](https://www.azure.cn/)                                                           | Allgemein verfügbar | 2017-04-02
[Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/)                    | Allgemein verfügbar | 2017-04-02

Diese Tabelle wird aktualisiert, wenn Dienstupdates oder neue unterstützte Versionen verfügbar sind.

Um den Instanzmetadatendienst zu testen, erstellen Sie eine VM über den [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) oder das [Azure-Portal](http://portal.azure.com) in den oben genannten Regionen, und absolvieren Sie die folgenden Beispiele.

## <a name="usage"></a>Verwendung

### <a name="versioning"></a>Versionsverwaltung
Für den Instanzmetadatendienst wird die Versionsverwaltung genutzt. Versionen sind obligatorisch, und die aktuelle Version in globalen Azure-Regionen ist `2017-08-01`. Aktuelle unterstützte Versionen: 2017-04-02, 2017-08-01

> [!NOTE] 
> In früheren Vorschauversionen von geplanten Ereignissen wird {latest} als „api-version“ unterstützt. Dieses Format wird nicht mehr unterstützt und wird zukünftig veraltet sein.

Wenn wir neuere Versionen hinzufügen, kann auf ältere Versionen aus Kompatibilitätsgründen weiterhin zugegriffen werden, falls Ihre Skripts von bestimmten Datenformaten abhängig sind. Beachten Sie jedoch, dass die vorherige Vorschauversion (2017-03-01) möglicherweise nicht verfügbar ist, sobald der Dienst allgemein zur Verfügung steht.

### <a name="using-headers"></a>Verwenden von Headern
Bei der Abfrage des Instanzmetadatendiensts müssen Sie den Header `Metadata: true` angeben, um sicherzustellen, dass die Anforderung nicht unbeabsichtigt umgeleitet wurde.

### <a name="retrieving-metadata"></a>Abrufen von Metadaten

Instanzmetadaten stehen für die Ausführung von VMs zur Verfügung, die mit dem [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) erstellt/verwaltet werden. Über folgende Anforderung können Sie auf alle Datenkategorien einer Instanz eines virtuellen Computers zugreifen:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> Bei Instanzmetadatenabfragen ist Groß-/Kleinschreibung zu berücksichtigen.

### <a name="data-output"></a>Datenausgabe
Standardmäßig gibt der Instanzmetadatendienst Daten im JSON-Format (`Content-Type: application/json`) zurück. Falls angefordert, geben andere APIs jedoch Daten in verschiedenen Formaten zurück.
In der folgenden Tabelle werden andere Datenformate aufgeführt, die die APIs eventuell unterstützen.

API | Standarddatenformat | Andere Formate
--------|---------------------|--------------
/instance | json | text
/scheduledevents | json | (Keine)

Um auf ein nicht standardmäßiges Antwortformat zuzugreifen, geben Sie das angeforderte Format als QueryString-Parameter in der Anforderung an. Beispiel:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a>Sicherheit
Auf den Instanzmetadatendienst-Endpunkt kann nur innerhalb der ausgeführten Instanz des virtuellen Computers an einer nicht routingfähigen IP-Adresse zugegriffen werden. Darüber hinaus wird jede Anforderung mit einem `X-Forwarded-For`-Header vom Dienst abgelehnt.
Zudem müssen Anforderungen einen `Metadata: true`-Header enthalten, um sicherzustellen, dass die eigentliche Anforderung direkt vorgesehen war und nicht Teil einer unbeabsichtigten Umleitung ist. 

### <a name="error"></a>Error
Wenn ein Datenelement nicht gefunden wird oder eine Anforderung ungültig ist, gibt der Instanzmetadatendienst standardmäßige HTTP-Fehler zurück. Beispiel:

HTTP-Statuscode | Grund
----------------|-------
200 – OK |
400 – Ungültige Anforderung | Fehlender Header `Metadata: true`
404 – Nicht gefunden | Das angeforderte Element ist nicht vorhanden. 
405 – Methode unzulässig | Es werden ausschließlich `GET`- und `POST`-Anforderungen unterstützt.
429 – Zu viele Anforderungen | Die API unterstützt derzeit maximal 5 Abfragen pro Sekunde.
500 – Dienstfehler     | Wiederholen Sie den Vorgang später.

### <a name="examples"></a>Beispiele

> [!NOTE] 
> Alle API-Antworten stellen JSON-Zeichenfolgen dar. Alle folgenden Beispielantworten werden zur besseren Lesbarkeit im Schöndruck gedruckt.

#### <a name="retrieving-network-information"></a>Abrufen von Netzwerkinformationen

**Anforderung**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Antwort**

> [!NOTE] 
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Abrufen der öffentlichen IP-Adresse

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Abrufen aller Metadaten für eine Instanz

**Anforderung**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

**Antwort**

> [!NOTE] 
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```
{
  "compute": {
    "location": "westus",
    "name": "avset2",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "placementGroupId": "",
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "publisher": "Canonical",
    "resourceGroupName": "myrg",
    "sku": "16.04-LTS",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "16.04.201708030",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmSize": "Standard_D1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Abrufen von Metadaten auf einem virtuellen Windows-Computer

**Anforderung**

Die Instanzmetadaten können unter Windows über das PowerShell-Hilfsprogramm `curl` abgerufen werden: 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

Alternativ ist dies auch über das Cmdlet `Invoke-RestMethod` möglich:
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**Antwort**

> [!NOTE] 
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Datenkategorien von Instanzmetadaten
Folgende Datenkategorien werden über den Instanzmetadatendienst zur Verfügung gestellt:

Daten | Beschreibung | Eingeführt in Version 
-----|-------------|-----------------------
location | Azure-Region, in der die VM ausgeführt wird | 2017-04-02 
Name | Name des virtuellen Computers | 2017-04-02
offer | Stellen Sie Informationen für das VM-Image bereit. Dieser Wert ist nur für Images vorhanden, die mithilfe des Azure-Imagekatalogs bereitgestellt werden. | 2017-04-02
Herausgeber | Herausgeber des VM-Images | 2017-04-02
sku | Spezifische SKU für das VM-Image | 2017-04-02
Version | Version des VM-Image | 2017-04-02
osType | Linux oder Windows | 2017-04-02
platformUpdateDomain |  [Updatedomäne](manage-availability.md), in der die VM ausgeführt wird | 2017-04-02
platformFaultDomain | [Fehlerdomäne](manage-availability.md), in der die VM ausgeführt wird | 2017-04-02
vmId | [Eindeutiger Bezeichner](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) für die VM | 2017-04-02
vmSize | [Größe des virtuellen Computers](sizes.md) | 2017-04-02
subscriptionId | Azure-Abonnement für den virtuellen Computer | 2017-08-01
tags | [Tags](../../azure-resource-manager/resource-group-using-tags.md) für den virtuellen Computer  | 2017-08-01
ResourceGroupName | [Ressourcengruppe](../../azure-resource-manager/resource-group-overview.md) für den virtuellen Computer | 2017-08-01
placementGroupId | [Platzierungsgruppe](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) der VM-Skalierungsgruppe | 2017-08-01
ipv4/privateIpAddress | Lokale IPv4-Adresse der VM | 2017-04-02
ipv4/publicIpAddress | Öffentliche IPv4-Adresse der VM | 2017-04-02
subnet/address | Subnetzadresse der VM | 2017-04-02 
subnet/prefix | Subnetzpräfix, Beispiel 24 | 2017-04-02 
ipv6/ipAddress | Lokale IPv6-Adresse der VM | 2017-04-02 
macAddress | VM-Mac-Adresse | 2017-04-02 
scheduledevents | Derzeit in der öffentlichen Vorschau, siehe [scheduledevents](scheduled-events.md) | 2017-03-01

## <a name="example-scenarios-for-usage"></a>Beispielszenarien für die Verwendung  

### <a name="tracking-vm-running-on-azure"></a>Nachverfolgen einer in Azure ausgeführten VM

Als Dienstanbieter müssen Sie möglicherweise die Anzahl von VMs nachverfolgen, auf denen Ihre Software ausgeführt wird, oder benötigen Agents, die die Eindeutigkeit der VM überwachen müssen. Um eine eindeutige ID für eine VM abrufen zu können, verwenden Sie das Feld `vmId` des Instanzmetadatendiensts.

**Anforderung**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**Antwort**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Platzierung von Containern und Datenpartitionen basierend auf der Fehler-/Updatedomäne 

In bestimmten Szenarien ist die Platzierung unterschiedlicher Datenreplikate von primärer Bedeutung. Beispielsweise müssen Sie bei der [HDFS-Replikatplatzierung](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) oder bei der Containerplatzierung über einen [Orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) die `platformFaultDomain` und die `platformUpdateDomain` kennen, auf denen die VM ausgeführt wird.
Sie können diese Daten direkt über den Instanzmetadatendienst abfragen.

**Anforderung**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**Antwort**

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Abrufen von weiteren Informationen zur VM während einer Supportanfrage 

Als Dienstanbieter erhalten Sie möglicherweise eine Supportanfrage, für die Sie weitere Informationen zur VM erfahren möchten. Wenn Sie den Kunden auffordern, die Computemetadaten weiterzuleiten, können Sie dem Supportmitarbeiter grundlegende Informationen über die Art von VM in Azure bereitstellen. 

**Anforderung**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**Antwort**

> [!NOTE] 
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>Beispiele zum Aufrufen des Metadatendiensts über verschiedene Sprachen auf der VM 

Sprache | Beispiel 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Go Lang  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go            
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
    

## <a name="faq"></a>Häufig gestellte Fragen
1. Ich erhalte die Fehlermeldung `400 Bad Request, Required metadata header not specified`. Was bedeutet dies?
   * Für den Instanzmetadatendienst muss der Header `Metadata: true` in der Anforderung übergeben werden. Durch Übergeben dieses Headers im REST-Aufruf wird der Zugriff auf den Instanzmetadatendienst ermöglicht. 
2. Warum erhalte ich keine Compute-Informationen für meine VM?
   * Der Instanzmetadatendienst unterstützt derzeit nur Instanzen, die mit dem Azure Resource Manager erstellt wurden. In Zukunft können wir zusätzliche Unterstützung für Clouddienst-VMs bieten.
3. Ich habe meinen virtuellen Computer vor einiger Zeit über den Azure Resource Manager erstellt. Warum sehe ich keine Computemetadateninformationen?
   * Fügen Sie für VMs, die nach September 2016 erstellt wurden, ein [Tag](../../azure-resource-manager/resource-group-using-tags.md) hinzu, damit Computemetadaten angezeigt werden. Fügen Sie bei älteren VMs (die vor September 2016 erstellt wurden) Erweiterungen oder Datenträger zur VM hinzu, bzw. entfernen Sie diese, um Metadaten zu aktualisieren.
4. Nicht alle Daten wurden für die neue Version 2017-08-01 aufgefüllt.
   * Fügen Sie für VMs, die nach September 2016 erstellt wurden, ein [Tag](../../azure-resource-manager/resource-group-using-tags.md) hinzu, damit Computemetadaten angezeigt werden. Fügen Sie bei älteren VMs (die vor September 2016 erstellt wurden) Erweiterungen oder Datenträger zur VM hinzu, bzw. entfernen Sie diese, um Metadaten zu aktualisieren.
5. Warum erhalte ich die Fehlermeldung `500 Internal Server Error`?
   * Wiederholen Sie Ihre Anforderung basierend auf dem Exponential-Backoff-System. Wenden Sie sich an den Azure-Support, wenn das Problem weiterhin besteht.
6. Wie kann ich weitere Fragen/Kommentare weiterleiten?
   * Senden Sie Ihre Kommentare unter „http://feedback.azure.com“ an uns.
7. Gilt dies auch für VM-Skalierungsgruppeninstanzen?
   * Ja, der Metadatendienst ist für Skalierungsgruppeninstanzen verfügbar. 
8. Wie beziehe ich Support für den Dienst?
   * Um Support für den Dienst zu beziehen, erstellen Sie im Azure-Portal ein Supportproblem für die VM, auf der Sie nach wiederholten Versuchen keine Metadatenantwort erhalten. 

   ![Instanzmetadatenunterstützung](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die API [Geplante Ereignisse](scheduled-events.md) **in der Public Preview**, die vom Instanzmetadatendienst bereitgestellt wird.
