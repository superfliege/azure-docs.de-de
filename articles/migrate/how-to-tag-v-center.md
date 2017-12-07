---
title: Gruppieren von VMs per VMware vCenter-Tagging | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie eine Gruppe erstellen, bevor Sie mit dem Azure Migrate-Dienst eine Bewertung ausführen."
services: migration-planner
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: db33e31cdef143aa70809442457e447446a1681a
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2017
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
