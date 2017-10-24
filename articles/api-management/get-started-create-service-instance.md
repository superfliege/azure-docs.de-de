---
title: Erstellen einer Azure API Management-Instanz | Microsoft-Dokumentation
description: "Führen Sie die Schritte dieses Tutorials zum Erstellen einer neuen Azure API Management-Instanz aus."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 6433ea1f0eb6ad375402b998b4dfa80bded35c4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Erstellen einer neuen Azure API Management-Dienstinstanz

In diesem Tutorial werden die Schritte zum Erstellen einer neuen API Management-Instanz mit dem [Azure-Portal](https://portal.azure.com/) beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

+ Ein aktives Azure-Abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-new-service"></a>Erstellen eines neuen Diensts

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) **Neu** > **Enterprise Integration** > **API Management**.

    Alternativ können Sie **Neu** wählen, `API management` in das Suchfeld eingeben und die EINGABETASTE drücken. Klicken Sie auf **Erstellen**.

2. Geben Sie im Fenster **API Management-Dienst** einen eindeutigen **Namen** für Ihren API Management-Dienst ein. Diesen Namen können Sie später nicht mehr ändern.

    > [!TIP]
    > Der Name des Diensts wird verwendet, um einen Standarddomänennamen in der Form *{Name}.azure-api.net* zu generieren. Wenn Sie einen benutzerdefinierten Domänennamen verwenden möchten, lesen Sie [Configure a custom domain name](configure-custom-domain.md) (Konfigurieren eines benutzerdefinierten Domänennamens). <br/>
    > Der Dienstname wird verwendet, um auf den Dienst und die entsprechende Azure-Ressource zu verweisen.

5. Wählen Sie unter den verschiedenen Azure-Abonnements ein **Abonnement** aus, auf das Sie Zugriff haben.
6. Wählen Sie unter **Ressourcengruppe**die neue oder vorhandene Ressource aus.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen mit gleichem Lebenszyklus, gleichen Berechtigungen und gleichen Richtlinien. [Hier](../azure-resource-manager/resource-group-overview.md#resource-groups) erhalten Sie weitere Informationen.
7. Wählen Sie unter **Speicherort** die geografische Region aus, wo der API Management-Dienst erstellt wird. Im Dropdownlistenfeld werden nur die verfügbaren Regionen für den API Management-Dienst angezeigt. 
9. Geben Sie einen **Organisationsnamen** ein. Dieser Name wird mehrfach verwendet. Beispielsweise als Titel des Entwicklerportals und Absender von E-Mail-Benachrichtigungen.
10. Legen Sie in **Administrator-E-Mail** die E-Mail-Adresse fest, an die alle Benachrichtigungen von **API Management** gesendet werden.
11. Legen Sie in **Tarif** den Tarif **Entwickler** fest, um den Dienst zu bewerten. Dieser Tarif ist nicht für die Produktion bestimmt. Weitere Informationen zum Skalieren der API Management-Tarife finden Sie unter [Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz).
12. Wählen Sie **Erstellen**.

    > [!TIP]
    > Es dauert in der Regel zwischen 20 und 30 Minuten, einen API Management-Dienst zu erstellen. Durch Auswählen von **An Dashboard anheften** ist ein neu erstellter Dienst einfacher zu finden.

## <a name="next-steps"></a>Nächste Schritte

[Publish an API with Azure API Management](#api-management-getstarted-publish-api.md) (Veröffentlichen einer API mit Azure API Management)