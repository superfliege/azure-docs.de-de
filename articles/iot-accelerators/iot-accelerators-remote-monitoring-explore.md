---
title: Erste Schritte mit der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: In diesem Tutorial werden simulierte Szenarien verwendet, um den Solution Accelerator für Remoteüberwachung vorzustellen. Diese Szenarien werden erstellt, wenn Sie den Solution Accelerator für Remoteüberwachung zum ersten Mal bereitstellen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: e354e110df8d168e153c50de6f7aad08dd416464
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627806"
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-solution-accelerator"></a>Erkunden der Funktionen des Solution Accelerators für Remoteüberwachung

In diesem Tutorial werden die wichtigsten Funktionen der vorkonfigurierten Remoteüberwachungslösung veranschaulicht. Zur Einführung in diese Funktionen werden im Tutorial gängige Kundenszenarien beschrieben, indem eine simulierte IoT-Anwendung für das Unternehmen Contoso verwendet wird.

Anhand des Tutorials können Sie sich mit den typischen IoT-Szenarien vertraut machen, die die Remoteüberwachungslösung standardmäßig bereitstellt.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Visualisieren und Filtern von Geräten im Dashboard
> * Reagieren auf eine Warnung
> * Aktualisieren der Firmware auf Ihren Geräten
> * Organisieren Ihrer Objekte
> * Anhalten und Starten der simulierten Geräte

Das folgende Video zeigt eine exemplarische Vorgehensweise für die Remoteüberwachungslösung:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-28-An-introduction-to-Azure-IoT-through-the-new-Remote-Monitoring-Preconfigured-Solution/Player]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial muss in Ihrem Azure-Abonnement eine bereitgestellte Instanz der Remoteüberwachungslösung vorhanden sein.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, absolvieren Sie zuerst das Tutorial [Bereitstellen des Solution Accelerators für die Remoteüberwachung](iot-accelerators-remote-monitoring-deploy.md).

## <a name="the-contoso-sample-iot-deployment"></a>Contoso-Beispiel für IoT-Bereitstellung

Sie können das Contoso-Beispiel für die IoT-Bereitstellung nutzen, um sich mit den grundlegenden Szenarien vertraut zu machen, die die Remoteüberwachungslösung standardmäßig bereitstellt. Diese Szenarien basieren auf IoT-Bereitstellungen aus der Praxis. Sie werden die Remoteüberwachungslösung später wahrscheinlich an Ihre jeweiligen Anforderungen anpassen, aber anhand des Contoso-Beispiels lernen Sie erst einmal die Grundlagen kennen.

> [!NOTE]
> Wenn Sie die Befehlszeilenschnittstelle zum Bereitstellen des Solution Accelerators verwendet haben, enthält die Datei `deployment-{your deployment name}-output.json` Informationen zur Bereitstellung, z.B. die URL für den Zugriff auf das bereitgestellte Beispiel.

Mit dem Contoso-Beispiel wird eine Reihe von simulierten Geräten und dazugehörigen Regeln bereitgestellt. Nachdem Sie sich mit den grundlegenden Szenarien vertraut gemacht haben, können Sie unter [Ausführen der erweiterten Überwachung mithilfe der Remoteüberwachungslösung](iot-accelerators-remote-monitoring-monitor.md) weitere Features der Lösung erkunden.

Contoso ist ein Unternehmen, das in unterschiedlichen Umgebungen eine Vielzahl von Objekten verwaltet. Contoso plant den Einsatz von cloudbasierten IoT-Anwendungen, um mehrere Objekte über eine zentralisierte Anwendung per Remotezugriff zu überwachen und zu verwalten. Die folgenden Abschnitte enthalten eine Zusammenfassung der anfänglichen Konfiguration für das Contoso-Beispiel:

> [!NOTE]
> Das Contoso-Demo ist nur eine Möglichkeit zur Bereitstellung von simulierten Geräten und für die Erstellung von Regeln. Zu den weiteren Bereitstellungsoptionen gehört auch das Erstellen Ihrer eigenen benutzerdefinierten Geräte. Weitere Informationen zur Erstellung Ihrer eigenen Geräte und Regeln finden Sie unter [Manage and configure your devices](iot-accelerators-remote-monitoring-manage.md) (Verwalten und Konfigurieren Ihrer Geräte) und [Detect issues using threshold-based rules](iot-accelerators-remote-monitoring-automate.md) (Erkennen von Problemen mit Regeln auf Schwellenwertbasis).

### <a name="contoso-devices"></a>Contoso-Geräte

Contoso setzt unterschiedliche Arten von intelligenten Geräten ein. Diese Geräte erfüllen für das Unternehmen verschiedene Aufgaben und senden unterschiedliche Telemetriedatenströme. Darüber hinaus verfügt jeder Gerätetyp über Geräteeigenschaften und unterstützte Methoden.

Die folgende Tabelle enthält eine Zusammenfassung der Typen von bereitgestellten Geräten:

| Gerätetyp        | Telemetrie                                  | Eigenschaften                                  | Tags                    | Methoden                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Kältemaschine            | Temperatur, Luftfeuchtigkeit, Druck            | Typ, Firmwareversion, Modell               | Standort, Etage, Gelände | Neustart, Firmwareupdate, Sicherheitsregelventil, Druckerhöhung                          |
| Gerät zur Prototyperstellung | Temperatur, Druck, geografischer Standort        | Typ, Firmwareversion, Modell               | Standort, Modus          | Neustart, Firmwareupdate, Gerät verschieben, Gerät beenden, Temperaturregelung, Temperaturanstieg |
| Motor             | Kraftstoffstand im Tank, Kühlmittelsensor, Vibration | Typ, Firmwareversion, Modell               | Standort, Etage, Gelände | Firmwareupdate, Leerer Tank, Tank füllen                                              |
| LKW              | Geografischer Standort, Geschwindigkeit, Ladungstemperatur     | Typ, Firmwareversion, Modell               | Standort, Last          | Ladungstemperatur senken, Ladungstemperatur erhöhen, Firmwareupdate                         |
| Aufzug           | Boden, Vibration, Temperatur              | Typ, Firmwareversion, Modell, geografischer Standort | Standort, Gelände        | Aufzug stoppen, Aufzug starten, Firmwareupdate                                               |

> [!NOTE]
> Im Contoso-Demobeispiel werden zwei Geräte pro Typ bereitgestellt. Für jeden Typ funktioniert ein Gerät gemäß den von Contoso als normal festgelegten Grenzen, und für das andere Gerät liegt eine Fehlfunktion vor. Im nächsten Abschnitt werden die Regeln beschrieben, die Contoso für die Geräte definiert. Mit diesen Regeln werden die Grenzen des richtigen Verhaltens definiert.

### <a name="contoso-rules"></a>Contoso-Regeln

Die Bediener bei Contoso kennen die Schwellenwerte, die bestimmen, ob ein Gerät richtig funktioniert. Eine Kältemaschine funktioniert beispielsweise nicht richtig, wenn der gemeldete Druck höher als 250 PSI ist. In der folgenden Tabelle sind die Regeln auf Schwellenwertbasis aufgeführt, die von Contoso für die einzelnen Gerätetypen definiert werden:

| Regelname | BESCHREIBUNG | Schwellenwert | Severity | Betroffene Geräte |
| --------- | ----------- | --------- | -------- | ---------------- |
| Druck der Kältemaschine zu hoch | Warnung, wenn Kältemaschinen einen höheren Druck als normal aufweisen   |Druck > 250 psi       | Kritisch | Kältemaschinen            |
| Temperatur des Geräts für die Prototyperstellung zu hoch  | Warnung, wenn Geräte für die Prototyperstellung höhere Temperaturen als normal aufweisen  |Temperatur > 80 &deg;F |Kritisch | Geräte für die Prototyperstellung |
| Motortank leer  | Warnung, wenn der Kraftstofftank des Motors in Kürze leer ist                     | Kraftstoff < 5 Gallonen | Information     | Motoren             |
| Temperatur der Ladung höher als normal | Warnung, wenn die Ladungstemperatur eines LKW höher als normal ist                 | Temperatur < 45 &deg;F |Warnung  | LKW              |
| Vibrationsstopp des Aufzugs      | Warnung, wenn der Aufzug vollständig stillsteht (basierend auf der Vibration)                     | Vibration < 0,1 mm |Warnung  | Aufzüge           |

### <a name="operate-the-contoso-sample-deployment"></a>Durchführen der Bereitstellung für das Contoso-Beispiel

Sie haben jetzt die anfängliche Einrichtung im Contoso-Beispiel gesehen. In den folgenden Abschnitten werden drei Szenarien im Contoso-Beispiel beschrieben, die veranschaulichen, wie der Solution Accelerator von einem Bediener genutzt werden kann.

## <a name="respond-to-a-pressure-alert"></a>Reagieren auf eine Druckwarnung

Dieses Szenario verdeutlicht, wie Sie eine Warnung identifizieren, die von einer Kältemaschine ausgelöst wird, und darauf reagieren. Die Kältemaschine befindet sich in Redmond, Gebäude 43, 2. Etage.

Ihnen als Bediener wird im Dashboard eine Warnung in Bezug auf den Druck einer Kältemaschine angezeigt. Sie können die Karte schwenken und zoomen, um weitere Details anzuzeigen.

1. Auf der Seite **Dashboard** im Raster **Alerts** (Warnungen) wird die Warnung **Chiller pressure too high** (Druck der Kältemaschine zu hoch) angezeigt. Außerdem wird die Kältemaschine in der Karte hervorgehoben:

    ![Dashboard: Druckwarnung und Gerät in der Karte](./media/iot-accelerators-remote-monitoring-explore/dashboardalarm.png)

1. Navigieren Sie zur Seite **Wartung**, und wählen Sie **Wartung** im Navigationsmenü aus. Auf der Seite **Wartung** können Sie die Details der Regel anzeigen, über die die Druckwarnung für die Kältemaschine ausgelöst wurde.

1. In der Liste der Warnungen wird die Anzahl von Warnungsauslösungen, Bestätigungen und offenen und geschlossenen Warnungen angezeigt:

    ![Seite „Wartung“ mit Liste der ausgelösten Warnungen](./media/iot-accelerators-remote-monitoring-explore/maintenancealarmlist.png)

1. Die erste Warnung in der Liste ist die zuletzt aufgetretene Warnung. Klicken Sie auf die Warnung **Chiller Pressure Too High** (Druck der Kältemaschine zu hoch), um die zugeordneten Geräte und Telemetriedaten anzuzeigen. In den Telemetriedaten wird für die Kältemaschine ein hoher Druck angezeigt:

    ![Seite „Wartung“ mit Telemetriedaten für die ausgewählte Warnung](./media/iot-accelerators-remote-monitoring-explore/maintenancetelemetry.png)

Sie haben nun das Problem und das zugeordnete Gerät identifiziert, für das die Warnung ausgelöst wurde. Für den Bediener sind die nächsten Schritte das Bestätigen der Warnung und das Beheben des Problems.

1. Ändern Sie den **Alert status** (Warnungsstatus) in **Acknowledged** (Bestätigt), um anzugeben, dass Sie jetzt an der Behebung der Warnung arbeiten:

    ![Auswählen und Bestätigen der Warnung](./media/iot-accelerators-remote-monitoring-explore/maintenanceacknowledge.png)

1. Wählen Sie die Kältemaschine aus, und klicken Sie auf **Jobs** (Aufträge). Wählen Sie **Run method** (Ausführungsmethode) und dann **EmergencyValveRelease**, fügen Sie den Auftragsnamen **ChillerPressureRelease** hinzu, und wählen Sie die Option **Apply** (Übernehmen). Mit diesen Einstellungen wird ein Auftrag erstellt, der sofort ausgeführt wird:

    ![Auswählen des Geräts und Planen einer Aktion](./media/iot-accelerators-remote-monitoring-explore/maintenanceschedule.png)

1. Kehren Sie zum Anzeigen des Auftragsstatus auf die Seite **Wartung** zurück, und zeigen Sie die Liste mit den Aufträgen in der Ansicht **Aufträge** an. Sie sehen, dass der Auftrag ausgeführt wird, um den Ventildruck in der Kältemaschine zu verringern:

    ![Status der Aufträge in der Ansicht „Aufträge“](./media/iot-accelerators-remote-monitoring-explore/maintenancerunningjob.png)

Bestätigen Sie abschließend, dass die Telemetriewerte der Kältemaschine wieder normal sind.

1. Navigieren Sie zum Anzeigen des Warnungsrasters zur Seite **Dashboard**.

1. Wählen Sie in der Karte das Gerät für die ursprüngliche Warnung aus, um die Gerätetelemetrie anzuzeigen und sicherzustellen, dass wieder Normalzustand herrscht.

1. Navigieren Sie zum Schließen des Vorfalls zur Seite **Maintenance** (Wartung), wählen Sie die Warnung aus, und legen Sie den Status auf **Closed** (Geschlossen) fest:

    ![Auswählen und Schließen der Warnung](./media/iot-accelerators-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Aktualisieren der Gerätefirmware

Contoso testet einen Gerätetyp im Außendienst. Im Rahmen des Testzyklus müssen Sie sicherstellen, dass Firmwareupdates für das Gerät richtig funktionieren. Die folgenden Schritte veranschaulichen, wie Sie die Remoteüberwachungslösung nutzen, um die Firmware auf mehreren Geräten zu aktualisieren.

Verwenden Sie die Seite **Geräte**, um die erforderlichen Aufgaben für die Geräteverwaltung durchzuführen. Filtern Sie zuerst nach allen Geräten für die Prototyperstellung:

1. Navigieren Sie zur Seite **Geräte**. Wählen Sie in der Dropdownliste **Filter** den Filter **Prototyping devices** (Geräte für die Prototyperstellung):

    ![Verwenden eines Filters auf der Seite „Geräte“](./media/iot-accelerators-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Klicken Sie auf **Verwalten**, um die verfügbaren Filter zu verwalten.

1. Wählen Sie ein Gerät für die Prototyperstellung aus:

    ![Auswählen eines Geräts auf der Seite „Geräte“](./media/iot-accelerators-remote-monitoring-explore/devicesselect.png)

1. Klicken Sie auf die Schaltfläche **Jobs** (Aufträge), und wählen Sie **Run method** (Ausführungsmethode) und dann **Firmware update** (Firmwareupdate) aus. Geben Sie Werte für **Job name** (Auftragsname), **Firmware Version** (Firmwareversion) und **Firmware URI** (Firmware-URI) ein. Wählen Sie **Übernehmen**, um den Auftrag zu planen, der jetzt ausgeführt werden soll:

    ![Planen des Firmwareupdates für das Gerät](./media/iot-accelerators-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Mit den simulierten Geräten können Sie eine beliebige URL als Wert für **Firmware URI** (Firmware-URI) und jeden gewünschten wert für die **Firmware Version** (Firmwareversion) verwenden. Von den simulierten Geräten wird nicht auf die URL zugegriffen.

1. Notieren Sie sich, wie viele Geräte von dem Auftrag betroffen sind, und wählen Sie **Übernehmen**:

    ![Senden des Auftrags für das Firmwareupdate](./media/iot-accelerators-remote-monitoring-explore/devicessubmitupdate.png)

Sie können die Seite **Wartung** verwenden, um den Auftrag während der Ausführung nachzuverfolgen.

1. Navigieren Sie zum Anzeigen der Liste mit den Aufträgen zur Seite **Wartung**, und klicken Sie auf **Aufträge**.

1. Suchen Sie nach dem Ereignis, das zu dem von Ihnen erstellten Auftrag gehört. Vergewissern Sie sich, dass der Prozess für das Firmwareupdate richtig initiiert wurde.

<!-- 05/01 broken 
You can create a filter to verify the firmware version updated correctly.

1. To create a filter, navigate to the **Devices** page and select **Manage device groups**:

    ![Manage device groups](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Create a filter that includes only devices with the new firmware version:

    ![Create device filter](./media/iot-accelerators-remote-monitoring-explore/devicescreatefilter.png)

1. Return to the **Devices** page and verify that the device has the new firmware version. -->

## <a name="organize-your-assets"></a>Organisieren Ihrer Objekte

Contoso verfügt über zwei unterschiedliche Teams für Außendienstaktivitäten:

* Das Team „Smart Building“ (Intelligentes Gebäude) verwaltet Kältemaschinen, Aufzüge und Motoren.
* Das Team „Smart Vehicle“ (Intelligentes Fahrzeug) verwaltet LKW und Geräte für die Prototyperstellung.

Um für Sie als Bediener das Organisieren und Verwalten Ihrer Geräte zu vereinfachen, möchten Sie die Teams mit dem entsprechenden Teamnamen kennzeichnen.

Sie können Tagnamen erstellen, die für Geräte verwendet werden.

1. Navigieren Sie zum Anzeigen aller Geräte zur Seite **Geräte**, und wählen Sie den Filter **Alle Geräte**:

    ![Anzeigen aller Geräte](./media/iot-accelerators-remote-monitoring-explore/devicesalldevices.png)

1. Wählen Sie die Geräte **Trucks** (LKW) und **Prototyping devices** (Geräte für die Prototyperstellung). Wählen Sie anschließend die Option **Jobs** (Aufträge) aus:

    ![Auswählen von Prototyp- und LKW-Geräten](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect.png)

1. Wählen Sie die Option **Tag**, und erstellen Sie dann ein neues Texttag namens **FieldService** und mit dem Wert **ConnectedVehicle**. Wählen Sie einen Namen für den Auftrag aus. Klicken Sie anschließend auf **Übernehmen**:

    ![Hinzufügen von Tags zu Prototyp- und LKW-Geräten](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag.png)

1. Wählen Sie die Geräte **Chiller** (Kältemaschine), **Elevator** (Aufzug) und **Engine** (Motor) aus. Wählen Sie anschließend die Option **Jobs** (Aufträge) aus:

    ![Auswählen der Geräte „Chiller“ (Kältemaschine), „Elevator“ (Aufzug) und „Engine“ (Motor)](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect2.png)

1. Wählen Sie die Option **Tag**, und erstellen Sie anschließend ein neues Texttag mit dem Namen **FieldService** und dem Wert **SmartBuilding**. Wählen Sie einen Namen für den Auftrag aus. Klicken Sie anschließend auf **Übernehmen**:

    ![Hinzufügen eines Tags zu den Geräten „Chiller“ (Kältemaschine), „Engine“ (Motor) und „Elevator“ (Aufzug)](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag2.png)

Sie können die Tagwerte zum Erstellen von Filtern verwenden.

1. Wählen Sie auf der Seite **Devices** (Geräte) die Option **Manage device groups** (Gerätegruppen verwalten) aus:

    ![Verwalten von Gerätegruppen](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Erstellen Sie einen neuen Filter, für den der Tagname **FieldService** und der Wert **SmartBuilding** verwendet werden. Speichern Sie den Filter unter dem Namen **Smart Building**.

1. Erstellen Sie einen neuen Filter, für den der Tagname **FieldService** und der Wert **ConnectedVehicle** verwendet werden. Speichern Sie den Filter unter dem Namen **Connected Vehicle**.

Der Contoso-Bediener kann Geräte jetzt anhand des Betriebsteams abfragen, ohne auf den Geräten hierfür etwas ändern zu müssen.

## <a name="stop-simulated-devices"></a>Anhalten der simulierten Geräte

Im Menü „Einstellungen“ können Sie die simulierten Geräte anhalten. Dadurch verringern Sie die Kosten für das Testen und Untersuchen der Lösung. So halten Sie die simulierten Geräte an oder starten sie

1. Wählen Sie das Symbol **Einstellungen** aus.

1. Schalten Sie dann **Flowing** (Fließen) ein oder aus:

    ![Einstellungsmenü](./media/iot-accelerators-remote-monitoring-explore/settings.png)

## <a name="customize-the-ui"></a>Anpassen der Benutzeroberfläche

Im Einstellungenmenü können Sie grundlegende Anpassungen für den Solution Accelerator für Remoteüberwachung vornehmen. Ihre Möglichkeiten:

- Wechseln Sie zwischen hellen und dunklen Designs.
- Ändern Sie den Namen der Lösung.
- Laden Sie ein benutzerdefiniertes Logo hoch.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

>[!div class="checklist"]
> * Visualisieren und Filtern von Geräten im Dashboard
> * Reagieren auf eine Warnung
> * Aktualisieren der Firmware auf Ihren Geräten
> * Organisieren Ihrer Objekte
> * Anhalten und Starten der simulierten Geräte

Nachdem Sie sich mit der Remoteüberwachungslösung vertraut gemacht haben, können Sie sich als Nächstes über die erweiterten Features der Remoteüberwachungslösung informieren:

* [Monitor your devices](./iot-accelerators-remote-monitoring-monitor.md) (Überwachen von Geräten)
* [Verwalten von Geräten](./iot-accelerators-remote-monitoring-manage.md)
* [Automate your solution with rules](./iot-accelerators-remote-monitoring-automate.md) (Automatisieren Ihrer Lösung mit Regeln)
* [Maintain your solution](iot-accelerators-remote-monitoring-maintain.md) (Warten von Lösungen)
