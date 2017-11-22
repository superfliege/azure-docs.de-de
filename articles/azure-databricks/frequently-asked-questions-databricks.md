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
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: b6b001087cba5f8550d4fea3e4a2f7c1c865beae
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
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

### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: Ihr Konto {E-Mail} weist nicht die Rolle „Besitzer“ oder „Mitwirkender“ für die Databricks-Arbeitsbereichsressource im Azure-Portal auf.

**Fehlermeldung**

Ihr Konto {E-Mail} weist nicht die Rolle „Besitzer“ oder „Mitwirkender“ für die Databricks-Arbeitsbereichsressource im Azure-Portal auf. Dieser Fehler kann auch auftreten, wenn Sie ein Gastbenutzer im Mandanten sind. Bitten Sie Ihren Administrator, Ihnen den Zugriff zu gewähren oder Sie als einen Benutzer direkt im Databricks-Arbeitsbereich hinzuzufügen. 

**Lösung**

Um den Mandanten zu initialisieren, müssen Sie als ein regulärer Benutzer des Mandanten und nicht als Gastbenutzer angemeldet sein. Sie müssen außerdem über die Rolle „Mitwirkender“ für die Databricks-Arbeitsbereichsressource verfügen. Sie können einem Benutzer Zugriff über die Registerkarte **Zugriffssteuerung (IAM)** in Ihrem Azure Databricks-Arbeitsbereich im Azure-Portal gewähren.

### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Ihr Konto {E-Mail} wurde nicht in Databricks registriert. 

**Lösung**

Wenn Sie den Arbeitsbereich nicht erstellt haben und als Benutzer des Arbeitsbereichs hinzugefügt werden, wenden Sie sich an die Person, die den Arbeitsbereich erstellt hat, damit diese Sie über die Verwaltungskonsole für Azure Databricks hinzufügt. Anweisungen hierzu finden Sie unter [Hinzufügen und Verwalten von Benutzern](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Wenn Sie den Arbeitsbereich erstellt haben und weiterhin diese Fehlermeldung erhalten, klicken Sie im Azure-Portal erneut auf „Arbeitsbereich initialisieren“.

### <a name="issue-cloud-provider-launch-failure-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problem: Startfehler des Cloudanbieters: Beim Einrichten des Clusters ist ein Fehler des Cloudanbieters aufgetreten.

**Fehlermeldung**

Startfehler des Cloudanbieters: Beim Einrichten des Clusters ist ein Fehler des Cloudanbieters aufgetreten. Weitere Informationen finden Sie im Databricks-Handbuch. Azure-Fehlercode: PublicIPCountLimitReached. Azure-Fehlermeldung: Es können maximal 60 öffentliche IP-Adressen für dieses Abonnement in dieser Region erstellt werden.

**Lösung**

Azure Databricks-Cluster verwenden eine öffentliche IP-Adresse pro Knoten. Wenn für Ihr Abonnement bereits alle öffentlichen IP-Adressen verwendet werden, sollten Sie [eine Erhöhung des Kontingents anfordern](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Wählen Sie **Kontingent** als **Problemtyp** und **Netzwerk-ARM** als **Kontingenttyp** aus, und fordern Sie eine Erhöhung des Kontingents an öffentlichen IP-Adressen in **Details** an (wenn Ihr aktueller Grenzwert beispielsweise bei 60 liegt und ein Cluster mit 100 Knoten erstellt werden soll, fordern Sie eine Erhöhung auf 160 an).

## <a name="next-steps"></a>Nächste Schritte
Schrittweise Anweisungen zum Erstellen einer Data Factory der Version 2 finden Sie in den folgenden Tutorials:

- [Schnellstartanleitung: Erste Schritte mit Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Was ist Azure Databricks?](what-is-azure-databricks.md)

