---
title: Herstellen einer Verbindung mit und Verwalten von Microsoft Azure Data Box Edge-Geräten über die Windows PowerShell-Schnittstelle | Microsoft-Dokumentation
description: Beschreibt, wie Sie Data Box Edge über die Windows PowerShell-Schnittstelle verbinden und dann verwalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b3effdbace2be582bfe85d0402088f8aa0d96fe7
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556452"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell-preview"></a>Verwalten eines Azure Data Box Edge-Geräts mittels Windows PowerShell (Vorschau)

Azure Data Box Edge ist eine Lösung, mit der Sie Daten verarbeiten und über ein Netzwerk an Azure senden können. In diesem Artikel werden einige der Konfigurations- und Verwaltungsaufgaben für Ihr Data Box Edge-Gerät beschrieben. Sie können das Azure-Portal, die lokale Webbenutzeroberfläche oder die Windows PowerShell-Schnittstelle verwenden, um Ihr Gerät zu verwalten.

Der Schwerpunkt dieses Artikels liegt auf den Aufgaben, die mithilfe der PowerShell-Schnittstelle ausgeführt werden.

Dieser Artikel enthält die folgenden Verfahren:

- Herstellen einer Verbindung mit der PowerShell-Schnittstelle
- Starten einer Supportsitzung
- Unterstützungspaket erstellen
- Hochladen des Zertifikats
- Zurücksetzen des Geräts
- Anzeigen von Geräteinformationen
- Abrufen von Computeprotokollen
- Überwachung und Problembehandlung von Computemodulen

> [!IMPORTANT]
> Azure Data Box Edge ist derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="connect-to-the-powershell-interface"></a>Herstellen einer Verbindung mit der PowerShell-Schnittstelle

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="start-a-support-session"></a>Starten einer Supportsitzung

[!INCLUDE [Connect to support runspace](../../includes/data-box-edge-gateway-connect-support.md)]

## <a name="create-a-support-package"></a>Unterstützungspaket erstellen

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Hochladen des Zertifikats

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="view-device-information"></a>Anzeigen von Geräteinformationen
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Zurücksetzen Ihres Geräts

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Abrufen von Computeprotokollen

Wenn die Computerolle auf Ihrem Gerät konfiguriert ist, können Sie die Computeprotokolle auch über die PowerShell-Schnittstelle abrufen.

1. [Herstellen einer Verbindung mit der PowerShell-Schnittstelle](#connect-to-the-powershell-interface).
2. Verwenden Sie `Get-AzureDataBoxEdgeComputeRoleLogs` zum Abrufen der Computeprotokolle für Ihr Gerät.

    Nachfolgend sehen Sie ein Beispiel für die Verwendung dieses Cmdlets:

    ```
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username/password" -RoleInstanceName "IotRole" -FullLogCollection
    ```
    Es folgt eine Beschreibung der verwendeten Parameter des Cmdlets: 
    - `Path`: Geben Sie einen Netzwerkpfad für die Freigabe an, in der Sie das Computeprotokollpaket erstellen möchten.
    - `Credential`: Geben Sie den Benutzernamen und das Kennwort für die Netzwerkfreigabe an.
    - `RoleInstanceName`: Geben Sie die Zeichenfolge `IotRole` für diesen Parameter ein.
    - `FullLogCollection`: Dieser Parameter stellt sicher, dass das Protokollpaket alle Computeprotokolle enthält. Standardmäßig enthält das Protokollpaket nur eine Teilmenge der Protokolle.


## <a name="next-steps"></a>Nächste Schritte

- Stellen Sie [Azure Data Box Edge](data-box-edge-deploy-prep.md) im Azure-Portal bereit.
