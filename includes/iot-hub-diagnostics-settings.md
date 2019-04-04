---
title: Includedatei
description: Includedatei
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 6f7772eb7f2c500bbb58c391b1bc4b7a73141699
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56675754"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Aktivieren der Protokollierung mit Diagnoseeinstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.

2. Wählen Sie **Diagnoseeinstellungen** aus.

3. Wählen Sie **Diagnose aktivieren**.

   ![Aktivieren der Diagnose](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Benennen Sie die Diagnoseeinstellungen.

5. Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:

   * In einem Speicherkonto archivieren
   * An einen Event Hub streamen
   * An Log Analytics senden

6. Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Protokolle für diese Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:

   * Verbindungen
   * Gerätetelemetrie
   * C2D-Nachrichten
   * Geräte-Identitätsvorgänge
   * Dateiuploads
   * Nachrichtenweiterleitung
   * Cloud-zu-Gerät-Zwillingsvorgänge
   * Geräte-zu-Cloud-Zwillingsvorgänge
   * Zwillingsvorgänge
   * Auftragsvorgänge
   * Direkte Methoden  
   * Verteilte Ablaufverfolgung (Vorschauversion)
   * Configurations
   * Gerätestreams
   * Gerätemetriken

6. Speichern Sie die neuen Einstellungen. 

Wenn Sie Diagnoseeinstellungen mit PowerShell aktivieren möchten, verwenden Sie den folgenden Code:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseeinstellungen** angezeigt. Weitere Informationen zur Konfiguration von Diagnosen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../articles/azure-monitor/platform/diagnostic-logs-overview.md).