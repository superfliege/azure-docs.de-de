---
title: Verwalten von reservierten Azure-VM-Instanzen | Microsoft Docs
description: Erfahren Sie, wie Sie den Abonnementbereich ändern und den Zugriff auf Azure Reserved VM Instances verwalten können.
services: billing
documentationcenter: ''
author: vikramdesai01
manager: vikramdesai01
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: vikdesai
ms.openlocfilehash: ddb9d46dc2689b0dbcd8734e276916f7cd9d2728
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064640"
---
# <a name="manage-reserved-instances-in-azure"></a>Verwalten von reservierten Instanzen in Azure

Nachdem Sie eine reservierte Azure-VM-Instanz erworben haben, können Sie die reservierte Instanz auf ein anderes Abonnement als das beim Kauf angegebene anwenden. Wenn Ihre übereinstimmenden virtuellen Computer in mehreren Abonnements ausgeführt werden, können Sie den Bereich der reservierten Instanz auch in „Freigegeben“ ändern. Um den Rabatt der reservierten Instanz zu maximieren, stellen Sie sicher, dass die Anzahl der von Ihnen erworbenen Instanzen mit den Attributen und der Anzahl der ausgeführten virtuellen Computer übereinstimmt. Weitere Informationen zu reservierten Azure-Instanzen finden Sie unter [Vorauszahlen für virtuelle Azure-Computer](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Ändern des Bereichs für eine reservierte Instanz
 Ihr Rabatt für reservierte Instanzen gilt für virtuelle Computer, die mit Ihrer reservierten Instanz übereinstimmen und innerhalb des Bereichs der reservierten Instanz ausgeführt werden. Der Bereich einer reservierten Instanz kann ein einzelnes Abonnement sein oder alle Abonnements in Ihrem Abrechnungskontext umfassen. Wenn Sie den Bereich auf ein einzelnes Abonnement festlegen, wird die reservierte Instanz auf die ausgeführten virtuellen Computer im ausgewählten Abonnement abgestimmt. Wenn Sie den Bereich auf „Freigegeben“ festlegen, ordnet Azure die reservierte Instanz virtuellen Computern zu, die in allen Abonnements innerhalb des Abrechnungskontexts ausgeführt werden. Der Abrechnungskontext ist abhängig vom Abonnement, das für den Erwerb der reservierten Instanz verwendet wurde. Weitere Informationen finden Sie unter [Vorauszahlen für virtuelle Computer mit reservierten VM-Instanzen](https://go.microsoft.com/fwlink/?linkid=861721).

Gehen Sie wie folgt vor, um den Bereich einer reservierten Instanz zu aktualisieren: 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **Alle Dienste** > **Reservierungen**.
3. Wählen Sie die reservierte Instanz aus.
4. Wählen Sie **Einstellungen** > **Konfiguration** aus.
5. Ändern Sie den Bereich. Wenn Sie den Bereich aus „Freigegeben“ in „Einzeln“ ändern, können Sie nur Abonnements auswählen, deren Besitzer Sie sind. Es können nur Abonnements ausgewählt werden, die sich im gleichen Abrechnungskontext wie die reservierte Instanz befinden. Der Abrechnungskontext wird durch das Abonnement bestimmt, das Sie beim Kauf der reservierten Instanz ausgewählt haben. Der Bereich gilt nur für nutzungsbasierte Angebote für MS-AZR-0003P-Abonnements und Enterprise-Angebote für MS-AZR-0017P-Abonnements. Bei Enterprise Agreements sind Abonnements für Entwicklung und Tests nicht berechtigt, den Rabatt für reservierte Instanzen zu erhalten.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>Aufteilen einer einzelnen reservierten Instanz in zwei reservierte Instanzen
 Wenn Sie mehrere Instanzen erworben haben, können Sie Instanzen innerhalb einer reservierten Instanz verschiedenen Abonnements zuweisen. Standardmäßig weisen alle Instanzen (Menge, die während des Kaufvorgangs angegeben wird) einen Bereich auf: ein einzelnes Abonnement oder „Freigegeben“. Beispiel: Sie haben 10 „Standard D2“-VMs erworben und den Bereich als Abonnement A festgelegt. Sie können nun den Bereich für sieben reservierte Instanzen in Abonnement A und die restlichen drei in Abonnement B ändern. Durch die Aufteilung einer reservierten Instanz können Sie Instanzen für die fein abgestufte Bereichsverwaltung verteilen. Sie können die Zuordnung zu Abonnements vereinfachen, indem Sie den Bereich „Freigegeben“ auswählen. Aus Gründen der Kostenverwaltung oder der Budgetierung können Sie jedoch bestimmten Abonnements Mengen zuordnen.

 Sie können eine reservierte Instanz mithilfe von PowerShell, der CLI oder über die API in zwei reservierte Instanzen aufteilen.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Aufteilen einer reservierten Instanz mithilfe von PowerShell
1. Rufen Sie die Auftrags-ID der reservierten Instanz ab, indem Sie den folgenden Befehl ausführen:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```
2. Rufen Sie die Details einer reservierten Instanz ab:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. Teilen Sie die reservierte Instanz in zwei Instanzen auf, und verteilen Sie die Instanzen:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Sie können den Bereich aktualisieren, indem Sie den folgenden Befehl ausführen:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Hinzufügen oder Ändern von Benutzern, die eine reservierte Instanz verwalten können
Sie können die Verwaltung einer reservierten Instanz delegieren, indem Sie Personen zu Rollen für die reservierte Instanz hinzufügen. Standardmäßig verfügen die Person, die die reservierte Instanz erworben hat, und der Kontoadministrator über die Rolle „Besitzer“ für die reservierte Instanz. 

Sie können den Zugriff auf reservierte Instanzen unabhängig von den Abonnements verwalten, die den Rabatt für reservierte Instanzen erhalten. Wenn Sie einer Person Berechtigungen zum Verwalten einer reservierten Instanz erteilen, erhält diese keine Rechte zum Verwalten des Abonnements. Und wenn Sie einer Person Berechtigungen zum Verwalten eines Abonnements im Bereich der reservierten Instanz erteilen, erhält diese Person keine Rechte zum Verwalten der reservierten Instanz.
 
Gehen Sie wie folgt vor, um die Zugriffsverwaltung für eine reservierte Instanz zu delegieren: 
1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2.  Wählen Sie **Alle Dienste** > **Reservierung** aus, um die reservierten Instanzen aufzulisten, auf die Sie Zugriff haben.
3.  Wählen Sie die reservierte Instanz aus, für die Sie den Zugriff an andere Benutzer delegieren möchten.
4.  Wählen Sie im Menü die Option **Zugriffssteuerung (IAM)** aus.
5.  Wählen Sie **Hinzufügen** > **Rolle** > **Besitzer** (oder eine andere Rolle, wenn Sie eingeschränkten Zugriff erteilen möchten) aus. 
6. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten. 
7. Wählen Sie den Benutzer und dann **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu reservierten Azure-Instanzen finden Sie in den folgenden Artikeln:

- [Was ist Azure Reserved VM Instances?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Grundlegendes zur Anwendung des Rabatts für reservierte Instanzen](billing-understand-vm-reservation-charges.md)
- [Grundlagen zur Verwendung reservierter Azure-Instanzen für Ihr Abonnement mit nutzungsbasierter Zahlung](billing-understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung reservierter Instanzen für die Enterprise-Registrierung](billing-understand-reserved-instance-usage-ea.md)
- [Nicht in reservierten Instanzen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
