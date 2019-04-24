---
title: Vordefinierter Domänenverweis
titleSuffix: Azure
description: Referenz für die vordefinierten Domänen, die vordefinierte Sammlungen von Absichten und Entitäten aus LUIS (Language Understanding Intelligent Services) darstellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: e1e579233a5ad1af1ef8ee84019cd995959d3b2b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57433772"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Vordefinierter Domänenverweis für die LUIS-App
Diese Referenz enthält Informationen über die [vordefinierten Domänen](luis-how-to-use-prebuilt-domains.md), bei denen es sich um vordefinierte Sammlungen der von LUIS gebotenen Absichten und Entitäten handelt.

[Benutzerdefinierte Domänen](luis-how-to-start-new-app.md) weisen im Gegensatz dazu anfänglich keine Absichten und Modelle auf. Sie können einem benutzerdefinierten Modell beliebige Absichten und Entitäten aus einer vordefinierten Domäne hinzufügen.

## <a name="list-of-prebuilt-domains"></a>Liste der vordefinierten Domänen
LUIS bietet 20 vordefinierte Domänen. 

| Vordefinierte Domäne | BESCHREIBUNG | Unterstützte Sprachen |
| ---------------- |-----------------------|:------:|
| Kalender | Die Kalenderdomäne stellt Absichten und Entitäten zum Hinzufügen, Löschen oder Bearbeiten von Terminen, Überprüfen der Verfügbarkeit von Teilnehmern und Suchen von Informationen über ein Kalenderereignis bereit.| en-US<br/> zh-CN |
| Camera | In der Kameradomäne stehen Absichten und Entitäten zum Erfassen von Fotos, zum Aufzeichnen von Videoclips und zum Übertragen von Videoclips an eine Anwendung zur Verfügung.| en-US |
| Kommunikation | Senden von Nachrichten und Tätigen von Telefonanrufen.| en-US <br/> zh-CN |
| Entertainment  | Behandlung von Abfragen im Zusammenhang mit Musik, Videos und TV.| en-US |
| Ereignisse | Buchung von Tickets für Konzerte, Festivals, Sportveranstaltungen und Comedyaufführungen.| en-US |
| Fitness | Behandlung von Anforderungen, die sich auf das Nachverfolgen von Fitnessaktivitäten beziehen.| en-US |
| Spiele | Behandlung von Anforderungen im Zusammenhang mit einer Spielparty in einem Multiplayer-Spiel.| en-US |
| HomeAutomation | Steuerung intelligenter Geräte zu Hause, wie Leuchten und Haushaltsgeräten.| en-US<br/> zh-CN |
| MovieTickets | Buchung von Tickets für Filme in einem Kino.| en-US |
| Musik | Wiedergeben von Musik auf einem Musikabspielgerät.| en-US<br/> zh-CN |
| Hinweis | Die Note-Domäne bietet Absichten und Entitäten im Zusammenhang mit dem Erstellen, Bearbeiten und Suchen von Notizen.| en-US<br/> zh-CN |
| OnDevice | Die OnDevice-Domäne stellt Absichten und Entitäten bereit, die sich auf die Steuerung des Geräts beziehen.| en-US<br/> zh-CN |
| Orte  | Behandlung von Abfragen im Zusammenhang mit Orten wie Geschäften, Institutionen, Restaurants, öffentlichen Räumen und Adressen.| en-US<br/> zh-CN |
| Reminder | Behandlung von Anforderungen, die sich auf das Erstellen, Bearbeiten und Suchen von Erinnerungen beziehen.| en-US<br/> zh-CN |
| RestaurantReservation | Behandlung von Anforderungen zum Verwalten von Restaurantreservierungen.| en-US<br/> zh-CN |
| Taxi | Verarbeitung von Taxibuchungen.| en-US<br/> zh-CN |
| Translate | Übersetzen von Text in eine Zielsprache.| en-US<br/> zh-CN |
| TV | Steuerung von Fernsehgeräten.| en-US |
| Versorgungsunternehmen  | Behandlung von Anforderungen, die in vielen Domänen häufig sind, wie etwa „Hilfe“, „Wiederholen“, „Von vorn beginnen“:| en-US |
| Weather | Abrufen von Wetterberichten und -vorhersagen.| en-US<br/> zh-CN |
| Web | Navigieren zu einer Website.| en-US<br/> zh-CN |

Weitere Einzelheiten zu den einzelnen Domänen finden Sie in den folgenden Abschnitten.

## <a name="calendar"></a>Kalender 

Die Calendar-Domäne enthält Absichten und Entitäten im Zusammenhang mit Kalendereinträgen. Zu den Absichten der Calendar-Domäne gehören das Hinzufügen, Löschen oder Bearbeiten von Terminen, das Überprüfen der Verfügbarkeit und das Suchen von Informationen zu einem Kalender oder Termin.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Hinzufügen | Hinzufügen eines neuen einmaligen Elements zum Kalender| Mache einen Termin mit Lisa am Sonntag um 14:00 Uhr. <br/><br/>Ich möchte eine Besprechung planen.<br/><br/>Ich muss eine Besprechung einrichten.|
| CheckAvailability | Prüfen der Verfügbarkeit für einen Termin oder eine Besprechung im Kalender des Benutzers oder einer anderen Person| Wann ist Klaus für eine Besprechung verfügbar? <br/><br/>Zeige, wann Helga morgen verfügbar ist.<br/><br/>Ist Chris am Samstag frei?|
| Löschen | Anforderung der Löschung eines Kalendereintrags| Storniere meinen Termin mit Helga. <br/><br/>Lösche meine Besprechung um 9 Uhr.<br/>|
| Edit (Bearbeiten) | Anforderung zum Ändern einer vorhandenen Besprechung oder eines Kalendereintrags| Verschiebe meine Besprechung von 9 Uhr auf 10 Uhr.<br/><br/>Ich möchte meinen Zeitplan aktualisieren.<br/><br/>Plane meine Besprechung mit Lorenz neu.|
| Suchen | Meinen Wochenkalender anzeigen| Suche den Termin für die Nachuntersuchung beim Zahnarzt. <br/><br/>Zeige meinen Kalender an.<br/>|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Standort | Ort des Kalenderelements, der Besprechung oder des Termins Adressen, Städte und Regionen sind gute Beispiele für Speicherorte.| Fitnesscenter Detmold <br/><br/>Pfannkuchenwelt<br/><br/>Garage|
| Antragsteller | Der Titel einer Besprechung oder eines Termins| Zahnarzttermin <br/><br/>Mittagessen mit Jule<br/><br/>Arzttermin|

## <a name="camera"></a>Camera 
Die Camera-Domäne enthält Absichten und Entitäten, die sich auf das Verwenden einer Kamera beziehen. Die Absichten reichen vom Aufnehmen eines Fotos, Selfies, Screenshots oder Videoclips bis zum Übertragen eines Videoclips an eine Anwendung.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| CapturePhoto| Aufnehmen eines Fotos| Foto aufnehmen<br/><br/>fotografieren|
| CaptureScreenshot | Erfassen eines Screenshots| Screenshot aufnehmen<br/><br/>Bildschirm erfassen|
| CaptureSelfie | Aufnehmen eines Selfies| Selfie aufnehmen <br/><br/>ein Foto von mir machen |
| CaptureVideo | Starten der Aufzeichnung eines Videoclips| Aufzeichnung starten <br/><br/>Aufzeichnung beginnen|
| StartBroadcasting| Starten einer Videoübertragung| Übertragung nach Facebook starten|
| StopBroadcasting| Beenden einer Videoübertragung.| Übertragung beenden|
| StopVideoRecording| Beenden der Aufzeichnung eines Videos| Das reicht<br/><br/>Aufzeichnung beenden|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AppName | Der Name einer Anwendung, an die das Video übertragen werden soll.| OneNote<br/><br/>Facebook<br/><br/>Skype|


## <a name="communication"></a>Kommunikation 
Die Communication-Domäne stellt Absichten und Entitäten im Zusammenhang mit E-Mail-Nachrichten und Telefonanrufen bereit.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AddContact| Hinzufügen eines neuen Kontakts zur Kontaktliste des Benutzers|Neuen Kontakt hinzufügen <br/><br/>Speichere diese Nummer unter dem Namen „Helga“.|
| AddMore| Fügen Sie im Rahmen des schrittweisen Verfassens einer E-Mail oder eines Texts mehr hinzu.|Mehr zu Text hinzufügen <br/><br/>Mehr zu E-Mail-Text hinzufügen|
| Antwort| Beantworten eines eingehenden Telefonanrufs|Antwort beantworten <br/><br/>Abheben|
| AssignContactNickname| Hinzufügen eines Spitznamens zu einem Kontakt|„Isaac“ in „Vati“ ändern <br/>Klaus Spitznamen bearbeiten<br/>Spitzname zu Leni Martin hinzufügen|
| CallVoiceMail| Verbinden mit der Voicemail des Benutzers|Verbinde mich mit meiner Voicemailbox <br/>Voicemail<br/>Voicemail anrufen|
| CheckIMStatus| Überprüfen des Status eines Kontakts in Skype|Ist Klaus Onlinestatus auf „abwesend“ festgelegt? <br/>Ist Helga zum Chatten verfügbar?|
| Confirm| Bestätigen einer Aktion.|Ja<br/>Okay<br/>In Ordnung<br/>Ich bestätige, dass ich diese E-Mail senden möchte.<br/>|
| Dial| Tätigen eines Telefonanrufs.|Klaus anrufen<br/>Bitte wähle 311<br/>|
| FindContact| Suchen von Kontaktinformationen anhand des Namens|Helgas Nummer suchen<br/>Zeige mir Helgas Nummer.<br/>|
| FindSpeedDial| Suchen der Kurzwahl, die für eine Telefonnummer festgelegt ist und umgekehrt.|Was ist meine Kurzwahl 5?<br/>Habe ich eine Kurzwahl festgelegt?<br/>Was ist die Kurzwahl für 94 15 55 53 33?|
| GetForwardingsStatus| Abrufen des aktuellen Status der Anrufweiterleitung|Ist meine Anrufweiterleitung aktiviert?<br/>Sag mir, ob meine Anrufweiterleitung an oder aus ist<br/>|
| GoBack| Zurück zur vorherigen Seite wechseln|Gehe zu Twitter zurück.<br/>Gehe einen Schritt zurück.<br/>Zurück navigieren|
| Ignorieren| Ignorieren eines eingehenden Anrufs|Nicht beantworten<br/>Anruf ignorieren|
| IgnoreWithMessage| Ignorieren eines eingehenden Anrufs, stattdessen mit Text antworten.|Beantworte diesen Anruf nicht, sende stattdessen eine Textnachricht.<br/>Ignorieren und Text zurückschicken|
| PressKey| Drücken einer Taste oder Ziffer auf der Zehnertastatur.|Wähle die Sterntaste.<br/>Drücke 1 2 3.|
| ReadAloud| Lautes Vorlesen einer Nachricht oder E-Mail für den Benutzer.|Lies Text.<br/>Was hat sie in der Nachricht gesagt?|
| TurnForwardingOff| Tätigen eines Telefonanrufs|<br/><br/>|
| Redial| Erneut wählen oder Nummer noch einmal wählen|Erneut wählen<br/>Wähle meinen letzten Anrufpartner noch einmal an.|
| Reject| Abweisen eines eingehenden Anrufs.|Aufruf abweisen<br/>Ich kann jetzt nicht antworten.<br/>Ich bin im Moment nicht zu sprechen und komme später wieder.|
| SendEmail| Senden von E-Mail. Diese Absicht betrifft E-Mail, aber keine Textnachrichten.|Email to Mike Waters: Mike, that dinner last week was splendid (E-Mail an Mike Waters: Mike, das Essen letzte Woche war hervorragend)<br/>Sende eine E-Mail an Tom<br/>|
| SendMessage| Senden einer Textnachricht oder Chatnachricht|Sende diesen Text an Chris und Helga.|
| SetSpeedDial| Festlegen einer Kurzwahl für die Telefonnummer eines Kontakts|Lege Kurzwahl eins für Helga fest.<br/>Richte Kurzwahl für Mami ein.|
| ShowNext| Anzeigen des nächsten Elements beispielsweise in einer Liste von Textnachrichten oder E-Mails|Nächstes anzeigen<br/>Gehe zur nächsten Seite.|
| ShowPrevious| Anzeigen des vorherigen Elements, beispielsweise in einer Liste mit Textnachrichten oder E-Mails|Zeige den letzten an.<br/>Zurück<br/>Gehe zum Vorherigen.|
| StartOver| Nochmals von vorn anfangen oder neue Sitzung starten|Von vorn beginnen<br/>Neue Sitzung<br/>restart|
| TurnForwardingOff| Deaktivieren der Anrufweiterleitung|Meine Anrufe nicht mehr weiterleiten<br/>Anrufweiterleitung ausschalten|
| TurnForwardingOn| Anrufweiterleitung einschalten|Leite meine Anrufe an 3333 weiter.<br/>Schalte Anrufweiterleitung an 3333 ein.|
| TurnSpeakerOff| Freisprecheinrichtung ausschalten|Lautsprecher ausschalten<br/>Schalte den Lautsprecher aus.<br/>|
| TurnSpeakerOn| Schalte den Lautsprecher ein.|Freisprechmodus<br/>Schalte den Lautsprecher ein.<br/>|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AudioDeviceType | Typ des Audiogeräts (Lautsprecher, Headset, Mikrofon usw.).| Referent<br/>Freihändig<br/>Bluetooth|
| Category (Kategorie) | Die Kategorie einer Nachricht oder E-Mail.| Wichtig<br/>Hohe Priorität|
| ContactAttribute | Ein Attribut des Kontakts, über den sich der Benutzer erkundigt.| Geburtstage<br/>Adresse<br/>Telefonnummer|
| ContactName | Der Name eines Kontakts oder Nachrichtenempfängers| Helga<br/>Klaus<br/>Chris|
| EmailSubject | Der Text, der als Betreffzeile für eine E-Mail-Nachricht verwendet wird| AW: Interessante Geschichte|
| Liniendiagramm | Die Leitung, die der Benutzer verwenden möchte, um einen Anruf zu tätigen oder eine SMS/E-Mail zu senden.| Büroleitung<br/>D-Netz<br/>Skype|
| Message | Die Nachricht, die als E-Mail oder SMS gesendet werden soll.| Es hat mich sehr gefreut, Sie heute zu treffen. Sehen Sie bald wieder rein!|
| MessageType | Der Name eines Kontakts oder Nachrichtenempfängers| Text<br/>E-Mail|
| OrderReference | Die Ordnungszahl oder relative Position in einer Liste, die ein abzurufendes Element angibt. Beispiel: „Letzte“ oder „vor Kurzem“ in „Welche Nachricht habe ich als letzte gesendet?“| Last (Letzter)<br/>Vor Kurzem|
| SenderName | Der Name des Absenders.| Leni Schneider|

## <a name="entertainment"></a>Entertainment  
Die Entertainment-Domäne enthält Absichten und Entitäten, die sich auf die Suche nach Filmen, Musik, Spielen und Fernsehsendungen beziehen.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Suchen,| Suche nach Filmen, Musik, Apps, Spielen und Fernsehsendungen.|Suche im Store nach „Halo“.<br/>Suche nach „Avatar“.|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| ContentRating | Medieninhaltsbewertung, wie FSK 0 oder 18 für Filme.|Kindervideo.<br/>Freigegeben ab 12.|
| Genre | Das Genre eines Films, Spiels, einer App oder eines Musiktitels.|Komödien<br/>Drama<br/>Lustig|
| Schlüsselwort| Ein allgemeines Stichwort für die Suche, das ein Attribut angibt, das in den spezifischeren Medienkategorien nicht vorkommt.|Soundtracks<br/>Moon River<br/>Elly Beinhorn|
| Sprache | Sprache, die in den Medien verwendet wird, z. B. gesprochene Sprache in Filmen oder Liedern.|Französisch<br/>Englisch<br/>Koreanisch|
| MediaFormat | Die Zusatzangabe des technischen Typs des gesuchten Medienformats.|HD-Filme<br/>3D-Filme<br/>Herunterladbar|
| MediaSource | Der Store oder Marketplace, in denen das Medium erworben werden kann.|Netflix<br/>Prime|
| MediaSubTypes| Medientypen unterhalb von Filmen und Spielen.|Demos<br/>DLC<br/>Trailer|
| Nationality| Das Land, in dem ein Film, eine Sendung oder ein Musiktitel erstellt wurde.|Französisch<br/>Deutsch<br/>Koreanisch|
| Person| Der Schauspieler, Regisseur, Produzent, Musiker oder Interpret eines Films, einer App, eines Spiels oder einer Fernsehsendung.|Madonna<br/>Stanley Kubrick|
| Rolle| Die Rolle, die eine Person bei der Erstellung des Mediums gespielt hat.|Singt<br/>Regie<br/>nach|
| Titel| Der Name eines Films, einer App, eines Spiels, einer Fernsehsendung oder eines Musiktitels.|Friends<br/>Minecraft|
| Type| Der Typ oder das Medienformat eines Films, einer App, eines Spiels, einer Fernsehsendung oder eines Musiktitels.|Musik<br/>MovieTV <br/>Show|
| UserRating| Benutzerbewertung mit Stern oder Daumen.|5 Sterne<br/>3 Sterne<br/>4 Sterne|

## <a name="events"></a>Ereignisse 
Die Events-Domäne enthält Absichten und Entitäten zum Buchen von Tickets für Veranstaltungen, wie Konzerte, Festivals, Sportveranstaltungen und Comedy Shows.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Buch| Kauf von Tickets für eine Veranstaltung.|Ich möchte eine Karte für die Symphonie an diesem Wochenende kaufen.|


### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Adresse | Ort oder Adresse der Veranstaltung. |Garmisch-Partenkirchen<br/>Südstr. 12 <br/> Seattle |
| NAME | Der Name einer Veranstaltung.|Ruhrtriennale|
| PlaceName| Der Name des Veranstaltungsorts.|Louvre<br/>Opernhaus<br/>Museumsmeile|
| PlaceType | Der Typ des Orts, an dem die Veranstaltung stattfindet.|Cafe<br/>Kino<br/>Bibliothek|
| Type | Der Typ einer Veranstaltung.|Konzert<br/>Sportveranstaltung|

## <a name="fitness"></a>Fitness 
Die Fitness-Domäne bietet Absichten und Entitäten für die Nachverfolgung von Fitnessaktivitäten. Zu den Absichten gehören das Speichern von Anmerkungen, die verbleibende Zeit oder Strecke oder das Speichern von Aktivitätsergebnissen.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AddNote| Fügt einer nachverfolgten Aktivität ergänzende Anmerkungen hinzu.|Der Schwierigkeitsgrad dieses Laufs war 6/10<br/>Der Geländetyp für den Lauf ist Asphalt<br/>Ich verwende ein Rad mit drei Gängen|
|GetRemaining| Ruft die verbleibende Zeit oder Strecke für eine Aktivität ab.|Wieviel Zeit bis zur nächsten Runde?<br/>Wie viele Kilometer bis zum Ziel? Wieviel Zeit habe ich für den Spagat?|
| LogActivity| Speichern oder Protokollieren der Ergebnisse von abgeschlossenen Aktivitäten.|Speichere meinen letzten Lauf<br/>Erfasse meinen Spaziergang am Samstagmorgen<br/>Speichere meine letzte Schwimmrunde|
| LogWeight| Speichern oder Protokollieren des aktuellen Gewichts des Benutzers.|Speichere mein aktuelles Gewicht<br/>Erfasse jetzt mein Gewicht<br/>Speichere mein derzeitiges Gewicht|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| ActivityType | Der Typ der nachzuverfolgenden Aktivität. |Ausführen<br/>Gehen<br/>Schwimmen<br/>Zyklus |
| Food | Ein Nahrungsmittel zur Nachverfolgung in einer Fitness-App. |Banana<br/>Lachs<br/>Eiweiß-Shake|
| MealType| Die Art der Mahlzeit zur Erfassung in einer Gesundheits- oder Fitness-App.|Frühstück<br/>Abendessen<br/>Mittagessen<br/>Imbiss|
| Messung| Ein Typ von Messungen von Zeit, Entfernung oder Gewicht für die Verwendung in einer Gesundheits- oder Fitness-App.|Kilometer<br/>Meilen<br/>Minuten<br/>Kilogramm|
| Number | Eine numerische Mengenangabe zur Verwendung in einer Gesundheits- oder Fitness-App.|19<br/>three<br/>200<br/>one|
| StatType | Ein Statistiktyp für zusammengefasste Daten für die Verwendung in einer Fitness- oder Gesundheits-App.|Summe<br/>Durchschnitt<br/>Maximum<br/>Minimum|

## <a name="gaming"></a>Spiele 
Die Gaming-Domäne stellt Absichten und Entitäten zum Verwalten einer Spielparty in einem Multiplayer-Spiel bereit.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| InviteParty| Laden Sie einen Kontakt zur Teilnahme an einer Spielparty ein.|Lade diese Spieler zu meiner Party ein<br/>Kommt zu meiner Party<br/>Werde Teil meines Clans|
|LeaveParty| Ruft die verbleibende Zeit oder Strecke für eine Aktivität ab.|Ich bin draußen<br/>Ich verlasse diese Party für eine andere.<br/>Ich gehe|
| StartParty| Starten einer Spielparty in einem Multiplayer-Spiel.|Alter, lass uns eine Party schmeißen<br/>Party schmeißen<br/>Sollen wir heute Nacht einen Clan bilden|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Kontakt| Der Name eines Kontakts für die Verwendung in einem Multiplayer-Spiel.|Helga<br/>Klaus|


## <a name="homeautomation"></a>HomeAutomation 
Die HomeAutomation-Domäne stellt Absichten und Entitäten zum Steuern von Geräten im Smarthome bereit, wie Leuchten und Haushaltsgeräten.

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| TurnOff| Ausschalten, Schließen oder Entsperren eines Geräts.|Mach das Licht aus<br/>Schalte die Kaffeemaschine aus<br/>Schließ das Garagentor|
|TurnOn| Einschalten eines Geräts oder Versetzen des Geräts in eine bestimmte Einstellung oder einen bestimmten Modus.|Schalte die Kaffeemaschine ein<br/>Kannst du die Kaffeemaschine einschalten?<br/>Stelle den Thermostaten auf 22 Grad ein.|


### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Gerät | Eine Art von Gerät, die ein- oder ausgeschaltet werden kann.|Kaffeemaschine<br/>Thermostat<br/>Leuchten|
| Vorgang | Der Status, der für das Gerät festgelegt werden soll.|Sperre<br/>öffnen<br/>on<br/>aus|
| Raum | Der Ort oder Raum, in dem sich das Gerät befindet.|Wohnzimmer<br/>Schlafzimmer<br/>Küche|

## <a name="movietickets"></a>MovieTickets 
Die MovieTickets-Domäne stellt Absichten und Entitäten für das Buchen von Tickets für Filme in einem Kino bereit.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Buche mir zwei Karten für Captain Omar und die zwei Musketiere|
|Tickets stornieren|
|Wann wird Captain Omar gezeigt?|

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Buch | Kauf von Filmtickets.|Buche mir zwei Karten für Captain Omar und die zwei Musketiere<br/>Ich möchte ein Ticket für den Film Morgen kaufen<br/>Ich möchte ein Ticket für Captain Omar Teil 2 am nächsten Mittwoch|
|GetShowTime| Abrufen der Vorstellungszeit eines Films.|Wann wird Captain Omar gezeigt?|


### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Adresse | Die Adresse eines Kinos.|Garmisch-Partenkirchen<br/>Südstr. 12<br/>Seattle|
| MovieTitle | Der Filmtitel.|Life of Pi<br/>Hunger Games<br/>Inception|
| PlaceName | Der Name eines Kinos oder Multiplexes.|Off-Broadway<br/>Savoy<br/>Lupe2|
| PlaceType | Die Art von Ort, an dem ein Film gezeigt wird.|Kino<br/>Theater<br/>IMAX-Kino|

## <a name="music"></a>Music 
Die Music-Domäne stellt Absichten und Entitäten für das Abspielen von Musik auf einem Musikabspielgerät bereit.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Beethoven abspielen|
|Titel lauter stellen|
|Zum nächsten Titel springen|

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| DecreaseVolume | Verringern der Gerätelautstärke.|Titel leiser stellen<br/>Leiser drehen|
| IncreaseVolume | Erhöhen der Gerätelautstärke.|Titel lauter stellen<br/>Lauter drehen|
| Mute |Musikwiedergabe stumm schalten.|Musiktitel stumm schalten<br/>Titel stumm schalten<br/>Ton aus |
| Pause | Anhalten der wiedergegebenen Musik.|Pause<br/>Musik anhalten<br/>Titel anhalten|
| PlayMusic | Musik auf einem Gerät wiedergeben.|Kevin Durant spielen<br/>Paradise von Coldplay spielen<br/>Hello von Adele spielen|
| Repeat |Die wiedergegebene Musik wiederholen.|Stück wiederholen<br/>Musiktitel noch einmal wiedergeben<br/>Musik wiederholen|
| Fortfahren | Fortsetzen der Musikwiedergabe.|Titel fortsetzen<br/>Musik wieder starten<br/>Pause Ende|
| SkipBack | Einen Titel zurück springen.|Zum nächsten Titel springen<br/>Nächsten Titel wiedergeben|
| SkipForward |Einen Titel nach vorn springen.|Vorhergehenden Titel wiedergeben<br/>Letzten Titel wiedergeben |
| Beenden | Beenden einer Aktion für die Musikwiedergabe. |Wiedergabe dieses Albums beenden.|
| Unmute | Stummschaltung eines Musikwiedergabegeräts aufheben.| Stummschaltung aufheben|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| ArtistName | Der Schauspieler, Regisseur, Produzent, Autor, Musiker oder Interpret eines Mediums, das auf einem Gerät wiedergegeben werden soll.|Elvis Presley<br/>Taylor Swift<br/>Adele<br/>Mozart|
| Genre | Das Genre der angeforderten Musik.|Country-Musik<br/>Musical-Klassiker<br/>Spiele meine Lieblingsstücke aus dem Barock|

## <a name="note"></a>Hinweis 
Die Note-Domäne bietet Absichten und Entitäten im Zusammenhang mit dem Erstellen, Bearbeiten und Suchen von Notizen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Zu meiner Einkaufsliste hinzufügen: Kopfsalat, Tomaten, Brot, Kaffee|
|Bananen in meiner Einkaufsliste abhaken|
|Entferne alle Elemente aus meiner Urlaubsliste|

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AddToNote | Hinzufügen von Informationen zu einer Notiz.|Zu meiner Einkaufsliste hinzufügen: Kopfsalat, Tomaten, Brot, Kaffee<br/>Zu meiner Aufgabenliste hinzufügen<br/>Cupcakes zu meiner Wunderlist hinzufügen|
| CheckOffItem | Abhaken von Elementen in einer bereits vorhandenen Notiz.|Bananen in meiner Einkaufsliste abhaken<br/>Käsekuchen in meiner Feiertags-Einkaufsliste als erledigt markieren|
| Clear | Löschen aller Elemente in einer bereits vorhandenen Notiz.|Entferne alle Elemente aus meiner Urlaubsliste<br/>Lösche alle Einträge in meiner Leseliste|
| Confirm | Bestätigen einer Aktion im Zusammenhang mit einer Notiz.|Ich bin einverstanden.<br/>Ja<br/>Ich bestätige, dass alle Elemente in Listen erhalten bleiben.|
| Erstellen | Erstellen einer neuen Notiz. | Erstelle eine Liste.<br/>Notiz zur Erinnerung, dass Tyge in der ersten Maiwoche in der Stadt ist|
| Löschen | Löschen einer gesamten Notiz. |Meine Urlaubsliste löschen <br/>Lösche meine Einkaufsliste|
| DeleteNoteItem | Löschen eines Elements in einer bereits vorhandenen Notiz.| Lösche Chips aus meiner Einkaufsliste<br/>Entferne Stifte aus meiner Schulbedarfsliste|
| ReadAloud | Sprachausgabe einer Liste.|Lies mit den ersten vor<br/>Lies mir die Details vor|
| ShowNext | Anzeigen des nächsten Elements in einer Notizenliste|Nächstes anzeigen.<br/>Nächste Seite<br/>Next (Weiter)|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AppName | Der Name der Anwendung zur Notizenerfassung.|Wunderlist<br/>OneNote|
| ContactName | Der Name eines Kontakts in einer Notiz|Helga<br/>Klaus<br/>Chris|
| DataSource | Speicherort der Notizen|OneDrive<br/>Google-Dokumente<br/>Arbeitsplatz|
| DataType | Der Typ der Datei oder des Dokuments, normalerweise bestimmten Softwareprogrammen zugeordnet.|Folien<br/>Spreadsheet<br/>Worksheet|
| Text | Der Text einer Notiz oder Erinnerung.|Dehnen vor dem Lauf<br/>Morgen Langlauf|
| Titel | Der Titel einer Notiz.|Lebensmittel<br/>Anzurufende Personen<br/>Aufgabe|

## <a name="ondevice"></a>OnDevice 
Die OnDevice-Domäne stellt Absichten und Entitäten bereit, die sich auf die Steuerung des Geräts beziehen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Videoplayer schließen|
|Wiedergabe abbrechen|
|Machst du den Bildschirm heller?|


### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AreYouListening | Fragt, ob das Gerät hört.|Ist es an?<br/>Hörst Du?|
|CloseApplication|Schließen der Geräteanwendung.|Videoplayer schließen|
|FileBug|Protokollieren eines Programmfehlers auf dem Gerät.|Bitte melde einen Fehler<br/>Kannst du einen Bug für mich festhalten?<br/>Ich möchte diesen Fehler zu melden|
|GoBack|Bitte, einen Schritt zurück zu gehen oder zum vorhergehenden Schritt zurückzukehren.|Bitte einen Schritt zurück<br/>Zurück zum letzten Bildschirm<br/>Zurück, nicht mehr zuhören|
|Hilfe| Anfordern von Hilfe|Hilf mir bitte<br/>Hallo<br/>Was können Sie tun?<br/>Ich brauche Hilfe| 
|LocateDevice|Aufspüren des Geräts.|Kannst du mein Handy suchen<br/>Suche Toms iPhone<br/>Suche mein Handy|
|LogIn|Anmelden bei einem Dienst mithilfe des Geräts.|Anmelden, bitte<br/>Facebook-Anmeldung<br/>Bei LinkedIn anmelden|
|LogOut|Abmelden bei einem Dienst mithilfe des Geräts.|Melde mein Handy ab<br/>Abmeldung bei Twitter<br/>Abmelden|
|MainMenu|Anzeigen des Hauptmenüs eines Geräts.|Menü anzeigen|
|OpenApplication|Öffnen einer Anwendung auf dem Gerät.|Bitte öffne den Alarm<br/>Schalte die Kamera ein<br/>Kalender öffnen|
|OpenSetting|Öffnen einer Einstellung auf dem Gerät|Netzwerkeinstellungen öffnen|
|PairDevice|Koppeln des Geräts.|Kannst du mir helfen, das Bluetooth-Signal mit dem Handy zu koppeln<br/>Schalte Bluetooth ein und kopple es mit dem Laptop<br/>Kopple das Bluetooth-Signal mit meinem Laptop|
|PowerOff | Ausschalten des Geräts|Bitte fahre den Computer herunterfahren<br/>Shutdown<br/>Schalte mein Handy aus|
|QueryBattery|Informationen über die Akkulaufzeit abrufen.|Zeige mir den Akkustand.<br/>Wie ist die Akkuladung<br/>Wie lange reicht der Akku noch?<br/>Batterie anzeigen|
|QueryWifi|Informationen über WLAN abrufen|WLAN-Informationen abrufen|
|Neu starten|Neustart des Geräts|Bitte neu starten|
|RingDevice| Gerät zum Läuten auffordern, um es bei Verlust zu finden.|Lass mein Handy klingeln.| 
|SetBrightness|Gerätehelligkeit festlegen.|Helligkeit auf mittel festlegen<br/>Helligkeit auf hoch festlegen<br/>Helligkeit auf niedrig festlegen|
|SetupDevice|Setup des Geräts einrichten|Ich möchte das Betriebssystem installieren<br/>Bitte einrichten<br/>Mach das Setup für mich|
|ShowAppBar|Anzeigen einer Anwendungsleiste.|Zeige mit die Anwendungsleiste<br/>Anwendungsleiste bitte<br/>Ich möchte die Anwendungsleiste sehen|
|ShowContextMenu|Anzeigen eines Kontextmenüs.|Ich möchte das Kontextmenü sehen<br/>Bitte Kontextmenü<br/>Kannst du mir das Kontextmenü anzeigen?|
|Sleep|Versetzen des Geräts in den Energiesparmodus.|Energie sparen<br/>Standby<br/>Computer-Standby|
|SwitchApplication|Wechseln der auf dem Gerät zu verwendenden Anwendung|Schalte auf den Medienplayer um|
|TurnDownBrightness|Verringern der Gerätehelligkeit.|Bildschirm abdunkeln|
|TurnOffSetting|Deaktivieren einer Geräteeinstellung|Bluetooth deaktivieren<br/>Daten deaktivieren<br/>Bluetooth trennen|
|TurnOnSetting|Aktivieren einer Geräteeinstellung|Andererseits <br/> Aus|
|TurnUpBrightness|Erhöhen der Gerätehelligkeit|Machst du den Bildschirm heller?|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AppName | Name einer Anwendung auf dem Gerät.|SoundCloud<br/>YouTube|
| BrightnessLevel | Festlegen der Helligkeit des Geräts.|Hundert Prozent<br/>Fünfzig<br/>40%|
| ContactName | Der Name eines Kontakts auf dem Gerät.|Paul<br/>Elke Endemann|
| DeviceType | Der Typ des Geräts. |Phone<br/>Kindle<br/>Laptop|
| MediaType | Der vom Gerät verarbeitete Medientyp.|Musik<br/>Film<br/>Fernsehsendungen|
| SettingType | Ein Typ von Einstellung oder Einstellungspanel, die bzw. das der Benutzer bearbeiten möchte.|WLAN<br/>Drahtlosnetzwerk<br/>Farbschema<br/>Mitteilungszentrale|

## <a name="places"></a>Places  
Die Places-Domäne bietet Absichten für das Verarbeiten von Abfragen, die sich auf Orte wie Unternehmen, Institutionen, Restaurants, öffentliche Orte und Adressen beziehen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Speichere diesen Ort in meinen Favoriten|
|Wie weit ist es bis zum Holiday Inn?|
|Um wie viel Uhr schließt Rewe?|


### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AddFavoritePlace | Hinzufügen eines Orts zur Favoritenliste des Benutzers.|Speichere diesen Ort in meinen Favoriten<br/>Fügen diese Adresse zu meinen Favoriten hinzu|
|CheckAccident|Frage, ob für eine angegebene Straße ein Unfall gemeldet ist.|Gibt es einen Unfall auf der A3?<br/>Zeige Unfallinformationen an|
|CheckAreaTraffic|Allgemeine Überprüfung des Verkehrs für ein Gebiet oder eine Autobahn, nicht für eine angegebene Fahrtstrecke.|Verkehr in Düsseldorf<br/>Wie ist der Verkehr in Düsseldorf?|
|CheckIntoPlace|Einchecken an einem Ort mithilfe von sozialen Medien.|Checke mich bei Foursquare ein<br/>Hier einchecken|
|CheckRouteTraffic| Überprüfen des Verkehrs auf einer bestimmten Fahrtstrecke, die vom Benutzer angegeben wurde.|Wie ist der Verkehr nach Mashiko?<br/>Zeige mir den Verkehr nach Kirkland<br/>Wie ist der Verkehr nach Düsseldorf?| 
|Confirm|Bestätigen einer Aktion im Zusammenhang mit einem Ort.|Bestätige meine Restaurantreservierung.|
|Beenden|Aktion zum Beenden einer Aufgabe, die sich auf einen Ort bezieht.|Bitte Ruhe<br/>Gib mir keine Wegbeschreibungen mehr|
|FindPlace|Suche nach einem Ort (Unternehmen, Institution, Restaurant, öffentlicher Ort, Adresse).|Wo ist die nächste Bibliothek?<br/>Suche mir ein gutes italienisches Restaurant in Marburg|
|GetAddress| Fragen nach der Adresse eines Orts.|Zeige mit die Adresse von Guu auf der Immermannstraße<br/>Wie lautet die Adresse des nächsten Starbucks?| 
|GetDistance|Frage nach der Entfernung zu einem bestimmten Ort.|Wie weit ist es bis zum Holiday Inn?<br/>Wie weit ist es von hier bis zum Barbarossaplatz?<br/>Wie ist die Entfernung nach Rüdesheim?|
|GetHours|Frage nach den Geschäftszeiten eines Orts.|Um wie viel Uhr schließt Rewe?<br/>Welche Öffnungszeiten hat Bauhaus?<br/>Ist Starbucks noch geöffnet?|
|GetMenu|Frage nach der Speisekarte für ein Restaurant.|Bietet Zucca irgendetwas Veganes an?<br/>Was ist bei Vapiano auf der Karte<br/>Zeige mir das Menü von Maredo|
|GetPhoneNumber| Frage nach der Telefonnummer für einen Ort|Wie lautet die Telefonnummer des nächsten Starbucks?<br/>Gib mit die Nummer von Bauhaus| 
|GetPriceRange| Fragt nach dem Preisbereich eines Orts.|Ist Zucca billig?<br/>Kostet das Cineplex Mittwoch die Hälfte?<br/>Wieviel kostet ein Steak bei Vapiano?|
|GetReviews|Fragen nach Rezensionen zu einem Ort.|Zeige mir Rezensionen zur Käsekuchenfabrik<br/>Cineplex-Rezensionen in Yelp vorlesen|
|GetRoute|Fragen nach der Wegbeschreibung zu einem Ort|Fußweg zum Barbarossaplatz<br/>Zeige mir den kürzesten Weg zur Kreuzung Aachener Str./Roonstr.<br/>Besorge mir eine Wegbeschreibung nach Marburg|
|GetStarRating|Frage nach der Bewertung in Sternen für einen Ort.|Wie ist Zucca laut Yelp bewertet?<br/>Wie viele Sterne hat die französische Wäscherei?<br/>Ist das Aquarium in Duisburg gut?|
|GetTransportationSchedule|Abrufen des Busfahrplans für einen Ort.|Wann fährt der nächste Bus in die Stadtmitte?<br/>Zeige mir die Linienbusse im VTS an|
|GetTravelTime|Frage nach der Reisedauer zu einem angegebenen Ziel.|Wie lange dauert es von hier nach Mainz<br/>Wie ist die Fahrzeit von Potsdam nach Leipzig?|
|MakeCall|Tätigen eines Telefonanrufs an einen Ort.|Mama anrufen<br/>Ich möchte einen Skype-Anruf mit Anna führen<br/>Klaus anrufen|
|MakeReservation|Anfordern einer Reservierung für ein Restaurant oder ein anderes Geschäft.|Reserviere bei Zucca einen Tisch für zwei für heute Abend<br/>Buche einen Tisch für morgen<br/>Tisch für 3 in Garmisch-Partenkirchen um Acht|
|MapQuestions|Anfordern von Informationen zur Wegstrecke oder ob eine angegebene Straße zu einem bestimmten Ziel führt.|Führt die B9 durch die Innenstadt?<br/>Kann ich die A3 nach Stuttgart nehmen?|
|Rating|Abrufen der Bewertungsbeschreibung eines Restaurants oder Orts.|Wie viele Sterne hat der Glatzenburger Hof?|
|ReadAloud|Sprachausgabe einer Liste mit Orten.|Lies mit den ersten vor<br/>Lies mir die Details vor|
|SelectItem|Auswählen eines Elements aus einer Liste mit Optionen, die sich auf einen Ort oder Orte beziehen.|Wähle den zweiten aus<br/>Wähle den ersten|
|ShowMap|Anzeigen einer Karte eines Gebiets.|Zeige eine Karte für den zweiten an<br/>Karte anzeigen<br/>Suche Mainz auf der Karte|
|ShowNext|Anzeigen des nächsten Elements in einer Reihe|Nächstes anzeigen<br/>Gehe zur nächsten Seite|
|ShowPrevious|Anzeigen des vorhergehenden Elements in einer Reihe|Vorheriges anzeigen<br/>Vorherige<br/>Zum Vorhergehenden wechseln|
|StartOver|Neustart der App oder Starten einer neuen Sitzung.|Von vorn beginnen<br/>Neue Sitzung<br/>
restart|
|TakesReservations|Frage, ob man an einem Ort reservieren kann.|Nimmt die Kunstgalerie Reservierungen an?<br/>Ist es möglich, im Olivengarten zu reservieren?

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| AbsoluteLocation | Der Ort oder die Adresse eines Orts.|Garmisch-Partenkirchen<br/>Südstr. 12<br/>Seattle|
| Amenities | Die objektiven Merkmale/Vorzüge eines Orts.|Kinder essen ohne Berechnung<br/>Am Ufer<br/>Kostenloses Parken|
| Atmosphere | Die Atmosphäre eines Orts.|Kinderfreundlich<br/>Ungezwungenes Restaurant<br/>sportiv|
| Cuisine | Die Küche eines Orts. |Mediterran<br/>Italienisch<br/>Indisch|
| DestinationAddress| Ein Zielort oder eine Adresse.|Garmisch-Partenkirchen<br/>Südstr. 12<br/>Seattle|
| DestinationPlaceName| Der Name eines Ziels, der ein Unternehmen, ein Restaurant, eine öffentliche Sehenswürdigkeit oder eine Institution darstellt.|Palmengarten<br/>Rewe<br/>Edeka|
| DestinationPlaceType | Der Typ eines Ziels, der ein ortsansässiges Unternehmen, ein Restaurant, eine öffentliche Sehenswürdigkeit oder eine Institution darstellt. |Restaurant<br/>Opera<br/>Kino|
| Distance | Die Entfernung zu einem Ort.|15 Kilometer<br/>5 Kilometer<br/>10 Kilometer weit weg|
| MealType | Art der Mahlzeit, wie Frühstück oder Mittagessen. |Frühstück<br/>Abendessen<br/>Mittagessen<br/>Imbiss|
| OpenStatus | Anzeige, ob ein Ort offen oder geschlossen ist.|Öffnen<br/>closed<br/>Öffnungszeiten|
| PlaceName | Der Name eines Orts.|Käsekuchenfabrik|
| PlaceType | Der Typ eines Orts.|Cafe<br/>Kino<br/>Bibliothek|
| PreferredRoute | Der vom Benutzer angegebene bevorzugte Weg. | 101 <br/>202 <br/>B37|
| Produkt | Das an einem Ort angebotene Produkt. | Bekleidung<br/>Digitale ASR-Kameras<br/>Fangfrischer Fisch | 
| PublicTransportationRoute | Der Name der Strecke im öffentlichen Nahverkehr, nach der der Benutzer sucht. | Regionalexpress R5<br/>Buslinie 36 |
| Rating | Die Bewertung eines Orts. | 5 Sterne<br/>3 Sterne<br/>4 Sterne|
| RouteAvoidanceCriteria | Kriterien zum Meiden bestimmter Wegstrecken, etwa zum Umfahren von Unfällen, Baustellen oder Mautstationen | Mautgebühren <br/>Baustellen<br/>B11|
| ServiceProvided | Dies ist die von einem Unternehmen oder einem Ort angebotene Dienstleistung, wie etwa Haarschnitt, Schnee pflügen oder Landschaftsbau. | Haarschnitt<br/>Mechaniker<br/>Klempner|
| TransportationCompany | Der Name eines Transportanbieters.|DB<br/>NordWestBahn<br/>Flixbus|
| TransportationType | Die Transportart|Bus<br/>Trainieren<br/>Driving (Auto)|

## <a name="reminder"></a>Reminder 
Die Reminder-Domäne bietet Absichten und Entitäten zum Erstellen, Bearbeiten und Suchen von Erinnerungen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Ändere mein Personalgespräch morgen auf 9 Uhr|
|Erinnere mich daran, auf dem Heimweg Milch zu kaufen|
|Kannst du nachsehen, ob ich eine Erinnerung für Christines Geburtstag eingerichtet habe?|


### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Change| Ändern von Erinnerungen|Ändere mein Personalgespräch morgen auf 9 Uhr<br/>Verschiebe meine Aufgabenerinnerung auf morgen|
| Erstellen| Neuen Bericht erstellen|Einen Bericht erstellen<br/>Erinnere mich daran, Milch zu kaufen<br/>Ich möchte daran erinnert werden, Rebekka anzurufen, wenn ich nach Hause komme|
| Löschen | Erinnerung löschen|Lösche meine Bilderinnerung<br/>Lösche diese Erinnerung|
| Suchen | Suchen einer Erinnerung|Habe ich eine Erinnerung zu meinem Jubiläum festgelegt?<br/>Kannst du nachsehen, ob ich eine Erinnerung für Christines Geburtstag eingerichtet habe?|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Text | Die Textbeschreibung einer Erinnerung.|Wäsche aus der Reinigung holen<br/>Auto bei der Werkstatt abgeben|

## <a name="restaurantreservation"></a>RestaurantReservation 
Die RestaurantReservation-Domäne enthält Absichten und Entitäten zum Verwalten von Restaurantreservierungen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Reserviere bei Zucca einen Tisch für zwei für heute Abend|
|Buche einen Tisch bei BJ‘s für morgen|
|Tisch für 3 in Garmisch-Partenkirchen um Sieben|

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Reserve | Anfordern einer Reservierung für ein Restaurant |Reserviere bei Zucca einen Tisch für zwei für heute Abend<br/>Buche einen Tisch für morgen<br/>Tisch für 3 in Garmisch-Partenkirchen um Sieben|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Adresse| Ein Veranstaltungsort oder die Adresse für eine Reservierung|Garmisch-Partenkirchen<br/>Südstr. 12<br/>Seattle|
| Amenities | Ein Attribut, das die Annehmlichkeiten eines Orts beschreibt.|Meerblick<br/>Rauchen verboten|
| AppName | Der Name einer Anwendung für das Tätigen von Reservierungen|OpenTable<br/>Yelp<br/>TripAdvisor|
| Atmosphere | Eine Beschreibung der Atmosphäre eines Restaurants oder eines anderen Orts.|romantisch<br/>ungezwungen<br/>gut für Gruppen geeignet|
| Cuisine | Die Art des Speisenangebots, der Kochstil oder die Nationalküche |Chinesisch<br/>Italienisch<br/>Mexikanisch|
| MealType | Die Art der Mahlzeit, auf die sich eine Reservierung bezieht.|Frühstück<br/>Abendessen<br/>Mittagessen<br/>Imbiss|
| PlaceName | Der Name eines lokalen Unternehmens, eines Restaurants, einer öffentlichen Sehenswürdigkeit oder einer Institution.|Nordsee<br/>Käsekuchenfabrik<br/>Louvre|
| PlaceType | Der Typ eines lokalen Unternehmens, eines Restaurants, einer öffentlichen Sehenswürdigkeit oder einer Institution.|Restaurant<br/>Oper<br/>Kino|
| Rating | Die Bewertung eines Orts oder eines Restaurants.|5 Sterne<br/>3 Sterne<br/>4 Sterne|

## <a name="taxi"></a>Taxi 
 
Die Taxi-Domäne enthält Absichten und Entitäten zum Erstellen und Verwalten von Taxibuchungen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Ruf mir zu 15.00 Uhr ein Taxi|
|Wie lange muss ich noch auf mein Taxi warten?|
|Bestelle den Funkmietwagen ab|

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Buch | Rufen eines Taxis. |Ruf mir ein Taxi<br/>Finde ein Taxi<br/>Buche mir einen Funkmietwagen|
| Abbrechen | Stornieren einer Aktion mit Bezug auf das Buchen eines Taxis|Storniere mein Taxi<br/>Bestelle den Funkmietwagen ab|
| Track | Nachverfolgen einer Taxiroute|Wie lange muss ich noch auf mein Taxi warten?<br/>Wo bleibt mein Funkmietwagen?|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Adresse| Die der Buchung eines Taxis zugeordnete Adresse |Garmisch-Partenkirchen<br/>Südstr. 12<br/>Seattle|
| DestinationAddress| Ein Zielort oder eine Adresse. |Garmisch-Partenkirchen<br/>Südstr. 12<br/>Seattle|
| DestinationPlaceName | Der Name eines Ziels, der ein ortsansässiges Unternehmen, ein Restaurant, eine öffentliche Sehenswürdigkeit oder eine Institution darstellt. |Palmengarten<br/>Rewe<br/>Edeka|
| DestinationPlaceType | Der Typ eines Ziels, der ein ortsansässiges Unternehmen, ein Restaurant, eine öffentliche Sehenswürdigkeit oder eine Institution darstellt. |Restaurant<br/>Opera<br/>Kino|
| PlaceName | Der Name eines lokalen Unternehmens, eines Restaurants, einer öffentlichen Sehenswürdigkeit oder einer Institution. |Palmengarten<br/>Rewe<br/>Edeka|
| PlaceType| Die Art des Orts in einer Buchungsanforderung für ein Taxi.|Restaurant<br/>Opera<br/>Kino|
| TransportationCompany | Der Name eines Transportanbieters.|DB<br/>NordWestBahn<br/>Flixbus|
| TransportationType | Die Transportart|Bus<br/>Trainieren<br/>Driving (Auto)|

## <a name="translate"></a>Translate 
Die Translate-Domäne enthält Absichten und Entitäten im Zusammenhang mit der Übersetzung von Text in eine Zielsprache.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Übersetze ins Französische|
|Übersetze „hello“ ins Deutsche|
|Übersetze diesen Satz ins Englische|


### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Translate| Übersetzen von Text in eine andere Sprache.|Übersetze ins Französische<br/>Übersetze „hello“ ins Deutsche|


### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| TargetLanguage | Die Zielsprache einer Übersetzung.|Französisch<br/>Deutsch<br/>Koreanisch|
| Text | Der zu übersetzende Text|Hallo Welt<br/>Guten Morgen<br/>Guten Abend|

## <a name="tv"></a>TV 
 
Die TV-Domäne enthält Absichten und Entitäten zum Steuern von Fernsehgeräten.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Wechsle den Kanal zu RTL|
|TV-Programm anzeigen|
|Zeige n-tv|

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| ChangeChannel| Wechseln des Kanals an einem Fernsehgerät|Wechsle den Kanal zu CNN<br/>Wechsle den Kanal zu RTL<br/>Gehe zu SWR|
| ShowGuide| Anzeigen des TV-Programms|TV-Programm anzeigen<br/>Was läuft jetzt im Filmkanal?<br/>Zeige meine Programmliste an|
| WatchTV| Bitte, einen TV-Kanal anzusehen|Ich möchte den Disney-Kanal ansehen<br/>Bitte schalte auf Fernsehen um<br/>Zeige n-tv|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| ChannelName | Der Name eines Fernsehkanals|CNN<br/>RTL<br/>Filmkanal|

## <a name="utilities"></a>Versorgungsunternehmen  
Die Utilities-Domäne enthält Absichten für Aufgaben, die vielen Aufgaben gemein sind, wie Begrüßung, Stornierung, Bestätigung, Hilfe, Wiederholung, Navigation, Starten und Beenden.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Gehe zu Twitter zurück|
|Hilfe, bitte|
|Bitte wiederhole die letzte Frage|


### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Abbrechen | Abbrechen einer Aktion|Storniere die Nachricht<br/>Ich möchte die E-Mail nicht mehr senden|
| Confirm | Bestätigen einer Aktion.|Klaro, ich bestätige<br/>Gut, ich bestätige<br/>OK, ich bestätige|
| FinishTask | Abschließen einer Aufgabe, die vom Benutzer gestartet wurde|Ich bin fertig<br/>Ich bin erledigt<br/>Es ist geschafft|
| GoBack | Gehe einen Schritt zurück, oder wechsle zu einem vorhergehenden Schritt|Gehe zu Twitter zurück<br/>Gehe einen Schritt zurück.<br/>Zurück navigieren|
| Hilfe | Anforderung von Hilfe|Hilfe, bitte<br/>Hilfe öffnen<br/>help|
| Repeat | Wiederholen einer Aktion|Bitte wiederhole die letzte Frage<br/>Wiederhole das letzte Stück|
| ShowNext | Anzeigen des nächsten Elements in einer Reihe |Nächstes anzeigen<br/>Gehe zur nächsten Seite|
| ShowPrevious | Anzeigen des vorhergehenden Elements in einer Reihe|Vorheriges anzeigen|
| StartOver | Neustart der App oder Starten einer neuen Sitzung.|Von vorn beginnen<br/>Neue Sitzung<br/>restart|
| Beenden | Beenden einer Aktion| Bitte sag das nicht<br/>Halt den Mund<br/>Hör bitte auf|

## <a name="weather"></a>Weather 
Die Weather-Domäne enthält Absichten und Entitäten zum Abrufen von Wetterberichten und -vorhersagen.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Wetter in London im September|
|Wie ist die Vorhersage für die nächsten 10 Tage?|
|Was ist die Durchschnittstemperatur in Indien im September?|


### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| GetCondition | Abrufen historischer Fakten im Zusammenhang mit dem Wetter |Wetter in London im September<br/>Was ist die Durchschnittstemperatur in Indien im September?|
| GetForecast | Abrufen des aktuellen Wetters und der Vorhersage für die nächsten Tage |Wie ist das Wetter heute?<br/>Wie ist die Vorhersage für die nächsten 10 Tage?<br/>Wie wird das Wetter am Wochenende?|

### <a name="entities"></a>Entitäten
| Name der Entität | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Standort| Der absolute Standort für eine Wetteranfrage|Seattle<br/>Paris<br/>Garmisch-Partenkirchen|

## <a name="web"></a>Web 
Die Web-Domäne bietet eine Absicht für das Navigieren zu einer Website.

### <a name="examples"></a>Beispiele

|Beispiele|
|--|
|Navigiere zu facebook.com|
|Gehe zu www.twitter.com |
|Navigiere zu www.bing.com |

### <a name="intents"></a>Absichten
| Name der Absicht | BESCHREIBUNG | Beispiele |
| ---------------- |-----------------------|----|
| Navigieren | Eine Anforderung, zu einer angegebenen Website zu navigieren |Navigiere zu facebook.com<br/>Gehe zu www.twitter.com|

