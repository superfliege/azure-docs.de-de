---
title: Azure Event Grid – Begriffe
description: In diesem Artikel werden Azure Event Grid und die zugehörigen Begriffe beschrieben. Zudem werden verschiedene Schlüsselkomponenten von Event Grid definiert.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/24/2018
ms.author: babanisa
ms.openlocfilehash: 8ddde98b448f4d6d6f24a2ee47acf9240593622c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="concepts-in-azure-event-grid"></a>Begriffe in Azure Event Grid

Dieser Artikel beschreibt die zentralen Begriffe in Azure Event Grid.

## <a name="events"></a>Ereignisse

Ein Ereignis ist die kleinste Informationsmenge, die einen Vorgang im System umfassend beschreibt. Jedes Ereignis enthält allgemeine Informationen wie Quelle des Ereignisses, Zeitpunkt, an dem das Ereignis aufgetreten ist, und den eindeutigen Bezeichner. Jedes Ereignis enthält auch spezielle Informationen, die nur für den jeweiligen Ereignistyp relevant sind. Beispielsweise enthält ein Ereignis zu einer neuen Datei, die in Azure Storage erstellt wird, Details über die Datei, z.B. den Wert von `lastTimeModified`. Alternativ dazu enthält ein Event Hubs-Ereignis die URL der Erfassungsdatei. Jedes Ereignis ist auf 64 KB Daten begrenzt.

## <a name="event-sourcespublishers"></a>Ereignisquellen/-herausgeber

Eine Ereignisquelle ist die Quelle, in der das Ereignis auftritt. Azure Storage ist z.B. die Ereignisquelle für durch Blobs erstellte Ereignisse. Das Azure VM-Fabric ist die Ereignisquelle für Ereignisse zu virtuellen Computern. Ereignisquellen sind für das Veröffentlichen von Ereignissen in Event Grid zuständig.

Informationen zum Implementieren der unterstützten Event Grid-Quellen finden Sie unter [Ereignisquellen in Azure Event Grid](event-sources.md).

## <a name="topics"></a>Themen

Herausgeber kategorisieren Ereignisse in Themen. Dieses Event Grid-Thema enthält einen Endpunkt, an den der Herausgeber Ereignisse sendet. Um auf bestimmte Arten von Ereignissen zu reagieren, legen Abonnenten fest, welche Themen sie abonnieren. Die Themen enthalten auch ein Ereignisschema, mit der Abonnenten feststellen können, wie die Ereignisse entsprechend genutzt werden.

Systemthemen sind integrierte Themen, die von Azure-Diensten bereitgestellt werden. Benutzerdefinierte Themen sind Anwendungs- und Drittanbieterthemen.

Beim Entwerfen Ihrer Anwendung können Sie flexibel entscheiden, wie viele Themen erstellt werden sollen. Erstellen Sie für große Lösungen ein benutzerdefiniertes Thema für jede Kategorie von verwandten Ereignissen. Denken Sie beispielsweise an eine Anwendung, die Ereignisse im Zusammenhang mit der Änderung von Benutzerkonten und der Verarbeitung von Bestellungen sendet. Es ist unwahrscheinlich, dass ein Ereignishandler beide Ereigniskategorien benötigt. Erstellen Sie zwei benutzerdefinierte Themen, und lassen Sie Ereignishandler das jeweils relevante Thema abonnieren. Für kleine Lösungen empfiehlt es sich ggf., alle Ereignisse an ein Thema zu senden. Ereignisabonnenten können nach den gewünschten Ereignistypen filtern.

## <a name="event-subscriptions"></a>Ereignisabonnements

Ein Abonnement gibt in Event Grid an, welche Ereignisse zu einem Thema ein Abonnent erhalten möchte. Ein Abonnement enthält auch Informationen darüber, wie Ereignisse an den Abonnenten übermittelt werden sollen.

## <a name="event-handlers"></a>Ereignishandler

In Bezug auf Event Grid ist ein Ereignishandler das Ziel, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Event Grid unterstützt mehrere Abonnententypen. Je nach Typ des Abonnenten führt Event Grid unterschiedliche Methoden durch, um die Übermittlung des Ereignisses zu gewährleisten. Bei HTTP-Webhookereignishandlern wird das Ereignis solange wiederholt, bis der Handler einen Statuscode von `200 – OK` zurückgibt. Bei Azure Storage Queue werden die Ereignisse solange wiederholt, bis der Warteschlangendienst den Nachrichtenpush in der Warteschlange verarbeiten kann.

Informationen zum Implementieren der unterstützten Event Grid-Handler finden Sie unter [Ereignishandler in Azure Event Grid](event-handlers.md).

## <a name="filters"></a>Filter

Wenn Sie ein Event Grid-Thema abonnieren, können Sie die Ereignisse filtern, die an den Endpunkt gesendet werden. Sie können nach Ereignistyp oder Betreffmuster filtern. Weitere Informationen finden Sie unter [Event Grid – Abonnementschema](subscription-creation-schema.md).

## <a name="security"></a>Sicherheit

Event Grid ermöglicht ein sicheres Abonnieren und Veröffentlichen von Themen. Für das Abonnieren sind entsprechende Berechtigungen für die Ressource oder das Event Grid-Thema erforderlich. Für das Veröffentlichen ist ein SAS-Token oder eine Schlüsselauthentifizierung für das Thema erforderlich. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).

## <a name="failed-delivery"></a>Fehlerhafte Übermittlung

Wenn Event Grid nicht bestätigen kann, dass ein Ereignis beim Endpunkt des Abonnenten eingegangen ist, wird das Ereignis erneut übermittelt. Weitere Informationen finden Sie unter [Event Grid – Nachrichtenübermittlung und -wiederholung](delivery-and-retry.md).

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in Event Grid finden Sie unter [Informationen zu Event Grid](overview.md).
* Um sich schnell mit der Verwendung von Event Grid vertraut zu machen, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).
