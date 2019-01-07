---
title: Herunterladen von Azure-Rechnungen und täglichen Nutzungsdaten | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure-Rechnungen und tägliche Nutzungsdaten herunterladen oder ansehen können.
keywords: Rechnung, Rechnungsdownload, Azure-Rechnung, Azure-Nutzung
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/16/2018
ms.author: cwatson
ms.openlocfilehash: fe3cb2d15602407b54ff47763d409c2e77614f0a
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311132"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Herunterladen oder Anzeigen Ihrer Azure-Rechnungen und täglichen Nutzungsdaten

Für die meisten Abonnements können Sie die Rechnung aus dem [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) herunterladen oder per E-Mail zugeschickt bekommen. Wenn Sie Azure-Kunde mit einem Enterprise Agreement (EA-Kunde) sind, können Sie die Rechnungen Ihrer Organisation nicht herunterladen. Rechnungen werden an den Benutzer gesendet, der für den Empfang von Rechnungen für die Registrierung festgelegt wurde.

Wenn Sie als EA-Kunde Nutzungsdaten herunterladen möchten, rufen Sie Folgendes auf: [Azure-Portal](https://portal.azure.com/) > **Kostenverwaltung + Abrechnung** > **Verbrauch + Gebühren**. Alle anderen Abonnements finden Sie im [Azure-Kontocenter](https://account.azure.com/Subscriptions).

Nur bestimmte Rollen, z.B. der Konto- oder Unternehmensadministrator, verfügen über die Berechtigung zum Abrufen von Abrechnungs- und Nutzungsinformationen. Weitere Informationen zum Zugriff auf Abrechnungsinformationen finden Sie unter [Verwalten des Zugriffs auf die Azure-Abrechnung mithilfe von Rollen](billing-manage-access.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-or-view-your-invoice"></a>Herunterladen oder Anzeigen von Rechnungen

 Wenn Sie EA-Kunde sind, können Sie die Rechnungen Ihrer Organisation nicht herunterladen. Rechnungen werden an den Benutzer gesendet, der für den Empfang von Rechnungen für die Registrierung festgelegt wurde. Für andere Abonnements können Sie Ihre Rechnung per E-Mail erhalten oder im Azure-Portal herunterladen.

### <a name="get-your-invoice-in-email-pdf"></a>Empfangen Ihrer Rechnung per E-Mail (PDF)
Sie können die entsprechende Option aktivieren und zusätzliche Empfänger konfigurieren, die Ihre Azure-Rechnung in einer E-Mail erhalten. Diese Funktion ist für bestimmte Abonnements möglicherweise nicht verfügbar, z.B. für Supportangebote, Enterprise Agreements oder Azure in Open.

1. Wählen Sie auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus. Wählen Sie jedes Abonnement aus, das Sie besitzen. Klicken Sie auf **Rechnungen** und dann auf **Rechnung per E-Mail**. 

    ![Screenshot mit dem Ablauf der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Klicken Sie auf **Aktivieren**, und stimmen Sie den Bedingungen zu.

    ![Screenshot mit dem Ablauf von Schritt 2 der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Nachdem Sie die Vereinbarung akzeptiert haben, können Sie zusätzliche Empfänger konfigurieren. Wenn ein Empfänger entfernt wird, wird die E-Mail-Adresse nicht mehr gespeichert. Wenn Sie Ihre Meinung ändern, müssen Sie ihn erneut hinzufügen.

    ![Screenshot mit dem Ablauf von Schritt 3 der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Wenn Sie keine E-Mail erhalten, nachdem Sie diese Schritte befolgt haben, stellen Sie sicher, dass Ihre E-Mail-Adresse in den [Kommunikationseinstellungen auf Ihrem Profil](https://account.windowsazure.com/profile) richtig ist.

### <a name="opt-out-from-getting-your-invoice-in-email"></a>Kündigen des Erhalts von Rechnungen per E-Mail
Wenn Sie Ihre Rechnung nicht per E-Mail erhalten möchten, klicken Sie auf **Abonnement für Rechnungen per E-Mail kündigen**. Dadurch werden alle E-Mail-Adressen entfernt, an die Rechnungen gesendet werden sollten. Wenn Sie den Empfang wieder aktivieren möchten, müssen Sie die Empfänger erneut konfigurieren.

 ![Screenshot mit dem Ablauf der Kündigung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="download-invoice-from-azure-portal-pdf"></a>Herunterladen der Rechnung aus dem Azure-Portal (PDF)

1. Wählen Sie im Azure-Portal auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement als [Benutzer mit Zugriff auf Rechnungen](billing-manage-access.md) aus.

2. Wählen Sie **Rechnungen** aus. 

    ![Screenshot mit der Option „Abrechnung und Nutzung“](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Klicken Sie auf **Rechnung herunterladen**, um eine Kopie Ihrer PDF-Rechnung anzuzeigen. Falls **Nicht verfügbar** angezeigt wird, siehe [Warum sehe ich keine Rechnung für den letzten Abrechnungszeitraum?](#noinvoice)

    ![Screenshot mit Abrechnungszeiträumen, der Option „Herunterladen“ und den Gesamtgebühren für jeden Abrechnungszeitraum](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Sie können Ihre Daten zur tägliche Nutzung auch durch Klicken auf den Abrechnungszeitraum anzeigen. 

Weitere Informationen über Ihre Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md). Hilfe beim Verwalten Ihrer Kosten finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](billing-getting-started.md).

### <a name="noinvoice"></a> Warum sehe ich keine Rechnung für den letzten Abrechnungszeitraum?

Mehrere Gründe können dafür ausschlaggebend sein, dass Sie keine Rechnung sehen:

- Sie haben einen monatlichen Guthabenbetrag in Ihrem Abonnement, den Sie nicht überschritten haben, oder eine kostenlose Testversion. Eine Rechnung wird nur generiert, wenn Sie Geld schuldig sind.

- Es sind weniger als 30 Tage seit dem Abschluss Ihres Azure-Abonnements vergangen.

- Die Rechnung ist nicht noch erstellt. Warten Sie bis zum Ende des Abrechnungszeitraums.

- Wenn Sie nicht der Kontoadministrator sind, stehen Ihnen ältere Rechnungen möglicherweise nicht zur Verfügung.

## <a name="download-usage"></a>Herunterladen von Nutzungsdaten

 Für die meisten Abonnements finden Sie Ihre tägliche Nutzungsdatei im [Azure-Kontocenter](https://account.azure.com/Subscriptions). Wenn Sie als EA-Kunde Nutzungsdaten herunterladen möchten, rufen Sie Folgendes auf: [Azure-Portal](https://portal.azure.com/) > **Kostenverwaltung + Abrechnung** > **Verbrauch + Gebühren**. 

### <a name="download-usage-from-the-account-center-csv"></a>Herunterladen der Nutzungsdaten aus dem Kontocenter (CSV)

1. Melden Sie sich als Kontoadministrator im [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) an.

2. Wählen Sie das Abonnement, für das Sie die Rechnung und Nutzungsinformationen abrufen möchten.

3. Klicken Sie auf **ABRECHNUNGSVERLAUF**. 

    ![Screenshot mit Abrechnungsverlaufsoption](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Sie können Ihre Aufstellungen für die letzten sechs Abrechnungszeiträume sowie den aktuellen, nicht berechneten Zeitraum anzeigen. 

    ![Screenshot, der Abrechnungszeiträume, Optionen zum Herunterladen von Rechnungen und Daten zur täglichen Nutzung sowie Gesamtgebühren für jeden Abrechnungszeitraum anzeigt](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Wählen Sie **Aktuelle Abrechnung anzeigen** , um eine Schätzung der Kosten zum Zeitpunkt der Erstellung der Schätzung anzuzeigen. Diese Informationen werden nur einmal täglich aktualisiert und umfassen möglicherweise nicht die gesamte Nutzung. Ihre monatliche Abrechnung kann von dieser Schätzung abweichen.

    ![Screenshot mit der Option „Aktuelle Erklärung anzeigen“](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Screenshot mit der Schätzung der aktuellen Gebühren](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Wählen Sie **Nutzung herunterladen**, um die täglichen Nutzungsdaten als CSV-Datei herunterzuladen. Wenn zwei Versionen verfügbar sind, laden Sie Version 2 herunter.

    ![Screenshot mit der Option „Nutzung herunterladen“](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Nur der Kontoadministrator hat Zugriff auf das Azure-Kontocenter. Andere Abrechnungsadministratoren, z.B. Besitzer, erhalten mithilfe der [Abrechnungs-APIs](billing-usage-rate-card-overview.md) Informationen zur Nutzung.

Weitere Informationen über Ihre Daten zur täglichen Nutzung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md). Hilfe beim Verwalten Ihrer Kosten finden Sie unter [Vermeiden unerwarteter Kosten bei der Azure-Abrechnung und -Kostenverwaltung](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>Herunterladen von Nutzungsdaten für EA-Kunden

Um Nutzungsdaten als EA-Kunde anzeigen und herunterladen zu können, müssen Sie Unternehmensadministrator, Kontobesitzer oder Abteilungsadministrator sein. Außerdem muss die Richtlinie zum Anzeigen von Gebühren aktiviert sein.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Wählen Sie **Verbrauch + Gebühren** aus.
1. Wählen Sie für den Monat, den Sie herunterladen möchten, **Herunterladen** aus.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
