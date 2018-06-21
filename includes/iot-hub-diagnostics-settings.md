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
ms.openlocfilehash: 1c7f006c066a4f1505a642af04a1ef027fde0a44
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34666941"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Aktivieren der Protokollierung mit Diagnoseeinstellungen

1. Melden Sie sich beim [Azure-Portal][lnk-portal] an, und navigieren Sie zu Ihrem IoT Hub.
1. Wählen Sie **Diagnoseeinstellungen** aus.
1. Wählen Sie **Diagnose aktivieren**.

   ![Aktivieren der Diagnose][1]

1. Benennen Sie die Diagnoseeinstellungen.
1. Wählen Sie aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der drei Optionen auswählen:
   * In einem Speicherkonto archivieren
   * An einen Event Hub streamen
   * An Log Analytics senden
1. Wählen Sie aus, welche Vorgänge Sie überwachen möchten, und aktivieren Sie die Protokolle für diese Vorgänge. Die Vorgänge, für die über die Diagnoseeinstellungen Berichte erstellt werden können, sind die folgenden:
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
1. Speichern Sie die neuen Einstellungen. 

Wenn Sie Diagnoseeinstellungen mit PowerShell aktivieren möchten, verwenden Sie den folgenden Code:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseeinstellungen** angezeigt. Weitere Informationen zur Konfiguration von Diagnosen finden Sie unter [Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen][lnk-diagnostics-settings].

<!-- Images -->
[1]: ./media/iot-hub-diagnostics-settings/turnondiagnostics.png

<!-- Links -->
[lnk-portal]: https://portal.azure.com
[lnk-diagnostics-settings]: ../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
