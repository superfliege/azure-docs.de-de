---
title: Migrieren von Apps zum aktuellen Schema – Azure Logic Apps | Microsoft-Dokumentation
description: Migrieren Ihrer Logik-Apps zur aktuellen Schemaversion
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: 8a6925d79b225a34d980472d4fb3241ab9eb1017
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127410"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Migrieren von Logik-Apps zur aktuellen Schemaversion

Führen Sie diese Schritte aus, um Ihre vorhandenen Logik-Apps in das neueste Schema zu verschieben: 

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

2. Klicken Sie im Menü der Logik-App auf **Übersicht**. Wählen Sie auf der Symbolleiste die Option **Schema aktualisieren** aus.

   > [!NOTE]
   > Wenn Sie **Schema aktualisieren** auswählen, führt Azure Logic Apps automatisch die Migrationsschritte aus und stellt die Codeausgabe für Sie bereit. Diese Ausgabe können Sie zum Aktualisieren der Definition Ihrer Logik-App verwenden. Stellen Sie jedoch sicher, dass Sie die im folgenden Abschnitt beschriebenen **bewährten Methoden** anwenden.

   ![Schema aktualisieren](./media/connectors-schema-migration/update-schema.png)

   Die Seite „Schema aktualisieren“ wird geöffnet und zeigt einen Link zu einem Dokument an, das die Verbesserungen im neuen Schema beschreibt.

## <a name="best-practices"></a>Bewährte Methoden

Hier finden Sie einige bewährte Methoden für das Migrieren von Logik-Apps zur aktuellen Schemaversion:

* Kopieren Sie das migrierte Skript in eine neue Logik-App. Überschreiben Sie die alte Version nicht, bevor Sie Ihre Tests abgeschlossen und sich vergewissert haben, das Ihre migrierte App wie erwartet funktioniert.

* Testen Sie Ihre Logik-App, **bevor** sie in Produktion geht.

* Beginnen Sie nach Abschluss der Migration mit der Aktualisierung Ihrer Logik-Apps, um nach Möglichkeit die [verwalteten APIs](../connectors/apis-list.md) zu nutzen. Verwenden Sie beispielsweise Dropbox v2 ab jetzt überall, wo Sie bisher DropBox v1 verwendet haben.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum [manuellen Migrieren von Logik-Apps](../logic-apps/logic-apps-schema-2015-08-01.md)
