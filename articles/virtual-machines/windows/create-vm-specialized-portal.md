---
title: Erstellen einer Windows-VM auf Grundlage einer speziellen VHD im Azure-Portal | Microsoft-Dokumentation
description: Erstellen einer neuen Windows-VM aus einer VHD im Azure-Portal.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: cynthn
ms.openlocfilehash: 33e94777047ea8a116ae6f393439521356a53509
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2018
---
# <a name="create-a-vm-from-a-vhd-using-the-azure-portal"></a>Erstellen einer VM aus einer VHD mithilfe des Azure-Portals


Es gibt mehrere Methoden zum Erstellen von VMs in Azure. Wenn Sie bereits über eine verwendbare VHD verfügen oder eine vorhandene VHD für die Verwendung kopieren möchten, können Sie eine neue VM erstellen, indem Sie die VHD als Betriebssystemdatenträger anfügen. In diesem Prozess wird die VHD einer neuen VM als Betriebssystemdatenträger *angefügt*.

Sie können eine neue VM auch aus der VHD einer VM erstellen, die gelöscht wurde. Wenn Sie beispielsweise über eine Azure VM verfügen, die nicht ordnungsgemäß funktioniert, können Sie die VM löschen und die VHD verwenden, um eine neue VM zu erstellen. Sie können entweder die gleiche VHD verwenden oder eine Kopie von ihr erstellen, indem Sie eine Momentaufnahme und dann aus dieser einen neuen verwalteten Datenträger erstellen. Dies erfordert zwei weitere Schritte, ermöglicht Ihnen jedoch, die ursprüngliche VHD zu behalten, und gibt Ihnen außerdem eine Momentaufnahme an die Hand, auf die bei Bedarf zurückgegriffen werden kann.

Sie verfügen über eine lokale VM, die Sie verwenden möchten, um eine VM in Azure zu erstellen. Sie können die VHD hochladen und sie an die neue VM anfügen. Um eine VHD hochzuladen, müssen Sie PowerShell oder ein anderes Tool verwenden, um sie in ein Speicherkonto hochzuladen, und dann aus der VHD einen verwalteten Datenträger erstellen. Weitere Informationen finden Sie unter [Hochladen einer speziellen VHD](create-vm-specialized.md#option-2-upload-a-specialized-vhd)

Wenn Sie eine VM oder VHD verwenden möchten, um mehrere VMs zu erstellen, sollten Sie dieses Verfahren nicht verwenden. Für größere Bereitstellungen sollten Sie [ein Image erstellen](capture-image-resource.md) und anschließend [das Image zum Erstellen mehrerer VMs verwenden](create-vm-generalized-managed.md).


## <a name="copy-a-disk"></a>Kopieren eines Datenträgers

Erstellen Sie eine Momentaufnahme, und erstellen Sie dann einen Datenträger aus der Momentaufnahme. Dadurch können Sie die ursprüngliche VHD zum Rückgriff behalten.

1. Klicken Sie im linken Menü auf **Alle Ressourcen**.
2. Deaktivieren Sie im Dropdownfeld **Alle Typen** die Option **Alle auswählen**, scrollen Sie dann nach unten, und aktivieren Sie **Datenträger**, um die verfügbaren Datenträger zu finden.
3. Klicken Sie auf den Datenträger, den Sie verwenden möchten. Die Seite **Übersicht** für den Datenträger wird geöffnet.
4. Klicken Sie auf der Übersichtsseite im Menü am oberen Rand auf **+ Momentaufnahme erstellen**. 
5. Geben Sie einen Namen für die Momentaufnahme ein.
6. Wählen Sie eine **Ressourcengruppe** für die Momentaufnahme aus. Sie können entweder eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen.
7. Wählen Sie, ob Sie Standardspeicher (HDD) oder Premium-Speicher (SDD) verwenden möchten.
8. Wenn Sie fertig sind, klicken Sie auf **Erstellen**, um die Momentaufnahme zu erstellen.
9. Sobald die Momentaufnahme erstellt wurde, klicken Sie im linken Menü auf **+ Ressource erstellen**.
10. Geben Sie in der Suchleiste **verwalteter Datenträger** ein, und wählen Sie in der Liste **Verwaltete Datenträger** aus.
11. Klicken Sie auf der Seite **Verwaltete Datenträger** auf **Erstellen**.
12. Geben Sie einen Namen für den Datenträger ein.
13. Wählen Sie eine **Ressourcengruppe** für den Datenträger aus. Sie können entweder eine vorhandene Ressourcengruppe verwenden oder eine neue erstellen. In dieser Ressourcengruppe werden Sie auch die VM aus dem Datenträger erstellen.
14. Wählen Sie, ob Sie Standardspeicher (HDD) oder Premium-Speicher (SDD) verwenden möchten.
15. Achten Sie unter **Quelltyp** darauf, dass **Momentaufnahme** ausgewählt ist.
16. Wählen Sie im Dropdownfeld **Quellmomentaufnahme** die zu verwendende Momentaufnahme aus.
17. Nehmen Sie alle weiteren Anpassungen nach Bedarf vor, und klicken Sie dann auf **Erstellen**, um den Datenträger zu erstellen.

## <a name="create-a-vm-from-a-disk"></a>Erstellen einer VM aus einem Datenträger

Sobald Sie über die verwaltete Datenträger-VHD verfügen, die Sie verwenden möchten, können Sie die VM im Portal erstellen.

1. Klicken Sie im linken Menü auf **Alle Ressourcen**.
2. Deaktivieren Sie im Dropdownfeld **Alle Typen** die Option **Alle auswählen**, scrollen Sie dann nach unten, und aktivieren Sie **Datenträger**, um die verfügbaren Datenträger zu finden.
3. Klicken Sie auf den Datenträger, den Sie verwenden möchten. Die Seite **Übersicht** für den Datenträger wird geöffnet.
Vergewissern Sie sich auf der Übersichtsseite, dass **DATENTRÄGERZUSTAND** als **Nicht angefügt** aufgelistet wird. Wenn dies nicht der Fall ist, müssen Sie möglicherweise entweder den Datenträger von der VM trennen oder die VM löschen, um den Datenträger freizugeben.
4. Klicken Sie im Menü oben im Bereich auf **+ VM erstellen**.
5. Geben Sie auf der Seite **Grundlagen** für die neue VM einen Namen ein, und wählen Sie entweder eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue.
6. Wählen Sie auf der Seite **Größe** eine VM-Größe aus, und klicken Sie dann auf **Auswählen**.
7. Auf der Seite **Einstellungen** können Sie entweder alle neuen Ressourcen vom Portal erstellen lassen oder ein vorhandenes **Virtuelles Netzwerk** und eine **Netzwerksicherheitsgruppe** auswählen. Das Portal erstellt immer eine neue NIC und eine öffentliche IP-Adresse für die neue VM. 
8. Nehmen Sie ggf. Änderungen an den Überwachungsoptionen vor, und fügen Sie nach Bedarf Erweiterungen hinzu.
9. Klicken Sie anschließend auf **OK**. 
10. Wenn die Überprüfung der VM-Konfiguration erfolgreich war, klicken Sie auf **OK**, um die Bereitstellung zu starten.

## <a name="next-steps"></a>Nächste Schritte

Sie können ebenfalls PowerShell verwenden, um [eine VHD in Azure hochzuladen und eine spezielle VM zu erstellen](create-vm-specialized.md).


