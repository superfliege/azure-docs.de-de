---
title: Konflikttypen und Konfliktauflösungsrichtlinien in Azure Cosmos DB
description: Dieser Artikel beschreibt die Konfliktkategorien und Konfliktauflösungsrichtlinien in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 4af3f4c60f186c34d0f5522461ba70c68b59d1ab
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033892"
---
# <a name="conflict-types-and-resolution-policies"></a>Konflikttypen und Konfliktauflösungsrichtlinien

Konflikte und Konfliktauflösungsrichtlinien sind anwendbar, wenn Ihr Azure Cosmos DB-Konto mit mehreren Schreibregionen konfiguriert ist.

In Azure Cosmos DB-Konten, die mit mehreren Schreibregionen konfiguriert sind, können Updatekonflikte auftreten, wenn Schreiber gleichzeitig dasselbe Element in mehreren Regionen aktualisieren. Updatekonflikte lassen sich in diese drei Typen einteilen:

* **Konflikte beim Einfügen**: Diese Konflikte treten ggf. auf, wenn eine Anwendung gleichzeitig zwei oder mehr Elemente mit demselben eindeutigen Index aus zwei oder mehr Regionen einfügt. Beispielsweise kann dieser Konflikt mit einer ID-Eigenschaft auftreten. Alle Schreibvorgänge werden zu Beginn möglicherweise erfolgreich in den jeweiligen lokalen Regionen ausgeführt. Aber auf Grundlage der Konfliktauflösungrichtlinie kann letztlich nur für ein Element mit der ursprünglichen ID ein Commit ausgeführt werden.

* **Konflikte beim Ersetzen**: Diese Konflikte treten ggf. auf, wenn eine Anwendung ein einzelnes Element aus zwei oder mehr Regionen gleichzeitig aktualisiert.

* **Konflikte beim Löschen**: Diese Konflikte treten ggf. auf, wenn eine Anwendung ein Element in einer Region löscht und es gleichzeitig in einer anderen Region aktualisiert.

## <a name="conflict-resolution-policies"></a>Konfliktlösungsrichtlinien

Azure Cosmos DB bietet einen flexiblen, richtlinienbasierten Mechanismus zum Auflösen von Updatekonflikten. Zwei Konfliktauflösungsrichtlinien stehen für einen Azure Cosmos DB-Container zur Auswahl:

- **Letzter Schreibvorgang gewinnt (LWW)**: Diese Auflösungsrichtlinie verwendet standardmäßig eine vom System definierte Zeitstempeleigenschaft. Sie basiert auf dem Protokoll zur Zeitsynchronisierung. Bei Verwendung der SQL-API von Azure Cosmos DB können Sie auch eine andere benutzerdefinierte numerische Eigenschaft für die Konfliktauflösung angeben. Eine benutzerdefinierte numerische Eigenschaft wird auch als Konfliktauflösungspfad bezeichnet. 

  Wenn beim Einfügen oder Ersetzen ein Konflikt zwischen mindestens zwei Elementen auftritt, erhält das Element mit dem höchsten Wert für den „Konfliktauflösungspfad“ Vorrang. Wenn mehrere Elemente denselben numerischen Wert für den Konfliktauflösungspfad aufweisen, bestimmt das System den „Gewinner“. Alle Regionen konvergieren garantiert zu einem einzigen Gewinner und erhalten dieselbe Version des Elements, für das ein Commit ausgeführt wurde. Wenn Konflikte beim Löschen beteiligt sind, „gewinnt“ immer die gelöschte Version gegenüber den Konflikten beim Einfügen oder Ersetzen. Dieses Ergebnis tritt unabhängig vom Wert des Konfliktauflösungspfads ein.

  > [!NOTE]
  > „Letzter Schreibvorgang gewinnt“ (Last Write Wins, LWW) ist der standardmäßige Konfliktauflösungsmodus. Er ist für SQL-, Azure Cosmos DB-Tabellen-, MongoDB-, Cassandra- und Gremlin-API-Konten verfügbar.

  Weitere Informationen finden Sie unter [Anwendungsbeispiele für LWW-Konfliktauflösungsrichtlinien](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Benutzerdefiniert**: Diese Auflösungsrichtlinie ist für eine anwendungsdefinierte Semantik zum Beilegen von Konflikten konzipiert. Beim Festlegen dieser Richtlinie für Ihren Azure Cosmos DB-Container müssen Sie eine gespeicherte Mergeprozedur registrieren. Diese Prozedur wird automatisch aufgerufen, wenn Konflikte bei einer Datenbanktransaktion auf dem Server erkannt werden. Das System garantiert genau eine Ausführung der Mergeprozedur im Rahmen des Commitprotokolls.  

  Zwei Punkte sind zu beachten, wenn Sie die benutzerdefinierte Auflösungsoption für Ihren Container konfigurieren. Wenn Sie keine Mergeprozedur im Container registrieren oder die Mergeprozedur zur Laufzeit eine Ausnahme auslöst, werden die Konflikte in den Konfliktfeed geschrieben. Ihre Anwendung muss die Konflikte im Konfliktfeed dann manuell auflösen. Weitere Informationen finden Sie unter [Anwendungsbeispiele für benutzerdefinierte Auflösungsrichtlinien und den Konfliktfeed](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Die benutzerdefinierte Konfliktauflösungsrichtlinie ist nur für SQL-API-Konten verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Richtlinien für die Konfliktauflösung konfigurieren. Entsprechende Informationen finden Sie in den folgenden Artikeln:

* [Verwenden der LWW-Konfliktauflösungsrichtlinie](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Verwenden der benutzerdefinierten Konfliktauflösungsrichtlinie](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Verwenden des Konfliktfeeds](how-to-manage-conflicts.md#read-from-conflict-feed)
