---
title: Was ist die benutzerdefinierte Bing-Suche? | Microsoft-Dokumentation
description: Enthält eine allgemeine Übersicht über die benutzerdefinierte Bing-Suche.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: b6f50844d6571cca6d63c1db7a85863e3d22d411
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948076"
---
# <a name="what-is-bing-custom-search"></a>Was ist die benutzerdefinierte Bing-Suche?

Die benutzerdefinierte Bing-Suche ermöglicht das Erstellen einer maßgeschneiderten Suchbenutzeroberfläche für Themen, die Sie interessieren. Wenn Sie beispielsweise Besitzer einer Website sind, die eine Suchbenutzeroberfläche umfasst, können Sie die Domänen, Websites und Webseiten angeben, die Bing durchsucht. Ihre Benutzer sehen Suchergebnisse, die auf den gewünschten Inhalt zugeschnitten sind, anstatt durch allgemeine Suchergebnisse mit irrelevanten Inhalten blättern zu müssen.

Um Ihre benutzerdefinierte Ansicht des Webs zu erstellen, verwenden Sie das [Portal](https://customsearch.ai) für die benutzerdefinierte Bing-Suche. Im Portal können Sie eine Instanz für die benutzerdefinierte Suche erstellen, mit der die von Bing zu durchsuchenden Domänen, Websites und Webseiten sowie die Websites angegeben werden, die nicht durchsucht werden sollen. Zusätzlich zur Angabe der URLs der Inhalte, die Ihnen bekannt sind, können Sie auch das Portal nutzen, um relevante Inhalte zu ermitteln, die Sie ggf. hinzufügen möchten.

Im Portal können Sie auch eine bestimmte Webseite am Anfang des Suchergebnisses anheften, wenn der Benutzer einen bestimmten Suchbegriff eingibt. 

Nach dem Definieren Ihrer Instanz können Sie die benutzerdefinierte Suche in Ihre Website, Desktop-App oder mobile App integrieren, indem Sie die API für die benutzerdefinierte Suche aufrufen. Wenn Sie über eine webbasierte Website oder Anwendung verfügen, können Sie der gehosteten Benutzeroberfläche das Rendern der Suchoberfläche für Sie überlassen.

In der folgenden Abbildung ist dargestellt, wie einfach die benutzerdefinierte Suche integriert werden kann.

![picture alt](./media/bcs-overview.png "Funktionsweise der benutzerdefinierten Bing-Suche")

## <a name="adding-custom-search-box-suggestions"></a>Hinzufügen eines Felds für benutzerdefinierte Suchvorschläge

Durch ein Feld für benutzerdefinierte Suchvorschläge können Sie die Funktion für benutzerdefinierte Suchen optimieren. Mit diesem Feature erhalten Sie benutzerdefinierte Suchvorschläge, die für Ihre Suche relevant sind. Wenn der Benutzer das gesuchte Schlüsselwort in das Suchfeld eingibt, wird eine Dropdownliste mit empfohlenen Abfragezeichenfolgen basierend auf der partiellen Abfragezeichenfolge des Benutzers angezeigt. Sie können bestimmen, ob nur benutzerdefinierte Vorschläge zurückgegeben oder auch Bing-Vorschläge eingeschlossen werden. [Weitere Informationen](define-custom-suggestions.md)

## <a name="adding-custom-image-search-experience"></a>Hinzufügen einer Funktion zur benutzerdefinierten Bildersuche

Durch Bilder können Sie die Funktion für benutzerdefinierte Suchen optimieren. Ähnlich wie bei Webergebnissen wird bei der benutzerdefinierten Suche das Suchen nach Bildern in der Liste von Websites in Ihrer Instanz ermöglicht. [Weitere Informationen](get-images-from-instance.md)

## <a name="adding-custom-video-search-experience"></a>Hinzufügen einer Funktion zur benutzerdefinierten Videosuche

Durch Videos können Sie die Funktion für benutzerdefinierte Suchen optimieren. Ähnlich wie bei Webergebnissen wird bei der benutzerdefinierten Suche das Suchen nach Videos in der Liste von Websites in Ihrer Instanz ermöglicht. [Weitere Informationen](get-videos-from-instance.md)

## <a name="sharing-your-custom-search-instance-with-others"></a>Freigeben von Instanzen für benutzerdefinierte Suchen für andere Benutzer

Sie können auf einfache Weise das gemeinsame Bearbeiten und Testen Ihrer Instanz ermöglichen, indem Sie sie für Mitglieder Ihres Teams freigeben. [Weitere Informationen](share-your-custom-search.md)

## <a name="next-steps"></a>Nächste Schritte

Informationen zum schnellen Einstieg finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).

Weitere Informationen zum Anpassen Ihrer Suchinstanz finden Sie unter [Konfigurieren der Benutzeroberfläche für die benutzerdefinierte Suche](define-your-custom-view.md).

Machen Sie sich mit dem Referenzinhalt für die einzelnen Endpunkte für die benutzerdefinierte Suche vertraut. Die Referenz enthält die Endpunkte, Header und Abfrageparameter, die Sie zum Anfordern von Suchergebnissen verwenden können. Darüber hinaus finden Sie dort Definitionen der Antwortobjekte.

- [API für die benutzerdefinierte Suche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [API für die benutzerdefinierte Bildersuche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [API für die benutzerdefinierte Videosuche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [API für die benutzerdefinierte Vorschlagssuche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Machen Sie sich vorher unbedingt mit den [Verwendungs- und Anzeigeanforderungen von Bing](./use-and-display-requirements.md) vertraut, um nicht gegen die Regeln für die Verwendung der Suchergebnisse zu verstoßen.