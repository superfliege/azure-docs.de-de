---
title: Bereitstellungsort von Modellen mit dem Azure Machine Learning-Dienst | Microsoft-Dokumentation
description: Erfahren Sie mehr über die verschiedenen Methoden zum Bereitstellen Ihrer Modelle in der Produktion mit dem Azure Machine Learning-Dienst.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/29/2018
ms.openlocfilehash: 606e8aed42bce69d5b3210b4e97f8cbfeaaf104c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961007"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst

Der Azure Machine Learning-Dienst bietet mehrere Möglichkeiten zum Bereitstellen Ihres trainierten Modells. In diesem Dokument erfahren Sie, wie Sie Ihr Modell als Webdienst in der Azure-Cloud oder auf IoT Edge-Geräten bereitstellen.

Sie können Modelle an folgenden Computezielen bereitstellen:

- Azure Container Instances (ACI)
- Azure Kubernetes Service (AKS)
- Azure IoT Edge
- Field-Programmable Gate Array (FPGA)

Im weiteren Verlauf dieses Dokuments werden die einzelnen Optionen ausführlich erläutert.

## <a name="azure-container-instances"></a>Azure Container Instances

Verwenden Sie Azure Container Instances für die Bereitstellung Ihrer Modelle als REST-API-Endpunkt, wenn mindestens eine der folgenden Bedingungen zutrifft:
- Sie möchten eine schnelle Bereitstellung, um Ihr Modell zu bewerten zu überprüfen. Eine ACI-Bereitstellung ist in der Regel in weniger als 5 Minuten abgeschlossen.
- Sie möchten Ihr Modell in einer Entwicklungs- oder Testumgebung bereitstellen. ACI ermöglicht Ihnen das Bereitstellen von 20 Containergruppen pro Abonnement. Weitere Informationen finden Sie im Dokument [Kontingente und Regionsverfügbarkeit für Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

Weitere Informationen finden Sie im Dokument [Bereitstellen eines Modells in Azure Container Instances](how-to-deploy-to-aci.md).

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Für Produktionsszenarien mit umfangreicher Skalierung können Sie Ihr Modell in Azure Kubernetes Service (AKS) bereitstellen. Sie können einen vorhandenen AKS-Cluster verwenden oder mithilfe des Azure Machine Learning-SDK, der CLI oder des Azure-Portals einen neuen erstellen.

Das Erstellen des AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Prozess. Nach der Erstellung können Sie diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe, die ihn enthält, löschen, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen.

Die Bereitstellung in AKS bietet automatische Skalierung, Protokollierung, Modelldatensammlung und schnelle Antwortzeiten für Ihre Webdienste. 

Der Erstellen eines AKS-Clusters dauert etwa 20 Minuten.

Weitere Informationen finden Sie im Dokument [Bereitstellen eines Modells in Azure Kubernetes Service](how-to-deploy-to-aks.md).

## <a name="azure-iot-edge"></a>Azure IoT Edge

Bei IoT-Geräten ist das Durchführen von Bewertungen auf dem Gerät schneller, als Daten in die Cloud senden und zu warten, bis ein in der Cloud gehostetes Modell Daten zurückgibt. Mit Azure IoT Edge können Sie Ihr Modell auf Edge-Geräten hosten. Stellen Sie Ihr Modell in IoT Edge bereit, wenn Sie eine oder mehrere der folgenden Funktionen benötigen:
- Lokale Verarbeitung von Prioritätsaufgaben auch ohne Cloudverbindung
- Arbeiten mit generierten Daten, die zu umfangreich für einen schnellen Abruf aus der Cloud sind
- Aktivieren von Echtzeitverarbeitung über Intelligenz auf oder in der Nähe von lokalen Geräten
- Erfüllen von Anforderungen bezüglich des Datenschutzes 

Weitere Informationen finden Sie im Dokument [Bereitstellen für Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).

Weitere Informationen zum IoT Edge-Dienst finden Sie in der [Azure IoT Edge-Dokumentation](https://docs.microsoft.com/azure/iot-edge/).


## <a name="field-programmable-gate-arrays-fpga"></a>Field-Programmable Gate Arrays (FPGA)

Von Project Brainwave unterstützte hardwarebeschleunigte Modelle ermöglichen äußerst geringe Wartezeiten für Echtzeit-Inferenzanforderungen. Project Brainwave beschleunigt tiefe neuronale Netzwerke (Deep Neural Networks, DNNS), die auf Field-Programmable Gate Arrays in der Azure-Cloud bereitgestellt sind. Häufig verwendete DNNs sind als Featurizer für das Lernen durch Transfer verfügbar oder können mit Gewichtungen angepasst werden, die mit Ihren eigenen Daten trainiert sind.

Weitere Informationen finden Sie im Dokument [Bereitstellen in einem FPGA](how-to-deploy-fpga-web-service.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen eines Modells an einem bestimmten Computeziel finden Sie in den folgenden Dokumenten:

* [Bereitstellen eines Modells in Azure Container Instances](how-to-deploy-to-aci.md)
* [Bereitstellen eines Modells in Azure Kubernetes Service](how-to-deploy-to-aks.md)
* [Bereitstellen eines Modells in Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)
* [Bereitstellen eines Modells in FPGA](how-to-deploy-fpga-web-service.md)
