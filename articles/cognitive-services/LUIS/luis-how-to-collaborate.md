---
title: Zusammenarbeit mit anderen Mitwirkenden an LUIS-Apps in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit anderen Mitwirkenden an LUIS-Anwendungen (Language Understanding) zusammenarbeiten können.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: c0451f7621a3c18dbf365f3a03934924c030092f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35376906"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Zusammenarbeit mit anderen an LUIS-Apps (Language Understanding)  

Sie können mit anderen zusammen an LUIS-Apps arbeiten. 

## <a name="owner-and-collaborators"></a>Besitzer und Mitarbeiter
Eine App hat einen einzelnen Besitzer, sie kann aber viele Mitarbeiter aufweisen. 

## <a name="add-collaborator"></a>Mitarbeiter hinzufügen

Um Mitarbeitern das Bearbeiten Ihrer LUIS-App zu erlauben, geben Sie auf der Seite **Einstellungen** Ihrer LUIS-App die E-Mail-Adresse des Mitarbeiters ein, und klicken Sie auf **Add collaborator** (Mitarbeiter hinzufügen).

![Mitarbeiter hinzufügen](./media/luis-how-to-collaborate/add-collaborator.png)

* Mitarbeiter können sich bei der LUIS-App anmelden und sie bearbeiten, während Sie an der App arbeiten. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Mitarbeiter können keine weiteren Mitarbeiter hinzufügen.

## <a name="set-application-as-public"></a>Festlegen der Anwendung als öffentlich
Weitere Informationen finden Sie unter [Zugriff auf Endpunkte öffentlicher Apps](luis-concept-security.md#public-app-endpoint-access).

### <a name="transfer-of-ownership"></a>Übertragen des Besitzes
Zwar unterstützt LUIS derzeit die Übertragung des Besitzes nicht, jedoch können Sie Ihre App exportieren, und ein anderer LUIS-Benutzer kann sie importieren. Zwischen den beiden Anwendungen kann es zu kleinen Unterschieden bei den LUIS-Bewertungen kommen. 
