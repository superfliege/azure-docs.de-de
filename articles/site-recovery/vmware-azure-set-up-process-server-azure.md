---
title: Einrichten eines Prozessservers in Azure für das Failback von VMware-VMs und physischen Servern mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie in Azure ein Prozessserver für ein Failback von Azure-VMs zu VMware eingerichtet wird.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 20c3642bb9f9ad5089c37bb6da8562e570896cb4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308479"
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Einrichten eines Prozessservers für das Failback in Azure

Nach dem Failover von VMware-VMs oder physischen Servern in Azure mit [Site Recovery](site-recovery-overview.md) können Sie ein Failback zurück zum lokalen Standort ausführen, wenn dieser wieder in Betrieb ist. Für das Failback müssen Sie in Azure einen temporären Prozessserver einrichten, der die Replikation von Azure zum lokalen Standort verarbeitet. Sie können diesen virtuellen Computer nach Abschluss des Failbacks löschen.

## <a name="before-you-start"></a>Vorbereitung

Erfahren Sie mehr über den [erneuten Schutz](vmware-azure-reprotect.md) und das [Failback](vmware-azure-failback.md).

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-in-azure"></a>Bereitstellen eines Prozessservers in Azure

1. Wählen Sie im Tresor unter **Site Recovery-Infrastruktur**> **Verwalten** > **Konfigurationsserver** den Konfigurationsserver aus.
2. Klicken Sie auf der Serverseite auf **+ Prozessserver**.
3. Wählen Sie auf der Seite **Prozessserver hinzufügen** die Bereitstellung eines Prozessservers in Azure aus.
4. Geben Sie die Azure-Einstellungen an, einschließlich des Abonnements für das Failover, einer Ressourcengruppe, der Azure-Region für das Failover und des virtuellen Netzwerks, in dem sich die virtuellen Azure-Computer befinden. Wenn Sie mehrere Azure-Netzwerke verwendet haben, benötigen Sie in jedem einen Prozessserver.

  ![Prozessserver-Katalogelement hinzufügen](./media/vmware-azure-set-up-process-server-azure/add-ps-page-1.png)

4. Geben Sie in **Servername**, **Benutzername** und **Kennwort** einen Namen für den Prozessserver und die Anmeldeinformationen (mit Administratorberechtigungen auf dem Server) an.
5. Geben Sie ein Speicherkonto, das für die Datenträger der Server-VMs verwendet werden soll, das Subnetz, in dem sich die Prozessserver-VM befindet, und die Server-IP-Adresse, die beim Starten des virtuellen Computers zugewiesen wird, an.
6. Klicken Sie auf die Schaltfläche **OK**, um mit der Bereitstellung der Prozessserver-VM zu beginnen.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registrieren des Prozessservers (in Azure ausgeführt) bei einem Konfigurationsserver (lokal ausgeführt)

Nachdem die Prozessserver-VM in Betrieb genommen wurde, müssen Sie sie beim lokalen Konfigurationsserver registrieren. Gehen Sie dazu wie folgt vor:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


