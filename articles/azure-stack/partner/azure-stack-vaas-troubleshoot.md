---
title: Problembehandlung bei Validation-as-a-Service in Azure Stack | Microsoft-Dokumentation
description: Problembehandlung für Validation-as-a-Service in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: ed070ac4fdf9ccca1b1b4b99b8031bc3fd035779
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160148"
---
# <a name="troubleshoot-validation-as-a-service"></a>Problembehandlung für Validation-as-a-Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Die folgenden allgemeinen Probleme treten unabhängig von Softwareversionen und deren Lösungen auf.

## <a name="the-portal-shows-local-agent-in-debug-mode"></a>Das Portal zeigt den lokalen Agent im Debugmodus.

Dies liegt wahrscheinlich daran, dass der Agent aufgrund einer instabilen Netzwerkverbindung keine Takte an den Dienst senden kann. Ein Takt wird alle fünf Minuten gesendet. Wenn der Dienst innerhalb von 15 Minuten keinen Takt erhält, berücksichtigt der Dienst den Agent als inaktiv, und es werden keine weiteren Tests für ihn geplant. Überprüfen Sie die Fehlermeldung in der *Agenthost.log*-Datei im Verzeichnis, in dem der Agent gestartet wurde.

> [!Note] 
> Zwar werden alle Tests, die bereits auf dem Agent ausgeführt werden, auch weiterhin ausgeführt, allerdings treten beim Aktualisieren von Teststatus oder Protokollen Fehlermeldungen auf, wenn der Takt vor Ende des Tests nicht wiederhergestellt wird. Der Test wird immer als **wird ausgeführt** angezeigt und muss abgebrochen werden.

## <a name="agent-process-on-machine-was-shut-down-while-executing-test-what-to-expect"></a>Der Agent-Prozess auf dem Computer wurde während der Ausführung des Tests beendet. Was können Sie erwarten?

Wenn der Agent-Prozess beispielsweise nicht ordnungsgemäß beendet wird, der Computer neu gestartet und der Prozess abgebrochen wird (STRG+C im Fenster „Agent“ als nicht ordnungsgemäßes Beenden angesehen wird), wird der darauf ausgeführte Test auch weiterhin als **wird ausgeführt** angezeigt. Wenn der Agent neu gestartet wird, aktualisiert der Agent den Status des Tests nach **abgebrochen**. Wenn der Agent nicht neu gestartet wird, wird der Test als **wird ausgeführt** angezeigt, und Sie müssen den Test manuell abbrechen.

> [!Note] 
> Tests in einem Workflow werden für eine sequenzielle Ausführung geplant. **Ausstehende** Tests werden nicht ausgeführt, bis Tests im Zustand **wird ausgeführt** im gleichen Workflow abgeschlossen werden.

## <a name="handle-slow-network-connectivity"></a>Behandeln langsamer Netzwerkverbindung

Sie können das PIR-Image in eine Freigabe in Ihrem lokalen Rechenzentrum herunterladen. Dann können Sie das Image überprüfen.

<!-- This is from the appendix to the Deploy local agent topic. -->

### <a name="download-pir-image-to-local-share-in-case-of-slow-network-traffic"></a>Herunterladen eines PIR-Images in die lokale Freigabe bei einem langsamen Netzwerkdatenverkehr

1. Download von AzCopy von: [vaasexternaldependencies(AzCopy)](https://vaasexternaldependencies.blob.core.windows.net/prereqcomponents/AzCopy.zip)

2. Extrahieren Sie „AzCopy.zip“, und wechseln Sie zum Verzeichnis mit „AzCopy.exe“.

3. Öffnen Sie Windows PowerShell über eine Eingabeaufforderung mit erhöhten Rechten. Führen Sie die folgenden Befehle aus:

```PowerShell  
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterFullBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Server2016DatacenterCoreBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'WindowsServer2012R2DatacenterBYOL.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1404LTS.vhd' /NC:12 /V:azcopylog.log /Y
    .\azcopy.exe /Source:'https://azurestacktemplate.blob.core.windows.net/azurestacktemplate-public-container' /Dest:'<LocalFileShare>' /Pattern:'Ubuntu1604-20170619.1.vhd' /NC:12 /V:azcopylog.log /Y
```

> [!Note]  
> LocalFileShare ist der Freigabepfad oder der lokale Pfad.

### <a name="verifying-pir-image-file-hash-value"></a>Überprüfen des Hashwerts von PIR-Imagedateien

Sie können das **Get-HashFile**-Cmdlet verwenden, um den Hashwert für die heruntergeladenen PIR-Imagedateien abzurufen und die Integrität der Images zu überprüfen.

| Dateiname | SHA256 |
|---------------------------------------|------------------------------------------------------------------|
| Server2016DatacenterFullBYOL.vhd | 6ED58DCA666D530811A1EA563BA509BF9C29182B902D18FCA03C7E0868F733E9 |
| WindowsServer2012R2DatacenterBYOL.vhd | 9792CBF742870B1730B9B16EA814C683A8415EFD7601DDB6D5A76D0964767028 |
| Server2016DatacenterCoreBYOL.vhd | 5E80E1A6721A48A10655E6154C1B90E320DF5558487D6A0D7BFC7DCD32C4D9A5 |
| Ubuntu1404LTS.vhd | B24CDD12352AAEBC612A4558AB9E80F031A2190E46DCB459AF736072742E20E0 |
| Ubuntu1604-20170619.1.vhd | C481B88B60A01CBD5119A3F56632A2203EE5795678D3F3B9B764FFCA885E26CB |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
