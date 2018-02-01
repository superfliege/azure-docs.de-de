---
title: "Erstellen von Ressourcen für die Verwendung mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure für die Replikation von lokalen Computern mithilfe des Azure Site Recovery-Diensts vorbereiten."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 63290127b298efced14ad34e9223840f3229f046
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Vorbereiten von Azure-Ressourcen für die Replikation von lokalen Computern

Der Dienst [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Tutorial wird gezeigt, wie Sie Azure-Komponenten vorbereiten, wenn Sie lokale virtuellen Computer (Hyper-V oder VMware) oder physische Windows/Linux-Server nach Azure replizieren möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Kontoberechtigungen zur Replikation
> * Erstellen eines Azure-Speicherkontos
> * Richten Sie ein Azure-Netzwerk ein. Wenn die Azure-VMs nach einem Failover erstellt werden, werden sie mit diesem Azure-Netzwerk verbunden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter „http://portal.azure.com“ beim Azure-Portal an.

## <a name="verify-account-permissions"></a>Überprüfen von Kontoberechtigungen

Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator Ihres Abonnements. Wenn Sie nicht der Abonnementadministrator sind, wenden Sie sich an den Administrator, damit dieser die erforderlichen Berechtigungen zuweist. Zum Aktivieren der Replikation für einen neuen virtuellen Computer benötigen Sie Folgendes:

- Berechtigung zum Erstellen einer VM in der ausgewählten Ressourcengruppe
- Berechtigung zum Erstellen einer VM im ausgewählten virtuellen Netzwerk
- Berechtigung zum Schreiben in das ausgewählte Speicherkonto

Die integrierte Rolle „Mitwirkender für virtuelle Computer“ verfügt über diese Berechtigungen. Sie benötigen außerdem die Berechtigung zum Verwalten von Azure Site Recovery-Vorgängen. Die Rolle „Site Recovery-Mitwirkender“ verfügt über alle erforderlichen Berechtigungen zum Verwalten von Site Recovery-Vorgängen in einem Recovery Services-Tresor.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Images der replizierten Computer sind in Azure Storage gespeichert. Azure-VMs werden aus dem Speicher erstellt, wenn Sie ein Failover von einem lokalen Standort nach Azure ausführen.

1. Klicken Sie im Menü des [Azure-Portals](https://portal.azure.com) auf **Neu** -> **Storage** -> **Speicherkonto**.
2. Geben Sie unter **Speicherkonto erstellen** einen Namen für das Konto ein. Für diese Tutorials verwenden wir den Namen **contosovmsacct1910171607**. Der Name muss in Azure eindeutig und zwischen 3 und 24 Zeichen lang sein, wobei nur Ziffern und Kleinbuchstaben zulässig sind.
3. Verwenden Sie das **Resource Manager**-Bereitstellungsmodell.
4. Wählen Sie **Allgemein** > **Standard** aus. Wählen Sie keinen Blobspeicher aus.
5. Wählen Sie den **RA-GRS**-Standardwert für die Speicherredundanz aus.
6. Wählen Sie das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.
7. Geben Sie eine neue Ressourcengruppe an. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Für diese Tutorials verwenden wir den Namen **ContosoRG**.
8. Wählen Sie den geografischen Standort für das Speicherkonto aus. Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden. Für diese Tutorials verwenden wir die Region **Europa, Westen**.

   ![create-storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

## <a name="create-a-vault"></a>Erstellen eines Tresors

1. Klicken Sie im Menü im Azure-Portal auf **Neu**>**Überwachung & Verwaltung**>
   **Backup und Site Recovery**.
2. Geben Sie unter **Name**einen Anzeigenamen an, mit dem der Tresor identifiziert wird. In diesem Tutorial verwenden wir **ContosoVMVault**.
3. Wählen Sie die vorhandene Ressourcengruppe mit dem Namen **contosoRG** aus.
4. Geben Sie die Azure-Region **Europa, Westen** an, die wir in diesen Tutorials verwenden.
5. Klicken Sie zum schnellen Zugreifen auf den Tresor über das Dashboard auf **An Dashboard anheften** > **Erstellen**.

   ![Neuer Tresor](./media/tutorial-prepare-azure/new-vault-settings.png)

   Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf der Hauptseite **Recovery Services-Tresore** angezeigt.

## <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Wenn die Azure-VMs nach einem Failover aus dem Speicher erstellt werden, werden sie mit diesem Netzwerk verknüpft.

1. Klicken Sie im Menü im [Azure-Portal](https://portal.azure.com) auf **Neu**>**Netzwerk**>
   **Virtuelles Netzwerk**.
2. Belassen Sie als Auswahl für das Bereitstellungsmodell **Resource Manager**. „Resource Manager“ ist das empfohlene Bereitstellungsmodell.
   - Geben Sie einen Netzwerknamen an. Der Name muss innerhalb der Azure-Ressourcengruppe eindeutig sein. Wir verwenden den Namen **ContosoASRnet**
   - Verwenden Sie die vorhandene Ressourcengruppe **contosoRG**.
   - Geben Sie den Netzwerk-Adressbereich 10.0.0.0/24 an.
   - Für dieses Tutorial benötigen wir kein Subnetz.
   - Wählen Sie das Abonnement aus, in dem das Netzwerk erstellt werden soll.
   - Wählen Sie als Standort **Europa, Westen** aus. Das Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden.
3. Klicken Sie auf **Create**.

   ![create-network](media/tutorial-prepare-azure/create-network.png)

   Die Erstellung des virtuellen Netzwerks dauert ein paar Sekunden. Nach der Erstellung wird es auf dem Dashboard im Azure-Portal angezeigt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorbereiten der lokalen VMware-Infrastruktur für die Notfallwiederherstellung in Azure](tutorial-prepare-on-premises-vmware.md)
