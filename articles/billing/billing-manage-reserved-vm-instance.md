---
title: Verwalten von reservierten Azure-VM-Instanzen | Microsoft Docs
description: "Erfahren Sie, wie Sie den Abonnementbereich ändern und den Zugriff auf reservierte Azure-VM-Instanzen verwalten können."
services: billing
documentationcenter: 
author: vikramdesai01
manager: vikramdesai01
editor: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/06/2017
ms.author: vikdesai
ms.openlocfilehash: e23eea52ff5d27beacf938a1ef153172e24f1aee
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="manage-reserved-virtual-machine-instances"></a>Verwalten von reservierten Azure-VM-Instanzen

Nachdem Sie eine reservierte Azure-VM-Instanz erworben haben, können Sie die Reservierung auf ein anderes Abonnement als das beim Kauf angegebene anwenden. Wenn Ihre übereinstimmenden virtuellen Computer in mehreren Abonnements ausgeführt werden, können Sie den Reservierungsbereich auch in „Freigegeben“ ändern. Um den Reservierungsrabatt zu maximieren, stellen Sie sicher, dass die Anzahl der von Ihnen erworbenen Instanzen mit den Attributen und der Anzahl der ausgeführten virtuellen Computer übereinstimmt. Weitere Informationen zu reservierten VM-Instanzen finden Sie unter [Sparen Sie Geld, indem Sie für virtuelle Azure-Computer im Voraus bezahlen](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reservation"></a>Ändern des Bereichs für eine Reservierung
 Ihr Reservierungsrabatt gilt für virtuelle Computer, die mit Ihrer Reservierung übereinstimmen und innerhalb des Reservierungsbereichs ausgeführt werden. Der Bereich einer Reservierung können ein einzelnes Abonnement oder alle Abonnements in Ihrem Abrechnungskontext sein. Wenn Sie den Bereich auf ein einzelnes Abonnement festlegen, wird die Reservierung mit den ausgeführten virtuellen Computern im ausgewählten Abonnement abgestimmt. Wenn Sie den Bereich auf „Freigegeben“ festlegen, ordnet Azure die Reservierung virtuellen Computern zu, die in allen Abonnements innerhalb des Abrechnungskontexts ausgeführt werden. Der Abrechnungskontext ist abhängig vom Abonnement, das verwendet wurde, um die Reservierung zu erwerben. Weitere Informationen finden Sie unter [Vorabzahlung für VMs mit reservierten VM-Instanzen](https://go.microsoft.com/fwlink/?linkid=861721).

So aktualisieren Sie den Bereich einer Reservierung: 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie **Weitere Dienste** > **Reservierungen** aus.
3. Wählen Sie die Reservierung aus.
4. Wählen Sie **Einstellungen** > **Konfiguration** aus.
5. Ändern Sie den Bereich. Wenn Sie den Bereich aus „Freigegeben“ in „Einzeln“ ändern, können Sie nur Abonnements auswählen, deren Besitzer Sie sind. Es können nur Abonnements ausgewählt werden, die sich im gleichen Abrechnungskontext wie die Reservierung befinden. Der Abrechnungskontext wird durch das Abonnement bestimmt, das Sie beim Kauf der Reservierung ausgewählt haben. Der Bereich gilt nur für nutzungsbasierte Angebote für MS-AZR-0003P-Abonnements und Enterprise-Angebote für MS-AZR-0017P-Abonnements. Bei Enterprise Agreements sind dev/test-Abonnements nicht berechtigt, den Reservierungsrabatt zu erhalten.

## <a name="split-a-single-reservation-into-two-reservations"></a>Aufteilten einer einzelnen Reservierung in zwei Reservierungen
 Wenn Sie mehrere Instanzen erworben haben, können Sie Instanzen innerhalb einer Reservierung verschiedenen Abonnements zuordnen. Standardmäßig weisen alle Instanzen (Menge, die während des Kaufvorgangs angegeben wird) einen Bereich auf: ein einzelnes Abonnement oder „Freigegeben“. Sie haben z.B. 10 Standard-D2-VMs gekauft und den Bereich als Abonnement A festgelegt. Sie können nun den Bereich für 7 reservierte VM-Instanzen in Abonnement A und die restlichen 3 in Abonnement B ändern. Durch die Aufteilung einer Reservierung können Sie Instanzen für die fein abgestufte Bereichsverwaltung verteilen. Sie können die Zuordnung zu Abonnements vereinfachen, indem Sie den Bereich „Freigegeben“ auswählen. Aus Gründen der Kostenverwaltung oder der Budgetierung können Sie jedoch bestimmten Abonnements Mengen zuordnen.

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
    # Split the reservation. The sum of the Reserved VM instances, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Sie können den Bereich aktualisieren, indem Sie den folgenden Befehl ausführen:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können
Sie können die Verwaltung einer Reservierung delegieren, indem Sie Personen zu Rollen für die Reservierung hinzufügen. Standardmäßig üben die Person, die die Reservierung erworben hat, und der Kontoadministrator die Rolle „Besitzer“ für die Reservierung aus. 

Sie können den Zugriff auf Reservierungen unabhängig von den Abonnements verwalten, die den Reservierungsrabatt erhalten. Wenn Sie einer Person die Berechtigung zum Verwalten einer Reservierung erteilen, erhält diese keine Rechte zum Verwalten des Abonnements. Und wenn Sie einer Person die Berechtigung erteilen, ein Abonnement im Bereich der Reservierung zu verwalten, erhält diese Person keine Rechte zum Verwalten der Reservierung.
 
So delegieren Sie die Zugriffsverwaltung für eine Reservierung: 
1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2.  Wählen Sie **Weitere Dienste** > **Reservierung** aus, um Reservierungen aufzulisten, auf die Sie zugreifen können.
3.  Wählen Sie die Reservierung aus, für die Sie den Zugriff an andere Benutzer delegieren möchten.
4.  Wählen Sie im Menü die Option **Zugriffssteuerung (IAM)** aus.
5.  Wählen Sie **Hinzufügen** > **Rolle** > **Besitzer** (oder eine andere Rolle, wenn Sie eingeschränkten Zugriff erteilen möchten) aus. 
6. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten. 
7. Wählen Sie den Benutzer und dann **Speichern** aus.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
