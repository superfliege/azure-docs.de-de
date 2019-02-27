---
title: Skalieren von Azure-Daten-Explorer-Clustern zur Anpassung an sich änderndem Bedarf
description: Dieser Artikel beschreibt Schritte zum horizontalen Hoch- und Herunterskalieren eines Azure-Daten-Explorer-Clusters basierend auf sich änderndem Bedarf.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 15ef5282e0a073e870f2ac12b5fc442407535770
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408441"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Verwalten der Clusterskalierung bei sich änderndem Bedarf

Die richtige Größe eines Clusters ist entscheidend für die Leistung von Azure-Daten-Explorer. Aber der Bedarf in einem Cluster kann nicht mit 100%iger Genauigkeit vorhergesagt werden. Eine statische Clustergröße kann zu einer Unter- oder Überauslastung führen, was beides nicht ideal ist. Ein besserer Ansatz ist die *Skalierung* eines Clusters, wobei je nach Bedarf Kapazitäten hinzugefügt und entfernt werden. Es gibt zwei Workflows für die Skalierung: zentrale Skalierung und horizontale Skalierung. In diesem Artikel wird der Workflow für die horizontale Skalierung beschrieben.

In diesem Artikel wird veranschaulicht, wie Sie die horizontale Skalierung eines Clusters verwalten, die auch als Autoskalierung bezeichnet wird. Die automatische Skalierung ermöglicht Ihnen das horizontale Skalieren der Instanzenzahl basierend auf vordefinierten Regeln und Zeitplänen. Legen Sie die Autoskalierungseinstellungen für Ihren Cluster im Azure-Portal wie unten beschrieben fest.

Navigieren Sie zu Ihrem Cluster, und wählen Sie unter **Einstellungen** wählen **Horizontal hochskalieren** aus. Wählen Sie unter **Konfigurieren** die Option **Automatische Skalierung aktivieren** aus.

![Aktivieren der automatischen Skalierung](media/manage-cluster-scaling/enable-autoscale.png)

Die folgende Grafik zeigt die Abfolge der nächsten Schritte. Weitere Details finden Sie unter der Grafik.

![Skalierungsregel](media/manage-cluster-scaling/scale-rule.png)

1. Geben Sie unter **Name der Einstellung für die automatische Skalierung** einen Namen ein, z. B. *Horizontale Skalierung: Cacheauslastung*.

1. Wählen Sie unter **Skalierungsmodus** die Option **Basierend auf einer Metrik skalieren** aus. Dieser Modus ermöglicht eine dynamische Skalierung. Sie können auch **Auf eine bestimmte Anzahl von Instanzen skalieren** auswählen.

1. Wählen Sie **Regel hinzufügen** aus.

1. Geben Sie im Abschnitt **Skalierungsregel** auf der rechten Seite Werte für jede Einstellung an.

    **Kriterien**

    | Einstellung | Beschreibung und Wert |
    | --- | --- | --- |
    | **Zeitaggregation** | Wählen Sie ein Aggregationskriterium aus, z. B. **Durchschnitt**. |
    | **Metrikname** | Wählen Sie die Metrik aus, auf der der Skalierungsvorgang basieren soll, z. B. **Cacheauslastung**. |
    | **Statistik zum Aggregationsintervall** | Wählen Sie zwischen **Durchschnitt**, **Minimum**, **Maximum** und **Summe** aus. |
    | **Operator** | Wählen Sie die entsprechende Option aus, z. B. **Größer als oder gleich**. |
    | **Schwellenwert** | Wählen Sie einen passenden Wert aus. Für die Cacheauslastung ist beispielsweise ist 80 % ein guter Ausgangspunkt. |
    | **Dauer (in Minuten)** | Wählen Sie eine angemessene Zeitspanne, die das System bei der Berechnung von Metriken berücksichtigt. Beginnen Sie mit dem Standardwert von 10 Minuten. |
    |  |  |

    **Aktion**

    | Einstellung | Beschreibung und Wert |
    | --- | --- | --- |
    | **Vorgang** | Wählen Sie die entsprechende Option zum horizontalen Hoch- oder Herunterskalieren aus. |
    | **Anzahl der Instanzen** | Wählen Sie die Anzahl von Knoten oder Instanzen aus, die hinzugefügt oder entfernt werden sollen, wenn eine Metrikbedingung erfüllt ist. |
    | **Abkühlen (Minuten)** | Wählen Sie ein geeignetes Intervall für die Zeit zwischen den einzelnen Skalierungsvorgängen. Beginnen Sie mit dem Standardwert von 5 Minuten. |
    |  |  |

1. Wählen Sie **Hinzufügen**.

1. Geben Sie im Abschnitt **Instanzgrenzwerte** auf der linken Seite Werte für jede Einstellung an.

    | Einstellung | Beschreibung und Wert |
    | --- | --- | --- |
    | *Mindestanforderungen* | Die Anzahl der Instanzen, unter die Ihr Cluster unabhängig von der Auslastung nicht herunterskaliert wird. |
    | *Maximum* | Die Anzahl der Instanzen, über die Ihr Cluster unabhängig von der Auslastung nicht hochskaliert wird. |
    | *Standard* | Standardanzahl der Instanzen, die verwendet wird, wenn es Probleme beim Lesen von Ressourcenmetriken gibt. |
    |  |  |

1. Wählen Sie **Speichern** aus.

Sie haben nun einen horizontalen Skalierungsvorgang für Ihren Azure-Daten-Explorer-Cluster konfiguriert. Fügen Sie eine weitere Regel für einen Vorgang zum horizontalen Herunterskalieren hinzu. So kann Ihr Cluster basierend auf den von Ihnen angegebenen Metriken dynamisch skaliert werden.

Sie können einen [Cluster auch zentral hochskalieren](manage-cluster-scale-up.md), um eine geeignete Größenanpassung vorzunehmen.

Wenn Sie Hilfe bei Clusterskalierungsproblemen benötigen, erstellen Sie im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) eine Supportanfrage.
