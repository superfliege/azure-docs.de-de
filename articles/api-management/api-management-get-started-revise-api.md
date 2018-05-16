---
title: Gefahrloses Vornehmen geringfügiger Änderungen mithilfe von Revisionen in Azure API Management | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie mithilfe von Revisionen geringfügige Änderungen in API Management vornehmen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 864c269da61bcea885249021a44fe0259ca83e90
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Gefahrloses Vornehmen geringfügiger Änderungen mithilfe von Revisionen
Wenn Ihre API einsatzbereit ist und von Entwicklern verwendet wird, müssen Sie in der Regel bei Änderungen an der API vorsichtig sein und darauf achten, keine Aufrufe Ihrer API zu beeinträchtigen. Außerdem empfiehlt es sich, Entwickler über die vorgenommenen Änderungen zu informieren. Hierzu können Sie **Revisionen** in Azure API Management verwenden. Weitere Informationen finden Sie unter [Versions & revisions](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) (Versionen und Revisionen) und [API Versioning with Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/) (API-Versionsverwaltung mit Azure API Management).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer neuen Revision
> * Vornehmen geringfügiger Änderungen an Ihrer Revision
> * Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll
> * Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal

![Änderungsprotokoll im Entwicklerportal](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).
+ Schließen Sie darüber hinaus das folgende Tutorial ab: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

## <a name="add-a-new-revision"></a>Hinzufügen einer neuen Revision

1. Klicken Sie auf die Seite **APIs**.
2. Wählen Sie in der API-Liste den Eintrag **Conference API** aus (oder eine andere API, der Sie Revisionen hinzufügen möchten).
3. Klicken Sie im Menü im oberen Bereich der Seite auf die Registerkarte **Revisionen**.
4. Klicken Sie auf **+ Revision hinzufügen**.

    > [!TIP]
    > Alternativ können Sie im Kontextmenü (**...**) für die API auf **Revision hinzufügen** klicken.
    
    ![Menü „Revisionen“ im oberen Bereich des Bildschirms](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Geben Sie eine Beschreibung für Ihre neue Revision an, damit Sie wissen, wofür sie verwendet wird.
6. Klicken Sie auf **Erstellen**
7. Die neue Revision wird erstellt.

    > [!NOTE]
    > Ihre ursprüngliche API bleibt in **Revision 1**. Dies ist die Revision, die weiterhin von den Benutzern aufgerufen wird, bis Sie beschließen, eine andere Revision zur aktuellen Revision zu machen.

## <a name="make-non-breaking-changes-to-your-revision"></a>Vornehmen geringfügiger Änderungen an Ihrer Revision

1. Wählen Sie in der API-Liste **Conference API** aus.
2. Klicken Sie im oberen Seitenbereich auf die Registerkarte **Entwurf**.
3. Beachten Sie, dass die **Revisionsauswahl** (direkt über der Registerkarte „Entwurf“) die aktuelle Revision als **Revision 2** anzeigt.

    > [!TIP]
    > Mit der Revisionsauswahl können Sie zwischen Revisionen wechseln, die Sie bearbeiten möchten.

4. Klicken Sie auf **+ Vorgang hinzufügen**.
5. Legen Sie den neuen Vorgang auf **POST** und Name und Anzeigename des Vorgangs auf **test** fest.
6. **Speichern** Sie Ihren neuen Vorgang.
7. Sie haben nun eine Änderung an **Revision 2** vorgenommen. Wechseln Sie mithilfe der **Revisionsauswahl** im oberen Seitenbereich zu **Revision 1** zurück.
8. Wie Sie sehen, ist der neue Vorgang in **Revision 1** nicht vorhanden. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll
1. Klicken Sie im oberen Bereich der Seite auf die Registerkarte **Revisionen**.

    ![Das Revisionsmenü auf dem Revisionsbildschirm](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Öffnen Sie das Kontextmenü (**...**) für **Revision 2**.
2. Klicken Sie auf **Make Current** (Als aktuell festlegen). Aktivieren Sie **Post to Public Change log for this API** (In öffentlichem Änderungsprotokoll für diese API veröffentlichen), wenn Sie Hinweise zu dieser Änderung veröffentlichen möchten.
3. Klicken Sie auf **Post to Public Change Log for this API** (In öffentlichem Änderungsprotokoll für diese API veröffentlichen).
4. Geben Sie eine Änderungsbeschreibung für Entwickler an (beispielsweise **Testrevisionen. Neuer Vorgang „Test“ hinzugefügt**).
5. **Revision 2** ist jetzt aktuell.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal
1. Klicken Sie im Azure-Portal auf **APIs**.
2. Klicken Sie im oberen Menü auf **Entwicklerportal**.
3. Klicken Sie auf **APIs** und anschließend auf **Conference API**.
4. Wie Sie sehen, ist Ihr neuer **test**-Vorgang nun verfügbar.
5. Klicken Sie unter dem API-Namen auf **API Change History** (API-Änderungsverlauf).
6. Der Änderungsprotokolleintrag ist in der Liste enthalten.

    ![Entwicklerportal](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen einer neuen Revision
> * Vornehmen geringfügiger Änderungen an Ihrer Revision
> * Festlegen der Revision als aktuelle Revision und Hinzufügen eines Eintrags zum Änderungsprotokoll
> * Anzeigen von Änderungen und Änderungsprotokoll im Entwicklerportal

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Planbares Veröffentlichen mehrerer Versionen Ihrer API](api-management-get-started-publish-versions.md)