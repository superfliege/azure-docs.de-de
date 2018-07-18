---
title: Aktivieren von SSL für einen Azure-MLC-Cluster (Machine Learning Compute) | Microsoft-Dokumentation
description: Hier finden Sie eine Anleitung zum Einrichten von SSL für Bewertungsaufrufe in einem Azure-MLC-Cluster (Machine Learning Compute).
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ms.openlocfilehash: 14f8dd29b7d4185d01529631333de045ad23cdd0
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831454"
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>Aktivieren von SSL für einen Azure-MLC-Cluster (Machine Learning Compute) 

Anhand dieser Anleitung können Sie SSL für Bewertungsaufrufe in einem MLC-Cluster (Machine Learning Compute) einrichten. 

## <a name="prerequisites"></a>Voraussetzungen 

1. Entscheiden Sie sich für einen CNAME-Eintrag (DNS-Name), der bei Echtzeit-Bewertungsaufrufen verwendet werden soll.

2. Erstellen Sie ein *kennwortfreies* Zertifikat mit diesem CNAME-Eintrag.

3. Wenn das Zertifikat nicht bereits im PEM-Format vorliegt, konvertieren Sie es mithilfe eines Tools wie *openssl* in das PEM-Format.

Nach Abschluss der Vorbereitung verfügen Sie über zwei Dateien:

* Eine Datei für das Zertifikat (beispielsweise `cert.pem`). Stellen Sie sicher, dass die Datei über die vollständige Vertrauenskette verfügt.
* Eine Datei für den Schlüssel (beispielsweise `key.pem`).



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Einrichten eines SSL-Zertifikats für einen neuen ACS-Cluster

Führen Sie über die Azure Machine Learning-Befehlszeilenschnittstelle den folgenden Befehl mit dem Switch `-c` aus, um einen ACS-Cluster mit einem angefügten SSL-Zertifikat zu erstellen:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Einrichten eines SSL-Zertifikats für einen vorhandenen ACS-Cluster

Bei Verwendung eines Clusters, der ohne SSL erstellt wurde, können Sie mithilfe von Azure PowerShell-Cmdlets ein Zertifikat hinzufügen.

Sie müssen den Schlüssel und das Zertifikat im PEM-Rohdatenformat bereitstellen. Diese können in PowerShell-Variablen gelesen werden:

```
$keyValueInPemFormat = [IO.File]::ReadAllText('<path to key.pem file>')
$certValueInPemFormat = [IO.File]::ReadAllText('<path to cert.pem file>')
```
Fügen Sie das Zertifikat zum Cluster hinzu: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>Zuordnen von CNAME-Eintrag und IP-Adresse

Erstellen Sie eine Zuordnung zwischen dem CNAME-Eintrag, den Sie im Rahmen der Vorbereitung gewählt haben, und der IP-Adresse des Echtzeit-Front-Ends (FE). Führen Sie zum Ermitteln der IP-Adresse des FEs den weiter unten angegebenen Befehl aus. Die Ausgabe enthält ein Feld namens „publicIpAddress“ mit der IP-Adresse des Echtzeitcluster-Front-Ends. Richten Sie gemäß den Anweisungen Ihres DNS-Anbieters einen Eintrag zwischen dem in CNAME verwendeten FQDN und der öffentlichen IP-Adresse ein.



## <a name="auto-detection-of-a-certificate"></a>Automatische Erkennung eines Zertifikats 

Wenn Sie einen Echtzeitwebdienst mithilfe des Befehls `az ml service create realtime` erstellen, erkennt Azure Machine Learning automatisch die SSL-Konfiguration des Clusters und richtet automatisch die Bewertungs-URL mit dem angegebenen Zertifikat ein. 

Führen Sie den folgenden Befehl aus, um den Zertifikatstatus anzuzeigen. Beachten Sie das Präfix „https“ des Bewertungs-URIs und den CNAME-Eintrag im Hostnamen. 

``` 
az ml service show realtime -n <service name>
{
                "id" : "<your service id>",
                "name" : "<your service name >",
                "state" : "Succeeded",
                "createdAt" : "<datetime>”,
                "updatedAt" : "< datetime>",
                "scoringUri" : "https://<your CNAME>/api/v1/service/<service name>/score"
}
```
