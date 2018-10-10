---
title: Erstellen eines IoT Hubs über das Azure-Portal | Microsoft Docs
description: Erstellen, Verwalten und Löschen von Azure IoT Hubs über das Azure-Portal. Enthält Informationen zu Tarifen, Skalierung, Sicherheit und die Messagingkonfiguration.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: 8f08141f5c14a734f89ba91045767e2a36a44fd2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985604"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Erstellen eines IoT Hubs über das Portal

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In diesem Artikel wird das Erstellen und Verwalten von IoT-Hubs über das [Azure-Portal](https://portal.azure.com) beschrieben.

Sie benötigen ein Azure-Abonnement, um die Schritte in diesem Tutorial durchzuführen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 

2. Wählen Sie +**Ressource erstellen**, und wählen Sie dann **Internet der Dinge**.

3. Klicken Sie in der Liste auf der rechten Seite auf **Iot Hub**. Daraufhin wird Ihnen der erste Bildschirm zum Erstellen eines IoT-Hubs angezeigt.

   ![Screenshot: Erstellen eines Hubs im Azure-Portal](./media/iot-hub-create-through-portal/iot-hub-create-screen-basics.png)

   Füllen Sie die Felder aus.

   **Abonnement**: Wählen Sie das Abonnement aus, das Sie für den IoT-Hub verwenden möchten.

   **Ressourcengruppe**: Sie können eine neue Ressourcengruppe erstellen oder eine vorhandene verwenden. Um eine neue Ressourcengruppe zu erstellen, klicken Sie auf **Neu erstellen**, und geben Sie den Namen ein, den Sie verwenden möchten. Um eine vorhandene Ressourcengruppe zu verwenden, klicken Sie auf **Vorhandene verwenden**, und wählen Sie die Ressourcengruppe in der Dropdownliste aus.

   **Region**: Wählen Sie die Region, in der sich Ihr Hub befinden soll, in der Dropdownliste aus.

   **IoT Hub-Name**: Geben Sie den Namen für Ihren IoT-Hub ein. Dieser Name muss global eindeutig sein. 

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

4. Klicken Sie auf **Weiter: Größe und Skalierung**, um zum nächsten Bildschirm zu wechseln.

   ![Screenshot mit der Einstellung für Größe und Skalierung für einen neuen IoT-Hub über das Azure-Portal](./media/iot-hub-create-through-portal/iot-hub-create-screen-size-scale.png)

   Auf diesem Bildschirm können Sie die Standardeinstellungen übernehmen und im unteren Bereich einfach auf **Überprüfen + erstellen** klicken. Alternativ dazu können Sie die Felder nach Bedarf ausfüllen.

   **Tarif und Skalierung**: Ihnen stehen abhängig davon, wie viele Features Sie wünschen, und wie viele Nachrichten Sie über Ihre Lösung pro Tag senden möchten, mehrere Tarife zur Auswahl. Der kostenlose Tarif ist für Test und Bewertung vorgesehen. Damit kann für 500 Geräte eine Verbindung mit dem IoT Hub hergestellt werden, und bis zu 8.000 Nachrichten pro Tag sind möglich. Jedes Azure-Abonnement kann einen IoT Hub im kostenlosen Tarif erstellen. 

   **IoT Hub-Einheiten**: Die Anzahl der pro Einheit und Tag zulässigen Nachrichten hängt von Ihrem Hubtarif ab. Beispiel: Wenn der IoT Hub 700.000 eingehende Nachrichten unterstützen soll, wählen Sie zwei Einheiten des Tarifs S1.

   Einzelheiten zu den anderen Tarifoptionen finden Sie unter [Skalieren einer IoT Hub-Lösung](iot-hub-scaling.md).

   **Erweitert/Gerät-zu-Cloud-Partitionen**: Diese Eigenschaft setzt die Gerät-zu-Cloud-Nachrichten in Relation zur Anzahl von gleichzeitigen Lesern der Nachrichten. Die meisten IoT-Hubs benötigen nur vier Partitionen. 

5. Klicken Sie auf **Überprüfen + erstellen**, um Ihre Auswahl zu überprüfen. Die Anzeige entspricht in etwa dem folgenden Bildschirm.

   ![Screenshot zur Überprüfung von Informationen bei der Erstellung des neuen IoT-Hubs](./media/iot-hub-create-through-portal/iot-hub-create-review.png)

5. Klicken Sie auf **Erstellen**, um Ihren neuen IoT-Hub zu erstellen. Das Erstellen des Hubs dauert einige Minuten.

## <a name="change-the-settings-of-the-iot-hub"></a>Ändern der Einstellungen des IoT Hubs

Sie können die Einstellungen eines vorhandenen IoT-Hubs nach seiner Erstellung im Bereich „IoT Hub“ ändern.

![Screenshot: Einstellungen für den IoT Hub](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

Hier werden einige der Eigenschaften aufgeführt, die Sie für einen IoT-Hub festlegen können:

**Tarif und Skalierung**: Mit dieser Eigenschaft können Sie die Migration zu einem anderen Tarif durchführen oder die Anzahl von IoT Hub-Einheiten festlegen. 

**Vorgangsüberwachung**: Aktivieren oder deaktivieren Sie die verschiedenen Überwachungskategorien, beispielsweise die Protokollierung von Ereignissen im Zusammenhang mit Gerät-zu-Cloud-Nachrichten oder Cloud-zu-Gerät-Nachrichten.

**IP-Filter**: Hier können Sie einen IP-Adressbereich angeben, der vom IoT-Hub akzeptiert oder abgelehnt wird.

**Eigenschaften**: Enthält die Liste der Eigenschaften, die Sie kopieren und an anderer Stelle verwenden können, z.B. die Ressourcen-ID, die Ressourcengruppe, den Standort usw.

### <a name="shared-access-policies"></a>Freigegebene Zugriffsrichtlinien

Sie können die Liste der freigegebenen Zugriffsrichtlinien auch anzeigen oder ändern, indem Sie im Abschnitt **Einstellungen** auf **Richtlinien für gemeinsamen Zugriff** klicken. Diese Richtlinien definieren die Berechtigungen für Geräte und Dienste zum Herstellen einer Verbindung mit dem IoT-Hub. 

Klicken Sie auf **Hinzufügen**, um das Blatt **Richtlinie für den gemeinsamen Zugriff hinzufügen** zu öffnen.  Sie können den Namen der neuen Richtlinie und die Berechtigungen, die dieser Richtlinie zugeordnet werden sollen, wie in der folgenden Abbildung gezeigt eingeben:

![Screenshot: Hinzufügen einer SAS-Richtlinie](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* Die Richtlinien **Registrierung lesen** und **In Registrierung schreiben** berechtigen zum Lesen und Schreiben in der Identitätsregistrierung. Beim Auswählen der Schreiboption wird die Leseoption automatisch ausgewählt.

* Die Richtlinie **Dienstverbindung** erteilt die Berechtigung für den Zugriff auf Dienstendpunkte, z.B. **Empfangen von Gerät-an-Cloud-Nachrichten**. 

* Die Richtlinie **Geräteverbindung** gewährt Berechtigungen zum Senden und Empfangen von Nachrichten mit den geräteseitigen IoT Hub-Endpunkten.

Klicken Sie auf **Erstellen** , um der vorhandenen Liste diese neu erstellte Richtlinie hinzuzufügen.

## <a name="message-routing-for-an-iot-hub"></a>Nachrichtenrouting für einen IoT-Hub

Klicken Sie unter **Messaging** auf **Nachrichtenrouting**, um den Bereich „Nachrichtenrouting“ anzuzeigen. Hier definieren Sie die Routen und benutzerdefinierten Endpunkte für den Hub. Über [Nachrichtenrouting](iot-hub-devguide-messages-d2c.md) können Sie verwalten, wie Daten von Ihren Geräten an die Endpunkte gesendet werden. Der erste Schritt besteht darin, eine neue Route hinzuzufügen. Anschließend können Sie der Route einen vorhandenen Endpunkt hinzufügen oder einen neuen mit einem der unterstützten Typen erstellen, z.B. Blobspeicher. 

![Bereich „Nachrichtenrouting“](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Routen

„Routen“ ist die erste Registerkarte im Bereich „Nachrichtenrouting“. Um eine neue Route hinzuzufügen, klicken Sie auf +**Hinzufügen**. Der folgende Bildschirm wird angezeigt. 

![Screenshot: Hinzufügen einer neuen Route](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Benennen Sie Ihren Hub. Der Name muss in der Liste der Routen für diesen Hub eindeutig sein. 

Als **Endpunkt** können Sie einen Eintrag aus der Dropdownliste auswählen oder einen neuen hinzufügen. In diesem Beispiel sind ein Speicherkonto und ein Container bereits verfügbar. Um sie als Endpunkt hinzuzufügen, klicken Sie neben der Dropdownliste „Endpunkt“ auf +**Hinzufügen**, und wählen Sie **Blob Storage** aus. Der folgende Bildschirm zeigt, wo das Speicherkonto und der Container angegeben werden.

![Screenshot: Hinzufügen eines Speicherendpunkts für die Routingregel](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Klicken Sie auf **Container auswählen**, um das Speicherkonto und den Container auswählen. Wenn Sie diese Felder ausgewählt haben, gelangen Sie zurück zum Bereich „Endpunkt“. Verwenden Sie die Standardeinstellungen für die übrigen Felder und dann **Erstellen**, um den Endpunkt für das Speicherkonto zu erstellen und den Routingregeln hinzuzufügen.

Wählen Sie für **Datenquelle** die Option „Gerätetelemetriemeldungen“ aus. 

Fügen Sie anschließend eine Routingabfrage hinzu. In diesem Beispiel werden die Nachrichten, die eine Anwendungseigenschaft namens `level` mit einem Wert von `critical` aufweisen, an das Speicherkonto weitergeleitet.

![Screenshot: Speichern einer neuen Routingregel](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Klicken Sie auf **Speichern**, um die Routingregel zu speichern. Sie kehren zum Bereich „Nachrichtenrouting“ zurück, und Ihre neue Routingregel wird angezeigt.

### <a name="custom-endpoints"></a>Benutzerdefinierte Endpunkte

Klicken Sie auf die Registerkarte **Benutzerdefinierte Endpunkte**. Bereits erstellte benutzerdefinierte Endpunkte werden angezeigt. Von hier aus können Sie neue Endpunkte hinzufügen oder vorhandene Endpunkte löschen. 

> [!NOTE]
> Wenn Sie eine Route löschen, werden nicht die Endpunkte gelöscht, die dieser Route zugewiesen sind. Um einen Endpunkt zu löschen, klicken Sie auf die Registerkarte „Benutzerdefinierte Endpunkte“, wählen Sie den Endpunkt aus, den Sie löschen möchten, und klicken Sie auf „Löschen“.
>

In [Referenz: IoT Hub-Endpunkte](iot-hub-devguide-endpoints.md) erfahren Sie mehr über benutzerdefinierte Endpunkte.

Sie können bis zu 10 benutzerdefinierte Endpunkte für einen IoT-Hub definieren. 

Ein vollständiges Beispiel zur Verwendung benutzerdefinierter Endpunkte mit der Routingfunktion finden Sie unter [Nachrichtenrouting mit IoT Hub](tutorial-routing.md).

## <a name="find-a-specific-iot-hub"></a>Suchen eines bestimmten IoT-Hubs

Es gibt zwei Möglichkeiten zum Finden eines bestimmten IoT-Hubs in Ihrem Abonnement:

1. Wenn Sie die Ressourcengruppe kennen, zu der der IoT-Hub gehört, klicken Sie auf **Ressourcengruppen**, und wählen Sie dann die Ressourcengruppe aus der Liste aus. Der Bildschirm „Ressourcengruppe“ zeigt alle Ressourcen in dieser Gruppe, einschließlich der IoT-Hubs. Klicken Sie auf den Hub, nach dem Sie suchen.

2. Klicken Sie auf **Alle Ressourcen**. Im Bereich **Alle Ressourcen** gibt es eine Dropdownliste mit dem Standardwert `All types`. Klicken Sie auf die Dropdownliste, und deaktivieren Sie `Select all`. Suchen Sie nach `IoT Hub`, und überprüfen Sie den Eintrag. Klicken Sie auf das Dropdown-Listenfeld, um es zu schließen. Die Einträge werden gefiltert und zeigen nur Ihre IoT-Hubs an.

## <a name="delete-the-iot-hub"></a>Löschen des IoT Hubs

Um einen IoT-Hub zu löschen, suchen Sie den betreffenden IoT-Hub, und klicken Sie dann unter dem Namen des IoT-Hubs auf die Schaltfläche **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [Nachrichtenrouting mit IoT Hub](tutorial-routing.md)
* [IoT Hub-Metriken](iot-hub-metrics.md)
* [Vorgangsüberwachung](iot-hub-operations-monitoring.md)