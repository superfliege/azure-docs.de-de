---
title: Verwalten eines virtuellen Azure-Computers mit Bestandserfassung | Microsoft-Dokumentation
description: Verwalten eines virtuellen Computers mit Bestandserfassung
services: automation
keywords: "Inventar, Inventur, Bestand, Automatisierung, Änderung, Nachverfolgung"
author: jennyhunter-msft
ms.author: jehunte
ms.date: 09/13/2017
ms.topic: hero-article
manager: carmonm
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 5291d112c2cdf157543fe301d5a5c80f3fe561ae
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Verwalten eines virtuellen Azure-Computers mit Bestandserfassung

Die Nachverfolgung des Inventars kann auf der Ressourcenseite eines virtuellen Azure-Computers für diesen Computer aktiviert werden. Diese Methode stellt eine browserbasierte Benutzeroberfläche zum Einrichten und Konfigurieren der Bestandserfassung bereit.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
Wenn Sie noch nicht über einen virtuellen Azure-Computer verfügen, erstellen Sie einen [virtuellen Computer](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal), bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Aktivieren der Bestandserfassung über die Ressourcenseite des virtuellen Computers

1. Wählen Sie links im Bildschirm **Virtuelle Computer** aus.
1. Wählen Sie einen virtuellen Computer aus der Liste aus.
1. Wählen Sie im Ressourcenmenü unterhalb von **Vorgänge** die Option **Bestand (Vorschau)** aus.
1. Am oberen Rand der Seite wird ein Banner angezeigt, in dem Sie darüber informiert werden, dass die Lösung nicht aktiviert ist. Klicken Sie auf das Banner, um die Lösung zu aktivieren.
1. Wählen Sie einen Log Analytics-Arbeitsbereich aus, in dem Ihre Datenprotokolle gespeichert werden sollen. Wenn für diese Region keine Arbeitsbereiche verfügbar sind, werden Sie aufgefordert, einen Standardarbeitsbereich und ein Automation-Konto zu erstellen. Klicken Sie auf **Aktivieren**, um das Onboarding Ihres Computers zu starten.

   ![Anzeigen von Onboardingoptionen](./media/automation-vm-inventory/inventory-onboarding-options.png)  

1. Sie werden in einer Statusleiste benachrichtigt, dass die Lösung aktiviert wird. Dieser Vorgang kann bis zu 15 Minuten dauern. In dieser Zeit können Sie das Blatt schließen. Wenn Sie es offen lassen, werden Sie darin informiert, sobald die Lösung aktiviert wurde. Sie können den Bereitstellungsstatus über den Benachrichtigungsbereich überwachen.

   ![Anzeigen der Inventarlösung direkt nach dem Onboarding](./media/automation-vm-inventory/inventory-onboarded.png)

1. Wenn die Bereitstellung abgeschlossen ist, wird die Statusleiste nicht mehr angezeigt. Zu diesem Zeitpunkt sammelt das System noch Bestandsdaten, und die Daten sind möglicherweise noch nicht sichtbar. Eine vollständige Sammlung der Daten kann 24 Stunden dauern.

## <a name="configure-your-inventory-settings"></a>Konfigurieren der Inventareinstellungen

Standardmäßig sind Software, Windows-Dienste und Linux-Daemons für die Sammlung konfiguriert. Um auch Daten zu Windows-Registrierung und Dateien zu sammeln, konfigurieren Sie die Einstellungen der Bestandserfassung.

1. Klicken Sie in der Ansicht **Bestand (Vorschau)** auf die Schaltfläche **Einstellungen bearbeiten** am oberen Rand der Seite.
2. Um eine neue Einstellung für die Sammlung hinzuzufügen, navigieren Sie über die Registerkarten **Windows-Registrierung**, **Windows-Dateien** und **Linux-Dateien** zu der Einstellungskategorie, die Sie hinzufügen möchten. Klicken Sie am oberen Seitenrand auf **Hinzufügen**.
3. Details und Beschreibungen jeder Einstellungseigenschaft finden Sie auf der [Dokumentationsseite zum Inventar](https://aka.ms/configinventorydocs).

## <a name="disconnecting-your-virtual-machine-from-management"></a>Trennen des virtuellen Computers von der Verwaltung

So trennen Sie Ihren Computer von der Bestandsverwaltung:

1. Klicken Sie Azure-Portal im linken Menü auf **Log Analytics**, und wählen Sie den Arbeitsbereich aus, den Sie beim Onboarding Ihres virtuellen Computers verwendet haben.
1. Wählen Sie auf der Log Analytics-Seite im Ressourcenmenü unter der Kategorie **Arbeitsbereichsdatenquellen** die Option **Virtuelle Computer** aus. 
1. Wählen Sie den virtuellen Computer aus, den Sie von der Liste trennen möchten. Für diesen Computer wird neben dem Text „Dieser Arbeitsbereich“ ein grünes Häkchen in der Spalte**OMS-Verbindung** angezeigt. Klicken Sie am oberen Rand der nächsten Seite auf **Trennen**, und klicken Sie im Bestätigungsdialogfeld auf **Ja**, um den Computer von der Verwaltung zu trennen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Verwalten von Änderungen in Dateien und Registrierungseinstellungen finden Sie unter [Änderungsnachverfolgung](../log-analytics/log-analytics-change-tracking.md).
* Informationen zur Verwaltung von Windows- und Paketupdates auf Ihren virtuellen Computern finden Sie unter [Updateverwaltung](../operations-management-suite/oms-solution-update-management.md).

