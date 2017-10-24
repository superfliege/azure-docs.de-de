---
title: Einrichten von Continuous Integration und Continuous Deployment mithilfe der Stream Analytics Visual Studio-Tools | Microsoft-Dokumentation
description: Tutorial zum Einrichten von Continuous Integration und Continuous Deployment mithilfe der Stream Analytics Visual Studio-Tools
keywords: Visual Studio, NuGet, DevOps, CI/CD
documentationcenter: 
services: stream-analytics
author: su-jie
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 9/27/2017
ms.author: sujie
ms.openlocfilehash: 947266dc94babab21556da26d8cc8c917cf81c12
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-stream-analytics-visual-studio-tools-to-set-up-the-continuous-integration-and-deployment-process"></a>Einrichten von Continuous Integration und Continuous Deployment mithilfe der Stream Analytics Visual Studio-Tools
In diesem Tutorial erfahren Sie, wie Sie Continuous Integration und Continuous Deployment mithilfe der Stream Analytics Visual Studio-Tools einrichten.

In der neuesten Version (2.3.0000.0 oder höher) der [Stream Analytics-Tools für Visual Studio](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-tools-for-visual-studio) wurde Unterstützung für **MSBuild** hinzugefügt. 

Es gibt auch das neu veröffentlichte NuGet-Paket [Microsoft.Azure.Stream Analytics.CICD](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/). Es bietet Tools für MSBuild, lokales Ausführen und Bereitstellung zur Unterstützung von Continuous Integration und Continuous Deployment für Stream Analytics Visual Studio-Projekte. 
> [!NOTE] 
Das NuGet-Paket kann nur mit Version 2.3.0000.0 oder höher der Stream Analytics-Tools für Visual Studio verwendet werden. Wenn einige Ihrer Projekte in früheren Versionen von Visual Studio-Tools erstellt wurden, öffnen Sie sie einfach mit Version 2.3.0000.0 oder einer höheren Version, und speichern Sie sie. Die neuen Funktionen sind dann aktiviert. 

[Erfahren Sie, wie Sie die Azure Stream Analytics-Tools für Visual Studio verwenden](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="msbuild"></a>MSBuild
Wie beim normalen Verfahren mit Visual Studio MSBuild können Sie zum Erstellen eines Projekts entweder mit der rechten Maustaste auf das Projekt klicken und dann **Erstellen** auswählen oder **MSBuild** im NuGet-Paket über die Befehlszeile verwenden.
```
./build/msbuild /t:build [Your Project Full Path] /p:CompilerTaskAssemblyFile=Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll  /p:ASATargetsFilePath="[NuGet Package Local Path]\build\StreamAnalytics.targets"

```

Wenn ein Stream Analytics Visual Studio-Projekt erfolgreich erstellt wurde, generiert es die folgenden beiden Azure Resource Manager-Vorlagendateien im Ordner **bin/[Debug/Retail]/Deploy**: 

Azure Resource Manager-Vorlagendatei.
*       [Projektname].JobTemplate.json 

Azure Resource Manager-Parameterdatei.
*       [Projektname].JobTemplate.parameters.json   

Die Standardparameter in der „parameters.json“-Datei werden aus den Einstellungen im Visual Studio-Projekt entnommen. Wenn Sie die Bereitstellung in einer anderen Umgebung ausführen möchten, ersetzen Sie die Parameter einfach entsprechend. 
> [!NOTE] 
Für alle Anmeldeinformationen sind sämtliche Standardwerte auf NULL festgelegt. Sie **MÜSSEN** festgelegt werden, bevor Sie eine Bereitstellung in der Cloud ausführen.
```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Weitere Informationen finden Sie unter [Bereitstellen mit einer Azure Resource Manager-Vorlagendatei und Azure PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-template-deploy) und [Verwenden eines Objekts als Parameter in einer Azure Resource Manager-Vorlage](https://docs.microsoft.com/en-us/azure/architecture/building-blocks/extending-templates/objects-as-parameters).


## <a name="command-line-tool"></a>Befehlszeilentool

### <a name="build-the-project"></a>Erstellen des Projekts
Im NuGet-Paket befindet sich ein Befehlszeilentool mit dem Namen **SA.exe**. Es unterstützt Projektbuilds und lokale Tests auf beliebigen Computern. Dies können Sie in Ihrem Continuous Integration- und Continuous Deployment-Verfahren nutzen. 

Die Bereitstellungsdateien befinden sich standardmäßig unter dem aktuellen Verzeichnis. Sie können den Ausgabepfad mithilfe des OutputPath-Parameters angeben.

```
./tools/SA.exe build -Project [Your Project Full Path] [-OutputPath <outputPath>] 
```

### <a name="test-the-script-locally"></a>Lokales Testen des Skripts

Wenn in Ihrem Projekt lokale Eingabedateien in Visual Studio angegeben werden, können Sie automatisierte Tests mit dem Befehl *localrun* ausführen. Das Ausgabeergebnis wird im aktuellen Verzeichnis abgelegt.
 
```
localrun -Project [ProjectFullPath]
```

### <a name="generate-job-definition-file-to-use-with-stream-analytics-power-shell"></a>Generieren Sie eine Auftragsdefinitionsdatei für die Verwendung mit PowerShell für Stream Analytics.

Der Befehl *arm* nimmt die generierte Auftragsvorlage und die Parameterdateien für die Auftragsvorlage als Eingabe entgegen. Kombinieren Sie diese dann in einer Auftragsdefinition-JSON-Datei, die mit der Stream Analytics-PowerShell-API verwendet werden kann.

```
arm -JobTemplate <templateFilePath> -JobParameterFile <jobParameterFilePath> [-OutputFile <asaArmFilePath>]
```

```
Example
./tools/SA.exe arm -JobTemplate "ProjectA.JobTemplate.json" -JobParameterFile "ProjectA.JobTemplate.parameters.json" -OutputFile "JobDefinition.json" 
```


