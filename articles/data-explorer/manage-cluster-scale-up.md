---
title: Skalieren von Azure-Daten-Explorer-Clustern zur Anpassung an sich änderndem Bedarf
description: Dieser Artikel beschreibt Schritte zum zentralen Hoch- und Herunterskalieren eines Azure Data Explorer-Clusters basierend auf sich änderndem Bedarf.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: bc3f97c798f5e040908e8103c00d3f015f8c824d
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415333"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Verwalten der Clusterskalierung bei sich änderndem Bedarf

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Aber der Bedarf in einem Cluster kann nicht mit 100%iger Genauigkeit vorhergesagt werden. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist. Ein besserer Ansatz ist die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazität und CPU hinzugefügt und entfernt werden. Es gibt zwei Workflows für die Skalierung: zentrale Skalierung und horizontale Skalierung. In diesem Artikel wird veranschaulicht, wie Sie einen Cluster zentral hochskalieren.

1. Navigieren Sie zu Ihrem Cluster, und wählen Sie unter **Einstellungen** die Option **Zentral hochskalieren** aus.

    Sie erhalten eine Liste der verfügbaren SKUs. In der folgenden Abbildung ist beispielsweise nur eine SKU verfügbar: D14_V2.

    ![Zentrales Hochskalieren](media/manage-cluster-scale-up/scale-up.png)

    D13_V2 ist deaktiviert, da es sich dabei um die aktuelle SKU des Clusters handelt. L8 und L16 sind deaktiviert, weil sie nicht in der Region verfügbar sind, in der sich der Cluster befindet.

1. Zum Ändern Ihrer SKU wählen Sie die gewünschte SKU aus und klicken auf die **Auswählen**.

> [!NOTE]
> Die zentrale Hochskalierung kann einige Minuten dauern, und während dieser Zeit wird Ihr Cluster angehalten. Beachten Sie, dass eine Herunterskalierung die Clusterleistung beeinträchtigen kann.

Sie haben nun einen zentralen Hoch- oder Herunterskalierungsvorgang für Ihren Azure Data Explorer-Cluster durchgeführt. Sie können auch eine [horizontale Skalierung des Clusters](manage-cluster-scale-out.md) – auch als Autoskalierung bezeichnet – durchführen, um basierend auf den von Ihnen angegebenen Metriken dynamisch zu skalieren.

Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.
