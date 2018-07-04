---
title: Data Science Virtual Machine-Pools – Azure | Microsoft-Dokumentation
description: Bereitstellen von Pools von Data Science VM-Instanzen als freigegebene Ressource für ein Team
keywords: Deep Learning, KI, Data Science-Tools, virtueller Data Science-Computer, räumliche Analysen, Data Science-Teamprozess
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 0740ff7542d066442146b8e80e188ad5ba49a2b5
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309397"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Erstellen eines freigegebenen Pools von Data Science Virtual Machine-Instanzen

In diesem Artikel erfahren Sie, wie Sie einen freigegebenen Pool von Data Science Virtual Machine-Instanzen (DSVMs) für die Verwendung durch ein Team erstellen können. Die Vorteile der Verwendung eines freigegebenen Pools sind die bessere Ressourcennutzung, die Vereinfachung von Freigaben und Zusammenarbeit und die effektivere Verwaltung der DSVM-Ressourcen. 

Zum Erstellen eines DSVM-Pools können Sie zahlreiche Methoden und Technologien verwenden. Die Schwerpunkte dieses Artikels liegen auf Pools für die Batchverarbeitung und interaktiven VMs.

## <a name="batch-processing-pool"></a>Pool für die Batchverarbeitung
Wenn Sie einen Pool von DSVM-Instanzen hauptsächlich für die Offlineausführung von Aufträgen in einem Batch einrichten möchten, können Sie den Dienst [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) oder [Azure Batch](https://docs.microsoft.com/azure/batch/) verwenden. In diesem Artikel wird Azure Batch AI genauer betrachtet.

Die Ubuntu-Edition der DSVM wird als eines der Images in Azure Batch AI unterstützt. In Azure CLI oder im Python SDK, in der bzw. dem Sie den Azure Batch AI-Cluster erstellen, geben Sie den `image`-Parameter an und legen ihn auf `UbuntuDSVM` fest. Sie können die gewünschte Art von Verarbeitungsknoten festlegen: GPU-basierte Instanzen oder einfache CPU-Instanzen, Anzahl der CPUs und Arbeitsspeicher aus einer [großen Auswahl von VM-Instanzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), die in Azure verfügbar sind. 

Wenn Sie das Ubuntu-DSVM-Image in Batch AI mit GPU-basierten Knoten verwenden, sind die erforderlichen GPU-Treiber und Deep Learning-Frameworks vorinstalliert. Dadurch sparen Sie sehr viel Zeit bei der Vorbereitung der Batchknoten. Wenn Sie auf einer Ubuntu-DSVM-Instanz interaktiv entwickeln, werden Sie bemerken, dass die Batch AI-Knoten genau das gleiche Setup und die gleiche Konfiguration wie die Umgebung aufweisen. 

Beim Erstellen eines Batch AI-Clusters erstellen Sie normalerweise auch eine Dateifreigabe, die von allen Knoten eingebunden wird. Die Dateifreigabe dient der Ein- und Ausgabe von Daten sowie der Speicherung des Batchauftragscodes bzw. -skripts. 

Nachdem dem Erstellen eines Batch AI-Clusters können Sie die gleiche CLI oder das Python SDK verwenden, um auszuführende Aufträge zu übermitteln. Sie zahlen nur für die Zeit, die zum Ausführen der Batchaufträge aufgewendet wird. 

Weitere Informationen:
* Schrittweise exemplarische Vorgehensweise zur Verwendung von [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) für die Verwaltung von Batch AI
* Schrittweise exemplarische Vorgehensweise zur Verwendung von [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) für die Verwaltung von Batch AI
* [Batch AI-Anleitungen](https://github.com/Azure/BatchAI) zur Verwendung verschiedener KI- und Deep Learning-Frameworks mit Batch AI

## <a name="interactive-vm-pool"></a>Interaktiver VM-Pool

Ein Pool von interaktiven VMs, die vom gesamten KI/Data Science-Team gemeinsam genutzt werden, ermöglicht Benutzern das Anmelden an einer verfügbaren DSVM-Instanz, statt eine dedizierte Instanz für jede Gruppe von Benutzern vorzuhalten. Dieses Setup verbessert die Verfügbarkeit und unterstützt eine effektivere Nutzung der Ressourcen. 

Die Technologie, die Sie zum Erstellen eines interaktiven VM-Pools verwenden, sind [VM-Skalierungsgruppen von Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Mit Skalierungsgruppen können Sie eine Gruppe von identischen virtuellen Computern mit Lastenausgleich und automatischer Skalierung erstellen und verwalten. 

Der Benutzer meldet sich bei der primären IP-Adresse oder DNS-Adresse des Pools an. Die Skalierungsgruppe leitet die Sitzung automatisch an eine verfügbare DSVM in der Skalierungsgruppe weiter. Da sich der Benutzer unabhängig von der VM, an der er sich angemeldet hat, eine ähnliche Umgebung wünscht, stellen alle Instanzen der VM in der Skalierungsgruppe ein freigegebenes Netzlaufwerk wie eine Azure Files- oder eine NFS-Freigabe bereit. Der freigegebene Arbeitsbereich des Benutzers wird normalerweise im freigegebenen Dateispeicher gespeichert, der in alle Instanzen eingebunden ist. 

Eine Azure Resource Manager-Beispielvorlage, mit der eine Skalierungsgruppe mit Ubuntu-DSVM-Instanzen erstellt wird, finden Sie auf [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Ein Beispiel der [Parameterdatei](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) für die Azure Resource Manager-Vorlage wird am gleichen Ort bereitgestellt. 

Sie können die Skalierungsgruppe mit der Azure Resource Manager-Vorlage erstellen, indem Sie mit Azure CLI Werte für die Parameterdatei angeben. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Die vorangehenden Befehle setzen voraus, das Folgendes vorhanden ist:
* Eine Kopie der Parameterdatei mit den für Ihre Instanz der Skalierungsgruppe angegebenen Werten
* Die Anzahl der VM-Instanzen
* Zeiger auf die Azure Files-Freigabe
* Anmeldeinformationen für das in jede VM eingebundene Speicherkonto 

Auf die Parameterdatei wird in den Befehlen lokal verwiesen. Sie können Parameter auch inline übergeben oder zu deren Eingabe im Skript auffordern.  

Die vorangegangene Vorlage ermöglicht den SSH- und den JupyterHub-Port von der Front-End-VM-Skalierungsgruppe zum Back-End-Pool der Ubuntu-DSVM-Instanzen. Als Benutzer melden Sie sich einfach an der VM über SSH oder JupyterHub auf dem normalen Weg an. Weil die VM-Instanzen dynamisch zentral hoch- und herunterskaliert werden können, muss jeder Zustand in der bereitgestellten Azure Files-Freigabe gespeichert werden. Der gleiche Ansatz kann zum Erstellen eines Pools von Windows-DSVM-Instanzen verwendet werden. 

Das [Skript, das Azure Files einbindet](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh), steht auch auf GitHub im „Azure DataScienceVM“-Repository zur Verfügung. Das Skript bindet die Azure Files-Freigabe am angegebenen Bereitstellungspunkt in der Parameterdatei ein. Zudem erstellt das Skript auch Softlinks auf das eingebundene Laufwerk im Startverzeichnis des ursprünglichen Benutzers. Ein benutzerspezifisches Notebook-Verzeichnis innerhalb der Azure Files-Freigabe ist mit dem Verzeichnis `$HOME/notebooks/remote` verknüpft, sodass Benutzer auf ihre Jupyter Notebook-Instanzen zugreifen, sie ausführen und speichern können. Sie können die gleiche Konvention verwenden, wenn Sie zusätzliche Benutzer auf der VM erstellen, um den Jupyter-Arbeitsbereich der einzelnen Benutzer auf die Azure Files-Freigabe zu verweisen. 

VM-Skalierungsgruppen unterstützen die automatische Skalierung. Sie können Regeln festlegen, wann Sie weitere Instanzen erstellen und wann Sie diese zentral herunterskalieren möchten. Beispielsweise können Sie die Anzahl der Instanzen auf Null reduzieren, um Kosten für die Nutzung der Cloudhardware zu sparen, wenn die VMs überhaupt nicht verwendet werden. Die Dokumentationsseiten zu VM-Skalierungsgruppen bieten ausführliche Schritte für die [automatische Skalierung](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten einer gemeinsamen Identität](dsvm-common-identity.md)
* [Sicheres Speichern von Anmeldeinformationen für den Zugriff auf Cloudressourcen](dsvm-secure-access-keys.md)















