---
title: "Azure-VM-Erweiterung für die Leistungsdiagnose unter Windows | Microsoft-Dokumentation"
description: "Hier finden Sie eine Einführung in die Azure-VM-Erweiterung für die Leistungsdiagnose unter Windows."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: b521b4cae29578798247921331d98fc5077ad266
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Azure-VM-Erweiterung für die Leistungsdiagnose unter Windows

## <a name="summary"></a>Zusammenfassung
Die Azure-VM-Erweiterung für die Leistungsdiagnose sammelt Leistungsdiagnosedaten von Windows-VMs, führt Analysen durch und stellt Berichte zu den Ergebnissen und Empfehlungen bereit, mit denen Leistungsprobleme auf dem virtuellen Computer identifiziert und behoben werden können. Diese Erweiterung installiert ein Tool zur Problembehandlung mit dem Namen [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Voraussetzungen
### <a name="operating-systems"></a>Operating Systems
Diese Erweiterung kann unter den Betriebssystemen Windows Server 2008 R2, 2012, 2012 R2, 2016, Windows 8.1 und Windows 10 installiert werden.

## <a name="extension-schema"></a>Erweiterungsschema
Der folgende JSON-Code zeigt das Schema für die Azure-Erweiterung zur Leistungsdiagnose. Diese Erweiterung erfordert den Namen und den Schlüssel für ein Speicherkonto, in dem die Ausgabe der Diagnose und der Bericht gespeichert werden. Diese Werte sind vertraulich und sollten in einer geschützten Einstellungskonfiguration gespeichert werden. Die geschützten Einstellungsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt. Beachten Sie, dass für storageAccountName und storageAccountKey die Groß- und Kleinschreibung beachtet werden muss. Andere erforderliche Parameter werden im folgenden Abschnitt aufgeführt.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "diagnosticsTrace": "[parameters('diagnosticsTrace')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Eigenschaftswerte

|   **Name**   |**Wert/Beispiel**|       **Beschreibung**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|Version der API
|Herausgeber|Microsoft.Azure.Performance.Diagnostics|Herausgebernamespace für die Erweiterung
|Typ|AzurePerformanceDiagnostics|Typ der VM-Erweiterung
|typeHandlerVersion|1,0|Version des Erweiterungshandlers
|performanceScenario|basic|Leistungsszenario, für das Daten erfasst werden sollen. Gültige Werte sind: **basic**, **vmslow**, **azurefiles** und **custom**.
|traceDurationInSeconds|300|Dauer der Ablaufverfolgungen, wenn Optionen für die Ablaufverfolgung ausgewählt wurden.
|DiagnosticsTrace|d|Option zum Aktivieren der Diagnoseablaufverfolgung. Gültige Werte sind **d** oder ein leerer Wert. Wenn Sie diese Ablaufverfolgung nicht erfassen möchten, lassen Sie den Wert leer.
|perfCounterTrace|p|Option zum Aktivieren der Ablaufverfolgung für Leistungsindikatoren. Gültige Werte sind **p** oder ein leerer Wert. Wenn Sie diese Ablaufverfolgung nicht erfassen möchten, lassen Sie den Wert leer.
|networkTrace|n|Option zum Aktivieren von Netmon-Ablaufverfolgung. Gültige Werte sind **n** oder ein leerer Wert. Wenn Sie diese Ablaufverfolgung nicht erfassen möchten, lassen Sie den Wert leer.
|xperfTrace|x|Option zum Aktivieren der XPerf-Ablaufverfolgung. Gültige Werte sind **x** oder ein leerer Wert. Wenn Sie diese Ablaufverfolgung nicht erfassen möchten, lassen Sie den Wert leer.
|storPortTrace|s|Option zum Aktivieren der StorPort-Ablaufverfolgung. Gültige Werte sind „s“ oder ein leerer Wert. Wenn Sie diese Ablaufverfolgung nicht erfassen möchten, lassen Sie den Wert leer.
|srNumber|123452016365929|Supportticketnummer, sofern verfügbar. Lassen Sie den Wert andernfalls leer.
|requestTimeUtc|9/2/2017 11:06:00 PM|Aktueller Wert für Datum und Uhrzeit in UTC. Sie müssen diesen Wert nicht angeben, wenn Sie das Portal verwenden, um diese Erweiterung zu installieren.
|storageAccountName|mystorageaccount|Der Name des Speicherkontos zum Speichern der Diagnoseprotokolle und Ergebnisse.
|storageAccountKey|lDuVvxuZB28NNP…hAiRF3voADxLBTcc==|Schlüssel für das Speicherkonto

## <a name="install-the-extension"></a>Installieren der Erweiterung

Führen Sie die folgenden Schritte aus, um die VM-Erweiterung auf virtuellen Windows-Computern zu installieren:

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Wählen Sie den virtuellen Computer aus, auf dem Sie diese Erweiterung installieren möchten.

    ![Wählen Sie den virtuellen Computer aus.](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Wählen Sie das Blatt **Erweiterungen** aus, und klicken Sie auf die Schaltfläche **Hinzufügen**.

    ![Auswählen von „Erweiterungen“](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Wählen Sie die Azure-Erweiterung für die Leistungsdiagnose aus, lesen Sie die Geschäftsbedingungen, und klicken Sie auf die Schaltfläche **Erstellen**.

    ![Erstellen der Azure-Erweiterung für die Leistungsdiagnose](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Geben Sie die Parameterwerte für die Installation an, und klicken Sie auf **OK**, um die Erweiterung zu installieren. Weitere Informationen zu den unterstützten Szenarien für die Problembehandlung finden Sie [hier](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Installieren der Erweiterung](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Sobald die Installation erfolgreich abgeschlossen wurde, werden Sie in einer Meldung über den Erfolg der Bereitstellung informiert.

    ![Meldung über die erfolgreiche Bereitstellung](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Die Ausführung der Erweiterung wird gestartet, sobald die Bereitstellung erfolgreich war. Es dauert einige Minuten oder weniger, um die Ausführung für das Szenario „basic“ abzuschließen. Für andere Szenarien wird die Dauer bei der Installation angegeben.

## <a name="remove-the-extension"></a>Entfernen der Erweiterung
Um die Erweiterung von einem virtuellen Computer zu entfernen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an, wählen Sie den virtuellen Computer, von dem Sie diese Erweiterung entfernen möchten, und dann das Blatt „Erweiterungen“ aus. 
2. Klicken Sie in der Liste neben dem Eintrag für die Erweiterung zur Leistungsdiagnose auf (**…**), und wählen Sie „Deinstallieren“ aus.

    ![Deinstallieren der Erweiterung](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Sie können auch den Eintrag für die Erweiterung und dann die Option „Deinstallieren“ auswählen.

## <a name="template-deployment"></a>Bereitstellung mit Vorlagen
Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Das im vorherigen Abschnitt erläuterte JSON-Schema kann in einer Azure Resource Manager-Vorlage zum Ausführen der Azure-Erweiterung zur Leistungsdiagnose im Rahmen einer Bereitstellung mit einer Azure Resource Manager-Vorlage verwendet werden. Die folgende Beispielvorlage kann für die Vorlagenbereitstellung verwendet werden:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "diagnosticsTrace": {
      "type": "string",
      "defaultValue": "d"
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "diagnosticsTrace": "[parameters('diagnosticsTrace')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung
Mit dem Befehl `Set-AzureRmVMExtension` können Sie die Azure-VM-Erweiterung zur Leistungsdiagnose auf einem vorhandenen virtuellen Computer bereitstellen. Vor dem Ausführen des Befehls müssen die öffentliche und die private Konfiguration in einer PowerShell-Hashtabelle gespeichert werden.

PowerShell

````
$PublicSettings = @{ "performanceScenario":"basic","traceDurationInSeconds":300,"diagnosticsTrace":"d","perfCounterTrace":"p","networkTrace":"","xperfTrace":"","storPortTrace":"","srNumber":"","requestTimeUtc":"2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName":"mystorageaccount","storageAccountKey":"mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Informationen zu den erfassten Daten
Das Tool PerfInsights erfasst abhängig vom ausgewählten Szenario verschiedene Protokolle, die Konfiguration, Diagnosedaten usw. Weitere Informationen zu den gesammelten Daten in den einzelnen Szenarien finden Sie in der [Dokumentation zu PerfInsights](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Anzeigen und Freigeben der Ergebnisse

Die Ausgabe der Erweiterung wird standardmäßig im Ordner „log_collection“ auf dem temporären Laufwerk (in der Regel „D:\log_collection“) gespeichert. In diesem Ordner finden Sie ZIP-Dateien mit den Diagnoseprotokollen und einen Bericht mit Ergebnissen und Empfehlungen.

Die erstellte ZIP-Datei wird auch in das während der Installation angegebene Speicherkonto hochgeladen und für 30 Tage über [Shared Access Signatures (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md) freigegeben. Die Textdatei „*zipfilename*_saslink.txt“ wird ebenfalls im Ordner „log_collection“ erstellt. Diese Datei enthält den SAS-Link, der zum Herunterladen der ZIP-Datei erstellt wurde. Jede Person kann mit diesem Link die ZIP-Datei herunterladen.

Microsoft kann diesen SAS-Link verwenden, um die Diagnosedaten zur weiteren Untersuchung durch den Supporttechniker, der Ihr Supportticket bearbeitet, herunterzuladen.

Um den Bericht anzuzeigen, extrahieren Sie einfach die ZIP-Datei und öffnen die Datei **PerfInsights Report.html**.

Sie können die ZIP-Datei auch direkt aus dem Portal herunterladen, indem Sie die Erweiterung auswählen.

![Anzeigen von Statusdetails](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Der im Portal angezeigte SAS-Link funktioniert möglicherweise in einigen Fällen nicht. Grund hierfür ist eine während der Codierung und Decodierung falsch formatierte URL (verursacht durch Sonderzeichen). Zum Umgehen dieses Problems entnehmen Sie den Link direkt aus der Datei „*_saslink.txt“ auf dem virtuellen Computer.

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support
### <a name="troubleshoot"></a>Problembehandlung

- Der Bereitstellungsstatus der Erweiterung (im Infobereich) zeigt möglicherweise „Bereitstellung wird ausgeführt“ an, obwohl die Erweiterung bereits erfolgreich bereitgestellt wurde.

    Dieses Problem kann problemlos ignoriert werden, solange der Erweiterungsstatus angibt, dass die Erweiterung erfolgreich bereitgestellt wurde.
- Einige Probleme während der Installation können mithilfe der Erweiterungsprotokolle behoben werden. Die Ausgabe der Erweiterungsausführung wird in Dateien im folgenden Verzeichnis protokolliert:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).