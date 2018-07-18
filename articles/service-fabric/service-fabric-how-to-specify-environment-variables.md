---
title: Angeben von Umgebungsvariablen für Dienste in Microsoft Azure Service Fabric | Microsoft-Dokumentation
description: Es wird gezeigt, wie Umgebungsvariablen für Anwendungen in Service Fabric verwendet werden können.
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 4325b3acd3cbc73ee5976021bebe96c267b2a6dd
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206027"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Angeben von Umgebungsvariablen für Dienste in Service Fabric

In diesem Artikel erfahren Sie, wie Sie Umgebungsvariablen für einen Dienst oder Container in Service Fabric angeben können.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Vorgehensweise zum Angeben von Umgebungsvariablen für Dienste

In diesem Beispiel legen Sie eine Umgebungsvariable für einen Container fest. Für den Artikel wird davon ausgegangen, dass Sie bereits eine Anwendung und ein Dienstmanifest haben.

1. Öffnen Sie die Datei „ServiceManifest.xml“.
1. Fügen Sie im `CodePackage`-Element ein neues `EnvironmentVariables`-Element und für jede Umgebungsvariable ein `EnvironmentVariable`-Element hinzu.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DeafultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    Umgebungsvariablen können im Anwendungsmanifest überschrieben werden.

1. Um die Umgebungsvariablen im Anwendungsmanifest zu überschreiben, verwenden Sie das `EnvironmentOverrides`-Element.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu einigen der grundlegenden Konzepte, die in diesem Artikel behandelt werden, finden Sie im Artikel [Verwalten von Anwendungen für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

Informationen zu anderen App-Verwaltungsfunktionen in Visual Studio finden Sie unter [Verwalten Ihrer Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).