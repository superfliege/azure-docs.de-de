---
title: Anmeldeinformationen in Azure Content Moderator | Microsoft-Dokumentation
description: Sie können die Content Moderator-Anmeldeinformationen zur Verwendung mit den APIs verwalten.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 6477879953dc2bb2c7503eb0b2d4b5effa7b6a11
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024654"
---
# <a name="manage-credentials"></a>Verwalten von Anmeldeinformationen

Ihre Content Moderator-Anmeldeinformationen werden in den folgenden Speicherorten erstellt:

- [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Content Moderator-Prüfungstool](http://contentmoderator.cognitive.microsoft.com/)

Dieser Artikel beschreibt, wo Sie sie finden können, und wie sie miteinander in Beziehung stehen.

## <a name="the-azure-portal"></a>Das Azure-Portal

Wählen Sie im Azure-Portal-Dashboard Ihr Content Moderator-Konto aus. Wählen Sie unter **Ressourcenverwaltung** die Option **Schlüssel** aus. Um den Schlüssel zu kopieren, wählen Sie das Symbol auf der rechten Seite des Schlüssels.

![Content Moderator-Schlüssel im Azure-Portal](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>Verwenden des Azure-Kontos mit dem Prüfungstool und der Überprüfungs-API
Um Ihren Azure-Schlüssel mit den Überprüfungs-APIs zu verwenden, kopieren Sie die Ressourcen-ID, die auf dem Bildschirm **Eigenschaften** im folgenden Screenshot, und geben Sie sie auf dem Anmeldebildschirm des Prüfungstools in die Felder **Ressource-ID(s) in der Whitelist** ein, wie im folgenden Abschnitt **Ressourcen-ID** dargestellt. 

> [!NOTE]
> Die Region Ihres Content Moderator-Abonnements muss mit der Region des Prüfungsteams übereinstimmen, damit es Ihr Team erkennt und auf die Teamdaten zugreifen kann. In den Bildern auf dieser Seite enthält die Region **West US** **(4)** beispielsweise das Content Moderator Azure-Abonnement und Ihr Prüfungsteam.
>
> Sobald Sie die beiden Stellen im Prüfungstool durch den Schlüssel und die Ressourcen-ID Ihres Azure-Abonnements ersetzt haben, wird Ihr **Trial Ocp-Apim-Subscription-Key**, der auf dem Bildschirm mit den Anmeldeinformationen angezeigt wird, nicht mehr verwendet, ist aber immer verfügbar.
> Der Testschlüssel ist auf maximal 5.000 Transaktionen pro Monat bei 1 Anforderung pro Sekunde (RPS) begrenzt.

![Ressourcen-ID für Content Moderator im Azure-Portal](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>Verwenden des Azure-Kontos mit Workflows im Prüfungstool

Um Ihren Azure-Schlüssel für die in Content Moderator verfügbaren Workflows zu verwenden, geben Sie ihn in das Feld **Ocp-Apim-Subscription-Key** im Abschnitt **Workfloweinstellungen** ein, wie im folgenden Abschnitt **Workflows** dargestellt. Drücken Sie auf **+**, um Ihre Ressourcen-ID zu speichern.

![Anmeldeinformationen für Content Moderator-Workflow im Prüfungstool](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>Das Prüfungstool

Wählen Sie im Prüfungstool-Dashboard auf der Registerkarte **Einstellungen** die Option **Anmeldeinformationen**.

![Content Moderator-Anmeldeinformationen im Prüfungstool](images/credentials-trial-resource-workflow.PNG)

Im folgenden Abschnitt wird der vorhergehende Sachverhalt näher betrachtet:

### <a name="api"></a>API

Der erste Teil listet Ihren **Überprüfungs-API-Endpunkt**, die **Team-ID** und den **Ocp-Apim-Subscription-Key (Content Moderator-Testschlüssel)** auf, der als Teil der Erstellung Ihres Prüfungsteams generiert wurde. Verwenden Sie diese Informationen, um alle Content Moderator-APIs aufzurufen, einschließlich der Überprüfungs-API.

Notieren Sie auch Ihren Regionsbezeichner für Ihren API-Endpunkt. Beispielsweise **westus** ist die Region in „https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0“.

![Content Moderator-Schlüssel im Prüfungstool](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Ressourcen-ID

Wir haben diesen Abschnitt im Abschnitt [Verwenden des Azure-Kontos mit dem Prüfungstool und der Überprüfungs-API](credentials.md#how-to-use-your-azure-account-with-the-review-tool) behandelt. Dieses Feld ist in der Regel leer, es sei denn, Sie fügen ihm Ihre Azure-Ressourcen-ID wie im vorherigen Abschnitt erläutert hinzu.

### <a name="workflows"></a>Workflows

Wir haben diese Felder im vorherigen Abschnitt zum [Verwenden des Azure-Schlüssels zum Ausführen von Workflows](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool) behandelt. Standardmäßig verwendet das Prüfungstool seinen automatisch generierten Testversionsschlüssel für die Ausführung der Workflows. Damit wird Folgendes angezeigt. Die anderen beiden Felder ermöglichen die Verwendung von Benennungs- und Bilderlisten für die Vorgänge „Fenstertext“ bzw. „Bild auswerten“.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie die Content Moderator Anmeldeinformationen in Ihren [Workflows](workflows.md) verwenden.
