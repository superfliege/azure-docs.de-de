---
title: Anzeigen von Azure-Reservierungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Reservierungen im Azure-Portal anzeigen.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2018
ms.author: cwatson
ms.openlocfilehash: 2ae30ca55f3ca03a64438025960ddd807e288216
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48272601"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Anzeigen von Azure-Reservierungen im Azure-Portal

Je nach Abonnementtyp und Berechtigungen gibt es mehrere Möglichkeiten, um Azure-Reservierungen anzuzeigen.

## <a name="view-reservations-as-owner-or-reader"></a>Anzeigen von Reservierungen als Besitzer oder Leser

Standardmäßig können Sie und der Kontoadministrator die Reservierung anzeigen, wenn Sie eine Reservierung kaufen. Ihnen und dem Kontoadministrator wird automatisch die Rolle „Besitzer“ für die Reservierung zugewiesen. Damit andere Personen die Reservierung anzeigen können, müssen Sie sie als **Besitzer** oder **Leser** für die Reservierung hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Gehen Sie folgendermaßen vor, um eine Reservierung als Besitzer oder Leser anzuzeigen:

1. Melden Sie sich am [Azure-Portal]( http://portal.azure.com) an.
1. Suchen Sie nach **Reservierungen**.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-view-reservation/portal-reservation-search.png)

1. Es wird eine Liste der Reservierungen angezeigt, für die Sie die Rolle „Besitzer“ oder „Leser“ ausüben.

Wenn Sie den Bereich einer Reservierung ändern, eine Reservierung aufteilen oder ändern möchten, wer eine Reservierung verwalten kann, lesen Sie [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Anzeigen von Reservierungstransaktionen für Enterprise-Registrierungen

 Wenn Sie über einen partnergesteuerte Enterprise-Registrierung verfügen, zeigen Sie Reservierungen an, indem Sie im EA-Portal zu **Berichte** navigieren. Bei anderen Enterprise-Registrierungen können Sie Reservierungen im EA-Portal und im Azure-Portal anzeigen. Sie müssen ein EA-Administrator sein, um Reservierungstransaktionen anzeigen zu können.

Gehen Sie folgendermaßen vor, um Reservierungstransaktionen im Azure-Portal anzuzeigen:

1. Melden Sie sich aim [Azure-Portal]( http://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Wählen Sie **Reservierungstransaktionen** aus.
1. Wählen Sie zum Filtern der Ergebnisse **Zeitraum**, **Typ** oder **Beschreibung** aus.
1. Wählen Sie **Übernehmen**.

    ![Screenshot, der Ergebnisse für Reservierungstransaktionen anzeigt](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Wenn Sie die Daten mithilfe einer API abrufen möchten, lesen Sie [Abrufen von Gebühren für reservierte Instanzen für Unternehmenskunden](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für reservierte Azure Cosmos DB-Kapazität](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
