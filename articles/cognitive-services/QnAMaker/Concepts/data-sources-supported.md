---
title: Unterstützte Datenquellen – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Unterstützte Datenquellen
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 698f96b15a9387cd30d26e684ed03ff4cc3346a7
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697899"
---
# <a name="data-sources"></a>Datenquellen 
QnA Maker kann automatisch Frage-Antwort-Paare aus gebräuchlichen semistrukturierten Inhaltsformaten wie Seiten mit häufig gestellten Fragen (FAQs) und Produkthandbüchern extrahieren. Inhalte können der Wissensdatenbank auch aus strukturierten Dateien hinzugefügt werden.

## <a name="plain-faq-pages"></a>Einfache FAQ-Seiten
Dies ist der am häufigsten verwendete Typ einer FAQ-Seite, bei der auf der gleichen Seite die Antworten unmittelbar auf die Fragen folgen. 

![Einfache FAQ-Seite](../media/qnamaker-concepts-datasources/plain-faq.png) 

 

## <a name="faq-pages-with-section-links"></a>FAQ-Seiten mit Abschnittsverknüpfungen 
Bei diesem FAQ-Seitentyp sind Fragen zusammengefasst und mit Antworten verknüpft, die sich in verschiedenen Abschnitten auf derselben Seite befinden.

 ![FAQ-Seite mit Abschnittsverknüpfung](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


## <a name="faq-pages-with-links-to-different-pages"></a>FAQ-Seiten mit Links zu anderen Seiten 
Dieser FAQ-Seitentyp ähnelt einer FAQ-Seite mit Abschnittsverknüpfung, mit dem Unterschied, dass die Links zu einer anderen Seite führen. QnA Maker durchsucht alle verknüpften Seiten, um die entsprechenden Antworten zu extrahieren.

 ![FAQ-Seite mit Deep-Link](../media/qnamaker-concepts-datasources/deeplink-faq.png) 


## <a name="product-manuals"></a>Produkthandbücher

Bei einem Handbuch handelt es sich in der Regel um Anleitungen, die mit einem Produkt geliefert werden. Es hilft dem Benutzer beim Einrichten, Verwenden und Warten des Produkts sowie der Problembehandlung. Wenn QnA Maker ein Handbuch verarbeitet, werden die Überschriften und Unterüberschriften als Fragen und die nachfolgenden Inhalte als Antworten extrahiert. Ein Beispiel finden Sie [hier](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

> [!NOTE]
> Das Extrahieren funktioniert am besten bei Handbüchern, die über ein Inhaltsverzeichnis und/oder eine Indexseite verfügen und eine klare Struktur mit hierarchischen Überschriften aufweisen.


## <a name="structured-data-format-through-file-upload"></a>Strukturiertes Datenformat durch Dateiupload

Strukturierte Dateien wie TSV und XLSX mit formatierten Spalten können während der Erstellung der Wissensdatenbank ebenfalls in QnA Maker hochgeladen werden. Sie können Dateien auch über die Registerkarte **Einstellungen** einer Wissensdatenbank hochladen.

| Frage  | Antwort  | Metadaten                |
|-----------|---------|-------------------------|
| Frage1 | Antwort1 | `Key1:Value1|Key2:Value2` |
| Frage2 | Antwort2 |      `Key:Value`           |
Alle weiteren Spalten in der Quelldatei werden ignoriert.

## <a name="structured-data-format-through-import"></a>Strukturiertes Datenformat durch Import
Beim Importieren einer Wissensdatenbank wird der Inhalt der vorhandenen Wissensdatenbank ersetzt. Der Import erfordert eine strukturierte TSV-Datei, die Informationen zur Datenquelle enthält. Diese Informationen helfen QnA Maker beim Gruppieren der Frage-Antwort-Paare und dem Zuweisen zu einer bestimmten Datenquelle.

| Frage  | Antwort  | Quelle| Metadaten                |
|-----------|---------|----|---------------------|
| Frage1 | Antwort1 | URL1|`Key1:Value1|Key2:Value2` |
| Frage2 | Antwort2 | Redaktionelle Änderung|    `Key:Value`       |

## <a name="editorial"></a>Redaktionelle Änderung
Wenn Sie nicht über bereits vorhandene Inhalte zum Füllen der Wissensdatenbank verfügen, können Sie diese auch redaktionell zur QnA Maker-Wissensdatenbank hinzufügen. Informationen zum Aktualisieren Ihrer Wissensdatenbank finden Sie [hier](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten eines QnA Maker-Diensts](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Weitere Informationen 

[Übersicht über QnA Maker](../Overview/overview.md)
