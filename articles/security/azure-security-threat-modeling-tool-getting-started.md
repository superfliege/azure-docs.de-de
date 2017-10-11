---
title: "Erste Schritte – Microsoft Threat Modellierungstool - Azure | Microsoft Docs"
description: "Dies ist eine detailliertere Übersicht über die Hervorhebung Threat Modeling Tool in Aktion."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 2d940b42108948f4cd36a585f1e79def05fe8fd3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Erste Schritte mit dem Threat Modeling-Tool

Die Cloud- und Enterprise-Sicherheitstools-Team veröffentlicht die Threat Modeling Tool-Vorschau in diesem Jahr als eine kostenlose  **[klicken Sie zum Herunterladen](https://aka.ms/tmtpreview)**. Die Änderung im Übermittlungsmechanismus ermöglicht uns die neuesten Verbesserungen und Fehlerbehebungen für Kunden jedes Mal push öffnen Sie das Tool, das einfacher zu verwalten und verwenden.
Dieser Artikel führt Sie durch den Prozess der Einstieg in die Microsoft SDL-Bedrohung modellierungsansatz und zeigt, wie das Tool zu verwenden, um hervorragende Gefahrenmodellen als Backbone Ihres Sicherheitsprozesses entwickeln.

Dieser Artikel setzt auf vorhandenen Kenntnisse von modellierungsansatz SDL Bedrohung. Eine kurze Übersicht finden Sie unter  **[Threat Modeling Web Applications](https://msdn.microsoft.com/library/ms978516.aspx)**  und eine archivierte Version des  **[aufdecken Sicherheit Fehler mithilfe der STRIDE-Ansatzes](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  MSDN-Artikel veröffentlicht im Jahr 2006.

Zusammenfassend lässt sich schnell, umfasst der Ansatz ein Diagramm zu erstellen, Identifizieren von Bedrohungen, Verringerung und jede Entschärfung überprüfen. Hier wird ein Diagramm, das diesen Prozess hervorgehoben:

![SDL-Prozesses](./media/azure-security-threat-modeling-tool/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>Starten Sie die Bedrohungsanalyse

Wenn Sie das Threat Modeling Tool starten, sehen Sie einige Dinge, wie in der Abbildung dargestellt:

![Leeren Startseite](./media/azure-security-threat-modeling-tool/tmtstart.png)

### <a name="threat-model-section"></a>Mindern von Bedrohungen Modell Abschnitt

| Komponente                                   | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feedback, Vorschläge und Probleme-Schaltfläche** | Führt Sie der  **[MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  für alle Angaben SDL. Er bietet Ihnen die Gelegenheit durchlesen, was andere Benutzer, zusammen mit problemumgehungen und Empfehlungen tun. Sie weiterhin gefunden, wonach Sie suchen, e-Mail- tmtextsupport@microsoft.com für unser Supportteam helfen Ihnen beim                                                                                                                            |
| **Erstellen eines Modells**                          | Öffnet einen leeren Zeichenbereich für Sie Ihr Diagramm gezeichnet werden soll. Achten Sie darauf, welche Vorlage wählen Sie für das Modell verwenden möchten                                                                                                                                                                                                                                                                                                                                                                       |
| **Vorlage für neue Modelle**                 | Sie müssen die Vorlage zu verwenden, bevor Sie die Erstellung eines Modells auswählen. Unsere main Vorlage ist die Azure Threat Model-Vorlage, die Azure-spezifischen Schablonen, Bedrohungen und Minderungen enthält. Wählen Sie für generische Modelle die SDL TM-Wissensdatenbank aus der Dropdown-Menü aus. Möchten Sie eine eigene Vorlage erstellen oder senden eine neue für alle Benutzer aus? Sehen Sie sich unsere  **[Vorlage Repository](https://github.com/Microsoft/threat-modeling-templates)**  GitHub Page erfahren Sie mehr                              |
| **Öffnen Sie ein Modell**                            | <p>Öffnet eine zuvor gespeicherte gefahrenmodelle zu entwickeln. Die zuletzt geöffnet Modelle-Funktion ist sehr nützlich, wenn Sie die neuesten Dateien zu öffnen müssen. Wenn Sie die Markierung zeigen, finden Sie 2 Möglichkeiten, Modelle zu öffnen:</p><p><ul><li>Öffnen von diesem Computer – klassischen Möglichkeit des Öffnens einer Datei mit dem lokalen Speicher</li><li>Öffnen von OneDrive – Teams können Ordner in OneDrive zu speichern und freizugeben ihre Bedrohungsmodelle in einem einzigen Ort aus, um die Steigerung der Produktivität und Zusammenarbeit</li></ul></p> |
| **Handbuch mit ersten Schritten**                   | Öffnet die  **[Microsoft Threat Modeling Tool](./azure-security-threat-modeling-tool.md)**  Hauptseite                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Abschnitt "" Vorlage

| Komponente               | Details                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Neue Vorlage erstellen** | Öffnet eine leere Vorlage für Sie erstellen auf. Wenn Sie umfassende Kenntnisse im Erstellen von Vorlagen von Grund auf neu verfügen, empfiehlt es sich aus vorhandenen erstellen |
| **Öffnen des Vorlagen**       | Vorlagen für Sie Änderungen an vorhandenen öffnet                                                                                                              |

Das Team Threat Modeling Tool funktioniert ständig erleben und Verbessern von Funktionen zu. Einige kleinere Änderungen kann z. B. stattfinden im Laufe des Jahres, aber alle wichtige Änderungen erfordern in der Anleitung ständig neu geschrieben. Finden Sie in der es häufig, um sicherzustellen, dass Sie die neueste Ankündigungen erhalten.

## <a name="building-a-model"></a>Erstellen eines Modells

In diesem Abschnitt führen Sie es aus:

- Cristina (ein Entwickler)
- Ricardo (Programmmanager) und
- Ashish (ein Tester)

Durch den Prozess der Entwicklung ihres ersten Bedrohungsmodells nachvollzogen werden.

> Ricardo: Hallo Cristina, ich die Bedrohungsmodelldiagramm gearbeitet und möchten, stellen Sie sicher, dass wir haben die Details rechts. Können Sie es anzusehen helfen?
> Cristina: absolut. Werfen wir einen Blick.
> Ricardo Öffnet das Tool und gibt seinen Bildschirm für Cristina frei.

![Grundlegende Gefahrenmodells](./media/azure-security-threat-modeling-tool/basictmt.png)

> Cristina: "Ok", einfach ist, sieht aber kannst du mir durchlaufen?
> Ricardo: sicher! Dies ist die Aufteilung:
> - Unsere menschlichen Benutzer wird gezeichnet, als eine externe Entität – ein Quadrat
> - Senden sie Befehle an unsere Webserver – den Kreis
> - Der Webserver ist auf eine Datenbank (zwei parallelen Linien) zurückzugreifen.

Was Ricardo nur Cristina ergab ist ein Datenflussdiagramm, kurz für  **[Data Flow Diagram](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Das Threat Modeling Tool ermöglicht Benutzern die Angabe von Vertrauensgrenzen, angegeben durch die rote gepunktete Linien angezeigt, in denen verschiedene Entitäten im Steuerelement sind. IT-Administratoren erforderlich z. B. eine Active Directory-Systemermittlung zur Authentifizierung, sodass die Active Directory außerhalb ihrer Kontrolle werden kann.

> Cristina: Sucht nach rechts, um mir. Welche Rolle spielt die Gefahren enthalten?
> Ricardo: Lassen Sie mich erläutern.

## <a name="analyzing-threats"></a>Analysieren von Bedrohungen

Sobald er für die Analyseansicht aus der Symbol-Menüauswahl (Datei mit dem Vergrößerungsglas) ist, wird er auf eine Liste von generierten Bedrohungen Threat Modeling Tool gefunden, die basierend auf der Standardvorlage verwendet klickt auf die SDL-Ansatzes namens verwendet  **[ STRIDE (Spoofing, Manipulation, Informationen offen legen, Denial of Service oder Rechteerweiterungen)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Die Idee ist, dass mit diesen Kategorien 6 befinden sich unter einem vorhersagbaren Satz von Bedrohungen, geliefert.

Dieser Ansatz ist wie Sichern Haus Leuchten jede Tür und Fenster von einem Sperrmechanismus datenschutzanforderungen vor dem Hinzufügen von einem Alarmsystem oder nach der Dieb nachjagen.

![Grundlegenden Bedrohungen](./media/azure-security-threat-modeling-tool/basicthreats.png)

Ricardo beginnt, indem Sie das erste Element in der Liste auswählen. Hier geschieht Folgendes:

Zunächst wird die Interaktion zwischen den zwei Schablonen verbessert.

![Interaktion](./media/azure-security-threat-modeling-tool/interaction.png)

Zweite werden zusätzliche Informationen zur Bedrohung angezeigt wird, im Fenster Eigenschaften Bedrohung

![Interaktion Info](./media/azure-security-threat-modeling-tool/interactioninfo.png)

Die generierten Bedrohung hilft potenzielle dimensionsentwurf zu verstehen. Die Kategorisierung STRIDE bietet ihm eine Vorstellung auf potenzielle Angriffsflächen offen, während die zusätzliche Beschreibung ihn informieren, genau was das Problem, und potenzielle Möglichkeiten dass, um es zu mindern ist. Er kann editierbaren Felder Notizen in den Details der durch den Blocksatz schreiben oder Ändern der Priorität Bewertungen abhängig von seiner Organisation Fehler-Leiste verwenden.

Azure-Vorlagen sind zusätzliche Details zum Benutzer verstehen, nicht nur Fehlersuche, sondern auch durch Hinzufügen von Beschreibungen, Beispiele und Links zu Dokumentation zu Azure-spezifischen Fehlerbehebungsmaßnahme.

Die Beschreibung vorgenommen, ihm die Wichtigkeit der hinzufügen-Authentifizierungsmechanismus, um zu verhindern, dass Benutzer verbreitet, beachten Sie die erste Bedrohung zu verarbeitende offenzulegen. Einige Minuten in der Diskussion mit Cristina, verstehen sie die Wichtigkeit der Implementierung von Access Control und Rollen. Ricardo ausgefüllt, einige kurze Hinweise, um sicherzustellen, dass diese implementiert wurden.

Wie die Bedrohungen unter Offenlegung von Informationen Ricardo eingeflossen, realisiert er, dass die Zugriffssteuerungsplan einige schreibgeschützte Konten für Überwachung und berichtgenerierung erforderlich. Er gefragt, ob dies eine neue Bedrohung dürfte jedoch Gegenmaßnahmen übereinstimmen, wurden damit er die Bedrohung entsprechend vermerkt.
Er auch zur Offenlegung von Informationen ein wenig mehr betrachtet und realisiert, dass der Sicherungsbänder benötigt, die Verschlüsselung, einen Auftrag für das Betriebsteam geschaltet wurden.

Gilt nicht für den Entwurf aufgrund von vorhandenen Gegenmaßnahmen oder Sicherheit Bedrohungen aus der Dropdownliste Status garantiert in "Nicht zutreffend" geändert werden kann. Es gibt drei weitere Optionen: Standardauswahl muss Untersuchung – nicht gestartet – verwendet werden, um die nachverfolgung auf Elemente und ausgeglichen –, sobald diese vollständig bearbeitet wird.

## <a name="reports--sharing"></a>Berichte und Freigabe

Sobald Ricardo die Liste mit Cristina durchläuft und wichtige Hinweise, Gegenmaßnahmen/Nachweise, Priorität und Status ändert fügt, -> He wählt Berichte erstellen, die vollständiger Bericht -> speichern den Bericht druckt einen übersichtlichen Bericht für ihn mit Kollegen durchlaufen um sicherzustellen, dass die richtigen Sicherheitseinstellungen Arbeit implementiert werden.

![Interaktion Info](./media/azure-security-threat-modeling-tool/report.png)

Wenn Ricardo freigeben möchte, die Datei stattdessen, kann er einfach durch Speichern in OneDrive-Konto der Organisation tun. Sobald er ausführt, kann er den dokumentlink kopieren und seine Kollegen freigeben. 

## <a name="threat-modeling-meetings"></a>Threat Modeling Besprechungen

Wenn Ricardo seine Gefahrenmodells an seine Kollegin mit OneDrive, Ashish, die Tester gesendet, underwhelmed wurde. Schien wie Ricardo und Cristina einige wichtige für Sonderfälle übersehen, die leicht beeinträchtigt werden könnten. Seine Skepsis ist eine Ergänzung zur gefahrenmodelle zu entwickeln.

In diesem Szenario, nachdem der Gefahrenanalyse Ashish übernommen hat er aufgerufen für zwei Threat Modeling Besprechungen: eine Besprechung zum Synchronisieren auf den Prozess und führt durch die Diagramme und dann eine zweite Besprechung für die Bedrohungen überprüfen und einholen.

In der ersten Besprechung verbrachte Ashish 10 Minuten "Jeder" über die Bedrohungsanalyse SDL durchlaufen. Er dann per Pull abgerufen, von der Bedrohungsmodelldiagramm und gestartet, es im Detail erläutert. Innerhalb von fünf Minuten war eine wichtige fehlende Komponente identifiziert wurden.

Ein paar Minuten haben später Ashish und Ricardo in einer erweiterten Erläuterung wie die Web-Server erstellt wurde. Es war nicht optimal für eine Besprechung ab, um den Vorgang fortzusetzen, aber jeder vereinbart schließlich, dass die Abweichung frühzeitig erkennen, die in der Zukunft viel Zeit sparen sie konfiguriert wurde.

Das Team in der zweiten Besprechung durch die Bedrohungen mit Firmenbranding geleitet, erläutert einige Möglichkeiten, um begegnen und Deaktivieren des Gefahrenmodells angemeldet. Das Dokument in die quellcodeverwaltung eingecheckt, und bei der Entwicklung fortgesetzt.

## <a name="thinking-about-assets"></a>Darum geht, Bestand

Einige Leser, die Bedrohung, die modelliert wurden möglicherweise feststellen, dass wir Bestand überhaupt Rede noch nicht. Wir haben festgestellt, dass viele Softwareentwickler ihre Software besser verstehen als das Konzept der Bestände und welche Objekte ein Angreifer möglicherweise interessiert.

Wenn Sie zum Gefahrenmodell ein Haus zumindest die offensichtlichen, können Sie darum geht, Ihrer Familie, unersetzbarer Fotos oder wertvolle Bildmaterial starten. Vielleicht könnte Sie damit beginnen, denken, die ggf. nicht mehr funktionsfähig im und das aktuelle Sicherheitssystem. Oder Sie könnte damit beginnen, erwägen die physischen Merkmale wie dem Pool oder der vorderen Veranda. Dies sind analog zu darum geht, Anlagen, Angreifer oder Softwareentwurf. Jede dieser drei Ansätze Arbeit.

Der Ansatz Bedrohungsmodellen, die wir hier vorgestellten ist wesentlich einfacher als in der Vergangenheit Microsoft getan hat. Wurde ermittelt, dass die Software Entwurfsansatz für viele Teams gut funktioniert. Wir hoffen, die Ihnen einschließen.

## <a name="next-steps"></a>Nächste Schritte

Senden Sie Ihre Fragen, Kommentare und Probleme zu tmtextsupport@microsoft.com. **[Herunterladen](https://aka.ms/tmtpreview)**  Threat Modeling Tool für den Einstieg.