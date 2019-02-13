---
title: Skalieren von Azure-Daten-Explorer-Clustern zur Anpassung an sich änderndem Bedarf
description: Dieser Artikel beschreibt Schritte zum horizontalen Hoch- und Herunterskalieren eines Azure-Daten-Explorer-Clusters basierend auf sich änderndem Bedarf.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 213a49d87d5e9f801bb17604a322c231a4e3dee2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735313"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Verwalten der Clusterskalierung bei sich änderndem Bedarf

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Aber der Bedarf in einem Cluster kann nicht mit 100%iger Genauigkeit vorhergesagt werden. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist. Ein besserer Ansatz ist die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazität und CPU hinzugefügt und entfernt werden. In diesem Artikel wird veranschaulicht, wie Sie einen Cluster zentral hochskalieren.

Navigieren Sie zu Ihrem Cluster, und wählen Sie unter **Einstellungen** die Option **Zentral hochskalieren** aus.

Daraufhin wird die Liste der verfügbaren SKUs angezeigt. Sie können aus der Liste der aktivierten Karten auswählen. In der folgenden Abbildung kann beispielsweise nur eine SKU aus D14_vs ausgewählt werden.

![Zentrales Hochskalieren](media/manage-cluster-scale-up/scale-up.png)

D13_v2 ist deaktiviert, weil es sich dabei um die aktuelle SKU des Clusters handelt. L8 und L16 sind deaktiviert, weil sie nicht in der Region verfügbar sind, in der sich der Cluster befindet.

Wenn Sie eine andere SKU verwenden möchten, klicken Sie einfach auf die gewünschte SKU, und klicken Sie dann auf die Schaltfläche **Auswählen**.

[!NOTE] Die zentrale Hochskalierung kann einige Minuten dauern, und während dieser Zeit wird Ihr Cluster angehalten. Beachten Sie, dass eine Herunterskalierung die Clusterleistung beeinträchtigen kann.

Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.