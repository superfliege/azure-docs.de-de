---
title: Startvorgang des virtuellen Azure-Computers bleibt bei Windows-Update hängen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Problem beheben, wenn der Startvorgang eines virtuellen Azure-Computers bei Windows-Update(s) hängen bleibt.
services: virtual-machines-windows
documentationCenter: ''
authors: genli
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/09/2018
ms.author: genli
ms.openlocfilehash: 2d42d2014432b72f35e9b0d9543fe499a6ab721b
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355219"
---
# <a name="azure-vm-startup-is-stuck-at-windows-update"></a>Startvorgang des virtuellen Azure-Computers bleibt bei Windows-Update hängen

In diesem Artikel wird beschrieben, wie Sie das Problem beheben, wenn Ihr virtueller Computer (Virtual Machine, VM) beim Startvorgang in der Windows Update-Phase hängen bleibt. 

> [!NOTE] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel behandelt die Verwendung des Ressourcen-Manager-Bereitstellungsmodells. Es wird empfohlen, dieses Modell anstelle des klassischen Bereitstellungsmodells für neue Bereitstellungen zu verwenden.

 ## <a name="symptom"></a>Symptom

 Eine Windows-VM wird nicht gestartet. Beim Überprüfen der Screenshots im Fenster [Startdiagnose](../troubleshooting/boot-diagnostics.md) sehen Sie, dass der Start während des Updatevorgangs hängen bleibt. Im Folgenden finden Sie Beispiele für Meldungen, die angezeigt werden können:

- Windows ##% wird installiert. Schalten Sie den Computer nicht aus. Dies kann eine Weile dauern. Ihr PC wird mehrmals neu gestartet.
- Lassen Sie den PC bis zum Abschluss dieses Vorgangs eingeschaltet. Update # von # wird installiert... 
- Einrichtung der Updates konnte nicht abgeschlossen werden. Änderungen werden rückgängig gemacht. Schalten Sie den Computer nicht aus.
- Fehler beim Konfigurieren der Windows-Updates. Änderungen werden rückgängig gemacht. Schalten Sie den Computer nicht aus.
- Fehler <Fehlercode> beim Updatevorgang ##### von ##### (\Regist...)
- Schwerer Fehler <Fehlercode> beim Updatevorgang ##### von ##### ($$...)


## <a name="solution"></a>Lösung

Abhängig von der Anzahl der Updates, die installiert werden oder für die ein Rollback ausgeführt wird, kann der Updatevorgang eine Weile dauern. Lassen Sie den virtuellen Computer 8 Stunden lang in diesem Zustand. Wenn sich der virtuelle Computer nach Ablauf dieses Zeitraums immer noch in diesem Zustand befindet, starten Sie den virtuellen Computer über das Azure-Portal neu, und testen Sie, ob er normal gestartet werden kann. Wenn dieser Schritt nicht funktioniert, versuchen Sie, das Problem mithilfe der folgenden Lösung zu beheben.

### <a name="remove-the-update-that-causes-the-problem"></a>Entfernen des Updates, das das Problem verursacht

1. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des betroffenen virtuellen Computers als Sicherung. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Datenträgers](../windows/snapshot-copy-managed-disk.md). 
2. [Fügen Sie den Betriebssystemdatenträger an einen virtuellen Computer für die Wiederherstellung an](troubleshoot-recovery-disks-portal-windows.md).
3. Nachdem Sie den Betriebssystemdatenträger an den Wiederherstellungs-VM angefügt haben, öffnen Sie die **Datenträgerverwaltung**, und vergewissern Sie sich, dass er **ONLINE** ist. Notieren Sie sich den Laufwerkbuchstaben, der dem angefügten Betriebssystemdatenträger zugewiesen ist, der den Ordner „\Windows“ enthält. Wenn der Datenträger verschlüsselt ist, entschlüsseln Sie den Datenträger, bevor Sie mit den nächsten Schritten in diesem Dokument fortfahren.

3. Rufen Sie die Liste der Updatepakete ab, die sich auf dem angefügten Betriebssystemdatenträger befinden:

        dism /image:<Attached OS disk>:\ /get-packages > c:\temp\Patch_level.txt

    Ist dem angefügten Betriebssystemdatenträger beispielsweise Laufwerk „F“ zugeordnet, führen Sie den folgenden Befehl aus:

        dism /image:F:\ /get-packages > c:\temp\Patch_level.txt
4. Öffnen Sie die Datei „C:\temp\Patch_level.txt“, und lesen Sie die Datei von unten nach oben. Suchen Sie nach dem Update mit dem Status **Installation steht aus** oder **Deinstallation steht aus**.  Im Folgenden finden Sie ein Beispiel für den Updatestatus:

     ```
    Package Identity : Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    State : Install Pending
    Release Type : Security Update
    Install Time :
    ```
5. Entfernen Sie das Update, das das Problem verursacht:
    
    ```
    dism /Image:<Attached OS disk>:\ /Remove-Package /PackageName:<PACKAGE NAME TO DELETE>
    ```
    Beispiel: 

    ```
    dism /Image:F:\ /Remove-Package /Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    ```

    > [!NOTE] 
    > Abhängig von der Größe des Pakets benötigt das DISM-Tool eine Weile, um die Deinstallation zu verarbeiten. Normalerweise wird der Vorgang innerhalb von 16 Minuten abgeschlossen.

6. Trennen Sie den Betriebssystemdatenträger, und [erstellen Sie dann den virtuellen Computer mithilfe des Betriebssystemdatenträgers neu](troubleshoot-recovery-disks-portal-windows.md). 