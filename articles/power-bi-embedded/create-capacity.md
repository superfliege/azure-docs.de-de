---
title: "Einrichten von Power BI Embedded-Kapazität im Azure-Portal | Microsoft-Dokumentation"
description: "In diesem Artikel erläutert, wie Sie Power BI Embedded-Kapazität in Microsoft Azure einrichten."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 1902e5c18cd7083ceeda79e6b9e779e4baaf175a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-power-bi-embedded-capacity-in-the-azure-portal"></a>Einrichten von Power BI Embedded-Kapazität im Azure-Portal

In diesem Artikel erläutert, wie Sie Power BI Embedded-Kapazität in Microsoft Azure einrichten. Power BI Embedded vereinfacht Power BI-Funktionen, indem es Ihnen hilft, beeindruckende Visuals, Berichte und Dashboards schnell in Ihre Apps einzubinden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

> [!VIDEO https://www.youtube.com/embed/aXrvFfg_iSk]

## <a name="before-you-begin"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

* **Azure-Abonnement:** Besuchen Sie die Webseite [Kostenlose Azure-Testversion](https://azure.microsoft.com/free/), und erstellen Sie ein Konto.
* **Azure Active Directory**: Ihr Abonnement muss einem Azure Active Directory-Mandanten (AAD) zugeordnet sein. Und ***Sie müssen bei Azure mit einem Konto in diesem Mandanten angemeldet sein***. Microsoft-Konten werden nicht unterstützt. Weitere Informationen finden Sie unter „Authentifizierung und Benutzerberechtigungen“.
* **Power BI-Mandant:** Mindestens ein Konto in Ihrem AAD-Mandanten muss bei Power BI registriert sein.
* **Ressourcengruppe:** Verwenden Sie eine bereits vorhandene Ressourcengruppe, oder [erstellen Sie eine neue](../azure-resource-manager/resource-group-overview.md).

## <a name="create-a-capacity"></a>Einrichten von Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie **+ (Neu)** > **Daten + Analysen** aus.

3. Geben Sie in das Suchfeld *Power BI Embedded* ein.

4. Wählen Sie in Power BI Embedded **Erstellen** aus.

5. Geben Sie die erforderlichen Informationen ein, und klicken Sie auf **Erstellen**.

    ![Zum Einrichten neuer Kapazität auszufüllende Felder](media/create-capacity/azure-portal-create-power-bi-embedded.png)

    |Einstellung |Beschreibung |
    |---------|---------|
    |**Ressourcenname**|Ein Name zur Identifizierung der Kapazität. Der Ressourcenname wird im Power BI-Verwaltungsportal sowie im Azure-Portal angezeigt.|
    |**Abonnement**|Das Abonnement, für das Sie die Kapazität einrichten möchten.|
    |**Ressourcengruppe**|Die Ressourcengruppe, die diese neue Kapazität enthält. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).|
    |**Power BI-Kapazitätsadministrator**|Power BI-Kapazitätsadministratoren können die Kapazität im Power-BI-Verwaltungsportal anzeigen und anderen Benutzern Zuweisungsberechtigungen erteilen. Ihr Konto ist standardmäßig das Kapazitätsadministratorkonto. Der Kapazitätsadministrator muss zu Ihrem Power BI-Mandanten gehören.|
    |**Standort**|Der Standort, an dem Power BI für Ihren Mandanten gehostet wird. Diese Einstellung wird automatisch aufgelöst. Ein anderer Standort kann nicht ausgewählt werden.|
    |**Tarif**|Wählen Sie die SKU (Anzahl der virtuellen Kerne und Arbeitsspeichergröße), die Ihre Anforderungen erfüllt.  Einzelheiten finden Sie unter [Power BI Embedded – Preise](https://azure.microsoft.com/pricing/details/power-bi-embedded/).|

6. Klicken Sie auf **Erstellen**.

Die Einrichtung dauert in der Regel weniger als eine Minute, meist nur wenige Sekunden. Wenn Sie **An Dashboard anheften** ausgewählt haben, navigieren Sie zum Dashboard, um zu prüfen, ob Ihre neue Kapazität angezeigt wird. Oder navigieren zu **Weitere Dienste** > **Power BI Embedded**, um zu prüfen, ob Ihre Kapazität einsatzbereit ist.

![Dashboard im Azure-Portal mit Power BI Embedded-Kapazität](media/create-capacity/azure-portal-dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

Um Ihre neue Power BI Embedded-Kapazität zu nutzen, wechseln Sie zum Power BI-Verwaltungsportal, um Arbeitsbereiche zuzuweisen. Weitere Informationen finden Sie unter [Verwalten von Kapazitäten in Power BI Premium und Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/).

Wenn Sie diese Kapazität nicht benötigen, halten Sie sie an, um die Abrechnung zu unterbrechen. Informationen zum Anhalten oder Starten Ihrer Kapazität finden Sie unter [Anhalten und Starten Ihrer Power BI Embedded-Kapazität im Azure-Portal](pause-start.md).

Informationen zum Einbetten von Power BI-Inhalten in Ihre Anwendung finden Sie unter [So können Sie Ihre Dashboards, Berichte und Kacheln von Power BI einbetten](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Weitere Fragen? [Fragen Sie die Power BI-Community](http://community.powerbi.com/)