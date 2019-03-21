---
title: Häufig gestellte Fragen (FAQ) – Content Moderator
titlesuffix: Azure Cognitive Services
description: Hier finden Sie häufig gestellte Fragen zu Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: 99b95a26a91d14edbef264ac3b107f8462e0ef5e
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57762176"
---
# <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

#### <a name="what-does-my-content-moderator-subscription-include"></a>Was umfasst mein Content Moderator-Abonnement?
Ihr Content Moderator-Abonnement umfasst den Zugriff auf das Prüfungstool und die APIs. Je nach Ihren Geschäftsanforderungen können Sie entweder die eine oder die andere Option oder auch beide verwenden.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>Welche Einschränkungen gibt es bezüglich der Inhalte, die über die API moderiert werden können?
Bei Verwendung der API müssen die Bilder mindestens 128 Pixel und eine Dateigröße von maximal 4 MB aufweisen. Text darf maximal 1024 Zeichen umfassen. Die Länge von Videos ist nicht beschränkt.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>Was geschieht, wenn die von der Text- oder Bild-API gefilterten Inhalte die Größenbeschränkungen überschreiten?
Die Text-API gibt einen Fehlercode zurück, der Ihnen mitteilt, dass der Text die zulässige Länge überschreitet. Die Bild-API gibt einen Fehlercode zurück, der Ihnen mitteilt, dass das Bild nicht die Größenanforderungen erfüllt.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Behalten Sie die Bilder, Texte oder Videos, die für die Moderation übermittelt werden?
Ihre Inhalte bleiben Ihr Eigentum und dürfen nicht ohne Ihre Zustimmung von Microsoft aufbewahrt werden. Microsoft verwendet branchenführende Sicherheitsmaßnahmen zum Schutz Ihrer Inhalte.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>Kann ich Content Moderator verwenden, um nach Bildern zu suchen, auf denen Kinder in nicht zulässiger Weise dargestellt sind?
 Nein. Qualifizierte Organisationen können jedoch den [PhotoDNA Cloud Service](https://www.microsoft.com/photodna "Microsoft PhotoDNA Cloud Service") verwenden, um nach derartigen Inhalten zu suchen.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>Wie vielen Überprüfungsteams kann ein Benutzen beitreten? Kann ein Benutzer zwischen den Teams wechseln?
Ein Benutzer kann jeweils nur einem Team beitreten.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>Welche Teammitgliederrollen werden vom Überprüfungstool unterstützt? Inwiefern unterscheiden sie sich?
Das Studio ermöglicht aktuell die Zuweisung von Administrator- und Prüferrollen. Die Administratoren können andere Benutzer einladen und auf die Konfigurationseinstellungen zugreifen, Prüfer hingegen können nur die Moderationsergebnisse überprüfen und Inhalte markieren oder die Markierung entfernen.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>Was ist eine Markierung? Unterstützt das Überprüfungstool benutzerdefinierte Markierungen?
Eine Markierung ist ein ein- oder zweistelliger Kurzcode, der ein Moderationskennzeichen zuweist, z.B. „a“ für „Adult“ (Erwachsene), „r“ für „Racy“ (gewagt) und so weiter. Administratoren können bei Bedarf neue Markierungen für ihr Unternehmen definieren.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>Wie viele Teammitglieder kann mein Überprüfungsteam umfassen?
Es können maximal fünf Teammitglieder – einschließlich des Administrators – hinzugefügt werden.
