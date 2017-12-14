---
title: Gruppieren von VMs per VMware vCenter-Tagging | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie eine Gruppe erstellen, bevor Sie mit dem Azure Migrate-Dienst eine Bewertung ausführen."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 967c2a1fc0e5144c6c7d5c8c4a81cec3d77ffdb5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2017
---
# <a name="group-vms-with-vcenter-tagging"></a>Gruppieren von VMs mit vCenter-Tagging

In diesem Artikel wird beschrieben, wie Sie eine Gruppe mit Computern für eine [Azure Migrate](migrate-overview.md)-Bewertung per Tagging in VMware vCenter erstellen. Beim Erstellen einer Gruppe geben Sie die gewünschte Kategorie für das Tag an. 

## <a name="set-up-tagging"></a>Einrichten des Tagging

Bei der Bereitstellung von Azure Migrate werden mit einer lokalen Azure Migrate-VM die Computer ermittelt, die auf ESXi-Hosts ausgeführt werden. Die Hosts werden hierbei mit einem vCenter Server verwaltet. Sie müssen das vCenter-Tagging vor dem Ermittlungsprozess durchführen.

1. Navigieren Sie im VMware vSphere-Webclient zum vCenter Server.
2. Klicken Sie auf **Tags**, um alle aktuellen Tags anzuzeigen.
3. Gehen Sie wie folgt vor, um eine VM mit einem Tag zu versehen: Wählen Sie **Zugehörige Objekte** > **Virtuelle Computer** und dann die VM aus, die Sie mit einem Tag versehen möchten.
4. Klicken Sie unter **Zusammenfassung** > **Tags** auf **Zuweisen**. 
5. Klicken Sie auf **Neues Tag**, und geben Sie einen Tagnamen und eine Beschreibung an.
6. Wählen Sie zum Erstellen einer Kategorie für das Tag in der Dropdownliste die Option **Neue Kategorie** aus.
7. Geben Sie einen Kategorienamen und eine Beschreibung sowie die Kardinalität an. Klicken Sie dann auf **OK**.

    ![VM-Tags](./media/how-to-tag-v-center/vm-tag.png)

## <a name="use-tagging-to-create-groups"></a>Verwenden von Tagging zum Erstellen von Gruppen

1. Richten Sie die Ermittlung von lokalen Computern wie im Tutorial zur VMware-Bewertung unter [Discover and assess on-premises VMware VMs for migration to Azure](tutorial-assessment-vmware.md#run-the-collector-to-discover-vms) (Ermitteln und Bewerten von lokalen VMware-VMs für die Migration zu Azure) beschrieben ein.
2. Wählen Sie unter **Tag category for grouping** (Tagkategorie für Gruppierung) die vCenter-Tagkategorie aus, auf der die Bewertungsgruppe basieren soll. Azure Migrate erstellt automatisch eine Gruppe für die gewählte Kategorie.

    

## <a name="next-steps"></a>Nächste Schritte

[Discover and assess on-premises VMware VMs for migration to Azure](tutorial-assessment-vmware.md) (Ermitteln und Bewerten von lokalen VMware-VMs für die Migration zu Azure)
