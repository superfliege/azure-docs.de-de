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
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: f3086f09e29664b816ba709fc5cda75d7b11d1b4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035248"
---
# <a name="manage-account-and-authoring-key"></a>Verwalten von Konto und Authoringschlüssel
Die zwei entscheidenden Informationen für ein LUIS-Konto sind das Benutzerkonto und der Authoringschlüssel. Ihre Anmeldeinformationen werden unter [account.microsoft.com](https://account.microsoft.com) verwaltet. Ihr Authoringschlüssel wird auf der Seite **Einstellungen** der [LUIS](luis-reference-regions.md)-Website verwaltet. 

## <a name="authoring-key"></a>Erstellungsschlüssel

Dieser eindeutige, regionsspezifische Authoringschlüssel auf der Seite **Einstellungen** ermöglicht Ihnen das Authoring Ihrer sämtlichen Apps von der [LUIS](luis-reference-regions.md)-Website sowie der [Authoring-APIs](https://aka.ms/luis-authoring-api). Zu Ihrer Bequemlichkeit ist es dem Authoringschlüssel gestattet, eine [beschränkte](luis-boundaries.md) Anzahl Endpunktabfragen im Monat auszuführen. 

![LUIS-Seite „Einstellungen“](./media/luis-how-to-account-settings/account-settings.png)

Der Authoringschlüssel wird für alle Apps verwendet, die Sie besitzen, sowie für alle Apps, für die Sie als Mitwirkender gelistet sind.

## <a name="authoring-key-regions"></a>Regionen von Authoringschlüsseln
Der Authoringschlüssel ist für die [Authoringregion](luis-reference-regions.md#publishing-regions) spezifisch. Der Schlüssel funktioniert in anderen Regionen nicht. 

## <a name="reset-authoring-key"></a>Zurücksetzen des Authoringschlüssels
Wenn Ihr Authoringschlüssel beschädigt wurde, setzen Sie den Schlüssel zurück. Der Schlüssel wird für alle Ihre Apps auf der [LUIS](luis-reference-regions.md)-Website zurückgesetzt. Wenn Sie Ihre Apps mithilfe der Authoring-APIs verfassen, müssen Sie den Wert von `Ocp-Apim-Subscription-Key` auf den neuen Schlüssel ändern. 

## <a name="delete-account"></a>Konto löschen
Informationen zu den Daten, die beim Löschen Ihres Kontos gelöscht werden, finden Sie unter [Datenspeicherung und -löschung](luis-concept-data-storage.md#accounts). 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Ihren [Authoringschlüssel](luis-concept-keys.md#authoring-key). 

