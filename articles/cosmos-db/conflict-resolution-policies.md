---
title: Konfliktauflösung in Azure Cosmos DB
description: Dieser Artikel beschreibt die Konfliktkategorien und Konfliktauflösungsrichtlinien in Azure Cosmos DB.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 2f219ed6c3155e8b7d3d978116e1748f6c115fea
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243885"
---
# <a name="conflict-types-and-resolution-policies"></a>Konflikttypen und Konfliktauflösungsrichtlinien

Konflikte und Konfliktauflösungsrichtlinien sind anwendbar, wenn Ihr Cosmos-Konto mit mehreren Schreibregionen konfiguriert ist.

In Cosmos DB-Konten, die mit mehreren Schreibregionen konfiguriert sind, können Updatekonflikte auftreten, wenn mehrere Writer gleichzeitig dasselbe Element in mehreren Regionen aktualisieren. Updatekonflikte lassen sich in diese drei Typen einteilen:

1. **Konflikte beim Einfügen** treten ggf. auf, wenn eine Anwendung gleichzeitig zwei oder mehr Elemente mit demselben eindeutigen Index (z.B. ID-Eigenschaft) aus zwei oder mehr Regionen einfügt. In diesem Fall werden alle Schreibvorgänge zunächst ggf. in der jeweiligen lokalen Region erfolgreich ausgeführt, basierend auf der ausgewählten Konfliktauflösungsrichtlinie wird jedoch nur ein Element mit der ursprünglichen ID committet.
2. **Konflikte beim Ersetzen** treten ggf. auf, wenn eine Anwendung ein einzelnes Element aus mindestens zwei Regionen gleichzeitig aktualisiert.
3. **Konflikte beim Löschen** treten ggf. auf, wenn eine Anwendung ein Element in einer Region löscht und es gleichzeitig in einer anderen Region aktualisiert.

## <a name="conflict-resolution-policies"></a>Konfliktlösungsrichtlinien

Cosmos DB bietet einen flexiblen, richtlinienbasierten Mechanismus zum Auflösen von Updatekonflikten. Diese beiden Konfliktauflösungsrichtlinien stehen Ihnen für einen Cosmos-Container zur Verfügung:

- **Letzter Schreibvorgang gewinnt (Last-Write-Wins, LWW)** : Diese Richtlinie nutzt standardmäßig eine vom System definierte Zeitstempeleigenschaft (basierend auf dem Protokoll zur Zeitsynchronisierung). Alternativ können Sie mit Cosmos DB beim Verwenden der SQL-API jede andere benutzerdefinierte numerische Eigenschaft (auch „Konfliktauflösungspfad“ genannt) angeben, die zur Konfliktauflösung verwendet werden soll.  

Wenn beim Einfügen oder Ersetzen ein Konflikt zwischen mindestens zwei Elementen auftritt, erhält das Element Vorrang, das den höchsten Wert für den „Konfliktauflösungspfad“ enthält. Wenn mehrere Elemente denselben numerischen Wert für den Konfliktauflösungspfad aufweisen, bestimmt das System den „Gewinner“. Alle Regionen konvergieren zu einem einzigen Gewinner und erhalten die identische Version des committeten Elements. Wenn Konflikte beim Löschen auftreten, gewinnt diese Version immer gegenüber Konflikten beim Einfügen oder Ersetzen, unabhängig vom Wert des Konfliktauflösungspfads.

> [!NOTE]
> LWW ist die standardmäßige Konfliktauflösungsrichtlinie und für SQL-, Tabellen-, MongoDB-, Cassandra- und Gremlin-APIs verfügbar.

Weitere Informationen finden Sie unter [Anwendungsbeispiele für LWW-Konfliktauflösungsrichtlinien](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Benutzerdefiniert**: Diese Richtlinie ist für eine anwendungsdefinierte Semantik zum Abgleichen von Konflikten konzipiert. Beim Festlegen dieser Richtlinie für Ihren Cosmos-Container müssen Sie eine gespeicherte Mergeprozedur registrieren, die automatisch aufgerufen wird, wenn Updatekonflikte bei einer Datenbanktransaktion auf Serverseite erkannt werden. Das System garantiert genau eine Ausführung der Mergeprozedur im Rahmen des Commitprotokolls.  

Wenn Sie Ihren Container jedoch mit der Option für die benutzerdefinierte Auflösung konfigurieren, jedoch keine Mergeprozedur im Container registriert wird oder die Mergeprozedur zur Laufzeit eine Ausnahme auslöst, werden die Konflikte in den Konfliktfeed geschrieben. Ihre Anwendung muss die Konflikte im Konfliktfeed dann manuell auflösen. Weitere Informationen finden Sie unter [Anwendungsbeispiele für benutzerdefinierte Auflösungsrichtlinien und den Konfliktfeed](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

> [!NOTE]
> Die benutzerdefinierte Konfliktauflösungsrichtlinie ist nur für SQL-API-Konten verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel erläutern, wie Sie Richtlinien zur Konfliktauflösung konfigurieren:

* [Verwenden der LWW-Konfliktauflösungsrichtlinie](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Verwenden der benutzerdefinierten Konfliktauflösungsrichtlinie](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Verwenden des Konfliktfeeds](how-to-manage-conflicts.md#read-from-conflict-feed)
