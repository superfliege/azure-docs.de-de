---
title: Registrieren Ihres Azure Enterprise Agreements bei der Azure Kostenverwaltung | Microsoft Docs
description: Verwenden Sie Ihr Enterprise Agreement, um sich bei Azure Cost Management zu registrieren.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 75ae3aabf51a14ceb3326e7f3cee3e20d9b98579
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Registrieren eines Azure Enterprise Agreements und Anzeigen von Kostendaten

Sie verwenden Ihr Enterprise Agreement, um sich bei Azure Cost Management zu registrieren. Durch die Registrierung erhalten Sie Zugriff auf das Cloudyn-Portal. In diesem Schnellstart wird der Registrierungsvorgang ausführlich erläutert, der zum Erstellen eines Cloudyn-Testabonnements und zum Anmelden beim Cloudyn-Portal erforderlich ist. Es wird auch gezeigt, wie die Anzeige von Kostendaten sofort gestartet werden kann.

## <a name="log-in-to-azure"></a>Anmelden an Azure

- Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="create-a-trial-registration"></a>Erstellen einer Registrierung für die Testversion

1. Klicken Sie im Azure-Portal in der Liste der Dienste auf **Kostenverwaltung und Abrechnung**.
2. Klicken Sie unter **Übersicht** auf **Kostenverwaltung**.  
    ![Seite „Kostenverwaltung“](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Navigieren Sie auf der Seite **Kostenverwaltung** zu **Zur Kostenverwaltung wechseln**, um die Cloudyn-Registrierungsseite in einem neuen Fenster zu öffnen.
4. Geben Sie im Cloudyn-Portal auf der Registrierungsseite für die Testversion den Namen Ihres Unternehmens ein, und wählen Sie dann **Azure Enterprise Enrollment Administrator** aus.  
    ![Registrierung der Testversion](./media/quick-register-ea/trial-reg.png)
5. Geben Sie Ihren Enterprise Portal-API-Registrierungsschlüssel ein. Wenn Ihr Schlüssel zurzeit nicht griffbereit ist, klicken Sie auf den Link [Enterprise Portal](https://ea.azure.com), und führen Sie dann die folgenden Schritte aus:
  1. Melden Sie sich auf der Azure Enterprise-Website an, und klicken Sie auf **Berichte**. Klicken Sie auf **API-Zugriffsschlüssel**, und kopieren Sie dann Ihren Primärschlüssel.  
    ![EA-API-Schlüssel](./media/quick-register-ea/ea-key.png)
  3. Navigieren Sie zurück zur Registrierungsseite, und fügen Sie Ihren API-Schlüssel ein.
6. Stimmen Sie den Nutzungsbedingungen zu, und überprüfen Sie dann Ihren Schlüssel. Klicken Sie auf **Weiter**, um Cloudyn für das Erfassen von Azure-Ressourcendaten zu autorisieren. Die erfassten Daten beziehen sich auf Nutzung, Leistung, Abrechnung sowie Tagdaten aus Ihren Abonnements.  
    ![Schlüsselüberprüfung](./media/quick-register-ea/ea-key-validated.png)
7. Unter **Andere Beteiligte einladen** können Sie Benutzer hinzufügen, indem Sie ihre E-Mail-Adressen eingeben. Klicken Sie auf **Weiter**, wenn Sie fertig sind. Je nach Größe Ihrer Azure-Registrierung kann es bis zu 24 Stunden dauern, bis Ihre gesamten Abrechnungsdaten Cloudyn hinzugefügt wurden.
8. Klicken Sie auf **Zu Cloudyn wechseln**, um das Cloudyn-Portal zu öffnen. Auf der Seite **Cloudkontenverwaltung** sollten Ihre registrierten EA-Kontoinformationen angezeigt werden.

Ein Videotutorial zur Registrierung Ihres Enterprise Agreements finden Sie unter [How to Find Your EA Enrollment ID and API Key for use in Azure Cost Management](https://youtu.be/u_phLs_udig) (Ermitteln Ihrer EA-Registrierungs-ID und Ihres API-Schlüssels zur Verwendung in Azure Cost Management).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Ihre Azure Enterprise Agreement-Informationen verwendet, um sich bei der Kostenverwaltung zu registrieren. Außerdem haben Sie sich beim Cloudyn-Portal angemeldet und die Anzeige von Kostendaten gestartet. Weitere Informationen zu Azure Cost Management finden Sie im Tutorial zu Cost Management.

> [!div class="nextstepaction"]
> [Überprüfen der Nutzung und der Kosten](./tutorial-review-usage.md)
