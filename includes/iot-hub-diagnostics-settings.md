---
title: Includedatei
description: Includedatei
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0039c4eff3571a96cba1ab36136e0a588d78eb75
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50159127"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Aktivieren der Protokollierung mit Diagnoseeinstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrer IoT Hub-Instanz.

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

6. Speichern Sie die neuen Einstellungen. 

Wenn Sie Diagnoseeinstellungen mit PowerShell aktivieren möchten, verwenden Sie den folgenden Code:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseeinstellungen** angezeigt. Weitere Informationen zur Konfiguration von Diagnosen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).