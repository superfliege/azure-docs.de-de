---
title: Erstellen, Verknüpfen, Löschen oder Verschieben eines Integrationskontos in Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen eines Integrationskontos und Verknüpfen eines Integrationskontos mit Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: anneta
editor: ''
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; divswa
ms.openlocfilehash: fb1d0ceb26c5ed792f22051e2af10a7572200bdc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="what-is-an-integration-account"></a>Was ist ein Integrationskonto?

Über ein Integrationskonto können Ihre Unternehmensintegrations-Apps, insbesondere Logik-Apps, auf B2B-Artefakte (z. B. Handelspartner, Vereinbarungen, Zuordnungen, Schemas, Zertifikate usw.) zugreifen und diese verwalten. Um diesen Zugriff zu ermöglichen, verknüpfen Sie Ihr Integrationskonto mit Ihrer Logik-App, nachdem Sie sichergestellt haben, dass sich das Integrationskonto und die Logik-App am *gleichen Azure-Speicherort* befinden.

## <a name="create-an-integration-account"></a>Erstellen eines Integrationskontos

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com "Azure-Portal") an. 

2. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie im Suchfeld das Wort „Integration“ ein, und wählen Sie dann **Integrationskonten** aus.

   ![Integrationskonto erstellen](./media/logic-apps-enterprise-integration-accounts/account-1.png)

3. Wählen Sie im oberen Seitenbereich die Option **Hinzufügen** aus.

   ![„Hinzufügen“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Benennen Sie Ihr Integrationskonto, und wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten. Erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus. Wählen Sie einen **Speicherort** zum Hosten Ihres Integrationskontos und einen **Tarif** aus. Wählen Sie abschließend **Erstellen** aus.

   ![Details für das Integrationskonto angeben](./media/logic-apps-enterprise-integration-accounts/account-4.png)

   Azure stellt Ihr Integrationskonto am ausgewählten Speicherort bereit. Dieser Vorgang dauert in der Regel maximal eine Minute.

5. Aktualisieren Sie die Seite. Das neue Integrationskonto wird nun aufgeführt.

   ![Das neue Integrationskonto erscheint.](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Verknüpfen Sie als Nächstes das erstellte Integrationskonto mit Ihrer Logik-App. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Verknüpfen eines Integrationskontos mit einer Logik-App

Damit Ihre Logik-Apps auf B2B-Artefakte wie Handelspartner, Vereinbarungen, Zuordnungen und Schemas in Ihrem Integrationskonto zugreifen können, müssen Sie das Integrationskonto mit Ihrer Logik-App verknüpfen. 

1. Wählen Sie im Azure-Portal Ihre Logik-App aus, und überprüfen Sie ihren Standort.

   ![Logik-App auswählen und Standort überprüfen](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Wählen Sie unter **Einstellungen** die Option **Integrationskonto** aus.

   ![„Integrationskonto“ auswählen](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Wählen Sie in der Liste **Wählen Sie ein Integrationskonto aus.** das Integrationskonto aus, das Sie mit Ihrer Logik-App verknüpfen möchten. Wählen Sie zum Fertigstellen der Verknüpfung die Option **Speichern** aus.

   ![Auswählen Ihres Integrationskontos](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

   Sie erhalten eine Benachrichtigung mit dem Hinweis, dass Ihr Integrationskonto mit Ihrer Logik-App verknüpft ist und dass Ihrer Logik-App nun alle Artefakte in Ihrem Integrationskonto zur Verfügung stehen.

   ![Die Logik-App ist mit Ihrem Integrationskonto verknüpft.](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Da Ihr Integrationskonto jetzt mit Ihrer Logik-App verknüpft ist, können Sie in Ihrer Logik-App die B2B-Connectors verwenden. Zu den gängigen B2B-Connectors gehören unter anderem die XML-Überprüfung und die Flatfilecodierung/-decodierung.  

## <a name="delete-your-integration-account"></a>Löschen Ihres Integrationskontos

1. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie im Suchfeld das Wort „Integration“ ein, und wählen Sie dann **Integrationskonten** aus.

   ![Suchen Ihres Integrationskontos](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Wählen Sie das Integrationskonto aus, das Sie löschen möchten.

    ![Zu löschendes Integrationskonto auswählen](./media/logic-apps-enterprise-integration-accounts/account-5.png)

3. Wählen Sie im Menü die Option **Löschen** aus.

    ![„Löschen“ auswählen](./media/logic-apps-enterprise-integration-accounts/delete.png)

4. Bestätigen Sie, dass Sie das Integrationskonto wirklich löschen möchten.

## <a name="move-your-integration-account"></a>Verschieben Ihres Integrationskontos

Gehen Sie wie folgt vor, wenn Sie ein Integrationskonto in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe verschieben möchten. Stellen Sie nach dem Verschieben des Integrationskontos sicher, dass alle Skripts zur Verwendung der neuen Ressourcen-IDs aktualisiert werden.

1. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie im Suchfeld das Wort „Integration“ ein, und wählen Sie dann **Integrationskonten** aus.

   ![Suchen Ihres Integrationskontos](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Wählen Sie das Integrationskonto aus, das Sie verschieben möchten. Wählen Sie unter **Einstellungen** die Option **Eigenschaften** aus.

   ![Zu verschiebendes Integrationskonto auswählen. Unter „Einstellungen“ die Option „Eigenschaften“ auswählen.](./media/logic-apps-enterprise-integration-accounts/move.png)

3. Ändern Sie die mit Ihrem Integrationskonto verknüpfte Ressourcengruppe oder das mit Ihrem Integrationskonto verknüpfte Azure-Abonnement.

   ![„Ressourcengruppe ändern“ oder „Abonnement ändern“ auswählen](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  

