---
title: Exportieren oder Löschen von Daten – Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Daten aus Custom Vision Service exportieren oder löschen können.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/25/2018
ms.author: pafarley
ms.openlocfilehash: e3932c27b7741f04dfeda2a64f88a890b1e908ad
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054978"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exportieren oder Löschen von Benutzerdaten aus Custom Vision

Content Moderator sammelt Benutzerdaten, um den Dienst zu betreiben. Die Kunden haben jedoch über die [Training-API](https://go.microsoft.com/fwlink/?linkid=865446) des Custom Vision Service vollständige Kontrolle über die Anzeige, den Export und die Löschung ihrer Daten.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Weitere Informationen zum Exportieren und Löschen von Benutzerdaten aus Custom Vision finden Sie in der folgenden Tabelle.

| Daten | Exportvorgang | Löschvorgang |
| ---- | ---------------- | ---------------- |
| Kontoinformationen (Abonnementschlüssel) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Löschvorgänge werden über das Azure-Portal (Azure-Abonnements) durchgeführt. Alternativ kann auch die Schaltfläche „Ihr Konto löschen“ auf der Einstellungsseite für „CustomVision.ai“ (Microsoft-Kontoabonnements) verwendet werden. | 
| Details zur Iteration | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Details zur Iterationsleistung | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Liste der Iterationen | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Projekte und Projektdetails | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) und [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Bildtags | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) und [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Bilder | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (stellt URI für den Bilddownload bereit) und [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (stellt URI für Bilddownload bereit) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Exportierte Modelle | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Wird bei Kontolöschung gelöscht. | 
