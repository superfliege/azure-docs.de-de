---
title: Verwalten von Einstellungen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Sie können die Einstellungen von Benutzerkonten sowie den Erstellungsschlüssel, der für alle Ihre Apps verwendet wird, auf der LUIS-Website verwalten.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 51b0d3f753ab89e5809e610f5754355d1c5090b0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228626"
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

