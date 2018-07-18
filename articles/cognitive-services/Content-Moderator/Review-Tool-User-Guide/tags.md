---
title: Verwenden von Tags in Azure Content Moderator | Microsoft-Dokumentation
description: Content Moderator enthält Standardtags, und Sie können darüber hinaus benutzerdefinierte Tags für die Moderation von unternehmensspezifischen Inhalten erstellen.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: add4c685c07c63944ae89f48a47ac78df28c1623
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372923"
---
# <a name="about-tags"></a>Infos zu Tags #

Neben den beiden Standardtags (a - isadult und r - isracy) können Sie benutzerdefinierte Tags für ein gezielteres Scannen erstellen. Diese benutzerdefinierten Tags stehen dann für menschliche Prüfer zur Verfügung, um sie Bildern oder Texten zuzuweisen.

## <a name="create-tags"></a>Erstellen von Tags ##

1.  Wählen Sie Tags auf der Registerkarte „Einstellungen“.

  ![Tags für die Inhaltsmoderation](images/tags-1.png)

2.  Geben Sie für den Tag einen kurzen Code bestehend aus zwei Buchstaben ein.
3.  Geben Sie einen Namen für den Tag ein. Der Name sollte kurz und beschreibend sein. Zum Beispiel „isNudity“.
4.  Geben Sie eine Beschreibung ein.
5.  Klicken Sie auf „Hinzufügen“.
6.  Wenn Sie alle gewünschten Tags erstellt haben, klicken Sie auf „Speichern“.

![Tags für die Inhaltsmoderation definieren](images/tags-2-define.png)

## <a name="using-custom-tags"></a>Verwenden von benutzerdefinierten Tags ##

Benutzerdefinierte Tags werden während der Überprüfung durch Personen verwendet. Sie werden in der Vorschau angezeigt, und der Prüfer wählt sie durch Anklicken aus.

![Tags für die Inhaltsmoderation verwenden](images/tags-3-use.png)

Sie können verschiedene Tags für verschiedene Überprüfungen deaktivieren, indem Sie das Kontrollkästchen „Ist sichtbar“ aktivieren oder deaktivieren.
 
![Tags für die Inhaltsmoderation deaktivieren](images/tags-4-disable.png)

Die beiden Standardtags „isadult“ und „isracy“ können nicht gelöscht werden, bei den von Ihnen definierten benutzerdefinierten Tags ist das jedoch möglich. Klicken Sie neben dem zu löschenden Tag auf das Mülltonnensymbol.

![Tags für die Inhaltsmoderation löschen](images/tags-5-delete.png)

## <a name="next-steps"></a>Nächste Schritte ##

Informationen zum Verwenden von Tags für die Bildmoderation finden Sie in [Überprüfen von moderierten Bildern](Review-Moderated-Images.md).
