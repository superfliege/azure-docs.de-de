---
title: 'Bearbeiten einer Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: f927e5b7ff65b82aef9d4224d22296e0fa48ad59
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451883"
---
# <a name="edit-a-knowledge-base"></a>Bearbeiten einer Wissensdatenbank

Mit QnA Maker können Sie die Inhalte Ihrer Wissensdatenbank auf einer benutzerfreundlichen Bearbeitungsoberfläche verwalten.

## <a name="edit-your-knowledge-base-content"></a>Bearbeiten der Inhalte Ihrer Wissensdatenbank

1.  Wählen Sie in der oberen Navigationsleiste **Meine Wissensdatenbanken** aus. 

    Es werden alle Dienste, die Sie erstellt haben oder die für Sie freigegeben wurden, in absteigender Reihenfolge nach dem Datum für **Letzte Änderung** angezeigt.

    ![Meine Wissensdatenbanken](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Wählen Sie eine bestimmte Wissensdatenbank aus, um Änderungen daran vorzunehmen.
 
3. Klicken Sie auf **Einstellungen**.

   Hier können Sie das Pflichtfeld „Dienstname“ bearbeiten.
  
   Sie können neue URLs hinzufügen, um neue FAQ-Inhalte zur Wissensdatenbank hinzuzufügen, indem Sie auf den Link **Wissensdatenbank verwalten > „+ URL hinzufügen“** klicken.
   
   Sie können vorhandene URLs löschen, indem Sie auf das **Löschsymbol** klicken.
   
   Wenn die Wissensdatenbank den neuesten Inhalt vorhandener URLs durchforsten soll, aktivieren Sie das Kontrollkästchen **„Aktualisieren“**, um die Wissensdatenbank mit dem neuesten URL-Inhalt zu aktualisieren.
   
Sie können unterstützte Dateidokumente als Teil der Wissensdatenbank hinzufügen, indem Sie auf **Wissensdatenbank verwalten > „+ Datei hinzufügen“** klicken.

Sie können auch jede vorhandene Wissensdatenbank importieren, indem Sie auf die Schaltfläche **„Wissensdatenbank importieren“** klicken. 
   
Die Aktualisierung der Wissensdatenbank hängt davon ab, ob der **Tarif für Verwaltung** bei der Erstellung des QnA Maker-Diensts verwendet wird, der Ihrer Wissensdatenbank zugeordnet ist. Sie können den Verwaltungstarif auch über das Azure-Portal aktualisieren, wenn erforderlich.

4. Nachdem Sie alle Änderungen an der Wissensdatenbank vorgenommen haben, klicken Sie in der oberen rechten Ecke der Seite auf **Speichern und trainieren**, um die Änderungen dauerhaft zu speichern.    

    ![Speichern und trainieren](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Wenn Sie die Seite verlassen, ohne auf „Speichern und trainieren“ zu klicken, werden die Änderungen nicht beibehalten.

## <a name="add-a-qna-pair"></a>Hinzufügen eines Fragen-und-Antworten-Paars

Wählen Sie **Fragen-und-Antworten-Paar hinzufügen** aus, um der Wissensdatenbanktabelle eine neue Zeile hinzuzufügen.

![Hinzufügen eines Fragen-und-Antworten-Paars](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Löschen eines Fragen-und-Antworten-Paars

Zum Löschen eines Fragen-und-Antworten-Paars klicken Sie in der Fragen-und-Antworten-Zeile ganz rechts auf das Symbol **Löschen**.

![Löschen eines Fragen-und-Antworten-Paars](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Hinzufügen alternativer Fragen

Fügen Sie alternative Fragen zu einem vorhandenen Fragen-und-Antworten-Paar hinzu, um die Wahrscheinlichkeit einer Übereinstimmung mit einer Benutzerabfrage zu erhöhen.

![Hinzufügen alternativer Fragen](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>Hinzufügen von Metadaten


Fügen Sie Metadatenpaare durch Auswahl des Filtersymbols hinzu.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Denken Sie daran, die Wissensdatenbank beim Durchführen von Änderungen in regelmäßigen Abständen zu speichern („Speichern und trainieren“), um einen Verlust von Änderungen zu vermeiden.

## <a name="manage-large-knowledge-bases"></a>Verwalten großer Wissensdatenbanken

1. Die Fragen und Antworten sind nach der Datenquelle **gruppiert**, aus der sie extrahiert wurden. Sie können die Datenquelle erweitern oder reduzieren.
2. Sie können in der Wissensdatenbank **suchen**, indem Sie einen Suchbegriff in das Textfeld am oberen Rand der Wissensdatenbanktabelle eingeben. Drücken Sie die EINGABETASTE, um in den Fragen, Antworten oder Metadaten danach zu suchen. Klicken Sie auf das X-Symbol, um den Suchfilter zu entfernen.
3. Mithilfe der **Paginierung** können Sie große Wissensdatenbanken verwalten.

    ![Suchen, Paginieren, Gruppieren](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Zusammenarbeiten an einer Wissensdatenbank](./collaborate-knowledge-base.md)
