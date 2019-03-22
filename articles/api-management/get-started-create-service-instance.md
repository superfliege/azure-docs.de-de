---
title: Erstellen einer Azure API Management-Instanz | Microsoft-Dokumentation
description: Führen Sie die Schritte dieses Tutorials zum Erstellen einer neuen Azure API Management-Instanz aus.
services: api-management
documentationcenter: ''
author: vladvino
manager: cflower
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: ef874e5d773e87963b6de8371986ac2196fc38f3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57901388"
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Erstellen einer neuen Azure API Management-Dienstinstanz

Azure API Management (APIM) unterstützt Organisationen beim Veröffentlichen von APIs für externe Entwickler, Partnerentwickler und interne Entwickler, um das volle Potenzial von Daten und Diensten ausschöpfen zu können. API Management bietet die Kernkompetenzen zur Sicherstellung eines erfolgreichen API-Programms in Form von Entwicklerengagement, geschäftlichen Erkenntnissen, Analysen, Sicherheit und Schutz. Mit APIM können Sie moderne API-Gateways für vorhandene Back-End-Dienste, die an einem beliebigen Ort gehostet werden, erstellen und verwalten. Weitere Informationen finden Sie im Thema [Übersicht](api-management-key-concepts.md).

In dieser Schnellstartanleitung werden die Schritte zum Erstellen einer neuen API Management-Instanz mit dem Azure-Portal beschrieben.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

![Neue Instanz](./media/get-started-create-service-instance/get-started-create-service-instance-created.png)

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-new-service"></a>Erstellen eines neuen Diensts

![Neue Azure API Management-Instanz](./media/get-started-create-service-instance/00-CreateResource-01.png)

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf **Ressource erstellen** > **Enterprise Integration** > **API Management**.

    Alternativ können Sie **Neu** wählen, `API management` in das Suchfeld eingeben und die EINGABETASTE drücken. Klicken Sie auf **Create**.

2. Geben Sie im Fenster **API Management-Dienst** Einstellungen ein.

    ![Neue Instanz](./media/get-started-create-service-instance/get-started-create-service-instance-create-new.png)

    | Einstellung                 | Empfohlener Wert                               | Beschreibung                                                                                                                                                                                                                                                                                                                         |
|-------------------------|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                | Eindeutiger Name für Ihren API Management-Dienst | Den Namen können Sie später nicht mehr ändern. Der Name des Diensts wird verwendet, um einen Standarddomänennamen in der Form *{Name}.azure-api.net* zu generieren. Wenn Sie einen benutzerdefinierten Domänennamen verwenden möchten, lesen Sie [Configure a custom domain name](configure-custom-domain.md) (Konfigurieren eines benutzerdefinierten Domänennamens). <br/> Der Dienstname wird verwendet, um auf den Dienst und die entsprechende Azure-Ressource zu verweisen. |
| **Abonnement**        | Ihr Abonnement                             | Das Abonnement, unter dem diese neue Dienstinstanz erstellt wird. Sie können aus den verschiedenen Azure-Abonnements, auf die Sie Zugriff haben, ein Abonnement auswählen.                                                                                                                                                            |
| **Ressourcengruppe**      | *apimResourceGroup*                           | Sie können eine neue oder eine vorhandene Ressource auswählen. Eine Ressourcengruppe ist eine Sammlung von Ressourcen mit gleichem Lebenszyklus, gleichen Berechtigungen und gleichen Richtlinien. [Hier](../azure-resource-manager/resource-group-overview.md#resource-groups)erhalten Sie weitere Informationen.                                                                                                  |
| **Location**            | *USA, Westen*                                    | Wählen Sie die geografische Region in Ihrer Nähe aus. Im Dropdownlistenfeld werden nur die verfügbaren Regionen für den API Management-Dienst angezeigt.                                                                                                                                                                                                          |
| **Name der Organisation**   | Der Name Ihrer Organisation                 | Dieser Name wird an verschiedenen Stellen verwendet, u.a. im Titel des Entwicklerportals und im Absender von Benachrichtigungs-E-Mails.                                                                                                                                                                                                             |
| **Administrator-E-Mail** | *admin\@org.com*                               | Legen Sie die E-Mail-Adresse fest, an die alle Benachrichtigungen von **API Management** gesendet werden.                                                                                                                                                                                                                                              |
| **Preisstufe**        | *Developer*                                   | Legen Sie den Tarif **Entwickler** fest, um den Dienst zu bewerten. Dieser Tarif ist nicht für die Produktion bestimmt. Weitere Informationen zum Skalieren der API Management-Tarife finden Sie unter [Upgrade and scale an API Management instance](upgrade-and-scale.md) (Upgraden und Skalieren einer API Management-Instanz).                                                                                                                                    |

3. Wählen Sie **Erstellen**.

    > [!TIP]
    > Es dauert in der Regel zwischen 20 und 30 Minuten, einen API Management-Dienst zu erstellen. Durch Auswählen von **An Dashboard anheften** ist ein neu erstellter Dienst einfacher zu finden.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit den folgenden Schritten entfernen:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus.
2. Geben Sie `resource groups` in das Suchfeld ein, und klicken Sie auf das Ergebnis.

    ![Ressourcengruppennavigation](./media/get-started-create-service-instance/00-DeleteResource-01.png)

3. Suchen Sie nach Ihrer Ressourcengruppe, und klicken Sie darauf.
4. Klicken Sie auf **Ressourcengruppe löschen**.

    ![Ressourcengruppennavigation](./media/get-started-create-service-instance/00-DeleteResource-02.png)

5. Bestätigen Sie den Löschvorgang, indem Sie den Namen Ihrer Ressourcengruppe eingeben.
6. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md)
