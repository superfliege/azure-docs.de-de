---
title: "Verwenden des Validierungssteuerelements für Vorlagen zum Überprüfen von Vorlagen für Azure Stack | Microsoft-Dokumentation"
description: "Überprüfen der Vorlagen für die Bereitstellung in Azure Stack"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 6a77efb3ef4236048ff08b14346175b592493982
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Überprüfen Ihrer Vorlagen für Azure Stack mit dem Validierungssteuerelement für Vorlagen

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können das Tool für die Vorlagenvalidierung verwenden, um zu überprüfen, ob Ihre [Vorlagen](azure-stack-arm-templates.md) von Azure Resource Manager bereit für Azure Stack sind. Das Tool für die Vorlagenvalidierung ist als Teil der Azure Stack-Tools verfügbar. Laden Sie die Azure Stack-Tools herunter, indem Sie die im Artikel [Herunterladen von Tools von GitHub](azure-stack-powershell-download.md) beschriebenen Schritte befolgen. 

Verwenden Sie zum Überprüfen der Vorlagen folgende PowerShell-Module in den Ordnern **TemplateValidator** und **CloudCapabilities**: 

 - „AzureRM.CloudCapabilities.psm1“ erstellt eine JSON-Datei mit Cloudfunktionen, die die Dienste und Versionen in einer Cloud wie Azure Stack darstellt.
 - „AzureRM.TemplateValidator.psm1“ verwendet eine JSON-Datei mit Cloudfunktionen, um Vorlagen für die Bereitstellung in Azure Stack zu testen.
 
In diesem Artikel erstellen Sie eine Datei mit Cloudfunktionen und führen dann das Validierungstool aus.

## <a name="build-cloud-capabilities-file"></a>Erstellen von Dateien mit Cloudfunktionen
Bevor Sie die Vorlagenvalidierung verwenden, führen Sie das PowerShell-Modul „AzureRM.CloudCapabilities“ aus, um eine JSON-Datei zu erstellen. Wenn Sie Ihr integriertes System aktualisieren oder neue Dienste oder VM-Erweiterungen hinzufügen, sollten Sie das Modul erneut ausführen.

1.  Stellen Sie sicher, dass Sie mit Azure Stack verbunden sind. Sie können diese Schritte auf dem Azure Stack Development Kit-Host ausführen oder ein [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verwenden, um eine Verbindung von Ihrer Arbeitsstation aus herzustellen. 
2.  Importieren Sie das PowerShell-Modul „AzureRM.CloudCapabilities“:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Verwenden Sie das Cmdlet „Get-CloudCapabilities“, um Dienstversionen abzurufen und eine JSON-Datei mit Cloudfunktionen zu erstellen. Wenn Sie „-OutputPath“ nicht angeben, wird die Datei „AzureCloudCapabilities.Json“ im aktuellen Verzeichnis erstellt. Verwenden Sie den tatsächlichen Speicherort:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```             

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

Alle Warnungen oder Fehler bei der Vorlagenvalidierung werden in der PowerShell-Konsole und in einer HTML-Datei im Quellverzeichnis protokolliert. Hier finden Sie ein Beispiel für den Validierungsbericht:

![Beispielbericht für die Validierung](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameter

| Parameter | BESCHREIBUNG | Erforderlich |
| ----- | -----| ----- |
| TemplatePath | Gibt den Pfad an, um Azure Resource Manager-Vorlagen rekursiv zu suchen | Ja | 
| TemplatePattern | Gibt den Namen der abzugleichenden Vorlagendateien an | Nein  |
| CapabilitiesPath | Gibt den Pfad zu der JSON-Datei mit Cloudfunktionen an | Ja | 
| IncludeComputeCapabilities | Enthält die Auswertung von IaaS-Ressourcen wie VM-Größen und VM-Erweiterungen | Nein  |
| IncludeStorageCapabilities | Enthält die Auswertung von Speicherressourcen wie SKU-Typen | Nein  |
| Bericht | Gibt den Namen des generierten HTML-Berichts an | Nein  |
| Ausführlich | Protokolliert Fehler und Warnungen in der Konsole | Nein |


### <a name="examples"></a>Beispiele
In diesem Beispiel wird überprüft, dass alle [Azure Stack Quickstart templates (Azure Stack-Vorlagen für den Schnellstart)](https://github.com/Azure/AzureStack-QuickStart-Templates) lokal heruntergeladen wurden. Außerdem werden die VM-Größen und -Erweiterungen für die Azure Stack Development Kit-Funktionen überprüft.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```


## <a name="next-steps"></a>Nächste Schritte
 - [Deploy templates to Azure Stack (Bereitstellen von Vorlagen in Azure Stack)](azure-stack-arm-templates.md)
 - [Entwickeln von Vorlagen für Azure Stack](azure-stack-develop-templates.md)

