---
title: Migrieren der Tarife von Endpunkten für den Custom Speech Service in Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Bereitstellungen von Custom Speech Service-Endpunkten in Cognitive Services aus den Tarifen S0 und S1 zu S2 migrieren.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: 71aa20c779ae0c73db3d7ce6f267524c5bf71ea5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214562"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Migrieren von Bereitstellungen zum neuen Preismodell

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Ab Juli 2017 wird für den Custom Speech Service ein [neues Preismodell](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) angeboten. Das neue Modell ist *einfacher zu verstehen*, *die Kosten können leichter berechnet werden*, und es ist in Bezug auf die Skalierung *flexibler*. Für die Skalierung hat Microsoft das Konzept der Skalierungseinheit eingeführt. Jede Skalierungseinheit kann gleichzeitig fünf Anforderungen verarbeiten. Die Skalierung für gleichzeitige Anforderungen war im alten Modell für den Tarif S0 auf fünf gleichzeitige Anforderungen und für den Tarif S1 auf zwölf gleichzeitige Anforderungen festgelegt. Wir haben diese Grenzwerte aufgehoben, damit Sie in Bezug auf die Anforderungen Ihrer Anwendungsfälle flexibler vorgehen können.

Bei der Nutzung eines alten S0- oder S1-Tarifs empfehlen wir Ihnen, Ihre vorhandenen Bereitstellungen zum neuen S2-Tarif zu migrieren. Der neue S2-Tarif deckt sowohl den Tarif S0 als auch den Tarif S1 ab. Die verfügbaren Optionen sind in der folgenden Abbildung dargestellt:

![Seite „Tarif wählen“](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft verarbeitet die Migration auf halbautomatisierte Weise. Zuerst lösen Sie die Migration aus, indem Sie den neuen Tarif auswählen. Anschließend migrieren wir Ihre Bereitstellung automatisch.

Die Zuordnung der alten Tarife zu Skalierungseinheiten ist in der folgenden Tabelle angegeben:

| Tarif | Gleichzeitige Anforderungen (altes Modell) | Migration | Gleichzeitige Anforderungen |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** mit einer Skalierungseinheit |   5 |
| S1 |  12  |   => **S2** mit drei Skalierungseinheiten |  15 |

Gehen Sie wie folgt vor, um zum neuen Tarif zu migrieren:

## <a name="step-1-check-your-existing-deployment"></a>Schritt 1: Überprüfen Ihrer vorhandenen Bereitstellung
Navigieren Sie zum [Custom Speech Service-Portal](http://cris.ai), und überprüfen Sie Ihre vorhandenen Bereitstellungen. In unserem Beispiel werden zwei Bereitstellungen verwendet. Eine Bereitstellung wird unter dem Tarif S0 und die andere unter dem Tarif S1 ausgeführt. Die Bereitstellungen werden in der folgenden Tabelle in der Spalte **Bereitstellungsoptionen** angezeigt:

![Seite „Bereitstellungen“](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>Schritt 2: Auswählen Ihres neuen Tarifs im Azure-Portal
1. Öffnen Sie eine neue Browserregisterkarte, und melden Sie sich am [Azure-Portal](http://ms.portal.azure.com/) an. 

2. Wählen Sie im Bereich **Cognitive Services** in der Liste **Abonnements** Ihr Custom Speech-Abonnement aus. 

3. Wählen Sie im Bereich für Ihr Abonnement die Option **Tarif**.

    ![Link „Tarif“](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. Wählen Sie auf der Seite **Tarif auswählen** die Option **S2 Standard**. Dies ist der neue, vereinfachte und flexiblere Tarif.

5. Wählen Sie die Schaltfläche **Auswählen** aus.

    ![Seite „Tarif wählen“](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>Schritt 3: Überprüfen des Migrationsstatus im Custom Speech Service-Portal
Kehren Sie zum Custom Speech Service-Portal zurück, und überprüfen Sie Ihre Bereitstellungen. (Aktualisieren Sie das Browserfenster, falls es noch geöffnet ist.) 

Der Status der entsprechenden Bereitstellung sollte sich in *Wird verarbeitet* geändert haben. Sie können die Migration auch überprüfen, indem Sie sich die Spalte **Bereitstellungsoptionen** ansehen. Darin finden Sie jetzt Informationen zu Skalierungseinheiten und zur Protokollierung. Die Skalierungseinheiten sollten Ihren vorherigen Tarif widerspiegeln. Die Protokollierung sollte ebenfalls aktiviert sein, wie in der Tabelle dargestellt:

![Spalte „Bereitstellungsoptionen“](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Nehmen Sie Kontakt mit uns auf, falls während der Migration Probleme auftreten.
>

## <a name="next-steps"></a>Nächste Schritte
Weitere Tutorials finden Sie auf den folgenden Seiten:
* [Erstellen eines benutzerdefinierten Akustikmodells](cognitive-services-custom-speech-create-acoustic-model.md)
* [Erstellen eines benutzerdefinierten Sprachmodells](cognitive-services-custom-speech-create-language-model.md)
* [Erstellen eines benutzerdefinierten Spracherkennungsendpunkts](cognitive-services-custom-speech-create-endpoint.md)
