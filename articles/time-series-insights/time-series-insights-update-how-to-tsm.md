---
title: Datenmodellierung in Azure Time Series Insights (Vorschauversion) | Microsoft-Dokumentation
description: Grundlegendes zur Datenmodellierung in Azure Time Series Insights Preview.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 1686b52c9dacf5c78335f76bd19b41c4c7d15cee
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555403"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Datenmodellierung in Azure Time Series Insights Preview

In diesem Dokument wird beschrieben, wie Sie mit Zeitreihenmodellen gemäß der Azure Time Series Insights Preview arbeiten. Es werden mehrere allgemeine Datenszenarien dargestellt.

Weitere Informationen zur Verwendung des Updates finden Sie unter [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).

## <a name="types"></a>Typen

### <a name="create-a-single-type"></a>Erstellen eines einzelnen Typs

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Typen** aus. Reduzieren Sie den Bereich, um den Fokus auf die Typen von Zeitreihenmodellen zu legen.

    ![Portal_one][1]

1. Wählen Sie **Hinzufügen**.
1. Geben Sie alle Details ein, die sich auf Typen beziehen, und wählen Sie **Erstellen** aus. Diese Aktion erstellt Typen in der Umgebung.

    ![Portal_two][2]

### <a name="bulk-upload-one-or-more-types"></a>Massenupload eines oder mehrerer Typen

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Typnutzlast enthält.
1. Wählen Sie die Option **Hochladen**.

    ![Portal_three][3]

### <a name="edit-a-single-type"></a>Bearbeiten eines einzelnen Typs

Wählen Sie den Typ und dann **Bearbeiten** aus. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

![Portal_four][4]

### <a name="delete-a-type"></a>Löschen eines Typs

Wählen Sie den Typ aus, und wählen Sie **Bearbeiten** aus. Wenn den Typen keine Instanzen zugeordnet sind, wird er gelöscht.

![Portal_five][5]

## <a name="hierarchies"></a>Hierarchien

### <a name="create-a-single-hierarchy"></a>Erstellen einer einzelnen Hierarchie

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Hierarchien** aus. Reduzieren Sie den Bereich, um den Fokus auf die Hierarchien von Zeitreihenmodellen zu legen.

    ![Portal_six][6]

1. Wählen Sie **Hinzufügen**.

    ![Portal_seven][7]

1. Wählen Sie im rechten Bereich **Ebene hinzufügen** aus.

    ![Portal_eight][8]

1. Geben Sie die Hierarchiedetails ein, und wählen Sie **Erstellen** aus.

    ![Portal_nine][9]

### <a name="bulk-upload-one-or-more-hierarchies"></a>Massenupload einer oder mehrerer Hierarchien

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Hierarchienutzlast enthält.
1. Wählen Sie die Option **Hochladen**.

    ![Portal_ten][10]

### <a name="edit-a-single-hierarchy"></a>Bearbeiten einer einzelnen Hierarchie

Wählen Sie die Hierarchie und dann **Bearbeiten** aus. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

![Portal_eleven][11]

### <a name="delete-a-hierarchy"></a>Löschen einer Hierarchie

Wählen Sie die Hierarchie und dann **Löschen** aus. Wenn der Hierarchie keine Instanzen zugeordnet sind, wird sie gelöscht.

![Portal_twelve][12]

## <a name="instances"></a>Instanzen

### <a name="create-a-single-instance"></a>Erstellen einer einzelnen Instanz

1. Wechseln Sie zum Auswahlbereich für Zeitreihenmodelle, und wählen Sie im Menü **Instanzen** aus. Reduzieren Sie den Bereich, um den Fokus auf die Instanzen von Zeitreihenmodellen zu legen.

    ![Portal_thirteen][13]

1. Wählen Sie **Hinzufügen**.

    ![Portal_fourteen][14]

1. Geben Sie die Instanzdetails ein, wählen Sie die Zuordnung von Typ und Hierarchie aus, und wählen Sie **Erstellen** aus.

### <a name="bulk-upload-one-or-more-instances"></a>Massenupload einer oder mehrerer Instanzen

1. Wählen Sie die Option **JSON hochladen** aus.
1. Wählen Sie die Datei aus, die die Instanzennutzlast enthält.

    ![Portal_fifteen][15]

1. Wählen Sie die Option **Hochladen**.

### <a name="edit-a-single-instance"></a>Bearbeiten einer einzelnen Instanz

Wählen Sie die Instanz und dann **Bearbeiten** aus. Nehmen Sie die erforderlichen Änderungen vor, und wählen Sie **Speichern** aus.

![Portal_sixteen][16]

### <a name="delete-an-instance"></a>Löschen einer Instanz

Wählen Sie die Instanz und dann **Löschen** aus. Wenn den Instanzen keine Ereignisse zugeordnet sind, wird sie gelöscht.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Zeitreihenmodellen finden Sie unter [Datenmodellierung](./time-series-insights-update-tsm.md).
- Weitere Informationen zur Preview finden Sie unter [Visualisieren von Daten im Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).
- Informationen zu unterstützten JSON-Formen finden Sie unter [Unterstützte JSON-Formen](./time-series-insights-send-events.md#json).

<!-- Images -->
[1]: media/v2-update-how-to-tsm/portal_one.png
[2]: media/v2-update-how-to-tsm/portal_two.png
[3]: media/v2-update-how-to-tsm/portal_three.png
[4]: media/v2-update-how-to-tsm/portal_four.png
[5]: media/v2-update-how-to-tsm/portal_five.png
[6]: media/v2-update-how-to-tsm/portal_six.png
[7]: media/v2-update-how-to-tsm/portal_seven.png
[8]: media/v2-update-how-to-tsm/portal_eight.png
[9]: media/v2-update-how-to-tsm/portal_nine.png
[10]: media/v2-update-how-to-tsm/portal_ten.png
[11]: media/v2-update-how-to-tsm/portal_eleven.png
[12]: media/v2-update-how-to-tsm/portal_twelve.png
[13]: media/v2-update-how-to-tsm/portal_thirteen.png
[14]: media/v2-update-how-to-tsm/portal_fourteen.png
[15]: media/v2-update-how-to-tsm/portal_fifteen.png
[16]: media/v2-update-how-to-tsm/portal_sixteen.png