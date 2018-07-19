---
title: Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal | Microsoft-Dokumentation
description: Anleitung zum Erstellen eines Microsoft Cognitive Services-APIs-Kontos im Azure-Portal.
services: cognitive-services
documentationcenter: ''
author: garyericson
manager: cgronlun
editor: ''
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: cognitive-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: garye
ms.reviewer: gibattag
ms.openlocfilehash: ed5f19b23375ecb83e19274c7405e9a1208a7985
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036152"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal

Sie müssen zuerst ein Konto im Azure-Portal erstellen, um Microsoft Cognitive Services-APIs verwenden zu können.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

2. Klicken Sie auf **+ Ressource erstellen**.

3. Wählen Sie unter Azure Marketplace die Option **KI + Cognitive Services**, und erkunden Sie die Liste mit den verfügbaren APIs. Klicken Sie auf **Alle anzeigen**, um die gesamte Liste mit den Cognitive Services-APIs anzuzeigen. Klicken Sie auf die gewünschte API, um den Vorgang fortzusetzen.

    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:

   - **Kontoname:** Der Name des Kontos. Es wird empfohlen einen aussagekräftigen Namen zu wählen, z.B. *AFaceAPIAccount*.

   - **Abonnement:** Wählen Sie eines der verfügbaren Azure-Abonnements aus, für das Sie mindestens Berechtigungen als Teilnehmer besitzen.

   - **API-Typ:** Wählen Sie die Cognitive Services-API aus, die Sie benutzen möchten. Weitere Informationen zu den verschiedenen verfügbaren Cognitive Services-APIs finden Sie auf der Website von [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

   - **Tarif:** Die Kosten für Ihr Cognitive Services-Konto hängen von der tatsächlichen Nutzung und den ausgewählten Optionen ab. Weitere Informationen zu den Preisen für einzelne APIs finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Ressourcengruppe:** Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die in Bezug auf Lebenszyklus, Berechtigungen und Richtlinien gleich sind. Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Ressourcengruppenstandort:** Ist nur erforderlich, wenn die ausgewählte API global (nicht an einen Standort gebunden) ist. Wenn die API global und nicht an einen Standort gebunden ist, müssen Sie allerdings einen Standort für die Ressourcengruppe mit den Metadaten angeben, die dem Cognitive Services-API-Konto zugeordnet sind. Dieser Standort hat keine Auswirkung auf die Laufzeitverfügbarkeit Ihres Kontos. Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Explizite Bestätigung von Bestimmungen für Onlinedienste:** Zum Erstellen eines Kontos müssen Besitzer oder Mitwirkende von Abonnements (wie gemäß [rollenbasierter Azure-Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/overview) definiert) explizit die Bestimmungen bestätigen, die für Cognitive Services in den [Bestimmungen für Onlinedienste](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx) angegeben sind. 

     Der Abonnementbesitzer kann die Erstellung eines Cognitive Services-Kontos für eine bestimmte Ressourcengruppe oder ein Abonnement mit [Azure-Richtlinien](../azure-policy/azure-policy-introduction.md) deaktivieren, indem er die Informationen im Artikel [Erstellen einer Richtlinienzuweisung zum Identifizieren nicht konformer Ressourcen in Ihrer Azure-Umgebung](../azure-policy/assign-policy-definition.md) verwendet, die Richtliniendefinition „Nicht zulässige Ressourcentypen“ zuweist und **Microsoft.CognitiveServices/accounts** als Zielressourcentyp angibt.

     Wenn die Kontoerstellung deaktiviert ist, wird bei der versuchten Erstellung der folgende Fehler angezeigt:

     ![Fehler bei Kontoerstellung](media/cognitive-services-apis-create-account/error-message.png)

5. Klicken Sie auf **An Dashboard anheften**, um das Konto an das Dashboard des Azure-Portals anzuheften.

6. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.

Klicken Sie auf die Kachel auf dem Dashboard, nachdem das Cognitive Services-Konto erfolgreich bereitgestellt wurde, um die Kontoinformationen anzuzeigen.

Sie können die **Endpunkt-URL** im Bereich **Übersicht** und die Schlüssel im Bereich **Schlüssel** verwenden, um damit zu beginnen API-Aufrufe in Ihrer Anwendung durchzuführen.

![Anzeigen der Kontoinformationen](media/cognitive-services-apis-create-account/display-account.png)

![Anzeigen der Kontoschlüssel](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu allen verfügbaren Diensten von Microsoft Cognitive Services finden Sie unter [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

Schnellstartanleitungen zur Verwendung einiger beispielhafter Cognitive Services-APIs:

 - [Maschinelles Sehen – Schnellstartanleitungen für C#](computer-vision/quickstarts/csharp.md)
 - [Textanalyse mit Python](text-analytics/quickstarts/python.md)
 - [Gesichtserkennungs-API mit JavaScript](face/quickstarts/javascript.md)
