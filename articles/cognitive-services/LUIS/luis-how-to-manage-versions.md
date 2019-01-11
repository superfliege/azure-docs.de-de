---
title: Verwalten von Versionen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Mithilfe von Versionen können Sie verschiedene Modelle erstellen und veröffentlichen. Es ist eine bewährte Methode, das aktuell aktive Modell in eine andere Version der App zu klonen, bevor Änderungen am Modell vorgenommen werden.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: f5f31ddee58d1d609d0ff4b2901aa38abf3147a0
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599092"
---
# <a name="use-versions-to-edit-and-test-without-impacting-staging-or-production-apps"></a>Verwenden von Versionen, um Staging- und Produktions-Apps nicht durch Bearbeitungsschritte oder Tests zu beeinträchtigen

Mithilfe von Versionen können Sie verschiedene Modelle erstellen und veröffentlichen. Es ist eine bewährte Methode, das aktuell aktive Modell in eine andere [Version](luis-concept-version.md) der App zu klonen, bevor Änderungen am Modell vorgenommen werden. 

Öffnen Sie zum Arbeiten mit Versionen Ihre App, indem Sie den jeweiligen Namen auf der Seite **Meine Apps**, dann in der oberen Leiste **Verwalten** und anschließend im linken Navigationsbereich **Versionen** auswählen. 

Aus der Liste der Versionen geht hervor, welche Versionen veröffentlicht wurden, wo sie veröffentlicht wurden und welche Version aktuell aktiv ist. 

[![Abschnitt „Verwalten“, Seite „Versionen“](./media/luis-how-to-manage-versions/versions-import.png "Abschnitt „Verwalten“, Seite „Versionen“")](./media/luis-how-to-manage-versions/versions-import.png#lightbox)

## <a name="clone-a-version"></a>Klonen einer Version

1. Wählen Sie die Version aus, die Sie klonen möchten, und wählen Sie dann auf der Symbolleiste **Klonen** aus. 

2. Geben Sie im Dialogfeld **Version klonen** einen Namen für die neue Version ein, z.B. „0.2“.

   ![Dialogfeld „Version klonen“](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
     > [!NOTE]
     > Die Versions-ID darf nur Zeichen, Ziffern oder „.“ enthalten und nicht länger als 10 Zeichen sein.
 
 Eine neue Version mit dem angegebenen Namen wird erstellt und als aktive Version festgelegt.

## <a name="set-active-version"></a>Festlegen der aktiven Version

Wählen Sie in der Liste eine Version und dann auf der Symbolleiste **Aktivieren** aus. 

[![Abschnitt „Verwalten“, Seite „Versionen“, Durchführen einer Versionsaktion](./media/luis-how-to-manage-versions/versions-other.png "Abschnitt „Verwalten“, Seite „Versionen“, Durchführen einer Versionsaktion")](./media/luis-how-to-manage-versions/versions-other.png#lightbox)

## <a name="import-version"></a>Importversion

1. Wählen Sie auf der Symbolleiste **Version importieren** aus. 

2. Geben Sie im Popupfenster **Neue Version importieren** den neuen, aus zehn Zeichen bestehenden Namen der Version ein. Wenn die Version in der JSON-Datei in der App bereits vorhanden ist, müssen Sie lediglich eine Versions-ID festlegen.

    ![Abschnitt „Verwalten“, Seite „Versionen“, Importieren einer neuen Version](./media/luis-how-to-manage-versions/versions-import-pop-up.png)

    Nachdem Sie eine Version importiert haben, wird die neue Version die aktive Version.

<a name = "export-version"></a>

## <a name="other-actions"></a>Andere Aktionen

* Um eine Version zu **löschen**, wählen Sie eine Version in der Liste und dann auf der Symbolleiste **Löschen** aus. Klicken Sie auf **OK**. 
* Um eine Version **umzubenennen**, wählen Sie eine Version in der Liste und dann auf der Symbolleiste **Umbenennen** aus. Geben Sie den neuen Namen ein, und wählen Sie **Fertig** aus. 
* Um eine Version zu **exportieren**, wählen Sie eine Version in der Liste und dann auf der Symbolleiste **Exportieren** aus. Die Datei wird auf Ihren lokalen Computer heruntergeladen. 

