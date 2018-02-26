---
title: Erstellen eines Batch-Kontos im Azure-Portal | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie ein Azure Batch-Konto im Azure-Portal erstellen, um umfangreiche parallele Workloads in der Cloud auszuführen."
services: batch
documentationcenter: 
author: dlepow
manager: jeconnoc
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5efe804806cb9c14a483e7393f0e8202897d53d6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Erstellen eines Batch-Kontos mit dem Azure-Portal

> [!div class="op_single_selector"]
> * [Azure portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
>
>

Hier erfahren Sie, wie Sie im [Azure-Portal][azure_portal] ein Azure Batch-Konto erstellen und geeignete Kontoeigenschaften für Ihr Computeszenario auswählen. Außerdem erfahren Sie, wo Sie wichtige Kontoeigenschaften wie Zugriffsschlüssel und Konto-URLs finden.

Hintergrundinformationen zu Batch-Konten und -Szenarien finden Sie in der [Funktionsübersicht](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Erstellen eines Batch-Kontos

> [!NOTE]
> Wenn Sie ein Batch-Konto erstellen, sollten Sie im Allgemeinen den Standardmodus **Batch-Dienst** verwenden, in dem Pools im Hintergrund in von Azure verwalteten Abonnements zugeordnet werden. Die Alternative (der Modus **Benutzerabonnement**) wird für die meisten Szenarien nicht mehr empfohlen. In diesem Modus werden virtuelle Batch-Computer und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Wenn Sie ein Batch-Konto im Benutzerabonnementmodus erstellen möchten, müssen Sie auch Ihr Abonnement in Azure Batch registrieren und das Konto einer Azure Key Vault-Instanz zuordnen.

1. Melden Sie sich beim [Azure-Portal][azure_portal] an.
2. Klicken Sie auf **Ressource erstellen**, und durchsuchen Sie den Marketplace nach **Batch-Dienst**.

    ![Batch im Marketplace][marketplace_portal]
3. Wählen Sie **Batch-Dienst** aus, klicken Sie auf **Erstellen**, und geben Sie Einstellungen für das **neue Batch-Konto** ein. Orientieren Sie sich an den folgenden Details:

    ![Erstellen eines Batch-Kontos][account_portal]

    a. **Kontoname**: Der gewählte Name muss innerhalb der Azure-Region, in der das Konto erstellt wird, eindeutig sein (siehe **Standort** weiter unten). Der Kontoname darf nur Kleinbuchstaben und Zahlen enthalten und muss 3 bis 24 Zeichen lang sein.

    b. **Abonnement**: Das Abonnement, in dem das Batch-Konto erstellt werden soll. Wenn Sie nur über ein Abonnement verfügen, ist es standardmäßig ausgewählt.

    c. **Poolzuordnungsmodus**: Falls diese Einstellung angezeigt wird, übernehmen Sie die Standardeinstellung **Batch-Dienst**.

    c. **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe für Ihr neues Batch-Konto aus, oder erstellen Sie eine neue.

    d. **Standort**: Die Azure-Region, in der das Batch-Konto erstellt werden soll. Nur die von Ihrem Abonnement und der Ressourcengruppe unterstützten Regionen werden als Optionen angezeigt.

    e. **Speicherkonto** (optional): Ein allgemeines Azure Storage-Konto, das Sie dem Batch-Konto zuordnen. Dies ist bei den meisten Batch-Konten empfehlenswert. Ausführliche Informationen finden Sie weiter unten in diesem Artikel unter [Verknüpftes Azure-Speicherkonto](#linked-azure-storage-account).

4. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.



## <a name="view-batch-account-properties"></a>Anzeigen der Eigenschaften des Batch-Kontos
Klicken Sie nach der Kontoerstellung auf das Konto, um auf dessen Einstellungen und Eigenschaften zuzugreifen. Über das linke Menü können Sie auf alle Kontoeinstellungen und -eigenschaften zugreifen.

![Seite „Batch-Konto“ im Azure-Portal][account_blade]

* **Batch-Konto-URL**: Wenn Sie eine Anwendung mit den [Batch-APIs](batch-apis-tools.md#azure-accounts-for-batch-development) entwickeln, benötigen Sie eine Konto-URL für den Zugriff auf Ihre Batch-Ressourcen. Eine Batch-Konto-URL weist das folgende Format auf:

    `https://<account_name>.<region>.batch.azure.com`

![Batch-Konto-URL im Portal][account_url]

* **Zugriffsschlüssel**: Für die Authentifizierung des Batch-Kontozugriffs über Ihre Anwendung können Sie einen Kontozugriffsschlüssel verwenden. (Batch unterstützt auch die Azure Active Directory-Authentifizierung.)

    Klicken Sie zum Anzeigen oder erneuten Generieren der Zugriffsschlüssel auf **Schlüssel**.

    ![Batch-Kontoschlüssel im Azure-Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Verknüpftes Azure-Speicherkonto

Sie können ein allgemeines Azure Storage-Konto mit Ihrem Batch-Konto verknüpfen. Dies ist in zahlreichen Szenarien hilfreich. Das Feature [Anwendungspakete](batch-application-packages.md) von Batch verwendet ebenso wie die .NET-Bibliothek [Batch-Dateikonventionen](batch-task-output.md) Azure Blob Storage. Diese optionalen Features unterstützen Sie beim Bereitstellen der von Ihren Batch-Aufgaben ausgeführten Anwendungen und Beibehalten der von ihnen erzeugten Daten.

Wir empfehlen die Erstellung eines neuen Speicherkontos, das ausschließlich von Ihrem Batch-Konto verwendet wird. Azure Batch unterstützt derzeit nur allgemeine Speicherkonten. Dieser Kontotyp ist unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md) in Schritt 5 ([Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account)) beschrieben.

![Erstellen eines allgemeinen Speicherkontos][storage_account]

> [!NOTE]
> Gehen Sie beim erneuten Generieren der Zugriffsschlüssel für ein verknüpftes Speicherkonto vorsichtig vor. Generieren Sie nur einen Speicherkontoschlüssel erneut, und klicken Sie auf der Seite des verknüpften Speicherkontos auf **Synchronisierungsschlüssel**. Warten Sie fünf Minuten, damit der Schlüssel an die Computeknoten in Ihren Pools verteilt werden kann. Anschließend können Sie ggf. den anderen Schlüssel erneut generieren und synchronisieren. Wenn Sie beide Schlüssel gleichzeitig generieren, können die Computeknoten keinen der Schlüssel synchronisieren und nicht mehr auf das Speicherkonto zugreifen.
>
>

![Erneutes Generieren der Speicherkontoschlüssel][4]

## <a name="additional-configuration-for-user-subscription-mode"></a>Zusätzliche Konfiguration für den Benutzerabonnementmodus

Wenn Sie ein Batch-Konto im Benutzerabonnementmodus erstellen möchten, führen Sie vor der Erstellung des Kontos die folgenden zusätzlichen Schritte aus.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Gewähren des Zugriffs auf Ihr Abonnement für Azure Batch (einmaliger Vorgang)
Wenn Sie Ihr erstes Batch-Konto im Modus „Benutzerabonnement“ erstellen, müssen Sie Ihr Abonnement bei Batch registrieren. (Falls Sie diesen Schritt bereits ausgeführt haben, fahren Sie mit dem nächsten Abschnitt fort.)

1. Melden Sie sich beim [Azure-Portal][azure_portal] an.

2. Klicken Sie auf **Weitere Dienste** > **Abonnements** und anschließend auf das Abonnement, das Sie für das Batch-Konto verwenden möchten.

3. Klicken Sie auf der Seite **Abonnement** auf **Zugriffssteuerung (IAM)** > **Hinzufügen**.

    ![Abonnementzugriffssteuerung][subscription_access]

4. Wählen Sie auf der Seite **Berechtigungen hinzufügen** die Rolle **Mitwirkender** aus, und suchen Sie die Batch-API. Suchen Sie diese Zeichenfolgen, bis Sie die API gefunden haben:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Neuere Azure AD-Mandanten verwenden diesen Namen unter Umständen.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** ist die ID für die Batch-API. 

5. Wenn Sie die Batch-API gefunden haben, wählen Sie diese aus, und klicken Sie auf **Speichern**.

    ![Hinzufügen von Batch-Berechtigungen][add_permission]

### <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors
Im Modus „Benutzerabonnement“ wird ein Azure-Schlüsseltresor benötigt. Dieser muss der gleichen Ressourcengruppe angehören wie das zu erstellende Batch-Konto. Stellen Sie sicher, dass sich die Ressourcengruppe in einer Region befindet, in der Batch [verfügbar](https://azure.microsoft.com/regions/services/) ist und die von Ihrem Abonnement unterstützt wird.

1. Klicken Sie im [Azure-Portal][azure_portal] auf **Neu** > **Sicherheit und Identität** > **Key Vault**.

2. Geben Sie auf der Seite **Schlüsseltresor erstellen** einen Namen für den Schlüsseltresor ein, und erstellen Sie eine Ressourcengruppe in der Region, die Sie für Ihr Batch-Konto verwenden möchten. Behalten Sie bei den übrigen Einstellungen die Standardwerte bei, und klicken Sie auf **Erstellen**.




## <a name="batch-service-quotas-and-limits"></a>Batch-Dienst – Kontingente und Limits
Für Batch-Konten gelten genau wie für Ihr Azure-Abonnement und andere Azure-Dienste bestimmte [Kontingente und Grenzwerte](batch-quota-limit.md). Aktuelle Kontingente für ein Batch-Konto finden Sie unter **Kontingente**.

![Batch-Kontokontingente im Azure-Portal][quotas]



Zudem können viele dieser Kontingente mithilfe einer kostenlosen Produktsupportanfrage über das Azure-Portal erhöht werden. Ausführliche Informationen zum Anfordern einer Kontingenterhöhung finden unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) .

## <a name="other-batch-account-management-options"></a>Weitere Optionen für die Verwaltung von Batch-Konten
Neben der Verwendung des Azure-Portals stehen Ihnen zum Erstellen und Verwalten von Batch-Konten die folgenden Möglichkeiten zur Verfügung:

* [Batch-PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md)
* [Azure-Befehlszeilenschnittstelle](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den Konzepten und Features des Batch-Diensts finden Sie in der [Funktionsübersicht für Batch](batch-api-basics.md). In diesem Artikel werden die primären Batch-Ressourcen (wie etwa Pools, Computeknoten, Aufträge und Aufgaben) beschrieben, und er enthält eine Übersicht über die Dienstfeatures für umfangreiche Computeworkloads.
* Informieren Sie sich über die Grundlagen der Entwicklung einer Batch-fähigen Anwendung mit der [Batch-.NET-Clientbibliothek](batch-dotnet-get-started.md) oder mit [Python](batch-python-tutorial.md). In diesen Einführungsartikeln werden Sie durch eine funktionierende Anwendung geführt, die den Batch-Dienst zum Ausführen einer Workload auf mehreren Computeknoten verwendet und Azure Storage zum Bereitstellen und Abrufen von Workloaddateien nutzt.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Erneutes Generieren der Speicherkontoschlüssel"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png

