---
title: Erstellen von benutzerdefinierten Analysen für die Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie als Operator benutzerdefinierte Analysen für Ihre Azure IoT Central-Anwendung analysieren.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 2f19998429daab9000e35b520673c417b4bc828a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34199934"
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Gewusst wie: Analysieren von Gerätedaten durch Analysen

Microsoft Azure IoT Central stellt umfassende Analysefunktionen bereit, um große Mengen an Daten von Ihren Geräten zu verarbeiten. Mithilfe von Analysen können Sie Daten für eine [Gerätegruppe](howto-use-device-sets.md) in Ihrer Anwendung anzeigen und analysieren. Eine Gerätegruppe ist eine benutzerdefinierte Gruppe Ihrer Geräte. Sie können die Analyse auf eine kleine Gruppe von Geräten oder auf ein einzelnes Gerät eingrenzen.

Wählen Sie als Operator im linken Navigationsmenü **Analyse**, eine Gerätegruppe und dann die im Diagramm anzuzeigenden Messungen aus. Hier sind einige Tools, die Sie verwenden können, um die Daten weiter zu unterteilen:

* **Messungen**: Wählen Sie die anzuzeigenden Messungen aus, z.B. zu Temperatur und Luftfeuchtigkeit.
* **Aggregation**: Wählen Sie die Aggregatfunktion für die Messungen aus. Beispiel: **Minimum** oder **Mittelwert**.
* **Aufteilen nach**: Führen Sie ein Drilldown aus, indem Sie die Daten nach Geräteeigenschaften oder Gerätenamen aufteilen. Beispiel für eine Aufteilung nach Gerätestandort:

     ![Hauptseite „Analyse“](media\howto-create-analytics\analytics-main.png)

* **Zeitbereich**: Sie können einen Zeitbereich aus einem der vordefinierten Zeitbereiche auswählen oder einen benutzerdefinierten Zeitbereich erstellen: ![Zeitbereich auf der Seite „Analyse“](media\howto-create-analytics\analytics-time-range.png)

## <a name="change-the-visualizations"></a>Ändern der Visualisierungen

Die Diagramme können durch Auswahl von einem der drei Modi entsprechend Ihrer Visualisierungsanforderungen angepasst werden:

* **Gestapelt**: Für jede Messung werden Diagramme als Stapel dargestellt, wobei jedes Diagramm mit einer eigenen Y-Achse versehen ist. Gestapelte Diagramme sind nützlich, wenn Sie mehrere Messungen ausgewählt haben und unterschiedliche Ansichten von diesen Messungen anzeigen möchten.
* **Nicht gestapelt**: Für jedes Measure wird ein Diagramm anhand einer Y-Achse dargestellt, wobei sich die Werte für die Y-Achse jedoch basierend auf dem markierten Measure ändern. Nicht gestapelte Diagramme sind nützlich, wenn Sie mehrere Measures überlagern und für denselben Zeitraum Muster in diesen Measures ermitteln möchten.
* **Gemeinsame Y-Achse**: Alle Diagramme besitzen dieselbe Y-Achse, und die Werte für die Achse ändern sich nicht. Diagramme mit gemeinsamen Y-Achsen sind hilfreich, wenn Sie ein einziges Measure untersuchen möchten und gleichzeitig die Daten nach bestimmten Kriterien aufteilen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie benutzerdefinierte Analysen für Ihre Azure IoT Central-Anwendung erstellt werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Eine Node.js-Anwendung vorbereiten und verbinden](howto-connect-nodejs.md)