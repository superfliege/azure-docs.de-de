---
title: Best Practices – QnA Maker – Azure Cognitive Services | Microsoft-Dokumentation
description: Nutzen Sie diese bewährten Methoden, um Ihre Knowledge Base zu verbessern und bessere Ergebnisse für die Endbenutzer Ihrer Anwendung bzw. Ihres Chatbots zu liefern.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 7a85ebbc3892a90e98e73a73425c1f8ec1de0b35
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "35378688"
---
# <a name="best-practices"></a>Bewährte Methoden
Die Anleitungen zum [Entwicklungszyklus einer Knowledge Base](../Concepts/development-lifecycle-knowledge-base.md) helfen Ihnen bei sämtlichen Schritten der Verwaltung Ihrer Knowledge Base. Nutzen Sie diese bewährten Methoden, um Ihre Knowledge Base zu verbessern und bessere Ergebnisse für die Endbenutzer Ihrer Anwendung bzw. Ihres Chatbots zu liefern.

## <a name="extraction"></a>Extraktion
QnA Maker optimiert kontinuierlich die Algorithmen zum Extrahieren von Fragen und Antworten (QnA) aus Inhalten und erweitert die Liste der unterstützten Datei- und HTML-Seitenformate. Befolgen Sie die [Anleitungen](../Concepts/data-sources-supported.md) zum Extrahieren für Ihren Typ von Ausgangsdokumenten. 

Ganz allgemein sollten die Seiten mit häufig gestellten Fragen eigenständig bereitgestellt und nicht mit anderen Informationen kombiniert werden. Produkthandbücher sollten klare Überschriften und vorzugsweise eine Indexseite aufweisen. 

## <a name="rankingmatching"></a>Rangfolge/Abgleich
Nutzen Sie unbedingt auch die von QnA Maker unterstützten Rangfolgefeatures. Damit erhöhen Sie die Wahrscheinlichkeit, dass eine bestimmten Benutzerfrage angemessen beantwortet wird.

### <a name="add-alternate-questions"></a>Hinzufügen alternativer Fragen
[Alternative Fragen](../How-To/edit-knowledge-base.md) verbessern die Wahrscheinlichkeit einer Übereinstimmung mit einer Benutzerfrage. Alternative Fragen sind besonders dann nützlich, wenn es mehrere Möglichkeiten gibt, die gleiche Frage zu stellen. Dies kann Änderungen in der Satzstruktur (z.B. *„Gibt es Parkmöglichkeiten?“* oder *„Haben Sie einen Parkplatz?“* ) oder Änderungen am Sprachstil wie Umgangssprache (z.B. *„Hallo“* anstelle von *„Hi“* oder *„Guten Tag“*) einschließen.

### <a name="use-metadata-filters"></a>Verwenden von Metadatenfiltern
[Metadaten](../How-To/edit-knowledge-base.md) bieten eine zusätzliche Möglichkeit, die Ergebnisse einer Benutzerfrage basierend auf Filtern einzugrenzen. Die Antwort aus der Knowledge Base kann basierend auf Metadatentags variieren, selbst wenn die Frage identisch ist. So hat beispielsweise *„Wo ist der Parkplatz?“* eine andere Antwort, wenn ein anderer Standort für eine Filiale der Restaurantkette verwendet wird: Die Metadaten sind für *Standort: Seattle* anders als für *Standort: Redmond*.)

### <a name="use-synonyms"></a>Verwenden von Synonymen
Auch wenn für die englische Sprache Synonyme teilweise unterstützt werden, verwenden Sie [Wortvarianten](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd), um Synonyme für Schlüsselwörter hinzuzufügen, die unterschiedliche Formen aufweisen (Beispiel: *kaufen* -> *erwerben*  oder *Net Banking* -> *Internetbanking*. Synonyme sollten auf QnA Maker-Dienstebene hinzugefügt und für alle Knowledge Bases im Dienst gemeinsam verwendet werden.

### <a name="use-distinct-words-to-differentiate-questions"></a>Verwenden unterschiedlicher Wörter für die Unterscheidung von Fragen
Die QnA Maker-Algorithmen für Übereinstimmungen und Rangfolgen, die eine Benutzerfrage einer Frage in der Knowledge Base zuordnen, funktionieren am besten, wenn jede Frage unterschiedliche Anforderungen behandelt. Die Wiederholung derselben Wortgruppe in unterschiedlichen Fragen reduziert die Wahrscheinlichkeit, dass die richtige Antwort für eine bestimmte Benutzerfrage mit diesen Wörtern ausgewählt wird.

## <a name="collaborate"></a>Zusammenarbeiten
QnA Maker ermöglicht Benutzern das [Zusammenarbeiten](../How-to/collaborate-knowledge-base.md) an einer Knowledge Base. Die Benutzer benötigen Zugriff auf die Azure QnA Maker-Ressourcengruppe, um auf Knowledge Bases zugreifen zu können. Einige Organisationen lagern die Bearbeitung und Verwaltung ihrer Knowledge Base aus, möchten aber eventuell trotzdem weiterhin den Zugriff auf ihre Azure-Ressourcen schützen. Dieses Modell aus Bearbeitenden und Genehmigenden erfolgt durch das Einrichten von zwei identischen [QnA Maker-Diensten](../How-to/set-up-qnamaker-service-azure.md) in unterschiedlichen Abonnements, von denen einer für den Bearbeitungs- und Testzyklus verwendet wird. Nach Abschluss der Tests können die Inhalte der Knowledge Base mit einem [Import/Export](../Tutorials/migrate-knowledge-base.md)-Prozess auf den QnA Maker-Dienst des Genehmigenden übertragen werden, der die Knowledge Base am Ende veröffentlicht und den Endpunkt aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bearbeiten einer Knowledge Base](../How-to/edit-knowledge-base.md)

