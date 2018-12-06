---
title: Vorauszahlen von virtuellen Kernen für Azure SQL-Datenbank zum Einsparen von Kosten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie reservierte Kapazitäten für Azure SQL-Datenbank kaufen, um Computekosten einzusparen.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 05c237eb071b48f2373ecfd78eeab6f7bceb8c5c
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584157"
---
# <a name="prepay-for-sql-database-compute-resources-with-azure-sql-database-reserved-capacity"></a>Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität

Sparen Sie mit Azure SQL-Datenbank Geld im Vergleich zur nutzungsbasierten Bezahlung, indem Sie im Voraus Azure SQL-Datenbank-Computeressourcen bezahlen. Mit reservierten Azure SQL-Datenbank-Kapazitäten leisten Sie eine Vorauszahlung für SQL-Datenbank für einen Zeitraum von einem oder drei Jahren und profitieren dafür von einem immensen Rabatt auf Computekosten. Um reservierte SQL-Datenbank-Kapazität zu erwerben, müssen Sie die Azure-Region, den Bereitstellungstyp, die Leistungsstufe und die Laufzeit angeben. 

Eine Zuweisung der Reservierung zu SQL-Datenbank-Instanzen ist nicht erforderlich. Betreffende SQL-Datenbank-Instanzen, die bereits ausgeführt oder neu bereitgestellt werden, profitieren automatisch von dem Vorteil. Beim Kauf einer Reservierung bezahlen Sie im Voraus die Computekosten für SQL-Datenbank-Instanzen für einen Zeitraum von einem oder drei Jahren. Sobald Sie eine Reservierung gekauft haben, werden die SQL-Datenbank-Computegebühren, die den Reservierungsattributen entsprechen, nicht mehr zu den Preisen der nutzungsbasierten Bezahlung abgerechnet. Eine Reservierung deckt nicht die Software-, Netzwerk- oder Speichergebühren für die SQL-Datenbank-Instanz ab. Nach Ablauf der Reservierungslaufzeit erlischt der Abrechnungsvorteil, und die SQL-Datenbank-Instanzen werden mit den Preisen für die nutzungsbasierte Bezahlung in Rechnung gestellt. Reservierungen werden nicht automatisch verlängert. Weitere Informationen zu den Preisen finden Sie unter [Azure SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/managed/).

Sie können die reservierte Azure SQL-Datenbank-Kapazität über das [Azure-Portal](https://portal.azure.com) erwerben. So erwerben Sie eine reservierte SQL-Datenbank-Kapazität
- Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Bezahlung zugeordnet sein.
- In Enterprise-Abonnements müssen Azure-Reservierungskäufe im [EA-Portal](https://ea.azure.com) aktiviert werden.
-  Für das Cloud Solution Provider-Programm (CSP) können nur die Administrator- oder Vertriebs-Agents reservierte SQL-Datenbank-Kapazitäten kaufen.

Einzelheiten zur Berechnung der Reservierung von Kapazitäten für Unternehmenskunden und Kunden mit nutzungsbasierter Bezahlung finden Sie unter [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../billing/billing-understand-reserved-instance-usage-ea.md) und [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../billing/billing-understand-reserved-instance-usage.md).

## <a name="determine-the-right-sql-size-before-purchase"></a>Bestimmen der passenden SQL-Datenbank-Instanzgröße vor dem Kauf

Die Größe der Reservierung sollte auf der Gesamtmenge der Computeressourcen basieren, die von den Einzeldatenbanken und/oder Pools für elastische Datenbanken in SQL-Datenbank, die bereits vorhanden sind oder in Kürze bereitgestellt werden, innerhalb einer bestimmten Region und unter Verwendung der gleichen Leistungsstufe und der gleichen Hardwaregeneration verwendet werden. 

Nehmen Sie beispielsweise an, Sie führen einen Pool für elastische Gen5-Datenbanken mit der SKU „Universell“ und 16 virtuellen Kernen sowie zwei Gen5-Einzeldatenbanken mit der SKU „Unternehmenskritisch“ und 4 virtuellen Kernen aus. Sie möchten innerhalb des nächsten Monats einen weiteren Pool für elastische Gen5-Datenbanken mit der SKU „Universell“ und 16 virtuellen Kernen sowie einen Pool für elastische Gen5-Datenbanken mit der SKU „Unternehmenskritisch“ und 32 virtuellen Kernen bereitstellen. Darüber hinaus wissen Sie, dass Sie diese Ressourcen mindestens 1 Jahr benötigen. In diesem Fall sollten Sie Folgendes erwerben: eine 1-Jahres-Reservierung für Einzeldatenbanken oder Pools für elastische Datenbanken in SQL-Datenbank mit der SKU „Universell“ (Compute Gen5) und 32 virtuellen Kernen (2 x 16) sowie eine 1-Jahres-Reservierung für Einzeldatenbanken bzw. Pools für elastische Datenbanken in SQL-Datenbank (Compute Gen5) mit der SKU „Unternehmenskritisch“ und 40 virtuellen Kernen (2 x 4 + 32).

## <a name="buy-sql-database-reserved-capacity"></a>Kaufen einer reservierten SQL-Datenbank-Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie **Hinzufügen** und dann im Bereich zur Auswahl des Produkttyps **SQL-Datenbank** aus, um eine neue Reservierung für SQL-Datenbank zu erwerben.
4. Füllen Sie die erforderlichen Felder aus. Bei vorhandenen oder neuen Einzeldatenbanken oder Pools für elastische Datenbanken, die den von Ihnen ausgewählten Attributen entsprechen, wird der Rabatt auf reservierte Kapazitäten angewendet. Die tatsächliche Anzahl Ihrer SQL-Datenbank-Instanzen, die den Rabatt erhalten, hängt vom ausgewählten Bereich und der ausgewählten Menge ab.

   ![Screenshot vor der Übermittlung des Kaufs der reservierten SQL-Datenbank-Kapazität](./media/sql-database-reserved-vcores/sql-reserved-vcores-purchase.png)

    | Feld      | BESCHREIBUNG|
    |:------------|:--------------|
    |NAME        |Der Name dieser Reservierung| 
    |Abonnement|Das Abonnement, das für die Zahlung der Reservierung von SQL-Datenbank-Kapazitäten verwendet wird. Die Zahlungsmethode für das Abonnement wird mit Vorauszahlungen für die Reservierung von SQL-Datenbank-Kapazitäten belastet. Als Abonnementtyp muss „Enterprise Agreement“ (Angebotsnummer: MS-AZR-0017P) oder „Nutzungsbasierte Zahlung“ (Angebotsnummer: MS-AZR-0003P) festgelegt werden. Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.|    
    |Bereich       |Der Umfang der Reservierung virtueller Kerne kann ein Abonnement oder mehrere Abonnements (freigegebener Bereich) umfassen. Optionen: <ul><li>Einzelabonnement: Der Rabatt auf die Reservierung virtueller Kerne wird auf SQL-Datenbank-Instanzen in diesem Abonnement angewendet. </li><li>Gemeinsam: Der Rabatt auf die Reservierung virtueller Kerne wird auf SQL-Datenbank-Instanzen angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Für Enterprise-Kunden stellt der freigegebene Bereich die Reservierung dar und umfasst alle Abonnements (mit Ausnahme von Dev/Test-Abonnements) innerhalb der Reservierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.</li></ul>|
    |Region      |Die Azure-Region, die durch die Reservierung von SQL-Datenbank-Kapazitäten abgedeckt wird.|    
    |Bereitstellungstyp|Der SQL-Ressourcentyp, für den Sie die Reservierung erwerben möchten.|
    |Leistungsstufe|Der Diensttarif für die SQL-Datenbank-Instanzen.
    |Begriff        |Ein Jahr oder drei Jahre|
    |Menge    |Die Anzahl von Instanzen, die innerhalb der Reservierung von SQL-Datenbank-Kapazitäten erworben werden. Die Menge ist die Anzahl der ausgeführten SQL-Datenbank-Instanzen, auf die der Abrechnungsrabatt angewendet werden kann. Beispiel: Wenn Sie zehn SQL-Datenbank-Instanzen in der Region „USA, Osten“ ausführen, geben Sie als Menge 10 an, um den Vorteil für alle ausgeführten Computer zu maximieren. |

5. Überprüfen Sie die Kosten für die Reservierung von SQL-Datenbank-Kapazitäten im Abschnitt **Kosten**.
6. Wählen Sie die Option **Kaufen**.
7. Klicken Sie auf **Diese Reservierung anzeigen**, um den Status des Einkaufs anzuzeigen.

## <a name="cancellations-and-exchanges"></a>Stornierungen und Umtausch

Wenn Sie Ihre Reservierung von SQL-Datenbank-Kapazitäten stornieren möchten, wird unter Umständen eine Gebühr für die vorzeitige Kündigung in Höhe von 12% berechnet. Rückerstattungen basieren auf dem niedrigsten Preis (entweder Ihrem Kaufpreis oder dem aktuellen Preis für die Reservierung). Rückerstattungen sind auf 50.000 US-Dollar pro Jahr beschränkt. Die Rückerstattung, die Sie erhalten, umfasst den verbleibenden anteiligen Saldo abzüglich der Gebühr für die vorzeitige Kündigung in Höhe von 12%. Navigieren Sie zum Anfordern einer Stornierung im Azure-Portal zu der Reservierung, und wählen Sie **Erstattung** aus, um eine Supportanfrage zu erstellen.

Wenn Sie Ihre Reservierung der reservierten SQL-Datenbank-Kapazitäten in eine andere Region, einen anderen Bereitstellungstyp, eine andere Leistungsstufe oder Laufzeit ändern müssen, können Sie sie gegen eine andere Reservierung mit gleichem oder höherem Wert umtauschen. Das Startdatum der Laufzeit für die neue Reservierung wird nicht von der umgetauschten Reservierung übernommen. Die Laufzeit von einem oder drei Jahren beginnt ab der Erstellung der neuen Reservierung. Navigieren Sie zum Anfordern eines Umtauschs im Azure-Portal zu der Reservierung, und wählen Sie **Umtausch** aus, um eine Supportanfrage zu erstellen.

## <a name="vcore-size-flexibility"></a>Flexibilität der V-Kern-Größe

Die Flexibilität der V-Kern-Größe ermöglicht ein zentrales Hoch- oder Herunterskalieren innerhalb einer Leistungsstufe und Region, ohne den Vorteil reservierter Kapazität einzubüßen. Mit der reservierten Kapazität von SQL-Datenbank erhalten Sie außerdem die Flexibilität, Ihre aktiven Datenbanken vorübergehend zwischen Pools und Einzeldatenbanken im Rahmen des normalen Betriebs (innerhalb derselben Region und Leistungsstufe) zu verschieben, ohne den Vorteil reservierter Kapazität einzubüßen. Dank eines nicht angewendeten Puffers in Ihrer Reservierung können Sie effektiv Leistungsspitzen verwalten, ohne Ihr Budget zu überschreiten.

## <a name="next-steps"></a>Nächste Schritte

Der Rabatt auf die Reservierung virtueller Kerne wird automatisch auf die Anzahl der SQL-Datenbank-Instanzen angewendet, die dem Reservierungsumfang und den -attributen reservierter SQL-Datenbank-Instanzen entsprechen. Sie können den Reservierungsumfang von SQL-Datenbank-Kapazitäten über das [Azure-Portal](https://portal.azure.com), PowerShell, die CLI oder die API aktualisieren. 

Informationen zum Verwalten der Reservierung von SQL-Datenbank-Kapazitäten finden Sie unter [Verwalten von reservierten Instanzen in Azure](../billing/billing-manage-reserved-vm-instance.md).

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](../billing/billing-save-compute-costs-reservations.md)
- [Verwalten von Azure-Reservierungen](../billing/billing-manage-reserved-vm-instance.md)
- [Grundlegendes zum Rabatt für Azure-Reservierungen](../billing/billing-understand-reservation-charges.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../billing/billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../billing/billing-understand-reserved-instance-usage-ea.md)
- [Verkaufen Microsoft Azure Reserved Instances](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

