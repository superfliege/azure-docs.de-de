---
title: IoT-Remoteüberwachung und -Benachrichtigungen mit Azure Logic Apps | Microsoft-Dokumentation
description: Verwenden Sie Azure Logic Apps für die IoT-Temperaturüberwachung auf Ihrem IoT Hub und zum automatischen Senden von E-Mail-Benachrichtigungen an Ihr Postfach, sobald Anomalien gefunden werden.
author: robinsh
keywords: IoT-Überwachung, IoT-Benachrichtigungen, IoT-Temperaturüberwachung
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: robinsh
ms.openlocfilehash: 5d5b1d1579600767153fcf5ad751e1224631d611
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59262515"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IoT-Remoteüberwachung und -Benachrichtigungen mit Azure Logic Apps zum Herstellen einer Verbindung zwischen Ihrem IoT Hub und Postfach

![Lückenloses Diagramm](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Azure Logic Apps bietet eine Möglichkeit zum Automatisieren von Prozessen als eine Reihe von Schritten. Eine Logik-App kann verschiedene Dienste und Protokolle miteinander verbinden. Den Anfang macht ein Auslöser wie „Wenn ein Konto hinzugefügt wird“, auf den eine Reihe von Aktionen folgt, wie z.B. „Senden einer Pushbenachrichtigung“. Hierdurch wird Azure Logic Apps zu einer perfekten IoT-Lösung für die IoT-Überwachung, u.a. zur Erkennung von Anomalien.

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie eine Logik-App erstellen, die Ihren IoT Hub und Ihr Postfach für die Temperaturüberwachung und Benachrichtigungen verbindet. Wenn die Temperatur mehr als 30°C beträgt, gibt die Clientanwendung `temperatureAlert = "true"` in der Nachricht an, die an Ihren IoT Hub gesendet wird. Die Nachricht löst die Logik-App so aus, dass eine E-Mail-Benachrichtigung an Sie gesendet wird.

## <a name="what-you-do"></a>Aufgaben

* Erstellen Sie einen Service Bus-Namespace, und fügen Sie diesem eine Warteschlange hinzu.
* Fügen Sie mit Ihrem IoT Hub einen Endpunkt und eine Routingregel hinzu.
* Erstellen, konfigurieren und testen Sie eine Logik-App.

## <a name="what-you-need"></a>Voraussetzungen

* Abschluss des Tutorials [Einrichten Ihres Geräts](iot-hub-raspberry-pi-kit-node-get-started.md), in dem die folgenden Anforderungen behandelt werden:

  * Ein aktives Azure-Abonnement.
  * Ein Azure IoT Hub in Ihrem Abonnement.
  * Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Erstellen eines Service Bus-Namespaces, dem eine Warteschlange hinzugefügt wird

### <a name="create-a-service-bus-namespace"></a>Erstellen eines Service Bus-Namespaces

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Ressource erstellen** > **Unternehmensintegration** > **Service Bus** aus.

2. Geben Sie die folgenden Informationen ein:

   **Name**: Der Name des Service Bus.

   **Tarif:** Wählen Sie **Basic** > **Auswählen** aus. Der Tarif „Basic“ ist für dieses Tutorial ausreichend.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Standort**: Verwenden Sie denselben Speicherort, den Ihr IoT-Hub verwendet.

3. Klicken Sie auf **Erstellen**.

   ![Erstellen eines Service Bus-Namespaces im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Hinzufügen einer Service Bus-Warteschlange

1. Öffnen Sie den Service Bus-Namespace, und wählen Sie dann **+ Warteschlange** aus.

1. Geben Sie einen Namen für die Warteschlange ein, und wählen Sie dann **Erstellen** aus.

1. Öffnen Sie die Service Bus-Warteschlange, und wählen Sie dann **Freigegebene Zugriffsrichtlinien** > **+ Hinzufügen** aus.

1. Geben Sie einen Namen für die Richtlinie ein, aktivieren Sie **Verwalten**, und wählen Sie dann **Erstellen** aus.

   ![Hinzufügen einer Service Bus-Warteschlange im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-query-to-your-iot-hub"></a>Hinzufügen eines Endpunkts und einer Routingabfrage zu Ihrem IoT-Hub

Fügen Sie Ihrem IoT-Hub nun einen Endpunkt und eine Routingabfrage hinzu.

### <a name="add-an-endpoint"></a>Hinzufügen eines Endpunkts

1. Öffnen Sie Ihren IoT-Hub, und wählen Sie **Endpunkte** > **+ Hinzufügen** aus.

1. Geben Sie Folgendes ein:

   **Name**: Der Name des Endpunkts.

   **Endpunkttyp:** Wählen Sie **Service Bus-Warteschlange** aus.

   **Service Bus-Namespace:** Wählen Sie den erstellten Namespace aus.

   **Service Bus-Warteschlange**: Wählen Sie die erstellte Warteschlange aus.

3. Klicken Sie auf **OK**.

   ![Hinzufügen eines Endpunkts zu Ihren IoT Hub im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Hinzufügen einer Routingregel

1. Wählen Sie in Ihrem IoT-Hub die Option **Routen** > **+ Hinzufügen** aus.

2. Geben Sie Folgendes ein:

   **Name**: Der Name der Routingregel.

   **Datenquelle**: Wählen Sie **DeviceMessages** aus.

   **Endpunkt**: Wählen Sie den erstellten Endpunkt aus.

   **Abfragezeichenfolge**: Geben Sie `temperatureAlert = "true"` ein.

3. Wählen Sie **Speichern** aus.

   ![Hinzufügen einer Routingregel im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Erstellen und Konfigurieren einer Logik-App

Als Nächstes erstellen und konfigurieren Sie eine Logik-App.

### <a name="create-a-logic-app"></a>Erstellen einer Logik-App

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Ressource erstellen** > **Unternehmensintegration** > **Logik-App** aus.

2. Geben Sie Folgendes ein:

   **Name**: Der Name der Logik-App.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Standort**: Verwenden Sie denselben Speicherort, den Ihr IoT-Hub verwendet.

3. Klicken Sie auf **Erstellen**.

### <a name="configure-the-logic-app"></a>Konfigurieren der Logik-App

1. Öffnen Sie die Logik-App, die im Designer für Logik-Apps geöffnet wird.

2. Wählen Sie im Designer für Logik-Apps **Leere Logik-App** aus.

   ![Beginnen Sie im Azure-Portal mit einer leeren Logik-App.](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

3. Wählen Sie **Service Bus** aus.

   ![Wählen Sie „Service Bus“ aus, um mit der Erstellung Ihrer Logik-App im Azure-Portal zu beginnen.](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

4. Wählen Sie **Service Bus – Bei Empfang mindestens einer Nachricht in der Warteschlange (autom. abschließen)** aus.

5. Erstellen Sie eine Service Bus-Verbindung.

   1. Geben Sie einen Verbindungsnamen ein.

   2. Wählen Sie den Service Bus-Namespace, die Service Bus-Richtlinie und dann **Erstellen** aus.

      ![Erstellen einer Service Bus-Verbindung für Ihre Logik-App im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   3. Wählen Sie nach dem Erstellen der Service Bus-Verbindung **Fortfahren** aus.

   4. Wählen Sie die erstellte Warteschlange aus, und geben Sie `175` für **Maximale Nachrichtenanzahl** ein.

      ![Angeben der maximalen Nachrichtenanzahl für die Service Bus-Verbindung in der Logik-App](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   5. Wählen Sie die Schaltfläche „Speichern“ aus, um die Änderungen zu speichern.

6. Erstellen Sie eine SMTP-Dienstverbindung.

   1. Wählen Sie **Nächster Schritt** > **Aktion hinzufügen** aus.

   2. Geben Sie `SMTP` ein, wählen Sie in den Suchergebnissen den Dienst **SMTP** aus, und wählen Sie dann **SMTP – E-Mail senden** aus.

      ![Erstellen einer SMTP-Verbindung in Ihrer Logik-App im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   3. Geben Sie die SMTP-Informationen Ihres Postfachs ein, und wählen Sie dann **Erstellen** aus.

      ![Eingeben der Informationen für die SMTP-Verbindung in Ihrer Logik-App im Azure-Portal](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Rufen Sie die SMTP-Informationen für [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en) und [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html) ab.

   4. Geben Sie für **Von** und **An** Ihre E-Mail-Adresse und `High temperature detected` für **Betreff** und **Text** ein.

   5. Wählen Sie **Speichern** aus.

Die Logik-App befindet sich in funktionsfähigem Zustand, wenn Sie sie speichern.

## <a name="test-the-logic-app"></a>Testen der Logik-App

1. Starten Sie die Clientanwendung, die Sie unter [Verbinden von ESP8266 mit Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md) auf Ihrem Gerät bereitgestellt haben.

2. Erhöhen Sie die Umgebungstemperatur um das SensorTag auf über 30°C. Entzünden Sie beispielsweise eine Kerze in der Nähe des SensorTag.

3. Sie sollten eine von der Logik-App gesendete E-Mail-Benachrichtigung erhalten.

   > [!NOTE]
   > Ihr E-Mail-Dienstanbieter muss ggf. die Identität des Absenders überprüfen, um sicherzustellen, dass die gesendete E-Mail von Ihnen stammt.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfolgreich eine Logik-App erstellt, die Ihren IoT Hub und Ihr Postfach für die Temperaturüberwachung und Benachrichtigungen verbindet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
