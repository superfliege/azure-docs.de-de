---
title: Erstellen eines IoT Hubs über das Azure-Portal | Microsoft Docs
description: Erstellen, Verwalten und Löschen von Azure IoT Hubs über das Azure-Portal. Enthält Informationen zu Tarifen, Skalierung, Sicherheit und die Messagingkonfiguration.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: ca0eff415c4ba0e887c3999e7a03e3c4fa1cc156
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635932"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Erstellen eines IoT Hubs über das Portal

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Dieser Artikel beschreibt Folgendes:

* Vorgehensweise zum Suchen nach dem IoT Hub-Dienst im Azure-Portal
* Vorgehensweise zum Erstellen und Verwalten von IoT Hubs

## <a name="where-to-find-the-iot-hub-service"></a>Bereiche zum Suchen nach dem IoT Hub-Dienst

Sie können in den folgenden Bereichen im Portal nach dem IoT Hub-Dienst suchen:

* Wählen Sie **+ Neu** und dann **Internet der Dinge**.
* Wählen Sie im Marketplace **Internet der Dinge**.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Sie können einen IoT Hub mit den folgenden Methoden erstellen:

* Mit der Option **+ Neu** wird das Blatt geöffnet, das im folgenden Screenshot gezeigt wird. Beim Erstellen des IoT Hubs mit dieser Methode gehen Sie genauso vor wie beim Erstellen über den Marketplace.
* Wählen Sie im Marketplace **Erstellen**, um das im folgenden Screenshot gezeigte Blatt zu öffnen.

In den folgenden Abschnitten werden die verschiedenen Schritte zum Erstellen von IoT Hub beschrieben:

### <a name="choose-the-name-of-the-iot-hub"></a>Auswählen des Namen des IoT Hubs

Um einen IoT Hub zu erstellen, müssen Sie einen Namen für den IoT Hub festlegen. Dieser Name muss unter allen IoT Hubs eindeutig sein.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Auswählen des Tarifs

Ihnen stehen abhängig davon, wie viele Features Sie wünschen, und wie viele Nachrichten Sie über Ihre Lösung pro Tag senden möchten, mehrere Tarife zur Auswahl. Der kostenlose Tarif ist für Test und Bewertung vorgesehen. Damit kann für 500 Geräte eine Verbindung mit dem IoT Hub hergestellt werden, und bis zu 8.000 Nachrichten pro Tag sind möglich. Jedes Azure-Abonnement kann einen IoT Hub im kostenlosen Tarif erstellen. 

Einzelheiten zu den anderen Tarifoptionen finden Sie unter [Skalieren einer IoT Hub-Lösung](iot-hub-scaling.md).

### <a name="iot-hub-units"></a>IoT Hub-Einheiten

Die Anzahl der pro Einheit und Tag zulässigen Nachrichten hängt von Ihrem Hubtarif ab. Beispiel: Wenn der IoT Hub 700.000 eingehende Nachrichten unterstützen soll, wählen Sie zwei Einheiten des Tarifs S1.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Gerät für Cloud-Partitionen und Ressourcengruppe

Sie können die Anzahl der Partitionen für einen IoT Hub ändern. Die Standardanzahl von Partitionen beträgt 4. Sie können eine andere Anzahl aus der Dropdownliste auswählen.

Sie müssen nicht explizit eine leere Ressourcengruppe erstellen. Beim Erstellen einer Ressource können Sie entweder eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden.

![][5]

### <a name="choose-subscription"></a>Auswählen eines Abonnements

Azure IoT Hub listet automatisch die Azure-Abonnements auf, mit denen das Benutzerkonto verknüpft ist. Sie können das Azure-Abonnement auswählen, das IoT Hub zugeordnet ist.

### <a name="choose-the-location"></a>Wählen des Standorts

Die Option „Standort“ enthält eine Liste der Regionen, in denen IoT Hub verfügbar ist.

### <a name="create-the-iot-hub"></a>Erstellen des IoT Hubs

Wenn alle vorherigen Schritte abgeschlossen sind, können Sie IoT Hub erstellen. Klicken Sie auf **Erstellen**, um den Back-End-Prozess zu starten, mit dem IoT Hub mit den von Ihnen gewählten Optionen erstellt und bereitgestellt wird.

Die Erstellung von IoT Hub kann einige Minuten in Anspruch nehmen, da es etwas dauert, bis die Back-End-Bereitstellung auf den jeweiligen Standortservern ausgeführt wird.

## <a name="change-the-settings-of-the-iot-hub"></a>Ändern der Einstellungen des IoT Hubs

Sie können die Einstellungen für einen IoT Hub nach dessen Erstellung im Blatt „IoT Hub“ ändern.

![][8]

**Richtlinien für gemeinsamen Zugriff**: Diese Richtlinien definieren die Berechtigungen für Geräte und Dienste zum Herstellen einer Verbindung mit dem IoT Hub. Sie finden diese Richtlinien, indem Sie unter **Allgemein** auf **Richtlinien für gemeinsamen** Zugriff klicken. Auf diesem Blatt können Sie vorhandene Richtlinien ändern oder eine neue Richtlinie hinzufügen.

### <a name="create-a-policy"></a>Erstellen einer Richtlinie

* Klicken Sie auf **Hinzufügen**, um ein Blatt zu öffnen. Hier können Sie den Namen der neuen Richtlinie und die Berechtigungen, die dieser Richtlinie zugeordnet werden sollen, wie in der folgenden Abbildung gezeigt eingeben:

    Diesen freigegebenen Richtlinien können mehrere Berechtigungen zugeordnet werden. Die Richtlinien **Registrierung lesen** und **In Registrierung schreiben** berechtigen zum Lesen und Schreiben in der Identitätsregistrierung. Beim Auswählen der Schreiboption wird die Leseoption automatisch ausgewählt.

    Die Richtlinie **Dienstverbindung** erteilt die Berechtigung für den Zugriff auf Dienstendpunkte, z.B. **Empfangen von Gerät-an-Cloud-Nachrichten**. Die Richtlinie **Geräteverbindung** gewährt Berechtigungen zum Senden und Empfangen von Nachrichten mit den geräteseitigen IoT Hub-Endpunkten.

* Klicken Sie auf **Erstellen** , um der vorhandenen Liste diese neu erstellte Richtlinie hinzuzufügen.

![][10]

## <a name="endpoints"></a>Endpunkte

Klicken Sie auf **Endpunkte**, um eine Liste der Endpunkte für den IoT-Hub anzuzeigen, den Sie ändern. Es gibt zwei Typen von Endpunkten: Endpunkte, die in den IoT-Hub integriert sind, und Endpunkte, die Sie dem IoT-Hub nach seiner Erstellung hinzufügen.

![][11]

### <a name="built-in-endpoints"></a>Integrierte Endpunkte

Es gibt zwei integrierte Endpunkte: **Cloud-zu-Gerät-Feedback** und **Ereignisse**.

* Einstellungen für **Cloud-zu-Gerät-Feedback**: Diese Einstellung hat zwei untergeordnete Einstellungen: **Cloud-zu-Gerät-TTL** (Time to Live, Gültigkeitsdauer) und **Aufbewahrungszeit** (in Stunden) für die Nachrichten. Bei der ersten Erstellung eines IoT-Hubs ist für beide Einstellungen ein Standardwert von einer Stunde festgelegt. Zum Anpassen dieser Einstellungen bewegen Sie die Schieberegler oder geben die entsprechenden Werte ein.
* Einstellungen für **Ereignisse**: Für diese Einstellung sind verschiedene Untereinstellungen verfügbar. Manche davon sind schreibgeschützt. Diese Einstellungen werden in der folgende Liste beschrieben:

  * **Partitionen**: Beim Erstellen des IoT-Hubs wird ein Standardwert festgelegt. Sie können die Anzahl von Partitionen über diese Einstellung ändern.

  * **Event Hub-kompatibler Name und Event-Hub-kompatibler Endpunkt**: Bei Erstellung des IoT Hubs wird intern ein Event Hub erstellt, auf den Sie unter bestimmten Umständen zugreifen müssen. Die Event Hub-kompatiblen Werte für Name und Endpunkt können nicht angepasst, aber kopiert werden. Klicken Sie dazu auf **Kopieren**.

  * **Aufbewahrungszeit**: Standardmäßig auf 1 Tag festgelegt, kann aber mithilfe der Dropdownliste geändert werden. Dieser Wert wird für die Gerät-zu-Cloud-Einstellung in Tagen angegeben.

  * **Consumergruppen**: Consumergruppen ermöglichen mehreren Lesern, Nachrichten unabhängig von IoT Hub zu lesen. Jeder IoT Hub wird mit einer Standard-Consumergruppe erstellt. Sie können mit dieser Einstellung Ihren IoT-Hubs jedoch Consumergruppen hinzufügen oder diese daraus löschen.

  > [!NOTE]
  > Die Standard-Consumergruppe kann nicht bearbeitet oder gelöscht werden.

### <a name="custom-endpoints"></a>Benutzerdefinierte Endpunkte

Sie können Ihrem IoT-Hub über das Portal benutzerdefinierte Endpunkte hinzufügen. Klicken Sie auf dem Blatt **Endpunkte** oben auf **Hinzufügen**, um das Blatt **Endpunkt hinzufügen** zu öffnen. Geben Sie die erforderlichen Informationen ein, und klicken Sie dann auf **OK**. Ihr benutzerdefinierter Endpunkt wird nun auf dem Hauptblatt für **Endpunkte** aufgeführt.

![][13]

In [Referenz: IoT Hub-Endpunkte][lnk-devguide-endpoints] erfahren Sie mehr über benutzerdefinierte Endpunkte.

## <a name="routes"></a>Routen

Klicken Sie auf **Routen**, um zu verwalten, wie IoT Hub Ihre D2C-Nachrichten sendet.

![][14]

Sie können Ihrem IoT-Hub Routen hinzufügen, indem Sie am oberen Rand des Blatts **Routen*** auf **Hinzufügen** klicken, die erforderlichen Informationen eingeben und auf **OK** klicken. Ihre Route wird dann auf dem Hauptblatt **Routen** aufgeführt. Sie können eine Route bearbeiten, indem Sie in der Liste der Routen darauf klicken. Um eine Route zu aktivieren, klicken sie in der Liste der Routen darauf, und legen Sie den Schalter **Aktiviert** auf **Aus** fest. Um die Änderung zu speichern, klicken Sie am unteren Rand des Blatts auf **OK**.

![][15]

## <a name="delete-the-iot-hub"></a>Löschen des IoT Hubs

Klicken Sie zum Suchen des zu löschenden IoT Hubs auf **Durchsuchen**, und wählen Sie dann den zu löschenden Hub aus. Durch Klicken auf die Schaltfläche **Löschen** unterhalb des IoT Hub-Namens wird der IoT Hub gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [Massenverwaltung von IoT-Geräten][lnk-bulk]
* [IoT Hub-Metriken][lnk-metrics]
* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Deploy Azure IoT Edge on a simulated device in Linux - preview][lnk-iotedge] (Bereitstellen von Azure IoT Edge auf einem simulierten Gerät in Linux – Vorschauversion)
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
