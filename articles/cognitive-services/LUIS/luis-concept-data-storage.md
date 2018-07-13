---
title: Grundlegendes zur Datenspeicherung in LUIS – Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten in Language Understanding Intelligent Service (LUIS) gespeichert werden.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/08/2018
ms.author: v-geberr
ms.openlocfilehash: f235c787e7d2064696e5421219a297d914b5882d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266004"
---
# <a name="data-storage-and-removal"></a>Datenspeicherung und -entfernung
LUIS speichert Daten verschlüsselt in einem Azure-Datenspeicher für die im Schlüssel angegebene Region. Diese Daten werden für 30 Tage gespeichert. 

## <a name="export-and-delete-app"></a>Exportieren und Löschen von Apps
Benutzer haben volle Kontrolle über das [Exportieren](create-new-app.md#export-app) und [Löschen](create-new-app.md#delete-app) ihrer Apps. 

## <a name="utterances-in-an-intent"></a>Äußerungen in Absichten
Löschen Sie die Beispieläußerungen für das Training von [LUIS][LUIS]. Wenn Sie eine Beispieläußerung aus Ihrer LUIS-App löschen, wird sie vom LUIS-Webdienst entfernt und ist nicht für den Export verfügbar.

## <a name="utterances-in-review"></a>Äußerungen in Überprüfungen
Sie können Äußerungen aus der Liste der Benutzeräußerungen, die LUIS auf der Seite **[Review endpoint utterance](label-suggested-utterances.md)** (Endpunktäußerungen überprüfen) vorschlägt, löschen. Das Löschen von Äußerungen aus dieser Liste verhindert, dass diese empfohlen werden. Sie werden aber nicht aus den Protokollen gelöscht.

## <a name="accounts"></a>Konten
Wenn Sie ein Konto löschen, werden alle Apps zusammen mit ihren Beispieläußerungen und Protokollen gelöscht. Die Daten werden für 60 Tage beibehalten, bevor das Konto und die Daten dauerhaft gelöscht werden.

Das Löschen von Konten erfolgt über die Seite **Einstellungen**. Wählen Sie Ihren Kontonamen rechts oben auf der Navigationsleiste aus, um die Seite **Einstellungen** anzuzeigen.

## <a name="data-inactivity-as-an-expired-subscription"></a>Dateninaktivität als Zeichen für ein abgelaufenes Abonnement
Im Zusammenhang mit der Beibehaltung und Löschung von Daten kann eine inaktive LUIS-App nach dem _Ermessen von Microsoft_ als abgelaufenes Abonnement behandelt werden. Eine App gilt als inaktiv, wenn sie für die letzten 90 Tage die folgenden Kriterien erfüllt: 

* Es wurden **keine** Aufrufe getätigt.
* Sie wurde nicht geändert.
* Ihr ist zurzeit kein aktueller Schlüssel zugewiesen.
* Es hat sich kein Benutzer bei ihr angemeldet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr zum Exportieren und Löschen von Apps.](create-new-app.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions