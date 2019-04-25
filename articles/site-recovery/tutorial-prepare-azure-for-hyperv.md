---
title: Vorbereiten von Azure für die Notfallwiederherstellung lokaler virtueller Hyper-V-Computer mit Azure Site Recovery | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure für die Notfallwiederherstellung lokaler virtueller Hyper-V-Computer mit Azure Site Recovery vorbereiten.
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 48101e49429225018381ed2a3b1e8e4e351c15a6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59362778"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Vorbereiten von Azure-Ressourcen für die Notfallwiederherstellung von lokalen Computern

 [Azure Site Recovery](site-recovery-overview.md) unterstützt Ihre Strategien für Geschäftskontinuität und Notfallwiederherstellung, indem die Verfügbarkeit Ihrer Geschäftsanwendungen bei geplanten und ungeplanten Ausfällen gewährleistet wird. Site Recovery verwaltet und koordiniert die Notfallwiederherstellung von lokalen Computern sowie virtuellen Azure-Computern (VMs), einschließlich Replikation, Failover und Wiederherstellung.

In diesem Artikel ist das erste Tutorial in einer Reihe, welche die Einrichtung der Notfallwiederherstellung für lokale virtuelle Computer veranschaulicht. Er ist relevant, wenn Sie virtuelle Hyper-V-Computer schützen möchten.

> [!NOTE]
> Tutorials dienen zur Veranschaulichung des einfachsten Bereitstellungspfads für ein Szenario. Sie verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Eine ausführliche Anleitung finden Sie jeweils im Abschnitt mit der **Gewusst wie-Anleitung** für das entsprechende Szenario.

In diesem Artikel wird gezeigt, wie Sie Azure-Komponenten vorbereiten, wenn Sie lokale virtuelle Computer (Hyper-V) in Azure replizieren möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen, ob Ihr Azure-Konto über Replikationsberechtigungen verfügt
> * Erstellen eines Azure-Speicherkontos Images von replizierten Computern werden in diesem Konto gespeichert.
> * Erstellen Sie einen Recovery Services-Tresor. Ein Tresor enthält Metadaten und Konfigurationsinformationen für virtuelle Computer und andere Replikationskomponenten.
> * Richten Sie ein Azure-Netzwerk ein. Wenn die Azure-VMs nach einem Failover erstellt werden, werden sie mit diesem Azure-Netzwerk verbunden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an.

## <a name="verify-account-permissions"></a>Überprüfen von Kontoberechtigungen

Wenn Sie gerade ein kostenloses Azure-Konto erstellt haben, sind Sie der Administrator Ihres Abonnements. Falls Sie nicht der Abonnementadministrator sind, können Sie sich an den Administrator wenden, damit dieser die erforderlichen Berechtigungen zuweist. Zum Aktivieren der Replikation für einen neuen virtuellen Computer benötigen Sie die folgenden Berechtigungen:

- Erstellen einer VM in der ausgewählten Ressourcengruppe
- Erstellen einer VM im ausgewählten virtuellen Netzwerk
- Schreiben in das ausgewählte Speicherkonto

Zum Ausführen dieser Aufgaben muss Ihrem Konto die integrierte Rolle „Mitwirkender von virtuellen Computern“ zugewiesen werden. Zum Verwalten von Site Recovery-Vorgängen in einem Tresor muss Ihrem Konto außerdem die integrierte Rolle „Site Recovery-Mitwirkender“ zugewiesen werden.

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Images der replizierten Computer sind in Azure Storage gespeichert. Azure-VMs werden aus dem Speicher erstellt, wenn Sie ein Failover von einem lokalen Standort nach Azure ausführen. Das Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden. In diesem Tutorial verwenden wir „Europa, Westen“.

1. Wählen Sie im Menü des [Azure-Portals](https://portal.azure.com) **Ressource erstellen** > **Speicher** > **Speicherkonto – Blob, Datei, Tabelle, Warteschlange**.
2. Geben Sie unter **Speicherkonto erstellen** einen Namen für das Konto ein. Verwenden Sie für diese Tutorials **contosovmsacct1910171607**. Der ausgewählte Name muss in Azure eindeutig und zwischen 3 und 24 Zeichen lang sein, wobei nur Ziffern und Kleinbuchstaben zulässig sind.
3. Wählen Sie unter **Bereitstellungsmodell** die Option **Resource Manager**.
4. Wählen Sie unter **Kontoart** die Option **Speicher (universell v1)** aus. Wählen Sie keinen Blobspeicher aus.
5. Wählen Sie unter **Replikation** für die Speicherredundanz die Standardoption **Georedundanter Speicher mit Lesezugriff**. Wir lassen die Option **Sichere Übertragung erforderlich** **deaktiviert**.
6. Wählen Sie unter **Leistung** die Option **Standard** und unter **Zugriffsebene** die Standardoption **Heiße Ebene** aus.
7. Wählen Sie unter **Abonnement** das Abonnement aus, in dem Sie das neue Speicherkonto erstellen möchten.
8. Geben Sie unter **Ressourcengruppe** eine neue Ressourcengruppe ein. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Für diese Tutorials verwenden wir **ContosoRG**.
9. Wählen Sie unter **Standort** den geografischen Standort für das Speicherkonto aus. 

   ![Speicherkonto erstellen](media/tutorial-prepare-azure/create-storageacct.png)

9. Wählen Sie **Erstellen**, um das Speicherkonto zu erstellen.

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

Wenn die Azure-VMs nach einem Failover aus dem Speicher erstellt werden, werden sie mit diesem Netzwerk verknüpft.

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
> [Vorbereiten lokaler Hyper-V-Server für die Notfallwiederherstellung in Azure](hyper-v-prepare-on-premises-tutorial.md)
