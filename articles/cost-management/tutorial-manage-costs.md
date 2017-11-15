---
title: Verwalten von Kosten mit Azure Cost Management | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Kosten mit Kostenzuteilung sowie Showback- und Chargeback-Berichten verwalten.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/06/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 83ddc0cb4227235069b0027a24a52f4d8e818126
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2017
---
# <a name="manage-costs-by-using-azure-cost-management"></a>Verwalten von Kosten mit Azure Cost Management

Sie verwalten die Kosten und erstellen Showback-Berichte in Azure Cost Management von Cloudyn, indem Sie die Kosten basierend auf Tags zuteilen. Beim Prozess der Kostenzuteilung werden Kosten Ihren verbrauchten Cloudressourcen zugewiesen. Die Kosten werden vollständig zugeteilt, wenn Ihre gesamten Ressourcen mit Tags kategorisiert sind. Nachdem die Kosten zugeteilt wurden, können Sie für Ihre Benutzer über Dashboards und Berichte Showback- oder Chargeback-Informationen bereitstellen. Es kann aber sein, dass viele Ressourcen kein Tag aufweisen oder nicht mit einem Tag versehen werden können, wenn Sie mit der Nutzung von Cost Management beginnen.

Beispielsweise bemühen Sie sich um die Erstattung von Engineering-Kosten. Sie müssen in der Lage sein, für Ihr Engineering-Team darzustellen, dass Sie basierend auf den Ressourcenkosten einen bestimmten Betrag benötigen. Sie können einen Bericht mit allen verbrauchten Ressourcen präsentieren, die über das Tag *Engineering* verfügen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden von benutzerdefinierten Tags zum Zuteilen von Kosten
> * Erstellen von Showback- und Chargeback-Berichten

## <a name="use-custom-tags-to-allocate-costs"></a>Verwenden von benutzerdefinierten Tags zum Zuteilen von Kosten

Zu Beginn der Kostenzuteilung definieren Sie zuerst den Bereich, indem Sie ein Kostenmodell verwenden. Mit dem Kostenmodell werden die Kosten nicht geändert, sondern verteilt. Wenn Sie ein Kostenmodell erstellen, segmentieren Sie Ihre Daten nach Kostenentität, Konto oder Abonnement und nach mehreren Tags. Zu den häufig verwendeten Beispieltags gehören beispielsweise ein Abrechnungscode, eine Kostenstelle oder ein Gruppenname. Außerdem sind Tags beim Durchführen von Showback- oder Chargeback-Vorgängen mit anderen Teilen Ihrer Organisation nützlich.

Wählen Sie zum Erstellen eines benutzerdefinierten Kostenzuteilungsmodells im Menü des Berichts die Optionen **Kosten** &gt; **Kostenverwaltung** &gt; **Cost Allocation 360°** (Kostenzuteilung 360°).

![Auswahl von „Cost Allocation 360°“ (Kostenzuteilung 360°)](./media/tutorial-manage-costs/cost-allocation-360.png)

Wählen Sie auf der Seite **Cost Allocation 360°** (Kostenzuteilung 360°) die Option **Hinzufügen**, und geben Sie dann einen Namen und eine Beschreibung für Ihr Kostenmodell ein. Wählen Sie entweder alle Konten oder einzelne Konten aus. Wenn Sie mehrere einzelne Konten verwenden möchten, können Sie mehrere Konten unterschiedlicher Clouddienstanbieter auswählen. Klicken Sie als Nächstes auf **Kategorisierung**, um die ermittelten Tags auszuwählen, mit denen Ihre Kostendaten kategorisiert sind. Wählen Sie die Tags (Kategorien) aus, die Sie in Ihr Modell einbeziehen möchten. Im folgenden Beispiel ist das Tag **Unit** (Einheit) ausgewählt.

![Beispiel für Kostenmodellkategorisierung](./media/tutorial-manage-costs/cost-model01.png)



Im Beispiel ist zu sehen, dass 14.444 US-Dollar nicht kategorisiert sind (keine Tags aufweisen).

Wählen Sie als Nächstes die Option **Uncategorized Resources** (Nicht kategorisierte Ressourcen), und wählen Sie Dienste aus, die über nicht zugeteilte Kosten verfügen. Definieren Sie anschließend Regeln zum Zuteilen von Kosten.

Es kann beispielsweise sein, dass Sie Ihre Azure-Speicherkosten gleichmäßig auf virtuelle Azure-Computer (VMs) verteilen möchten. Wählen Sie hierzu den Dienst **Azure/Storage** und dann die Optionen **Proportional to Categorized** (Proportional zu kategorisiert) und **Azure/VM** aus. Wählen Sie anschließend **Erstellen**.

![Beispiel: Kostenmodell-Zuteilungsregel für die gleichmäßige Verteilung](./media/tutorial-manage-costs/cost-model02.png)



Ein anderes Beispiel ist ein Fall, in dem Sie Ihre gesamten Azure-Netzwerkkosten einer bestimmten Geschäftseinheit Ihrer Organisation zuteilen möchten. Wählen Sie hierzu den Dienst **Azure/Network** (Azure/Netzwerk) und dann die Option **Explicit Distribution** (Explizite Verteilung). Legen Sie den Verteilungsprozentsatz anschließend auf den Wert 100 fest, und wählen Sie die Geschäftseinheit aus. In der folgenden Abbildung ist dies **G&amp;A**:

![Beispiel: Kostenmodell-Zuteilungsregel für eine bestimmte Geschäftseinheit](./media/tutorial-manage-costs/cost-model03.png)



Erstellen Sie für alle verbleibenden nicht kategorisierten Ressourcen zusätzliche Zuteilungsregeln.

Falls Sie über nicht zugeteilte reservierte AWS-Instanzen (Amazon Web Services) verfügen, können Sie sie mit der Option **Reservierte Instanzen** getaggten Kategorien zuweisen.

Wählen Sie **Zusammenfassung**, um Informationen zu der Auswahl anzuzeigen, die Sie für das Zuteilen von Kosten getroffen haben. Wählen Sie **Als Entwurf speichern**, um Ihre Informationen zu speichern und später an zusätzlichen Regeln weiterzuarbeiten. Sie können auch **Speichern und aktivieren** wählen, um Ihre Informationen zu speichern und die Verarbeitung Ihres Kostenzuteilungsmodells von Cloudyn starten zu lassen.

In der Liste mit den Kostenmodellen wird Ihr neues Kostenmodell mit dem **Verarbeitungsstatus** angezeigt. Es kann einige Zeit dauern, bis die Cloudyn-Datenbank mit Ihrem Kostenmodell aktualisiert wird. Nach Abschluss der Verarbeitung wird der Status in **Abgeschlossen** geändert. Sie können dann Daten aus Ihrem Kostenmodell im Bericht mit der Kostenanalyse unter **Extended Filters** (Erweiterte Filter) &gt; **Cost Model** (Kostenmodell) anzeigen.

### <a name="category-manager"></a>Kategorie-Manager:

Der Kategorie-Manager ist ein Datenbereinigungstool, mit dem Sie die Werte mehrerer Kategorien (Tags) zusammenführen können, um neue Werte zu erstellen. Es ist ein einfaches regelbasiertes Tool, bei dem Sie eine Kategorie auswählen und Regeln erstellen, um vorhandene Werte zusammenzuführen. Beispielsweise verfügen Sie über vorhandene Kategorien für **F&amp;E** und **dev**, die beide für die Entwicklungsgruppe stehen.

Klicken Sie im Cloudyn-Portal auf das Zahnradsymbol in der rechten oberen Ecke, und wählen Sie **Category Manager** (Kategorie-Manager) aus. Wählen Sie zum Erstellen einer neuen Kategorie das Plussymbol (**+**). Geben Sie einen Namen für die Kategorie ein, und geben Sie unter **Schlüssel** dann die Kategorieschlüssel an, die Sie in die neue Kategorie einbeziehen möchten.

Beim Definieren einer Regel können Sie mehrere Werte mit einer OR-Bedingung hinzufügen. Außerdem können Sie einige einfache Zeichenfolgenvorgänge durchführen. Klicken Sie in beiden Fällen auf das Auslassungszeichen (**…**) rechts von **Regel**.

Erstellen Sie im Bereich **Regeln** eine neue Regel, um eine neue Regel zu definieren. Geben Sie unter **Regeln** beispielsweise **dev** und unter **Aktionen** dann **F&amp;E** ein. Speichern Sie Ihre neue Kategorie, wenn Sie fertig sind.

Die folgende Abbildung enthält ein Beispiel für Regeln, die für eine neue Kategorie mit dem Namen **Work-Load** erstellt wurden:

![Beispiel für Kategorie](./media/tutorial-manage-costs/category01.png)



## <a name="create-showback-and-chargeback-reports"></a>Erstellen von Showback- und Chargeback-Berichten

Das Verfahren, das von Organisationen zum Durchführen von Showback- und Chargeback-Vorgängen genutzt wird, kann stark variieren. Sie können aber alle Dashboards und Berichte im Cloudyn-Portal als Basis für beide Zwecke nutzen. Sie können allen Benutzern in Ihrer Organisation Zugriff gewähren, damit diese bei Bedarf Dashboards und Berichte anzeigen können. Showback wird für alle Kostenanalyseberichte unterstützt, da Benutzern darin die Ressourcen angezeigt werden, die sie verbraucht haben. Außerdem können Benutzer einen Drilldown für Kosten- oder Nutzungsdaten durchführen, die jeweils für ihre Gruppe der Organisation gelten.

Öffnen Sie zum Anzeigen der Ergebnisse einer Kostenzuteilung den Kostenanalysebericht, und wählen Sie das von Ihnen erstellte Kostenmodell aus. Fügen Sie anschließend eine Gruppierung nach einem oder mehreren Tags hinzu, die im Kostenmodell ausgewählt werden.

![Bericht „Kostenanalyse“](./media/tutorial-manage-costs/cost-analysis.png)

Sie können leicht Berichte erstellen und speichern, bei denen der Schwerpunkt auf bestimmten Diensten liegt, die von bestimmten Gruppen genutzt werden. Beispielsweise verfügen Sie ggf. über eine Abteilung, in der Azure-VMs häufig verwendet werden. Sie können einen Bericht erstellen, der nach Azure-VMs gefiltert ist, um die Nutzung und die Kosten anzuzeigen.

Falls Sie für andere Teams Momentaufnahmedaten bereitstellen müssen, können Sie alle Berichte im PDF- oder CSV-Format exportieren.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verwenden von benutzerdefinierten Tags zum Zuteilen von Kosten
> * Erstellen von Showback- und Chargeback-Berichten



Fahren Sie mit der Cloudyn-Dokumentation fort, um mehr über die ersten Schritte mit Cloudyn und die Verwendung der Features von Cloudyn zu erfahren.

> [!div class="nextstepaction"]
> [Cloudyn-Dokumentation](https://support.cloudyn.com/hc/)
