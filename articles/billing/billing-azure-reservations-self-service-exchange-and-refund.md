---
title: Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie für Azure-Reservierungen einen Umtausch durchführen oder Rückerstattungen erhalten können.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: banders
ms.openlocfilehash: 229b5a0948a81c9b5d78dd49f304e7b6891f95ee
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652854"
---
# <a name="self-service-exchanges-and-refunds-for-azure-reservations"></a>Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen

Mit Azure-Reservierungen können Sie Ihre sich ändernden Anforderungen flexibel erfüllen. Sie können eine Reservierung gegen eine andere Reservierung des gleichen Typs umtauschen. Sie können auch eine Rückerstattung für eine Reservierung erhalten, wenn Sie sie nicht mehr benötigen (bis zu 50.000 US-Dollar pro Jahr).

## <a name="exchange-an-existing-reserved-instance"></a>Umtauschen einer vorhandenen reservierten Instanz

Sie können Ihre Reservierung mit drei schnellen Schritten im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) umtauschen.

1. Wählen Sie die Reservierungen aus, die Sie umtauschen möchten, und klicken Sie auf **Umtausch**.  
    ![Beispielbild mit Reservierungen, die zurückgegeben werden sollen](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-return.png)
2. Wählen Sie das VM-Produkt aus, das Sie erwerben möchten, und geben Sie eine Menge ein. Achten Sie darauf, dass der neue Gesamtbetrag des Kaufs höher als der Umtauschwert ist. [Bestimmen der richtigen Größe vor dem Kauf](../virtual-machines/windows/prepay-reserved-vm-instances.md#determine-the-right-vm-size-before-you-buy)  
    ![Beispielbild mit dem VM-Produkt, das per Umtausch erworben werden soll](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-select-purchase.png)
3. Überprüfen Sie die Transaktion, und schließen Sie sie ab.  
    ![Beispielbild mit dem VM-Produkt, das per Umtausch erworben werden soll, und Abschluss des Vorgangs ](./media/billing-azure-reservations-self-service-exchange-and-refund/exchange-refund-confirm-exchange.png)

Navigieren Sie zum Erhalten einer Erstattung für eine Reservierung zu **Reservierungsdetails**, und klicken Sie auf **Erstattung**.

## <a name="how-return-and-exchange-transactions-are-processed"></a>Verarbeiten von Transaktionen für Rückgabe und Umtausch

Zunächst storniert Microsoft die vorhandene Reservierung und erstattet den entsprechenden anteiligen Betrag. Bei einem Umtausch wird der neue Kauf verarbeitet. Microsoft nutzt für die Verarbeitung von Erstattungen eine der folgenden Methoden. Dies richtet sich nach Ihrem Kontotyp und der Zahlungsmethode:

### <a name="enterprise-agreement-customers"></a>Enterprise Agreement-Kunden

Ein Geldbetrag wird der finanziellen Verpflichtung für Umtausch und Rückerstattungen hinzugefügt, wenn dies Teil des ursprünglichen Kaufs war. Alle Überschreitungsrechnungen seit den ursprünglichen Käufen werden erneut geöffnet und neu bewertet, um sicherzustellen, dass die finanzielle Verpflichtung verwendet wird. Falls der Zeitraum der finanziellen Verpflichtung für die Nutzung der erworbenen Reservierung nicht mehr aktiv ist, wird das Guthaben dem Zeitraum Ihrer finanziellen Verpflichtung unter dem derzeitigen Enterprise Agreement hinzugefügt.

Falls der ursprüngliche Kauf als Überschreitung durchgeführt wurde, stellt Microsoft eine Gutschrift aus.

### <a name="pay-as-you-go-invoice-payment-customers-and-cloud-solution-provider-program"></a>Kunden mit nutzungsbasierter Bezahlung von Rechnungen und Cloud Solution Provider-Programm

Die ursprüngliche Rechnung für den Kauf der Reservierung wird storniert, und anschließend wird eine neue Rechnung für die Erstattung erstellt. Bei einem Umtausch werden für die neue Rechnung die Erstattung und der neue Kauf angezeigt. Der Erstattungsbetrag wird anhand des Kaufbetrags angepasst. Falls Sie nur eine Erstattung für eine Reservierung durchgeführt haben, verbleibt der anteilige Betrag bei Microsoft, und die Anpassung wird bei einem zukünftigen Kauf einer Reservierung vorgenommen.

### <a name="pay-as-you-go-credit-card-customers"></a>Kreditkartenkunden mit nutzungsbasierter Bezahlung

Die ursprüngliche Rechnung wird storniert, und eine neue Rechnung wird erstellt. Der Betrag wird über die Kreditkarte erstattet, die für den ursprünglichen Kauf genutzt wurde. [Wenden Sie sich an den Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest), falls sich Ihre Kartenangaben geändert haben.

## <a name="exchange-policies"></a>Umtauschrichtlinien

- Sie können mehrere vorhandene Reservierungen zurückgeben, um eine neue Reservierung des gleichen Typs zu erwerben. Ein Wechsel des Typs von Reservierungen ist beim Umtausch nicht möglich. Beispielsweise können Sie keine VM-Reservierung zurückgeben, um eine SQL-Reservierung zu erwerben.
- Nur Besitzer von Reservierungen können einen Umtausch verarbeiten. [Informieren Sie sich über das Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance#add-or-change-users-who-can-manage-a-reservation).
- Ein Umtausch wird als Erstattung und erneuter Kauf verarbeitet. Für die Stornierung und den neuen Kauf werden separate Transaktionen erstellt. Der anteilige Reservierungsbetrag für die zurückgegebenen Reservierungen wird erstattet. Der neue Kauf wird Ihnen vollständig berechnet. Der anteilige Reservierungsbetrag ist der tägliche anteilige Restwert der zurückgegebenen Reservierung.
- Sie können Reservierungen auch dann umtauschen oder zurückgeben, wenn das für den Kauf der Reservierung verwendete Enterprise Agreement abgelaufen ist und in Form eines neuen Vertrags verlängert wurde.
- Sie können alle Reservierungseigenschaften ändern, z. B. Größe, Region, Menge und Zeitraum eines Umtauschs.
- Der neue Gesamtbetrag des Kaufs sollte größer oder gleich dem Rückgabebetrag sein.
- Für die neue Reservierung, die im Rahmen des Umtauschs erworben wird, gilt ein neuer Zeitraum, der ab dem Umtauschzeitpunkt beginnt.
- Für den Umtausch gibt es keine Strafzahlungen oder jährlichen Grenzwerte.

## <a name="refund-policies"></a>Erstattungsrichtlinien

- Ihr Gesamtbetrag für Erstattungen darf innerhalb eines rollierenden Zeitfensters von 12 Monaten den Wert von 50.000 US-Dollar nicht überschreiten.
- Nur Besitzer von Reservierungen können eine Erstattung verarbeiten. [Informieren Sie sich über das Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
- Microsoft behält sich das Recht vor, für alle Rückgaben eine Strafzahlung in Höhe von 12 % zu berechnen. Derzeit wird diese Strafzahlung aber nicht angewendet.

## <a name="exchange-a-non-premium-storage-vm-reservation-for-a-premium-storage-reservation"></a>Umtauschen einer VM-Reservierung ohne Storage Premium gegen eine Reservierung mit Storage Premium

Sie können eine erworbene Reservierung für eine VM-Größe, für die Storage Premium nicht unterstützt wird, gegen eine entsprechende VM-Größe umtauschen, für die diese Unterstützung vorhanden ist. Beispiel: _F1_ gegen _F1s_. Navigieren Sie zum Durchführen des Umtauschs zu „Reservierungsdetails“, und klicken Sie auf **Umtausch**. Beim Umtausch wird der Zeitraum der reservierten Instanz nicht zurückgesetzt und auch keine neue Transaktion erstellt.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md).
- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
    - [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
    - [Verwalten von Reservierungen für Ressourcen in Azure](billing-manage-reserved-vm-instance.md)
    - [Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](billing-understand-vm-reservation-charges.md)
    - [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
    - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
    - [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)
    - [Verkaufen Microsoft Azure Reserved Instances](/partner-center/azure-reservations)
