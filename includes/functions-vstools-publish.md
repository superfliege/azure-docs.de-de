---
title: Includedatei
description: Includedatei
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 07/17/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: baa1ea8e2c8727197ef6ee58520f4b55abf782c7
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40100580"
---
1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.

2. Wählen Sie **Azure-Funktionen-App**, **Neu erstellen** und dann **Veröffentlichen**.

    ![Auswählen eines Veröffentlichungsziels](./media/functions-vstools-publish/functions-vstools-create-new-function-app.png)

3. Wenn Sie Visual Studio noch nicht mit Ihrem Azure-Konto verbunden haben, wählen Sie **Konto hinzufügen** aus.

4. Verwenden Sie im Dialogfeld **App Service erstellen** die **Hosting**-Einstellungen in der Tabelle unter der Abbildung:

    ![Dialogfeld „App Service erstellen“](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App-Name** | Global eindeutiger Name | Name, der Ihre neue Funktions-App eindeutig identifiziert. |
    | **Abonnement** | Auswählen Ihres Abonnements | Das zu verwendende Azure-Abonnement. |
    | **[Ressourcengruppe](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Name der Ressourcengruppe, in der die Funktions-App erstellt wird. Wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen.|
    | **[App Service-Plan](../articles/azure-functions/functions-scale.md)** | Verbrauchsplan | Achten Sie darauf, unter **Größe** den **Verbrauch** auszuwählen, nachdem Sie auf **Neu** geklickt haben, um einen serverlosen Tarif zu erstellen. Wählen Sie einen **Standort** in einer [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe anderer Dienste aus, auf die Ihre Funktionen zugreifen. Bei der Ausführung in einem anderen Plan als **Verbrauch** müssen Sie die [Skalierung Ihrer Funktions-App](../articles/azure-functions/functions-scale.md) verwalten.  |
    | **[Speicherkonto](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** | Allgemeines Speicherkonto | Für die Functions-Runtime wird ein Azure Storage-Konto benötigt. Klicken Sie auf **Neu**, um ein universelles Speicherkonto zu erstellen. Sie können auch ein vorhandenes Konto verwenden, das die [Anforderungen an das Speicherkonto](../articles/azure-functions/functions-scale.md#storage-account-requirements) erfüllt.  |

5. Klicken Sie auf **Erstellen**, um eine Funktions-App und zugehörige Ressourcen in Azure mit diesen Einstellungen zu erstellen und Ihren Funktionsprojektcode bereitzustellen. 

6. Notieren Sie sich nach Abschluss der Bereitstellung den Wert der **Website-URL**, da es sich dabei um die Adresse Ihrer Funktions-App in Azure handelt.

    ![Veröffentlichen einer Erfolgsmeldung](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
