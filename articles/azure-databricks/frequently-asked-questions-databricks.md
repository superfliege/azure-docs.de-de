---
title: "Azure Databricks: Häufig gestellte Fragen und Hilfe | Microsoft-Dokumentation"
description: "Erhalten Sie Antworten auf häufig gestellte Fragen und Informationen zur Problembehandlung für Azure Databricks."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: nitinme
ms.openlocfilehash: 6bb542537ec713be272f7e58e0b247763214ef4a
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2017
---
# <a name="azure-databricks-preview-common-questions-and-help"></a>Azure Databricks (Vorschauversion): Häufig gestellte Fragen und Hilfe

In diesem Artikel sind die wichtigsten Fragen im Zusammenhang mit Azure Databricks aufgeführt. Außerdem werden einige häufige Probleme genannt, die bei Verwendung von Azure Databricks auftreten können. Weitere Informationen zu Azure Databricks finden Sie unter [Was ist Azure Databricks?](what-is-azure-databricks.md) 

## <a name="common-questions"></a>Häufig gestellte Fragen

Dieser Abschnitt enthält die häufig gestellten Fragen im Zusammenhang mit Azure Databricks.

### <a name="can-i-use-my-own-keys-for-local-encryption"></a>Kann ich meine eigenen Schlüssel für die lokale Verschlüsselung verwenden? 
In der aktuellen Version wird die Verwendung eigener Schlüssel aus Azure Key Vault nicht unterstützt. 

### <a name="can-i-use-azure-vnets-with-azure-databricks"></a>Kann ich Azure-VNETs mit Azure Databricks verwenden?
Im Rahmen der Bereitstellung von Azure Databricks wird ein neues VNET erstellt. Bei dieser Version können Sie kein eigenes Azure-VNET verwenden.

### <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Wie greife ich von einem Notizbuch auf Azure Data Lake Store zu? 

1. Stellen Sie in Azure Active Directory einen Dienstprinzipal bereit, und erfassen Sie dessen Schlüssel.
2. Weisen Sie dem Dienstprinzipal in Azure Data Lake Store die erforderlichen Berechtigungen zu.
3. Verwenden Sie zum Zugreifen auf eine Datei in Azure Data Lake Store die Anmeldeinformationen für den Dienstprinzipal im Notizbuch.

Weitere Informationen finden Sie unter [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="troubleshooting"></a>Problembehandlung

Dieser Abschnitt beschreibt die Behandlung häufig auftretender Probleme mit Azure Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problem: Dieses Abonnement ist nicht für die Verwendung des Namespace „Microsoft.Databricks“ registriert.

**Fehlermeldung**

Dieses Abonnement ist nicht für die Verwendung des Namespace „Microsoft.Databricks“ registriert. Informationen zum Registrieren von Abonnements finden Sie unter „https://aka.ms/rps-not-found“. (Code: MissingSubscriptionRegistration)

**Lösung**

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Abonnements**, klicken Sie auf das von Ihnen verwendete Abonnement, und klicken Sie dann auf **Ressourcenanbieter**. 
3. Klicken Sie in der Liste der Ressourcenanbieter für **Microsoft.Databricks** auf **Registrieren**. Sie müssen über die Rolle „Mitwirkender“ oder „Besitzer“ für das Abonnement verfügen, um den Ressourcenanbieter zu registrieren.


### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: Ihr Konto {E-Mail} weist nicht die Rolle „Besitzer“ oder „Mitwirkender“ für die Databricks-Arbeitsbereichsressource im Azure-Portal auf.

**Fehlermeldung**

Ihr Konto {E-Mail} weist nicht die Rolle „Besitzer“ oder „Mitwirkender“ für die Databricks-Arbeitsbereichsressource im Azure-Portal auf. Dieser Fehler kann auch auftreten, wenn Sie ein Gastbenutzer im Mandanten sind. Bitten Sie Ihren Administrator, Ihnen den Zugriff zu gewähren oder Sie als einen Benutzer direkt im Databricks-Arbeitsbereich hinzuzufügen. 

**Lösung**

Nachfolgend sind einige Lösungen für dieses Problem aufgeführt:

* Um den Mandanten zu initialisieren, müssen Sie als ein regulärer Benutzer des Mandanten und nicht als Gastbenutzer angemeldet sein. Sie müssen außerdem über die Rolle „Mitwirkender“ für die Databricks-Arbeitsbereichsressource verfügen. Sie können einem Benutzer Zugriff über die Registerkarte **Zugriffssteuerung (IAM)** in Ihrem Azure Databricks-Arbeitsbereich im Azure-Portal gewähren.

* Dieser Fehler kann auch auftreten, wenn Ihr E-Mail-Domänenname mehreren Active Directorys zugewiesen ist. Um dieses Problem zu umgehen, erstellen Sie einen neuen Benutzer in dem Active Directory, das das Abonnement mit Ihrem Databricks-Arbeitsbereich enthält.

    a. Wechseln Sie im Azure-Portal zu Azure Active Directory, klicken Sie auf **Benutzer und Gruppen** und dann auf **Benutzer hinzufügen**.

    b. Fügen Sie einen Benutzer mit einer E-Mail-Adresse vom Typ `@<tenant_name>.onmicrosoft.com` anstelle einer E-Mail-Adresse vom Typ „@<Ihre_Domäne>“ hinzu. Sie finden die Angabe „<Mandantenname>.onmicrosoft.com“, die Ihrem Active Directory zugeordnet ist, im Azure-Portal unter Azure Active Directory in **Benutzerdefinierte Domänen**.
    
    c. Gewähren Sie diesem neuen Benutzer die Rolle **Mitwirkender** für die Databricks-Arbeitsbereichsressource.
    
    d. Melden Sie sich beim Azure-Portal mit dem neuen Benutzer an, und suchen Sie den Databricks-Arbeitsbereich.
    
    e. Starten Sie den Databricks-Arbeitsbereich als dieser Benutzer.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Ihr Konto {E-Mail} wurde nicht in Databricks registriert. 

**Lösung**

Wenn Sie den Arbeitsbereich nicht erstellt haben und als Benutzer des Arbeitsbereichs hinzugefügt werden, wenden Sie sich an die Person, die den Arbeitsbereich erstellt hat, damit diese Sie über die Verwaltungskonsole für Azure Databricks hinzufügt. Anweisungen hierzu finden Sie unter [Hinzufügen und Verwalten von Benutzern](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Wenn Sie den Arbeitsbereich erstellt haben und weiterhin diese Fehlermeldung erhalten, klicken Sie im Azure-Portal erneut auf „Arbeitsbereich initialisieren“.

### <a name="issue-cloud-provider-launch-failure-publicipcountlimitreached-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problem: Startfehler des Cloudanbieters (PublicIPCountLimitReached): Beim Einrichten des Clusters ist ein Fehler des Cloudanbieters aufgetreten.

**Fehlermeldung**

Startfehler des Cloudanbieters: Beim Einrichten des Clusters ist ein Fehler des Cloudanbieters aufgetreten. Weitere Informationen finden Sie im Databricks-Handbuch. Azure-Fehlercode: PublicIPCountLimitReached. Azure-Fehlermeldung: Es können maximal 60 öffentliche IP-Adressen für dieses Abonnement in dieser Region erstellt werden.

**Lösung**

Azure Databricks-Cluster verwenden eine öffentliche IP-Adresse pro Knoten. Wenn für Ihr Abonnement bereits alle öffentlichen IP-Adressen verwendet werden, sollten Sie [eine Erhöhung des Kontingents anfordern](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Wählen Sie **Kontingent** als **Problemtyp** und **Netzwerk: ARM** als **Kontingenttyp** aus, und fordern Sie eine Erhöhung des Kontingents an öffentlichen IP-Adressen in **Details** an. Wenn Ihr aktueller Grenzwert beispielsweise bei 60 liegt und ein Cluster mit 100 Knoten erstellt werden soll, fordern Sie eine Erhöhung auf 160 an.

### <a name="issue-cloud-provider-launch-failure-missingsubscriptionregistration-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problem: Startfehler des Cloudanbieters (MissingSubscriptionRegistration): Beim Einrichten des Clusters ist ein Fehler des Cloudanbieters aufgetreten.

**Fehlermeldung**

Startfehler des Cloudanbieters: Beim Einrichten des Clusters ist ein Fehler des Cloudanbieters aufgetreten. Weitere Informationen finden Sie im Databricks-Handbuch.
Azure-Fehlercode: MissingSubscriptionRegistration; Azure-Fehlermeldung: Das Abonnement ist nicht für die Verwendung des Namespace „Microsoft.Compute“ registriert. Informationen zum Registrieren von Abonnements finden Sie unter „https://aka.ms/rps-not-found“.

**Lösung**

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Abonnements**, klicken Sie auf das von Ihnen verwendete Abonnement, und klicken Sie dann auf **Ressourcenanbieter**. 
3. Klicken Sie in der Liste der Ressourcenanbieter für **Microsoft.Compute** auf **Registrieren**. Sie müssen über die Rolle „Mitwirkender“ oder „Besitzer“ für das Abonnement verfügen, um den Ressourcenanbieter zu registrieren.

Genauere Anweisungen finden Sie unter [Ressourcenanbieter und -typen](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Nächste Schritte
Schrittweise Anweisungen zum Erstellen einer Data Factory der Version 2 finden Sie in den folgenden Tutorials:

- [Schnellstartanleitung: Erste Schritte mit Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Was ist Azure Databricks?](what-is-azure-databricks.md)

