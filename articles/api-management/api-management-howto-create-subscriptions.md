---
title: Erstellen von Abonnements in Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Abonnements in Azure API Management erstellen.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 3f4e113125ec9644aac974e47996afe290e57cee
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621703"
---
# <a name="how-to-create-subscriptions-in-azure-api-management"></a>Erstellen von Abonnements in Azure API Management

Beim Veröffentlichen von APIs über Azure API Management (APIM) ist die einfachste und gängigste Methode zum Schutz des Zugriffs auf diese APIs die Verwendung von Abonnementschlüsseln. Anders ausgedrückt: Clientanwendungen, die die veröffentlichten APIs nutzen möchten, müssen einen gültigen Abonnementschlüssel in HTTP-Anforderungen einschließen, wenn Aufrufe an diese APIs gesendet werden. Um einen Abonnementschlüssel für den API-Zugriff zu erhalten, ist ein Abonnement erforderlich. Weitere Informationen zu Abonnements finden Sie unter [Abonnements in Azure API Management](api-management-subscriptions.md).

In diesem Artikel werden Sie durch die Schritte zum Erstellen von Abonnements im Azure-Portal geleitet.

## <a name="prerequisites"></a>Voraussetzungen

Die Durchführung der in diesem Artikel aufgeführten Schritte setzt Folgendes voraus:

+ [Erstellen einer APIM-Instanz](get-started-create-service-instance.md)
+ Grundkenntnisse zu [Abonnements in APIM](api-management-subscriptions.md)

## <a name="create-a-new-subscription"></a>Erstellen eines neuen Abonnements

1. Klicken Sie im Menü links auf **Abonnements**.
2. Klicken Sie auf **Abonnement hinzufügen**.
3. Geben Sie einen Namen für das Abonnement an, und wählen Sie den Bereich aus.
4. Klicken Sie auf **Speichern**.

![Flexible Abonnements](./media/api-management-subscriptions/flexible-subscription.png)

Nachdem das Abonnement erstellt wurde, wird ein API-Schlüsselpaar (primärer Schlüssel und sekundärer Schlüssel) für den Zugriff auf die APIs bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu API Management:

+ Informationen zu weiteren [Konzepten](api-management-terminology.md) in API Management
+ [Tutorials](import-and-publish.md) zum weiteren Kennenlernen von API Management
+ [FAQ-Seite](api-management-faq.md) für häufige Fragen