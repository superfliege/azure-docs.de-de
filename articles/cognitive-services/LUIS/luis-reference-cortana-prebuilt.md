---
title: Vordefinierte Cortana-App – Referenz | Microsoft-Dokumentation
description: Referenz zum persönlichen Cortana-Assistenten, einer vordefinierten Anwendung, die auf Language Understanding Intelligent Services (LUIS) basiert.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: cd808c30a6781fc0252992c13ba247486e35ad44
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35373459"
---
# <a name="cortana-prebuilt-app-reference"></a>Vordefinierte Cortana-App – Referenz
In dieser Referenz sind die Absichten und Entitäten aufgeführt, die von der vordefinierten Cortana-App erkannt werden.

## <a name="cortana-prebuilt-intents"></a>Absichten der vordefinierten Cortana-App
Die vordefinierte Anwendung vom Typ „Persönlicher Assistent“ kann die folgenden Absichten identifizieren:

| Absicht | Beispiele für Äußerungen |
|--------| ------------------|
| builtin.intent.alarm.alarm_other|update my 7:30 alarm to be eight o'clock (Meine Weckzeit 7:30 auf acht Uhr aktualisieren) <br/>change my alarm from 8am to 9am (Meine Weckzeit von 8 in 9 Uhr ändern) |
| builtin.intent.alarm.delete_alarm|delete an alarm (Alarm löschen) <br/>delete my alarm "wake up" (Alarm „Wecker“ löschen)|
| builtin.intent.alarm.find_alarm|what time is my wake-up alarm set for? (Auf welche Uhrzeit ist meine Weckzeit festgelegt?) <br/> is my wake-up alarm on? (Ist mein Wecker aktiviert?) |
| builtin.intent.alarm.set_alarm|turn on my wake-up alarm (Wecker aktivieren)<br/>can you set an alarm for 12 called take antibiotics? (Alarm „Antibiotika nehmen“ auf 12 Uhr festlegen)|
| builtin.intent.alarm.snooze|snooze alarm for 5 minutes (Nach 5 Minuten erneut erinnern)<br/>snooze alarm („Erneut erinnern“ für Alarm)|
| builtin.intent.alarm.time_remaining| how much longer do I have until "wake-up"? (Wie lange ist es noch bis zur Weckzeit?) <br/> how much time until my next alarm? (Wie lange ist es bis zu meinem nächstem Alarm?)|
| builtin.intent.alarm.turn_off_alarm | turn off my 7am alarm (Alarm um 7 Uhr deaktivieren) <br/> turn off my wake-up alarm (Wecker deaktivieren) |
| builtin.intent.calendar.change_calendar_entry| change an appointment (Termin ändern)<br/>move my meeting from today to tomorrow (Besprechung von heute auf morgen verschieben)|
|builtin.intent.calendar.check_availability|is tim busy on Friday? (Ist Tim am Freitag beschäftigt?)<br/>is brian free on Saturday (Ist Brian am Samstag verfügbar?)|
|builtin.intent.calendar.connect_to_meeting|connect to a meeting (Mit einer Besprechung verbinden)<br/>join the meeting online (Online an Besprechung teilnehmen)|
|builtin.intent.calendar.create_calendar_entry|I need to schedule an appointment with tony for Friday (Termin mit Tony am Freitag planen)<br/>make an appointment with lisa at 2pm on Sunday (Mache einen Termin mit Lisa am Sonntag um 14 Uhr)|
|builtin.intent.calendar.delete_calendar_entry|delete my appointment (Meinen Termin löschen)<br/>remove the meeting at 3 pm tomorrow from my calendar (Besprechung morgen um 15 Uhr aus meinem Kalender entfernen)|
|builtin.intent.calendar.find_calendar_entry|show my calendar (Kalender anzeigen)<br/>display my weekly calendar (Meinen Wochenkalender anzeigen)|
|builtin.intent.calendar.find_calendar_when|when is my next meeting with jon? (Wann ist meine nächste Besprechung mit Jon?)<br/>what time is my appointment with larry on Monday? (Wann ist mein Termin mit Larry am Montag?)|
|builtin.intent.calendar.find_calendar_where|show me the location of my 6 o'clock meeting (Ort meiner Besprechung um 18 Uhr anzeigen)<br/>where is that meeting with jon? (Wo findet diese Besprechung mit Jon statt?)|
|builtin.intent.calendar.find_calendar_who|who is this meeting with? (Mit wem findet die Besprechung statt?)<br/>who else is invited? (Wer ist noch eingeladen?)|
|builtin.intent.calendar.find_calendar_why|what are the details of my 11 o'clock meeting? (Wie lauten die Details zur Besprechung um 11 Uhr?)<br/>what is that meeting with jon about? (Um was geht es bei der Besprechung mit Jon?)|
|builtin.intent.calendar.find_duration|how long is my next meeting (Wie lange dauert meine nächste Besprechung?)<br/>how many minutes long is my meeting this afternoon (Wie viele Minuten dauert meine Besprechung heute Nachmittag?)|
|builtin.intent.calendar.time_remaining|how much time until my next appointment? (Wie lange ist es bis zu meinem nächsten Termin?)<br/>how much more time do I have before the meeting? (Wie viel Zeit habe ich noch vor der Besprechung?)|
|builtin.intent.communication.add_contact|save this number and put the name as jose (Speichere diese Nummer unter dem Namen „Jose“)<br/>put jason in my contacts list (Füge Jason in meine Liste mit den Kontakten ein)|
|builtin.intent.communication.answer_phone|answer (Annehmen)<br/>answer the call (Anruf annehmen)|
|builtin.intent.communication.assign_nickname|edit nickname for nickolas (Spitzname für Nickolas bearbeiten)<br/>add a nickname for john doe (Spitzname für John Doe hinzufügen)|
|builtin.intent.communication.call_voice_mail|voice mail (Voicemail)<br/>call voicemail (Voicemail anrufen)|
|builtin.intent.communication.find_contact|show me my contacts (Meine Kontakte anzeigen)<br/>find contacts (Kontakte suchen)|
|builtin.intent.communication.forwarding_off|stop forwarding my calls (Meine Anrufe nicht mehr weiterleiten)<br/>switch off call forwarding (Anrufweiterleitung ausschalten)|
|builtin.intent.communication.forwarding_on|set call forwarding to send calls to home phone (Anrufweiterleitung festlegen, um Anrufe an Privatnummer zu senden)<br/>forward calls to home phone (Anrufe an Privatnummer weiterleiten)|
|builtin.intent.communication.ignore_incoming|do not answer that call (Anruf nicht annehmen)<br/>not now, I'm busy (Nicht jetzt, ich bin beschäftigt)|
|builtin.intent.communication.ignore_with_message|don't answer that call but send a message instead (Beantworte diesen Anruf nicht, sende stattdessen eine Nachricht)<br/>ignore and send a text back (Ignorieren und SMS zurückschicken)|
|builtin.intent.communication.make_call|call bob and john (Bob und John anrufen)<br/>call mom and dad (Mama und Papa anrufen)|
|builtin.intent.communication.press_key|dial star (Wähle die Sterntaste)<br/>press the 1 2 3 (1 2 3 drücken)|
|builtin.intent.communication.read_aloud|read text (Text lesen)<br/>what did she say in the message (Was hat sie in der Nachricht gesagt?)|
|builtin.intent.communication.redial|redial my last call (Wähle meinen letzten Anrufpartner noch einmal an)<br/>redial (Erneut anwählen)|
|builtin.intent.communication.send_email|email to mike waters mike that dinner last week was splendid (E-Mail an Mike Waters: Mike, das Essen letzte Woche war hervorragend)<br/>send an email to bob (E-Mail an Bob senden)|
|builtin.intent.communication.send_text|send text to bob and john (SMS an Bob und John senden)<br/>Nachricht|
|builtin.intent.communication.speakerphone_off|take me off speaker (Lautsprecher ausschalten)<br/>turn off speakerphone (Schalte Freisprechen aus)|
|builtin.intent.communication.speakerphone_on|speakerphone mode (Freisprechmodus)<br/>put on speakerphone (Freisprechmodus aktivieren)|
|builtin.intent.mystuff.find_attachment|find the document john emailed me yesterday (Dokument suchen, das John mir gestern per E-Mail gesendet hat) <br/>find the doc from john (Suche das Dokument von John)|
|builtin.intent.mystuff.find_my_stuff|I want to edit my shopping list from yesterday (Ich möchte meine Einkaufsliste von gestern bearbeiten)<br/>find my chemistry notes from September (Suche meine Chemienotizen vom September)
|builtin.intent.mystuff.search_messages|open message (Nachricht öffnen) <br/>Nachrichten|
|builtin.intent.mystuff.transform_my_stuff|share my shopping list with my husband (Meine Einkaufsliste für meinen Ehemann freigeben)<br/>delete my shopping list (Meine Einkaufsliste löschen)|
|builtin.intent.ondevice.open_setting|open cortana settings (Cortana-Einstellungen öffnen) <br/>jump to notifications (Springe zu Benachrichtigungen)|
|builtin.intent.ondevice.pause|turn off music (Musik ausschalten)<br/>music off (Musik aus)|
|builtin.intent.ondevice.play_music|I want to hear owner of a lonely heart (Ich möchte „Owner of a lonely heart“ hören)<br/>play some gospel music (Gospel-Musik wiedergeben)|
|builtin.intent.ondevice.recognize_song|tell me what this song is (Sag mir, welcher Song dies ist)<br/>analyze and retrieve title of song (Songtitel analysieren und abrufen)|
|builtin.intent.ondevice.repeat|repeat this track (Titel erneut wiedergeben)<br/>play this song again (Diesen Titel erneut wiedergeben)|
|builtin.intent.ondevice.resume|restart music (Musik neu starten)<br/>start music again (Musikwiedergabe erneut starten)|
|builtin.intent.ondevice.skip_back|back up a track (Titel sichern)<br/>previous song (Vorheriger Song)|
|builtin.intent.ondevice.skip_forward|next song (Nächster Song)<br/>skip ahead a track (Einen Titel überspringen)|
|builtin.intent.ondevice.turn_off_setting|wifi off (WLAN aus)<br/>disable airplane mode (Flugzeugmodus deaktivieren)|
|builtin.intent.ondevice.turn_on_setting|wifi on (WLAN ein)<br/>turn on bluetooth (Bluetooth aktivieren)|
|builtin.intent.places.add_favorite_place|add this address to my favorites (Füge diese Adresse meinen Favoriten hinzu)<br/>save this location to my favorites (Speichere diesen Ort in meinen Favoriten)|
|builtin.intent.places.book_public_transportation|buy a train ticket (Zugfahrkarte kaufen)<br/>book a tram pass (Straßenbahnticket buchen)|
|builtin.intent.places.book_taxi|can you find me a ride at this hour? (Kannst du für mich um diese Uhrzeit ein Transportmittel finden?)<br/>find a taxi (Finde ein Taxi)|
|builtin.intent.places.check_area_traffic|what's the traffic like on 520 (Wie ist der Verkehr auf der 520?)<br/>how is the traffic on i-5 (Wie ist der Verkehr auf der I-5?)|
|builtin.intent.places.check_into_place|check into joya (In Joya einchecken)<br/>check in here (Hier einchecken)|
|builtin.intent.places.check_route_traffic|show me the traffic on the way to kirkland (Verkehr auf dem Weg nach Kirkland anzeigen)<br/>how is the traffic to seattle? (Wie ist der Verkehr auf dem Weg nach Seattle?)|
|builtin.intent.places.find_place|where do I live (Wo wohne ich?) <br/>give me the top 3 Japanese restaurants (Nenne mir die drei besten japanischen Restaurants)|
|builtin.intent.places.get_address|show me the address of guu on robson street (Zeige mir die Adresse von Guu in der Robson Street)<br/>what is the address of the nearest starbucks? (Wie lautet die Adresse des nächsten Starbucks?)|
|builtin.intent.places.get_coupon|find deals on tvs in my area (Suche Angebote für Fernseher in der Nähe)<br/>discounts in mountain view (Rabatte in Mountain View)|
|builtin.intent.places.get_distance|how far away is holiday inn? (Wie weit ist es bis zum Holiday Inn?)<br/>what's the distance to tahoe (Wie ist die Entfernung nach Tahoe?)|
|builtin.intent.places.get_hours|what are bar del corso's hours on Mondays? (Wann ist die Bar del Corso montags geöffnet?)<br/>when is the library open? (Wann ist die Bibliothek geöffnet?)|
|builtin.intent.places.get_menu|show me applebee's menu (Zeige mir die Karte von Applebee)<br/>what's on the menu at sizzler (Was ist bei Sizzler auf der Karte?)|
|builtin.intent.places.get_phone_number|give the number for home depot (Gib mir die Nummer von Home Depot)<br/>what is the phone number of the nearest starbucks? (Wie lautet die Telefonnummer des nächsten Starbucks?)|
|builtin.intent.places.get_price_range|how much does dinner at red lobster cost (Was kostet ein Abendessen bei Red Lobster?)<br/>how expensive is purple cafe (Wie teuer ist Purple Cafe?)|
|builtin.intent.places.get_reviews|show me reviews for local hardware stores (Zeige Bewertungen für Baumärkte in der Nähe an)<br/>I want to see restaurant reviews (Ich möchte Restaurantkritiken angezeigt bekommen)|
|builtin.intent.places.get_route|give me directions to (Wegbeschreibung nach)|it is possible to get to pizza hut on foot (Ist es möglich, Pizza Hut zu Fuß zu erreichen?)|
|builtin.intent.places.get_star_rating|how many stars does the French laundry have? (Wie viele Sterne hat die französische Wäscherei?)<br/>is the aquarium in monterrey good? (Ist das Aquarium in Monterrey gut?)|
|builtin.intent.places.get_transportation_schedule|what time does the san francisco ferry leave? (Wann fährt die Fähre nach San Francisco ab?)<br/>when is the latest train to seattle? (Wann fährt der letzte Zug nach Seattle?)|
|builtin.intent.places.get_travel_time|what's the driving time to denver from SF (Wie ist die Fahrzeit von San Francisco nach Denver?)<br/>how long will it take to get to san francisco from here (Wie lange dauert es von hier nach San Francisco?)|
|builtin.intent.places.make_call|call Dr. smith in bellevue (Dr. Smith in Bellevue anrufen)<br/>call the home depot on 1st street (Rufe Home Depot in der 1st Street an)|
|builtin.intent.places.rate_place|give a rating for this restaurant (Bewertung für dieses Restaurant abgeben)<br/>rate il fornaio 5 stars on yelp (Il Fornaio bei Yelp mit 5 Sternen bewerten)|
|builtin.intent.places.show_map|what's my current location (Was ist mein aktueller Standort?) <br/>what's my location (Wie lautet mein Standort?)|
|builtin.intent.places.takes_reservations|is it possible to make a reservation at the olive garden (Ist es möglich, im Olivengarten zu reservieren?)<br/>does the art gallery accept reservations (Nimmt die Kunstgalerie Reservierungen an?)|
|builtin.intent.reminder.change_reminder|change a reminder (Erinnerung ändern)<br/>move up my picture day reminder 30 minutes (Verschiebe meine Tag-Bilderinnerung um 30 Minuten nach vorne)|
|builtin.intent.reminder.create_single_reminder|remind me to wake up at 7 am (Erinnere mich daran, um 7 Uhr aufzustehen)<br/>remind me to go trick or treating with luca at 4:40pm (Erinnere mich daran, um 16:40 Uhr mit Luca die Halloween-Runde zu gehen)|
|builtin.intent.reminder.delete_reminder|delete this reminder (Lösche diese Erinnerung)<br/>delete my picture reminder (Lösche meine Bilderinnerung)|
|builtin.intent.reminder.find_reminder|do I have any reminders set up for today (Sind für mich heute Erinnerungen vorhanden?)<br/>do I have a reminder set for luca's party (Habe ich eine Erinnerung für Lucas Party eingerichtet?)|
|builtin.intent.reminder.read_aloud|read reminder out loud (Erinnerung laut vorlesen)<br/>read that reminder (Lies die Erinnerung vor)|
|builtin.intent.reminder.snooze|snooze that reminder until tomorrow (Erinnerung auf morgen verschieben)<br/>snooze this reminder (Später erneut erinnern)|
|builtin.intent.reminder.turn_off_reminder|turn off reminder (Erinnerung deaktivieren)<br/>dismiss airport pickup reminder (Erinnerung an Flughafenabholung verwerfen)|
|builtin.intent.weather.change_temperature_unit|change from fahrenheit to kelvin (Von Fahrenheit zu Kelvin wechseln)<br/>change from fahrenheit to celsius (Von Fahrenheit zu Celsius wechseln)|
|builtin.intent.weather.check_weather|show me the forecast for my upcoming trip to seattle (Wettervorhersage für meine Reise nach Seattle anzeigen)<br/>how will the weather be this weekend (Wie wird das Wetter am Wochenende?)|
|builtin.intent.weather.check_weather_facts|what is the weather like in hawaii in December? (Wie ist das Wetter auf Hawaii im Dezember?)<br/>what was the temperature this time last year? (Welche Temperatur hatten wir im letzten Jahr zur gleichen Zeit?)|
|builtin.intent.weather.compare_weather|give me a comparison between the temperature high and lows of dallas and houston, tx (Vergleiche die höchsten und niedrigsten Temperaturen für Dallas und Houston, Texas)<br/>how does the weather compare to slc and nyc (Wie ist das Wetter im Vergleich zu Salt Lake City und New York City?)|
|builtin.intent.weather.get_frequent_locations|give me my most frequent location (Nenne mir meinen häufigsten Standort)<br/>show most often stops (Zeige die häufigsten Stopps an)|
|builtin.intent.weather.get_weather_advisory|weather warnings (Unwetterwarnungen)<br/>show weather advisory for sacramento (Zeige die Wetternachrichten für Sacramento an)|
|builtin.intent.weather.get_weather_maps|display a weather map (Wetterkarte anzeigen)<br/>show me weather maps for africa (Zeige mir Wetterkarten für Afrika an)|
|builtin.intent.weather.question_weather|will it be foggy tomorrow morning? (Wird es morgen früh neblig sein?)<br/>will I need to shovel snow this weekend? (Muss ich am Wochenende Schnee schaufeln?)|
|builtin.intent.weather.show_weather_progression|show local weather radar (Lokalen Wetterradar anzeigen)<br/>begin radar (Beginne Radar)|
|builtin.intent.none|how old are you (Wie alt sind Sie?)<br/>open camera (Kamera öffnen)|

## <a name="prebuilt-entities"></a>Vordefinierte Entitäten 

Hier sind einige Beispiele für Entitäten angegeben, die von der Anwendung vom Typ „Persönlicher Assistent“ identifiziert werden können:

|Entität |Beispiel für Entität in der Äußerung |
|-------|------------------|
|builtin.alarm.alarm_state | turn `off` my wake-up alarm (Weckzeit „ausschalten“)    <br/> is my wake-up alarm `on` (Ist mein Wecker „eingeschaltet“?)  | 
|builtin.alarm.duration |snooze for `10 minutes` (In „10 Minuten“ erneut erinnern)    <br/> snooze alarm for `5 minutes` (Erneut erinnern in „5 Minuten“)  |
|builtin.alarm.start_date | set an alarm for `monday` at 7 am (Alarm für „Montag“ um 7 Uhr festlegen)   <br/> set an alarm for `tomorrow` at noon (Lege einen Alarm für „morgen“ 12 Uhr mittags fest)   |
|builtin.alarm.start_time | create an alarm for `30 minutes` (Erstelle einen Alarm für „30 Minuten“)    <br/> set the alarm to go off `in 20 minutes` (Alarm „in 20 Minuten“ festlegen)   |
|builtin.alarm.title | is my `wake up` alarm on (Ist mein Alarm „Wecker“ aktiviert) <br/> can you set an alarm for quarter to 12 Monday to Friday called `take antibiotics` (Lege Alarm „Antibiotika nehmen“ auf 12 Uhr von Montag bis Freitag fest) |
|builtin.calendar.absolute_location | create an appointment for tomorrow at `123 main street` (Termin für morgen in „123 Main Street“ erstellen)   <br/> the meeting will take place in `cincinnati` on the 5th of june (Die Besprechung findet in „Cincinnati“ am 5. Juni statt)    |
|builtin.calendar.contact_name|put a marketing meeting on my calendar and be sure that `joe` is there (Marketingbesprechung in meinen Kalender einfügen und sicherstellen, dass „Joe“ dabei ist) <br/>I want to set up a lunch at il fornaio and invite `paul` (Ich möchte ein Mittagessen im Il Fornaio planen und „Paul“ einladen) |   
|builtin.calendar.destination_calendar|add this to my `work` schedule (Füge dies meinem „geschäftlichen“ Zeitplan hinzu)   <br/>put this on my `personal` calendar (Füge dies in meinen „persönlichen“ Kalender ein) |
|builtin.calendar.duration| set up an appointment for `an hour` at 6 tonight (Termin mit einer Dauer von „einer Stunde“ heute um 18 Uhr einrichten) <br/>  book a `2 hour` meeting with joe („Zweistündige“ Besprechung mit Joe buchen) |  
|builtin.calendar.end_date | create a calendar entry called vacation from tomorrow until `next monday` (Erstelle einen Kalendereintrag „Urlaub“ ab morgen bis „nächsten Montag“) <br/>    block my time as busy until `monday, october 5th` (Lege für mich „Beschäftigt“ bis „Montag, 5. Oktober“ fest) | 
|builtin.calendar.end_time | the meeting ends at `5:30 PM` (Besprechung endet um „17:30 Uhr“) <br/> schedule it from 11 to `noon` (Von 11 bis „12 Uhr mittags“ einplanen)  |   
|builtin.calendar.implicit_location | cancel the appointment at the dmv (Sage Termin bei DMV ab) <br/> change the location of miles' birthday to `poppy restaurant` (Ort für Geburtstag von Miles in „Poppy Restaurant“ ändern) |    
|builtin.calendar.move_earlier_time | push the meeting forward `an hour` (Besprechung um „eine Stunde“ verschieben) <br/> move the dentist's appointment up `30 minutes` (Zahnarzttermin um „30 Minuten“ verschieben)       |
|builtin.calendar.move_later_time | move my dentist appointment `30 minutes` (Verschiebe meinen Zahnarzttermin um „30 Minuten)    <br/> push the meeting out `an hour` (Besprechung um „eine Stunde“ verschieben) |  
|builtin.calendar.original_start_date | reschedule my appointment at the barber from 'Tuesday' to Wednesday (Termin beim Friseur von „Dienstag“ auf „Mittwoch“ verlegen) <br/> move my meeting with ken from `monday` to Tuesday (Verschiebe meine Besprechung mit Ken von „Montag“ auf Dienstag) |
|builtin.calendar.original_start_time | reschedule my meeting from `2:00` to 3 (Verlege meine Besprechung von „14 Uhr“ auf 15 Uhr)  <br/> change my dentist appointment from `3:30` to 4 (Zahnarzttermin von „15:30 Uhr“ auf 16:00 Uhr verlegen) |  
|builtin.calendar.start_date | what time does my party start on `flag day` (Wann beginnt meine Party am „Flag Day“?) <br/> schedule lunch for the `friday after next` at noon (Mittagessen für „übernächsten Freitag“ um 12 Uhr mittags planen) 
|builtin.calendar.start_time | I want to schedule it for `this morning` (Ich möchte dies für „heute Vormittag“ einplanen) <br/> I want to schedule it in the `morning` (Ich möchte dies für „vormittags“ einplanen) |  
|builtin.calendar.title | `vet appointment`  <br/> `dentist` Tuesday („Zahnarzt“ Dienstag) |
|builtin.communication.audio_device_type | make the call using `bluetooth` (Per „Bluetooth“ anrufen)  <br/> call using my `headset` (Über das „Headset“ anrufen) | 
|builtin.communication.contact_name | text `bob jones` (SMS an „Bob Jones“ senden)  <br/> | call `sarah` („Sarah“ anrufen)|
|builtin.communication.destination_platform | call dave in `london` (Dave in „London“ anrufen) <br/> call his `work line` („Geschäftliche Nummer“ anrufen) |    
|builtin.communication.from_relationship_name | show calls from my `daughter` (Anrufe von meiner „Tochter“ anzeigen) <br/> read the email from `mom` (E-Mail von „Mama“ lesen)   |   
|builtin.communication.key | dial `star` („Stern“ wählen) <br/>  press the `hash` key („Nummernzeichen“ drücken)    |
|builtin.communication.message | email carly to say `i'm running late` (Sende E-Mail mit „Ich komme später“ an Carly) <br/> text angus smith `good luck on your exam` (SMS mit „Viel Glück bei der Prüfung“ an Angus Smith senden) |    
|builtin.communication.message_category | new email marked for `follow up` (Neue E-Mail mit Kennzeichnung „Nachverfolgung“)  <br/> new email marked `high priority` (Neue E-Mail mit Kennzeichnung „Hohe Priorität“) |    
|builtin.communication.message_type | send an `email` („E-Mail“ senden)   <br/> read my `text` messages aloud („SMS“-Nachrichten laut vorlesen) |
|builtin.communication.phone_number | I want to dial `1-800-328-9459` (Ich möchte „1-800-328-9459“ wählen) <br/>     call `555-555-5555` („555-555-5555“ anrufen) |   
|builtin.communication.relationship_name | text my `husband` (SMS an meinen „Ehemann“ senden) <br/>  email `family` (E-Mail an „Familie“)| 
|builtin.communication.slot_attribute | change the `recipient` („Empfänger“ ändern) <br/>    change the `text` („Text“ ändern) | 
|builtin.comminication.source_platform | call him from `skype` (Über „Skype“ anrufen) <br/> call him from my `personal line` (Über „eigene Leitung“ anrufen) |
|builtin.mystuff.attachment| with documents `attached` (Mit „angefügten“ Dokumenten) <br/> find the email `attachment` bob sent („Anhang“ der E-Mail von Bob suchen) |
|builtin.mystuff.contact_name| find the spreadsheet lisa sent to `me` (Suche Tabellenkalkulation, die Lisa an „mich“ gesendet hat) <br/> where's the document i sent to `susan` last night (Wo ist das Dokument, das ich gestern Abend an „Susan“ gesendet habe?) |
|builtin.mystuff.data_source | `c:\dev\` <br/> my `desktop` (Mein „Desktop“) <br/> |`"resolution": { "resolution_type": "metadataItems", "metadataType": CanonicalEntity", "value": "desktop" }`|
|builtin.mystuff.data_type | locate the `document` i worked on last night (Finde das „Dokument“, an dem ich gestern Abend gearbeitet habe) <br/> |`"resolution": {"resolution_type": "metadataItems", "metadataType": "CanonicalEntity", "value":Document" }` <br/> bring up mike's `visio diagram` (Öffne das „Visio-Diagramm“ von Mike)  |
|builtin.mystuff.end_date| show me the docs i worked on `between yesterday and today` (Dokumente anzeigen, an denen ich „gestern und heute“ gearbeitet habe)   <br/> find what doc i was working on `before thursday the 31st` (Suche das Dokument, an dem ich vor „Donnerstag, dem 31.“, gearbeitet habe) |
|builtin.mystuff.end_time| find files i saved `before noon` (Suche Dateien, die ich „vor 12 Uhr mittags“ gespeichert habe) <br/> find what doc i was working on `before 4pm` (Suche das Dokument, an dem ich vor „16 Uhr“ gearbeitet habe)|      
|builtin.mystuff.file_action| open the spreadsheet i `saved` yesterday (Öffne die Tabellenkalkulation, die ich gestern „gespeichert“ habe) <br/> find the spreadsheet kevin `created` (Finde die Tabellenkalkulation, die Kevin „erstellt“ hat)| 
|builtin.mystuff.from_contact_name | find the proposal `jason` sent me (Suche das Angebot, das „Jason“ gesendet hat) <br/> open `isaac`'s last email (Öffne die letzte E-Mail von „Isaac“) |
|builtin.mystuff.keyword | show me the `french conjugation` files (Zeige die Dateien mit der „französischen Konjugation“ an) <br/> find the `marketing plan` i drafted yesterday (Suche nach dem „Marketingplan“, den ich gestern erstellt habe) |
|builtin.mystuff.location | the document i edited at `work` (Das Dokument, das ich bei der „Arbeit“ bearbeitet habe) <br/> photos i took in `paris` (Fotos, die ich in „Paris“ aufgenommen habe) |
|builtin.mystuff.message_category | look for my `new` emails (Suche nach meinen „neuen“ E-Mails) <br/> search for my `high priority` email (Suche nach meiner E-Mail mit „hoher Priorität“) |
|builtin.mystuff.message_type | check my `email` (Meine „E-Mail“ prüfen) <br/>  show me my `text` messages (Meine „SMS“-Nachrichten anzeigen)  |
|builtin.mystuff.source_platform | search my `hotmail` email for email from john (Meine „Hotmail“ nach E-Mails von John durchsuchen)    <br/> find the document i sent from `work` (Suche das Dokument, das ich von der „Arbeit“ gesendet habe) |
|builtin.mystuff.start_date | find notes from `january` (Notizen aus dem „Januar“ suchen) <br/> find the email i send rob `after january 1st` (Suche die E-Mail, die ich „nach dem 1. Januar“ an Rob gesendet habe) |
|builtin.mystuff.start_time | find that email i sent rob sometime before 2pm but `after noon`? (Suche die E-Mail, die ich vor 14 Uhr und „nach 12 Uhr mittags“ an Rob gesendet habe) <br/> find the worksheet kristin sent to me that I edited `last night` (Suche das Arbeitsblatt von Kristin, das ich „gestern Abend“ bearbeitet habe) | 
|builtin.mystuff.title | `c:\dev\mystuff.txt` <br/> `*.txt` |
|builtin.mystuff.transform_action | `download` the file john sent me (Von John gesendete Datei „herunterladen“) <br/> `open` my annotation guidelines doc („Öffne“ mein Dokument mit den Richtlinien zu Anmerkungen) |
|builtin.note.note_text | create a grocery list including `pork chops, applesauce and milk` (Erstelle einen Einkaufszettel mit „Schweinekoteletts, Apfelsoße und Milch“) <br/> make a note to `buy milk` (Erstelle Notiz „Milch kaufen“) |
|builtin.note.title | make a note called `grocery list` (Erstelle Notiz „Einkaufszettel“) <br/> make a note called `people to call` (Erstelle Notiz „Anzurufende Personen“) |
|builtin.ondevice.music_artist_name | play everything by `rufus wainwright` (Alle Titel von „Rufus Wainwright“ wiedergeben) <br/> play `garth brooks` music (Musik von „Garth Brooks“ wiedergeben) |   
|builtin.ondevice.music_genre | show `classic rock` songs („Classic Rock“-Songs anzeigen) <br/> play my `classical` music from the baroque period (Spiele meine „klassische“ Musik aus dem Barock) |
|builtin.ondevice.music_playlist | shuffle all britney spears from `workout` playlist (Zufallswiedergabe aller Titel von Britney Spears der Wiedergabeliste „Workout“) <br/> play `breakup` playlist (Wiedergabeliste „Breakup“ wiedergeben)
|builtin.ondevice.music_song_name | play `summertime` („Summertime“ wiedergeben) <br/> play `me and bobby mcgee` („Me and Bobby McGee“ wiedergeben) | 
|builtin.ondevice.setting_type | `quiet hours` <br/> `airplane mode` |
|builtin.places.absolute_location | take me to the intersection of `5th and pike` (Führe mich zur Kreuzung von „5th und Pike“) <br/> no, I want directions to `1 microsoft way redmond wa 98052` (Nein, Wegbeschreibung zu „1 Microsoft Way Redmond WA 98052“) |
|builtin.places.atmosphere | look for `interesting` places to go out (Suche nach „interessanten“ Orten zum Ausgehen)    <br/> where can I find a `casual` restaurant (Wo finde ich ein „einfaches“ Restaurant?) |  
|builtin.places.audio_device_type |call the post office on `hands free` (Postamt über „Freisprechen“ anrufen) <br/> call papa john's with `speakerphone` (Papa John‘s über „Lautsprecher“ anrufen) |    
|builtin.places.avoid_route | avoid the `toll road` („Mautstraße“ vermeiden) <br/> get me to san francisco avoiding the `construction on 101` (Führe mich nach San Francisco und meide die „Baustelle auf der 101“) |  
|builtin.places.cuisine | `halal` deli near mountain view („Halal“-Geschäft in der Nähe von Mountain View)   <br/> `kosher` fine dining on the peninsula (Gute „koschere“ Restaurants auf der Halbinsel) |
|builtin.places.date | make a reservation for `next friday the 12th` (Reserviere für „nächsten Freitag, den 12.“) <br/>  is mashiko open on `mondays`? (Hat Mashiko „montags“ geöffnet?) |
|builtin.places.discount_type | find a `coupon` for macy's (Finde „Gutschein“ für Macy‘s) <br/> find me a `coupon` („Gutschein“ suchen) |
|builtin.places.distance | is there a good diner `within 5 miles` of here (Gibt es „innerhalb von fünf Meilen“ einen guten Diner?) <br/> find ones `within 15 miles` (Suche „innerhalb von 15 Meilen“) |   
|builtin.places.from_absolute_location | directions from `45 elm street` to home (Wegbeschreibung von „45 Elm Street“ nach Hause) <br/> get me directions from `san francisco` to palo alto (Wegbeschreibung von „San Francisco“ nach Palo Alto)  |
|builtin.places.from_place_name | driving from the `post office` to 56 center street (Fahrt vom „Postamt“ in die 56 Center Street) <br/> get me directions from `home depot` to lowes (Wegbeschreibung von „Home Depot“ nach Lowes) |
|builtin.places.from_place_type | directions to downtown from `work` (Wegbeschreibung von der „Arbeit“ in die Stadt) <br/>  get me directions from the `drug store` to home (Wegbeschreibung von der „Drogerie“ nach Hause) |
|builtin.places.meal_type | nearby places for `dinner` (Restaurants in der Nähe für „Abendessen“) <br/> find a good place for a business `lunch` (Suche guten Ort für ein geschäftliches „Mittagessen“) | 
|builtin.places.nearby | show me some cool shops `near` me (Zeige coole Shops in meiner „Nähe“ an)  <br/> are there any good Lebanese restaurants `around` here? (Gibt es „hier“ gute libanesische Restaurants?) |
|builtin.places.open_status | when is the mall `closed` (Wann ist das Einkaufszentrum „geschlossen“?) <br/> get me the `opening` hours of the store (Zeige die „Öffnungszeiten“ des Geschäfts an) | 
|builtin.places.place_name | take me to `central park` (Führe mich zum „Central Park“) <br/> look up the `eiffel tower` (Suche nach dem „Eiffelturm“) |   
|builtin.places.place_type | `atms` <br/> `post office` |
|builtin.places.prefer_route | show directions by the `shortest` route (Zeige Wegbeschreibung mit der „kürzesten“ Route an) <br/> take the `fastest` route („Schnellste“ Route) | 
|builtin.places.price_range| give me places that are `moderately affordable` (Zeige mir Orte an, die „einigermaßen erschwinglich“ sind) <br/>  I want an `expensive` one (Ich möchte eine „teure“ Option)   |
|builtin.places.product | where can I get `fresh fish` around here (Wo finde ich hier „frischen Fisch“?)  <br/> where around here sells `bare minerals` (Wo wird hier „Bare Minerals“ verkauft?) |
|builtin.places.public_transportation_route | bus schedule for the `m2` bus (Busfahrplan für den Bus „M2“) <br/> bus route `3x` (Buslinie „3X“) |  
|builtin.places.rating | show `3 star` restaurants (Zeige Restaurants mit „3 Sternen“ an) <br/> show results that are `3 stars or higher` (Zeige Ergebnisse mit „3 oder mehr Sternen“ an)  |
|builtin.places.reservation_number | book a table for `seven` people (Buche einen Tisch für „sieben“ Personen) <br/>  make a reservation for `two` at il fornaio (Reservierung für „zwei“ Personen im Il Fornaio) |
|builtin.places.results_number | show me the `10` coffee shops closest to here (Zeige die „10“ Cafés an, die am nächsten liegen) <br/> show me top `3` aquariums (Zeige die besten „3“ Aquarien an)  
|builtin.places.service_provided | where can I go to `whale watch` by bus? (Wohin kann ich zum „Wale beobachten“ mit dem Bus fahren?) <br/> I need a mechanic to `fix my brakes` (Ich brauche einen Mechaniker zum „Bremsen reparieren“) |    
|builtin.places.time | I want places that are open on Saturday at `8 am` (Zeige Läden an, die samstags um „8 Uhr“ geöffnet sind)| is mashiko open on `now` (Hat Mashiko „jetzt“ geöffnet?) |
|builtin.places.transportation_company | train schedules for `new jersey transit` (Zugfahrpläne für „New Jersey Transit“) <br/> can I get there on `bart` (Kann ich per „Bart“ dorthin fahren?) | 
|builtin.places.transportation_type | where is a music store i can get to `on foot`? (Wo ist ein Musikgeschäft, das „zu Fuß“ erreichbar ist?) <br/>  give me `biking` directions to mashiko (Wegbeschreibung per „Fahrrad“ zu Mashiko)|
|builtin.places.travel_time | I want to be able to drive `less than 15 minutes` (Ich möchte „weniger als 15 Minuten“ fahren müssen) <br/>   I want somewhere I can get to in `under 15 minutes` (Ich möchte einen Ort, der in „unter 15 Minuten“ erreichbar ist) |   
|builtin.reminder.absolute_location | remind me to call my dad when i land in `chicago` (Erinnere mich daran, nach der Landung in „Chicago“ meinen Vater anzurufen) <br/> when I get back to `seattle` remind me to get gas (Nach der Rückkehr nach „Seattle“ an Tanken erinnern) |
|builtin.reminder.contact_name | when `bob` calls, remind me to tell him the joke (Erinnere mich daran, den Witz zu erzählen, wenn „Bob“ anruft) <br/> create a reminder to mention the school bus when I talk to `arthur` (Erstelle eine Erinnerung, dass ich den Schulbus erwähne, wenn ich mit „Arthur“ rede) |
|builtin.reminder.leaving_absolute_location | reminder pick up craig when leaving `1200 main` (Erinnerung: Craig abholen, wenn ich „1200 Main“ verlasse) |
|builtin.reminder.leaving_implicit_location | remind me to get gas when I leave `work` (Erinnere mich daran zu tanken, wenn ich von der „Arbeit“ nach Hause fahre)  |
|builtin.reminder.original_start_date | change the reminder about the lawn from `saturday` to Sunday (Ändere die Erinnerung an den Rasen von „Samstag“ in Sonntag) <br/> move my reminder about school from `monday` to Tuesday (Meine Erinnerung zur Schule von „Montag“ auf Dienstag verschieben)   |
|builtin.reminder.relationship_name | when my `husband` calls, remind me to tell him about the pta meeting (Wenn mein „Ehemann“ anruft, erinnere mich daran, ihn über die PTA-Besprechung zu informieren) <br/> remind me again when `mom` calls (Erneut erinnern, wenn „Mama“ anruft)|
|builtin.reminder.reminder_text | can you remind me to `bring up my small spot of patchy skin` when dr smith'calls (Erinnere mich daran, „meine kleine fleckige Hautstelle zu erwähnen“, wenn Dr. Smith anruft)  <br/> remind me to `pick up dry cleaning` at 4:40 (Erinnere mich daran, um 16:40 Uhr die „Kleidungsstücke aus der Reinigung abzuholen“)   |
|builtin.reminder.start_date | remind me the `thursday after next` at 8 pm (Erinnere mich „übernächsten Donnerstag“ um 20 Uhr)  <br/> remind me `next thursday the 18th` at 8 pm (Erinnere mich „nächsten Donnerstag, den 18.“ um 20 Uhr)    |
|builtin.reminder.start_time | create a reminder `in 30 minutes` (Erinnerung „in 30 Minuten“ erstellen) <br/> create a reminder to water the plants `this evening at 7` (Erinnerung für das Blumengießen „heute Abend um 19 Uhr“ erstellen) |  
|builtin.weather.absolute_location | will it rain in `boston` (Gibt es in „Boston“ Regen?) <br/> what's the forecast for `seattle`? (Wie ist die Vorhersage für „Seattle“?)  |
|builtin.weather.date_range | weather in nyc `this weekend` (Wetter in New York City „an diesem Wochenende“) <br/>   look up the `five day` forecast in hollywood florida (Suche nach der „5-Tages“-Vorhersage für Hollywood, Florida) |
|builtin.weather.suitable_for | can I go `hiking` in shorts this weekend? (Kann ich an diesem Wochenende in Shorts „wandern“ gehen?)   <br/> will it be nice enough to `walk` to the game today? (Ist das Wetter gut genug, um heute „zu Fuß“ zum Spiel zu gehen?) | 
|builtin.weather.temperature_unit | what is the temperature today in `kelvin` (Wie hoch ist die Temperatur heute in „Kelvin“?)   <br/> show me the temps in `celsius` (Zeige die Temperatur in „Celsius“ an) |  
|builtin.weather.time_range | does it look like it will snow `tonight`? (Sieht es für „heute Abend“ nach Schnee aus?) <br/> is it windy right `now`? (Ist es „jetzt gerade“ windig?)  |
|builtin.weather.weather_condition | show `precipitation` (Zeige den „Niederschlag“ an) <br/> how thick is the `snow` at lake tahoe now? (Wie hoch ist der „Schnee“ gerade in Lake Tahoe?)  |

