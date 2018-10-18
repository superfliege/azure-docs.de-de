---
title: Exportieren oder Löschen Ihrer Daten – Content Moderator
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr darüber, wie Sie Daten aus Content Moderator exportieren oder löschen können.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: pafarley
ms.openlocfilehash: f4da7cdffc2d2aad21c2ea4cfc67939d0dbba530
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339371"
---
# <a name="export-or-delete-user-data-in-content-moderator"></a>Exportieren oder Löschen von Benutzerdaten aus Content Moderator

Content Moderator sammelt Benutzerdaten, um den Dienst zu betreiben. Die Benutzer haben jedoch mithilfe der [Benutzeroberfläche für die Überprüfung](http://contentmoderator.cognitive.microsoft.com/) und der [APIs](https://docs.microsoft.com/azure/cognitive-services/content-moderator/api-reference) Vollzugriff auf das Anzeigen, Exportieren und Löschen ihrer Daten.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Weitere Informationen zum Exportieren und Löschen von Benutzerdaten aus Content Moderator finden Sie in der folgenden Tabelle.

| Daten | Exportvorgang | Löschvorgang |
| ---- | ---------------- | ---------------- |
| Kontoinformationen (Abonnementschlüssel) | N/V | Der Löschvorgang wird über das Azure-Portal (Azure-Abonnements) durchgeführt. Verwenden Sie alternativ die Schaltfläche **Team löschen** auf der Seite „Teameinstellungen“ auf der [Benutzeroberfläche für die Überprüfung](http://contentmoderator.cognitive.microsoft.com/). |
| Bilder für benutzerdefinierte Vergleiche | [Ruft Bild-IDs ab](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f676). Bilder werden in einem unidirektionalen proprietären Hashformat gespeichert, und es gibt keine Möglichkeit, das tatsächliche Bild zu extrahieren. | [Löscht alle Bilder](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f686). Löschen Sie die Content Moderator-Ressource alternativ über das Azure-Portal. |
| Begriffe für benutzerdefinierte Vergleiche | [Ruft alle Begriffe ab](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67e). | [Löscht alle Begriffe](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67d). Löschen Sie die Content Moderator-Ressource alternativ über das Azure-Portal. |
| Tags | N/V | Verwenden Sie das Symbol **Löschen**, das für jedes Tag auf der Einstellungsseite für Tags der Benutzeroberfläche für die Überprüfung verfügbar ist. Verwenden Sie alternativ die Schaltfläche **Team löschen** auf der Seite „Teameinstellungen“ auf der [Benutzeroberfläche für die Überprüfung](http://contentmoderator.cognitive.microsoft.com/). |
| Überprüfungen | [Ruft Überprüfungen ab](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2). | Verwenden Sie die Schaltfläche **Team löschen** auf der Seite „Teameinstellungen“ auf der [Benutzeroberfläche für die Überprüfung](http://contentmoderator.cognitive.microsoft.com/).
| Benutzer | N/V | Verwenden Sie das Symbol **Löschen**, das für jeden Benutzer auf Seite „Teameinstellungen“ der [Benutzeroberfläche für die Überprüfung](http://contentmoderator.cognitive.microsoft.com/) verfügbar ist. Verwenden Sie alternativ die Schaltfläche **Team löschen** auf der Seite „Teameinstellungen“ auf der [Benutzeroberfläche für die Überprüfung](http://contentmoderator.cognitive.microsoft.com/). |

