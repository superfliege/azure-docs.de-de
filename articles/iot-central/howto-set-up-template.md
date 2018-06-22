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
ms.openlocfilehash: bda056a75ae9d696dab389b85fe1bfb2935ee1a8
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261983"
---
# <a name="set-up-a-device-template"></a>Einrichten einer Gerätevorlage

Eine Gerätevorlage ist eine Blaupause zur Definition der Merkmale und Verhaltensweisen eines Gerätetyps, der eine Verbindung mit einer Microsoft Azure IoT Central-Anwendung herstellt.

Beispielsweise kann ein Ersteller eine Gerätevorlage für einen mit IoT verbundenen Lüfter mit folgenden Merkmalen erstellen:

- Messung der Temperaturtelemetrie

- Messung von Lüftermotor-Fehlerereignissen

- Messung des Lüfterbetriebszustands

- Einstellung der Lüfterdrehzahl

- Standorteigenschaft

- Regeln zum Senden von Warnungen

- Dashboard mit einer Übersicht über das Gerät

Anhand dieser Gerätevorlage kann ein Operator reale Lüftergeräte mit Namen wie z.B. **Lüfter 1** und **Lüfter 2** erstellen und verbinden. All diese Lüfter weisen Messungen, Einstellungen, Eigenschaften, Regeln und ein Dashboard auf, die von Benutzern Ihrer Anwendung überwacht und verwaltet werden können.

> [!NOTE]
Nur Ersteller und Administratoren können Gerätevorlagen erstellen, bearbeiten und löschen. Auf der Seite **Device Explorer** kann jeder Benutzer Geräte anhand von vorhandenen Gerätevorlagen erstellen.

## <a name="create-a-new-device-template"></a>Erstellen einer neuen Gerätevorlage

1. Wechseln Sie zur Seite **Anwendungs-Generator**.

1. Wählen Sie zum Erstellen einer leeren Vorlage **Gerätevorlage erstellen**, und wählen Sie dann **Benutzerdefiniert**.

1. Geben Sie einen Namen für Ihre neue Gerätevorlage ein, und wählen Sie **Erstellen**.

    ![Seite mit Gerätedetails](./media/howto-set-up-template/devicedetailspage.png)

1. Sie befinden sich jetzt auf der Seite **Gerätedetails** eines neuen simulierten Geräts. Ein simuliertes Gerät wird automatisch für Sie erstellt, wenn Sie eine neue Gerätevorlage erstellen. Es meldet Daten und kann genau wie ein echtes Gerät gesteuert werden.

Sehen Sie sich nun die einzelnen Registerkarten auf der Seite **Gerätedetails** an.

## <a name="measurements"></a>Messungen

Messungen sind die Daten, die von Ihrem Gerät übertragen werden. Sie können Ihrer Gerätevorlage mehrere Messungen hinzufügen, die den Funktionen Ihres Geräts entsprechen. Zurzeit werden Telemetrie und Ereignisse als Typen von Messungen unterstützt.

- **Telemetriemessungen** sind die numerischen Datenpunkte, die im Lauf der Zeit von Ihrem Gerät erfasst und in einem kontinuierlichen Datenstrom dargestellt werden. Dazu gehört beispielsweise die Temperatur.
- **Ereignismessungen** sind Point-in-Time-Daten, die ein bedeutsames Ereignis auf dem Gerät darstellen. Den Ereignissen sind Schweregrade zugeordnet, die die Wichtigkeit des Ereignisses widerspiegeln. Dazu gehört beispielsweise ein Fehler des Lüftermotors.
- **Zustandsmessungen** bilden den Zustand des Geräts oder der zugehörigen Komponenten über einen Zeitraum ab. Beispielsweise können für den Lüftermodus „In Betrieb“ und „Angehalten“ als mögliche Zustandsangaben definiert werden.

### <a name="create-a-telemetry-measurement"></a>Erstellen einer Telemetriemessung
Klicken Sie zum Hinzufügen einer neuen Telemetriemessung auf die Schaltfläche **+ Neue Messung**. Ein Formular mit Optionen zur Auswahl des Messungstyps wird geöffnet. Wählen Sie **Telemetrie**, und geben Sie die Details im Formular **Telemetrie erstellen** ein.

> [!NOTE]
> Wenn ein echtes Gerät verbunden ist, achten Sie auf den Namen der vom Gerät gemeldeten Messung. Der Name muss mit dem **Feldnamen** einer Messung genau übereinstimmen.

Beispielsweise können Sie eine neue Temperaturtelemetriemessung hinzufügen:

![Formular für Messungen](./media/howto-set-up-template/measurementsform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird die Messung **Temperatur** in der Liste der Messungen angezeigt, und der Operator sieht die Visualisierung der vom Gerät erfassten Temperaturdaten.

![Graph mit Messungen](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Erstellen einer Ereignismessung
Klicken Sie zum Hinzufügen einer neuen Ereignismessung auf die Schaltfläche **+ Neue Messung**. Ein Formular mit Optionen zur Auswahl des Messungstyps wird geöffnet. Wählen Sie **Ereignis**, und geben Sie die Details im Formular **Ereignis erstellen** ein.

Geben Sie in diesem Formular den **Anzeigenamen**, den **Feldnamen** und den **Schweregrad** des Ereignisses an. Ihnen stehen die drei verfügbare Schweregrade zur Auswahl: **Fehler**, **Warnung** und **Information**.  

Beispielsweise können Sie ein neues Ereignis „Lüftermotorfehler“ hinzufügen.

![Formular für Ereignismessungen](./media/howto-set-up-template/eventmeasurementsform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird die Messung **Lüftermotorfehler** in der Liste der Messungen angezeigt, und der Operator sieht die Visualisierung der vom Gerät gesendeten Ereignisdaten.

![Diagramm mit Ereignismessungen](./media/howto-set-up-template/eventmeasurementschart.png)

Klicken Sie im Diagramm auf das Ereignissymbol, um zusätzliche Ereignisdetails anzuzeigen:

![Ereignismessungsdetail](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Erstellen einer Zustandsmessung
Klicken Sie zum Hinzufügen einer neuen Zustandsmessung auf die Schaltfläche **+ Neue Messung**. Ein Formular mit Optionen zur Auswahl des Messungstyps wird geöffnet. Wählen Sie **Zustand**, und geben Sie die Details im Formular **Zustand erstellen** ein.

Geben Sie in diesem Formular den **Anzeigenamen**, den **Feldnamen** und die möglichen **Werte** des Zustands an. Jeder **Wert** kann auch einen Anzeigenamen aufweisen, der bei der Anzeige des Werts in Diagrammen und Tabellen verwendet wird.

Sie können z.B. einen neuen Zustand „Lüftermodus“ hinzufügen, der zwei mögliche Werte besitzt, die vom Gerät gesendet werden können: **In Betrieb** und **Angehalten**.

![Formular für Zustandsmessungen](./media/howto-set-up-template/statemeasurementsform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird die Zustandsmessung **Lüftermodus** in der Liste der Messungen angezeigt, und der Operator sieht die Visualisierung der vom Gerät gesendeten Zustandsdaten.

![Diagramm mit Zustandsmessungen](./media/howto-set-up-template/statemeasurementschart.png)

Sollten vom Gerät innerhalb kurzer Zeit zu viele Datenpunkte gesendet werden, wird die Zustandsmessung anders dargestellt, wie in der folgenden Abbildung gezeigt. Wenn Sie auf das Diagramm klicken, werden alle Datenpunkte innerhalb dieses Zeitraums in chronologischer Reihenfolge angezeigt. Sie können den Zeitbereich auch einschränken, um die Messung im Diagramm anzuzeigen.

![Zustandsmessungsdetail](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Einstellungen

Ein Gerät wird durch Einstellungen gesteuert. Diese ermöglichen den Operatoren Ihrer Anwendung die Angabe von Eingaben für das Gerät. Sie können Ihrer Gerätevorlage mehrere Einstellungen hinzufügen, die auf der Registerkarte **Einstellungen** als Kacheln angezeigt und von Operatoren verwendet werden können. Es gibt sechs Typen von Einstellungen, die Sie hinzufügen können: Zahl, Text, Datum, Ein-/Ausschalten, Auswahlliste und Abschnittsbezeichnung.

> [!NOTE]
> Wenn ein echtes Gerät verbunden ist, achten Sie auf den Namen der vom Gerät gemeldeten Einstellung. Der Name muss mit dem **Feldnamen** einer Einstellung genau übereinstimmen.

Einstellungen können einen von drei Zuständen aufweisen. Diese Zustände werden vom Gerät gemeldet.

- **Synchronisiert**: Das Gerät wurde geändert, um dem Einstellungswert zu entsprechen.

- **Ausstehend**: Das Gerät wird zurzeit auf den Einstellungswert geändert.

- **Fehler**: Das Gerät hat einen Fehler zurückgegeben.

Beispielsweise können Sie eine neue Einstellung für die Lüfterdrehzahl hinzufügen:

![Einstellungsformular](./media/howto-set-up-template/settingsform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird die Einstellung **Lüfterdrehzahl** als Kachel angezeigt und kann verwendet werden, um die Lüfterdrehzahl des Geräts zu ändern.

> [!NOTE]
> Sobald Sie eine neue Kachel erstellt haben, können Sie Ihre neue Einstellung ausprobieren. Deaktivieren Sie als Erstes den Entwurfsmodus oben rechts im Bildschirm:

![Kachel „Einstellungen“](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Eigenschaften

Eigenschaften sind die dem Gerät zugeordneten Gerätemetadaten, z.B. der Gerätestandort und die Seriennummer. Sie können Ihrer Gerätevorlage mehrere Eigenschaften hinzufügen, die als Kacheln auf der Registerkarte **Eigenschaften** angezeigt werden. Ein Operator kann die Werte für Eigenschaften beim Erstellen eines neuen Geräts angeben und sie anschließend jederzeit bearbeiten. Es gibt sechs Typen von Eigenschaften, die Sie hinzufügen können: Zahl, Text, Datum, Ein-/Ausschalten, Geräteeigenschaft und Bezeichnung.

Es gibt zwei Typen von Eigenschaften:

- **Geräteeigenschaften** sind Eigenschaften, die vom Gerät gemeldet werden.
- **Anwendungseigenschaften** sind Eigenschaften, die ausschließlich in der Anwendung gespeichert werden. Das Gerät hat von Anwendungseigenschaften keine Kenntnis.

> [!NOTE]
> Wenn ein echtes Gerät verbunden ist, achten Sie bei den Geräteeigenschaften auf den Namen der vom Gerät gemeldeten Eigenschaft. Der Name muss mit dem **Feldnamen** der Eigenschaft genau übereinstimmen. Bei den Anwendungseigenschaften kann der Feldname einen beliebigen Wert aufweisen, solange er in der Gerätevorlage eindeutig ist.

Beispielsweise können Sie den Gerätestandort als neue Eigenschaft hinzufügen:

![Formular für Eigenschaften](./media/howto-set-up-template/propertiesform.png)

Nachdem Sie auf **Speichern** geklickt haben, wird der Gerätestandort als Kachel angezeigt:

![Eigenschaftenkachel](./media/howto-set-up-template/propertiestile.png)

> [!NOTE]
> Nach dem Erstellen einer neuen Kachel können Sie den Wert der Eigenschaft ändern. Deaktivieren Sie als Erstes den Entwurfsmodus oben rechts im Bildschirm.

### <a name="create-a-location-property-powered-by-azure-maps"></a>Erstellen einer von Azure Maps unterstützten Standorteigenschaft
Sie können Ihre Standortdaten mit geografischem Kontext in Azure IoT Central versehen und Breiten- und Längengradkoordinaten einer Anschrift oder einfach die Breiten- und Längengradkoordinaten zuordnen. Diese Funktion in Azure IoT Central wird von Azure Maps unterstützt.

Es gibt zwei Arten von Standorteigenschaften, die Sie hinzufügen können:
- **Standort als Anwendungseigenschaft**, die ausschließlich in der Anwendung gespeichert wird. Das Gerät hat von Anwendungseigenschaften keine Kenntnis.
- **Standort als Geräteeigenschaft**, die vom Gerät gemeldet wird.

####<a name="adding-location-as-an-application-property"></a>Hinzufügen eines Standorts als Anwendungseigenschaft 
Sie können mithilfe von Azure Maps eine Standorteigenschaft als Anwendungseigenschaft in Ihrer Azure IoT Central-Anwendung erstellen. Beispielsweise können Sie die Geräteinstallationsadresse hinzufügen. 

1. Navigieren Sie zur Registerkarte „Geräteeigenschaft“, und stellen Sie sicher, dass der Entwurfsmodus aktiviert ist.

![Standorteigenschaft](./media/howto-set-up-template/locationcloudproperty1.png)

2. Klicken Sie auf der Registerkarte „Eigenschaft“ auf „Standort“.
3. Konfigurieren Sie optional den Anzeigenamen, Feldnamen und Anfangswert des Standorts. 

![Formular für die Standorteigenschaften](./media/howto-set-up-template/locationcloudproperty2.png)

Es gibt zwei unterstützte Formate zum Hinzufügen eines Standorts:
- **Standort als Adresse**
- **Standort als Koordinaten** 

4. Klicken Sie auf Speichern. 

![Feld für die Standorteigenschaft](./media/howto-set-up-template/locationcloudproperty3.png)

Ab sofort kann ein Operator den Standortwert im Formular für das Standortfeld aktualisieren. 

####<a name="adding-location-as-a-device-property"></a>Hinzufügen eines Standorts als Geräteeigenschaft 

Sie können eine Standorteigenschaft als Geräteeigenschaft erstellen, die vom Gerät gemeldet wird.
Nehmen wir beispielsweise an, Sie möchten den Gerätestandort nachverfolgen.

1.  Navigieren Sie zur Registerkarte „Geräteeigenschaft“, und stellen Sie sicher, dass der Entwurfsmodus aktiviert ist.
2.  Klicken Sie in der Bibliothek auf „Geräteeigenschaft“.

![Feld für die Standorteigenschaft](./media/howto-set-up-template/locationdeviceproperty1.png)

3.  Konfigurieren Sie den Anzeigenamen und den Feldnamen, und wählen Sie „location“ als Datentyp. 

> [!NOTE]
Der Feldname muss mit dem Namen der Eigenschaft der Geräteberichte genau übereinstimmen. 

![Feld für die Standorteigenschaft](./media/howto-set-up-template/locationdeviceproperty2.png)

![Ansicht des Operators für die Standorteigenschaft](./media/howto-set-up-template/locationdeviceproperty2.png)

Nachdem Sie nun Ihre Standorteigenschaft konfiguriert haben, können Sie eine Karte hinzufügen, um den Standort auf dem Gerätedashboard zu visualisieren. Weitere Informationen finden Sie unter [Hinzufügen eines Standorts auf dem Azure Maps-Dashboard](howto-set-up-template.md).




## <a name="rules"></a>Regeln

Anhand von Regeln können Operatoren Geräte nahezu in Echtzeit überwachen. Regeln rufen automatisch **Aktionen** auf. Beispielsweise kann bei Auslösen der Regel eine E-Mail gesendet werden. Zurzeit ist ein Regeltyp verfügbar:

- **Telemetrieregel**: Eine Telemetrieregel wird ausgelöst, wenn die ausgewählten Gerätetelemetriedaten einen angegebenen Schwellenwert überschreiten. Informationen zu [Telemetrieregeln](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

Im Dashboard findet ein Operator Informationen zu einem Gerät. Als Ersteller können Sie dieser Seite Kacheln hinzufügen, mit deren Hilfe Operatoren das Verhalten des Geräts nachvollziehen können. Sie können Ihrer Gerätevorlage mehrere Dashboardkacheln hinzufügen. Es gibt sechs Typen von Dashboardkacheln, die Sie hinzufügen können: Bild, Liniendiagramm, Balkendiagramm, KPI, Einstellungen und Eigenschaften sowie Bezeichnung.

Beispielsweise können Sie eine Kachel **Einstellungen und Eigenschaften** hinzufügen, um eine Auswahl der aktuellen Werte von Einstellungen und Eigenschaften anzuzeigen:

![Formular für Gerätedetails im Dashboard](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Wenn ein Operator nun das Dashboard aufruft, wird ihm diese Kachel mit den Eigenschaften und Einstellungen des Geräts angezeigt:

![Dashboardkachel](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-location-azure-map-in-dashboard"></a>Hinzufügen eines Standorts auf dem Azure Maps-Dashboard

Wenn Sie eine Standorteigenschaft konfiguriert haben, wie in den Schritten unter [Erstellen einer von Azure Maps unterstützten Standorteigenschaft]((howto-set-up-template.md) beschrieben wird, können Sie den Standort mithilfe einer Karte auf der rechten Seite auf Ihrem Gerätedashboard visualisieren.

1.  Navigieren Sie zur Registerkarte „Gerätedashboard“, und stellen Sie sicher, dass der Entwurfsmodus aktiviert ist.
2.  Wählen Sie auf der Registerkarte „Gerätedashboard“ die Option „Karte“ aus der Bibliothek aus. 

![Auswählen eines Standorts auf dem Azure Maps-Dashboard](./media/howto-set-up-template/locationcloudproperty4map.png)

3.  Geben Sie einen Titel ein, und wählen Sie die Standorteigenschaft aus, die Sie zuvor als Teil Ihrer Geräteeigenschaft konfiguriert haben.

![Konfigurieren eines Standorts auf dem Azure Maps-Dashboard](./media/howto-set-up-template/locationcloudproperty5map.png)

4.  Speichern Sie diese. Daraufhin wird die Kartenkachel mit dem Standort angezeigt, den Sie ausgewählt haben. 

![Visualisieren eines Standorts auf dem Azure Maps-Dashboard](./media/howto-set-up-template/locationcloudproperty6map.png) 

Sie können die Karte auf die gewünschte Größe ändern.

Wenn ein Operator nun das Dashboard anzeigt, kann dieser alle von Ihnen konfigurierte Dashboardkacheln einsehen, einschließlich einer Standortkarte.

![Azure Maps-Dashboard mit Standort](./media/howto-set-up-template/locationcloudproperty7map.png) 



## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung eingerichtet wird, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen einer neuen Gerätevorlagenversion](howto-version-devicetemplate.md)
