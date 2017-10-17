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
ms.openlocfilehash: bfdd67ac1018f242e415da1a66d93f22bea8d054
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-virtual-machine-with-inventory-collection"></a>Verwalten eines virtuellen Azure-Computers mit Bestandserfassung

Sie können die Bestandsnachverfolgung für einen virtuellen Azure-Computer auf der Ressourcenseite des virtuellen Computers aktivieren. Diese Methode stellt eine browserbasierte Benutzeroberfläche zum Einrichten und Konfigurieren der Bestandserfassung bereit.

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
Falls Sie noch nicht über einen virtuellen Azure-Computer verfügen, können Sie einen [virtuellen Computer erstellen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.

## <a name="enable-inventory-collection-from-the-virtual-machine-resource-page"></a>Aktivieren der Bestandserfassung über die Ressourcenseite des virtuellen Computers

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Virtuelle Computer**.
2. Wählen Sie in der Liste mit den virtuellen Computern eine VM aus.
3. Wählen Sie im Menü **Ressource** unter **Operations** (Vorgänge) die Option **Bestand (Vorschau)**.  
    Am oberen Rand des Fensters wird ein Banner angezeigt, in dem Sie darüber informiert werden, dass die Lösung nicht aktiviert ist. 
4. Wählen Sie das Banner aus, um die Lösung zu aktivieren.
5. Wählen Sie einen Log Analytics-Arbeitsbereich aus, in dem Ihre Datenprotokolle gespeichert werden sollen.  
    Wenn für diese Region kein Arbeitsbereich verfügbar ist, werden Sie aufgefordert, einen Standardarbeitsbereich und ein Automation-Konto zu erstellen. 
6. Wählen Sie die Option **Aktivieren**, um das Onboarding für Ihren Computer zu starten.

   ![Anzeigen von Onboardingoptionen](./media/automation-vm-inventory/inventory-onboarding-options.png)  
    Sie werden in einer Statusleiste benachrichtigt, dass die Lösung aktiviert wird. Dieser Vorgang kann bis zu 15 Minuten dauern. Während dieser Zeit können Sie das Fenster schließen oder geöffnet lassen. Sie werden benachrichtigt, wenn die Lösung aktiviert wurde. Sie können den Bereitstellungsstatus über den Benachrichtigungsbereich überwachen.

   ![Anzeigen der Inventarlösung direkt nach dem Onboarding](./media/automation-vm-inventory/inventory-onboarded.png)

Nachdem die Bereitstellung abgeschlossen ist, wird die Statusleiste nicht mehr angezeigt. Es werden weiterhin Bestandsdaten gesammelt, und es kann sein, dass die Daten noch nicht sichtbar sind. Eine vollständige Sammlung der Daten kann 24 Stunden dauern.

## <a name="configure-your-inventory-settings"></a>Konfigurieren der Inventareinstellungen

Standardmäßig sind Software, Windows-Dienste und Linux-Daemons für die Sammlung konfiguriert. Konfigurieren Sie die Einstellungen der Bestandserfassung, um auch Bestandsdaten zur Windows-Registrierung und zu den Dateien zu sammeln.

1. Wählen Sie in der Ansicht **Bestand (Vorschau)** am oberen Rand des Fensters die Schaltfläche **Einstellungen bearbeiten**.
2. Navigieren Sie zum Hinzufügen einer neuen Sammlungseinstellung zu der Einstellungskategorie, die Sie hinzufügen möchten, indem Sie die Registerkarten **Windows-Registrierung**, **Windows-Dateien** und **Linux-Dateien** wählen. 
3. Wählen Sie oben im Fenster die Option **Hinzufügen**.
4. Details und Beschreibungen der einzelnen Einstellungseigenschaften finden Sie auf der [Dokumentationsseite zur Bestandserfassung](https://aka.ms/configinventorydocs).

## <a name="disconnect-your-virtual-machine-from-management"></a>Trennen des virtuellen Computers von der Verwaltung

Gehen Sie wie folgt vor, um Ihren virtuellen Computer aus der Bestandsverwaltung herauszunehmen:

1. Wählen Sie im linken Bereich des Azure-Portals die Option **Log Analytics** und dann den Arbeitsbereich, den Sie beim Onboarding Ihres virtuellen Computers verwendet haben.
2. Wählen Sie im Fenster **Log Analytics** im Menü **Ressource** unter der Kategorie **Arbeitsbereichsdatenquellen** die Option **Virtuelle Computer**. 
3. Wählen Sie in der Liste den virtuellen Computer aus, den Sie trennen möchten. Der virtuelle Computer ist in der Spalte **OMS-Verbindung** neben **Dieser Arbeitsbereich** mit einem grünen Häkchen gekennzeichnet. 
4. Wählen Sie oben auf der nächsten Seite die Option **Disconnect** (Trennen).
5. Wählen Sie im Bestätigungsfenster die Option **Ja**.  
    Mit dieser Aktion wird der Computer von der Verwaltung getrennt.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Verwaltung von Änderungen in Dateien und Registrierungseinstellungen auf Ihren virtuellen Computern finden Sie unter [Nachverfolgen von Änderungen an der Software in Ihrer Umgebung mit der Change Tracking-Lösung](../log-analytics/log-analytics-change-tracking.md).
* Informationen zur Verwaltung von Windows- und Paketupdates auf Ihren virtuellen Computern finden Sie unter [Lösung für die Updateverwaltung in OMS](../operations-management-suite/oms-solution-update-management.md).
