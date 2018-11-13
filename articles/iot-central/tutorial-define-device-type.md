---
title: Definieren eines neuen Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung einen neuen Gerätetyp definieren. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Einstellungen für den Typ.
author: tbhagwat3
ms.author: tanmayb
ms.date: 10/30/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: dd81c10bcda6665de7ffabe94a4c1be991687797
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963593"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung

In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Microsoft Azure IoT Central-Anwendung mithilfe einer Gerätevorlage eine neue Art von Gerät definieren. Eine Gerätevorlage definiert die Telemetriedaten, den Zustand, die Eigenschaften und die Einstellungen für Ihren Gerätetyp.

Damit Sie Ihre Anwendung testen können, bevor Sie eine Verbindung mit einem echten Gerät herstellen, generiert IoT Central auf der Grundlage der erstellten Vorlage ein simuliertes Gerät.

In diesem Tutorial erstellen Sie die Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage). Für eine verbundene Klimaanlage gilt Folgendes:

* Sie sendet Telemetriedaten wie Temperatur und Luftfeuchtigkeit.
* Sie meldet den Zustand (etwa, ob sie eingeschaltet ist).
* Sie verfügt über Geräteeigenschaften wie Firmwareversion und Seriennummer.
* Sie verfügt über Einstellungen wie etwa die Zieltemperatur.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Gerätevorlage
> * Hinzufügen von Telemetriedaten zu Ihrem Gerät
> * Anzeigen simulierter Telemetriedaten
> * Definieren der Ereignismessung
> * Anzeigen simulierter Ereignisse
> * Definieren der Zustandsmessung
> * Anzeigen des simulierten Zustands
> * Verwenden von Einstellungen und Eigenschaften
> * Verwenden von Befehlen
> * Anzeigen Ihres simulierten Geräts auf dem Dashboard

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine Azure IoT Central-Anwendung. Wenn Sie die Schritte der Schnellstartanleitung [Create an Azure IoT Central application](quick-deploy-iot-central.md) (Erstellen einer Azure IoT Central-Anwendung) ausgeführt haben, können Sie die so erstellte Anwendung verwenden. Führen Sie andernfalls die folgenden Schritte aus, um eine leere Azure IoT Central-Anwendung zu erstellen:

1. Navigieren Sie zur Azure IoT Central-Seite [Application Manager](https://aka.ms/iotcentral) (Anwendungs-Manager).

2. Geben Sie die E-Mail-Adresse und das Kennwort für den Zugriff auf Ihr Azure-Abonnement ein:

   ![Eingeben Ihres Organisationskontos](./media/tutorial-define-device-type/sign-in.png)

3. Klicken Sie auf **Neue Anwendung**, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen:

    ![Azure IoT Central-Seite „Application Manager“ (Anwendungs-Manager)](./media/tutorial-define-device-type/iotcentralhome.png)

4. So erstellen Sie eine neue Azure IoT Central-Anwendung:
    
    * Wählen Sie **Testversion** aus. Sie benötigen kein Azure-Abonnement zum Erstellen einer Testanwendung.
    
       Weitere Informationen zu Verzeichnissen und Abonnements finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).
    
    * Wählen Sie **Benutzerdefinierte Anwendung** aus.
    
    * Sie können optional auch einen Anzeigenamen für die Anwendung angeben (beispielsweise **Contoso Air Conditioners**). Azure IoT Central generiert automatisch ein eindeutiges URL-Präfix. Dieses URL-Präfix kann in einen einprägsameren Wert geändert werden.
    
    * Wählen Sie **Erstellen**.

    ![Azure IoT Central-Seite „Anwendung erstellen“](./media/tutorial-define-device-type/iotcentralcreate.png)

    Weitere Informationen finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

## <a name="create-a-new-custom-device-template"></a>Erstellen einer neuen benutzerdefinierten Gerätevorlage

Als Ersteller können Sie die Gerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Wenn Sie eine Gerätevorlage erstellen, generiert Azure IoT Central auf der Grundlage der Vorlage ein simuliertes Gerät. Das simulierte Gerät generiert Telemetriedaten, mit denen Sie das Verhalten Ihrer Anwendung testen können, bevor Sie eine Verbindung mit einem physischen Gerät herstellen.

Wenn Sie Ihrer Anwendung eine neue Gerätevorlage hinzufügen möchten, navigieren Sie zur Seite **Application Builder** (Anwendungs-Generator). Klicken Sie hierzu im linken Navigationsmenü auf **Application Builder** (Anwendungs-Generator).

![Seite „Application Builder“ (Anwendungs-Generator)](./media/tutorial-define-device-type/builderhome.png)

## <a name="add-a-device-and-define-telemetry"></a>Hinzufügen eines Geräts und Definieren von Telemetriedaten

In diesem Abschnitt erfahren Sie Schritt für Schritt, wie Sie für Geräte, die temperaturbezogene Telemetriedaten an Ihre Anwendung senden, eine neue Gerätevorlage namens **Connected Air Conditioner** erstellen:

1. Klicken Sie auf der Seite **Application Builder** (Anwendungs-Generator) auf **Create Device Template** (Gerätevorlage erstellen):

    ![Seite „Application Builder“ (Anwendungs-Generator), Option „Create Device Template“ (Gerätevorlage erstellen)](./media/tutorial-define-device-type/builderhomedevices.png)

2. Klicken Sie auf der Seite **Device Templates** (Gerätevorlagen) auf **Benutzerdefiniert**. Mit einer Gerätevorlage vom Typ **Benutzerdefiniert** können Sie sämtliche Eigenschaften und Verhaltensweisen Ihrer verbundenen Klimaanlage definieren:

    ![Geräte](./media/tutorial-define-device-type/builderhomedevicescustom.png)

3. Geben Sie auf der Seite **New Device Template** (Neue Gerätevorlage) den Gerätenamen **Connected Air Conditioner** ein, und klicken Sie anschließend auf **Erstellen**. Sie können auch ein Bild Ihres Geräts hochladen, das Bedienern im Device Explorer angezeigt wird:

    ![Benutzerdefiniertes Gerät](./media/tutorial-define-device-type/createcustomdevice.png)

4. Vergewissern Sie sich in der Gerätevorlage **Connected Air Conditioner**, dass Sie sich auf der Seite **Measurements** (Messungen) befinden, um die Telemetriedaten zu definieren. Jede Gerätevorlage, die Sie definieren, bietet individuelle Seiten für Folgendes:

    * Angeben der Messungen, die vom Gerät gesendet werden (etwa Telemetriedaten, Ereignisse und Zustände)
    
    * Definieren der Einstellungen zum Steuern des Geräts
    
    * Definieren der Eigenschaften, die die Metadaten des Geräts bilden

    * Definieren der Befehle, die direkt auf dem Gerät ausgeführt werden sollen
    
    * Definieren der Regeln für das Gerät
    
    * Anpassen des Gerätedashboards für die Bediener

    Klicken Sie zum Definieren der Gerätevorlage immer zuerst auf **Vorlage bearbeiten**, um die Vorlage zu bearbeiten. Klicken Sie auf **Fertig**, wenn Sie fertig sind. 

    ![Messungen für die Klimaanlage](./media/tutorial-define-device-type/airconmeasurements.png)

    > [!NOTE]
    > Wenn Sie den Namen des Geräts oder der Gerätevorlage ändern möchten, klicken Sie auf den Text im oberen Seitenbereich.

5. Klicken Sie auf **New Measurement** (Neue Messung), um die Messung für die Temperaturtelemetrie hinzuzufügen. Wählen Sie **Telemetrie** als Messungstyp aus:

    ![Messungen für die verbundene Klimaanlage](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Jede Art von Telemetrie, die Sie für eine Gerätevorlage definieren, beinhaltet [Konfigurationsoptionen](howto-set-up-template.md) wie etwa:

    * Anzeigeoptionen

    * Telemetriedetails

    * Simulationsparameter

    Verwenden Sie zum Konfigurieren der Telemetrie **Temperature** (Temperatur) die Informationen aus der folgenden Tabelle:

    | Einstellung              | Wert         |
    | -------------------- | -----------   |
    | Anzeigename         | Temperatur   |
    | Feldname           | Temperatur   |
    | Units                | F             |
    | Min                  | 60            |
    | max                  | 110           |
    | Dezimalstellen       | 0             |

    Sie können auch eine Farbe für die Telemetrieanzeige auswählen. Klicken Sie auf **Speichern**, um die Telemetriedefinition zu speichern:

    ![Konfigurieren der Temperatursimulation](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Nach kurzer Zeit wird auf der Seite **Measurements** (Messungen) ein Diagramm der Temperaturtelemetriedaten Ihrer simulierten verbundenen Klimaanlage angezeigt. Mithilfe der Steuerelemente können Sie die Sichtbarkeit sowie die Aggregation verwalten und die Telemetriedefinition bearbeiten:

    ![Anzeigen der Temperatursimulation](./media/tutorial-define-device-type/viewsimulation.png)

8. Darüber hinaus können Sie das Diagramm mithilfe der Steuerelemente **Linie**, **Gestapelt** und **Zeitbereich bearbeiten** anpassen:

    ![Anpassen des Diagramms](./media/tutorial-define-device-type/customizechart.png)

## <a name="define-event-measurement"></a>Definieren der Ereignismessung

Mithilfe der Ereignisoption können Sie Zeitpunktdaten definieren, die vom Gerät gesendet werden, um ein bedeutsames Ereignis wie etwa einen Fehler oder den Ausfall einer Komponente anzugeben. Geräteereignisse können ähnlich wie Telemetriemessungen von Azure IoT Central simuliert werden, sodass Sie das Verhalten Ihrer Anwendung testen können, bevor Sie eine Verbindung mit einem physischen Gerät herstellen. Ereignismessungen für den Gerätetyp werden in der Ansicht **Measurements** (Messungen) definiert.

1. Klicken Sie auf **New Measurement** (Neue Messung), um die Ereignismessung **Fan Motor Error** (Lüftermotorfehler) hinzuzufügen. Wählen Sie dann **Ereignis** als Messungstyp aus:

    ![Messungen für die verbundene Klimaanlage](./media/tutorial-define-device-type/eventnew.png)

2. Jede Art von Ereignis, das Sie für eine Gerätevorlage definieren, beinhaltet [Konfigurationsoptionen](howto-set-up-template.md) wie etwa:

   * Anzeigename

   * Feldname

   * Schweregrad

    Verwenden Sie zum Konfigurieren des Ereignisses **Fan Motor Error** die Informationen aus der folgenden Tabelle:

    | Einstellung              | Wert             |
    | -------------------- | -----------       |
    | Anzeigename         | Fan Motor Error   |
    | Feldname           | fanmotorerr       |
    | Severity             | Error             |

    Klicken Sie auf **Speichern**, um die Ereignisdefinition zu speichern:

    ![Konfigurieren der Ereignismessung](./media/tutorial-define-device-type/eventconfiguration.png)

3. Nach kurzer Zeit wird auf der Seite **Measurements** (Messungen) ein Diagramm der Ereignisse angezeigt, die nach dem Zufallsprinzip auf der Grundlage Ihrer simulierten verbundenen Klimaanlage generiert wurden. Mithilfe der Steuerelemente können Sie die Sichtbarkeit verwalten und die Ereignisdefinition bearbeiten:

    ![Anzeigen der Ereignissimulation](./media/tutorial-define-device-type/eventview.png)

1. Klicken Sie im Diagramm auf das Ereignis, um zusätzliche Ereignisdetails anzuzeigen:

    ![Anzeigen von Ereignisdetails](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-state-measurement"></a>Definieren der Zustandsmessung

Mithilfe der Zustandsoption können Sie den Zustand des Geräts oder der dazugehörigen Komponenten über einen Zeitraum definieren und visualisieren. Der Gerätezustand kann ähnlich wie Telemetriemessungen von Azure IoT Central simuliert werden, sodass Sie das Verhalten Ihrer Anwendung testen können, bevor Sie eine Verbindung mit einem physischen Gerät herstellen. Zustandsmessungen für den Gerätetyp werden in der Ansicht **Measurements** (Messungen) definiert.

1. Klicken Sie auf **New Measurement** (Neue Messung), um die Messung **Fan Mode** (Lüftermodus) hinzuzufügen. Wählen Sie dann **Zustand** als Messungstyp aus:

    ![Zustandsmessungen für die verbundene Klimaanlage](./media/tutorial-define-device-type/statenew.png)

2. Jede Art von Zustand, den Sie für eine Gerätevorlage definieren, beinhaltet [Konfigurationsoptionen](howto-set-up-template.md) wie etwa:

   * Anzeigename

   * Feldname

   * Werte mit optionalen Anzeigebeschriftungen

   * Wertspezifische Farben

    Verwenden Sie zum Konfigurieren des Zustands **Fan Mode** (Lüftermodus) die Informationen aus der folgenden Tabelle:

    | Einstellung              | Wert             |
    | -------------------- | -----------       |
    | Anzeigename         | Fan Mode          |
    | Feldname           | fanmode           |
    | Wert                | 1                 |
    | Anzeigebeschriftung        | Operating         |
    | Wert                | 0                 |
    | Anzeigebeschriftung        | Beendet           |

    Klicken Sie auf **Speichern**, um die Definition der Zustandsmessung zu speichern:

    ![Konfigurieren der Zustandsmessung](./media/tutorial-define-device-type/stateconfiguration.png)

3. Nach kurzer Zeit wird auf der Seite **Measurements** (Messungen) ein Diagramm der Zustände angezeigt, die nach dem Zufallsprinzip auf der Grundlage Ihrer simulierten verbundenen Klimaanlage generiert wurden. Mithilfe der Steuerelemente können Sie die Sichtbarkeit verwalten und die Zustandsdefinition bearbeiten:

    ![Anzeigen der Zustandssimulation](./media/tutorial-define-device-type/stateview.png)

4. Sollten vom Gerät innerhalb kurzer Zeit zu viele Datenpunkte gesendet werden, wird die Zustandsmessung anders dargestellt, wie in der folgenden Abbildung zu sehen. Wenn Sie auf das Diagramm klicken, werden alle Datenpunkte innerhalb dieses Zeitraums in chronologischer Reihenfolge angezeigt. Sie können den Zeitbereich auch einschränken, um detailliertere Messungen anzuzeigen.

    ![Anzeigen von Zustandsdetails](./media/tutorial-define-device-type/stateviewdetail.png)

## <a name="settings-properties-and-commands"></a>Einstellungen, Eigenschaften und Befehle

Einstellungen Eigenschaften und Befehle sind unterschiedliche Werte, die in einer Gerätevorlage definiert und jedem einzelnen Gerät zugeordnet werden:

* _Einstellungen_ dienen dazu, Konfigurationsdaten aus Ihrer Anwendung an ein Gerät zu senden. Mithilfe einer Einstellung kann ein Bediener beispielsweise das Telemetrieintervall des Geräts von zwei Sekunden in fünf Sekunden ändern. Wenn ein Bediener eine Einstellung ändert, wird diese auf der Benutzeroberfläche als ausstehend markiert, bis das Gerät bestätigt, dass die Einstellungsänderung durchgeführt wurde.

* _Eigenschaften_ dienen zum Definieren von Metadaten, die mit Ihrem Gerät verknüpft sind. Es gibt zwei Kategorien von Eigenschaften:
    
    * _Anwendungseigenschaften_ dienen zum Erfassen von Geräteinformationen in Ihrer Anwendung. Mithilfe von Anwendungseigenschaften können Sie beispielsweise den Standort eines Geräts und das Datum der letzten Wartung erfassen. Diese Eigenschaften werden in der Anwendung gespeichert und nicht mit dem Gerät synchronisiert. Ein Bediener kann Eigenschaften Werte zuweisen.

    * _Geräteeigenschaften_ dienen dazu, einem Gerät das Senden von Eigenschaftswerten an Ihre Anwendung zu ermöglichen. Diese Eigenschaften können nur durch das Gerät geändert werden. Für Bediener sind Geräteeigenschaften schreibgeschützt. In diesem Szenario für eine verbundene Klimaanlage werden vom Gerät die Geräteeigenschaften „Firmwareversion“ und „Seriennummer des Geräts“ gemeldet. 
    
    Weitere Informationen finden Sie in der Schrittanleitung zum Einrichten einer Gerätevorlage unter [Eigenschaften][lnk-define-template].

* Verwenden Sie _Befehle_, um Ihr Gerät aus Ihrer Anwendung remote zu verwalten. Sie können Befehle auf Ihrem Gerät direkt aus der Cloud ausführen, um die Geräte zu steuern. Operatoren können z.B. Befehle wie den Neustart ausführen, um das Gerät sofort neu zu starten.

## <a name="use-settings"></a>Verwenden von Einstellungen

*Einstellungen* ermöglichen es einem Bediener, Konfigurationsdaten an ein Gerät zu senden. In diesem Abschnitt fügen Sie der Gerätevorlage **Connected Air Conditioner** eine Einstellung hinzu, die es Bedienern ermöglicht, die Zieltemperatur der verbundenen Klimaanlage festzulegen.

1. Navigieren Sie zur Seite **Einstellungen** für die Gerätevorlage **Connected Air Conditioner**:

    ![Vorbereiten des Hinzufügens einer Einstellung](./media/tutorial-define-device-type/deviceaddsetting.png)

    Sie können Einstellungen unterschiedlicher Art erstellen – beispielsweise Zahlen oder Text.

2. Klicken Sie auf **Zahl**, um Ihrem Gerät eine Zahleneinstellung hinzuzufügen.

3. Verwenden Sie zum Konfigurieren der Einstellung **Set Temperature** (Sollwerttemperatur) die Informationen aus der folgenden Tabelle:

    | Feld                | Wert           |
    | -------------------- | -----------     |
    | Anzeigename         | Set Temperature |
    | Feldname           | setTemperature  |
    | Berechnungseinheit      | F               |
    | Dezimalstellen       | 1               |
    | Mindestwert        | 20              |
    | Maximalwert        | 200             |
    | Anfangswert        | 80              |
    | BESCHREIBUNG          | Set the target temperature for the air conditioner |

    Klicken Sie anschließend auf **Speichern**:

    ![Konfigurieren der Einstellung für die Sollwerttemperatur](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Wenn das Gerät eine Einstellungsänderung bestätigt, ändert sich der Status der Einstellungsänderung in **Synchronisiert**.

4. Sie können die Einstellungskacheln verschieben und ihre Größe ändern, um das Layout der Seite **Einstellungen** anzupassen:

    ![Anpassen des Layouts der Einstellungen](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties"></a>Verwenden von Eigenschaften 

*Anwendungseigenschaften* dienen dazu, Geräteinformationen in der Anwendung zu speichern. In diesem Abschnitt fügen Sie der Gerätevorlage **Connected Air Conditioner** Anwendungseigenschaften hinzu, um den Standort des Geräts und das Datum der letzten Wartung zu speichern. Beachten Sie, dass diese beiden Eigenschaften des Geräts bearbeitet werden können. Es gibt auch schreibgeschützte Eigenschaften des Geräts, die nicht geändert werden können – etwa die Seriennummer des Geräts und die Firmwareversion.
 
1. Navigieren Sie zur Seite **Eigenschaften** für die Gerätevorlage **Connected Air Conditioner**:

    ![Vorbereiten des Hinzufügens einer Eigenschaft](./media/tutorial-define-device-type/deviceaddproperty.png)

    Sie können Geräteeigenschaften unterschiedlicher Art erstellen, wie Zahlen oder Text. Klicken Sie auf **Standort**, um Ihrer Gerätevorlage eine Standorteigenschaften hinzuzufügen.

1. Verwenden Sie zum Konfigurieren der Standorteigenschaft die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                |
    | -------------------- | -------------------- |
    | Anzeigename         | Standort             |
    | Feldname           | location             |
    | Anfangswert        | Seattle, WA          |
    | BESCHREIBUNG          | Gerätestandort      |

    Behalten Sie für die anderen Felder die Standardwerte bei.

    ![Konfigurieren der Geräteeigenschaften](./media/tutorial-define-device-type/configureproperties.png)

    Wählen Sie **Speichern** aus.

1. Wählen Sie auf **Datum** aus, um Ihrer Gerätevorlage eine Eigenschaft für das Datum der letzten Wartung hinzuzufügen.

1. Verwenden Sie zum Konfigurieren der Eigenschaft für das Datum der letzten Wartung die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                   |
    | -------------------- | ----------------------- |
    | Anzeigename         | Datum der letzten Wartung       |
    | Feldname           | serviceDate             |
    | Anfangswert        | 1/1/2018                |
    | BESCHREIBUNG          | Datum der letzten Wartung           |

    ![Konfigurieren der Geräteeigenschaften](./media/tutorial-define-device-type/configureproperties2.png)

    Wählen Sie **Speichern** aus.

5. Sie können das Layout der Seite **Properties** (Eigenschaften) anpassen, indem Sie die Eigenschaftenkacheln verschieben und ihre Größe ändern:

    ![Anpassen des Layouts der Eigenschaften](./media/tutorial-define-device-type/propertieslayout.png)

1. Klicken Sie auf **Geräteeigenschaft**, um Ihrer Gerätevorlage eine Geräteeigenschaft wie etwa die Firmwareversion hinzuzufügen.

1.  Verwenden Sie zum Konfigurieren der Firmwareversion die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                   |
    | -------------------- | ----------------------- |
    | Anzeigename         | Firmware version        |
    | Feldname           | firmwareVersion         |
    | Datentyp            | text                    |
    | BESCHREIBUNG          | Die Firmwareversion der Klimaanlage |

    ![Konfigurieren der Firmwareversion](./media/tutorial-define-device-type/configureproperties3.png)
    
    Wählen Sie **Speichern** aus.

1. Klicken Sie auf **Geräteeigenschaft**, um Ihrer Gerätevorlage eine Geräteeigenschaft wie etwa eine Seriennummer hinzuzufügen.

1. Verwenden Sie zum Konfigurieren der Seriennummer die Informationen aus der folgenden Tabelle:

    | Feld                | Wert                   |
    | -------------------- | ----------------------- |
    | Anzeigename         | Serial number           |
    | Feldname           | serialNumber            |
    | Datentyp            | text                    |
    | BESCHREIBUNG          | Die Seriennummer der Klimaanlage  |

    ![Konfigurieren der Seriennummer](./media/tutorial-define-device-type/configureproperties4.png)
    
    Wählen Sie **Speichern** aus.
    
    > [!NOTE]
    > Die Geräteeigenschaft wird vom Gerät an die Anwendung gesendet. Die Werte der Firmwareversion und der Seriennummer werden aktualisiert, wenn Ihr echtes Gerät eine Verbindung mit IoT Central herstellt.

## <a name="use-commands"></a>Verwenden von Befehlen

Verwenden Sie _Befehle_, damit ein Operator Befehle direkt auf dem Gerät ausführen kann. In diesem Abschnitt fügen Sie Ihrer Gerätevorlage **Connected Air Conditioner** einen Befehl hinzu, der es einem Bediener ermöglicht, eine bestimmte Nachricht an der verbundenen Klimaanlage auszugegeben.

1. Navigieren Sie zur Seite **Befehle** Ihrer Gerätevorlage **Connected Air Conditioner**, um die Vorlage zu bearbeiten. 

1. Klicken Sie auf **Neuer Befehl**, um Ihrem Gerät einen Befehl hinzuzufügen, und konfigurieren Sie Ihren neuen Befehl.

   Sie können je nach Anforderung Befehle unterschiedlichen Typs erstellen. 

1. Um Ihren neuen Befehl zu konfigurieren, verwenden Sie die Informationen aus der folgenden Tabelle:

    | Feld                | Wert           |
    | -------------------- | -----------     |
    | Anzeigename         | Echo-Befehl    |
    | Feldname           | Echo            |
    | Standardzeitlimit      | 30              |
    | Typ anzeigen         | text            |
    | BESCHREIBUNG          | Gerätebefehl  |  

    Sie können zusätzliche Eingaben für den Befehl hinzufügen, indem Sie unter **Eingabefelder** auf **+** klicken.

    ![Vorbereiten des Hinzufügens einer Einstellung](media/tutorial-define-device-type/commandsecho1.png)

     Wählen Sie **Speichern** aus.

1. Sie können das Layout der Seite **Commands** (Befehle) anpassen, indem Sie die Einstellungskacheln verschieben und ihre Größe ändern:

    ![Anpassen des Layouts der Einstellungen](media/tutorial-define-device-type/commandstileresize1.png)

## <a name="view-your-simulated-device"></a>Anzeigen Ihres simulierten Geräts

Nachdem Sie Ihre Gerätevorlage **Connected Air Conditioner** definiert haben, können Sie das dazugehörige **Dashboard** anpassen, um ihm die definierten Messungen, Einstellungen und Eigenschaften hinzuzufügen. Anschließend können Sie eine Dashboardvorschau als Bediener anzeigen:

1. Navigieren Sie zur Seite **Dashboard** für die Gerätevorlage **Connected Air Conditioner**:

    ![Dashboards für die verbundene Klimaanlage](./media/tutorial-define-device-type/aircondashboards.png)

1. Klicken Sie auf **Liniendiagramm**, um die Komponente dem **Dashboard** hinzuzufügen:

    ![Dashboardkomponenten](./media/tutorial-define-device-type/dashboardcomponents1.png)

1. Konfigurieren Sie die Komponente **Liniendiagramm** mit den Informationen aus der folgenden Tabelle:

    | Einstellung      | Wert       |
    | ------------ | ----------- |
    | Titel        | Temperatur |
    | Zeitbereich   | Letzte 30 Minuten |
    | Measures     | temperature (Klicken Sie neben **temperature** auf **Sichtbarkeit**.) |

    ![Einstellungen für Liniendiagramm](./media/tutorial-define-device-type/linechartsettings.png)

    Klicken Sie auf **Speichern**.

1. Konfigurieren Sie die Komponente **Ereignisverlauf** mit den Informationen aus der folgenden Tabelle:

    | Einstellung      | Wert       |
    | ------------ | ----------- |
    | Titel        | Ereignisse |
    | Zeitbereich   | Letzte 30 Minuten |
    | Measures     | Fan Motor Error (Klicken Sie neben **Fan Motor Error** auf **Sichtbarkeit**.) |

    ![Einstellungen für Liniendiagramm](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Klicken Sie auf **Speichern**.

1. Konfigurieren Sie die Komponente **Statusverlauf** mit den Informationen aus der folgenden Tabelle:

    | Einstellung      | Wert       |
    | ------------ | ----------- |
    | Titel        | Fan Mode |
    | Zeitbereich   | Letzte 30 Minuten |
    | Measures | Fan Mode (Klicken Sie neben **Fan Mode** auf **Sichtbarkeit**.) |

    ![Einstellungen für Liniendiagramm](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Klicken Sie auf **Speichern**.

1. Klicken Sie auf **Settings and Properties** (Einstellungen und Eigenschaften), um dem Dashboard die Einstellung für die Sollwerttemperatur hinzuzufügen. Klicken Sie auf **Hinzufügen/Entfernen**, um dem Dashboard die gewünschten Einstellungen oder Eigenschaften hinzuzufügen. 

    ![Dashboardkomponenten](./media/tutorial-define-device-type/dashboardcomponents4.png)

1. Konfigurieren Sie die Komponente **Settings and Properties** (Einstellungen und Eigenschaften) mit den Informationen aus der folgenden Tabelle:

    | Einstellung                 | Wert         |
    | ----------------------- | ------------- |
    | Titel                   | Set target temperature |
    | Settings and Properties (Einstellungen und Eigenschaften) | Set Temperature |

    Einstellungen und Eigenschaften, die Sie zuvor auf den entsprechenden Seiten definiert haben, werden unter den verfügbaren Spalten angezeigt. 

    ![Eigenschaftseinstellungen für „Set Temperature“](./media/tutorial-define-device-type/propertysettings4.png)

    Klicken Sie anschließend auf **OK**.

1. Klicken Sie auf **Settings and Properties** (Einstellungen und Eigenschaften), um dem Dashboard die Seriennummer des Geräts hinzuzufügen:

    ![Dashboardkomponenten](./media/tutorial-define-device-type/dashboardcomponents3.png)

1. Konfigurieren Sie die Komponente **Settings and Properties** (Einstellungen und Eigenschaften) mit den Informationen aus der folgenden Tabelle:

    | Einstellung                 | Wert         |
    | ----------------------- | ------------- |
    | Titel                   | Serial number |
    | Settings and Properties (Einstellungen und Eigenschaften) | Serial number |

    ![Einstellungen für die Seriennummerneigenschaft](./media/tutorial-define-device-type/propertysettings5.png)

    Klicken Sie anschließend auf **OK**.

1. Klicken Sie auf **Settings and Properties** (Einstellungen und Eigenschaften), um dem Dashboard die Firmwareversion des Geräts hinzuzufügen:

    ![Dashboardkomponenten](./media/tutorial-define-device-type/dashboardcomponents4.png)

1. Konfigurieren Sie die Komponente **Settings and Properties** (Einstellungen und Eigenschaften) mit den Informationen aus der folgenden Tabelle:

    | Einstellung                 | Wert            |
    | ----------------------- | ---------------- |
    | Titel                   | Firmware version |
    | Settings and Properties (Einstellungen und Eigenschaften) | Firmware version |

    ![Einstellungen für die Seriennummerneigenschaft](./media/tutorial-define-device-type/propertysettings6.png)

    Klicken Sie anschließend auf **OK**.

1. Deaktivieren Sie rechts oben auf der Seite die Option **Vorlage bearbeiten**, um das Dashboard als Bediener anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Erstellen einer neuen Gerätevorlage
> * Hinzufügen von Telemetriedaten zu Ihrem Gerät
> * Anzeigen simulierter Telemetriedaten
> * Definieren von Geräteereignissen
> * Anzeigen simulierter Ereignisse
> * Definieren des Zustands
> * Anzeigen des simulierten Zustands
> * Verwenden von Einstellungen und Eigenschaften
> * Verwenden von Befehlen
> * Anzeigen Ihres simulierten Geräts auf dem Dashboard

Nachdem Sie nun eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung definiert haben, können Sie mit folgenden Schritten fortfahren:

* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules.md)
* [Anpassen der Azure IoT Central-Ansicht für Bediener](tutorial-customize-operator.md)

[lnk-define-template]: /azure/iot-central/howto-set-up-template#properties