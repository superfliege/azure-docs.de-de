---
title: Diagnostizieren und Behandeln von Problemen bei der Trennung von Geräteverbindungen mit Azure IoT Hub
description: Erfahren Sie, wie Sie häufige Fehler anhand der Gerätekonnektivität für Azure IoT Hub diagnostizieren und behandeln.
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: jlian
ms.openlocfilehash: 5bd66e3cb3902665aab9245a524a2bec6f57dc8c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2018
ms.locfileid: "42141353"
---
# <a name="detect-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Erkennen und Behandeln von Problemen bei der Trennung von Geräteverbindungen mit Azure IoT Hub

Verbindungsprobleme bei IoT-Geräten können schwierig zu behandeln sein, weil es viele mögliche Fehlerquellen gibt. Geräteseitige Anwendungslogik, physikalische Netzwerke, Protokolle, Hardware und Azure IoT Hub können Probleme verursachen. Dieses Dokument enthält Empfehlungen zum Erkennen und Behandeln von cloudseitigen Gerätekonnektivitätsproblemen (im Gegensatz zu geräteseitigen Problemen).

## <a name="get-alerts-and-error-logs"></a>Abrufen von Warnungen und Fehlerprotokollen

Verwenden Sie Azure Monitor, um Warnungen abzurufen und Protokolle zu schreiben, wenn die Geräteverbindung unterbrochen wird.

### <a name="turn-on-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen 

Aktivieren Sie Diagnosen für IoT Hub, um Geräteverbindungsereignisse und -fehler zu protokollieren. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu Ihrem IoT Hub.
1. Wählen Sie **Diagnoseeinstellungen** aus.
1. Wählen Sie dann **Diagnose aktivieren** aus.
1. Aktivieren Sie **Verbindungen**, damit Protokolle gesammelt werden. 
1. Um die Analyse zu vereinfachen, aktivieren Sie auf **An Log Analytics senden** ([siehe Preisübersicht](https://azure.microsoft.com/pricing/details/log-analytics/)). Ein späteres Beispiel in diesem Artikel verwendet Log Analytics.

   ![Empfohlene Einstellungen][2]

Weitere Informationen finden Sie unter [Schnelle Überwachung der Integrität von Azure IoT Hub und Diagnose von Problemen](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-the-connected-devices-count-metric"></a>Einrichten von Warnungen für die Metrik „Verbundene Geräte“

Um Warnungen zu erhalten, wenn die Verbindung von Geräten getrennt wird, konfigurieren Sie Warnungen in der Metrik *Verbundene Geräte*. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu Ihrer IoT Hub-Instanz.
1. Wählen Sie **Warnungen (klassisch)** aus.
1. Klicken Sie auf **Metrikwarnung hinzufügen (klassisch)**.
1. Füllen Sie das Formular aus, und wählen Sie **OK** aus. 

   ![Empfohlene Metrikwarnung][3]

Weitere Informationen finden Sie unter [Was sind klassische Warnungen in Microsoft Azure?](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

## <a name="resolve-connectivity-errors"></a>Lösen von Konnektivitätsproblemen

Wenn Diagnoseprotokolle und Warnungen für verbundene Geräte aktiviert sind, erhalten Sie Warnungen, wenn Probleme auftreten. In diesem Abschnitt wird beschrieben, wie Sie häufig auftretende Probleme beheben, wenn Sie eine Warnung erhalten haben. In der folgenden Anleitung wird davon ausgegangen, dass Sie für Ihre Diagnoseprotokolle Log Analytics eingerichtet haben. 

1. Rufen Sie Ihren Arbeitsbereich für **Log Analytics** im Azure-Portal auf.
1. Klicken Sie auf **Protokollsuche**.
1. Um Verbindungsfehlerprotokolle für IoT Hub zu isolieren, geben Sie diese Abfrage in das Feld ein und drücken Sie dann **Ausführen**.

    ```
    search *
    | where ( Type == "AzureDiagnostics" and ResourceType == "IOTHUBS")
    | where ( Category == "Connections" and Level == "Error")
    ```

1. Wenn es Ergebnisse gibt, suchen Sie nach `OperationName`, `ResultType` (Fehlercode) und `ResultDescription` (Fehlermeldung), um mehr Details über den Fehler zu erhalten.

   ![Beispiel für ein Fehlerprotokoll][4]

1. Verwenden Sie diese Tabelle, um häufig auftretende Fehler zu verstehen und zu beheben.

    | Error | Grundursache | Lösung |
    |---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 404104 DeviceConnectionClosedRemotely | Die Verbindung wurde durch das Gerät geschlossen, aber IoT Hub kennt nicht die Ursache. Häufige Ursachen sind ein MQTT/AMQP-Timeout und eine Unterbrechung der Internetverbindung. | [Testen Sie die Verbindung](tutorial-connectivity.md), um zu überprüfen, ob das Gerät eine Verbindung mit IoT Hub herstellen kann. Wenn die Verbindung in Ordnung ist, die Geräteverbindung aber zeitweilig unterbrochen wird, stellen Sie sicher, dass Sie die Keep-Alive-Gerätelogik für das von Ihnen gewählte Protokoll (MQTT/AMPQ) korrekt implementieren. |
    | 401003 IoTHubUnauthorized | IoT Hub konnte die Verbindung nicht authentifizieren. | Stellen Sie sicher, dass die SAS oder andere verwendete Sicherheitstoken nicht abgelaufen sind. [Azure IoT SDKs](iot-hub-devguide-sdks.md) generieren Token automatisch, ohne dass eine spezielle Konfiguration erforderlich ist. |
    | 409002 LinkCreationConflict | Es gibt mehrere Verbindungen für ein einziges Gerät. Wenn eine neue Verbindungsanforderung für ein Gerät kommt, schließt IoT Hub die vorherige Verbindung mit dieser Fehlermeldung. | Im Allgemeinen erkennt ein Gerät das Trennen einer Verbindung und versucht, die Verbindung wiederherzustellen. IoT Hub hat die getrennte Verbindung jedoch noch nicht berücksichtigt, schließt daher die vorherige Verbindung und protokolliert diesen Fehler. Dieser Fehler tritt normalerweise als Nebenwirkung eines anderen vorübergehenden Problems auf. Suchen Sie also nach anderen Fehlern in den Protokollen, um eine Problembehandlung durchführen. Stellen Sie andernfalls nur dann eine neue Verbindungsanforderung, wenn die Verbindung unterbrochen wird. |
    | 500001 ServerError | Bei IoT Hub ist ein serverseitiges Problem aufgetreten. In den meisten Fällen ist das Problem vorübergehend. Während das IoT Hub-Team daran arbeitet, [die SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/) zu verwalten, können kleine Teilmengen von IoT Hub-Knoten gelegentlich vorübergehende Fehler aufweisen. Wenn Ihr Gerät versucht, eine Verbindung zu einem Knoten herzustellen, der Probleme aufweist, erhalten Sie diese Fehlermeldung. | Um den vorübergehenden Fehler zu beheben, führen Sie einen erneuten Versuch vom Gerät aus. Um [Wiederholungsversuche automatisch zu verwalten](iot-hub-reliability-features-in-sdks.md#connection-and-retry), stellen Sie sicher, dass Sie die neueste Version des [Azure IoT SDKs](iot-hub-devguide-sdks.md) verwenden.<br><br>Die Best Practices für die Behandlung vorübergehender Fehler und Wiederholungsversuche finden Sie unter [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults).  <br><br>Wenn das Problem nach mehreren Versuchen weiterhin besteht, überprüfen Sie [Ressourcenintegrität](iot-hub-monitor-resource-health.md#use-azure-resource-health) und [Azure-Status](https://azure.microsoft.com/status/history/), um festzustellen, ob IoT Hub ein bekanntes Problem aufweist. Wenn keine bekannten Probleme vorliegen, und das Problem fortbesteht, [wenden Sie sich an den Support](https://azure.microsoft.com/support/options/), um weitere Unterstützung zu erhalten. |
    | 500008 GenericTimeout | IoT Hub konnte die Verbindungsanforderung nicht vor dem Timeout abschließen. Wie 500001 ServerError ist dieser Fehler wahrscheinlich vorübergehend. | Befolgen Sie die Schritte zur Fehlerbehandlung für 500001 ServerError, um die Grundursache zu ermitteln und diesen Fehler zu beheben.|

## <a name="other-steps-to-try"></a>Weitere Hilfestellungen

Wenn die oben genannten Schritte nicht hilfreich waren, können Sie die folgenden Optionen ausprobieren:

* Wenn Sie Zugriff auf die problematischen Geräte haben, entweder physisch oder remote (wie SSH), folgen Sie der [Anleitung zur geräteseitigen Problembehandlung](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices), um die Fehlerbehebung fortzusetzen.
* Überprüfen Sie, ob Ihre Geräte im Azure-Portal > Mein IoT Hub > IoT-Geräte **Aktiviert** sind.
* Weitere Hilfe erhalten Sie im [Azure IoT Hub-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), im [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub) oder vom [Azure-Support](https://azure.microsoft.com/support/options/).

Um die Dokumentation allgemein zu verbessern, hinterlassen Sie unten einen Kommentar, wenn Ihnen diese Anleitung nicht geholfen hat.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Beheben von vorübergehenden Problemen finden Sie unter [Behandlung vorübergehender Fehler](/azure/architecture/best-practices/transient-faults).
* Mehr über das Azure IoT-SDK und den Umgang mit Wiederholungen erfahren Sie unter [Verwalten von Konnektivität und zuverlässigem Messaging mithilfe von Azure IoT Hub-Geräte-SDKs](iot-hub-reliability-features-in-sdks.md#connection-and-retry).

<!-- Images -->
[1]: ../../includes/media/iot-hub-diagnostics-settings/turnondiagnostics.png
[2]: ./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png
[3]: ./media/iot-hub-troubleshoot-connectivity/metric-alert.png
[4]: ./media/iot-hub-troubleshoot-connectivity/diag-logs.png
