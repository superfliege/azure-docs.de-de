---
title: "Erweitern Sie die Betriebssystem-Datenträger für Linux-VM mit dem Azure-CLI-1.0 | Microsoft Docs"
description: "Informationen Sie zum Erweitern der virtuellen Datenträger (BS) auf einer Linux-VM mit der Azure-CLI-1.0 und der Ressourcen-Manager-Bereitstellungsmodell"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0aedcd70b54c2ed47ec327ccf0529a48351353c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-the-azure-cli-with-the-azure-cli-10"></a>Erweitern Sie die Betriebssystem-Datenträger für einen Linux-VM, die mithilfe der Azure-CLI mit dem Azure-CLI-1.0
Die Standardgröße für die virtuelle Festplatte für das Betriebssystem (BS) befindet sich normalerweise 30 GB auf einem virtuellen Linux-Computer (VM) in Azure. Sie können [Hinzufügen von Datenträgern](add-disk.md) verliehen wird zusätzlicher Speicherplatz, aber Sie können auch möchten, um den Betriebssystem-Datenträger zu erweitern. In diesem Artikel erläutert, wie den Betriebssystemdatenträger für eine nicht verwaltete Datenträger mit dem Azure-CLI-1.0 mit Linux-VM zu erweitern.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen, die Aufgabe abgeschlossen
Führen Sie die Aufgabe, die mit einer der folgenden CLI-Versionen:

- [Azure-CLI 1.0](#prerequisites) – unsere CLI für den klassischen und Ressource Management Bereitstellungsmodelle (in diesem Artikel)
- [Azure-CLI-2.0](expand-disks.md) -unsere nächste Generation CLI für das ressourcenbereitstellungsmodell für die Verwaltung

## <a name="prerequisites"></a>Voraussetzungen
Müssen Sie die [neuesten Azure-CLI-1.0](../../cli-install-nodejs.md) installiert und bei angemeldet ein [Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) mithilfe des Ressourcen-Manager-Modus wie folgt:

```azurecli
azure config mode arm
```

Ersetzen Sie in den folgenden Beispielen Beispielnamen für die Parameter durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten *MyResourceGroup* und *"MyVM"*.

## <a name="expand-os-disk"></a>Erweitern Sie die Betriebssystem-Datenträger

1. Vorgänge für virtuelle Festplatten mit dem virtuellen Computer mit nicht möglich. Im folgenden Beispiel wird beendet, und hebt die Zuordnung des virtuellen Computers mit dem Namen *"MyVM"* in der Ressourcengruppe mit dem Namen *MyResourceGroup*:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop`die Compute-Ressourcen werden nicht freigegeben werden. Um Serverressourcen freizugeben, verwenden `azure vm deallocate`. Der virtuelle Computer muss freigegeben werden, um die virtuelle Festplatte zu erweitern.

2. Aktualisieren Sie die Größe des nicht verwalteten OS Datenträger mithilfe der `azure vm set` Befehl. Das folgende Beispiel aktualisiert die virtuellen Computer namens *"MyVM"* in der Ressourcengruppe mit dem Namen *MyResourceGroup* werden *50* GB:

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. Starten Sie Ihren virtuellen Computer wie folgt:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH eine Verbindung mit Ihrem virtuellen Computer mit den entsprechenden Anmeldeinformationen. Verwenden Sie zum Überprüfen der Betriebssystemdatenträger Größe `df -h`. Die folgende Beispielausgabe zeigt die primäre Partition (*/Dev/sda1*) ist jetzt 50 GB:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie zusätzlichen Speicher benötigen Sie auch [fügen Sie Datenträger hinzu, um eine Linux-VM](add-disk.md). Weitere Informationen zu datenträgerverschlüsselung, finden Sie unter [Verschlüsseln von Datenträger auf einem Linux-VM mithilfe der Azure-CLI](encrypt-disks.md).
