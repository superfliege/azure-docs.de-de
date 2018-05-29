---
title: Beibehalten von Dateien in PowerShell in Azure Cloud Shell (Vorschauversion) | Microsoft-Dokumentation
description: Exemplarische Vorgehensweise für das Beibehalten von Dateien in Azure Cloud Shell.
services: azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: b87c4a408393e4ae341898e8cfa23e9acbcb4fc2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
ms.locfileid: "32154397"
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Funktionsweise von PowerShell in Azure Cloud Shell (Vorschauversion)
PowerShell in Cloud Shell (Vorschauversion) nutzt die folgende Methode, um Dateien beizubehalten: 
* Ihre angegebene Azure-Dateifreigabe wird zur direkten Interaktion mit der Freigabe als `clouddrive` in Ihr Verzeichnis `$Home` eingebunden.

## <a name="list-clouddrive-azure-file-shares"></a>Auflisten von `clouddrive`-Azure-Dateifreigaben
Der Befehl `Get-CloudDrive` ruft Informationen zu den derzeit von `clouddrive` in Cloud Shell eingebundenen Azure-Dateifreigaben ab. <br>
![Get-CloudDrive ausführen](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-clouddrive"></a>Aufheben der Bereitstellung von `clouddrive`
Sie können die Einbindung einer Azure-Dateifreigabe in Cloud Shell jederzeit aufheben. Wenn die Azure-Dateifreigabe entfernt wurde, werden Sie in der nächsten Sitzung aufgefordert, eine neue Azure-Dateifreigabe zu erstellen und einzubinden.

Der Befehl `Dismount-CloudDrive` hebt die Einbindung einer Azure-Dateifreigabe für das aktuelle Speicherkonto auf. Durch das Aufheben der Einbindung von `clouddrive` wird die aktuelle Sitzung beendet. Der Benutzer wird in der nächsten Sitzung aufgefordert, eine neue Azure-Dateifreigabe zu erstellen und einzubinden.
![Dismount-CloudDrive ausführen](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Nächste Schritte
[Schnellstart für PowerShell](quickstart-powershell.md) <br>
[Informationen zu Azure Files](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Weitere Informationen zu Speichertags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>