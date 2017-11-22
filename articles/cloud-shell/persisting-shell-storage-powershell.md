---
title: Beibehalten von Dateien in PowerShell in Azure Cloud Shell (Vorschauversion) | Microsoft-Dokumentation
description: "Exemplarische Vorgehensweise für das Beibehalten von Dateien in Azure Cloud Shell."
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: 66d0e20d670e49cdfe64614d1fc6f5739fde6155
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Funktionsweise von PowerShell in Azure Cloud Shell (Vorschauversion)
PowerShell in Cloud Shell (Vorschauversion) nutzt die folgende Methode, um Dateien beizubehalten: 
* Ihre angegebene Dateifreigabe wird zur direkten Interaktion mit der Freigabe als `clouddrive` in Ihrem Verzeichnis `$Home` eingebunden.

## <a name="list-cloud-drive-file-shares"></a>Auflisten von Dateifreigaben des clouddrive-Verzeichnisses
Der `Get-CloudDrive`-Befehl ruft die derzeit vom clouddrive-Verzeichnis in Cloud Shell eingebundenen Dateifreigabeinformationen ab. <br>
![Get-CloudDrive ausführen](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Aufheben der Einbindung von clouddrive
Sie können die Bereitstellung einer Dateifreigabe in Cloud Shell jederzeit aufheben. Wenn die Dateifreigabe entfernt wurde, werden Sie in der nächsten Sitzung aufgefordert, eine neue Dateifreigabe zu erstellen und einzubinden.

Der `Dismount-CloudDrive`-Befehl hebt die Einbindung einer Dateifreigabe für das aktuelle Speicherkonto auf. Durch das Aufheben der Einbindung von clouddrive wird die aktuelle Sitzung beendet. Der Benutzer wird in der nächsten Sitzung aufgefordert, eine neue Dateifreigabe zu erstellen und einzubinden.
![Dismount-CloudDrive ausführen](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Nächste Schritte
[Schnellstart für PowerShell](quickstart-powershell.md) <br>
[Erfahren Sie mehr über Azure File Storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage). <br>
[Weitere Informationen zu Speichertags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>