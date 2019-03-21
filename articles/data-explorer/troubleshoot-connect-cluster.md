---
title: Fehler beim Herstellen einer Verbindung mit einem Cluster im Azure-Daten-Explorer
description: Dieser Artikel beschreibt die Schritte zur Problembehandlung beim Herstellen einer Verbindung mit einem Cluster im Azure-Daten-Explorer.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 89ae8bd4139623cfafe811b7c82433cfb8400611
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189664"
---
# <a name="troubleshoot-failure-to-connect-to-a-cluster-in-azure-data-explorer"></a>Problembehandlung: Fehler beim Herstellen einer Verbindung mit einem Cluster im Azure-Daten-Explorer

Wenn Sie im Azure-Daten-Explorer keine Verbindung mit einem Cluster herstellen können, führen Sie die folgenden Schritte aus.

1. Stellen Sie sicher, dass die Verbindungszeichenfolge richtig ist. Sie sollte in folgendem Format vorliegen: `https://<ClusterName>.<Region>.kusto.windows.net`. Beispiel: `https://docscluster.westus.kusto.windows.net`.

1. Stellen Sie sicher, dass Sie über die erforderlichen Berechtigungen verfügen. Andernfalls erhalten Sie die Antwort, dass Sie *nicht autorisiert* sind.

    Informationen zu Berechtigungen finden Sie unter [Verwalten von Datenbankberechtigungen](manage-database-permissions.md). Arbeiten Sie ggf. mit Ihrem Clusteradministrator zusammen, damit dieser Sie zur entsprechenden Rolle hinzufügen kann.

1. Stellen Sie sicher, dass der Cluster nicht gelöscht wurde: Sehen Sie sich das Aktivitätsprotokoll in Ihrem Abonnement an.

1. Sehen Sie sich das [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/) an. Suchen Sie nach dem Status des Azure-Daten-Explorers in der Region, in der Sie versuchen, eine Verbindung mit einem Cluster herzustellen.

    Wenn der Status nicht **Gut** lautet (grünes Häkchen), versuchen Sie die Verbindung mit dem Cluster herzustellen, wenn sich der Status verbessert hat.

1. Wenn Sie weitere Unterstützung bei der Lösung Ihres Problems benötigen, öffnen Sie eine Supportanfrage im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).