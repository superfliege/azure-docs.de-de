---
title: Erfordern der Benutzerzuweisung – Azure AD | Microsoft-Dokumentation
description: Informationen zum Erfordern der Benutzerzuweisung für Azure-Anwendungen
services: active-directory
documentationcenter: ''
author: kgremban
manager: mtillman
editor: ''
ms.assetid: 30b78cba-1e0f-472f-8314-f2250a9b91c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
robots: noindex
ms.openlocfilehash: b02460435edca336325e472ea910b73e7895c948
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2017
ms.locfileid: "26598828"
---
# <a name="azure-ad-and-applications-require-user-assignment"></a>Azure AD und Anwendungen: Erfordern der Benutzerzuweisung
## <a name="requiring-user-assignment"></a>Erfordern der Benutzerzuweisung
1. Melden Sie sich beim Azure-Portal mit dem Administratorkonto an.
2. Klicken Sie im Hauptmenü auf **Alle Elemente** .
3. Wählen Sie das Verzeichnis aus, das Sie für die Anwendung verwenden.
4. Klicken Sie auf die Registerkarte **Anwendungen** .
5. Wählen Sie die Anwendung aus der Liste der Programme aus, die diesem Verzeichnis zugeordnet sind.
6. Klicken Sie auf die Registerkarte **KONFIGURIEREN** .
7. Legen Sie **Benutzerzuweisung für den Zugriff auf die App erforderlich** auf "Ja" fest.
8. Klicken Sie unten auf der Seite auf die Schaltfläche **Speichern** .

Sie müssen jetzt der Anwendung Benutzer und/oder Gruppen zuweisen. Weitere Informationen finden Sie unter [Zuweisen von Benutzern zu einer Anwendung](active-directory-applications-guiding-developers-assigning-users.md) und [Zuweisen von Gruppen zu einer Anwendung](active-directory-applications-guiding-developers-assigning-groups.md).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
