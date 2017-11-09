---
title: "Erste Schritte mit der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial werden simulierte Szenarien verwendet, um die vorkonfigurierte Remoteüberwachungslösung vorzustellen. Diese Szenarien werden erstellt, wenn Sie die vorkonfigurierte Remoteüberwachungslösung zum ersten Mal bereitstellen."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 09/16/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 222050410bfc809991f03a452eb6640e5f9e1802
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-preconfigured-solution"></a>Erkunden der Funktionen der vorkonfigurierten Remoteüberwachungslösung

In diesem Tutorial werden die wichtigsten Funktionen der vorkonfigurierten Remoteüberwachungslösung veranschaulicht. Zur Einführung in diese Funktionen werden im Tutorial gängige Kundenszenarien beschrieben, indem eine simulierte IoT-Anwendung für das Unternehmen Contoso verwendet wird.

Mit dem Tutorial können Sie sich mit den typischen IoT-Szenarien vertraut machen, die im Lieferumfang der Remoteüberwachungslösung enthalten sind.

In diesem Tutorial lernen Sie Folgendes:

>[!div class="checklist"]
> * Visualisieren und Filtern von Geräten im Dashboard
> * Reagieren auf einen Alarm
> * Aktualisieren der Firmware auf Ihren Geräten
> * Organisieren Ihrer Objekte

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Tutorials benötigen Sie eine bereitgestellte Instanz der Remoteüberwachungslösung in Ihrem Azure-Abonnement.

Falls Sie die Remoteüberwachungslösung noch nicht bereitgestellt haben, ist es ratsam, das Tutorial [Bereitstellen der vorkonfigurierten Remoteüberwachungslösung](iot-suite-remote-monitoring-deploy.md) durchzuarbeiten.

## <a name="the-contoso-sample-iot-deployment"></a>Contoso-Beispiel für IoT-Bereitstellung

Sie können das Contoso-Beispiel für die IoT-Bereitstellung nutzen, um sich mit den grundlegenden Szenarien vertraut zu machen, die im Lieferumfang der Remoteüberwachungslösung enthalten sind. Diese Szenarien basieren auf IoT-Bereitstellungen aus der Praxis. Sie werden die Remoteüberwachungslösung später wahrscheinlich an Ihre jeweiligen Anforderungen anpassen, aber anhand des Contoso-Beispiels lernen Sie erst einmal die Grundlagen kennen.

> [!NOTE]
> Wenn Sie die Befehlszeilenschnittstelle zum Bereitstellen der vorkonfigurierten Lösung verwendet haben, enthält die Datei `deployment-{your deployment name}-output.json` Informationen zur Bereitstellung, z.B. die URL für den Zugriff auf das bereitgestellte Beispiel.

Mit dem Contoso-Beispiel wird eine Reihe von simulierten Geräten und dazugehörigen Regeln bereitgestellt. Nachdem Sie sich mit den grundlegenden Szenarien vertraut gemacht haben, können Sie unter [Perform advanced device monitoring using the remote monitoring solution](iot-suite-remote-monitoring-monitor.md) (Durchführen der erweiterten Geräteüberwachung mit der Remoteüberwachungslösung) weitere Features der Lösung erkunden.

Contoso ist ein Unternehmen, das in unterschiedlichen Umgebungen eine Vielzahl von Objekten verwaltet. Contoso plant den Einsatz von cloudbasierten IoT-Anwendungen, um mehrere Objekte über eine zentralisierte Anwendung per Remotezugriff zu überwachen und zu verwalten. Die folgenden Abschnitte enthalten eine Zusammenfassung der anfänglichen Konfiguration für das Contoso-Beispiel:

> [!NOTE]
> Das Contoso-Demo ist nur eine Möglichkeit zur Bereitstellung von simulierten Geräten und für die Erstellung von Regeln. Zu den weiteren Bereitstellungsoptionen gehört auch das Erstellen Ihrer eigenen benutzerdefinierten Geräte. Weitere Informationen zur Erstellung Ihrer eigenen Geräte und Regeln finden Sie unter [Manage and configure your devices](iot-suite-remote-monitoring-manage.md) (Verwalten und Konfigurieren Ihrer Geräte) und [Detect issues using threshold-based rules](iot-suite-remote-monitoring-automate.md) (Erkennen von Problemen mit Regeln auf Schwellenwertbasis).

### <a name="contoso-devices"></a>Contoso-Geräte

Contoso setzt unterschiedliche Arten von intelligenten Geräten ein. Diese Geräte erfüllen für das Unternehmen verschiedene Aufgaben und senden unterschiedliche Telemetriedatenströme. Darüber hinaus verfügt jeder Gerätetyp über Geräteeigenschaften und unterstützte Methoden.

Die folgende Tabelle enthält eine Zusammenfassung der Typen von bereitgestellten Geräten:

| Gerätetyp        | Telemetrie                                  | Eigenschaften                                  | Tags                    | Methoden                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Kältemaschine            | Temperatur, Luftfeuchtigkeit, Druck            | Typ, Firmwareversion, Modell               | Standort, Etage, Gelände | Neustart, Firmwareupdate, Sicherheitsregelventil, Druckerhöhung                          |
| Gerät zur Prototyperstellung | Temperatur, Druck, geografischer Standort        | Typ, Firmwareversion, Modell               | Standort, Modus          | Neustart, Firmwareupdate, Gerät verschieben, Gerät beenden, Temperaturregelung, Temperaturanstieg |
| Motor             | Kraftstoffstand im Tank, Kühlmittelsensor, Vibration | Typ, Firmwareversion, Modell               | Standort, Etage, Gelände | Neustart, Firmwareupdate, Leerer Tank, Tank füllen                                              |
| LKW              | Geografischer Standort, Geschwindigkeit, Ladungstemperatur     | Typ, Firmwareversion, Modell               | Standort, Last          | Ladungstemperatur senken, Ladungstemperatur erhöhen, Firmwareupdate                         |
| Aufzug           | Boden, Vibration, Temperatur              | Typ, Firmwareversion, Modell, geografischer Standort | Standort, Gelände        | Aufzug stoppen, Aufzug starten, Firmwareupdate                                               |

> [!NOTE]
> Im Contoso-Demobeispiel werden zwei Geräte pro Typ bereitgestellt. Für jeden Typ funktioniert ein Gerät gemäß den von Contoso als normal festgelegten Grenzen, und für das andere Gerät liegt eine Fehlfunktion vor. Im nächsten Abschnitt werden die Regeln beschrieben, die Contoso für die Geräte definiert. Mit diesen Regeln werden die Grenzen des richtigen Verhaltens definiert.

### <a name="contoso-rules"></a>Contoso-Regeln

Die Bediener bei Contoso kennen die Schwellenwerte, die bestimmen, ob ein Gerät richtig funktioniert. Eine Kältemaschine funktioniert beispielsweise nicht richtig, wenn der gemeldete Druck höher als 250 PSI ist. In der folgenden Tabelle sind die Regeln auf Schwellenwertbasis aufgeführt, die von Contoso für die einzelnen Gerätetypen definiert werden:

| Regelname | Beschreibung | Schwellenwert | Schweregrad | Betroffene Geräte |
| --------- | ----------- | --------- | -------- | ---------------- |
| Druck der Kältemaschine zu hoch | Warnung, wenn Kältemaschinen einen höheren Druck als normal aufweisen   |Druck > 250 psi       | Kritisch | Kältemaschinen            |
| Temperatur des Geräts für die Prototyperstellung zu hoch  | Warnung, wenn Geräte für die Prototyperstellung höhere Temperaturen als normal aufweisen  |Temperatur > 80 &deg;F |Kritisch | Geräte für die Prototyperstellung |
| Motortank leer  | Warnung, wenn der Kraftstofftank des Motors in Kürze leer ist                     | Kraftstoff < 5 Gallonen | Information     | Motoren             |
| Temperatur der Ladung höher als normal | Warnung, wenn die Ladungstemperatur eines LKW höher als normal ist                 | Temperatur < 45 &deg;F |Warnung  | LKW              |
| Vibrationsstopp des Aufzugs      | Warnung, wenn der Aufzug vollständig stillsteht (basierend auf der Vibration)                     | Vibration < 0,1 mm |Warnung  | Aufzüge           |

### <a name="operate-the-contoso-sample-deployment"></a>Durchführen der Bereitstellung für das Contoso-Beispiel

Sie haben jetzt die anfängliche Einrichtung im Contoso-Beispiel gesehen. In den folgenden Abschnitten werden drei Szenarien im Contoso-Beispiel beschrieben, die veranschaulichen, wie die vorkonfigurierte Lösung von einem Bediener genutzt werden kann.

## <a name="respond-to-a-pressure-alarm"></a>Reagieren auf einen Druckalarm

Dieses Szenario verdeutlicht, wie Sie einen Alarm identifizieren, der von einer Kältemaschine ausgelöst wird, und darauf reagieren. Die Kältemaschine befindet sich in Redmond, Gebäude 43, 2. Etage.

Ihnen als Bediener wird im Dashboard ein Alarm in Bezug auf den Druck einer Kältemaschine angezeigt. Sie können die Karte schwenken und zoomen, um weitere Details anzuzeigen.

1. Auf der Seite **Dashboard** im Raster **System Alarms** (Systemalarme) wird der Alarm **Chiller pressure too high** (Druck der Kältemaschine zu hoch) angezeigt. Außerdem wird die Kältemaschine in der Karte hervorgehoben:

    ![Dashboard: Druckalarm und Gerät in der Karte](media/iot-suite-remote-monitoring-explore/dashboardalarm.png)

1. Klicken Sie in der Karte auf die hervorgehobene Kältemaschine, um die Gerätedetails und Telemetriedaten anzuzeigen. In den Telemetriedaten wird ein hoher Wert für den Druck angezeigt:

    ![Auswählen des Geräts in der Karte, um weitere Details anzuzeigen](media/iot-suite-remote-monitoring-explore/dashboarddetail.png)

1. Wählen Sie **Device detail** (Gerätedetails).

1. Klicken Sie zum Navigieren zur Seite **Wartung** in der Spalte **Explore Alarm** (Alarm untersuchen) neben dem Alarm im Alarmraster auf **...**.

Auf der Seite **Wartung** können Sie die Details der Regel anzeigen, über die der Druckalarm für die Kältemaschine ausgelöst wurde.

1. Sie können die Anzahl von Alarmauslösungen, Bestätigungen und offene und geschlossene Alarme sehen:

    ![Seite „Wartung“ mit Liste der ausgelösten Alarme](media/iot-suite-remote-monitoring-explore/maintenancealarmlist.png)

1. Der erste Alarm in der Liste ist der zuletzt aufgetretene Alarm. Klicken Sie auf den Alarm **Chiller Pressure** (Druck der Kältemaschine), um die zugeordneten Geräte und Telemetriedaten anzuzeigen. In den Telemetriedaten wird für die Kältemaschine ein hoher Druck angezeigt:

    ![Seite „Wartung“ mit Telemetriedaten für den ausgewählten Alarm](media/iot-suite-remote-monitoring-explore/maintenancetelemetry.png)

Sie haben nun das Problem und das zugeordnete Gerät identifiziert, für das der Alarm ausgelöst wurde. Als Bediener sind die nächsten Schritte das Bestätigen des Alarms und das Beheben des Problems.

1. Ändern Sie den **Alarmstatus** in **Bestätigt**, um anzugeben, dass Sie jetzt an der Behebung des Alarms arbeiten:

    ![Auswählen und Bestätigen des Alarms](media/iot-suite-remote-monitoring-explore/maintenanceacknowledge.png)

1. Wählen Sie die Kältemaschine aus, und klicken Sie auf **Schedule** (Zeitplan). Wählen Sie **EmergencyValveRelease**, fügen Sie den Auftragsnamen **ChillerPressureRelease** hinzu, und wählen Sie die Option **Übernehmen**. Mit diesen Einstellungen wird ein Auftrag erstellt, der sofort ausgeführt wird:

    ![Auswählen des Geräts und Planen einer Aktion](media/iot-suite-remote-monitoring-explore/maintenanceschedule.png)

1. Kehren Sie zum Anzeigen des Auftragsstatus auf die Seite **Wartung** zurück, und zeigen Sie die Liste mit den Aufträgen in der Ansicht **Systemstatus** an. Sie sehen, dass der Auftrag ausgeführt wird, um den Ventildruck in der Kältemaschine zu verringern:

    ![Status der Aufträge unter „Systemstatus“](media/iot-suite-remote-monitoring-explore/maintenancerunningjob.png)

Bestätigen Sie abschließend, dass die Telemetriewerte der Kältemaschine wieder normal sind.

1. Navigieren Sie zum Anzeigen des Alarmrasters zur Seite **Dashboard**.

1. Wählen Sie in der Karte das Gerät für den ursprünglichen Alarm aus, um die Gerätetelemetrie anzuzeigen und sicherzustellen, dass wieder Normalzustand herrscht.

1. Navigieren Sie zum Schließen des Vorfalls zur Seite **Wartung**, wählen Sie den Alarm aus, und legen Sie den Status auf **Geschlossen** fest:

    ![Auswählen und Schließen des Alarms](media/iot-suite-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Aktualisieren der Gerätefirmware

Contoso testet einen Gerätetyp im Außendienst. Im Rahmen des Testzyklus müssen Sie sicherstellen, dass Firmwareupdates für das Gerät richtig funktionieren. Die folgenden Schritte veranschaulichen, wie Sie die Remoteüberwachungslösung nutzen, um die Firmware auf mehreren Geräten zu aktualisieren.

Verwenden Sie die Seite **Geräte**, um die erforderlichen Aufgaben für die Geräteverwaltung durchzuführen. Filtern Sie zuerst nach allen Geräten für die Prototyperstellung:

1. Navigieren Sie zur Seite **Geräte**. Wählen Sie in der Dropdownliste **Filter** den Filter **Prototyping devices** (Geräte für die Prototyperstellung):

    ![Verwenden eines Filters auf der Seite „Geräte“](media/iot-suite-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Klicken Sie auf **Verwalten**, um die verfügbaren Filter zu verwalten.

1. Wählen Sie ein Gerät für die Prototyperstellung aus:

    ![Auswählen eines Geräts auf der Seite „Geräte“](media/iot-suite-remote-monitoring-explore/devicesselect.png)

1. Klicken Sie auf die Schaltfläche **Schedule** (Zeitplan), und wählen Sie die Option **Firmwareupdate**. Geben Sie Werte für **Job name** (Auftragsname) und **Firmware URI** (Firmware-URI) ein. Wählen Sie **Übernehmen**, um den Auftrag zu planen, der jetzt ausgeführt werden soll:

    ![Planen des Firmwareupdates für das Gerät](media/iot-suite-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Mit den simulierten Geräten können Sie eine beliebige URL als Wert für **Firmware URI** (Firmware-URI) verwenden. Von den simulierten Geräten wird nicht auf die URL zugegriffen.

1. Notieren Sie sich, wie viele Geräte von dem Auftrag betroffen sind, und wählen Sie **Übernehmen**:

    ![Senden des Auftrags für das Firmwareupdate](media/iot-suite-remote-monitoring-explore/devicessubmitupdate.png)

Sie können die Seite **Wartung** verwenden, um den Auftrag während der Ausführung nachzuverfolgen.

1. Navigieren Sie zum Anzeigen der Liste mit den Aufträgen zur Seite **Wartung**, und klicken Sie auf **Systemstatus**.

1. Suchen Sie nach dem Ereignis, das zu dem von Ihnen erstellten Auftrag gehört. Vergewissern Sie sich, dass der Prozess für das Firmwareupdate richtig initiiert wurde.

Sie können einen Filter erstellen, um sicherzustellen, dass die Firmwareversion richtig aktualisiert wurde.

1. Navigieren Sie zum Erstellen eines Filters zur Seite **Geräte**, und wählen Sie **Verwalten**:

    ![Verwalten von Gerätefiltern](media/iot-suite-remote-monitoring-explore/devicesmanagefilters.png)

1. Erstellen Sie einen Filter, der nur Geräte mit der neuen Firmwareversion einschließt:

    ![Erstellen eines Gerätefilters](media/iot-suite-remote-monitoring-explore/devicescreatefilter.png)

1. Wechseln Sie zurück zur Seite **Geräte**, und überprüfen Sie, ob das Gerät über die neue Firmwareversion verfügt.

## <a name="organize-your-assets"></a>Organisieren Ihrer Objekte

Contoso verfügt über zwei unterschiedliche Teams für Außendienstaktivitäten:

* Das Team „Smart Building“ (Intelligentes Gebäude) verwaltet Kältemaschinen, Aufzüge und Motoren.
* Das Team „Smart Vehicle“ (Intelligentes Fahrzeug) verwaltet LKW und Geräte für die Prototyperstellung.

Um für Sie als Bediener das Organisieren und Verwalten Ihrer Geräte zu vereinfachen, möchten Sie die Teams mit dem entsprechenden Teamnamen kennzeichnen.

Sie können Tagnamen erstellen, die für Geräte verwendet werden.

1. Navigieren Sie zum Anzeigen aller Geräte zur Seite **Geräte**, und wählen Sie den Filter **Alle Geräte**:

    ![Anzeigen aller Geräte](media/iot-suite-remote-monitoring-explore/devicesalldevices.png)

1. Wählen Sie die Geräte **Trucks** (LKW) und **Prototyping devices** (Geräte für die Prototyperstellung). Wählen Sie anschließend die Option **Tag**:

    ![Auswählen von Prototyp- und LKW-Geräten](media/iot-suite-remote-monitoring-explore/devicesmultiselect.png)

1. Wählen Sie die Option **Tag**, und erstellen Sie dann ein neues Zeichenfolgentag mit dem Namen **FieldService** und dem Wert **ConnectedVehicle**. Wählen Sie einen Namen für den Auftrag aus. Klicken Sie anschließend auf **Übernehmen**:

    ![Hinzufügen von Tags zu Prototyp- und LKW-Geräten](media/iot-suite-remote-monitoring-explore/devicesaddtag.png)

1. Wählen Sie die Geräte **Chiller** (Kältemaschine), **Elevator** (Aufzug) und **Engine** (Motor) aus. Wählen Sie anschließend die Option **Tag**:

    ![Auswählen der Geräte „Chiller“ (Kältemaschine), „Elevator“ (Aufzug) und „Engine“ (Motor)](media/iot-suite-remote-monitoring-explore/devicesmultiselect2.png)

1. Wählen Sie die Option **Tag**, und erstellen Sie anschließend ein neues Tag mit dem Namen **FieldService** und dem Wert **SmartBuilding**. Wählen Sie einen Namen für den Auftrag aus. Klicken Sie anschließend auf **Speichern**:

    ![Hinzufügen eines Tags zu den Geräten „Chiller“ (Kältemaschine), „Engine“ (Motor) und „Elevator“ (Aufzug)](media/iot-suite-remote-monitoring-explore/devicesaddtag2.png)

Sie können die Tagwerte zum Erstellen von Filtern verwenden.

1. Wählen Sie auf der Seite **Geräte** die Option **Manage filters** (Filter verwalten):

    ![Verwalten von Gerätefiltern](media/iot-suite-remote-monitoring-explore/devicesmanagefilters.png)

1. Erstellen Sie einen neuen Filter, für den der Tagname **FieldService** und der Wert **SmartBuilding** verwendet werden. Speichern Sie den Filter unter dem Namen **Smart Building**.

1. Erstellen Sie einen neuen Filter, für den der Tagname **FieldService** und der Wert **ConnectedVehicle** verwendet werden. Speichern Sie den Filter unter dem Namen **Connected Vehicle**.

Der Contoso-Bediener kann Geräte jetzt anhand des Betriebsteams abfragen, ohne auf den Geräten hierfür etwas ändern zu müssen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

>[!div class="checklist"]
> * Visualisieren und Filtern von Geräten im Dashboard
> * Reagieren auf einen Alarm
> * Aktualisieren der Firmware auf Ihren Geräten
> * Organisieren Ihrer Objekte

Nachdem Sie sich mit der Remoteüberwachungslösung vertraut gemacht haben, schlagen wir vor, dass Sie sich als Nächstes über die erweiterten Features der Remoteüberwachungslösung informieren:

* [Monitor your devices](./iot-suite-remote-monitoring-monitor.md) (Überwachen von Geräten)
* [Verwalten von Geräten](./iot-suite-remote-monitoring-manage.md)
* [Automate your solution with rules](./iot-suite-remote-monitoring-automate.md) (Automatisieren Ihrer Lösung mit Regeln)
* [Maintain your solution](./iot-suite-remote-monitoring-maintain.md) (Warten von Lösungen)
