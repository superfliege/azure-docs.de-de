---
title: Verwenden eines Validierungstools für Vorlagen zum Überprüfen von Vorlagen für Azure Stack | Microsoft-Dokumentation
description: Überprüfen der Vorlagen für die Bereitstellung in Azure Stack
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 88fac41ce2c9fa0c5569beae02ab90a507c89a34
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358641"
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Überprüfen Ihrer Vorlagen für Azure Stack mit dem Validierungstool für Vorlagen

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können das Tool für die Vorlagenvalidierung verwenden, um zu überprüfen, ob Ihre [Vorlagen](azure-stack-arm-templates.md) von Azure Resource Manager in Azure Stack bereitgestellt werden können. Das Tool für die Vorlagenvalidierung ist als Teil der Azure Stack-Tools verfügbar. Laden Sie die Azure Stack-Tools herunter, indem Sie die im Artikel [Herunterladen von Tools von GitHub](azure-stack-powershell-download.md) beschriebenen Schritte befolgen.

## <a name="overview"></a>Übersicht

Um eine Vorlage zu überprüfen, müssen Sie zuerst eine Datei mit Cloudfunktionen erstellen und dann das Validierungstool ausführen. Sie verwenden die folgenden PowerShell-Module aus den Azure Stack-Tools:

- Im Ordner **TemplateValidator**:<br>         „AzureRM.CloudCapabilities.psm1“ erstellt eine JSON-Datei mit Cloudfunktionen, die die Dienste und Versionen in einer Azure Stack-Cloud darstellt.
- Im Ordner **CloudCapabilities**:<br>
„AzureRM.TemplateValidator.psm1“ verwendet eine JSON-Datei mit Cloudfunktionen, um Vorlagen für die Bereitstellung in Azure Stack zu testen.

## <a name="build-the-cloud-capabilities-file"></a>Erstellen der Datei mit Cloudfunktionen

Bevor Sie die Vorlagenvalidierung verwenden, führen Sie das PowerShell-Modul „AzureRM.CloudCapabilities“ aus, um eine JSON-Datei zu erstellen.

>[!NOTE]
>Wenn Sie Ihr integriertes System aktualisieren oder neue Dienste oder virtuelle Erweiterungen hinzufügen, sollten Sie dieses Modul erneut ausführen.

1. Stellen Sie sicher, dass Sie mit Azure Stack verbunden sind. Sie können diese Schritte auf dem Azure Stack Development Kit-Host ausführen oder ein [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verwenden, um eine Verbindung von Ihrer Arbeitsstation aus herzustellen.
2. Importieren Sie das PowerShell-Modul „AzureRM.CloudCapabilities“:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Verwenden Sie das Cmdlet „Get-CloudCapabilities“, um Dienstversionen abzurufen und eine JSON-Datei mit Cloudfunktionen zu erstellen. Wenn Sie **-OutputPath** nicht angeben, wird die Datei „AzureCloudCapabilities.Json“ im aktuellen Verzeichnis erstellt. Verwenden Sie den tatsächlichen Speicherort:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Überprüfen von Vorlagen

Mit diesen Schritten überprüfen Sie Vorlagen, indem Sie das PowerShell-Modul „AzureRM.TemplateValidator“ verwenden. Sie können Ihre eigenen Vorlagen verwenden, oder die [Azure Stack Quickstart templates (Azure Stack-Vorlagen für den Schnellstart)](https://github.com/Azure/AzureStack-QuickStart-Templates) verwenden.

1. Importieren Sie das PowerShell-Modul „AzureRM.TemplateValidator.psm1“:

    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Führen Sie das Validierungssteuerelement für Vorlagen aus:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Warnungen oder Fehler bei der Vorlagenvalidierung werden in der PowerShell-Konsole und in einer HTML-Datei im Quellverzeichnis protokolliert. Die folgende Bildschirmaufnahme zeigt ein Beispiel für einen Validierungsbericht:

![Vorlagenvalidierungsbericht](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parameter

Das Validierungssteuerelement für Vorlagen unterstützt die folgenden Parameter.

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

In diesem Beispiel werden alle [Vorlagen aus dem Azure Stack-Schnellstart](https://github.com/Azure/AzureStack-QuickStart-Templates) überprüft, die in den lokalen Speicher heruntergeladen wurden. Im Beispiel werden auch die Größen und Erweiterungen virtueller Computer anhand der Azure Stack Development Kit-Funktionen überprüft.

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
