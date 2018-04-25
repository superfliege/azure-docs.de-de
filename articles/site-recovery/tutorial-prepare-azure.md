---
title: Erstellen von Ressourcen für die Verwendung mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure für die Replikation von lokalen Computern mit Azure Site Recovery vorbereiten.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 0aec94ce4d53e1d0f5ecfbc7c667f7d4ceea1d2d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Vorbereiten von Azure-Ressourcen für die Replikation von lokalen Computern

 [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Tutorial wird gezeigt, wie Sie Azure-Komponenten vorbereiten, wenn Sie lokale virtuelle Computer (Hyper-V oder VMware) oder physische Windows/Linux-Server in Azure replizieren möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen, ob Ihr Azure-Konto über Replikationsberechtigungen verfügt
> * Erstellen eines Azure-Speicherkontos. Darin werden replizierte Daten gespeichert.
> * Erstellen Sie einen Recovery Services-Tresor.
> * Richten Sie ein Azure-Netzwerk ein. Wenn die Azure-VMs nach einem Failover erstellt werden, werden sie mit diesem Azure-Netzwerk verbunden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

## <a name="verify-account-permissions"></a>Überprüfen von Kontoberechtigungen

Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator Ihres Abonnements. Falls Sie nicht der Abonnementadministrator sind, können Sie sich an den Administrator wenden, damit dieser die erforderlichen Berechtigungen zuweist. Zum Aktivieren der Replikation für einen neuen virtuellen Computer benötigen Sie die folgenden Berechtigungen:

- Erstellen einer VM in der ausgewählten Ressourcengruppe
- Erstellen einer VM im ausgewählten virtuellen Netzwerk
- Schreiben in das ausgewählte Speicherkonto

Zum Ausführen dieser Aufgaben muss Ihrem Konto die integrierte Rolle „Mitwirkender von virtuellen Computern“ zugewiesen werden. Zum Verwalten von Site Recovery-Vorgängen in einem Tresor muss Ihrem Konto außerdem die integrierte Rolle „Site Recovery-Mitwirkender“ zugewiesen werden.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Images der replizierten Computer sind in Azure Storage gespeichert. Azure-VMs werden aus dem Speicher erstellt, wenn Sie ein Failover von einem lokalen Standort nach Azure ausführen.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) im Menü auf **Neu** > **Speicher** > **Speicherkonto**.
2. Geben Sie unter **Speicherkonto erstellen** einen Namen für das Konto ein. Verwenden Sie für diese Tutorials den Namen **contosovmsacct1910171607**. Der Name muss in Azure eindeutig und zwischen 3 und 24 Zeichen lang sein, wobei nur Ziffern und Kleinbuchstaben zulässig sind.
3. Wählen Sie unter **Bereitstellungsmodell** die Option **Resource Manager**.
4. Wählen Sie unter **Kontoart** die Option **Allgemein**. Wählen Sie unter **Leistung** die Option **Standard**. Wählen Sie keinen Blobspeicher aus.
5. Wählen Sie unter **Replikation** für die Speicherredundanz die Standardoption **Georedundanter Speicher mit Lesezugriff**.
6. Wählen Sie unter **Abonnement** das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.
7. Geben Sie unter **Ressourcengruppe** eine neue Ressourcengruppe ein. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Verwenden Sie für diese Tutorials den Namen **ContosoRG**.
8. Wählen Sie unter **Standort** den geografischen Standort für das Speicherkonto aus. Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden. Verwenden Sie für diese Tutorials die Region **Europa, Westen**.

   ![Speicherkonto erstellen](media/tutorial-prepare-azure/create-storageacct.png)

9. Wählen Sie **Erstellen**, um das Speicherkonto zu erstellen.

## <a name="create-a-vault"></a>Erstellen eines Tresors

1. Wählen Sie im Azure-Portal die Option **Ressource erstellen** > **Überwachung und Verwaltung** > **Backup und Site Recovery**.
2. Geben Sie unter **Name**einen Anzeigenamen ein, über den der Tresor identifiziert wird. Verwenden Sie für dieses Tutorial den Namen **ContosoVMVault**.
3. Wählen Sie unter **Ressourcengruppe** die vorhandene Ressourcengruppe mit dem Namen **contosoRG** aus.
4. Geben Sie unter **Standort** die Azure-Region **Europa, Westen** ein, die in dieser Tutorialreihe verwendet wird.
5. Wählen Sie **An Dashboard anheften** > **Erstellen**, um über das Dashboard schnell auf den Tresor zuzugreifen.

   ![Erstellen eines neuen Tresors](./media/tutorial-prepare-azure/new-vault-settings.png)

   Der neue Tresor wird unter **Dashboard** > **Alle Ressourcen** und auf der Hauptseite **Recovery Services-Tresore** angezeigt.

## <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

Wenn die Azure-VMs nach einem Failover aus dem Speicher erstellt werden, werden sie mit diesem Netzwerk verknüpft.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**.
2. Belassen Sie als Auswahl für das Bereitstellungsmodell **Resource Manager**. „Resource Manager“ ist das empfohlene Bereitstellungsmodell. Führen Sie anschließend diese Schritte aus:

   a. Geben Sie unter **Name** einen Netzwerknamen ein. Der Name muss innerhalb der Azure-Ressourcengruppe eindeutig sein. Verwenden Sie den Namen **ContosoASRnet**.

   b. Verwenden Sie unter **Ressourcengruppe** die vorhandene Ressourcengruppe **contosoRG**.

   c. Geben Sie unter **Adressbereich** den Netzwerkadressbereich **10.0.0.0/24** ein.

   d. Für dieses Tutorial benötigen Sie kein Subnetz.

   e. Wählen Sie unter **Abonnement** das Abonnement aus, in dem das Netzwerk erstellt werden soll.

   f. Wählen Sie als **Ort** die Option **Europa, Westen** aus. Das Netzwerk muss sich in derselben Region wie der Recovery Services-Tresor befinden.

3. Klicken Sie auf **Erstellen**.

   ![Erstellen eines virtuellen Netzwerks](media/tutorial-prepare-azure/create-network.png)

   Die Erstellung des virtuellen Netzwerks dauert ein paar Sekunden. Nach der Erstellung wird es auf dem Dashboard im Azure-Portal angezeigt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Vorbereiten der lokalen VMware-Infrastruktur für die Notfallwiederherstellung in Azure](tutorial-prepare-on-premises-vmware.md)
