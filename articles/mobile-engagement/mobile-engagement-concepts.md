---
title: Mobile Engagement-Konzepte | Microsoft Docs
description: Azure Mobile Engagement-Konzepte
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 8450651528007b4527366b89a6ad7615169f93c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile Engagement-Konzepte
Mobile Engagement definiert einige allgemeine Konzepte auf allen unterstützten Plattformen. Dieser Artikel beschreibt kurz die Konzepte.

Dieser Artikel ist ein guter Ausgangspunkt, wenn Sie Mobile Engagement nicht vertraut sind. Stellen Sie außerdem sicher, dass die Dokumentation, die spezifisch für die Plattform, die Sie verwenden, die lesen, während es die Konzepte in diesem Artikel mit Weitere Details und Beispiele sowie mögliche Einschränkungen beschrieben verfeinern wird.

## <a name="devices-and-users"></a>Geräte und Benutzer
Mobile Engagement identifiziert den Benutzer durch einen eindeutigen Bezeichner für jedes Gerät generieren. Dieser Bezeichner wird aufgerufen, die Geräte-ID (oder `deviceid`). Es wird in eine Möglichkeit, dass alle Anwendungen ausgeführt wird, von dem gleichen Gerät nutzen die gleiche Geräte-ID generiert.

Implizit, bedeutet dies, dass Mobile Engagement betrachtet, ein Gerät nur ein Benutzer gehört, und daher Benutzer und Geräte entsprechende Konzepte sind.

## <a name="sessions-and-activities"></a>Sitzungen und Aktivitäten
Eine Sitzung ist eine Verwendung der Anwendung von einem Benutzer ausgeführt, von dem Zeitpunkt der Benutzer startet verwenden, bis der Benutzer wird beendet.

Eine Aktivität ist eine Verwendung von einem bestimmten untergeordneten Teil der Anwendung von einem Benutzer ausgeführt (normalerweise ein Bildschirm ist, aber kann alles an die Anwendung geeignet sein).

Ein Benutzer kann nur eine Aktivität zu einem Zeitpunkt durchführen.

Eine Aktivität wird durch einen Namen (maximal 64 Zeichen) identifiziert und kann optional auch einige zusätzlichen Daten (in den Grenzwert von 1024 Bytes) einbetten.

Sitzungen werden aus der Abfolge von Aktivitäten, die von Benutzern durchgeführt werden automatisch berechnet. Eine Sitzung gestartet wird, wenn der Benutzer seine erste Aktivität startet und beendet, wenn er seine letzte Aktivität abgeschlossen wurde. Dies bedeutet, dass eine Sitzung nicht explizit gestartet oder beendet werden muss. Aktivitäten werden stattdessen explizit gestartet oder beendet. Wenn keine Aktivität gemeldet wird, wird keine Sitzung gemeldet.

## <a name="events"></a>Ereignisse
Ereignisse werden verwendet, um sofortige Aktionen (z. B. gedrückt oder Artikel lesen, die von Benutzern) zu melden.

Ein Ereignis kann sich auf der aktuellen Sitzung, um einen gerade ausgeführten Auftrag beziehen, oder sie können ein eigenständiges Ereignis sein.

Ein Ereignis wird durch einen Namen (maximal 64 Zeichen) identifiziert und kann optional auch einige zusätzlichen Daten (in den Grenzwert von 1024 Bytes) einbetten.

## <a name="error"></a>Fehler
Fehler werden verwendet, um Probleme, die von der Anwendung (z. B. falsche Benutzeraktionen oder API-Aufruffehler) ordnungsgemäß erkannt zu melden.

Ein Fehler kann auf der aktuellen Sitzung, um einen gerade ausgeführten Auftrag beziehen, oder sie können ein eigenständiges Fehler sein.

Ein Fehler wird durch einen Namen (maximal 64 Zeichen) identifiziert und kann optional auch einige zusätzlichen Daten (in den Grenzwert von 1024 Bytes) einbetten.

## <a name="job"></a>Auftrag
Aufträge werden verwendet, um die Aktionen, die über einen Zeitraum zu Berichten (wie die Dauer der API-Aufrufe, Anzeigen von werbeeinblendungen, Dauer Hintergrundaufgaben oder Dauer von Benutzeraktionen).

Ein Auftrag mit einer Sitzung bezieht sich nicht, da eine Aufgabe im Hintergrund und ohne Eingreifen des Benutzers ausgeführt werden kann.

Ein Auftrag wird durch einen Namen (maximal 64 Zeichen) identifiziert und kann optional auch einige zusätzlichen Daten (in den Grenzwert von 1024 Bytes) einbetten.

## <a name="crash"></a>Absturz (Crash)
Abstürze werden automatisch vom Mobile Engagement SDK eines Absturzes Melden von Anwendungsfehlern, in dem von der Anwendung nicht erkannte Probleme erleichtern, ausgegeben.

## <a name="application-information"></a>Anwendungsinformationen
Anwendungsinformationen (oder AppInfo) wird zum Kennzeichnen von Benutzern, d. h. verwendet, um einige Daten für die Benutzer von einer Anwendung zuzuordnen (dieser Vorgang ähnelt Web Cookies, außer dass die app-Informationen auf der Azure Mobile Engagement-Plattform auf dem Server gespeichert ist).

AppInfo kann mithilfe der Mobile Engagement-SDK-API oder mithilfe von Mobile Engagement-Plattform Geräte-API registriert werden.

AppInfo ist eine Schlüssel-/Wertepaar, die einem Gerät zugeordnet sind. Der Schlüssel ist der Name des AppInfo (auf 64 ASCII-Buchstaben [a-zA-Z], [0-9]-Zahlen und Unterstriche [_] beschränkt). Der Wert (maximal 1024 Zeichen sind zulässig) kann es sich um eine beliebige Zeichenfolge, die ganze Zahl, die Datum (JJJJ-MM-TT) oder die boolesche Werte ("true" oder "false") sein.

Eine beliebige Anzahl von AppInfo kann auf ein Gerät innerhalb der Grenzen, die durch die Mobile Engagement-preiskonditionen definierten verknüpft werden. Für einen bestimmten Schlüssel der nachverfolgt Mobile Engagement nur der neueste Wertsatz (kein Verlauf). Festlegen oder Ändern des Werts von einer AppInfo erzwingt die Mobile Engagement, Zielgruppe erneut auszuwerten festgelegten Kriterien zu dieser AppInfo (sofern vorhanden), AppInfo bedeutet verwendet werden kann, Echtzeit-Push-Vorgänge ausgelöst.

## <a name="extra-data"></a>Zusätzliche Daten
Zusätzliche Daten (oder Extras) werden einige Daten, die Ereignisse und Fehler, Aktivitäten und Aufträge angefügt werden können.

Extras auf JSON-Objekte entsprechend strukturiert sind: eine Struktur von Schlüssel-/Wertpaaren hergestellt werden. Schlüssel sind beschränkt auf 64 ASCII-Buchstaben, [a-zA-Z], [0-9]-Zahlen und Unterstriche [_]) und die Gesamtgröße der Extras auf 1024 Zeichen (einmal im JSON-Format durch das Mobile Engagement SDK codiert) beschränkt ist.

Die gesamte Struktur von Schlüssel/Wert-Paare werden als JSON-Objekt gespeichert. Nichtsdestotrotz ist nur die erste Ebene der Tastenwerte/zerlegten einige erweiterten Funktionen wie Segmente direkt zugegriffen werden (z. B. können problemlos definiert ein Segment "SciFi Lüfter" ab, das alle Benutzer müssen mindestens 10 Mal das Ereignis mit dem Namen "Content_viewed" gesendet wird aufgerufen, mit den zusätzlichen Schlüssel "Content_type" auf den Wert "Scifi" im letzten Monat festgelegt). Es wird daher empfohlen senden, dass nur Extras einfache Listen von Schlüssel/Wert-Paaren, die mit skalaren Werten (z. B. Zeichenfolgen, Datumsangaben, Zahlen oder boolescher Wert) vorgenommen.

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das universelle Windows-SDK für Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
* [Übersicht über das Windows Phone Silverlight-SDK für Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
* [iOS SDK für Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
* [Android-SDK für Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

