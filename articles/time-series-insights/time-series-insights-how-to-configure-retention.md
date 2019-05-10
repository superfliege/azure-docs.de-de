---
title: Konfigurieren der Vermerkdauer in der Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Vermerkdauer in Ihrer Azure Time Series Insights-Umgebung konfigurieren.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: 5f95ed2103bcd7787c36836641bbe3f7e8d8b263
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406344"
---
# <a name="configuring-retention-in-time-series-insights"></a>Konfigurieren der Vermerkdauer in Time Series Insights

In diesem Artikel wird beschrieben, wie Sie die **Datenaufbewahrungsdauer** und das **Verhalten bei Überschreitung des Speicherlimits** in Azure Time Series Insights konfigurieren.

## <a name="summary"></a>Zusammenfassung

Jeder Time Series Insights-Umgebung (TSI) verfügt über eine Einstellung zum Konfigurieren der **Datenaufbewahrungsdauer**. Der Wert kann zwischen 1 und 400 Tagen liegen. Die Daten werden anhand der Speicherkapazität der Umgebung oder der Aufbewahrungsdauer (1–400) gelöscht, je nachdem, welcher Fall zuerst eintritt.

Jede TSI-Umgebung verfügt über die zusätzliche Einstellung **Verhalten bei Überschreitung des Speicherlimits**. Diese Einstellung steuert das Verhalten bei Eingang und Bereinigung, wenn in einer Umgebung die maximale Kapazität erreicht ist. Sie können zwischen zwei Verhaltensweisen wählen:

- **Alte Daten bereinigen** (Standard)
- **Eingang anhalten**

Ausführliche Informationen zu einem besseren Verständnis dieser Einstellungen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Konfigurieren der Datenaufbewahrung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie die vorhandene Time Series Insights-Umgebung. Wählen Sie im Azure-Portal im Menü auf der linken Seite **Alle Ressourcen** aus. Wählen Sie Ihre Time Series Insights-Umgebung aus.

1. Wählen Sie unter der Überschrift **Einstellungen** die Option **Konfigurieren** aus.

1. Wählen Sie die **Datenaufbewahrungsdauer** aus, um die Vermerkdauer über den Schieberegler oder durch Eingabe einer Zahl in das Textfeld zu konfigurieren.

1. Notieren Sie die Einstellung **Kapazität**, da diese Konfiguration Auswirkungen auf die maximale Anzahl von Datenereignissen und die Gesamtspeicherkapazität für das Speichern von Daten hat.

1. Schalten Sie die Einstellung **Verhalten bei Überschreitung des Speicherlimits** um. Wählen Sie als Verhalten **Alte Daten bereinigen** oder **Eingang anhalten** aus.

1. Wählen Sie zum Konfigurieren der Änderungen **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).