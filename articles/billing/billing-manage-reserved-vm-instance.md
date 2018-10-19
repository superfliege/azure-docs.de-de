---
title: Verwalten von Azure-Reservierungen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Abonnementumfang ändern und den Zugriff auf Azure-Reservierungen verwalten können.
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
ms.date: 08/08/2018
ms.author: cwatson
ms.openlocfilehash: 0b19bb0d77bb600258596ce369713464641a7d2f
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423237"
---
# <a name="manage-reservations-for-azure-resources"></a>Verwalten von Reservierungen für Azure-Ressourcen

Nachdem Sie eine Azure-Reservierung erworben haben, müssen Sie möglicherweise die Reservierung auf ein anderes Abonnement anwenden, ändern, wer die Reservierung verwalten kann, oder den Reservierungsumfang ändern. Sie können auch eine Reservierung in zwei Reservierungen aufteilen, um einige der von Ihnen erworbenen Instanzen auf ein anderes Abonnement anzuwenden.

Wenn Sie Azure Reserved Virtual Machine Instances erworben haben, können Sie die Optimierungseinstellung für die Reservierung ändern. Der Reservierungsrabatt kann für virtuelle Computer (Virtual Machines, VMs) in der gleichen Reihe gelten. Sie können aber auch Rechenzentrumskapazität für eine bestimmte VM-Größe reservieren.

## <a name="change-the-scope-for-a-reservation"></a>Ändern des Bereichs für eine Reservierung

 Ihr Reservierungsrabatt gilt für virtuelle Computer, SQL-Datenbank-Instanzen, Azure Cosmos DB oder andere Ressourcen, die mit Ihrer Reservierung übereinstimmen und innerhalb des Reservierungsumfangs ausgeführt werden. Der Bereich einer Reservierung können ein einzelnes Abonnement oder alle Abonnements in Ihrem Abrechnungskontext sein. Wenn Sie den Umfang auf ein Einzelabonnement festlegen, wird die Reservierung mit den ausgeführten Ressourcen im ausgewählten Abonnement abgestimmt. Wenn Sie den Umfang auf „Freigegeben“ festlegen, ordnet Azure die Reservierung Ressourcen zu, die in allen Abonnements innerhalb des Abrechnungskontexts ausgeführt werden. Der Abrechnungskontext ist abhängig vom Abonnement, das verwendet wurde, um die Reservierung zu erwerben.

So aktualisieren Sie den Bereich einer Reservierung:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie die Reservierung aus.
4. Wählen Sie **Einstellungen** > **Konfiguration** aus.
5. Ändern Sie den Bereich. 

Wenn Sie den Bereich aus „Freigegeben“ in „Einzeln“ ändern, können Sie nur Abonnements auswählen, deren Besitzer Sie sind. Es können nur Abonnements ausgewählt werden, die sich im gleichen Abrechnungskontext wie die Reservierung befinden.

Der Bereich gilt nur für das nutzungsbasierte Angebot MS-AZR-0003P, für das Enterprise-Angebot MS-AZR-0017P oder für CSP-Abonnements. Bei Enterprise Agreements sind dev/test-Abonnements nicht berechtigt, den Reservierungsrabatt zu erhalten.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können

Sie können die Verwaltung einer Reservierung delegieren, indem Sie Personen zu Rollen für die Reservierung hinzufügen. Standardmäßig üben die Person, die die Reservierung erworben hat, und der Kontoadministrator die Rolle „Besitzer“ für die Reservierung aus.

Sie können den Zugriff auf Reservierungen unabhängig von den Abonnements verwalten, die den Reservierungsrabatt erhalten. Wenn Sie einer Person die Berechtigung zum Verwalten einer Reservierung erteilen, erhält diese keine Rechte zum Verwalten des Abonnements. Und wenn Sie einer Person die Berechtigung erteilen, ein Abonnement im Bereich der Reservierung zu verwalten, erhält diese Person keine Rechte zum Verwalten der Reservierung.

So delegieren Sie die Zugriffsverwaltung für eine Reservierung:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Alle Dienste** > **Reservierungen** aus, um Reservierungen aufzulisten, auf die Sie Zugriff haben.
3. Wählen Sie die Reservierung aus, für die Sie den Zugriff an andere Benutzer delegieren möchten.
4. Wählen Sie **Access Control (IAM)** aus.
5. Wählen Sie **Hinzufügen** > **Rolle** > **Besitzer**. Oder wählen Sie eine andere Rolle aus, wenn Sie eingeschränkten Zugriff erteilen möchten.
6. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten.
7. Wählen Sie den Benutzer und dann **Speichern** aus.

## <a name="split-a-single-reservation-into-two-reservations"></a>Aufteilten einer einzelnen Reservierung in zwei Reservierungen

 Wenn Sie mehrere Ressourceninstanzen innerhalb einer Reservierung erworben haben, können Sie Instanzen innerhalb dieser Reservierung verschiedenen Abonnements zuweisen. Standardmäßig weisen alle Instanzen einen Bereich auf: ein Einzelabonnement oder „Freigegeben“. Beispiel: Sie haben 10 Reservierungsinstanzen erworben und den Bereich als Abonnement A festgelegt. Sie können nun den Bereich für sieben Reservierungen in Abonnement A und die restlichen drei in Abonnement B ändern. Durch die Aufteilung einer Reservierung können Sie Instanzen für die fein abgestufte Bereichsverwaltung verteilen. Sie können die Zuordnung zu Abonnements vereinfachen, indem Sie den Bereich „Freigegeben“ auswählen. Aus Gründen der Kostenverwaltung oder der Budgetierung können Sie jedoch bestimmten Abonnements Mengen zuordnen.

 Sie können eine Reservierung mithilfe von PowerShell, der CLI oder über die API in zwei Reservierungen aufteilen.

### <a name="split-a-reservation-by-using-powershell"></a>Aufteilen einer Reservierung mithilfe von PowerShell

1. Rufen Sie die Reservierungsauftrags-ID ab, indem Sie den folgenden Befehl ausführen:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```

2. Abrufen der Details einer Reservierung:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Aufteilen der Reservierung in zwei Reservierungen und Verteilen der Instanzen:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Sie können den Bereich aktualisieren, indem Sie den folgenden Befehl ausführen:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="cancellations-and-exchanges"></a>Stornierungen und Umtausch

Abhängig vom Reservierungstyp können Sie eine Reservierung stornieren oder ändern. Weitere Informationen finden Sie in den Abschnitten zu Stornierung und Umtausch in den folgenden Themen:

- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](..//virtual-machines/windows/prepay-reserved-vm-instances.md#cancellations-and-exchanges)
- [Vorauszahlen für SUSE-Softwarepläne aus Azure-Reservierungen](../virtual-machines/linux/prepay-suse-software-charges.md#cancellation-and-exchanges-not-allowed)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md#cancellations-and-exchanges)

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Ändern der Optimierungseinstellung für reservierte VM-Instanzen

 Wenn Sie eine reservierte VM-Instanz erwerben, wählen Sie die Flexibilität bei der Instanzgröße oder die Kapazitätspriorität aus. Bei Auswahl der Flexibilität bei der Instanzgröße wird der Reservierungsrabatt auf weitere VMs in der [Gruppe mit VMs derselben Größe](https://aka.ms/RIVMGroups) angewendet. Die Option „Kapazitätspriorität“ priorisiert Rechenzentrumskapazität für Ihre Bereitstellungen. Diese Option bietet Ihnen zusätzliche Sicherheit, dass die VM-Instanzen gestartet werden können, wenn sie benötigt werden.

Wenn der Reservierungsumfang „Freigegeben“ lautet, ist die Flexibilität bei der Instanzgröße standardmäßig aktiviert. Die Rechenzentrumskapazität wird bei VM-Bereitstellungen nicht priorisiert.

Für Reservierungen, bei denen der Umfang auf Einzelabonnements festgelegt ist, können Sie die Reservierung anstelle von Flexibilität bei der VM-Instanzgröße im Hinblick auf die Kapazitätspriorität optimieren.

Gehen Sie wie folgt vor, um die Optimierungseinstellung für die Reservierung zu aktualisieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie die Reservierung aus.
4. Wählen Sie **Einstellungen** > **Konfiguration** aus.
5. Ändern Sie die Einstellung **Optimiert für**.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md)
- [Vorauszahlen für Azure Cosmos DB-Ressourcen mit reservierter Azure Cosmos DB-Kapazität](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Vorauszahlen für SUSE-Softwarepläne aus Azure-Reservierungen](../virtual-machines/linux/prepay-suse-software-charges.md)
- [Grundlegendes zur Anwendung des Rabatts für VM-Reservierungen](billing-understand-vm-reservation-charges.md)
- [Grundlegendes zur Anwendung des Rabatts für den SUSE Linux Enterprise-Softwareplan](../billing/billing-understand-suse-reservation-charges.md)
- [Grundlegendes zur Anwendung anderer Reservierungsrabatte](billing-understand-reservation-charges.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
