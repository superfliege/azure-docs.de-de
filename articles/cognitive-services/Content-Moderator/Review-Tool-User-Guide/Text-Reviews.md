---
title: Überprüfen von Text in Azure Content Moderator | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Text im Content Moderator überprüfen können, um seine Bewertung und die erkannten Tags anzuzeigen. Verwenden Sie die Informationen, um zu entscheiden, ob der Inhalt angemessen ist.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/03/2017
ms.author: sajagtap
ms.openlocfilehash: cb8774395b7374677e8de3b80630a2d4abf490f9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372915"
---
# <a name="review-text"></a>Überprüfen von Text

Sie können Azure Content Moderator verwenden, um Text anhand von Bewertungen und erkannten Tags zu überprüfen. Verwenden Sie die Informationen, um zu entscheiden, ob der Inhalt angemessen ist. 

## <a name="select-or-enter-the-text-to-review"></a>Auswählen oder Eingeben des zu überprüfenden Textes

Wählen Sie in Content Moderator die Registerkarte **Ausprobieren**. Wählen Sie dann die **Text**-Option, um den Startbildschirm für die Textmoderation aufzurufen. Geben Sie einen beliebigen Text ein oder übermitteln Sie den Standardtext für die automatische Textmoderation. Sie können maximal 1.024 Zeichen eingeben.

## <a name="get-ready-to-review-results"></a>Vorbereiten der Ergebnisüberprüfung

Das Prüfungstool ruft zunächst die Textmoderations-API auf. Anschließend werden mithilfe der erkannten Tags Textüberprüfungen generiert. Das Prüfungstool gleicht die Ergebnisse für Ihr Team ab.

## <a name="review-text-results"></a>Überprüfen der Textergebnisse

Die ausführlichen Ergebnisse werden in den Fenstern angezeigt. Die Ergebnisse enthalten erkannten Tags und Ausdrücke, die von der Textmoderations-API zurückgegeben wurden. Um den Auswahlstatus eines Tags zu wechseln, wählen Sie den Tag aus. Sie können auch mit beliebigen benutzerdefinierten Tags arbeiten, die Sie möglicherweise erstellt haben.

![Überprüfen der Textergebnisse](images/3-review-text-2.png)
