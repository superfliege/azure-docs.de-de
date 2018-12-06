---
title: Informationen zu Ihrer Rechnung für Azure Enterprise | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Nutzung und Abrechnung für Azure-Kunden mit einem Enterprise Agreement lesen und verstehen.
services: ''
documentationcenter: ''
author: adpick
manager: dougeby
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: cwatson
ms.openlocfilehash: b724fc7a887550b4115a988149b4b7a6c95de830
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584467"
---
# <a name="understand-your-bill-for-azure-customers-with-an-enterprise-agreement"></a>Erläuterungen zur Rechnung für Azure-Kunden mit Enterprise Agreement

Azure-Kunden mit Enterprise Agreement erhalten eine Rechnung, wenn sie das Guthaben der Organisation überschreiten oder Dienste nutzen, die nicht durch das Guthaben abgedeckt sind. 

Das Guthaben Ihrer Organisation schließt Ihre finanzielle Verpflichtung ein. Der finanzielle Verpflichtung ist der Betrag, den Ihre Organisation für die Nutzung von Azure-Diensten im Voraus bezahlt hat. Sie können Ihrem Enterprise Agreement ein Guthaben für finanzielle Verpflichtungen hinzufügen, indem Sie sich an Ihren Microsoft-Kundenbetreuer oder -Reseller wenden.  

## <a name="when-credit-exceeded-or-doesnt-apply"></a>Gründe für überschrittene oder nicht zutreffende Guthaben

Sie erhalten in den folgenden Fällen eine oder mehrere Rechnungen:

- **Dienstüberschreitung:** Die Kosten für die Nutzung durch Ihre Organisation überschreiten Ihr Guthaben.
- **Separat abgerechnete Gebühren:** Die von Ihrer Organisation genutzten Dienste sind nicht durch das Guthaben abgedeckt. Die folgenden Dienste werden Ihnen unabhängig von Ihrem Guthaben in Rechnung gestellt:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop 
    - Registrierter Benutzer
    - OpenLogic
    - Registrierter Benutzer von Remote Access Rights XenApp Essentials
    - Ubuntu Advantage
    - Visual Studio Enterprise (monatlich)
    - Visual Studio Enterprise (jährlich)
    - Visual Studio Professional (monatlich)
    - Visual Studio Professional (jährlich)
- **Marketplace-Gebühren:** Käufe und Nutzung im Azure Marketplace sind nicht vom Guthaben Ihrer Organisation abgedeckt und werden separat in Rechnung gestellt. Der Unternehmensadministrator kann Marketplace-Einkäufe für seine Organisation im Enterprise Portal aktivieren und deaktivieren. 

Wenn sowohl Gebühren für Dienstüberschreitung als auch separat während des Abrechnungszeitraums abgerechnete Gebühren fällig sind, erhalten Sie eine Rechnung mit beiden Arten von Gebühren. Marketplace-Gebühren werden immer separat abgerechnet.

## <a name="review-charges"></a>Anzeigen von Gebühren

Um die Gebühren in Ihrer Rechnung anzuzeigen und zu überprüfen, müssen Sie ein Unternehmensadministrator sein. Weitere Informationen finden Sie unter [Informationen zu Azure Enterprise Agreement-Administratorrollen in Azure](billing-understand-ea-roles.md). Wenn Sie nicht wissen, wer der Unternehmensadministrator für Ihre Organisation ist, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="review-service-overage-invoice"></a>Überprüfen der Rechnung für Dienstüberschreitung

Vergleichen Sie den Gesamtnutzungsbetrag im Enterprise-Portal unter **Berichte** > **Nutzungszusammenfassung** mit Ihrer Rechnung für Dienstüberschreitung. Die Rechnung für Dienstüberschreitung umfasst die Nutzung, die das Guthaben Ihrer Organisation überschreitet, sowie Dienste, die nicht durch das Guthaben abgedeckt sind. Die Beträge der **Nutzungszusammenfassung** schließen keine Steuern ein. 

1. Melden Sie sich beim [Enterprise-Portal](https://ea.azure.com) an.
1. Wählen Sie **Berichte** aus.
1. Wechseln Sie in der rechten oberen Ecke der Registerkarte die Ansicht von **M** zu **C**, und passen Sie den Zeitraum entsprechend der Rechnung an.
 
   ![Screenshot mit den Optionen „M“ und „C“ unter „Nutzungszusammenfassung“.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. Der Betrag unter **Nutzung gesamt** sollte mit dem Betrag **Total Extended Amount** (Erweiterter Gesamtbetrag) in Ihrer Rechnung für Dienstüberschreitung übereinstimmen. In der folgenden Tabelle werden die Begriffe und Beschreibungen auf der Rechnung und unter **Nutzungszusammenfassung** im Enterprise-Portal aufgeführt:

   |Begriff in Rechnung|Begriff in Nutzungszusammenfassung|BESCHREIBUNG|
   |---|---|---|
   |Total Extended Amount (Erweiterter Gesamtbetrag)|Nutzung gesamt|Die Gebühr für Gesamtnutzung vor Steuern für den jeweiligen Zeitraum vor dem Anwenden des Guthabens.|
   |Commitment Usage (Nutzung der Mindestabnahme)|Commitment Usage (Nutzung der Mindestabnahme)|Das angewandte Guthaben für diesen spezifischen Zeitraum.|
   |Verkauf gesamt|Überschreitung gesamt|Die Gebühr für Gesamtnutzung, die Ihren Guthabenbetrag überschreitet. Dieser Betrag schließt keine Steuern ein.|
   |Steuerbetrag|Nicht zutreffend|Steuern für den gesamten Umsatz für den jeweiligen Zeitraum.|
   |Gesamtbetrag|Nicht zutreffend|Der für die Rechnung fällige Betrag nach Anwendung des Guthabens und Hinzufügung der Steuern.|
1. Weitere Informationen zu Ihren Gebühren erhalten Sie unter **Nutzung herunterladen** > **Advanced Report Download** (Erweiterten Bericht herunterladen). Dieser Bericht enthält keine Steuern, keine Gebühren für Reservierungen und keine Marketplace-Gebühren.

      ![Screenshot von „Advanced report Download“ (Erweiterten Bericht herunterladen) auf der Registerkarte „Nutzung herunterladen“](./media/billing-understand-your-bill-ea/ea-portal-download-usage-advanced.png)

### <a name="review-marketplace-invoice"></a>Überprüfen der Marketplace-Rechnung

Vergleichen Sie Ihren Azure Marketplace-Gesamtbetrag unter **Berichte** > **Nutzungszusammenfassung** im Enterprise-Portal mit Ihrer Marketplace-Rechnung. Die Marketplace-Rechnung enthält nur Azure Marketplace-Käufe und -Nutzung. Die Beträge der **Nutzungszusammenfassung** schließen keine Steuern ein. 

1. Melden Sie sich beim [Enterprise-Portal](https://ea.azure.com) an.
1. Wählen Sie **Berichte** aus.
1. Wechseln Sie in der rechten oberen Ecke der Registerkarte die Ansicht von **M** zu **C**, und passen Sie den Zeitraum entsprechend der Rechnung an.

     ![Screenshot mit den Optionen „M“ und „C“ unter „Nutzungszusammenfassung“.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. Der Gesamtbetrag **Azure Marketplace** sollte mit dem Betrag **Verkauf gesamt** in Ihrer Marketplace-Rechnung übereinstimmen.
1. Weitere Informationen zu Ihren nutzungsbasierten Gebühren erhalten Sie unter **Nutzung herunterladen**. Wählen Sie unter **Marketplace-Gebühren** die Option **Herunterladen** aus. Dieser Bericht schließt keine Steuern ein und enthält keine einmaligen Käufe.

     ![Screenshot der Option „Herunterladen“ unter „Marketplace-Gebühren“.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-marketplace.png)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
