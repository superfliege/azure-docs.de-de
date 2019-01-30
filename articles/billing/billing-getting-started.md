---
title: Vermeiden unerwarteter Kosten, Verwalten der Abrechnung in Azure | Microsoft-Dokumentation
description: Hier erwarten Sie, wie Sie unerwartete Gebühren in Ihrer Azure-Abrechnung vermeiden. Verwenden Sie Kostenüberwachungs- und Kostenverwaltungsfunktionen für ein Microsoft Azure-Abonnement.
services: ''
documentationcenter: ''
author: tonguyen10
manager: alherz
editor: ''
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2018
ms.author: cwatson
ms.openlocfilehash: 5aca80a4ebeadc9e54cf99fb4a220c6ee7c37cae
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857011"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Vermeiden unerwarteter Gebühren bei der Azure-Abrechnung und -Kostenverwaltung

Wenn Sie sich für Azure registrieren, sollten Sie sich einen Überblick über Ihre Ausgaben verschaffen. Der [Preisrechner](https://azure.microsoft.com/pricing/calculator/) kann eine Kostenschätzung bereitstellen, bevor Sie eine Azure-Ressource erstellen. Im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) erhalten Sie den aktuellen Kostenstrukturplan und die Prognose für Ihr Abonnement. Wenn Sie Kosten für unterschiedliche Projekte oder Teams zusammenfassen und verstehen möchten, beschäftigen Sie sich mit [Tags für Ressourcen](../azure-resource-manager/resource-group-using-tags.md). Falls Sie das Berichtssystem Ihrer Organisation verwenden möchten, informieren Sie sich über die [Abrechnung-APIs](billing-usage-rate-card-overview.md).

- Für Abonnements über Enterprise Agreement (EA) ist die öffentliche Vorschau zum Anzeigen Ihrer Kosten im Azure-Portal verfügbar. Einige der folgenden Features sind für Abonnements von CSP-Kunden (Cloud Solution Provider, Cloud-Lösungsanbieter) oder Azure Sponsorship-Kunden eventuell nicht relevant. Weitere Informationen finden Sie unter [Zusätzliche Ressourcen für EA, CSP und Sponsorship](#other-offers).

- Wenn Sie ein Abonnement für eine kostenlose Testversion, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure in Open (AIO) oder BizSpark besitzen, wird Ihr Abonnement automatisch deaktiviert, sobald Ihr gesamtes Guthaben verbraucht ist. Machen Sie sich mit [Ausgabenlimits](#spending-limit) vertraut, um zu vermeiden, dass Ihr Abonnement überraschend deaktiviert wird.

- Wenn Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) angemeldet haben, [können Sie einige der beliebtesten Azure-Dienste kostenlos für 12 Monate nutzen](billing-create-free-services-included-free-account.md). Weitere Informationen finden Sie zusammen mit den unten aufgelisteten Empfehlungen unter [Vermeiden Sie, dass Ihnen für Ihr kostenloses Azure-Konto Gebühren angezeigt werden](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Abrufen der geschätzten Kosten vor dem Hinzufügen von Azure-Diensten

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Schätzen der Kosten mithilfe des Onlinepreisrechners

Verschaffen Sie sich mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator/) einen ungefähren Eindruck von den monatlichen Kosten des Diensts, für den Sie sich interessieren. Sie können eine beliebige Erstanbieter-Azure-Ressource hinzufügen, um eine Kostenschätzung zu erhalten.

![Screenshot des Preisrechnermenüs](./media/billing-getting-started/pricing-calc.png)

So fallen beispielsweise für die Computestunden eines virtuellen A1-Windows-Computers voraussichtlich 66,96 USD/Monat an, wenn der Computer durchgehend in Betrieb ist:

![Screenshot des Preisrechners, der zeigt, dass für einen virtuellen A1-Windows-Computer pro Monat voraussichtlich Kosten in Höhe von 66,96 USD anfallen](./media/billing-getting-started/pricing-calcVM.png)

Weitere Informationen zu Preisen finden Sie unter diesen [häufig gestellten Fragen](https://azure.microsoft.com/pricing/faq/). Wenn Sie lieber mit einer für den Azure-Vertrieb zuständigen Person sprechen möchten, nutzen die Rufnummer 1-800-867-1389.

### <a name="review-the-estimated-cost-in-the-azure-portal"></a>Überprüfen der geschätzten Kosten im Azure-Portal

Wenn Sie im Azure-Portal einen Dienst hinzufügen, erscheint üblicherweise eine ähnliche Schätzung der monatlichen Kosten. Wenn Sie also etwa die Größe Ihres virtuellen Windows-Computers auswählen, werden die voraussichtlichen monatlichen Kosten für die Computestunden angezeigt:

![Beispiel: Die monatlichen Kosten für einen virtuellen A1-Windows-Computer belaufen sich voraussichtlich auf 66,96 USD.](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="spending-limit"></a>Überprüfen, ob ein Ausgabenlimit aktiviert ist

Bei Verwendung eines guthabenbasierten Abonnements wird das Ausgabenlimit standardmäßig für Sie aktiviert. Dadurch wird Ihre Kreditkarte nicht belastet, nachdem Sie Ihr gesamtes Guthaben verbraucht haben. Eine vollständige Liste mit Azure-Angeboten sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

Bei Erreichen Ihres Ausgabenlimits werden Ihre Dienste allerdings deaktiviert. Das bedeutet, dass die Zuordnung Ihrer virtuellen Computer aufgehoben wird. Um Ausfallzeiten zu vermeiden, müssen Sie das Ausgabenlimit deaktivieren. Überschreitungen werden über die hinterlegte Kreditkarte abgerechnet. 

Um zu ermitteln, ob ein Ausgabenlimit aktiviert ist, [navigieren Sie im Kontocenter zur Ansicht „Abonnements“](https://account.windowsazure.com/Subscriptions). Bei aktiviertem Ausgabenlimit wird ein Banner angezeigt:

![Screenshot mit einer Warnung aufgrund eines aktivierten Ausgabenlimits im Kontocenter](./media/billing-getting-started/spending-limit-banner.PNG)

Klicken Sie auf das Banner, und folgen Sie den Anweisungen, um das Ausgabenlimit zu entfernen. Falls Sie bei der Registrierung keine Kreditkarteninformationen eingegeben haben, müssen Sie dies nun nachholen, um das Ausgabenlimit entfernen zu können. Weitere Informationen finden Sie unter [Azure-Ausgabenlimit – Funktionsweise und Aktivierung/Deaktivierung](https://azure.microsoft.com/pricing/spending-limits/).

Sie können über den Dienst [Cloudyn](https://www.cloudyn.com/) Warnungsmeldungen erstellen, um die Projektbeteiligten automatisch über Ausgabenanomalien und Risiken zu hoher Ausgaben zu informieren. Sie können Warnungen auf der Grundlage von Berichten erstellen, die auf Budget- und Kostenschwellenwerten basierende Warnungen unterstützen. Weitere Informationen zur Verwendung von Cloudyn finden Sie hier: [Tutorial: Überprüfen der Nutzung und der Kosten](../cost-management/tutorial-review-usage.md).

In diesem Beispiel wird der Bericht **Actual Cost Over Time** (Ist-Kosten im Zeitverlauf) verwendet, um eine Benachrichtigung zu senden, wenn sich die Ausgaben für einen virtuellen Azure-Computer Ihrem Gesamtbudget annähern. In diesem Szenario beträgt das Gesamtbudget 20.000 USD, und Sie möchten eine Benachrichtigung erhalten, wenn die Kosten sich der Hälfte Ihres Budgets (9.000 USD) annähern, und eine Warnung, wenn die Kosten 10.000 USD erreichen.

1. Wählen Sie oben im Menü des Cloudyn-Portals Folgendes aus: **Costs** > **Cost Analysis** > **Actual Cost Over Time** („Kosten“ > „Kostenanalyse“ > „Ist-Kosten im Zeitverlauf“). 
2. Legen Sie **Groups** (Gruppen) auf **Service** (Dienst) und **Filter on the service** (Filtern nach Dienst) auf **Azure/VM** fest. 
3. Wählen Sie in der rechten oberen Ecke des Berichts **Actions** (Aktionen) und anschließend **Schedule report** (Bericht planen) aus.
4. Wählen Sie im Dialogfeld **Save or Schedule this report** (Diesen Bericht speichern oder planen) die Registerkarte **Scheduling** (Planung) aus, um im gewünschten Intervall eine E-Mail mit dem Bericht zu erhalten. Achten Sie darauf, dass Sie **Send via email** (Per E-Mail senden) auswählen. Alle verwendeten Tags, Gruppierungen und Filter werden in den per E-Mail gesendeten Bericht eingeschlossen. 
5. Wählen Sie die Registerkarte **Threshold** (Schwellenwert) und anschließend **Actual Cost vs. Threshold** (Tatsächliche Kosten im Vergleich zu Schwellenwert) aus. 
   1. Geben Sie im Schwellenwertfeld für die **rote Warnung** den Wert „10000“ ein. 
   2. Geben Sie im Schwellenwertfeld für die **gelbe Warnung** den Wert „9000“ ein. 
   3. Geben Sie im Feld **Number of consecutive alerts** (Anzahl der aufeinanderfolgenden Warnungen) ein, wie viele aufeinanderfolgende Warnungen sie erhalten möchten. Wenn Sie die festgelegte Gesamtzahl von Warnungen erhalten haben, werden keine weiteren Warnungen mehr gesendet. 
6. Wählen Sie **Save** (Speichern) aus.

    ![Beispiel für rote und gelbe Warnungen auf der Grundlage von Ausgabeschwellenwerten](./media/billing-getting-started/schedule-alert01.png)

Sie können auch die Schwellenwertmetrik **Cost Percentage vs. Budget** (Prozentuale Kosten im Vergleich zu Budget) verwenden, um Warnungen zu erstellen. In diesem Fall können Sie die Schwellenwerte als Prozentsatz Ihres Budgets (anstelle von Geldbeträgen) angeben.

## <a name="ways-to-monitor-your-costs-when-using-azure-services"></a>Möglichkeiten, Ihre Kosten bei Verwendung von Azure-Diensten zu überwachen

### <a name="tags"></a> Gruppieren von Abrechnungsdaten durch Hinzufügen von Tags zu Ressourcen

Mithilfe von Tags können Sie Abrechnungsdaten für unterstützte Dienste gruppieren. Wenn Sie also beispielsweise mehrere virtuelle Computer für unterschiedliche Teams verwenden, können Sie die Kosten mithilfe von Tags nach Kostenstelle (Personal, Marketing, Finanzen) oder Umgebung (Produktion, Präproduktion, Test) kategorisieren. 

![Screenshot zur Einrichtung von Tags im Portal](./media/billing-getting-started/tags.PNG)

Die Tags werden in den unterschiedlichen Kostenberichtsansichten angezeigt. So sind sie beispielsweise sofort in der [Kostenanalyseansicht](#costs) und nach dem ersten Abrechnungszeitraum in der CSV-Datei mit den Nutzungsdetails zu sehen.

Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Regelmäßiges Überprüfen der Kostenaufschlüsselung und Verbrauchsrate im Portal

Überprüfen Sie regelmäßig die Kosten, die für Ihre aktiven Dienste anfallen. Die aktuellen Ausgaben sowie die Verbrauchsrate werden im Azure-Portal angezeigt.

1. Öffnen Sie [„Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), und wählen Sie ein Abonnement aus.

2. Dort werden die Kostenaufschlüsselung und die Verbrauchsrate angezeigt (sofern dies für Ihr Abonnement unterstützt wird).

    ![Screenshot der Verbrauchsrate und der Kostenaufschlüsselung im Azure-Portal](./media/billing-getting-started/burn-rate.PNG)

3. Klicken Sie in der Liste auf der linken Seite auf **Kostenanalyse**, um die Kostenaufschlüsselung nach Ressource anzuzeigen. Warten Sie nach dem Hinzufügen eines Diensts 24 Stunden, damit die Daten aufgefüllt werden können.

    ![Screenshot der Kostenanalyseansicht im Azure-Portal](./media/billing-getting-started/cost-analysis.PNG)

4. Sie können nach verschiedenen Eigenschaften wie [Tags](#tags), Ressourcentyp, Ressourcengruppe und Zeitraum filtern. Klicken Sie auf **Übernehmen**, um die Filter zu bestätigen, und auf **Herunterladen**, um die Ansicht in eine CSV-Datei zu exportieren.

5. Darüber hinaus können Sie auf eine Ressource klicken, um Ihren täglichen Ausgabenverlauf und die täglichen Kosten der Ressource anzuzeigen.

    ![Screenshot der Ausgabenverlaufsansicht im Azure-Portal](./media/billing-getting-started/costhistory.PNG)

Es empfiehlt sich, die angezeigten Kosten mit den voraussichtlichen Kosten zu vergleichen, die beim Auswählen der Dienste angezeigt wurden. Sollten die Kosten stark von den Schätzungen abweichen, überprüfen Sie noch einmal den Tarif, den Sie für Ihre Ressourcen ausgewählt haben.

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Aktivieren von Kostensenkungsfeatures wie dem automatischen Herunterfahren für virtuelle Computer

Abhängig von Ihrem Szenario können Sie im Azure-Portal ggf. das automatische Herunterfahren für Ihre virtuellen Computer konfigurieren. Weitere Informationen finden Sie unter [Announcing auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Ankündigung des automatischen Herunterfahrens für virtuelle Computer mit Azure Resource Manager).

![Screenshot der Option für automatisches Herunterfahren im Portal](./media/billing-getting-started/auto-shutdown.PNG)

Das automatische Herunterfahren ist nicht dasselbe wie das Herunterfahren des virtuellen Computers über die Energieoptionen. Beim automatischen Herunterfahren werden Ihre virtuellen Computer beendet, und ihre Zuordnung wird aufgehoben, um zu verhindern, dass weitere Nutzungsgebühren anfallen. Weitere Informationen finden Sie in den häufig gestellten Fragen zu Preisen für [virtuelle Linux-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [virtuelle Windows-Computer](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Informationen zu weiteren Kostensenkungsfeatures für Ihre Entwicklungs- und Testumgebungen finden Sie unter [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Aktivieren und Überprüfen von Azure Advisor-Empfehlungen

[Azure Advisor](../advisor/advisor-overview.md) ist ein Feature, mit dem Sie die Kosten reduzieren können, indem wenig genutzte Ressourcen identifiziert werden. Navigieren Sie im Azure-Portal zum Advisor:

![Screenshot der Schaltfläche für Azure Advisor im Azure-Portal](./media/billing-getting-started/advisor-button.PNG)

Daraufhin stehen auf der Registerkarte **Kosten** des Advisor-Dashboards nützliche Empfehlungen zur Verfügung:

![Screenshot mit einem Beispiel für eine Advisor-Kostenempfehlung](./media/billing-getting-started/advisor-action.PNG)

Weitere Informationen finden Sie unter [Advisor-Empfehlungen zu Kosten](../advisor/advisor-cost-recommendations.md).

## <a name="reviewing-costs-at-the-end-of-your-billing-cycle"></a>Überprüfen der Kosten am Ende des Abrechnungszyklus

Nach dem Ende Ihres Abrechnungszyklus wird Ihre Rechnung zur Verfügung gestellt. Sie können auch [bisherige Rechnungen und detaillierte Verwendungsdateien herunterladen](billing-download-azure-invoice-daily-usage-date.md), um sicherzustellen, dass Sie richtige Rechnungen erhalten haben. Weitere Informationen für den Vergleich Ihrer täglichen Nutzung mit Ihrer Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>Abrechnungs-API

Über unsere Abrechnungs-API können Sie Nutzungsdaten programmgesteuert abrufen. Verwenden Sie die RateCard-API und die Usage-API, um Ihre abgerechnete Nutzung abzurufen. Weitere Informationen finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Zusätzliche Ressourcen und Sonderfälle

### <a name="ea-csp-and-sponsorship-customers"></a>EA-, CSP- und Sponsorship-Kunden
Wenden Sie sich an Ihren Kundenbetreuer oder Azure-Partner.

| Angebot | Ressourcen |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-Portal](https://ea.azure.com/), [Hilfedokumente](https://ea.azure.com/helpdocs) und [Power BI-Bericht](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Wenden Sie sich an Ihren Anbieter. |
| Azure Sponsorship | [Sponsorship-Portal](https://www.microsoftazuresponsorships.com/) |

Für IT-Manager großer Organisationen empfehlen wir den Artikel zum [Azure-Unternehmensgerüst](/azure/architecture/cloud-adoption-guide/subscription-governance) und das [Whitepaper zu Enterprise IT](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF-Download, nur auf Englisch verfügbar).

#### <a name="EA"></a> Vorschau von Enterprise Agreement-Kostenansichten im Azure-Portal 

Enterprise-Kostenansichten sind derzeit als öffentliche Vorschau verfügbar. Folgende Punkte sind zu beachten:

- Abonnementkosten basieren auf der Nutzung. Bereits gezahlte Beträge, Überschreitungen, enthaltene Mengen, Anpassungen und Steuern werden nicht berücksichtigt. Die tatsächlichen Gebühren werden auf Registrierungsebene berechnet.
- Die im Azure-Portal angezeigten Beträge weichen möglicherweise von den Angaben im Enterprise Portal ab. Nach einer Aktualisierung im Enterprise Portal dauert es unter Umständen ein paar Minuten, bis die Änderungen im Azure-Portal angezeigt werden.
- Sollten keine Kosten angezeigt werden, kann das auf eine der folgenden Ursachen zurückzuführen sein:
    - Sie verfügen über keine Berechtigungen auf Abonnementebene. Um Übersichten der Unternehmenskosten anzuzeigen, müssen Sie ein Abrechnungsleser, Leser, Mitwirkender oder Besitzer auf Abonnementebene sein.
    - Sie sind Kontobesitzer, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Kontobesitzer deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff auf die Kosten zu erhalten. 
    - Sie sind Abteilungsadministrator, und der Registrierungsadministrator hat die Einstellung „Gebühren anzeigen“ für Abteilungsadministratoren deaktiviert.  Wenden Sie sich an den Registrierungsadministrator, um Zugriff zu erhalten.
    - Sie haben Azure über einen Channelpartner gekauft, und der Partner hat die Preisinformationen nicht freigegeben.  
- Wenn Sie Einstellungen im Zusammenhang mit dem Kostenzugriff im Enterprise Portal aktualisieren, werden die Änderungen erst nach einigen Minuten im Azure-Portal angezeigt.
- Das Ausgabelimit und die Rechnungsanleitungen gelten nicht für EA-Abonnements.

### <a name="check-your-subscription-and-access"></a>Überprüfen von Abonnement und Zugriff

Zum Anzeigen von Kosten müssen Sie über [Zugriff auf Abrechnungsinformationen auf Abonnementebene](billing-manage-access.md) verfügen. Nur der Kontoadministrator kann auf das [Kontocenter](https://account.azure.com/Subscriptions) zugreifen, Abrechnungsinformationen ändern und Abonnements verwalten. Der Kontoadministrator ist die Person, die den Registrierungsprozess durchlaufen hat. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](billing-add-change-azure-subscription-administrator.md).

Um zu ermitteln, ob Sie der Kontoadministrator sind, navigieren Sie [im Azure-Portal zu „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Sehen Sie sich die Liste mit den Abonnements an, auf die Sie Zugriff haben. Ausschlaggebend ist die Angabe unter **Meine Rolle**. Wenn hier *Kontoadministrator* angegeben ist, ist alles in Ordnung. Bei einer Angabe wie *Besitzer* verfügen Sie nicht über sämtliche Berechtigungen.

![Screenshot Ihrer Rolle in der Ansicht „Abonnements“ im Azure-Portal](./media/billing-getting-started/sub-blade-view.PNG)

Falls Sie nicht der Kontoadministrator sind, wurde Ihnen wahrscheinlich von einer anderen Person über die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) in Azure Active Directory](../role-based-access-control/role-assignments-portal.md) eingeschränkter Zugriff gewährt. Um Abonnements verwalten und Abrechnungsinformationen ändern zu können, müssen Sie [den Kontoadministrator ermitteln](billing-subscription-transfer.md#whoisaa). Bitten Sie den Kontoadministrator, die Aufgaben für Sie auszuführen oder [das Abonnement auf Sie zu übertragen](billing-subscription-transfer.md).

Falls der Kontoadministrator Ihre Organisation verlassen hat und Sie die Abrechnung verwalten müssen, [wenden Sie sich an uns](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
