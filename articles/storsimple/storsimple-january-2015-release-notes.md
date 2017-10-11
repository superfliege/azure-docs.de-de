---
title: 0,2 Versionshinweise StorSimple 8000-Update | Microsoft Docs
description: "Beschreibt die neuen Features und Fixes, offene Probleme und problemumgehungen verfügbar für die Januar 2015 Microsoft Azure StorSimple-Version (Update 0,2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 2fc50f7faddb4b61ea5e7d328469fc3cc0eb6f3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 Series Update 0,2 Versionshinweise – Januar 2015
## <a name="overview"></a>Übersicht
Die folgenden Anmerkungen zu dieser Version identifizieren Sie die kritischen offenen Probleme für die vom Januar 2015-Version von Microsoft Azure StorSimple. Sie enthalten auch eine Liste der StorSimple-Software und-Firmware Updates in dieser Version enthalten. Dies ist die zweite Version nach die Version von StorSimple 8000-Serie Version in der Regel im Juli 2014 verfügbar waren.

Dieses Update ändert nicht die physische Geräte verwendete Softwareversion Gerätesoftware des Oktober-Updates. Weiterhin um Version 6.3.9600.17312. Das Bild verwendet, die für das Image des virtuellen Geräts wurde in dieser Version geändert. Aus diesem Grund werden alle neuen virtuellen Geräte nach 1/20/2015 erstellt die 6.3.9600.17361 als Version angezeigt.  

Überprüfen Sie die folgende Informationen, die in den Anmerkungen zur Version für das Update vom Januar 2015 enthalten sind.

> [!IMPORTANT]
> * Dieses Update ist nicht über Windows Update verfügbar und kann nicht wie andere Updates installiert werden. Ihr Gerät wird dieses Update nicht empfangen, auch wenn Sie die Updates angewendet haben, mithilfe des klassische Azure-Portal. Dieses Update gilt nur für virtuelle Geräte, die nach dem 20. Januar 2015 erstellt wurden. 
> * Das Januar-Version von StorSimple enthält keine Updates an das physische StorSimple-Gerät. Können Sie weiterhin alle verfügbaren Windows-Updates anwenden, um das virtuelle Gerät, einschließlich jüngster Sicherheitsupdates, aber Sie werden feststellen, dass eine Änderung der Version für das physische StorSimple-Gerät.
> 
> 

## <a name="whats-new-in-the-january-release"></a>Was ist neu in Version vom Januar
Dieses Update enthält eine Korrektur hinsichtlich der Volumes auf dem virtuellen Gerät offline geschaltet. (Siehe [Probleme in dieser Version behobene](#issues-fixed-in-the-january-release).)  

Das Update enthält keine neuen Features oder Funktionen.  

## <a name="issues-fixed-in-the-january-release"></a>In der Version vom Januar behobene Probleme
Die folgende Tabelle beschreibt das Problem, das in diesem Update behoben wurde.

| Nein. | Komponente | Problem | Gilt für physische Geräte | Gilt für das virtuelle Gerät |
| --- | --- | --- | --- | --- |
| 1 |Volumes offline geschaltet |Wenn mehrere Minuten hohe Cloud-Latenzen beibehalten werden, gehen die StorSimple-Volumes virtuelles Gerät auf den Hosts offline. Diese Korrektur erhöht den Schwellenwert für die Cloud-Latenzen, minimieren dadurch die Situationen, in denen die Volumes auf Hosts offline geschaltet führen würde. |Nein |Ja |

## <a name="known-issues-in-the-january-release"></a>Bekannte Probleme in der Version vom Januar
Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.

| Nein. | Komponente | Problem | Kommentare/problemumgehung | Gilt für physische Geräte | Gilt für das virtuelle Gerät |
| --- | --- | --- | --- | --- | --- |
| 1 |Zurück auf Werkseinstellungen |In einigen Fällen beim Ausführen von auf die werkseinstellungen zurückgesetzt, das StorSimple-Gerät kann hängen bleiben und diese Nachricht anzeigen: **Zurücksetzung auf werkseinstellungen wird ausgeführt (Phase 8).** Dies geschieht, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. |Drücken Sie STRG + C nicht nach dem Initiieren des auf die werkseinstellungen zurückgesetzt. Wenn Sie bereits in diesem Zustand befinden, wenden Sie sich an den Microsoft Support, für die nächsten Schritte. |Ja |Nein |
| 2 |Datenträgerquorum |In seltenen Fällen Wenn der Großteil der Datenträger im ebod-Gehäuse eines Geräts 8600 getrennt werden, sodass kein datenträgerquorum wird dann der Speicherpool offline sein. Es wird offline geschaltet bleiben, selbst wenn der Datenträger wieder verbunden ist. |Sie müssen das Geräte neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für die nächsten Schritte. |Ja |Nein |
| 3 |Momentaufnahmefehler in der Cloud |In seltenen Fällen kann eine Cloud-Momentaufnahme mit der Fehlermeldung fehlschlagen **Maximales sicherungslimit erreicht**. Dies tritt auf, wenn Sie 255 onlineklone auf demselben Gerät, von demselben ursprünglichen Volume überschreiten, das gelöscht wurde. | |Ja |Ja |
| 4 |Falsche Controller-ID |Bei einem Austausch eines Controllers ausgeführt wird, kann der Controller 0 als Controller 1 angezeigt. Während des controlleraustauschs Wenn das Image vom Peerknoten geladen ist kann die Controller-ID anfänglich als dem peercontroller-ID angezeigt  In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems auftreten. |Es ist keine Benutzeraktion erforderlich. Diese Situation sollte sich auflösen, nachdem der controlleraustausch abgeschlossen ist. |Ja |Nein |
| 5 |Geräteüberwachungsdiagramme |In StorSimple Manager-Dienst funktionieren die geräteüberwachungsdiagramme nicht, wenn grundlegende oder NTLM-Authentifizierung in der Proxyserverkonfiguration für das Gerät aktiviert ist. |Ändern Sie die webproxykonfiguration für das Gerät bei Ihrem StorSimple Manager-Dienst registriert werden, damit die Authentifizierung auf NONE festgelegt wird. Führen Sie hierzu die Windows PowerShell für StorSimple Set HcsWebProxy-Cmdlet. |Ja |Ja |
| 6 |Speicherkonten |Verwenden des Speicherdiensts So löschen Sie das Speicherkonto ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in der Benutzerdaten abgerufen werden können. | |Ja |Ja |
| 7 |Geräte-failover |Mehrere Failover eines volumecontainers von demselben Quellgerät auf verschiedene Zielgeräte wird nicht unterstützt. |Failover von einem einzelnen nicht reagierenden Gerät auf mehrere Geräte stellen die volumecontainer auf dem ersten Gerät mit erfolgtem Failover dateneigentümerschaft verlieren. Nach einem Failover werden diese volumecontainer angezeigt oder Verhalten sich anders, wenn Sie sie im klassischen Azure-Portal anzeigen. |Ja |Nein |
| 8 |Installation |Während des StorSimple-Adapters für SharePoint-Installation müssen Sie bieten eine Geräte-IP, damit die Installation erfolgreich abgeschlossen. | |Ja |Nein |
| 9 |Webproxy |Wenn Ihre webproxykonfiguration HTTPS als das angegebene Protokoll verfügt, wirkt sich Ihr Gerät-zu-Dienst-Kommunikation, und das Gerät offline geschaltet wird. Supportpakete werden auch bei der Nutzung umfangreiche Ressourcen auf Ihrem Gerät generiert werden. |Stellen Sie sicher, dass die Webproxy-URL über HTTP als das angegebene Protokoll verfügt. Weitere Informationen zum finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](storsimple-configure-web-proxy.md). |Ja |Nein |
| 10 |Webproxy |Wenn Sie konfigurieren und des Webproxys auf einem registrierten Gerät aktivieren, müssen Sie den aktiven Controller auf Ihrem Gerät neu starten. | |Ja |Nein |
| 11 |Hohe Cloud-Latenz und hoher e/a-arbeitsauslastung |Wenn Ihr StorSimple-Gerät eine Kombination aus sehr hohen Cloud-Latenzen (mehrere Sekunden) und hoher e/a-arbeitsauslastung stößt, wird den gerätevolumes wechseln, verschlechtert sich die Leistung, und die e/as möglicherweise ein Fehler mit dem Fehler "Gerät nicht bereit". |Sie müssen manuell neu starten die Geräte-Controller oder Ausführen eines gerätefailovers, um diese Situation zu beheben. |Ja |Nein |

## <a name="physical-device-updates-in-the-january-release"></a>Physisches Geräteupdates in der Januar zu veröffentlichen
Dieses Update enthält keine anderen Änderungen an das StorSimple-Gerät.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Serial attached SCSI (SAS)-Controller und Firmware-Updates in der Januar freigeben
Diese Version enthält keine Updates an den Serial attached SCSI (SAS)-Controller oder die Firmware. Die treiberaktualisierung erfolgte in der Oktober 2014-Version. 

## <a name="virtual-device-updates-in-the-january-release"></a>Virtuelles Geräteupdates in der Januar zu veröffentlichen
Diese Version enthält ein aktualisiertes Image für das virtuelle Gerät an. Alle virtuellen Geräte, die nach dem 20. Januar 2015 erstellt zeigt die Version der Software als "6.3.9600.17361" angezeigt.

