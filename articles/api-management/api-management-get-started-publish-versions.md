---
title: "Veröffentlichen von Versionen Ihrer API mithilfe von Azure API Management | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie in API Management mehrere Versionen veröffentlichen."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: d63bdd3110f5c5db3e7bfec424644fdbc8d8d90c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2017
---
# <a name="publish-multiple-versions-of-your-api"></a>Veröffentlichen mehrerer Versionen Ihrer API 

Manchmal ist es unpraktisch, wenn alle Aufrufer genau die gleiche Version Ihrer API verwenden müssen. Manchmal möchten Sie neue oder andere API-Features für einige Benutzer veröffentlichen, während andere weiterhin die API verwenden möchten, die derzeit für sie geeignet ist. Wenn Aufrufer auf eine höhere Version upgraden möchten, möchten Sie dafür einen leicht verständlichen Ansatz verwenden.  Dies ist mit **Versionen** in Azure API Management möglich. Weitere Informationen finden Sie unter [Versions & revisions](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) (Versionen und Revisionen).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer neuen Version zu einer vorhandenen API
> * Auswählen eines Versionsschemas
> * Hinzufügen der Version zu einem Produkt
> * Anzeigen der Version im Entwicklerportal

![Angezeigte Version im Entwicklerportal](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Voraussetzungen

+ Absolvieren Sie das folgende Schnellstarttutorial: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md).
+ Schließen Sie darüber hinaus das folgende Tutorial ab: [Import and publish your first API](import-and-publish.md) (Importieren und Veröffentlichen Ihrer ersten API).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-version"></a>Hinzufügen einer neuen Version

![API-Kontextmenü – Version hinzufügen](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Wählen Sie in der API-Liste **Conference API** aus.
2. Klicken Sie auf das Kontextmenü (**...**) daneben.
3. Klicken Sie auf **+ Version hinzufügen**.

    > [!TIP]
    > Versionen können auch beim Erstellen einer neuen API aktiviert werden. Wählen Sie hierzu auf dem Bildschirm **API hinzufügen** die Option **Versionsverwaltung für diese API?** aus.

## <a name="choose-a-versioning-scheme"></a>Auswählen eines Schemas für die Versionsverwaltung

Mit Azure API Management können Sie auswählen, wie Aufrufer angeben können, welche Version Ihrer API sie verwenden möchten. Hierzu wählen Sie ein **Schema für die Versionsverwaltung** aus. Mögliche Optionen für dieses Schema sind **Pfad, Header oder Abfragezeichenfolge**. In unserem Beispiel verwenden wir „Pfad“.

![Bildschirm zum Hinzufügen von Versionen](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Lassen Sie **Pfad** als **Schema für Versionsverwaltung** ausgewählt.
2. Fügen Sie **v1** als **Versionsbezeichner** hinzu.

    > [!TIP]
    > Wenn Sie **Header** oder **Abfragezeichenfolge** als Schema für die Versionsverwaltung auswählen, müssen Sie einem zusätzlichen Wert (den Namen des Headers oder Abfragezeichenfolgen-Parameters) angeben.

3. Geben Sie bei Bedarf eine Beschreibung an.
4. Klicken Sie auf **Erstellen**, um Ihre neue Version einzurichten.
5. In der API-Liste werden unter **Big Conference API** nun zwei unterschiedliche APIs angezeigt: **Original** und **v1**.

    ![Unter einer API aufgeführte Versionen im Azure-Portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Wenn Sie eine Version für eine API ohne Versionsverwaltung hinzufügen, wird immer ein **Original** hinzugefügt, das auf die Standard-URL reagiert. Dadurch wird sichergestellt, dass bereits vorhandene Aufrufer durch das Hinzufügen einer Version nicht beeinträchtigt werden. Wenn Sie Versionen gleich bei der Erstellung einer neuen API aktivieren, wird kein Original erstellt.

6. Nun können Sie **v1** als eine API bearbeiten und konfigurieren, die vom **Original** getrennt ist. Änderungen an einer Version haben keine Auswirkungen auf die andere.

## <a name="add-the-version-to-a-product"></a>Hinzufügen der Version zu einem Produkt

Damit Ihre neue Version für Aufrufer sichtbar ist, muss sie einem **Produkt** hinzugefügt werden. (Produkte werden nicht von übergeordneten Versionen geerbt.)

1. Klicken Sie auf der Dienstverwaltungsseite auf **Produkte**.
2. Klicken Sie auf **Unbegrenzt**.
3. Klicken Sie auf **APIs**.
4. Wählen Sie **Hinzufügen**.
5. Klicken Sie auf **Conference API, Version v1**.
6. Kehren Sie zur Dienstverwaltungsseite zurück, und klicken Sie auf **APIs**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Anzeigen der Version im Entwicklerportal

1. Klicken Sie im oberen Menü auf **Entwicklerportal**.
2. Klicken Sie auf **APIs**. Wie Sie sehen, werden für **Conference API** die Versionen **Original** und **v1** angezeigt.
3. Klicken Sie auf **v1**.
4. Beachten Sie die **Anforderungs-URL** des ersten Vorgangs in der Liste. Dort sehen Sie, dass der API-URL-Pfad **v1** enthält.

    ![API-Kontextmenü – Version hinzufügen](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer neuen Version zu einer vorhandenen API
> * Auswählen eines Versionsschemas 
> * Hinzufügen der Version zu einem Produkt
> * Anzeigen der Version im Entwicklerportal

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Upgraden und Skalieren](upgrade-and-scale.md)