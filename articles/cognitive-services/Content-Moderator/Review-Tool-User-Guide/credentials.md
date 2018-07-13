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
ms.openlocfilehash: 4531fa4c8bbb7bb9c1daeaaac6f9e7078dae250a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372922"
---
# <a name="manage-credentials"></a>Verwalten von Anmeldeinformationen

Ihre Content Moderator-Anmeldeinformationen werden in den folgenden Speicherorten erstellt:

- [Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Content Moderator-Prüfungstool](http://contentmoderator.cognitive.microsoft.com/)

Dieser Artikel beschreibt, wo Sie sie finden können, und wie sie miteinander in Beziehung stehen.

## <a name="the-azure-portal"></a>Das Azure-Portal

Wählen Sie im Azure-Portal-Dashboard Ihr Content Moderator-Konto aus. Wählen Sie unter **Ressourcenverwaltung** die Option **Schlüssel** aus. Um den Schlüssel zu kopieren, wählen Sie das Symbol auf der rechten Seite des Schlüssels.

![Content Moderator-Schlüssel im Azure-Portal](images/credentials-azure-portal-keys.PNG)

### <a name="how-to-use-your-azure-account-with-the-review-tool"></a>Verwenden Ihres Azure-Kontos mit dem Prüfungstool
Um Ihren Azure-Schlüssel mit den Überprüfungs-APIs zu verwenden, kopieren Sie die Ressourcen-ID, die auf dem Bildschirm **Eigenschaften** im folgenden Screenshot, und geben Sie sie auf dem Anmeldebildschirm des Prüfungstools in die Felder **Ressource-ID(s) in der Whitelist** ein, wie im folgenden Abschnitt **Ressourcen-ID** dargestellt. 

Um Ihren Azure-Schlüssel für die in Content Moderator verfügbaren Workflows zu verwenden, geben Sie ihn in das Feld **Ocp-Apim-Subscription-Key** im Abschnitt **Workfloweinstellungen** ein, wie im folgenden Abschnitt **Workflows** dargestellt.

> [!NOTE]
> Sobald Sie die beiden Stellen im Prüfungstool durch den Schlüssel und die Ressourcen-ID Ihres Azure-Abonnements ersetzt haben, wird Ihr **Trial Ocp-Apim-Subscription-Key**, der auf dem Bildschirm mit den Anmeldeinformationen angezeigt wird, nicht mehr verwendet, ist aber immer verfügbar.
> Der Testschlüssel ist auf maximal 5.000 Transaktionen pro Monat bei 1 Anforderung pro Sekunde (RPS) begrenzt.

![Ressourcen-ID für Content Moderator im Azure-Portal](images/credentials-azure-portal-resourceid.PNG)


## <a name="the-review-tool"></a>Das Prüfungstool

Wählen Sie im Prüfungstool-Dashboard auf der Registerkarte **Einstellungen** die Option **Anmeldeinformationen**.

![Content Moderator-Anmeldeinformationen im Prüfungstool](images/credentials-trial-resource-workflow.PNG)

Im folgenden Abschnitt wird der vorhergehende Sachverhalt näher betrachtet:


### <a name="api"></a>API

Der erste Teil listet Ihren **Überprüfungs-API-Endpunkt**, die **Team-ID** und den **Ocp-Apim-Subscription-Key (Content Moderator-Testschlüssel)** auf, der als Teil der Erstellung Ihres Prüfungsteams generiert wurde. Verwenden Sie diese Informationen, um alle Content Moderator-APIs aufzurufen, einschließlich der Überprüfungs-API.

Notieren Sie auch Ihren Regionsbezeichner für Ihren API-Endpunkt. Beispielsweise **westus** ist die Region in „https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0“.

![Content Moderator-Schlüssel im Prüfungstool](images/credentials-trialkey.PNG)


### <a name="resource-id"></a>Ressourcen-ID

Im zweiten Teil wird zunächst keine Ressourcen-ID angezeigt. **Um Ihren Azure-Abonnementschlüssel mit der Überprüfungs-API zu verwenden, navigieren Sie wie zuvor gezeigt zum Bildschirm mit der Ressourcen-ID und kopieren Sie ihn in das angezeigte Feld**. Drücken Sie auf **+**, um Ihre Ressourcen-ID zu speichern.

> [!NOTE]
> Die Region Ihres Content Moderator-Abonnements muss mit der Region des Prüfungsteams übereinstimmen, damit es Ihr Team erkennt und auf die Teamdaten zugreifen kann. In den Bildern auf dieser Seite enthält die Region **West US** **(4)** beispielsweise das Content Moderator Azure-Abonnement und Ihr Prüfungsteam.

![Content Moderator-Ressourcen-ID im Prüfungstool](images/credentials-resourceids.PNG)


### <a name="workflows"></a>Workflows

Der dritte Teil zeigt die Informationen zum Ausführen von Workflows an. Er beginnt standardmäßig mit der Anzeige des automatisch generierten Testschlüssels. 

**Aktualisieren Sie ihn mit Ihrem Azure-Schlüssel, wenn Sie ein Azure-Abonnement erhalten**. Die anderen beiden Felder ermöglichen die Verwendung von Benennungs- und Bilderlisten für die Vorgänge „Fenstertext“ bzw. „Bild auswerten“.

![Anmeldeinformationen für Content Moderator-Workflow im Prüfungstool](images/credentials-workflow.PNG)


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie die Content Moderator Anmeldeinformationen in Ihren [Workflows](workflows.md) verwenden.
