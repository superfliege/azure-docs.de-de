---
title: Includedatei
description: Includedatei
services: cognitive-services
author: MikeDodaro
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 04/19/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: f166ceac1ae848565f861a94781ce0500c24747e
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289154"
---
Die Anforderungen an die Verwendung und Anzeige gelten für jede Implementierung der Inhalte und der zugehörigen Informationen. Beispielsweise gelten die Anforderungen für Beziehungen, Metadaten und andere Signale. Diese können durch Aufrufe der folgenden APIs verfügbar sein:

- Benutzerdefinierte Bing-Suche
- Bing-Entitätssuche
- Bing-Bildersuche
- Bing News-Suche
- Bing-Videosuche
- Visuelle Bing-Suche
- Bing-Websuche
- Bing-Rechtschreibprüfung
- Bing-Vorschlagssuche

Implementierungsdetails im Zusammenhang mit diesen Anforderungen finden Sie in der Dokumentation für bestimmte Features und Ergebnisse.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>Bing-Rechtschreibprüfungs-API und Bing-Vorschlagssuche-API

Vermeiden Sie Folgendes:

- Kopieren, Speichern oder Zwischenspeichern von Daten, die Sie von der Bing-Rechtschreibprüfungs-API oder der Bing-Vorschlagssuche-API erhalten
- Verwenden der Daten, die Sie von der Bing-Rechtschreibprüfungs-API oder der Bing-Vorschlagssuche-API erhalten, im Rahmen einer Aktivität mit maschinellem Lernen oder einer ähnlichen algorithmischen Aktivität Verwenden Sie diese Daten nicht zum Trainieren, Auswerten oder Verbessern neuer oder vorhandener Dienste, die Sie oder Dritte möglicherweise anbieten

## <a name="definitions"></a>Definitionen

- *Antwort* bezieht sich auf eine Kategorie von Ergebnissen, die in einer Reaktion zurückgegeben werden. Eine Reaktion der Bing-Websuche-API kann beispielsweise Antworten in den Kategorien „Webseitenergebnisse“, „Bild“, „Video“ „Thema“ und „News“ enthalten.   
- Als *Reaktion* werden sämtliche Antworten und die zugehörigen Daten bezeichnet, die infolge eines einzelnen Aufrufs einer Such-API empfangen werden.
- *Ergebnis* bezieht sich auf ein Informationselement in einer Antwort. Die Daten in Verbindung mit einem einzelnen Nachrichtenartikel sind beispielsweise ein Ergebnis in einer Nachrichtenantwort.
- *Such-APIs* stehen als Sammelbegriff für die APIs der benutzerdefinierten Bing-Suche, der Entitätssuche, der Bildersuche, der News-Suche, der Videosuche, der thematischen Suche und der Websuche. 


## <a name="search-apis"></a>Such-APIs

Die Anforderungen in diesem Abschnitt gelten für die Such-APIs. Zu den Such-APIs gehören nicht die Bing-Rechtschreibprüfung oder die Bing-Vorschlagssuche. Die Anforderungen für diese beiden APIs werden im vorherigen Abschnitt behandelt.

### <a name="internet-search-experience"></a>Internetsuche

Alle in einer Reaktion zurückgegebenen Daten dürfen ausschließlich im Rahmen von Internetsuchvorgängen verwendet werden. Internetsuche bedeutet, dass für den angezeigten Inhalt Folgendes gilt (sofern zutreffend): 
- Er ist für die direkte Abfrage des Endbenutzers oder für einen anderen Hinweis auf das Suchinteresse oder die Suchabsicht des Benutzers (beispielsweise eine vom Benutzer angegebene Suchabfrage) relevant und eine Reaktion darauf. 
- Er hilft Benutzern dabei, Datenquellen zu finden und aufzurufen. (Beispiel: Die bereitgestellten URLs werden als Links implementiert, sodass es sich bei dem Inhalt oder der Namensnennung um einen klickbaren Link handelt, der gut sichtbar zusammen mit den Daten angezeigt wird.) Im Fall der Bing-Entitätssuche-API müssen Sie sichtbar zur bing.com-URL aus der Reaktion verlinken, über die der Benutzer zu den Suchergebnissen der relevanten Abfrage auf bing.com gelangt.
- Er enthält mehrere Ergebnisse, aus denen der Benutzer wählen kann. (Beispiel: Es werden mehrere Ergebnisse aus der Nachrichtenantwort angezeigt – oder alle, falls weniger Ergebnisse zurückgegeben werden.) 
- Er ist auf einen für den Zweck der Suche angemessenen Umfang beschränkt. (Beispiel: Miniaturansichten von Bildern haben relativ zum Display des Benutzers die Größe von Miniaturansichten.) 
- Er ist für den Endbenutzer gut sichtbar als Internetsuchergebnis gekennzeichnet (etwa durch einen Hinweis wie „Aus dem Web“).
- Er umfasst eine beliebige andere Kombination von geeigneten Maßnahmen, die sicherstellen, dass Ihre Verwendung von Daten, die von den Such-APIs empfangen wurden, nicht gegen geltende Gesetze verstößt oder die Rechte Dritter verletzt. Wenn Sie sich beispielsweise nach einer Creative Commons-Lizenz richten, müssen Sie die geltenden Lizenzbedingungen einhalten. Wenden Sie sich gegebenenfalls an Ihre Rechtsberater, um zu klären, welche Maßnahmen hierfür geeignet sind.
Die einzige Ausnahme für die Anforderung bei der Internetsuche betrifft die URL-Ermittlung, wie weiter unten in diesem Artikel beschrieben. 

### <a name="restrictions"></a>Einschränkungen

Vermeiden Sie Folgendes:

- Kopieren, Speichern oder Zwischenspeichern von Daten aus Reaktionen (mit Ausnahme der zulässigen Aufbewahrung gemäß dem Abschnitt zur Dienstkontinuität weiter unten in diesem Artikel) 
- Verwenden der Daten, die Sie von den Such-APIs erhalten, im Rahmen einer Aktivität mit maschinellem Lernen oder einer ähnlichen algorithmischen Aktivität Verwenden Sie diese Daten nicht zum Trainieren, Auswerten oder Verbessern neuer oder vorhandener Dienste, die Sie oder Dritte möglicherweise anbieten
- Ändern des Inhalts von Ergebnissen (mit Ausnahme der Anpassung der Formatierung unter Einhaltung sämtlicher anderer Anforderungen), sofern dies nicht gesetzlich erforderlich ist oder mit Microsoft abgestimmt wurde 
- Entfernen der dem Ergebnisinhalt zugeordneten Namensnennung und URLs
- Ändern der Reihenfolge (einschließlich Auslassung) von Ergebnissen in einer Antwort, wenn eine Reihen- oder Rangfolge bereitgestellt wird, sofern dies nicht gesetzlich erforderlich ist oder mit Microsoft abgestimmt wurde (Bei der API für die benutzerdefinierte Bing-Suche gilt diese Regel nicht für Änderungen der Reihenfolge, die durch das customsearch.ai-Portal implementiert wurden.)
- Anzeigen anderer Inhalte innerhalb eines beliebigen Teils einer Reaktion auf eine Weise, die einem Benutzer vorspiegelt, der andere Inhalt sei Teil der Reaktion 
- Anzeigen von Werbung, die nicht von Microsoft bereitgestellt wird, auf einer Seite, auf der ein Teil einer Reaktion angezeigt wird 
- Anzeigen von Werbung mit Reaktionen (i) aus den Bing-APIs der Bildsuche, der News-Suche, der Videosuche oder der thematischen Suche oder (ii), die hauptsächlich (oder ausschließlich) auf Bild-, News- und/oder Video- oder thematische Ergebnisse gefiltert oder eingeschränkt wurden

### <a name="notices-and-branding"></a>Hinweise und Branding 

- Schließen Sie nahe jedem Punkt auf der Benutzeroberfläche, der einem Benutzer die Möglichkeit zum Eingen einer Suchanfrage bietet, an hervorgehobener Stelle einen funktionsfähigen Link zur [Datenschutzerklärung von Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839) ein. Bezeichnen Sie den Link als **Datenschutzerklärung von Microsoft**.
- Stellen Sie nahe jedem Punkt auf der Benutzeroberfläche, die einem Benutzer die Möglichkeit zum Einengen einer Suchanfrage bietet, Bing-Branding an hervorgehobener Stelle in Übereinstimmung mit den [Richtlinien für die Bing-Markennutzung](https://go.microsoft.com/fwlink/?linkid=833278) dar. Dieses Branding muss für den Benutzer eindeutig kennzeichnen, dass die Internetsuche von Microsoft bereitgestellt wird.
- Sie können jede angezeigte Reaktion (oder einen Teil einer Reaktion) der Bing-Websuche-API, der Bing-Bildersuche-API, der Bing-News-Suche-API, der Bing-Videosuche-API und der Bing-API für thematische Suche Microsoft zuschreiben, sofern Microsoft nicht schriftlich andere Angaben für Ihre Nutzung gemacht hat. Dies ist in den [Richtlinien für die Bing-Markennutzung](https://go.microsoft.com/fwlink/?linkid=833278) beschrieben. 
- Schreiben Sie angezeigte Reaktionen (oder Teile von Reaktionen) der API für die benutzerdefinierte Bing-Suche nicht Microsoft zu, sofern Microsoft nicht schriftlich andere Angaben speziell für Ihre Nutzung gemacht hat.

### <a name="transferring-responses"></a>Weiterleiten von Reaktionen

Wenn Sie einem Benutzer die Weiterleitung einer Reaktion von einer Such-API an einen anderen Benutzer ermöglichen (etwa über eine Messaging-App oder über einen Beitrag in sozialen Medien), gilt Folgendes: 
- Weitergeleitete Reaktionen müssen folgende Anforderungen erfüllen:
  - Der Inhalt muss exakt dem Inhalt entsprechen, der dem weiterleitenden Benutzer angezeigt wurde. Die Formatierung darf geändert werden.
  - Es dürfen keine Daten in Metadatenform enthalten sein.
  - Bei Reaktionen von der Bing-Websuche-API, der Bing-Bildersuche-API, der Bing-News-Suche-API, der Bing-Videosuche-API und der Bing-API für thematische Suche muss ein Text angezeigt werden, der angibt, dass die Reaktion über eine durch Bing bereitgestellte Internetsuche erhalten wurde. Beispielsweise können Sie einen Text wie „Unterstützt durch Bing“ oder „Weitere Informationen über dieses Bild bei Bing“ anzeigen, oder Sie können das Bing-Logo verwenden.
  - Bei Reaktionen der API für die benutzerdefinierte Bing-Suche muss ein Text angezeigt werden, der angibt, dass die Reaktion über eine Internetsuche erhalten wurde. Beispielsweise können Sie einen Text wie „Weitere Informationen über dieses Suchergebnis“ anzeigen.
  - Sie müssen gut sichtbar die vollständige Abfrage anzeigen, die zum Generieren der Reaktion verwendet wurde.
  - Sie müssen einen gut sichtbaren Link oder eine ähnliche Nennung der Quelle enthalten, die der Reaktion zugrunde liegt, entweder direkt oder über die Suchmaschine (bing.com, m.bing.com oder Ihre benutzerdefinierte Suchmaschine).
- Die Weiterleitung von Reaktionen darf nicht automatisiert werden. Eine Weiterleitung muss durch eine Benutzeraktion initiiert werden, die eindeutig auf die Absicht zurückzuführen ist, eine Reaktion weiterzuleiten.
- Einem Benutzer darf nur die Weiterleitung von Reaktionen ermöglicht werden, die als Antwort auf die Abfrage des weiterleitenden Benutzers angezeigt wurden.

### <a name="continuity-of-service"></a>Dienstkontinuität 

Daten aus Reaktionen von Such-APIs dürfen nicht kopiert, gespeichert oder zwischengespeichert werden. Zur Gewährleistung der Kontinuität beim Dienstzugriff und beim Rendern von Daten ist es unter folgenden Bedingungen jedoch gestattet, Ergebnisse aufzubewahren:

**Gerät.** Sie können einem Benutzer die Aufbewahrung von Ergebnissen auf einem Gerät für den kürzeren der beiden folgenden Zeiträume ermöglichen: (i) 24 Stunden ab dem Zeitpunkt der Abfrage oder (ii) bis ein Benutzer eine weitere Abfrage für aktualisierte Ergebnisse übermittelt – vorausgesetzt, die aufbewahrten Ergebnisse werden ausschließlich zu folgenden Zwecken verwendet:

- Um dem Benutzer Zugriff auf Ergebnisse zu ermöglichen, die zuvor an diesen Benutzer auf diesem Gerät zurückgegeben wurden (etwa im Fall einer Dienstunterbrechung)
- Zur Speicherung der zurückgegebenen Ergebnisse für Ihre proaktive Abfrage, die in Erwartung des Benutzerbedarfs auf der Grundlage der Signale des Benutzers personalisiert wurde (etwa bei einer erwarteten Dienstunterbrechung)

**Server.** Spezifische Ergebnisse für einen einzelnen Benutzer dürfen sicher auf einem von Ihnen kontrollierten Server aufbewahrt werden, und die aufbewahrten Ergebnisse dürfen angezeigt werden, allerdings ausschließlich zu folgenden Zwecken:

- Um dem Benutzer einen Verlaufsbericht der dem Benutzer in Ihrer Lösung zuvor zurückgegebenen Ergebnisse zur Verfügung zu stellen. Die Ergebnisse dürfen nicht (i) für mehr als 21 Tage ab dem Zeitpunkt der ursprünglichen Abfrage des Endbenutzers beibehalten und (ii) als Antwort auf eine neue oder wiederholte Abfrage eines Benutzers angezeigt werden.
- Zur Speicherung der zurückgegebenen Ergebnisse für Ihre proaktive Abfrage, die in Erwartung des Benutzerbedarfs auf der Grundlage der Signale des Benutzers personalisiert wurde. Sie können diese Ergebnisse für den kürzeren der beiden folgenden Zeiträume ermöglichen: (i) 24 Stunden ab dem Zeitpunkt der Abfrage oder (ii) bis ein Benutzer eine weitere Abfrage für aktualisierte Ergebnisse übermittelt.

Wenn die Ergebnisse für einen bestimmten Benutzer beibehalten werden, dürfen sie nicht mit Ergebnissen für einen anderen Benutzer vermischt werden. Die Ergebnisse müssen also für jeden Benutzer separat beibehalten und übermittelt werden.

### <a name="general"></a>Allgemein 

Bei jeder Präsentation aufbewahrter Ergebnisse müssen folgende Anforderungen erfüllt sein:

- Der Zeitpunkt, zu dem die Abfrage gesendet wurde, muss eindeutig und gut sichtbar angegeben sein.
- Dem Benutzer muss eine Schaltfläche oder ein ähnliches Element zur Verfügung stehen, mit der bzw. dem er die Abfrage erneut ausführen und aktualisierte Ergebnisse abrufen kann. 
- Bei der Präsentation der Ergebnisse muss das Bing-Branding erhalten bleiben.
- Die gespeicherten Ergebnisse müssen innerhalb der angegebenen Fristen gelöscht (und bei Bedarf mit einer neuen Abfrage aktualisiert) werden.

### <a name="non-display-url-discovery"></a>Ermittlung von Nicht-Anzeige-URLs 

Suchreaktionen dürfen in nicht internetbasierten Suchen ausschließlich zur Ermittlung der URLs von Informationsquellen verwendet werden, bei denen es sich um eine Reaktion auf eine Abfrage Ihres Benutzers oder Kunden handelt. Sie dürfen solche URLs in einem Bericht oder einer ähnlichen Antwort kopieren, die Sie unter folgenden Umständen bereitstellen:

- Nur für diesen Benutzer oder Kunden und als Antwort auf diese Abfrage
- Nur, wenn sie einen erheblichen zusätzlichen wertvollen Inhalt enthält, der relevant für die Abfrage ist

Die vorhergehenden Abschnitte dieser Nutzungs- und Anzeigeanforderungen für Such-APIs gelten nicht für diese Nicht-Anzeige-Verwendung, jedoch mit folgenden Ausnahmen: 

- Mit Ausnahme des weiter oben beschriebenen eingeschränkten Kopierens von URLs dürfen keine Daten und keine Inhalte, die aus der Suchreaktion stammen oder daraus abgeleitet wurden, zwischengespeichert, kopiert oder gespeichert werden.
- Sie müssen sicherstellen, dass Ihre Verwendung von Daten (einschließlich URLs), die von den Such-APIs empfangen wurden, nicht gegen geltende Gesetze verstößt oder die Rechte Dritter verletzt.
- Sie dürfen die Daten (einschließlich URLs), die Sie von den Such-APIs erhalten, nicht im Rahmen einer Suchindizierung, einer Aktivität des maschinellen Lernens oder einer ähnlichen algorithmischen Aktivität verwenden. Verwenden Sie diese Daten nicht zum Erstellen, Trainieren, Auswerten oder Verbessern von Diensten, die Sie oder Dritte möglicherweise anbieten.

## <a name="gdpr-compliance"></a>Einhaltung der DSGVO  

Betreffend personenbezogenen Daten, die der Datenschutz-Grundverordnung (DSGVO) der Europäischen Union unterliegen und die im Zusammenhang mit Aufrufen an die Such-APIs, die Bing-Rechtschreibprüfungs-API oder die Bing-Vorschlagssuche-API verarbeitet werden, bestätigen Sie, dass Sie und Microsoft unabhängige Verantwortliche gemäß der DSGVO sind. Sie sind unabhängig voneinander für die Einhaltung der DSGVO verantwortlich.  

