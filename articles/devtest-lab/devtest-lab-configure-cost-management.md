---
title: "Anzeigen des monatlichen geschätzten Labkostentrends in Azure DevTest Labs | Microsoft Docs"
description: "Informieren Sie sich über das Diagramm „Monatlicher geschätzter Kostentrend“ in Azure DevTest Labs."
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: v-craic
ms.openlocfilehash: 660180fac4f4743bc543b1babf7dbe921bf8c26b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2018
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>Anzeigen des monatlichen geschätzten Labkostentrends in Azure DevTest Labs
Mithilfe des DevTest Labs-Features für das Kostenmanagement können Sie die Kosten Ihres Labs nachverfolgen. Dieser Artikel veranschaulicht, wie Sie das Diagramm **Monatlicher geschätzter Kostentrend** verwenden, um die bisherigen geschätzten Kosten für den aktuellen Kalendermonat sowie die veranschlagten Kosten am Monatsende für den aktuellen Kalendermonat anzuzeigen. Außerdem wird in diesem Artikel beschrieben, wie Sie die Labkosten besser verwalten können, indem Sie Ausgabenziele und Schwellenwerte festlegen, bei denen beim Erreichen des entsprechenden Werts das Melden der Ergebnisse durch DevTest Labs ausgelöst wird.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>Anzeigen des Diagramms „Monatlicher geschätzter Kostentrend“
Führen Sie folgende Schritte aus, um das Diagramm „Monatlicher geschätzter Kostentrend“ anzuzeigen: 

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie ggf. **Alle Dienste** und dann in der Liste die Option **DevTest Labs**. (Ihr Lab wird im Dashboard unter Umständen bereits unter **Alle Ressourcen** angezeigt.)
1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  
1. Wählen Sie im Bereich **Übersicht** des Labs die Option **Konfiguration und Richtlinien** aus.   
1. Wählen Sie auf der linken Seite unter **KOSTENÜBERWACHUNG** die Option **Kostentrend**.

   Der folgende Screenshot zeigt ein Beispiel eines Kostendiagramms. 
   
    ![Kostendiagramm](./media/devtest-lab-configure-cost-management/graph.png)

Der Wert für **Estimated cost** (Geschätzte Kosten) entspricht den geschätzten Kosten im aktuellen Kalendermonat bis zum heutigen Tag. Der Wert für **Projected cost** (Prognostizierte Kosten) entspricht den geschätzten Kosten für den gesamten aktuellen Kalendermonat, berechnet anhand der Labkosten für die vergangenen fünf Tage.

Die Kostenbeträge werden auf die nächste ganze Zahl aufgerundet. Beispiel:  

* 5.01 wird auf 6 aufgerundet. 
* 5.50 wird auf 6 aufgerundet.
* 5.99 wird auf 6 aufgerundet.

Oberhalb des Diagramms wird darauf hingewiesen, dass es sich bei den in diesem Diagramm standardmäßig angezeigten Kosten um *geschätzte* Kosten handelt, die anhand der geltenden Preise für die [nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) kalkuliert werden. Sie können auch Ihre eigenen Ausgabenziele festlegen, die in den Diagrammen angezeigt werden, indem Sie die [Kostenziele für Ihr Lab verwalten](#managing-cost-targets-for-your-lab).

Darüber hinaus wird bei der Kostenkalkulation Folgendes *nicht* berücksichtigt:

* CSP- und Dreamspark-Abonnements werden derzeit nicht unterstützt, da Azure DevTest Labs die [Azure-Abrechnungs-APIs](../billing/billing-usage-rate-card-overview.md) zum Berechnen der Labkosten verwendet und diese APIs keine CSP- oder Dreamspark-Abonnements unterstützen.
* Sonderpreise. Zurzeit können Sie keine Sonderpreise (wie in Ihrem Abonnement angezeigt) verwenden, die Sie mit Microsoft oder Microsoft-Partnern vereinbart haben. Es werden nur Preise für die nutzungsbasierte Bezahlung verwendet.
* Steuern
* Rabatte
* Rechnungswährung. Zurzeit werden die Labkosten nur in USD angezeigt.

## <a name="managing-cost-targets-for-your-lab"></a>Verwalten von Kostenzielen für Ihr Lab
Mit DevTest Labs können Sie die Kosten für Ihr Lab besser verwalten, indem Sie ein Ausgabenziel festlegen, das Sie dann im Diagramm „Monatlicher geschätzter Kostentrend“ anzeigen können. DevTest Labs ermöglicht auch das Senden einer Benachrichtigung, wenn das angegebene Ausgabenziel oder der Schwellenwert erreicht ist. 

1. Wählen Sie im Bereich **Übersicht** des Labs die Option **Konfiguration und Richtlinien**.
1. Wählen Sie auf der linken Seite unter **KOSTENÜBERWACHUNG** die Option **Kostentrend**.

    ![Schaltfläche „Ziel verwalten“](./media/devtest-lab-configure-cost-management/cost-trend.png)

1. Wählen Sie im Bereich **Kostentrend** die Option **Ziel verwalten**.

    ![Schaltfläche „Ziel verwalten“](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)

1. Geben Sie im Bereich „Ziel verwalten“ Ihr gewünschtes Ausgabenziel und die Schwellenwerte an. Sie können auch festlegen, ob die einzelnen ausgewählten Schwellenwerte im Kostentrenddiagramm oder über eine Webhookbenachrichtigung gemeldet werden.

    ![Bereich „Ziel verwalten“](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Wählen Sie einen Zeitraum, für den die Kostenziele nachverfolgt werden sollen.
      - **Monatlich**: Die Kostenziele werden pro Monat nachverfolgt.
      - **Fest**: Die Kostenziele werden für den Datumsbereich nachverfolgt, den Sie in den Feldern für das Startdatum und das Enddatum angeben. Dies kann ggf. damit übereinstimmen, wie lange die Ausführung Ihres Projekts geplant ist.
   - Geben Sie **Zielkosten** an. Dies kann beispielsweise ein Betrag sein, den Sie für dieses Lab im definierten Zeitraum aufwenden möchten.
   - Aktivieren bzw. deaktivieren Sie die Schwellenwerte, die gemeldet werden sollen – in Inkrementen von 25% bis zum Höchstwert von 125% der angegebenen **Zielkosten**.
      - **Benachrichtigen**: Wenn dieser Schwellenwert erreicht ist, werden Sie über eine Webhook-URL benachrichtigt, die Sie angegeben haben.
      - **Im Diagramm zeichnen**: Wenn dieser Schwellenwert erreicht ist, werden die Ergebnisse im Kostentrendgraphen angegeben, und Sie können diesen Graphen anzeigen. Die Vorgehensweise ist unter [Anzeigen des Diagramms „Monatlicher geschätzter Kostentrend“](#viewing-the-monthly-estimated-cost-trend-chart) beschrieben.
   - Wenn Sie für die Erreichung des Schwellenwerts die Option **Benachrichtigen** wählen, müssen Sie eine Webhook-URL angeben. Wählen Sie im Bereich „Kostenintegrationen“ die Option **Klicken Sie hier, um eine Integration hinzuzufügen**.

      Geben Sie im Bereich „Benachrichtigung konfigurieren“ eine Webhook-URL ein, und wählen Sie **OK**.

       ![Bereich „Benachrichtigung konfigurieren“](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Wenn Sie **Benachrichtigen** angeben, müssen Sie eine Webhook-URL definieren.
      - Entsprechend müssen Sie beim Definieren einer Webhook-URL im Bereich „Kostenschwellenwert“ die Option **Benachrichtigung** auf **Ein** festlegen.
      - Vor der Eingabe ist es erforderlich, dass Sie einen Webhook erstellen.  

      Weitere Informationen zu Webhooks finden Sie unter [Erstellen eines Webhooks oder einer API-Azure-Funktion](../azure-functions/functions-create-a-web-hook-or-api-function.md). 
 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge
* [Two more things to keep your cost on track in DevTest Labs (Zwei weitere Möglichkeiten für die Kostenkontrolle in DevTest Labs)](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Why Cost Thresholds? (Gründe für Kostenschwellenwerte)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Nächste Schritte
Diese Schritte könnten Sie als Nächstes ausführen:

* [Definieren von Labrichtlinien](devtest-lab-set-lab-policy.md): Erfahren Sie, wie Sie die verschiedenen Richtlinien festlegen, mit denen Ihr Lab und die zugehörigen virtuellen Computer verwendet werden. 
* [Erstellen eines benutzerdefinierten Images](devtest-lab-create-template.md): Wenn Sie eine VM erstellen, geben Sie eine Basis an. Dabei kann es sich entweder um ein benutzerdefiniertes Image oder ein Marketplace-Image handeln. In diesem Artikel erfahren Sie, wie Sie ein benutzerdefiniertes Image aus einer VHD-Datei erstellen.
* [Konfigurieren von Marketplace-Images](devtest-lab-configure-marketplace-images.md): DevTest Labs unterstützt die Erstellung virtueller Computer auf der Basis von Azure Marketplace-Images. In diesem Artikel erfahren Sie, wie Sie ggf. angeben, welche Azure Marketplace-Images zum Erstellen virtueller Computer in einem Lab verwendet werden können.
* [Erstellen einer VM in einem Lab](devtest-lab-add-vm.md): In diesem Artikel wird veranschaulicht, wie Sie eine VM aus einem Basisimage erstellen (entweder aus einem benutzerdefinierten Image oder einem Marketplace-Image) und wie Sie mit Artefakten auf Ihrer VM arbeiten.

