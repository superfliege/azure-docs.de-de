---
title: Vorbereiten von Azure für die Notfallwiederherstellung von lokalen Computern mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure für die Notfallwiederherstellung von lokalen Computern mit Azure Site Recovery vorbereiten.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8bdb711d39f514375362235388943ec42451b312
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58315571"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Vorbereiten von Azure-Ressourcen für die Notfallwiederherstellung von lokalen Computern

 [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Artikel ist das erste Tutorial in einer Reihe, welche die Einrichtung der Notfallwiederherstellung für lokale virtuelle Computer veranschaulicht. Die Informationen gelten für das Schützen von lokalen virtuellen VMware-Computern, virtuellen Hyper-V-Computern und physischen Servern.

> [!NOTE]
> Tutorials dienen zur Veranschaulichung des einfachsten Bereitstellungspfads für ein Szenario. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Eine ausführliche Anleitung finden Sie jeweils im Abschnitt mit der **Gewusst wie-Anleitung** für das entsprechende Szenario.

In diesem Artikel wird gezeigt, wie Sie Azure-Komponenten vorbereiten, wenn Sie lokale virtuelle Computer (Hyper-V oder VMware) oder physische Windows/Linux-Server in Azure replizieren möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen, ob Ihr Azure-Konto über Replikationsberechtigungen verfügt
> * Erstellen Sie einen Recovery Services-Tresor. Ein Tresor enthält Metadaten und Konfigurationsinformationen für virtuelle Computer und andere Replikationskomponenten.
> * Richten Sie ein Azure-Netzwerk ein. Wenn die Azure-VMs nach einem Failover erstellt werden, werden sie mit diesem Azure-Netzwerk verbunden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="verify-account-permissions"></a>Überprüfen von Kontoberechtigungen

Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator Ihres Abonnements. Falls Sie nicht der Abonnementadministrator sind, können Sie sich an den Administrator wenden, damit dieser die erforderlichen Berechtigungen zuweist. Zum Aktivieren der Replikation für einen neuen virtuellen Computer benötigen Sie die folgenden Berechtigungen:

- Erstellen einer VM in der ausgewählten Ressourcengruppe
- Erstellen einer VM im ausgewählten virtuellen Netzwerk
- Schreiben ins Speicherkonto
- Schreiben auf verwaltete Datenträger

Zum Ausführen dieser Aufgaben muss Ihrem Konto die integrierte Rolle „Mitwirkender von virtuellen Computern“ zugewiesen werden. Zum Verwalten von Site Recovery-Vorgängen in einem Tresor muss Ihrem Konto außerdem die integrierte Rolle „Site Recovery-Mitwirkender“ zugewiesen werden.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

1. Klicken Sie im Azure-Portal auf **+Ressource erstellen**, und suchen Sie im Marketplace nach **Wiederherstellungsdienste**.
2. Klicken Sie auf **Backup & Site Recovery (OMS)**, und klicken Sie auf der Seite „Backup & Site Recovery“ auf **Erstellen**. 
1. Geben Sie unter **Recovery Services-Tresor** > **Name** einen Anzeigenamen ein, über den der Tresor identifiziert wird. Für diese Tutorials verwenden wir **ContosoVMVault**.
2. Wählen Sie in **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue. Für dieses Tutorial verwenden wir **contosoRG**.
3. Wählen Sie in **Standort** die Region aus, in der der Tresor gespeichert werden soll. verwenden wir **Europa, Westen**.
4. Wählen Sie **An Dashboard anheften** > **Erstellen**, um über das Dashboard schnell auf den Tresor zuzugreifen.

   ![Erstellen eines neuen Tresors](./media/tutorial-prepare-azure/new-vault-settings.png)

   Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf der Hauptseite **Recovery Services-Tresore** angezeigt.

## <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Wenn die Azure-VMs nach einem Failover aus verwalteten Datenträgern erstellt werden, werden sie mit diesem Netzwerk verknüpft.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**.
2. Wir behalten **Resource Manager** als Auswahl für das Bereitstellungsmodell bei.
3. Geben Sie unter **Name** einen Netzwerknamen ein. Der Name muss innerhalb der Azure-Ressourcengruppe eindeutig sein. Wir verwenden **ContosoASRnet** in diesem Tutorial.
4. Geben Sie die Ressourcengruppe an, in der das Netzwerk erstellt wird. Wir verwenden die vorhandene Ressourcengruppe **contosoRG**.
5. Geben Sie unter **Adressbereich** den Netzwerkadressbereich **10.0.0.0/24** ein. In diesem Netzwerk verwenden wir kein Subnetz.
6. Wählen Sie unter **Abonnement** das Abonnement aus, in dem das Netzwerk erstellt werden soll.
7. Wählen Sie als **Ort** die Option **Europa, Westen** aus. Das Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden.
8. Wir behalten die Standardoptionen des DDoS-Basisschutzes ohne Dienstendpunkt im Netzwerk bei.
9. Klicken Sie auf **Create**.

   ![Erstellen eines virtuellen Netzwerks](media/tutorial-prepare-azure/create-network.png)

   Die Erstellung des virtuellen Netzwerks dauert ein paar Sekunden. Nach der Erstellung wird es auf dem Dashboard im Azure-Portal angezeigt.

## <a name="useful-links"></a>Nützliche Links

- [Informationen zu](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) Azure-Netzwerken.
- [Informationen zu verwalteten Datenträgern](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorbereiten der lokalen VMware-Infrastruktur für die Notfallwiederherstellung in Azure](tutorial-prepare-on-premises-vmware.md)
