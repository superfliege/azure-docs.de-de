---
title: Versionsverwaltung in LUIS-Apps in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Versionen in LUIS-Anwendungen (Language Understanding Intelligent Service) verwalten.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/29/2017
ms.author: diberry
ms.openlocfilehash: 4941cf533f1b860ead07a416d5af6f62a1978305
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226593"
---
# <a name="manage-versions"></a>Verwalten von Versionen

Jedes Mal, wenn Sie am Modell Änderungen vorgenommen haben, wird eine neue [Version](luis-concept-version.md) der App erstellt. 

## <a name="set-active-version"></a>Festlegen der aktiven Version
Öffnen Sie zum Arbeiten mit Versionen Ihre App, indem Sie den jeweiligen Namen auf der Seite **Meine Apps** und in der oberen Leiste dann **Einstellungen** auswählen.

![Seite „Versionen“](./media/luis-how-to-manage-versions/settings.png)

Auf der Seite **Einstellungen** können Sie Einstellungen für die gesamte App konfigurieren, einschließlich der Versionen und Projektmitarbeiter. 

## <a name="clone-a-version"></a>Klonen einer Version
1. Suchen Sie auf der Seite **Einstellungen** unter den Abschnitten „App-Einstellungen“ > „Projektmitarbeiter“ die Zeile mit der Version, die geklont werden soll. Wählen Sie die Auslassungspunkte (***...*** ) ganz rechts aus. 

    ![Versionszeileneigenschaften](./media/luis-how-to-manage-versions/version-section.png)

2. Wählen Sie ihn in der Liste **Klonen** aus.

    ![Auswahl der Versionszeileneigenschaften](./media/luis-how-to-manage-versions/version-three-dots-modal.png)

3. Geben Sie im Dialogfeld **Version klonen** einen Namen für die neue Version ein, z.B. „0.2“.

   ![Dialogfeld „Version klonen“](./media/luis-how-to-manage-versions/version-clone-version-dialog.png)
 
 > [!NOTE]
 > Die Versions-ID darf nur Zeichen, Ziffern oder „.“ enthalten und nicht länger als 10 Zeichen sein.
 
 Eine neue Version mit dem angegebenen Namen wird erstellt und als aktive Version festgelegt.
 
  ![Erstellte und zur Liste hinzugefügte Version](./media/luis-how-to-manage-versions/new-version.png)

 > [!NOTE]
 > Wie in der vorhergehenden Abbildung zu sehen ist, ist eine veröffentlichte Version mit einer farbigen Markierung versehen, die den Typ des Slots angibt, in dem sie veröffentlicht wurde: Produktion (grün), Staging (rot) und beides (schwarz). Für jede veröffentlichte Version werden die Trainings- und Veröffentlichungsdaten angezeigt.

## <a name="set-active-version"></a>Festlegen der aktiven Version
1. Wählen Sie auf der Seite **Einstellungen** in der Liste **Versionen** ganz rechts die Auslassungspunkte (***...***) aus.

2. Wählen Sie in der Popupliste **Als aktiv festlegen** aus.

    ![Festlegen der aktiven Version](./media/luis-how-to-manage-versions/set-active-version.png)

    Die aktive Version wird durch eine hellblaue Farbe hervorgehoben, wie im folgenden Screenshot dargestellt wird:

    ![Die aktive Version](./media/luis-how-to-manage-versions/set-active-version-done.png) 


## <a name="import-version"></a>Importversion
Eine Version kann aus einer JSON-Datei importiert werden. Nachdem Sie eine Version importiert haben, wird die neue Version die aktive Version.

**So importieren Sie eine Version**

1. Klicken Sie auf der Seite **Einstellungen** auf die Schaltfläche **Neue Version importieren**.

    ![Schaltfläche „Importieren“](./media/luis-how-to-manage-versions/import-version.png)

2. Klicken Sie auf **Durchsuchen**, und wählen Sie die JSON-Datei.

    ![Dialogfeld „Neue Version importieren“](./media/luis-how-to-manage-versions/import-version-dialog.png)

Wenn die Version in der JSON-Datei in der App bereits vorhanden ist, müssen Sie lediglich eine Versions-ID festlegen.

## <a name="export-version"></a>Exportieren einer Version
Sie können eine Version in eine JSON-Datei exportieren.

**So exportieren Sie eine Version**

1. Wählen Sie auf der Seite **Einstellungen** in der Liste **Versionen** ganz rechts die Auslassungspunkte (***...***) aus.

2. Wählen Sie in der Popupliste der Aktionen **Exportieren** und dann den Speicherort aus, an dem die Datei gespeichert werden soll.

## <a name="delete-a-version"></a>Löschen einer Version
Versionen können gelöscht werden, allerdings muss mindestens eine Version der App vorhanden sein. Sie können alle Versionen außer der aktiven Version löschen. 

1. Wählen Sie auf der Seite **Einstellungen** in der Liste **Versionen** ganz rechts die Auslassungspunkte (***...***) aus.

2. Wählen Sie in der Popupliste der Aktionen **Löschen** und dann den Speicherort aus, an dem die Datei gespeichert werden soll.

    ![Bestätigung bezüglich der Löschung einer Version](./media/luis-how-to-manage-versions/delete-menu.png) 


## <a name="rename-a-version"></a>Umbenennen einer Version
Versionen können umbenannt werden, sofern der Versionsname noch nicht verwendet wird.  

1. Wählen Sie auf der Seite **Einstellungen** in der Liste **Versionen** ganz rechts die Auslassungspunkte (***...***) aus.

2. Wählen Sie in der Popupliste der Aktionen **Umbenennen** aus.

3. Geben Sie den neuen Versionsnamen ein, und klicken Sie auf **Fertig**.

    ![Bestätigung bezüglich der Umbenennung einer Version](./media/luis-how-to-manage-versions/rename-popup.png) 
