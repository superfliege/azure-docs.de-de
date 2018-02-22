---
title: Konfigurieren der Vermerkdauer in der Azure Time Series Insights-Umgebung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Vermerkdauer in Ihrer Azure Time Series Insights-Umgebung konfigurieren.
services: time-series-insights
ms.service: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
editor: MicrosoftDocs/tsidocs
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 02/09/2018
ms.openlocfilehash: bd688f516e200a37a6c88a8779282f7391eaf8b8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="configuring-retention-in-time-series-insights"></a>Konfigurieren der Vermerkdauer in Time Series Insights
In diesem Artikel wird beschrieben, wie Sie die **Datenaufbewahrungsdauer** und das **Verhalten bei Überschreitung des Speicherlimits** in Azure Time Series Insights konfigurieren.

Jeder Time Series Insights-Umgebung (TSI) verfügt über eine Einstellung zum Konfigurieren der **Datenaufbewahrungsdauer**. Der Wert kann zwischen 1 und 400 Tagen liegen. Die Daten werden anhand der Speicherkapazität der Umgebung oder der Aufbewahrungsdauer (1–400) gelöscht, je nachdem, welcher Fall zuerst eintritt.

Jede TSI-Umgebung verfügt über die zusätzliche Einstellung **Verhalten bei Überschreitung des Speicherlimits**. Diese Einstellung steuert das Verhalten bei Eingang und Bereinigung, wenn in einer Umgebung die maximale Kapazität erreicht ist. Sie können zwischen zwei Verhaltensweisen wählen:
- **Alte Daten bereinigen** (Standard)  
- **Eingang anhalten**

Ausführliche Informationen zu einem besseren Verständnis dieser Einstellungen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Konfigurieren der Datenaufbewahrung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie die vorhandene Time Series Insights-Umgebung. Wählen Sie im Azure-Portal im Menü auf der linken Seite **Alle Ressourcen** aus. Wählen Sie Ihre Time Series Insights-Umgebung aus.

3. Wählen Sie unter der Überschrift **Einstellungen** die Option **Konfigurieren** aus.

4. Wählen Sie die **Datenaufbewahrungsdauer** aus, um die Vermerkdauer über den Schieberegler oder durch Eingabe einer Zahl in das Textfeld zu konfigurieren.

5. Notieren Sie die Einstellung **Kapazität**, da diese Konfiguration Auswirkungen auf die maximale Anzahl von Datenereignissen und die Gesamtspeicherkapazität für das Speichern von Daten hat. 

6. Schalten Sie die Einstellung **Verhalten bei Überschreitung des Speicherlimits** um. Wählen Sie als Verhalten **Alte Daten bereinigen** oder **Eingang anhalten** aus.

7. Wählen Sie zum Konfigurieren der Änderungen **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).
