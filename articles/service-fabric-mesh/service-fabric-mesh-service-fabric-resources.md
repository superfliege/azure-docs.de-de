---
title: Einführung in das Azure Service Fabric-Ressourcenmodell | Microsoft-Dokumentation
description: Einführung in das Service Fabric-Ressourcenmodell, einem vereinfachten Ansatz zum Definieren von Service Fabric Mesh-Anwendungen
services: service-fabric-mesh
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 6a96995a46cd2ad0a1afe4f4afe49d62409f2b68
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39075289"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Einführung in das Service Fabric-Ressourcenmodell

Mit dem Service Fabric-Ressourcenmodell können Ressourcen, die eine Service Fabric Mesh-Anwendung umfassen, einfach definiert werden. Derzeit unterstützt das Modell die folgenden Ressourcentypen:

- ANWENDUNGEN
- Dienste
- Volumes
- Netzwerke

Jede Ressource wird deklarativ in einer Ressourcendatei angegeben, die ein einfaches YAML- oder JSON-Dokument ist, das die Mesh-Anwendung beschreibt, und von der Service Fabric-Plattform bereitgestellt.

## <a name="resource-overview"></a>Ressourcenübersicht

### <a name="applications-and-services"></a>Anwendungen und Dienste

Eine Anwendungsressource ist die Einheit von Bereitstellung, Versionsverwaltung und Lebensdauer einer Mesh-Anwendung. Sie besteht aus mindestens einer Dienstressource, die einen Microservice darstellt. Jede Dienstressource besteht wiederum aus mindestens einem Codepakete, in dem alle erforderlichen Informationen zum Ausführen des mit dem Codepaket verbundenen Containerimages angegeben sind. Dazu gehören:

- Name, Version und Registrierung des Containers
- Für jeden Container erforderliche CPU- und Speicherressourcen
- Netzwerkendpunkte
- Im Container einzubindende Volumes, die auf eine separate Volumeressource verweisen

Alle Codepakete, die als Teil einer Dienstressource definiert sind, werden zusammen als Gruppe bereitgestellt und aktiviert. Die Dienstressource beschreibt auch, wie viele Dienstinstanzen ausgeführt werden, und verweist außerdem auf andere Ressourcen von denen sie abhängt (z. B. Netzwerkressource).

Wenn eine Mesh-Anwendung aus mehreren Diensten besteht, können diese nicht unbedingt zusammen auf demselben Knoten ausgeführt werden. Wenn bei einem Anwendungsupgrade die Aktualisierung eines einzigen Diensts fehlschlägt, werden alle Dienste auf eine frühere Version zurückgesetzt.



```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

Wie bereits erwähnt, kann der Lebenszyklus jeder Anwendungsinstanz separat verwaltet werden. So kann eine Anwendungsinstanz beispielsweise unabhängig von den anderen Anwendungsinstanzen aktualisiert werden. Die Anzahl von Diensten in einer Anwendung sollte relativ klein gehalten werden. Denn je mehr Dienste Sie einer Anwendung hinzufügen, umso schwieriger ist es, jeden Dienst separat zu verwalten.

### <a name="networks"></a>Netzwerke

Die Netzwerkressource ist eine separat bereitstellbare Ressource. Es besteht keine Abhängigkeit zwischen ihr und anderen Anwendungen oder Dienstressourcen. Sie wird verwendet, um ein privates Netzwerk für Ihre Anwendungen zu erstellen. Mehrere Dienste verschiedener Anwendungen können demselben Netzwerk angehören.

> [!NOTE]
> Die aktuelle Vorschauversion unterstützt nur ein 1:1-Mapping zwischen Anwendungen und Netzwerken.

### <a name="volumes"></a>Volumes

Volumes sind Verzeichnisse, die in Ihre Containerinstanzen eingebunden werden, und mit denen Sie Zustände erhalten können. Mithilfe der Volumeressource können Sie deklarativ beschreiben, wie ein Verzeichnis bereitgestellt wird. Außerdem dient sie dem Verzeichnis als Hintergrundspeicher.

## <a name="programming-models"></a>Programmmodelle
Die Dienstressource erfordert nur ein ausgeführtes Containerimage, das in den Codepaketen referenziert wird, die der Ressource zugeordnete sind. Sie können jeden Code, in einer beliebigen Sprache, mit einem beliebigen Framework im Container ausführen, ohne die spezifischen APIs von Service Fabric Mesh zu kennen oder zu verwenden. 

Ihr Anwendungscode bleibt auch außerhalb von Service Fabric Mesh portabel, und Ihre Anwendungsbereitstellungen bleiben konsistent, unabhängig von der Sprache oder vom Framework, das zum Implementieren Ihrer Dienste verwendet wurde. Unabhängig davon, ob Ihre Anwendung ASP.NET Core, Go oder eine Reihe von Prozessen und Skripten ist, das Service Fabric Mesh-Ressourcenbereitstellungsmodell bleibt unverändert. 

## <a name="deployment"></a>Bereitstellung

Beim Bereitstellen in Service Fabric Mesh werden Ressourcen als Azure Resource Manager-Vorlagen über HTTP oder die Azure CLI in Azure bereitgestellt. 


## <a name="next-steps"></a>Nächste Schritte 
In der Übersicht erfahren Sie mehr über Service Fabric Mesh:
- [Übersicht über Service Fabric Mesh](service-fabric-mesh-overview.md)
