---
title: Technische Spezifikationen StorSimple | Microsoft Docs
description: "Beschreibt die technischen Spezifikationen und Kompatibilität normungsinformationen für die StorSimple-Hardware-Komponenten."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 17ebf6b3-0872-4332-ac6e-074cc20a2b8e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: ac1f6fbd40770374f68d0d280fc1cc040e41b1ef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Technische Spezifikationen und Kompatibilität für das StorSimple-Gerät
## <a name="overview"></a>Übersicht
Die Hardwarekomponenten Ihres Microsoft Azure StorSimple-Geräts entsprechen, auf die technischen Spezifikationen und gesetzlichen Standards, die in diesem Artikel beschriebenen Schritte aus. Die technischen Spezifikationen beschreiben die Stromversorgung und Kühleinheiten (PCMs), Laufwerke, Speicherkapazität und Gehäuse. Die Kompatibilitätsinformationen werden u. a. die internationalen Institutionen, Sicherheit und Emissionen und Verkabelung behandelt.

## <a name="power-and-cooling-module-specifications"></a>Spezifikationen für Stromversorgung und Kühlung-Modul
Das StorSimple-Gerät verfügt über zwei dual 100-240-v-Lüfter, SBB-kompatible Power Kühleinheiten (PCMs). Dies bietet eine redundante stromversorgungskonfiguration. Wenn ein PCM ausfällt, funktioniert das Gerät weiterhin normal auf dem anderen PCM ausgeführt werden, bis das fehlerhafte Modul ausgetauscht wurde.  

Das ebod-Gehäuse wird ein PCM mit 580 W und primäres Gehäuse wird ein PCM mit 764 W verwendet. Die folgenden Tabellen enthalten die technischen Spezifikationen der pcms an.

| Spezifikation | PCM MIT 580 W (EBOD) | PCM mit 764 W (primär) |
| --- | --- | --- |
| Maximale ausgangsleistung |580 W |764 |
| Häufigkeit |50/60 Hz |50/60 Hz |
| Spannung Bereichsauswahl |Automatische reichen: 90-264 V Wechselstrom, 47/63 Hz |Automatische reichen: 90-264 V Wechselstrom, 47/63 Hz |
| Maximale Einschaltstrom |20 EIN |20 EIN |
| Blindstromkompensation |> 95 % nominale eingangsspannung |> 95 % nominale eingangsspannung |
| Oberschwingungen |Erfüllt en 61000-3-2 |Erfüllt en 61000-3-2 |
| Ausgabe |5 v bereitschaftsspannung bei 2,0 A |5 v bereitschaftsspannung bei 2.7 ein |
| + 5V @ 42 EIN |+ 5V @ 40 A | |
| + 12 V BEI @ 38 EIN |+ 12 V BEI @ 38 EIN | |
| Laufenden Sie austauschbar im Betrieb |Ja |Ja |
| Schalter und LEDs |AC/aus-Schalter und vier Statusindikator-LEDs |AC/aus-Schalter und sechs Statusindikator-LEDs |
| Gehäusekühlung |Axiale Lüfter mit variabler Lüfterdrehzahl |Axiale Lüfter mit variabler Lüfterdrehzahl |

## <a name="power-consumption-statistics"></a>Energieverbrauchsstatistik
Die folgende Tabelle enthält die typische Energieverbrauchsdaten (tatsächlichen Werte können aus den veröffentlichten variieren) für die verschiedenen Modelle eines StorSimple-Gerät. 

| Bedingungen | 240 V WECHSELSTROM | 240 V WECHSELSTROM | 240 V WECHSELSTROM | 110 V WECHSELSTROM | 110 V WECHSELSTROM | 110 V WECHSELSTROM |
| --- | --- | --- | --- | --- | --- | --- |
|  Lüfter langsam, Laufwerke im Leerlauf |1,45 A |0,31 kW |1057.76 BTU/h |3.19 EIN |0.34 kW |1160.13 BTU/h |
|  Lüfter langsam, Zugriff auf Laufwerke |1.54 EIN |0,33 kW |1126.01 BTU/h |3.27 EIN |0,36 kW |1228.37 BTU/h |
|  Lüfter schnell, Laufwerke im Leerlauf, zwei aktive Netzteile |2.14 EIN |0.49 kW |1671.95 BTU/h |4,99 EIN |0.54 kW |1842.56 BTU/h |
|  Lüfter schnell, Laufwerke im Leerlauf, ein aktives Netzteil, eines im Leerlauf |2.05 EIN |0.48 kW |1637.83 BTU/h |4.58 EIN |0,50 kW |1706.07 BTU/h |
|  Lüfter schnell, Zugriff auf Laufwerke, zwei aktive Netzteile |2.26 EIN |0.51 kW |1740.19 BTU/h |4,95 EIN |0.54 kW |1842.56 BTU/h |
|  Lüfter schnell, Netzteil, Zugriff auf Laufwerke, ein aktives eines im Leerlauf |2.14 EIN |0.49 kW |1671.95 BTU/h |4.81 EIN |0.53 kW |1808.44 BTU/h |

## <a name="disk-drive-specifications"></a>Spezifikationen für Laufwerke
Ihr StorSimple-Gerät unterstützt bis zu 12 3,5 Zoll Faktor Serial Attached SCSI (SAS)-Laufwerke. Die tatsächlichen Laufwerke können eine Mischung aus Solid State Drives (SSDs) oder Festplattenlaufwerken (HDDs), abhängig von der Produktkonfiguration sein. Die 12 Einschubfächer befinden sich in einer 3 x 4-Konfiguration vor dem Gehäuse. Das ebod-Gehäuse ermöglicht zusätzlichen Speicher für weitere 12 Laufwerke. Diese stellen immer HDDs.  

## <a name="storage-specifications"></a>Speicher-Spezifikationen
Der StorSimple-Geräte haben eine Mischung von Festplattenlaufwerken und Solid State Drives für 8100 und 8600. Die nutzbare Gesamtkapazität für 8100 und 8600 lauten etwa 15 TB und 38 TB. In der folgenden Tabelle werden die Details des SSD und HDD cloudkapazität im Rahmen der Kapazität der StorSimple-Lösung beschrieben.

| Gerätemodell / Kapazität | 8100 | 8600 |
| --- | --- | --- |
| Anzahl von Festplattenlaufwerken (HDDs) |8 |19 |
| Anzahl von Solid State Drives (SSDs) |4 |5 |
| Einzelne Festplattenkapazität |4 TB |4 TB |
| Einzelne SSD-Kapazität |400 GB |800 GB |
| Reservekapazität |4 TB |4 TB |
| HDD Kapazität |14 TB |36 TB |
| Nutzbare SSD-Kapazität |800 GB |2 TB |
| Insgesamt verwendbaren Kapazität * |~ 15 TB |~ 38 TB |
| Maximale Lösung Kapazität (einschließlich der Cloud) |200 TB |500 TB |

<sup>* </sup>- *Die Gesamtkapazität für nutzbare umfasst die verfügbare Kapazität für Daten, Metadaten und Puffer.*

## <a name="enclosure-dimensions-and-weight-specifications"></a>Gehäuseabmessungen und Gewichtung-Spezifikationen
Die folgenden Tabellen enthalten die verschiedenen gehäusespezifikationen für Abmessungen und Gewicht.  

### <a name="enclosure-dimensions"></a>Gehäuseabmessungen
In der folgenden Tabelle sind die Dimensionen des Gehäuses in Millimeter und Zoll aufgeführt.

| Gehäuse | Millimeter | Zoll |
| --- | --- | --- |
| Höhe |87.9 |3.46 |
| Breite Frontplatte |483 |19.02 |
| Breite des gehäusekörpers |443 |17.44 |
| Tiefe von der Frontplatte bis zum hinteren Ende des gehäusekörpers |577 |22.72 |
| Tiefe vom Bedienungsfeld bis zum hintersten Ende des Gehäuses |630.5 |24.82 |
| Tiefe von der Frontplatte bis zum hintersten Ende des Gehäuses |603 |23.74 |

### <a name="enclosure-weight"></a>Gewicht des Gehäuses
Abhängig von der Konfiguration vollständig geladen primäres Gehäuse zwischen 21 und 33 kg wiegen bestücktes und sind zwei Personen für seine Handhabung erforderlich. 

| Gehäuse | Gewichtung |
| --- | --- |
| Maximales Gewicht (hängt von der Konfiguration) |30 kg – 33 kg |
| Leer (keine eingebauten Laufwerke) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Die gehäuseumgebung
Dieser Abschnitt enthält die Spezifikationen, die sich auf die gehäuseumgebung beziehen. In dieser Kategorie sind die Temperatur, Luftfeuchtigkeit, einsatzhöhe, erschütterungen, Vibrationen, Ausrichtung, Sicherheit und elektromagnetische (EMV) enthalten.  

### <a name="temperature-and-humidity"></a>Temperatur und Luftfeuchtigkeit
| Gehäuse | Bereich für Umgebungstemperatur | Relative Luftfeuchtigkeit | Maximale feuchtkugeltemperatur |
| --- | --- | --- | --- |
| Betriebsbereit |5° C - 35° C (41° C - 95° F) |20 % - 80 % nicht-kondensierend- |28° C (82° F) |
| Außer Betrieb |-40° C - 70° C (40° C - 158° F) |5 % - 100 % nicht kondensierend |29° C (84° F) |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Luftstrom, einsatzhöhe, erschütterungen, Vibrationen, Ausrichtung, Sicherheit und EMV
| Gehäuse | Spezifikationen für den Betrieb |
| --- | --- |
| Luftstrom |Der Luftstrom im System erfolgt von vorne nach hinten. System muss mit einer niederdruckstallation hinten Installation betrieben werden. Gegendruck durch gestelltüren und Hindernisse sollte 5 PA (0,5 mm Wassersäule) nicht überschreiten. |
| Einsatzhöhe, in Betrieb |-30 Meter 3045 Meter (-100 Feet auf 10.000 Feet) mit maximale Betriebstemperatur Deduplizierung von 5 ° C oberhalb 7000 Feet bewertet. |
| Einsatzhöhe, nicht in Betrieb |-305 Meter 12.192 Meter (-1,000 Feet zu 40.000 Feet) |
| Erschütterungen, in Betrieb |5g 10 ms ½ Sinus |
| Erschütterungen, nicht in Betrieb |30g 10 ms ½ Sinus |
| Vibrationen, in Betrieb |0, 21 g RMS 5-500 Hz zufällig |
| Vibrationen, nicht in Betrieb |1, 04 g RMS 2-200 Hz zufällig |
| Vibrationen, umplatzierung |3g 2-200 Hz Sinus |
| Ausrichtung und Einbau |19-Zoll-Gestell (2 EIA-Einheiten) |
| Des Gestells |Entsprechend (31.50 Zoll) mit mindestens 700 mm Tiefe racks kompatibel mit IEC 297 |
| Sicherheit und Zulassungen |CE und UL EN 61000-3, IEC 61000-3, UL 61000-3 |
| EMC |EN 55022 (CISPR - EIN), FCC EIN |

## <a name="international-standards-compliance"></a>Internationale Normen-Kompatibilität
Ihr Microsoft Azure StorSimple-Gerät erfüllt die folgenden internationalen Normen:  

* CE - EN 60950-1  
* CB-Bericht zu IEC 60950-1  
* UL und cUL in UL 60950-1  

## <a name="safety-compliance"></a>Sicherheitskonformität
Ihr Microsoft Azure StorSimple-Gerät erfüllt die folgenden sicherheitsbewertungen:  

* Produkttypzulassung des Systems: UL, cUL, CE  
* Sicherheitskonformität: UL 60950, IEC 60950, EN 60950  

## <a name="emc-compliance"></a>EMV-Konformität
Ihr Microsoft Azure StorSimple-Gerät erfüllt die folgenden EMC Bewertungen.  

### <a name="emissions"></a>Emissionen
Das Gerät ist der leitungsgebundenen und der feldgebundenen Emissionen EMV EMV-konform.  

* Leitungsgebundenen Emissionen Ebenen zu beschränken: CFR 47 Teil 15 b Klasse A en 55022 Klasse A CISPR Klasse A  
* Feldgebundenen Emissionen Ebenen zu beschränken: CFR 47 Teil 15 b Klasse A en 55022 Klasse A CISPR Klasse A   

### <a name="harmonics-and-flicker"></a>Oberschwingungen und Flimmern
Das Gerät erfüllt en 61000-3-2/3.  

### <a name="immunity-limit-levels"></a>Störfestigkeitsgrenzwerte
Das Gerät erfüllt en 55024.  

## <a name="ac-power-cord-compliance"></a>AC Power Kabel Kompatibilität
Der Stecker und das gesamte Netzkabel müssen die Normen erfüllen für das Land, in dem das Gerät verwendet wird, und sie müssen sicherheitszulassungen, die in diesem Land gültig sind. Die folgenden Tabellen enthalten die Standards für die USA und Europa.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>Netzkabel - USA (NRTL aufgeführt sein muss)
| Komponente | Spezifikation |
| --- | --- |
| Kabeltyp |SV oder SVT, mindestens 18 AWG Minimum, 3 Adern, 2,0 Meter maximale Länge |
| Plug & |NEMA 5-15P Erdung Typ Anlage Plug bewertet 120 V, 10 A; oder IEC 320 C14, 250 V, 10 A |
| Socket |IEC 320 C-13, 250 V, 10 A |

### <a name="ac-power-cords---europe"></a>Netzkabel - Europa
| Komponente | Spezifikation |
| --- | --- |
| Kabeltyp |Harmonisiert, H05-VVF 3g1,0 |
| Socket |IEC 320 C-13, 250 V, 10 A |

## <a name="supported-network-cables"></a>Unterstützte Netzwerkkabel
Die 10-GbE-Netzwerkschnittstellen data2 und data3, finden Sie in der [Liste der unterstützten Netzwerkkabel](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Nächste Schritte
Sie können nun ein virtuelles StorSimple-Gerät in Ihrem Datencenter bereitstellen. Weitere Informationen finden Sie unter [Bereitstellen des lokalen Geräts](storsimple-deployment-walkthrough-u2.md).  

