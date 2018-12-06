---
title: Verwalten Ihrer Kontoeinstellungen in LUIS | Microsoft Docs
description: Verwenden Sie die LUIS-Website, um Ihre Kontoeinstellungen zu verwalten.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: bb41331228e700c55da21c627d617d16faa2dcb9
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335399"
---
# <a name="manage-account-and-authoring-key"></a>Verwalten von Konto und Authoringschlüssel
Die zwei entscheidenden Informationen für ein LUIS-Konto sind das Benutzerkonto und der Authoringschlüssel. Ihre Anmeldeinformationen werden unter [account.microsoft.com](https://account.microsoft.com) verwaltet. Ihr Authoringschlüssel wird auf der Seite **Einstellungen** der [LUIS](luis-reference-regions.md)-Website verwaltet. 

## <a name="authoring-key"></a>Erstellungsschlüssel

Dieser eindeutige, regionsspezifische Authoringschlüssel auf der Seite **Einstellungen** ermöglicht Ihnen das Authoring Ihrer sämtlichen Apps von der [LUIS](luis-reference-regions.md)-Website sowie der [Authoring-APIs](https://aka.ms/luis-authoring-api). Zu Ihrer Bequemlichkeit ist es dem Authoringschlüssel gestattet, eine [beschränkte](luis-boundaries.md) Anzahl Endpunktabfragen im Monat auszuführen. 

[![LUIS-Seite „Einstellungen“](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

Der Authoringschlüssel wird für alle Apps verwendet, die Sie besitzen, sowie für alle Apps, für die Sie als Mitwirkender gelistet sind.

## <a name="authoring-key-regions"></a>Regionen von Authoringschlüsseln
Der Authoringschlüssel ist für die [Authoringregion](luis-reference-regions.md#publishing-regions) spezifisch. Der Schlüssel funktioniert in anderen Regionen nicht. 

## <a name="reset-authoring-key"></a>Zurücksetzen des Authoringschlüssels
Wenn Ihr Authoringschlüssel beschädigt wurde, setzen Sie den Schlüssel zurück. Der Schlüssel wird für alle Ihre Apps auf der [LUIS](luis-reference-regions.md)-Website zurückgesetzt. Wenn Sie Ihre Apps mithilfe der Authoring-APIs verfassen, müssen Sie den Wert von `Ocp-Apim-Subscription-Key` auf den neuen Schlüssel ändern. 

## <a name="delete-account"></a>Konto löschen
Informationen zu den Daten, die beim Löschen Ihres Kontos gelöscht werden, finden Sie unter [Datenspeicherung und -löschung](luis-concept-data-storage.md#accounts). 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Ihren [Authoringschlüssel](luis-concept-keys.md#authoring-key). 

