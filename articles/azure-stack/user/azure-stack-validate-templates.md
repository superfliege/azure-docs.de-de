---
title: "Verwenden des Validierungssteuerelements für Vorlagen zum Überprüfen von Vorlagen für Azure Stack | Microsoft-Dokumentation"
description: "Überprüfen der Vorlagen für die Bereitstellung in Azure Stack"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: c99e5ebc2612e10f42bddbbd2f1c17d7404305d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Überprüfen Ihrer Vorlagen für Azure Stack mit dem Validierungssteuerelement für Vorlagen

*Gilt für: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können das Tool für die Vorlagenvalidierung verwenden, um zu überprüfen, ob Ihre [Vorlagen](azure-stack-arm-templates.md) von Azure Resource Manager bereit für Azure Stack sind. Das Tool für die Vorlagenvalidierung ist als Teil der Azure Stack-Tools verfügbar. Laden Sie die Azure Stack-Tools herunter, indem Sie die im Artikel [Herunterladen von Tools von GitHub](azure-stack-powershell-download.md) beschriebenen Schritte befolgen. 

Verwenden Sie zum Überprüfen der Vorlagen folgende PowerShell-Module und die JSON-Datei, die sich in den Ordnern **TemplateValidator** und **CloudCapabilities** befinden: 

 - „AzureRM.CloudCapabilities.psm1“ erstellt eine JSON-Datei mit Cloudfunktionen, die die Dienste und Versionen in einer Cloud wie Azure Stack darstellt.
 - „AzureRM.TemplateValidator.psm1“ verwendet eine JSON-Datei mit Cloudfunktionen, um Vorlagen für die Bereitstellung in Azure Stack zu testen.
 - „AzureStackCloudCapabilities_with_AddOns_20170627.json“ ist eine Standarddatei mit Cloudfunktionen.  Sie können eine eigene Datei erstellen, oder diese für die ersten Schritte verwenden. 

In diesem Thema führen Sie die Überprüfung Ihrer Vorlagen aus und erstellen optional eine Datei mit Cloudfunktionen.

## <a name="validate-templates"></a>Überprüfen von Vorlagen
In diesen Schritten überprüfen Sie Vorlagen, indem Sie das PowerShell-Modul „AzureRM.TemplateValidator“ verwenden. Sie können Ihre eigenen Vorlagen verwenden, oder die [Azure Stack Quickstart templates (Azure Stack-Vorlagen für den Schnellstart)](https://github.com/Azure/AzureStack-QuickStart-Templates) verwenden.

1.  Importieren Sie das PowerShell-Modul „AzureRM.TemplateValidator.psm1“:
    
    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2.  Führen Sie das Validierungssteuerelement für Vorlagen aus:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Alle Warnungen oder Fehler bei der Vorlagenvalidierung werden in der PowerShell-Konsole und in einer HTML-Datei im Quellverzeichnis protokolliert. Die Ausgabe des Validierungsberichts kann z.B. folgendermaßen aussehen:

![Beispielbericht für die Validierung](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameter

| Parameter | Beschreibung | Erforderlich |
| ----- | -----| ----- |
| TemplatePath | Gibt den Pfad an, um Resource Manager-Vorlagen rekursiv zu suchen | Ja | 
| TemplatePattern | Gibt den Namen der abzugleichenden Vorlagendateien an | Nein |
| CapabilitiesPath | Gibt den Pfad zu der JSON-Datei mit Cloudfunktionen an | Ja | 
| IncludeComputeCapabilities | Enthält die Auswertung von IaaS-Ressourcen wie VM-Größen und VM-Erweiterungen | Nein |
| IncludeStorageCapabilities | Enthält die Auswertung von Speicherressourcen wie SKU-Typen | Nein |
| Bericht | Gibt den Namen des generierten HTML-Berichts an | Nein |
| Ausführlich | Protokolliert Fehler und Warnungen in der Konsole | Nein|


### <a name="examples"></a>Beispiele
In diesem Beispiel wird überprüft, dass alle [Azure Stack Quickstart templates (Azure Stack-Vorlagen für den Schnellstart)](https://github.com/Azure/AzureStack-QuickStart-Templates) lokal heruntergeladen wurden. Außerdem werden die VM-Größen und -Erweiterungen für die Azure Stack Development Kit-Funktionen überprüft.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="build-cloud-capabilities-file"></a>Erstellen von Dateien mit Cloudfunktionen
Die heruntergeladenen Dateien enthalten eine Standarddatei (*AzureStackCloudCapabilities_with_AddOns_20170627.json*), die beschreibt, welche Dienstversionen im Azure Stack Development Kit mit installierten PaaS-Diensten verfügbar sind.  Wenn Sie zusätzliche Ressourcenanbieter installieren, können Sie das PowerShell-Modul „AzureRM.CloudCapabilities“ verwenden, um eine JSON-Datei einschließlich der neuen Dienste zu erstellen.  

1.  Stellen Sie sicher, dass Sie mit Azure Stack verbunden sind.  Diese Schritte können über den Azure Stack Development Kit-Host ausgeführt werden. Sie können jedoch auch [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verwenden, um eine Verbindung von Ihrer Arbeitsstation aus herzustellen. 
2.  Importieren Sie das PowerShell-Modul „AzureRM.CloudCapabilities“:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Verwenden Sie das Cmdlet „Get-CloudCapabilities“, um Dienstversionen abzurufen und eine JSON-Datei mit Cloudfunktionen zu erstellen:

    ```PowerShell
    Get-AzureRMCloudCapabilities -Location 'local' -Verbose
    ```             


## <a name="next-steps"></a>Nächste Schritte
 - [Deploy templates to Azure Stack (Bereitstellen von Vorlagen in Azure Stack)](azure-stack-arm-templates.md)
 - [Entwickeln von Vorlagen für Azure Stack](azure-stack-develop-templates.md)

