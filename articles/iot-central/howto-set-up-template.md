---
title: Einrichten einer Gerätevorlage in einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Informationen zum Einrichten einer Gerätevorlage mit Messungen, Einstellungen, Eigenschaften, Regeln und Dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: a076ee5275a4d053613902a1980542590263385c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034273"
---
# <a name="set-up-a-device-template"></a>Einrichten einer Gerätevorlage

Eine Gerätevorlage ist eine Blaupause zur Definition der Merkmale und Verhaltensweisen eines Gerätetyps, der eine Verbindung mit einer Microsoft Azure IoT Central-Anwendung herstellt.

Beispielsweise kann ein Hersteller eine Gerätevorlage für einen mit IoT verbundenen Lüfter mit folgenden Merkmalen erstellen:

- Messung der Temperaturtelemetrie

- Messung von Lüftermotor-Fehlerereignissen

- Messung des Lüfterbetriebszustands

- Einstellung der Lüfterdrehzahl

- Standorteigenschaft

- Regeln zum Senden von Warnungen

- Dashboard mit einer allgemeinen Übersicht über das Gerät

Anhand dieser Gerätevorlage kann ein Operator reale Lüftergeräte mit Namen wie z.B. **Lüfter 1** und **Lüfter 2** erstellen und verbinden. All diese Lüfter weisen Messungen, Einstellungen, Eigenschaften, Regeln und ein Dashboard auf, die von Benutzern Ihrer Anwendung überwacht und verwaltet werden können.

> [!NOTE]
> Nur Ersteller und Administratoren können Gerätevorlagen erstellen, bearbeiten und löschen. Auf der Seite **Device Explorer** kann jeder Benutzer Geräte anhand vorhandener Gerätevorlagen erstellen.

## <a name="create-a-device-template"></a>Erstellen einer Gerätevorlage

1. Wechseln Sie zur Seite **Anwendungs-Generator**.

2. Wählen Sie zum Erstellen einer leeren Vorlage **Gerätevorlage erstellen** und dann **Benutzerdefiniert** aus.

3. Geben Sie einen Namen für Ihre neue Gerätevorlage ein, und wählen Sie **Erstellen** aus.

   ![Seite mit Gerätedetails mit „Refrigerator“ als Name der Vorlage](./media/howto-set-up-template/devicedetailspage.png)

4. Sie befinden sich jetzt auf der Seite **Gerätedetails** eines neuen simulierten Geräts. Ein simuliertes Gerät wird automatisch für Sie erstellt, wenn Sie eine Gerätevorlage erstellen. Es meldet Daten und kann genau wie ein echtes Gerät gesteuert werden.

Sehen Sie sich nun die einzelnen Registerkarten auf der Seite **Gerätedetails** an.

## <a name="measurements"></a>Messungen

Messungen sind die Daten, die von Ihrem Gerät stammen. Sie können Ihrer Gerätevorlage mehrere Messungen hinzufügen, die den Funktionen Ihres Geräts entsprechen.

- **Telemetriemessungen** sind die numerischen Datenpunkte, die im Lauf der Zeit von Ihrem Gerät erfasst werden. Diese werden als kontinuierlicher Datenstrom dargestellt. Ein Beispiel ist Temperatur.
- **Ereignismessungen** sind zeitpunktbezogene Daten, die ein bedeutsames Ereignis auf dem Gerät darstellen. Ein Schweregrad stellt die Bedeutung eines Ereignisses dar. Ein Beispiel ist ein Lüftermotorfehler.
- **Zustandsmessungen** bilden den Zustand des Geräts oder der zugehörigen Komponenten über einen Zeitraum ab. Beispielsweise können für den Lüftermodus **In Betrieb** und **Angehalten** als die beiden möglichen Zustandsangaben definiert werden.

### <a name="create-a-telemetry-measurement"></a>Erstellen einer Telemetriemessung
Klicken Sie zum Hinzufügen neuer Telemetriemessung auf **Vorlage bearbeiten**, und klicken Sie dann auf die Schaltfläche **+ New Measurement** (+ Neue Messung). Wählen Sie als Messungstyp **Telemetrie** aus, und geben Sie die Details im Formular **Telemetrie erstellen** ein.

> [!NOTE]
> Wenn ein echtes Gerät verbunden ist, achten Sie auf den Namen der vom Gerät gemeldeten Messung. Der Name muss mit dem Eintrag **Feldname** einer Messung genau übereinstimmen.

Beispielsweise können Sie eine neue Temperaturtelemetriemessung hinzufügen:

![Formular „Telemetrie erstellen“ mit Details zur Temperaturmessung](./media/howto-set-up-template/measurementsform.png)

Nachdem Sie auf **Fertig** geklickt haben, wird die Messung **Temperatur** in der Liste der Messungen angezeigt. Ein Bediener kann die Visualisierung der vom Gerät erfassten Temperaturdaten einsehen.

> [!NOTE]
  Die Telemetriemessung gibt Daten vom Typ „double“ aus.

### <a name="create-an-event-measurement"></a>Erstellen einer Ereignismessung
Klicken Sie zum Hinzufügen neuer Ereignismessung auf **Vorlage bearbeiten**, und klicken Sie dann auf die Schaltfläche **+ New Measurement** (+ Neue Messung). Wählen Sie als Messungstyp **Ereignis**, und geben Sie die Details im Formular **Ereignis erstellen** ein.

Geben Sie in diesem Formular den **Anzeigenamen**, **Feldnamen** und Details zum **Schweregrad** des Ereignisses an. Ihnen stehen die drei verfügbare Schweregrade zur Auswahl: **Fehler**, **Warnung** und **Information**.  

Beispielsweise können Sie das neue Ereignis **Lüftermotorfehler** hinzufügen.

![Formular „Ereignis erstellen“ mit Details zu einem Lüftermotorereignis](./media/howto-set-up-template/eventmeasurementsform.png)

Nachdem Sie auf **Fertig** geklickt haben, wird die Messung **Lüftermotorfehler** in der Liste der Messungen angezeigt. Ein Bediener kann die Visualisierung der vom Gerät gesendeten Ereignisdaten einsehen.

![Ereignismessungsdiagramm](./media/howto-set-up-template/eventmeasurementschart.png)

Klicken Sie im Diagramm auf das Ereignissymbol, um zusätzliche Ereignisdetails anzuzeigen.

![Details für das Ereignis „Lüftermotorfehler“](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
  Die Ereignismessung gibt Daten vom Typ „string“ aus.

### <a name="create-a-state-measurement"></a>Erstellen einer Zustandsmessung
Klicken Sie zum Hinzufügen neuer Zustandsmessung auf **Vorlage bearbeiten**, und klicken Sie dann auf die Schaltfläche **+ New Measurement** (+ Neue Messung). Wählen Sie als Messungstyp **Zustand**, und geben Sie die Details im Formular **Zustand erstellen** ein.

Geben Sie in diesem Formular den **Anzeigenamen**, **Feldnamen** und **Werte** für den Zustand an. Jeder Wert kann auch einen Anzeigenamen aufweisen, der bei der Anzeige des Werts in Diagrammen und Tabellen verwendet wird.

Sie können z.B. den neuen Zustand **Lüftermodus** hinzufügen, der zwei mögliche Werte hat, die vom Gerät gesendet werden können: **In Betrieb** und **Angehalten**.

![Formular „Zustand bearbeiten“ mit Details zum Lüftermodus](./media/howto-set-up-template/statemeasurementsform.png)

Nachdem Sie auf **Fertig** geklickt haben, wird die Messung **Lüftermodus** in der Liste der Messungen angezeigt. Der Bediener kann die Visualisierung der vom Gerät gesendeten Zustandsdaten einsehen.

![Zustandsmessungsdiagramm](./media/howto-set-up-template/statemeasurementschart.png)

Wenn das Gerät in einer kleinen Zeitspanne zu viele Datenpunkte sendet, wird die Zustandsmessung mit einem anderen visuellen Element angezeigt (siehe den folgenden Screenshot). Wenn Sie auf das Diagramm klicken, werden alle Datenpunkte innerhalb dieses Zeitraums in chronologischer Reihenfolge angezeigt. Sie können den Zeitbereich auch einschränken, um die Messung im Diagramm anzuzeigen.

> [!NOTE]
  Die Statusmessung gibt Daten vom Typ „string“ aus.

## <a name="settings"></a>Einstellungen

Ein Gerät wird durch Einstellungen gesteuert. Diese ermöglichen den Operatoren Ihrer Anwendung die Angabe von Eingaben für das Gerät. Sie können Ihrer Gerätevorlage mehrere Einstellungen hinzufügen, die auf der Registerkarte **Einstellungen** als Kacheln angezeigt und von Operatoren verwendet werden können. Sie können sechs Typen von Einstellungen hinzufügen: Drehzahl, Text, Datum, Ein-/Ausschalten, Auswahlliste und Abschnittsbezeichnung.

> [!NOTE]
> Wenn ein echtes Gerät verbunden ist, achten Sie auf den Namen der vom Gerät gemeldeten Einstellung. Der Name muss mit dem Eintrag **Feldname** einer Einstellung genau übereinstimmen.

Einstellungen können einen von drei Zuständen aufweisen. Das Gerät meldet diese Zustände.

- **Synchronisiert**: Das Gerät wurde entsprechend dem Einstellungswert geändert.

- **Ausstehend**: Das Gerät wird zurzeit in den Einstellungswert geändert.

- **Fehler**: Das Gerät hat einen Fehler zurückgegeben.

Sie können zum Beispiel eine neue Einstellung für die Lüfterdrehzahl hinzufügen, indem Sie auf **Vorlage bearbeiten** klicken und die neue Einstellung eingeben:

![Formular „Drehzahl konfigurieren“ mit Details zu Geschwindigkeitseinstellungen](./media/howto-set-up-template/settingsform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird die Einstellung **Lüfterdrehzahl** als Kachel angezeigt und kann verwendet werden, um die Lüfterdrehzahl des Geräts zu ändern.

Nachdem Sie eine Kachel erstellt haben, können Sie Ihre neue Einstellung ausprobieren. Klicken Sie zunächst rechts oben auf dem Bildschirm auf **Fertig**.

![Registerkarte „Einstellungen“ mit dem Schalter „Entwurfsmodus“ für die Kachel](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Eigenschaften

Eigenschaften sind die dem Gerät zugeordneten Gerätemetadaten, z.B. Gerätestandort und Seriennummer. Sie können Ihrer Gerätevorlage mehrere Eigenschaften hinzufügen, die auf der Registerkarte **Eigenschaften** als Kacheln angezeigt werden. Ein Bediener kann die Werte für Eigenschaften beim Erstellen eines neuen Geräts angeben und sie anschließend jederzeit bearbeiten. Sie können sechs Typen von Eigenschaften hinzufügen: Drehzahl, Text, Datum, Ein-/Ausschalten, Geräteeigenschaft und Bezeichnung.

Es gibt zwei Kategorien von Eigenschaften:

- Eigenschaften des **Geräts**, die das Gerät meldet.
- Eigenschaften der **Anwendung**, die ausschließlich in der Anwendung gespeichert werden. Das Gerät erkennt keine Anwendungseigenschaften.

> [!NOTE]
> Wenn ein echtes Gerät verbunden ist, achten Sie bei den Geräteeigenschaften auf den Namen der vom Gerät gemeldeten Eigenschaft. Der Name muss mit dem Eintrag **Feldname** der Eigenschaft genau übereinstimmen. Bei den Anwendungseigenschaften kann der Feldname einen beliebigen Wert aufweisen, solange er in der Gerätevorlage eindeutig ist.

Sie können zum Beispiel den Gerätestandort als neue Eigenschaft hinzufügen, indem Sie auf **Vorlage bearbeiten** klicken und die neue Eigenschaft eingeben:

![Formular „Text konfigurieren“ auf der Registerkarte „Eigenschaften“](./media/howto-set-up-template/propertiesform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird der Gerätestandort als Kachel angezeigt:

![Kachel „Standort“](./media/howto-set-up-template/propertiestile.png)

Nach dem Erstellen einer Kachel können Sie den Wert der Eigenschaft ändern. Klicken Sie zunächst rechts oben auf dem Bildschirm auf **Fertig**.

### <a name="create-a-location-property-through-azure-maps"></a>Erstellen einer Standorteigenschaft über Azure Maps
Sie können Ihre Standortdaten mit geografischem Kontext in Azure IoT Central versehen und Breiten- und Längengradkoordinaten einer Anschrift zuordnen. Oder Sie können einfach Breiten- und Längengradkoordinaten zuordnen. Azure Maps unterstützt diese Funktion in IoT Central.

Sie können zwei Arten von Standorteigenschaften hinzufügen:
- **Standort als eine Anwendungseigenschaft**, die ausschließlich in der Anwendung gespeichert wird. Das Gerät erkennt keine Anwendungseigenschaften.
- **Standort als eine Geräteeigenschaft**, die vom Gerät gemeldet wird.

#### <a name="add-location-as-an-application-property"></a>Hinzufügen eines Standorts als eine Anwendungseigenschaft 
Sie können mithilfe von Azure Maps in Ihrer Azure IoT Central-Anwendung eine Standorteigenschaft als eine Anwendungseigenschaft erstellen. Beispielsweise können Sie die Geräteinstallationsadresse hinzufügen. 

1. Klicken Sie auf der Registerkarte **Eigenschaften** auf **Vorlage bearbeiten**.

   ![Registerkarte „Eigenschaften“ mit aktiviertem Entwurfsmodus](./media/howto-set-up-template/locationcloudproperty1.png)

2. Wählen Sie in der Bibliothek **Standort** aus.
3. Konfigurieren Sie den **Anzeigenamen**, **Feldnamen** und (optional) den **Anfangswert** des Standorts. 

   ![Formular „Standort konfigurieren“ mit Details zum Standort](./media/howto-set-up-template/locationcloudproperty2.png)

   Es gibt zwei unterstützte Formate zum Hinzufügen eines Standorts:
   - **Standort als Adresse**
   - **Standort als Koordinaten** 

4. Klicken Sie auf **Speichern** und **Fertig**. Ab sofort kann ein Operator den Standortwert im Formular für das Standortfeld aktualisieren. 

#### <a name="add-location-as-a-device-property"></a>Hinzufügen eines Standorts als eine Geräteeigenschaft 

Sie können eine Standorteigenschaft als eine Geräteeigenschaft erstellen, die vom Gerät gemeldet wird. Angenommen, Sie möchten den Gerätestandort nachverfolgen:

1. Klicken Sie auf der Registerkarte **Eigenschaften** auf **Vorlage bearbeiten**.

   ![Registerkarte „Eigenschaften“ mit aktiviertem Entwurfsmodus](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Klicken Sie in der Bibliothek auf **Geräteeigenschaft**.
3. Konfigurieren Sie den Anzeigenamen und den Feldnamen, und wählen Sie **Standort** als Datentyp. 

   > [!NOTE]
   > Der Feldname muss mit dem Namen der Eigenschaft genau übereinstimmen, die das Gerät meldet. 

   ![Formular „Geräteeigenschaften konfigurieren“ mit Details zum Standort](./media/howto-set-up-template/locationdeviceproperty2.png)

Nachdem Sie nun Ihre Standorteigenschaft konfiguriert haben, können Sie [eine Karte hinzufügen, um den Standort auf dem Gerätedashboard zu visualisieren](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Befehle

Befehle werden verwendet, um ein Gerät remote zu verwalten. Sie ermöglichen Bedienern Ihrer Anwendung die sofortige Ausführung von Befehlen auf dem Gerät. Sie können Ihrer Gerätevorlage mehrere Befehle hinzufügen, die auf der Registerkarte **Befehle** als Kacheln angezeigt und von Bedienern verwendet werden können. Als Gerätehersteller können Sie Befehle nach Ihren Wünschen flexibel definieren.

Wie unterscheidet sich ein Befehl von einer Einstellung? 

* **Einstellung**: Eine Einstellung ist eine Konfiguration, die Sie auf ein Gerät anwenden möchten. Sie möchten außerdem, dass das Gerät diese Konfiguration beibehält, bis Sie sie ändern. Angenommen, Sie möchten die Temperatur Ihres Gefrierschranks einstellen, und Sie wünschen diese Einstellung auch dann, wenn der Gefrierschrank neu gestartet wird. 

* **Befehl**: Sie verwenden Befehle, um einen Befehl auf dem Gerät sofort von IoT Central aus remote auszuführen. Wenn ein Gerät nicht angeschlossen ist, wird der Befehl mit einem Fehler abgebrochen. Angenommen, Sie möchten ein Gerät neu starten.  

Wenn Sie einen Befehl ausführen, kann es einen von drei Zuständen geben, je nachdem, ob das Gerät den Befehl empfangen hat.

Sie können zum Beispiel einen neuen **Echo**-Befehl hinzufügen, indem Sie auf **Vorlage bearbeiten** und dann auf **+ New Command** (+ Neuer Befehl) klicken. Anschließend geben Sie den neuen Befehl ein:

![Formular „Befehl konfigurieren“ mit Details zu „Echo“](./media/howto-set-up-template/commandsecho.png)

Nachdem Sie auf **Speichern** und auf **Fertig** geklickt haben, wird der Befehl **Echo** als Kachel angezeigt und kann verwendet werden, um eine Echoanforderung an das Gerät zu senden.

Nachdem Sie eine Kachel erstellt haben, können Sie Ihren neuen Befehl ausprobieren.

## <a name="rules"></a>Regeln

Anhand von Regeln können Operatoren Geräte nahezu in Echtzeit überwachen. Regeln rufen automatisch Aktionen auf. Beispielsweise kann bei Auslösen der Regel eine E-Mail gesendet werden. Zurzeit ist ein Regeltyp verfügbar:

- Eine **Telemetrieregel** wird ausgelöst, wenn die ausgewählten Gerätetelemetriedaten einen angegebenen Schwellenwert überschreiten. [Weitere Informationen zu Telemetrieregeln](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Im Dashboard findet ein Operator Informationen zu einem Gerät. Als Hersteller können Sie dieser Seite Kacheln hinzufügen, mit deren Hilfe Bediener das Verhalten des Geräts nachvollziehen können. Sie können Ihrer Gerätevorlage mehrere Dashboardkacheln hinzufügen. Sie können sechs Typen von Dashboardkacheln hinzufügen: Bild, Liniendiagramm, Balkendiagramm, KPI, Einstellungen und Eigenschaften sowie Bezeichnung.

Beispielsweise können Sie eine Kachel **Einstellungen und Eigenschaften** hinzufügen, um eine Auswahl der aktuellen Werte für Einstellungen und Eigenschaften anzuzeigen, indem Sie auf **Vorlage bearbeiten** klicken und die Kachel aus der Bibliothek auswählen:

![Formular „Gerätedetails konfigurieren“ mit Details zu Einstellungen und Eigenschaften](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Wenn ein Operator nun das Dashboard aufruft, wird ihm diese Kachel mit den Eigenschaften und Einstellungen des Geräts angezeigt:

![Registerkarte „Dashboard“ mit Anzeige der Einstellungen und Eigenschaften für die Kachel](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Hinzufügen eines Azure Maps-Standorts im Dashboard

Wenn Sie zuvor in [Erstellen einer Standorteigenschaft über Azure Maps](#create-a-location-property-through-azure-maps) eine Standorteigenschaft konfiguriert haben, können Sie den Standort mithilfe einer Karte im Dashboard Ihres Geräts visualisieren.

1. Klicken Sie auf der Registerkarte **Dashboard** auf **Vorlage bearbeiten**.

   ![Registerkarte „Dashboard“ mit aktiviertem Entwurfsmodus](./media/howto-set-up-template/locationcloudproperty4map.png)

2. Wählen Sie auf dem Gerätedashboard in der Bibliothek **Karte** aus. 
3. Geben Sie einen Titel ein, und wählen Sie die Standorteigenschaft aus, die Sie zuvor als Teil Ihrer Geräteeigenschaften konfiguriert haben.

   ![Formular „Karte konfigurieren“ mit Details zu Titel und Eigenschaften](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Wählen Sie **Speichern**aus. Die Kartenkachel zeigt jetzt den Standort an, den Sie ausgewählt haben. 

   ![Kartenkachel mit ausgewähltem Standort](./media/howto-set-up-template/locationcloudproperty6map.png) 

Sie können die Karte in die gewünschte Größe ändern. Wenn ein Bediener nun das Dashboard anzeigt, kann dieser alle von Ihnen konfigurierten Dashboardkacheln einsehen, einschließlich einer Standortkarte.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung eingerichtet wird, wird als Nächstes Folgendes empfohlen:

> [!div class="nextstepaction"]
> [Erstellen einer neuen Gerätevorlagenversion](howto-version-devicetemplate.md)
