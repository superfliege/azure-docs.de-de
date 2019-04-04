---
title: Skalieren von Azure-Daten-Explorer-Clustern zur Anpassung an sich änderndem Bedarf
description: Dieser Artikel beschreibt Schritte zum zentralen Hoch- und Herunterskalieren eines Azure Data Explorer-Clusters basierend auf sich änderndem Bedarf.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 71f1d197b4a3fb437607f876ea524e582e62c0fd
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756324"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Verwalten der Clusterskalierung bei sich änderndem Bedarf

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Aber der Bedarf in einem Cluster kann nicht mit absoluter Genauigkeit vorhergesagt werden. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist.

Ein besserer Ansatz ist die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazität und CPU-Ressourcen hinzugefügt und entfernt werden. Es gibt zwei Workflows für die Skalierung: zentrale Skalierung und horizontale Skalierung. In diesem Artikel wird veranschaulicht, wie Sie einen Cluster zentral hochskalieren.

1. Zum Custer wechseln Wählen Sie unter **Einstellungen** die Option **Zentral hochskalieren** aus.

    Ihnen wird eine Liste der verfügbaren SKUs angezeigt. In der folgenden Abbildung sind z. B. nur vier SKUs verfügbar.

    ![Zentrales Hochskalieren](media/manage-cluster-scale-up/scale-up.png)

    SKUs sind deaktiviert, weil sie entweder die aktuelle SKU sind, oder weil sie in der Region nicht verfügbar sind, in der sich der Cluster befindet.

1. Zum Ändern Ihrer SKU wählen Sie die gewünschte SKU aus, und wählen Sie **Auswählen** aus.

> [!NOTE]
> Die zentrale Hochskalierung kann einige Minuten dauern, und während dieser Zeit wird Ihr Cluster angehalten. Beachten Sie, dass eine Herunterskalierung die Clusterleistung beeinträchtigen kann.

Sie haben nun einen zentralen Hoch- oder Herunterskalierungsvorgang für Ihren Azure Data Explorer-Cluster durchgeführt. Sie können auch die [horizontale Skalierung des Clusters verwalten](manage-cluster-scale-out.md), um die Instanzanzahl basierend auf den von Ihnen angegebenen Metriken dynamisch zu skalieren.

Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.
