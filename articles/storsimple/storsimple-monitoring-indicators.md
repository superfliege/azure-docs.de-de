---
title: "StorSimple-Überwachungsindikatoren | Microsoft Docs"
description: "Beschreibt die LEDs (LEDs) und akustische Alarme, die zum Überwachen des Status des StorSimple-Geräts verwendet."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 59dee7b9-ca6d-4fd9-96e6-a0071e8d248e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 0d44f94719bff0cb58b3727050598e55f4774e66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Verwenden Sie StorSimple-Überwachungsindikatoren, um Ihr Gerät verwalten
## <a name="overview"></a>Übersicht
Ihr StorSimple-Gerät umfasst LEDs (LEDs) und Alarme, die Sie zum Überwachen der Module und der Gesamtstatus des StorSimple-Geräts verwenden können. Die Überwachungsindikatoren können in den Hardwarekomponenten des Geräts primäres Gehäuse und ebod-Gehäuse gefunden werden. Die Überwachungsindikatoren können LEDs oder akustische Alarme sein.

Es gibt drei LED-Statusanzeigen verwendet, um den Status eines Moduls anzugeben: Grün, Grün bis Rot-bernsteinfarben blinkend oder rot-bernsteinfarben.  

* Grüne LEDs stellen einen fehlerfreien Betrieb Status dar.  
* Blinken das Vorhandensein der unkritische Bedingungen, die möglicherweise Eingreifen des Benutzers, für Grün bis Rot-Bernsteinfarbene LEDs darstellen.  
* Rot-Bernsteinfarbene LEDs zeigen an, dass ein Kritischer Fehler innerhalb des Moduls vorhanden ist.  

Der Rest dieses Artikels werden die verschiedenen Überwachungsindikator-LEDs, ihre Speicherorte auf dem StorSimple-Gerät, den Gerätestatus basierend auf den LED-Statusangaben und alle zugehörigen akustische Alarme beschrieben.

## <a name="front-panel-indicator-leds"></a>Indikator-LEDs des vorderen bedienungsfelds
Im Vordergrund, auch bekannt als Bereich der *bedienungsfelds* oder *Ops-Bedienungsfeld*, Statusangaben zu allen Modulen im System angezeigt. Das vordere Bedienungsfeld auf der primären StorSimple und das ebod-Gehäuse identisch ist, und ist unten dargestellt.  

   ![Vorderseite des Geräts][1]

Das vordere Bedienfeld enthält die folgenden Indikatoren:  

1. Stummtaste
2. Betriebsanzeige-LED (Grün/Rot-bernsteinfarben)
3. Modulfehleranzeige-LED (Rot-bernsteinfarben/aus)
4. Logische Fehleranzeige-LED (Rot-bernsteinfarben/aus
5. Anzeige der Einheit-ID  

Der Hauptunterschied zwischen der vorderen Bedienfelds für das Gerät als auch für das ebod-Gehäuse ist die **-Identifikationsnummer** auf den LED-Anzeige dargestellt. Die Standardeinheit für ID, die auf dem Gerät angezeigt wird **00**, während die auf dem ebod-Gehäuse angezeigte Standard-Einheit-ID ist **01**. Dadurch können Sie schnell zwischen dem Gerät und das ebod-Gehäuse unterscheiden, wenn das Gerät eingeschaltet ist. Wenn Ihr Gerät deaktiviert ist, verwenden Sie die Informationen im [Einschalten eines neuen Geräts](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) , das Gerät aus dem ebod-Gehäuse zu unterscheiden.  

## <a name="front-panel-led-status"></a>Vorderseite LED-status
Verwenden Sie in der folgenden Tabelle, um den Status durch die LEDs auf dem vorderen Bedienfeld für das Gerät oder das ebod-Gehäuse zu identifizieren.  

| Systemstromversorgung | Modulfehler | Logischer Fehler | Alarm | Status |
| --- | --- | --- | --- | --- |
| Rot-bernsteinfarben |AUSSCHALTEN |AUSSCHALTEN |N/V |Stromausfall, Betrieb auf Stromversorgung oder die Stromversorgung ist eingeschaltet und der Controller, Module entfernt wurden. |
| Grün |EIN |EIN |N/V |Teststatus für eingeschaltete Stromversorgung des OPS Bedienfelds (5) |
| Grün |AUSSCHALTEN |AUSSCHALTEN |N/V |Eingeschaltet, alle Funktionen einwandfrei |
| Grün |EIN |N/V |Fehler-LEDs, Lüfterfehler-LEDs für PCM |Jeder PCM-Fehler, Lüfterfehler, oberhalb oder unterhalb Temperatur |
| Grün |EIN |N/V |E/a-Modul-LEDs |Jeder controllermodulfehler |
| Grün |EIN |N/V |N/V |Gehäuselogikfehler |
| Grün |Flash |N/V |Modulstatus-LED am Controllermodul. Fehler-LEDs, Lüfterfehler-LEDs für PCM |Unbekannter controllermodultyp installiert, I2C-Bus-Fehler, Modulfehler mit Controller wichtiger Product Data (VPD) |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Indikator stromversorgungs-und Kühleinheit (PCM) LEDs
Stromversorgung Kühlung (PCM)-Modul-Indikator-LEDs des kann auf der Rückseite des primären Gehäuses oder ebod-Gehäuse für jede PCM-Modul gefunden werden. In diesem Thema wird erläutert, wie die folgenden LEDs zur Überwachung des Status Ihres StorSimple-Geräts verwendet werden.  

* PCM-LEDs für das primäre Gehäuse
* PCM-LEDs für ebod-Gehäuse

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM-LEDs für das primäre Gehäuse
Das StorSimple-Gerät verfügt über eine 764w PCM mit zusätzlichem Akku. Die folgende Abbildung zeigt das LED-Bedienfeld für das Gerät.  

   ![PCM-LEDs auf dem primären Gehäuse][2]

LED-Legende:

1. Stromausfall
2. Lüfterfehler
3. Akkufehler
4. PCM OK
5. Gleichstromfehler
6. Akku OK  

Der Status der PCM wird auf den LED-Bedienfeld angezeigt. Die Geräte-PCM-LED-Bedienfeld verfügt über sechs LEDs. Vier dieser LEDs zeigen den Status der Stromversorgung und den Lüfter. Die beiden anderen LEDs zeigen den Status des pufferbatteriemoduls im PCM. Sie können in den folgenden Tabellen verwenden, um zu bestimmen, den Status der PCM.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM-Indikator-LEDs für Stromversorgung und Lüfter
| Status | PCM OK (Grün) | Stromausfall (bernsteinfarben) | Lüfterfehler (bernsteinfarben) | Gleichstromfehler (bernsteinfarben) |
| --- | --- | --- | --- | --- |
| Keine Stromversorgung (am Gehäuse) |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |
| Keine Stromversorgung (nur diese PCM) |AUSSCHALTEN |EIN |AUSSCHALTEN |EIN |
| Stromversorgung verfügbar, PCM - OK |EIN |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |
| PCM-Fehler (Lüfterfehler) |AUSSCHALTEN |AUSSCHALTEN |EIN |N/V |
| PCM-Fehler (hohe Spannung oder Stromstärke) |AUSSCHALTEN |EIN |EIN |EIN |
| PCM (Lüfter außerhalb des Toleranzbereichs) |EIN |AUSSCHALTEN |AUSSCHALTEN |EIN |
| Standby-Modus |Blinken |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |
| PCM-firmwaredownload |AUSSCHALTEN |Blinken |Blinken |Blinken |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM-Indikator-LEDs für Sicherungsakku
| Status | Akku OK (Grün) | Akkufehler (bernsteinfarben) |
| --- | --- | --- |
| Akku nicht vorhanden |AUSSCHALTEN |AUSSCHALTEN |
| Akku vorhanden und aufgeladen |EIN |AUSSCHALTEN |
| Akku aufgeladen oder zur Wartung entladen |Blinken |AUSSCHALTEN |
| "Soft" Akkufehler (wiederherstellbar) |AUSSCHALTEN |Blinken |
| "Harten" Akkufehler (nicht wiederherstellbar) |AUSSCHALTEN |EIN |
| Akku deaktiviert |Blinken |AUSSCHALTEN |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM-LEDs für ebod-Gehäuse
Das ebod-Gehäuse verfügt über eine 580W PCM und keinen zusätzlichen Akku. Das PCM-Bedienfeld für das ebod-Gehäuse hat Indikator-LEDs für nur für die Stromversorgung und den Lüfter. Die folgende Abbildung zeigt diese LEDs.

   ![PCM-LEDs am ebod-Gehäuse][3] 

In der folgenden Tabelle können Sie um den Status der PCM zu bestimmen.  

| Status | PCM OK (Grün) | Stromausfall (bernsteinfarben) | Lüfterfehler (bernsteinfarben) | Gleichstromfehler (bernsteinfarben) |
| --- | --- | --- | --- | --- |
| Keine Stromversorgung (am Gehäuse) |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |
| Keine Stromversorgung (nur diese PCM) |AUSSCHALTEN |EIN |AUSSCHALTEN |EIN |
| Stromversorgung verfügbar, PCM – OK |EIN |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |
| PCM-Fehler (Lüfterfehler) |AUSSCHALTEN |AUSSCHALTEN |EIN |X |
| PCM-Fehler (hohe Spannung oder Stromstärke |AUSSCHALTEN |EIN |EIN |EIN |
| PCM (Lüfter außerhalb des Toleranzbereichs) |EIN |AUSSCHALTEN |AUSSCHALTEN |EIN |
| Standby-Modell |Blinken |AUSSCHALTEN |AUSSCHALTEN |AUSSCHALTEN |
| PCM-firmwaredownload |AUSSCHALTEN |Blinken |Blinken |Blinken |

## <a name="controller-module-indicator-leds"></a>Indikator-LEDs des Controller-Modul
Das StorSimple-Gerät enthält LEDs für den primären Controller und die ebod-controllermodule.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Überwachen der LEDs für den primären controller
Die folgende Abbildung können Sie die LEDs auf dem primären Controller zu identifizieren. (Alle Komponenten werden aufgelistet, um Orientierung zu vereinfachen.)  

   ![Überwachen der LEDs - primären controller][4]

Verwenden Sie in der folgenden Tabelle, um zu bestimmen, ob das Controllermodul ordnungsgemäß funktioniert.  

### <a name="controller-indicator-leds"></a>Indikator-LEDs des Controllers
| LED | Beschreibung |
| --- | --- |
| ID-LED (Blau) |Gibt an, dass das Modul identifiziert wird. Wenn die blaue LED an einem laufenden Controller blinkt, ist der Controller ist der aktive Controller, und der andere Controller ist der standbycontroller. Weitere Informationen finden Sie unter [Identifizieren des aktiven Controllers auf Ihrem Gerät](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Fehler-LED (bernsteinfarben) |Gibt einen Fehler im Controller an. |
| OK-LED (Grün) |Gleichmäßiges Grün zeigt an, dass der Controller OK ist. Blinkendes Grün zeigt einen VPD-Konfigurationsfehler des Controllers an. |
| Aktivität der SAS-LEDs (Grün) |Gleichmäßiges Grün zeigt an, eine Verbindung ohne aktuelle Aktivität. Blinkendes Grün zeigt an, dass die Verbindung eine laufende Aktivität besteht. |
| Ethernetstatus-LEDs |Rechte Seite = Verbindung/Netzwerkaktivität: (gleichmäßig Grün) Verbindung aktiv, (blinkendes Grün) Netzwerkaktivität. Linke Seite = netzwerkgeschwindigkeit: (gelb) 1000 Mbit/s, 100 Mbit/s (Grün) und (OFF) 10 Mbit/s. Je nach Komponentenmodell kann diese LED auch blinken, wenn die Netzwerkschnittstelle nicht aktiviert ist. |
| POST-LEDs |Gibt den Startstatus an, wenn der Controller eingeschaltet wird. Wenn das StorSimple-Gerät kann nicht mehr gestartet werden, hilft dieser LED identifizieren, Punkt während des Startvorgangs der Fehler von Microsoft Support. |

> [!IMPORTANT]
> Wenn der Fehler-LED leuchtet, besteht ein Problem mit dem Controllermodul, das durch einen Neustart des Controllers behoben werden kann. Wenden Sie sich an Microsoft Support, wenn Neustart des Controllers dieses Problem nicht behoben wird.  
> 
> 

### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Überwachen der LEDs für EBOD (ebod-Gehäuse)
Jeder der 6 Gbit/s SAS EBOD-Controller verfügt über LEDs, die den Status angeben, wie in der folgenden Abbildung dargestellt.  

  ![Überwachen der LEDs - ebod-Gehäuse][5]

Verwenden Sie in der folgenden Tabelle, um zu bestimmen, ob das ebod-Controllermodul normal funktioniert.  

### <a name="ebod-controller-module-indicator-leds"></a>Ebod-Controller-Modul Indikator-LEDs
| Status | E/a-Modul OK (Grün) | E/a-Modulfehler (bernsteinfarben) | Portaktivität des Hosts (Grün) |
| --- | --- | --- | --- |
| Controllermodul OK |EIN |AUSSCHALTEN |- |
| Controllermodulfehler |AUSSCHALTEN |EIN |- |
| Keine externe hostportverbindung |- |- |AUSSCHALTEN |
| Externe hostportverbindung – keine Aktivität |- |- |EIN |
| Externe hostportverbindung – Aktivität |- |- |Blinken |
| Metadatenfehler des controllermoduls |Blinken |- |- |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Laufwerk-LED für das primäre Gehäuse und ebod-Gehäuse
Das StorSimple-Gerät verfügt über Datenträgerlaufwerke im primären Gehäuse und ebod-Gehäuse. Jedes Datenträgerlaufwerk enthält Überwachungsindikator-LEDs, wie in diesem Abschnitt beschrieben. 

Für die Datenträgerlaufwerke wird der Laufwerkstatus angezeigt, durch eine grüne LED und eine Rot-Bernsteinfarbene LED an der Vorderseite jedes lauferkträgermoduls bereitgestellt. Die folgende Abbildung zeigt diese LEDs.

  ![LEDs des Datenträgerlaufwerks][6]

Verwenden Sie in der folgenden Tabelle zum Ermitteln des Status jedes Datenträgerlaufwerks, was sich wiederum auf der gesamten Vorderseite LED-Status auswirkt.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Laufwerk-LED für das ebod-Gehäuse
| Status | Aktivität OK-LED (Grün) | Fehler-LED (Rot-bernsteinfarben) | Zugehörige Ops-bedienungsfelds |
| --- | --- | --- | --- |
| Kein Laufwerk installiert |AUSSCHALTEN |AUSSCHALTEN |Keine |
| Laufwerk installiert und betriebsbereit |Blinken/aus bei Aktivität |X |Keine |
| Identity-Gerätegruppe SCSI Enclosure Services (SES) |EIN |Blinken 1 Sekunde auf/1 Sekunde aus |Keine |
| SES-Geräts festgelegt |EIN |EIN |Logischer Fehler (Rot) |
| Stromausfall für Steuerelement-Verbindung |AUSSCHALTEN |EIN |Modulfehler (Rot) |

## <a name="audible-alarms"></a>Akustische Alarme
StorSimple-Gerät enthält akustische Alarme, die das primäre Gehäuse und ebod-Gehäuse zugeordnet. Der akustische Alarm befindet sich auf dem vorderen Bedienungsfeld (auch bekannt als der Ops-Bedienungsfeld) der beiden Gehäuse. Der akustische Alarm zeigt an, wenn eine fehlerbedingung vorhanden ist. Die folgenden Bedingungen aktivieren den Alarm:  

* Lüfterfehler oder-Ausfall
* Spannung außerhalb des gültigen Bereichs
* Failover oder Temperatur
* Thermaler Überlauf
* Systemfehler
* Logischer Fehler
* Stromversorgungsfehler
* Entfernung einer Kühleinheit (PCM) power  

Die folgende Tabelle beschreibt die verschiedenen alarmzustände.  

### <a name="alarm-states"></a>Alarmzustände
| Alarmzustand | Aktion | Aktion mit Stummtaste |
| --- | --- | --- |
| S0 |Normaler Modus: automatische |Zweimaliger Piepton |
| S1 |Fehlermodus: 1 Sekunde auf/1 Sekunde aus |Übergang in S2 oder S3 (siehe Hinweise) |
| S2 |Erinnerungsmodus: intermittierender Piepton |Keine |
| S3 |Stumm geschalteter Modus: automatische |Keine |
| S4 |Kritischer Fehlermodus: daueralarm |Nicht verfügbar: stummschaltung nicht aktiv |

> [!NOTE]
> * Im Alarmzustand S1 Wenn Sie keine stumm schalten innerhalb von zwei Minuten drücken geht der Status automatisch in S2 oder S3.  
> * Die alarmzustände S1 bis S4 kehren zu S0 zurück, nachdem die fehlerbedingung behoben wurde.  
> * Kritische Fehlerzustand S4 kann aus jedem anderen Zustand erreicht werden.  


Den akustischen Alarm kann durch Drücken der Stummschalttaste auf dem Ops-Bedienungsfeld stumm geschaltet werden. Die automatische stummschaltung erfolgt nach zwei Minuten, wenn der Stummschalter nicht manuell betrieben wird. Wenn der Alarm stumm geschaltet wird, wird er weiterhin kurze intermittierende Pieptöne, um anzugeben, dass weiterhin ein Problem vorliegt. Der Alarm wird verstummt, wenn alle Probleme behoben sind.

Die folgende Tabelle beschreibt die verschiedenen alarmbedingungen.

### <a name="alarm-conditions"></a>Alarmbedingungen
| Status | Schweregrad | Alarm | LED des OPS-bedienungsfelds |
| --- | --- | --- | --- |
| PCM-Alarm – Verlust des gleichstroms von einer PCM |Fehler – kein Verlust der Redundanz |S1 |Modulfehler |
| PCM-Alarm – Verlust des gleichstroms von einer PCM |Fehler – Verlust der Redundanz |S1 |Modulfehler |
| PCM-Lüfterfehler |Fehler – Verlust der Redundanz |S1 |Modulfehler |
| SBB-Modul hat PCM-Fehler erkannt. |Fehlertoleranz |S1 |Modulfehler |
| PCM entfernt |Fehler bei der Konfiguration |Keine |Modulfehler |
| Gehäusekonfigurationsfehler |Fehler – kritisch |S1 |Modulfehler |
| Niedrige Temperatur-Warnung |Warnung |S1 |Modulfehler |
| Hoher Temperatur-Warnung |Warnung |S1 |Modulfehler |
| Bei temperaturüberschreitung |Fehler – kritisch |S1 |Modulfehler |
| I2C-Bus-Fehler |Fehler – Verlust der Redundanz |S1 |Modulfehler |
| OPS-bedienungsfelds (I2C)-Kommunikationsfehler |Fehler – kritisch |S1 |Modulfehler |
| Controllerfehler |Fehler – kritisch |S1 |Modulfehler |
| Fehler des SBB-Schnittstellenmoduls |Fehler – kritisch |S1 |Modulfehler |
| Fehler des SBB-Schnittstellenmoduls – keine funktionierenden Module Verbleibend |Fehler – kritisch |S4 |Modulfehler |
| Fehler des SBB-Schnittstellenmodul entfernt |Warnung |Keine |Modulfehler |
| Fehler der energiesteuerung Laufwerk |Warnung – kein Verlust der laufwerkenergie |S1 |Modulfehler |
| Fehler der energiesteuerung Laufwerk |Fehler – kritisch; Verlust der laufwerkenergie |S1 |Modulfehler |
| Laufwerk entfernt |Warnung |Keine |Modulfehler |
| Keine ausreichende Energie verfügbar |Warnung |Keine |Modulfehler |

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [StorSimple Hardwarekomponenten und Status](storsimple-8000-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png


