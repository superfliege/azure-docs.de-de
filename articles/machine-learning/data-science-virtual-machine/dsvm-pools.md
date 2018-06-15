---
title: Pools von virtuellen Computern für Data Science – Azure | Microsoft-Dokumentation
description: Bereitstellen von Pools von Data Science-VMs als freigegebene Ressource für das Team
keywords: Deep Learning, KI, Data Science-Tools, Data Science-VM, räumliche Analysen, Data Science-Teamprozess
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
ms.openlocfilehash: c7aab0435ecbd0aee57a15008ac0270159ec2eb3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837081"
---
# <a name="creating-a-shared-pool-of-data-science-virtual-machines"></a>Erstellen eines freigegebenen Pools von virtuellen Computern für Data Science

In diesem Artikel wird erläutert, wie ein freigegebener Pool von virtuellen Computern für Data Science (Data Science Virtual Machine, DSVM) für die Verwendung durch das Team erstellt werden kann. Die Vorteile der Verwendung eines freigegebenen Pools sind die bessere Ressourcennutzung, die Vereinfachung von Freigaben und Zusammenarbeit und die effektivere Verwaltung der DSVM-Ressourcen durch die IT. 

Es gibt viele Methoden und unterschiedliche Technologien, die zum Erstellen eines Pools von DSVMs verwendet werden können.  Dies sind die Hauptszenarien:

* Pool für die Batchverarbeitung
* Pool für interaktive VMs

## <a name="batch-processing-pool"></a>Pool für die Batchverarbeitung
Wenn Sie einen Pool von DSVMs hauptsächlich für die Offlineausführung von Aufträgen in einem Batch einrichten möchten, können Sie den Dienst [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) oder [Azure Batch](https://docs.microsoft.com/azure/batch/) verwenden. 

### <a name="azure-batch-ai"></a>Azure Batch AI
Die Ubuntu-Edition der DSVM wird als eines der Images in Azure Batch AI unterstützt. In der Azure CLI oder im Python SDK, in der bzw. dem Sie den Azure Batch AI-Cluster erstellen, geben Sie den ```image```-Parameter an und legen ihn auf ```UbuntuDSVM``` fest. Sie können die gewünschte Art von Verarbeitungsknoten festlegen: GPU-basierte Instanzen oder einfache CPU-Instanzen, Anzahl der CPUs, Arbeitsspeicher aus einer [großen Auswahl von VM-Instanzen](https://azure.microsoft.com/pricing/details/virtual-machines/linux/), die in Azure verfügbar sind. Wenn Sie das Ubuntu-DSVM-Image in Batch AI mit GPU-basierten Knoten verwenden, sind die erforderlichen GPU-Treiber und Deep Learning-Frameworks vorinstalliert. Dadurch sparen Sie sehr viel Zeit bei der Vorbereitung der Batchknoten. Wenn Sie auf einer Ubuntu-DSVM interaktiv entwickeln, werden Sie bemerken, dass die Batch AI-Knoten genau das gleiche Setup und die gleiche Konfiguration wie die Umgebung aufweisen. Wenn ein Batch AI-Cluster erstellt wird, erstellen Sie normalerweise auch eine Dateifreigabe, die in allen Knoten eingebunden wird und zur Eingabe und Ausgabe von Daten sowie zum Speichern von Code/Skripts der Batchaufträge dient. 

Nachdem Ihr Batch AI-Cluster erstellt wurde, können Sie die gleiche CLI oder das Python SDK verwenden, um auszuführende Aufträge zu übermitteln. Sie zahlen nur für die Zeit, die zum Ausführen der Batchaufträge aufgewendet wird. 

#### <a name="more-information"></a>Weitere Informationen
* Schrittweise exemplarische Vorgehensweise zur Verwendung der [Azure CLI](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) für die Verwaltung von Batch AI
* Schrittweise exemplarische Vorgehensweise zur Verwendung von [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) für die Verwaltung von Batch AI
* [Batch AI-Anleitungen](https://github.com/Azure/BatchAI) sind verfügbar und veranschaulichen, wie verschiedene AI/Deep Learning-Frameworks mit Batch AI verwendet werden.

## <a name="interactive-vm-pool"></a>Interaktiver VM-Pool

Ein Pool von interaktiven DSVMs, die vom gesamten AI/Data Science-Team gemeinsam genutzt werden, ermöglicht Benutzern das Anmelden an einer verfügbaren Instanz der DSVM, statt eine dedizierte Instanz für jede Gruppe von Benutzern vorzuhalten. Dies verbessert die Verfügbarkeit und unterstützt eine effektivere Nutzung der Ressourcen. 

Die zum Erstellen eines interaktiven VM-Pools verwendete Technologie sind [Azure VM-Skalierungsgruppen](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) (VMSS). Damit können Sie eine Gruppe von identischen VMs mit Lastenausgleich und automatischer Skalierung erstellen und verwalten. Der Benutzer meldet sich bei der primären IP-Adresse oder DNS-Adresse des Pools an. Die Skalierungsgruppe leitet die Sitzung automatisch an eine verfügbare DSVM in der Skalierungsgruppe weiter. Da sich der Benutzer unabhängig von der VM, an der er sich angemeldet hat, eine ähnliche Umgebung wünscht, stellen alle Instanzen der VM in der Skalierungsgruppe ein freigegebenes Netzlaufwerk wie eine Azure Files- oder eine NFS-Freigabe bereit. Der freigegebene Arbeitsbereich des Benutzers wird normalerweise im freigegebenen Dateispeicher gespeichert, der in alle Instanzen eingebunden ist. 

Eine Azure Resource Manager-Beispielvorlage, mit der eine VM-Skalierungsgruppe mit Ubuntu-DSVM-Instanzen erstellt wird, finden Sie auf [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Ein Beispiel der [Parameterdatei](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) für die Azure Resource Manager-Vorlage wird am gleichen Ort bereitgestellt. 

Sie können die VM-Skalierungsgruppe mit der Azure Resource Manager-Vorlage erstellen, indem Sie mit Azure CLI geeignete Werte für die Parameterdatei angeben. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Bei den oben aufgeführten Befehlen wird davon ausgegangen, dass Sie über eine Kopie der Parameterdatei verfügen, in der die Werte für die Instanz der VM-Skalierungsgruppe, die Anzahl der VM-Instanzen, Zeiger auf Ihre Azure Files-Instanz zusammen mit Anmeldeinformationen für das Speicherkonto, das auf den einzelnen VMs bereitgestellt wird, angegeben sind. Auf die Parameterdatei wird im obigen Befehl lokal verwiesen. Sie können Parameter auch inline übergeben oder zu deren Eingabe im Skript auffordern.  

Die obige Vorlage ermöglicht den SSH- und den JupyterHub-Port von der Front-End-VM-Skalierungsgruppe zum Back-End-Pool der Ubuntu-DSVMs.  Als Benutzer melden Sie sich einfach an der VM über SSH oder JupyterHub auf normale Weise an. Da die VM-Instanzen dynamisch zentral hoch- und herunterskaliert werden können, muss jeder Zustand in der bereitgestellten Azure Files-Freigabe gespeichert werden. Der gleiche Ansatz kann zum Erstellen eines Pools von Windows-DSVMs verwendet werden. 

Das [Skript, das Azure Files bereitstellt](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh), steht auch auf GitHub für „Azure DataScienceVM“ zur Verfügung. Zusätzlich zum Bereitstellen von Azure Files am angegebenen Bereitstellungspunkt in der Parameterdatei werden damit auch zusätzliche weiche Links zum bereitgestellten Laufwerk im Basisverzeichnis des ursprünglichen Benutzers erstellt. Zudem wird ein benutzerspezifisches Notebook-Verzeichnis in der Azure Files-Freigabe per weichem Link mit dem Verzeichnis ```$HOME/notebooks/remote``` verknüpft, damit Benutzer auf ihre Jupyter Notebooks zugreifen, sie ausführen und speichern können.  Die gleiche Konvention kann verwendet werden, wenn Sie zusätzliche Benutzer auf der VM erstellen, um den Jupyter-Arbeitsbereich der einzelnen Benutzer auf die Azure Files-Freigabe zu verweisen. 

Azure-VM-Skalierungsgruppen unterstützen die automatische Skalierung. Dabei können Sie Regeln festlegen, wann zusätzliche Instanzen erstellt und unter welchen Umständen Instanzen zentral herunterskaliert werden. Dazu gehört auch, die Anzahl der Instanzen auf null zu reduzieren, um Kosten für die Nutzung von Cloudhardware zu sparen, wenn die VMs nicht verwendet werden. Die Dokumentationsseiten zu VM-Skalierungsgruppen bieten ausführliche Schritte für die [automatische Skalierung](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten einer gemeinsamen Identität](dsvm-common-identity.md)
* [Sicheres Speichern von Anmeldeinformationen für den Zugriff auf Cloudressourcen](dsvm-secure-access-keys.md)















