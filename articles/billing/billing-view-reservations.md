---
title: Anzeigen von Reservierungen für Azure-Ressourcen | Microsoft-Dokumentation
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
ms.author: banders
ms.openlocfilehash: 3a421e71cf93820716298fcfe4ccb1bcbe843bd1
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904028"
---
# <a name="view-reservations-for-azure-in-the-azure-portal"></a>Anzeigen von Reservierungen für Azure im Azure-Portal

Je nach Abonnementtyp und Berechtigungen gibt es mehrere Möglichkeiten, um Reservierungen für Azure-Ressourcen anzuzeigen.

## <a name="view-reservations-as-owner-or-reader"></a>Anzeigen von Reservierungen als Besitzer oder Leser

Standardmäßig können Sie und der Kontoadministrator die Reservierung anzeigen, wenn Sie eine Reservierung kaufen. Ihnen und dem Kontoadministrator wird automatisch die Rolle „Besitzer“ für die Reservierung zugewiesen. Damit andere Personen die Reservierung anzeigen können, müssen Sie sie als **Besitzer** oder **Leser** für die Reservierung hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Gehen Sie folgendermaßen vor, um eine Reservierung als Besitzer oder Leser anzuzeigen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Reservierungen**.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-view-reservation/portal-reservation-search.png)

1. Es wird eine Liste der Reservierungen angezeigt, für die Sie die Rolle „Besitzer“ oder „Leser“ ausüben.

Wenn Sie den Bereich einer Reservierung ändern, eine Reservierung aufteilen oder ändern möchten, wer eine Reservierung verwalten kann, lesen Sie [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Anzeigen von Reservierungstransaktionen für Enterprise-Registrierungen

 Wenn Sie über einen partnergesteuerte Enterprise-Registrierung verfügen, zeigen Sie Reservierungen an, indem Sie im EA-Portal zu **Berichte** navigieren. Bei anderen Enterprise-Registrierungen können Sie Reservierungen im EA-Portal und im Azure-Portal anzeigen. Sie müssen ein EA-Administrator sein, um Reservierungstransaktionen anzeigen zu können.

Gehen Sie folgendermaßen vor, um Reservierungstransaktionen im Azure-Portal anzuzeigen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Wählen Sie **Reservierungstransaktionen** aus.
1. Wählen Sie zum Filtern der Ergebnisse **Zeitraum**, **Typ** oder **Beschreibung** aus.
1. Wählen Sie **Übernehmen**.

    ![Screenshot, der Ergebnisse für Reservierungstransaktionen zeigt](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

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

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
