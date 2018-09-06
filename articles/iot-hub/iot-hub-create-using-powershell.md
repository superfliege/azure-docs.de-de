---
title: Erstellen einer Azure IoT Hub-Instanz mithilfe eines PowerShell-Cmdlets | Microsoft Docs
description: Verwenden eines PowerShell-Cmdlets zum Erstellen einer IoT Hub-Instanz
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: robinsh
ms.openlocfilehash: 7ecd35ba33d2860ba052aa27286c69985c2f7dd9
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190229"
---
# <a name="create-an-iot-hub-using-the-new-azurermiothub-cmdlet"></a>Erstellen einer IoT Hub-Instanz mithilfe des Cmdlets New-AzureRmIotHub

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Mit Azure PowerShell-Cmdlets können Sie Azure IoT Hubs erstellen und verwalten. In diesem Tutorial erfahren Sie, wie Sie eine IoT Hub-Instanz mit PowerShell erstellen.

Für diese Anleitung wird ein Azure-Abonnement benötigt. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="connect-to-your-azure-subscription"></a>Verbinden mit Ihrem Azure-Abonnement

Wenn Sie die Cloud Shell-Befehlszeile verwenden, sind Sie bereits bei Ihrem Abonnement angemeldet. Wenn Sie stattdessen PowerShell lokal ausführen, geben Sie den folgenden Befehl ein, um sich bei Ihrem Azure-Abonnement anzumelden:

```powershell
# Log into Azure account.
Login-AzureRMAccount
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Für das Bereitstellen einer IoT Hub-Instanz benötigen Sie eine Ressourcengruppe. Sie können eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen.

Wenn Sie eine neue Ressourcengruppe für Ihre IoT Hub-Instanz erstellen möchten, verwenden Sie den Befehl [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Mit diesem Beispiel wird die Ressourcengruppe **MyIoTRG1** in der Region **USA, Osten** erstellt:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="create-an-iot-hub"></a>Erstellen einer IoT Hub-Instanz

Verwenden Sie den Befehl [New-AzureRmIotHub](https://docs.microsoft.com/powershell/module/AzureRM.IotHub/New-AzureRmIotHub), um eine IoT Hub-Instanz in der Ressourcengruppe zu erstellen, die Sie im vorherigen Schritt erstellt haben. Mit diesem Beispiel wird ein **S1**-Hub mit dem Namen **MyTestIoTHub** in der Region **USA, Osten** erstellt:

```azurepowershell-interactive
New-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub `
    -SkuName S1 -Units 1 `
    -Location "East US"
```

Der Name der IoT Hub-Instanz muss global eindeutig sein.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Mit dem Befehl [Get-AzureRmIotHub](https://docs.microsoft.com/powershell/module/AzureRM.IotHub/Get-AzureRmIotHub) können Sie alle IoT Hub-Instanzen in Ihrem Abonnement auflisten:

```azurepowershell-interactive
Get-AzureRmIotHub
```

Dieses Beispiel zeigt die IoT Hub-Instanz vom Typ „S1 Standard“, die Sie im vorherigen Schritt erstellt haben.

Sie können die IoT Hub-Instanz mit dem Befehl [Remove-AzureRmIotHub](https://docs.microsoft.com/powershell/module/azurerm.iothub/remove-azurermiothub) löschen:

```azurepowershell-interactive
Remove-AzureRmIotHub `
    -ResourceGroupName MyIoTRG1 `
    -Name MyTestIoTHub
```

Alternativ können Sie eine Ressourcengruppe und alle darin enthaltenen Ressourcen mit dem Befehl [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Remove-AzureRmResourceGroup) entfernen:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name MyIoTRG1
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine IoT Hub-Instanz mit einem PowerShell-Cmdlet bereitgestellt haben, möchten Sie vielleicht mehr wissen. Lesen Sie die folgenden Artikel:

* [PowerShell-Cmdlets zur Verwendung mit Ihrer IoT Hub-Instanz](https://docs.microsoft.com/powershell/module/azurerm.iothub/)

* [IoT Hub-Ressourcenanbieter-REST-API](https://docs.microsoft.com/rest/api/iothub/iothubresource)

Weitere Informationen zum Entwickeln für IoT Hub finden Sie in folgenden Artikeln:

* [Einführung in das C SDK](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDKs](iot-hub-devguide-sdks.md)

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Bereitstellen von KI auf Edge-Geräten mit Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)