---
title: Häufig gestellte Fragen zu Azure Traffic Analytics | Microsoft-Dokumentation
description: Hier finden Sie Antworten auf einige der häufig gestellten Fragen zu Azure Traffic Analytics.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: fd97e0ca7615691c537dcb1dc18643627046742d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Häufig gestellte Fragen zu Traffic Analytics

1.  Welche Voraussetzungen müssen zur Verwendung von Traffic Analytics erfüllt sein?

    Für Traffic Analytics gelten die folgenden Voraussetzungen:

    - Ein Abonnement mit aktiviertem Network Watcher
    - Für die zu überwachenden Netzwerksicherheitsgruppen aktivierte NSG-Datenflussprotokolle
    - Ein Azure Storage-Konto zum Speichern von unformatierte Datenflussprotokollen
    - Ein Log Analytics (OMS)-Arbeitsbereich mit Lese- und Schreibzugriff

2.  In welchen Azure-Regionen ist Traffic Analytics verfügbar?

    Als Vorschauversion können Sie Traffic Analytics für Netzwerksicherheitsgruppen in einer der folgenden **unterstützten Regionen** verwenden: USA, Westen-Mitte; USA, Osten; USA, Osten 2; USA, Norden-Mitte; USA, Süden-Mitte; USA, Mitte; USA, Westen; USA, Westen 2; Europa, Westen; Europa, Norden; Vereinigtes Königreich, Westen; Vereinigtes Königreich, Süden; Australien, Osten; Australien, Südosten. Der Log Analytics-Arbeitsbereich muss in einer der folgenden Regionen vorhanden sein: USA, Westen-Mitte; USA, Osten; Europa, Westen; Australien, Südosten; Vereinigtes Königreich, Süden.

3.  Können sich die Netzwerksicherheitsgruppen, für die ich Datenflussprotokolle aktiviere, in anderen Regionen befinden als mein OMS-Arbeitsbereich?

    Ja

4.  Können mehrere Netzwerksicherheitsgruppen in einem einzigen Arbeitsbereich konfiguriert werden?

    Ja

5.  Kann ich einen bestehenden OMS-Arbeitsbereich verwenden?

    Ja, wenn Sie einen bestehenden Arbeitsbereich auswählen, vergewissern Sie sich, dass dieser zur neuen Abfragesprache migriert wurde. Wenn Sie kein Upgrade des Arbeitsbereichs ausführen möchten, müssen Sie einen neuen Arbeitsbereich anlegen. Weitere Informationen über die neue Abfragesprache finden Sie unter [Upgrade von Azure Log Analytics auf die neue Protokollsuche](../log-analytics/log-analytics-log-search-upgrade.md).

6.  Kann mein Azure Storage-Konto zu einem Abonnement und mein OMS-Arbeitsbereich zu einem anderen Abonnement gehören?

    Ja

7.  Kann ich unformatierte Protokolle in unterschiedlichen Storage-Accounts in verschiedenen Abonnements speichern?

    Nein. Sie können unformatierte Protokolle in jedem beliebigen Storage-Konto speichern, in dem eine Netzwerksicherheitsgruppe für Datenflussprotokolle aktiviert ist, jedoch müssen sich sowohl das Storage-Konto als auch die unformatierten Protokolle im selben Abonnement und in derselben Region befinden.

8.  Wenn mir bei der Konfiguration einer Netzwerksicherheitsgruppe für Traffic Analytics der Fehler „Nicht gefunden“ angezeigt wird, wie kann ich ihn beheben?

    Wählen Sie eine unterstützte Region aus, die in Frage 2 aufgelistet ist. Wenn Sie eine nicht unterstützte Region auswählen, erhalten Sie die Fehlermeldung „Nicht gefunden“.

9.  Unter den NSG-Datenflussprotokollen wird der NSG-Status „Fehler beim Laden“ angezeigt. Was kann ich als Nächstes tun?

    Der Microsoft.Insights-Anbieter muss registriert sein, damit die Datenflussprotokollierung ordnungsgemäß funktioniert. Wenn Sie sich nicht sicher sind, ob der Microsoft.Insights-Anbieter für Ihr Abonnement registriert ist oder nicht, ersetzen Sie *xxxxx-xxxx-xxxxx-xxxxxxxx-xxxxxx-xxxxxx* im folgenden Befehl und führen dann die folgenden Befehle über PowerShell aus:

    ```powershell-interactive
    **Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
    **Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
    ```

10. Ich habe die Lösung konfiguriert. Warum sehe ich davon nichts im Dashboard?

    Es kann bis zu 30 Minuten dauern, bis das aktualisierte Dashboard zum ersten Mal erscheint. Die Lösung muss zunächst genügend Daten aggregieren, um aussagekräftige Erkenntnisse zu gewinnen, bevor Berichte generiert werden. 

11.  Ich erhalte die folgende Meldung: „Es wurden für den ausgewählten Zeitraum keine Daten in diesem Arbeitsbereich gefunden. Ändern Sie den Zeitraum, oder wählen Sie einen anderen Arbeitsbereich aus.“ Wie kann ich das Problem lösen?

        Versuchen Sie Folgendes:
        - Versuchen Sie, den Zeitraum in der oberen Leiste zu ändern.
        - Wählen Sie in der oberen Leiste einen anderen Log Analytics-Arbeitsbereich aus.
        - Versuchen Sie, nach 30 Minuten auf Traffic Analytics zuzugreifen, wenn es kürzlich aktiviert wurde.
    
        Wenn das Problem weiterhin auftritt, wenden Sie sich an das [User Voice-Forum](https://feedback.azure.com/forums/217313-networking?category_id=195844).

12.  Ich erhalte die folgende Meldung: „1) Ihre NSG-Datenflussprotokolle werden zum ersten Mal analysiert. Dieser Vorgang kann 20 bis 30 Minuten in Anspruch nehmen. Schauen Sie später wieder vorbei. 2) Wenn der oben genannte Schritt nicht funktioniert und sich Ihr Arbeitsbereich unter der SKU „Free“ befindet, überprüfen Sie hier die Nutzung Ihres Arbeitsbereichs, um das Kontingent zu prüfen, oder suchen Sie in den häufig gestellten Fragen nach weiteren Informationen.“ Wie behebe ich dieses Problem?

        Es gibt folgende Gründe für diese Fehlermeldung:
        - Traffic Analytics wurde möglicherweise erst kürzlich aktiviert und aggregiert gerade eine ausreichende Datenmenge, um aussagekräftige Erkenntnisse zu gewinnen, bevor Berichte erstellt werden können. In diesem Fall versuchen Sie es in 30 Minuten noch einmal.
        - Ihr OMS-Arbeitsbereich befindet sich unter der SKU „Free“ und hat die Kontingentgrenzen überschritten. In diesem Fall kann es erforderlich sein, einen Arbeitsbereich in einer SKU mit größerer Kapazität zu verwenden.
    
        Wenn das Problem weiterhin auftritt, wenden Sie sich an das [User Voice-Forum](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
13.  Ich erhalte die folgende Meldung: „Es sieht so aus, als wären Ressourcendaten (Topologie), aber keine Datenflussinformationen verfügbar. Klicken Sie in der Zwischenzeit hier, um Ressourcendaten anzuzeigen, und lesen Sie die häufig gestellten Fragen für weitere Informationen.“ Wie behebe ich das Problem?

        Sie sehen die Ressourceninformationen im Dashboard, aber es gibt keine datenflussbezogenen Statistiken. Es dürfen keine Daten, da keine Kommunikationsdatenflüsse zwischen den Ressourcen stattfindet. Warten Sie 60 Minuten, und überprüfen Sie dann den Status erneut. Wenn Sie sicher sind, dass es Kommunikationsdatenflüsse zwischen Ressourcen gibt, dann wenden Sie sich an das [User Voice-Forum](https://feedback.azure.com/forums/217313-networking?category_id=195844).

14.  Was kostet Traffic Analytics?

        Solange Traffic Analytics als öffentliche Vorschauversion erhältlich ist, entstehen keine Kosten. Die Generierung von NSG-Datenflussprotokollen und die Speicherung von Daten in einem OMS-Arbeitsbereich sind entsprechend der veröffentlichen Preise kostenpflichtig.

15.  Wie kann ich mit einer Tastatur in der Kartenansicht navigieren?

        Die Seite mit der Kartenansicht enthält zwei Hauptbereiche:
    
        - **Banner**: Das Banner im oberen Bereich der Kartenansicht bietet die Möglichkeit, Filter für die Datenverkehrverteilung über Schaltflächen auszuwählen. Hierzu gehören: Bereitstellung/Keine Bereitstellung/Aktiv/Inaktiv/Datenverkehrsanalyse aktiviert/Datenverkehrsanalyse nicht aktiviert/Datenverkehr aus/Gutartiger/Bösartiger/Zulässiger bösartiger Datenverkehr und Legendeninformationen. Bei Auswahl der definierten Schaltflächen wird der entsprechende Filter auf die Karte angewendet. Wenn ein Benutzer unter dem Banner z.B. den Filter „Aktiv“ wählt, dann werden die „aktiven“ Rechenzentren Ihrer Bereitstellung in der Karte hervorgehoben.
        - **Karte**: Der Kartenbereich befindet sich unter dem Banner und zeigt die Verteilung des Datenverkehrs für Azure-Rechenzentren und Länder.
    
        **Tastaturnavigation im Banner**
    
        - Standardmäßig ist auf der Kartenseite für das Banner der Filter „Azure-Rechenzentren“ ausgewählt.
        - Um zu einer anderen Filterschaltfläche vorwärts zu navigieren, können Sie entweder die Taste `Tab` oder `Right arrow` verwenden. Um rückwärts zu navigieren, verwenden Sie entweder die Taste `Shift+Tab` oder `Left arrow`. Die Navigationsrichtung ist von links nach rechts, und dann von oben nach unten.
        - Drücken Sie `Enter` oder die Pfeiltaste `Down`, um den ausgewählten Filter anzuwenden. Je nach Filterauswahl und Bereitstellung werden ein oder mehrere Knoten unter dem Kartenbereich hervorgehoben.
            - Um zwischen **Banner** und **Karte** zu wechseln, drücken Sie `Ctrl+F6`.
        
        **Tastaturnavigation in der Karte**
    
        - Nachdem Sie einen beliebigen Filter im Banner ausgewählt und `Ctrl+F6` gedrückt haben, bewegt sich der Fokus auf einen der hervorgehobenen Knoten (**Azure-Rechenzentrum** oder **Land/Region**) in der Kartenansicht.
        - Um zu anderen hervorgehobenen Knoten in der Karte zu navigieren, können Sie entweder die Taste `Tab` oder `Right arrow` für die Vorwärtsbewegung und die Taste `Shift+Tab` oder `Left arrow` für die Rückwärtsbewegung verwenden.
        - Zur Auswahl eines markierten Knotens in der Karte verwenden Sie `Enter` oder die Taste `Down arrow`.
        - Bei Auswahl eines solchen Knotens wird der Fokus auf die **Infotoolbox** des Knotens verschoben. Standardmäßig wird der Fokus auf die geschlossene Schaltfläche in der **Infotoolbox** verschoben. Zur weiteren Navigation in der Ansicht **Box** drücken Sie auf die Tasten `Right` und `Left arrow`, um vorwärts bzw. rückwärts zu navigieren. Das Drücken von `Enter` hat die gleiche Wirkung wie die Auswahl der fokussierten Schaltfläche in der **Infotoolbox**.
        - Wenn Sie `Tab` drücken, während der Fokus auf der **Infotoolbox** liegt, bewegt sich der Fokus zu den Endpunkten auf demselben Kontinent wie der ausgewählte Knoten. Mit den Tasten `Right` und `Left arrow` können Sie durch diese Endpunkte navigieren.
        - Um zu anderen Clustern von Datenflussendpunkten bzw. Kontinenten zu navigieren, verwenden Sie `Tab` für eine Vorwärtsbewegung und `Shift+Tab` für eine Rückwärtsbewegung.
        - Sobald der Fokus auf `Continent clusters` liegt, können Sie mit `Enter` oder der Pfeiltaste `Down` die Endpunkte innerhalb des Kontinentclusters markieren. Um im Informationsfeld des Kontinentclusters durch die Endpunkte und die Schaltfläche zum Schließen zu navigieren, verwenden Sie die Tasten `Right` und `Left arrow` für die Vorwärts- bzw. Rückwärtsbewegung. An jedem beliebigen Endpunkt können Sie mit `Shift+L` zur Verbindungslinie vom ausgewählten Knoten zum Endpunkt wechseln. Durch erneutes Drücken von `Shift+L` gelangen Sie zum ausgewählten Endpunkt.
        
        Zu jedem Zeitpunkt:
    
        - `ESC` klappt die erweiterte Auswahl zu.
        - Die Taste `UP Arrow` führt die gleiche Aktion wie `ESC` aus. Die Taste `Down arrow` führt die gleiche Aktion wie `Enter` aus.
        - Verwenden Sie `Shift+Plus` zum Vergrößern und `Shift+Minus` zum Verkleinern.