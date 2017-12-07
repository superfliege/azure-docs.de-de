---
title: Grundlagen zu Kostenberichten in Azure Cost Management | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie Informationen zur grundlegenden Struktur und den Funktionen von Cloudyn-Berichte.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/27/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: df2108a6e2a01195340a09eacf1c56f9d738c923
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="understanding-cost-reports"></a>Grundlagen zu Kostenberichten

In diesem Artikel erhalten Sie Informationen zur grundlegenden Struktur und den Funktionen von Cloudyn-Berichte. Die meisten Cloudyn-Berichte sind intuitiv und weisen ein einheitliches Erscheinungsbild auf. Nachdem Sie diesen Artikel gelesen haben, sind Sie zum Verwenden aller Berichte bereit. In den verschiedenen Berichten sind zahlreiche Standardfunktionen zur problemlosen Navigation in den Berichten verfügbar. Berichte können angepasst werden, und Sie haben die Wahl zwischen mehreren Optionen zum Berechnen und Anzeigen von Ergebnissen.

## <a name="report-fields-and-options"></a>Felder und Optionen von Berichten

Hier sehen Sie ein Beispiel für den Bericht für die Kosten im Lauf der Zeit, Die meisten Cloudyn-Berichte sind ähnlich aufgebaut.

![Beispielbericht](./media/understanding-cost-reports/sample-report.png)

Jeder nummerierte Bereich in der vorherigen Abbildung wird in den folgenden Informationen im Detail beschrieben:

1. **Datumsbereich**

    Verwenden Sie die Liste „Datumsbereich“, um ein voreingestelltes oder benutzerdefiniertes Berichtzeitintervall zu definieren.
2. **Gespeicherter Filter**

    Verwenden Sie die Liste „Gespeicherter Filter“, um die Gruppen und Filter zu speichern, die aktuell auf den Bericht angewendet werden. Gespeicherte Filter sind in Kosten- und Leistungsberichten verfügbar, einschließlich:

      - Kostenanalyse
      - Zuteilung
      - Ressourcenverwaltung
      - Optimierung

  Geben Sie einen Filternamen ein, und klicken Sie dann auf **Speichern**.

3. **Tags**

    Verwenden Sie den Bereich „Tags“, um nach Tag-Kategorien zu gruppieren. Bei den Tags im Menü handelt es sich um Tags für Azure-Abteilungen oder -Kostenstellen oder um die Entitäts- und Abonnement-Tags von Cloudyn. Wählen Sie Tags zum Filtern von Ergebnissen aus. Sie können auch einen Tag-Namen (Schlüsselwort) eingeben, um Ergebnisse zu filtern.

    ![Optionen auswählen](./media/understanding-cost-reports/select-options.png)

    Klicken Sie auf **Hinzufügen**, um einen neuen Filter hinzuzufügen.

    ![Filter hinzufügen](./media/understanding-cost-reports/add-filter.png)

    Das Gruppieren oder Filtern bezieht sich nicht auf Azure-Ressourcen oder Ressourcengruppen-Tags.

    Tag-Gruppierung und -Filterung für Kostenzuteilung sind in der Menüoption **Gruppen** verfügbar.

4. **Gruppen in Berichten**

    Verwenden Sie Gruppen in Kostenanalyseberichten, um Standardkategorien aus Abrechnungsdaten in Ihrem Bericht einzeln aufzuführen.  Gruppen in Kostenzuteilungsberichten zeigen jedoch Tag-basierte Kategorien an. Tag-basierte Kategorien werden im Kostenzuteilungsmodell und in aufgeschlüsselten Standardkategorien aus Abrechnungsdaten definiert.

    ![Gruppen-Tags](./media/understanding-cost-reports/groups-tags01.png)

    ![Gruppen-Tags](./media/understanding-cost-reports/groups-tags02.png)

    In Kostenzuteilungsberichten können Gruppen in Tag-basierten Gruppenkategorien Folgendes umfassen:
      - Tags
      - Ressourcengruppen-Tags
      - Kostenentitäts-Tags von Cloudyn
      - Abonnement-Tag-Kategorien für die Kostenzuteilung

  Beispiele:
     - Kostenstelle
     - Department
     - Anwendung
     - Environment
     - Kostencode

5. **Filter**

    Verwenden Sie Einfach- oder Mehrfachauswahlfilter, um Bereiche für ausgewählte Werte festzulegen. Zum Festlegen eines Filters klicken Sie auf **Hinzufügen** und wählen dann Filterkategorien und -werte aus.

6. **Kostenmodell**

    Verwenden Sie „Kostenmodell“ zum Auswählen eines Kostenmodells, das Sie zuvor mit „Kostenzuteilung 360“ erstellt haben. Je nach Ihren Kostenzuteilungsanforderungen können Sie über mehrere Cloudyn-Kostenmodelle verfügen. Die Kostenzuteilungsanforderungen Ihrer einzelnen Organisationsteams können unterschiedlich sein. Jedes Team kann ein eigenes dediziertes Kostenmodell nutzen.

    Informationen zum Erstellen einer Modelldefinition für Kostenzuteilung finden Sie unter [Verwenden von benutzerdefinierten Tags zum Zuteilen von Kosten](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Amortisierung**

    Verwenden Sie „Amortisierung“ in Kostenzuteilungsberichten zum Anzeigen von nicht nutzungsbasierten Dienstgebühren oder einmalig zu zahlenden Kosten und zur gleichmäßigen Verteilung dieser Kosten während ihrer Lebensdauer. Beispiele für die einmalige Gebühren:
    - Jährliche Supportgebühren
    - Jährliche Gebühren für Sicherheitskomponenten
    - Gebühren für den Kauf von reservierten VM-Instanzen
    - Einige Azure Marketplace-Elemente

  Wählen Sie unter Amortisierung die Option **Amortized cost** (Amortisierte Kosten) oder **Actual Cost** (Ist-Kosten) aus.

8. **Lösung**

    Verwenden Sie „Lösung“, um die Zeitauflösung innerhalb des ausgewählten Datumsbereichs auszuwählen. Ihre Zeitauflösung legt fest, wie Einheiten im Bericht angezeigt werden. Folgende Optionen sind verfügbar:
    - Täglich
    - Wöchentlich
    - Monatlich
    - Vierteljährlich
    - Jährlich

9. **Zuteilungsregeln**

    Verwenden Sie Zuteilungsregeln, um die Kostenneuberechnung für die Kostenzuteilung anzuwenden oder zu deaktivieren. Sie können die Neuberechnung der Kostenzuteilung für Abrechnungsdaten aktivieren oder deaktivieren. Die Neuberechnung wird für die ausgewählten Kategorien im Bericht durchgeführt. Sie ermöglicht Ihnen das Bewerten der Auswirkung einer Neuberechnung der Kostenzuteilung für Abrechnungsrohdaten.

10. **Nicht kategorisiert**

    Verwenden Sie „Nicht kategorisiert“, um nicht kategorisierte Kosten in den Bericht einzubeziehen bzw. davon auszuschließen.

11. **Felder anzeigen/ausblenden**

    Die Option zum Anzeigen/Ausblenden Option hat keine Auswirkung in Berichten.

12.   **Anzeigeformate**

    Verwenden Sie „Anzeigeformate“, um verschiedene Diagramm- oder Tabellenansichten auszuwählen.

    ![Anzeigeformate](./media/understanding-cost-reports/display-formats.png)

13. **Mehrfarbig**

    Verwenden Sie „Mehrfarbig“, um in Ihrem Bericht die Farbe von Diagrammen festzulegen.

14. **Aktionen**

    Verwenden Sie „Aktionen“ zum Speichern, Exportieren oder Planen des Berichts.

## <a name="next-steps"></a>Nächste Schritte

- Falls Sie das erste Tutorial zum Kostenmanagement noch nicht abgeschlossen haben, lesen Sie es unter [Überprüfen der Nutzung und der Kosten](tutorial-review-usage.md).
