---
title: Bereitstellen der Java-Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieses Tutorial zeigt, wie Sie mithilfe der CLI den Solution Accelerator für die Remoteüberwachung bereitstellen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 603ce00a036822fd0c7411b47cf3fe630671cc5a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628122"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-using-the-cli"></a>Bereitstellen des Solution Accelerators für die Remoteüberwachung mithilfe der CLI

Dieses Tutorial zeigt, wie Sie den Solution Accelerator für die Remoteüberwachung bereitstellen. Sie stellen die Lösung über die Befehlszeilenschnittstelle bereit. Sie können die Lösung auch über die webbasierte Benutzeroberfläche auf azureiotsuite.com bereitstellen. Informationen zu dieser Möglichkeit finden Sie unter [Bereitstellen des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung des Solution Accelerators für die Remoteüberwachung benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](http://azure.microsoft.com/pricing/free-trial/).

Zum Ausführen der Befehlszeilenschnittstelle muss [Node.js](https://nodejs.org/) auf Ihrem lokalen Computer installiert sein.

## <a name="install-the-cli"></a>Installieren der Befehlszeilenschnittstelle

Führen Sie zum Installieren der Befehlszeilenschnittstelle den folgenden Befehl in der Befehlszeilenumgebung aus:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Anmelden bei der Befehlszeilenschnittstelle

Damit Sie den Solution Accelerator bereitstellen können, müssen Sie sich wie folgt über die CLI bei Ihrem Azure-Abonnement anmelden:

```cmd/sh
pcs login
```

Befolgen Sie die Anweisungen auf dem Bildschirm, um den Anmeldevorgang abzuschließen.

## <a name="deployment-options"></a>Bereitstellungsoptionen

Beim Bereitstellen des Solution Accelerators sind mehrere Optionen verfügbar, mit denen der Bereitstellungsvorgang konfiguriert wird:

| Option | Werte | BESCHREIBUNG |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard`, `local` | Eine Bereitstellung des Typs _basic_ ist für Test- und Demonstrationszwecke vorgesehen. Dabei werden alle Microservices auf einem einzelnen virtuellen Computer bereitgestellt. Eine Bereitstellung des Typs _standard_ ist für die Produktion vorgesehen. Die Microservices werden auf mehreren virtuellen Computern bereitgestellt. Ein _lokale_ Bereitstellung konfiguriert einen Docker-Container für die Ausführung der Microservices auf Ihrem lokalen Computer und verwendet Azure-Dienste wie Storage und Cosmos DB in der Cloud. |
| Laufzeit | `dotnet`, `java` | Wählt die Implementierung der Sprache für die Microservices aus. |

Informationen zur Verwendung der lokalen Bereitstellung finden Sie unter [Lokales Ausführen der Remoteüberwachungslösung](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="basic-vs-standard-deployments"></a>Gegenüberstellung von Basic- und Standard-Bereitstellung

### <a name="basic"></a>Basic
Die Basic-Bereitstellung ist auf die Demonstration der Lösung ausgelegt. Aus Kostengründen werden hierbei alle Microservices auf einem einzelnen virtuellen Computer bereitgestellt. Diese Architektur ist nicht für eine Produktionsumgebung geeignet.

Wenn Sie eine produktionsbereite, auf Skalierbarkeit und Erweiterbarkeit ausgelegte Architektur anpassen möchten, verwenden Sie die Bereitstellungsoption „Standard“.

Bei der Erstellung einer Basic-Lösung werden folgende Azure-Dienste kostenpflichtig in Ihrem Azure-Abonnement bereitgestellt: 

| Count | Ressource                       | Typ         | Verwendung |
|-------|--------------------------------|--------------|----------|
| 1     | [Virtueller Linux-Computer](https://azure.microsoft.com/services/virtual-machines/) | Standard D1 v2  | Hosten von Microservices |
| 1     | [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)                  | S1 (Basic-Tarif) | Geräteverwaltung und Kommunikation |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)              | Standard        | Speichern von Konfigurationsdaten und Gerätetelemetrie wie Regeln, Warnungen und Meldungen |  
| 1     | [Azure Storage-Konto](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)  | Standard        | Speicher für VM und Streamingprüfpunkte |
| 1     | [Webanwendung](https://azure.microsoft.com/services/app-service/web/)        |                 | Hosten der Front-End-Webanwendung |

### <a name="standard"></a>Standard
Die Standard-Bereitstellung ist für eine Produktionsumgebung geeignet und kann von einem Entwickler erweitert und an die jeweiligen Anforderungen angepasst werden. Für Zuverlässigkeit und Skalierbarkeit werden Anwendungsmicroservices als Docker-Container erstellt und über einen Orchestrator (standardmäßig [Kubernetes](https://kubernetes.io/)) bereitgestellt. Der Orchestrator ist für die Bereitstellung, Skalierung und Verwaltung der Anwendung zuständig.

Bei der Erstellung einer Standard-Lösung werden folgende Azure-Dienste kostenpflichtig in Ihrem Azure-Abonnement bereitgestellt:

| Count | Ressource                                     | SKU/Größe      | Verwendung |
|-------|----------------------------------------------|-----------------|----------|
| 4     | [Virtuelle Linux-Computer](https://azure.microsoft.com/services/virtual-machines/)   | Standard D2 v2  | Ein Master und drei Agents zum Hosten von Microservices mit Redundanz |
| 1     | [Azure Container Service](https://azure.microsoft.com/services/container-service/) |                 | Orchestrator [Kubernetes](https://kubernetes.io) |
| 1     | [Azure IoT Hub][https://azure.microsoft.com/services/iot-hub/]                     | S1 (Basic-Tarif) | Geräteverwaltung und -steuerung |
| 1     | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)                 | Standard        | Speichern von Konfigurationsdaten und Gerätetelemetrie wie Regeln, Warnungen und Meldungen |
| 5     | [Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-introduction#types-of-storage-accounts)    | Standard        | Vier für VM-Speicher, eins für die Streamingprüfpunkte |
| 1     | [App Service](https://azure.microsoft.com/services/app-service/web/)             | S1 Standard     | Application Gateway über SSL |

> Preisinformationen für diese Dienste finden Sie [hier](https://azure.microsoft.com/pricing). Informationen zum Verbrauch sowie Abrechnungsdetails für Ihr Abonnement finden Sie im [Azure-Portal](https://portal.azure.com/).

## <a name="deploy-the-solution-accelerator"></a>Bereitstellen des Solution Accelerators

### <a name="example-deploy-net-version"></a>Beispiel: Bereitstellen der .NET-Version

Das folgende Beispiel veranschaulicht die Bereitstellung der grundlegenden .NET-Version des Solution Accelerators für die Remoteüberwachung:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Beispiel: Bereitstellen der Java-Version

Das folgende Beispiel veranschaulicht die Bereitstellung der Java-Standardversion des Solution Accelerators für die Remoteüberwachung:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Optionen des Befehls „pcs“

Beim Ausführen des Befehls `pcs` zum Bereitstellen einer Lösung müssen Sie folgende Elemente angeben:

- Einen Namen für die Lösung. Dieser Name muss eindeutig sein.
- Das zu verwendende Azure-Abonnement.
- Einen Speicherort.
- Anmeldeinformationen für die virtuellen Computer, auf denen die Microservices gehostet werden. Sie können diese Anmeldeinformationen verwenden, um zur Problembehandlung auf die virtuellen Computer zuzugreifen.

Nach Abschluss des Befehls `pcs` wird die URL der neuen Bereitstellung des Solution Accelerators angezeigt. Mit dem Befehl `pcs` wird auch die Datei `{deployment-name}-output.json` mit zusätzlichen Informationen (z.B. dem Namen des bereitgestellten IoT Hub) erstellt.

Wenn Sie weitere Informationen zu den Befehlszeilenparametern benötigen, führen Sie den folgenden Befehl aus:

```cmd/sh
pcs -h
```

Weitere Informationen zur Befehlszeilenschnittstelle finden Sie unter [How to use the CLI](https://github.com/Azure/pcs-cli/blob/master/README.md) (Verwenden der Befehlszeilenschnittstelle).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren des Solution Accelerators
> * Bereitstellen des Solution Accelerators
> * Anmelden beim Solution Accelerator

Nach Bereitstellung der Remoteüberwachungslösung können Sie sich als Nächstes [mit den Funktionen des Lösungsdashboards vertraut machen](./iot-accelerators-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->