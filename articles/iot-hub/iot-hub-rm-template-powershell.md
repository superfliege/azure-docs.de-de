---
title: Erstellen einer Azure IoT Hub-Instanz mithilfe einer Vorlage (PowerShell) | Microsoft Docs
description: Erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage in PowerShell eine IoT Hub-Instanz erstellen.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.openlocfilehash: fcc9af9e614b0a1b7977ba18f3147fddab8b7b7d
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045053"
---
# <a name="create-an-iot-hub-using-azure-resource-manager-template-powershell"></a>Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Sie können den Azure-Ressourcen-Manager verwenden, um Azure IoT Hubs programmgesteuert zu erstellen und zu verwalten. In diesem Tutorial erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage und PowerShell einen IoT Hub erstellen.

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel behandelt die Verwendung des Azure Resource Manager-Bereitstellungsmodells.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. <br/>Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure PowerShell 1.0][lnk-powershell-install] oder höher.

> [!TIP]
> Der Artikel [Verwenden von Azure PowerShell mit dem Azure Resource Manager][lnk-powershell-arm] enthält weitere Informationen zum Verwenden von PowerShell und Azure Resource Manager-Vorlagen zum Erstellen von Azure-Ressourcen.

## <a name="connect-to-your-azure-subscription"></a>Verbinden mit Ihrem Azure-Abonnement

Geben Sie in einer PowerShell-Befehlszeile den folgenden Befehl zur Anmeldung bei Ihrem Azure-Abonnement ein:

```powershell
Connect-AzAccount
```

Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch die Anmeldung bei Azure Zugriff auf alle Azure-Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Führen Sie den folgenden Befehl aus, um eine Liste der Azure-Abonnements anzuzeigen, die Sie verwenden können:

```powershell
Get-AzSubscription
```

Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, das Sie zum Ausführen der Befehle zum Erstellen Ihres IoT Hubs verwenden möchten. Sie können entweder den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

```powershell
Select-AzSubscription `
    -SubscriptionName "{your subscription name}"
```

Mit den folgenden Befehlen können Sie ermitteln, wo Sie einen IoT-Hub und die derzeit unterstützten API-Versionen bereitstellen können:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Erstellen Sie mit dem folgenden Befehl an einem Speicherort, der IoT Hub unterstützt, eine Ressourcengruppe, die Ihren IoT-Hub enthalten soll. Dieses Beispiel erstellt eine Ressourcengruppe namens **MyIoTRG1**:

```powershell
New-AzResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-a-template-to-create-an-iot-hub"></a>Einsenden einer Vorlage zum Erstellen eines IoT Hubs

Verwenden Sie eine JSON-Vorlage, um einen IoT Hub in der Ressourcengruppe zu erstellen. Sie können auch eine Azure Resource Manager-Vorlage verwenden, um Änderungen an einem vorhandenen IoT Hub vorzunehmen.

1. Erstellen Sie mithilfe eines Texteditors eine Azure Resource Manager-Vorlage namens **template.json** mit der folgenden Ressourcendefinition, um einen neuen Standard-IoT Hub zu erstellen. In diesem Beispiel wird der IoT Hub in der Region **USA, Osten** hinzugefügt, es werden zwei Consumergruppen (**cg1** und **cg2**) auf dem Event Hub-kompatiblen Endpunkt erstellt, und es wird die API-Version **2016-02-03** verwendet. Diese Vorlage erwartet auch, dass Sie den IoT Hub-Namen als Parameter **hubName**übergeben. Die aktuelle Liste der Standorte, die IoT Hub unterstützen, finden Sie unter [Azure-Status][lnk-status].

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Speichern Sie die Azure Resource Manager-Vorlagendatei auf Ihrem lokalen Computer. In diesem Beispiel wird davon ausgegangen, dass Sie sie im Ordner **c:\templates** speichern.

3. Führen Sie den folgenden Befehl zum Bereitstellen Ihres neuen IoT-Hubs aus, der den Namen des IoT-Hubs als Parameter übergibt. In diesem Beispiel lautet der Name des IoT-Hubs `abcmyiothub`. Der Name des IoT-Hubs muss global eindeutig sein:

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```
   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. Die Ausgabe zeigt die Schlüssel für den IoT-Hub an, den Sie erstellt haben.

5. Um zu überprüfen, ob Ihre Anwendung den neuen IoT-Hub hinzugefügt hat, besuchen Sie das [Azure-Portal][lnk-azure-portal], und zeigen Sie die Liste der Ressourcen an. Verwenden Sie alternativ das PowerShell-Cmdlet **Get-AzResource**.

> [!NOTE]
> Diese Beispielanwendung fügt einen für Sie kostenpflichtigen S1-Standard-IoT Hub hinzu. Sie können den IoT-Hub nach Abschluss des Beispiels über das [Azure-Portal][lnk-azure-portal] oder mithilfe des PowerShell-Cmdlets **Remove-AzResource** löschen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen IoT Hub mithilfe einer Azure Resource Manager-Vorlage mit PowerShell bereitgestellt haben, möchten Sie vielleicht mehr wissen:

* Informieren Sie sich über die Funktionen der [IoT Hub-Ressourcenanbieter-REST-API][lnk-rest-api].
* Weitere Informationen zu den Funktionen des Azure Resource Manager finden Sie unter [Übersicht über Azure Resource Manager][lnk-azure-rm-overview].
* Informationen zur JSON-Syntax und zu den Eigenschaften, die in Vorlagen verwendet werden sollen, finden Sie unter [Microsoft.Devices resource types](/azure/templates/microsoft.devices/iothub-allversions) (Microsoft.Devices-Ressourcentypen).

Weitere Informationen zum Entwickeln für IoT Hub finden Sie in folgenden Artikeln:

* [Einführung in das C SDK][lnk-c-sdk]
* [Azure IoT SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Deploy Azure IoT Edge on a simulated device in Linux - preview][lnk-iotedge] (Bereitstellen von Azure IoT Edge auf einem simulierten Gerät in Linux – Vorschauversion)

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: /powershell/azure/install-Az-ps
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../azure-resource-manager/manage-resources-powershell.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
