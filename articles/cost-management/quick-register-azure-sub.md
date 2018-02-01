---
title: Registrieren Ihres Azure-Abonnements bei der Azure-Kostenverwaltung | Microsoft-Dokumentation
description: Verwenden Sie Ihr Azure-Abonnement, um sich bei der Azure-Kostenverwaltung von Cloudyn zu registrieren.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 666919e37ffa3bb039fc9387737b8e6c3789311a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="register-an-individual-azure-subscription-and-view-cost-data"></a>Registrieren eines einzelnen Azure-Abonnements und Anzeigen von Kostendaten

Mithilfe Ihres Azure-Abonnements können Sie sich bei der Azure-Kostenverwaltung von Cloudyn registrieren. Durch die Registrierung erhalten Sie Zugriff auf das Cloudyn-Portal. In diesem Schnellstart wird der Registrierungsvorgang ausführlich erläutert, der zum Erstellen eines Cloudyn-Testabonnements und zum Anmelden beim Cloudyn-Portal erforderlich ist. Es wird auch gezeigt, wie die Anzeige von Kostendaten sofort gestartet werden kann.

## <a name="log-in-to-azure"></a>Anmelden an Azure

- Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-trial-registration"></a>Erstellen einer Registrierung für die Testversion

1. Klicken Sie im Azure-Portal in der Liste der Dienste auf **Kostenverwaltung und Abrechnung**.
2. Klicken Sie unter **Übersicht** auf **Kostenverwaltung**.  
    ![Seite „Kostenverwaltung“](./media/quick-register-azure-sub/cost-mgt-billing-service.png)
3. Klicken Sie auf der Seite **Kostenverwaltung** auf **Zur Kostenverwaltung wechseln**, um die Cloudyn-Registrierungsseite in einem neuen Fenster zu öffnen.
4. Geben Sie im Cloudyn-Portal auf der Registrierungsseite für die Testversion den Namen Ihres Unternehmens ein, und wählen Sie dann **Einzelner Azure-Abonnementbesitzer** aus. Klicken Sie anschließend auf **Weiter**. Ihr Kontoname und die Mandanten-ID werden automatisch zum Formular hinzugefügt.  
    ![Registrierung für die Testversion](./media/quick-register-azure-sub/trial-reg-ind.png)
5. Wählen Sie die **Angebots-ID – Name** aus, die Ihrem Abonnement zugeordnet ist. Wenn Sie nicht genau wissen, wie Ihre Kosten-ID für Ihr Abonnement lautet, können Sie in Ihrer Azure-Rechnung nach **Angebots-ID** suchen.
6. Stimmen Sie den Nutzungsbedingungen zu, und überprüfen Sie dann Ihre Informationen. Klicken Sie anschließend auf **Weiter**.
7. Klicken Sie auf der Seite **Weitere Daten sammeln** auf **Weiter**, um Cloudyn für das Erfassen von Azure-Ressourcendaten zu autorisieren. Die erfassten Daten beziehen sich auf Nutzungs-, Leistungs- und Abrechnungs- sowie Tagdaten aus Ihren Abonnements.  
    ![Weitere Daten sammeln](./media/quick-register-azure-sub/gather-additional.png)
8. Sie werden über Ihren Browser zur Anmeldeseite von Cloudyn weitergeleitet. Melden Sie sich mit Ihren Azure-Anmeldeinformationen an.
9. Klicken Sie auf **Zu Cloudyn wechseln**, um das Cloudyn-Portal zu öffnen. Auf der Seite **Kontenverwaltung** sollten die Kontoinformationen Ihres Azure-Abonnements angezeigt werden.  
    ![Kontenverwaltung](./media/quick-register-azure-sub/accounts-mgt.png)

Ein Videotutorial über das Registrieren Ihres Azure-Abonnements finden Sie unter [Finding your Directory GUID and Rate ID for use in Azure Cost Management Powered by Cloudyn (Suchen Ihrer Directory-GUID und Rate-ID für die Verwendung in Azure Cost Management von Cloudyn)](https://youtu.be/PaRjnyaNGMI).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Ihre Azure-Abonnementinformationen verwendet, um sich bei der Kostenverwaltung zu registrieren. Außerdem haben Sie sich beim Cloudyn-Portal angemeldet und die Anzeige von Kostendaten gestartet. Weitere Informationen zur Azure-Kostenverwaltung von Cloudyn finden Sie im Tutorial zur Kostenverwaltung.

> [!div class="nextstepaction"]
> [Überprüfen der Nutzung und der Kosten](./tutorial-review-usage.md)
