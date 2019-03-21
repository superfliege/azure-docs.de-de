---
title: Dienstkontingente und -grenzwerte für Azure Batch AI | Microsoft-Dokumentation
description: Erfahren Sie etwas über Standardkontingente, Limits und Einschränkungen für Azure Batch AI sowie die Anforderung von Kontingenterhöhungen.
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 1f9bd8503c2e8f2aa3c54d41b8585c6fbf81c92b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542082"
---
# <a name="batch-ai-service-quotas-and-limits"></a>Batch AI-Dienst – Kontingente und Grenzwerte

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Wie bei anderen Azure-Diensten gelten bei bestimmten Ressourcen für den Batch AI-Dienst Grenzwerte. In Batch AI sind diese Grenzwerte Standardkontingente, die auf Abonnementebene für alle Regionen gelten, in denen der Dienst [verfügbar](https://azure.microsoft.com/global-infrastructure/services/) ist. In diesem Artikel werden diese Standardwerte erläutert, und Sie erfahren, wie Sie Kontingenterhöhungen anfordern können.

Berücksichtigen Sie diese Kontingente beim Entwerfen und Skalieren Ihrer Batch AI-Ressourcen. Wenn Ihr Cluster beispielsweise nicht die von Ihnen vorgegebene Anzahl von Knoten erreicht, haben Sie möglicherweise die Kernkontingentgrenze von Batch AI für Ihr Abonnement erreicht.

Wenn Sie Produktionsworkloads in Batch AI ausführen möchten, müssen Sie möglicherweise ein oder mehrere Kontingente über den Standardwert erhöhen.

> [!NOTE]
> Bei einem Kontingent handelt es sich um ein Kreditlimit und keine Kapazitätsgarantie. Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support.
> 
> 

## <a name="resource-quotas"></a>Ressourcenkontingente

[!INCLUDE [azure-batch-ai-limits](../../includes/azure-batch-ai-limits.md)]

## <a name="other-limits"></a>Andere Limits

Die folgenden Grenzwerte sind strikte Limits, die nicht überschritten werden dürfen.

| **Ressource** | **Maximales Limit** |
| --- | --- |
| Maximale Arbeitsbereiche pro Ressourcengruppe | 800 |
| Maximale Clustergröße | 100 Knoten |
| Maximale GPU MPI-Prozesse pro Knoten | 1–4 |
| Maximale GPU-Worker pro Knoten | 1–4 |
| Maximale Lebensdauer von Aufträgen | 7 Tage<sup>1</sup> |
| Maximale Parameterserver pro Knoten | 1 |

<sup>1</sup> Die maximale Lebensdauer bezieht sich auf den Zeitraum, in dem ein Auftrag ausgeführt und beendet wird. Abgeschlossene Aufträge bleiben unbegrenzt lange erhalten. Daten für Aufträge, die nicht innerhalb der maximalen Lebensdauer abgeschlossen wurden, stehen hingegen nicht zur Verfügung.

## <a name="view-batch-ai-quotas"></a>Anzeigen von Batch AI-Kontingenten

Sie können die Kontingente Ihres aktuellen Batch AI-Abonnements im [Azure-Portal][portal] anzeigen.

1. Klicken Sie im linken Bereich auf **Alle Dienste**. Suchen Sie dann nach **Batch AI**, und klicken Sie auf den Dienst, um ihn zu öffnen.
2. Klicken Sie im Batch AI-Menü auf **Nutzung + Kontingente**.
3. Wählen Sie Ihr Abonnement aus, um die Kontingentgrenzen anzuzeigen.

## <a name="increase-a-batch-ai-cores-quota"></a>Erhöhen eines Kernkontingents für Batch AI

Führen Sie die folgenden Schritte aus, um über das [Azure-Portal][portal] eine Kontingenterhöhung für Ihr Batch AI-Abonnement anzufordern. 

1. Klicken Sie im linken Bereich auf **Alle Dienste**. Suchen Sie dann nach **Batch AI**, und klicken Sie auf den Dienst, um ihn zu öffnen.
2. Klicken Sie im Batch AI-Menü auf **Neue Supportanfrage**.
3. Gehen Sie unter **Grundlegende Einstellungen** wie folgt vor:
   
    a. **Problemtyp** > **Kontingent**
   
    b. **Abonnement** > Wählen Sie Ihr Abonnement aus.
   
    c. **Kontingenttyp** > **Batch AI**
   
    d. **Supportplan** > Wählen Sie Ihren Supportplan aus.

    Klicken Sie auf **Weiter**.
4. Gehen Sie unter **Problem** wie folgt vor:
   
    a. Wählen Sie einen **Schweregrad** gemäß der [geschäftlichen Auswirkung][support_sev] aus.
   
    b. Geben Sie in **Kontingentdetails** den Speicherort, den Kontingenttyp und den Ressourcentyp an. Geben Sie den neuen Grenzwert an, den Sie anfordern möchten. Klicken Sie auf **Speichern und fortfahren**.

    c. Optional: Laden Sie alle relevanten Dateien mit weiteren Informationen zum Grund für die Erhöhung hoch.
   
    Klicken Sie auf **Weiter**.
5. Gehen Sie unter **Kontaktinformationen** wie folgt vor:
   
    a. Wählen Sie eine **bevorzugte Kontaktmethode**aus.
   
    b. Überprüfen Sie die erforderlichen Kontaktdetails, und geben Sie sie ein.
   
    Klicken Sie auf **Erstellen** , um die Supportanfrage zu übermitteln.

Nachdem Sie die Supportanfrage übermittelt haben, wird sich der Azure-Support mit Ihnen in Verbindung setzen. Die Bearbeitung der Anfrage kann bis zu zwei Werktage in Anspruch nehmen.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit den Kontingentgrenzwerten vertraut gemacht haben, finden Sie in den folgenden Artikeln Informationen zu den ersten Schritten bei der Verwendung von Batch AI.

> [!div class="nextstepaction"]
> [Schnellstarttutorial für Batch AI](quickstart-tensorflow-training-cli.md)
> [Batch AI-Anleitungen](https://github.com/Azure/BatchAI/tree/master/recipes)
> [Weitere Informationen zu Batch AI-Ressourcen](resource-concepts.md)

[portal]: https://portal.azure.com
[support_sev]: https://aka.ms/supportseverity