---
title: "Bereitstellen der Java-Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "Dieses Tutorial veranschaulicht, wie Sie die Java-Microservices der vorkonfigurierten Remoteüberwachungslösung über die Befehlszeilenschnittstelle bereitstellen."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: d0ed9a7fbb202b2008fee7f810ae0102b6f51c3d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-using-the-cli"></a>Bereitstellen der vorkonfigurierten Remoteüberwachungslösung über die Befehlszeilenschnittstelle

Dieses Tutorial zeigt, wie Sie eine vorkonfigurierte Lösung für die Remoteüberwachung bereitstellen. Sie stellen die Lösung über die Befehlszeilenschnittstelle bereit. Sie können die Lösung auch über die webbasierte Benutzeroberfläche auf azureiotsuite.com bereitstellen. Informationen zu dieser Möglichkeit finden Sie unter [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung der vorkonfigurierten Remoteüberwachungslösung benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](http://azure.microsoft.com/pricing/free-trial/).

Zum Ausführen der Befehlszeilenschnittstelle muss [Node.js](https://nodejs.org/) auf Ihrem lokalen Computer installiert sein.

## <a name="install-the-cli"></a>Installieren der Befehlszeilenschnittstelle

Führen Sie zum Installieren der Befehlszeilenschnittstelle den folgenden Befehl in der Befehlszeilenumgebung aus:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Anmelden bei der Befehlszeilenschnittstelle

Damit Sie die vorkonfigurierte Lösung bereitstellen können, müssen Sie sich wie folgt über die Befehlszeilenschnittstelle bei Ihrem Azure-Abonnement anmelden:

```cmd/sh
pcs login
```

Befolgen Sie die Anweisungen auf dem Bildschirm, um den Anmeldevorgang abzuschließen.

## <a name="deployment-options"></a>Bereitstellungsoptionen

Beim Bereitstellen der vorkonfigurierten Lösung sind mehrere Optionen verfügbar, mit denen der Bereitstellungsvorgang konfiguriert wird:

| Option | Werte | Beschreibung |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard` | Eine Bereitstellung des Typs _basic_ ist für Test- und Demonstrationszwecke vorgesehen. Dabei werden alle Microservices auf einem einzelnen virtuellen Computer bereitgestellt. Eine Bereitstellung des Typs _standard_ ist für die Produktion vorgesehen. Die Microservices werden auf mehreren virtuellen Computern bereitgestellt. |
| Laufzeit | `dotnet`, `java` | Wählt die Implementierung der Sprache für die Microservices aus. |

## <a name="deploy-the-preconfigured-solution"></a>Bereitstellen der vorkonfigurierten Lösung

### <a name="example-deploy-net-version"></a>Beispiel: Bereitstellen der .NET-Version

Das folgende Beispiel veranschaulicht, wie die .NET-Version des Typs „basic“ der vorkonfigurieren Remoteüberwachungslösung bereitgestellt wird:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Beispiel: Bereitstellen der Java-Version

Das folgende Beispiel veranschaulicht, wie die Java-Version des Typs „standard“ der vorkonfigurieren Remoteüberwachungslösung bereitgestellt wird:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Optionen des Befehls „pcs“

Beim Ausführen des Befehls `pcs` zum Bereitstellen einer Lösung müssen Sie folgende Elemente angeben:

- Einen Namen für die Lösung. Dieser Name muss eindeutig sein.
- Das zu verwendende Azure-Abonnement.
- Einen Speicherort.
- Anmeldeinformationen für die virtuellen Computer, auf denen die Microservices gehostet werden. Sie können diese Anmeldeinformationen verwenden, um zur Problembehandlung auf die virtuellen Computer zuzugreifen.

Nach Abschluss des Befehls `pcs` wird die URL für die Bereitstellung der neu vorkonfigurieren Lösung angezeigt. Mit dem Befehl `pcs` wird auch die Datei `{deployment-name}-output.json` mit zusätzlichen Informationen (z.B. dem Namen des bereitgestellten IoT Hub) erstellt.

Wenn Sie weitere Informationen zu den Befehlszeilenparametern benötigen, führen Sie den folgenden Befehl aus:

```cmd/sh
pcs -h
```

Weitere Informationen zur Befehlszeilenschnittstelle finden Sie unter [How to use the CLI](https://github.com/Azure/pcs-cli/blob/master/README.md) (Verwenden der Befehlszeilenschnittstelle).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren der vorkonfigurierten Lösung
> * Bereitstellen der vorkonfigurierten Lösung
> * Anmelden bei der vorkonfigurierten Lösung

Nach dem Bereitstellen der Remoteüberwachungslösung können Sie nun im nächsten Schritt [die Funktionen des Lösungsdashboards untersuchen](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->