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
ms.openlocfilehash: 7cd61fc63d0d7734b842ed222c67c6753da9a418
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35374738"
---
# <a name="what-is-bing-custom-search"></a>Was ist die benutzerdefinierte Bing-Suche?

Die benutzerdefinierte Bing-Suche ermöglicht das Erstellen einer maßgeschneiderten Suchbenutzeroberfläche für Themen, die Sie interessieren. Wenn Sie beispielsweise Besitzer einer Website sind, die eine Suchbenutzeroberfläche umfasst, können Sie die Domänen, Websites und Webseiten angeben, die Bing durchsucht. Ihre Benutzer sehen Suchergebnisse, die auf den gewünschten Inhalt zugeschnitten sind, anstatt durch allgemeine Suchergebnisse mit irrelevanten Inhalten blättern zu müssen.

Um Ihre benutzerdefinierte Ansicht des Webs zu erstellen, verwenden Sie das [Portal](https://customsearch.ai) für die benutzerdefinierte Bing-Suche. Im Portal können Sie eine Instanz für die benutzerdefinierte Suche erstellen, mit der die von Bing zu durchsuchenden Domänen, Websites und Webseiten sowie die Websites angegeben werden, die nicht durchsucht werden sollen. Zusätzlich zur Angabe der URLs der Inhalte, die Ihnen bekannt sind, können Sie auch das Portal nutzen, um relevante Inhalte zu ermitteln, die Sie ggf. hinzufügen möchten.

Im Portal können Sie auch eine bestimmte Webseite am Anfang des Suchergebnisses anheften, wenn der Benutzer einen bestimmten Suchbegriff eingibt. 

Nach dem Definieren Ihrer Instanz können Sie die benutzerdefinierte Suche in Ihre Website, Desktop-App oder mobile App integrieren, indem Sie die API für die benutzerdefinierte Suche aufrufen. Wenn Sie über eine webbasierte Website oder Anwendung verfügen, können Sie der gehosteten Benutzeroberfläche das Rendern der Suchoberfläche für Sie überlassen.

In der folgenden Abbildung ist dargestellt, wie einfach die benutzerdefinierte Suche integriert werden kann.

![picture alt](./media/bcs-overview.png "Funktionsweise der benutzerdefinierten Bing-Suche")

## <a name="customize-search-suggestions"></a>Anpassen von Suchvorschlägen

Wenn Sie über das entsprechende Abonnement für die benutzerdefinierte Bing-Suche verfügen (siehe [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), können Sie die Suchvorschläge anpassen, die Ihnen in der benutzerdefinierten Bing-Suche angezeigt werden. Die API für die benutzerdefinierte Vorschlagssuche gibt basierend auf einem Teil der Abfragezeichenfolge, die der Benutzer bereitstellt, eine Liste mit vorgeschlagenen Abfragen zurück. Mit der benutzerdefinierten Vorschlagssuche erhalten Sie benutzerdefinierte Suchvorschläge, die für Ihre Suche relevant sind. Sie geben an, ob nur benutzerdefinierte Vorschläge zurückgegeben oder Bing-Vorschläge eingebunden werden. Bei der Einbindung von Bing-Vorschlägen werden benutzerdefinierte Vorschläge vor den von Bing bereitgestellten Vorschlägen angezeigt. Bing-Vorschläge sind auf Ihre Instanz für die benutzerdefinierte Suche beschränkt.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum schnellen Einstieg finden Sie unter [Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche](quick-start.md).

Weitere Informationen zu verfügbaren Optionen zum Anpassen Ihrer Suchinstanz finden Sie unter [Konfigurieren der Benutzeroberfläche für die benutzerdefinierte Suche](define-your-custom-view.md).

Machen Sie sich mit der Referenz zur [API für die benutzerdefinierte Suche](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) vertraut. Die Referenz enthält die Liste mit den Endpunkten, Headern und Abfrageparametern, die Sie zum Anfordern von Suchergebnissen verwenden können. Darüber hinaus finden Sie dort Definitionen der Antwortobjekte.

Informationen zum Anpassen von Vorschlägen finden Sie unter [Define custom search suggestions](define-custom-suggestions.md) (Definieren von Vorschlägen für die benutzerdefinierte Suche).

Machen Sie sich vorher unbedingt mit den [Verwendungs- und Anzeigeanforderungen von Bing](./use-and-display-requirements.md) vertraut, um nicht gegen die Regeln für die Verwendung der Suchergebnisse zu verstoßen.