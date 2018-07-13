---
title: Verwalten Ihrer Schlüssel in LUIS | Microsoft-Dokumentation
description: Verwenden Sie Language Understanding Intelligent Service (LUIS) zum Verwalten Ihrer programmgesteuerten API-, Endpunkt- und externen Schlüssel.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 8e6e363649a0bdab5525de7b8e7abe9a53d14573
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266021"
---
# <a name="manage-your-luis-keys"></a>Verwalten Ihrer LUIS-Schlüssel
Mit einem Schlüssel können Sie Ihre LUIS-App erstellen und veröffentlichen oder den Endpunkt abfragen. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Wichtige Begriffe
Weitere Informationen zum Verständnis der Konzepte zu LUIS-Erstellungs- und -Endpunktschlüsseln finden Sie unter [Schlüssel in LUIS](luis-concept-keys.md).

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Zuweisen des Endpunktschlüssels
Auf der Seite **App veröffentlichen** befindet sich bereits ein Schlüssel in der Tabelle **Resources and Keys** (Ressourcen und Schlüssel). Dies ist der Erstellungsschlüssel (Startschlüssel). 

1. Erstellen Sie einen LUIS-Schlüssel im [Azure-Portal](https://portal.azure.com). Weitere Anweisungen finden Sie unter [Erstellen eines Abonnementschlüssels mithilfe von Azure](luis-how-to-azure-subscription.md).
 
2. Um den im vorherigen Schritt erstellten LUIS-Schlüssel hinzuzufügen, klicken Sie auf die Schaltfläche **Schlüssel hinzufügen**, um das Dialogfeld **Assign a key to your app** (Ihrer App einen Schlüssel zuweisen) zu öffnen. 

    ![Assign a key to your app (Ihrer App einen Schlüssel zuweisen)](./media/luis-manage-keys/assign-key.png)
3. Wählen Sie im Dialogfeld einen Mandanten aus. 
 
    > [!Note]
    > In Azure stellt ein Mandant die Azure Active Directory-ID des Clients oder der Organisation dar, der bzw. die einem Dienst zugeordnet ist. Wenn Sie sich zuvor mit Ihrem persönlichen Microsoft-Konto für ein Azure-Abonnement registriert haben, verfügen Sie bereits über einen Mandanten! Wenn Sie sich beim Azure-Portal anmelden, werden Sie automatisch bei [Ihrem Standardmandanten](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) angemeldet. Sie können diesen Mandanten nutzen, sollten aber das Erstellen eines Organisationsadministratorkontos erwägen.

4. Wählen Sie das Azure-Abonnement aus, das mit dem Azure-LUIS-Schlüssel, den Sie hinzufügen möchten, verknüpft ist.

5. Wählen Sie das Azure-LUIS-Konto aus. Die Region des Kontos wird in Klammern angezeigt. 

    ![Auswählen des Schlüssels](./media/luis-manage-keys/assign-key-filled-out.png)

6. Nachdem Sie diesen Endpunktschlüssel zuweisen, verwenden Sie ihn für alle Endpunktabfragen. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Veröffentlichungsregionen
Erfahren Sie mehr über die [Veröffentlichungsregionen](luis-reference-regions.md), einschließlich der Veröffentlichung in [Europa](luis-reference-regions.md#publishing-to-europe) und [Australien](luis-reference-regions.md#publishing-to-australia). Veröffentlichungsregionen unterscheiden sich von Erstellungsregionen. Stellen Sie sicher, dass Sie eine App in der Erstellungsregion erstellen, die der gewünschten Veröffentlichungsregion entspricht.

## <a name="unassign-key"></a>Abheben der Zuweisung eines Schlüssels

* Klicken Sie in der Liste **Resources and Keys** (Ressourcen und Schlüssel) auf das Papierkorbsymbol neben der Entität, deren Zuweisung Sie aufheben möchten. Klicken Sie in der Bestätigungsmeldung auf **OK**, um den Löschvorgang zu bestätigen.
 
    ![Aufheben der Zuweisung einer Entität](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> Beim Aufheben der Zuweisung des LUIS-Schlüssels wird dieser nicht aus Ihrem Azure-Abonnement gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie Ihren Schlüssel zum Veröffentlichen Ihrer App auf der Seite **App veröffentlichen**. Anweisungen zum Veröffentlichen finden Sie unter [Veröffentlichen von Apps](PublishApp.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website