---
title: "Beibehalten von Dateien für Bash in Azure Cloud Shell (Vorschauversion) | Microsoft-Dokumentation"
description: "Exemplarische Vorgehensweise für Bash zum Beibehalten von Dateien in Azure Cloud Shell."
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: juluk
ms.openlocfilehash: 8b8a82e1c1328d952a85ea5afd975a95f5a6e740
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-storage-works"></a>Funktionsweise von Cloud Shell-Speicher 
Cloud Shell nutzt die beiden folgenden Methoden zum Beibehalten von Dateien: 
* Es wird ein Datenträgerimage Ihres Verzeichnisses `$Home` erstellt, um alle Inhalte im Verzeichnis beizubehalten. Das Datenträgerimage wird in der von Ihnen angegebenen Dateifreigabe als `acc_<User>.img` unter `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img` gespeichert, und die Änderungen werden automatisch synchronisiert. 
* Ihre angegebene Dateifreigabe wird zur direkten Interaktion mit der Freigabe als `clouddrive` in Ihrem Verzeichnis `$Home` eingebunden. `/Home/<User>/clouddrive` wird `fileshare.storage.windows.net/fileshare` zugeordnet.
 
> [!NOTE]
> Alle Dateien in Ihrem Verzeichnis `$Home`, z.B. SSH-Schlüssel, werden dauerhaft in Ihrem Benutzerdatenträgerimage beibehalten, das in der eingebundenen Dateifreigabe gespeichert ist. Nutzen Sie beim dauerhaften Speichern von Informationen in Ihrem Verzeichnis `$Home` und auf der eingebundenen Dateifreigabe die bewährten Methoden.

## <a name="use-the-clouddrive-command"></a>Verwenden des Befehls `clouddrive`
Mit Bash in Cloud Shell können Sie den Befehl `clouddrive` ausführen, mit dem Sie die in Cloud Shell bereitgestellte Dateifreigabe manuell aktivieren können.
![Ausführen des Befehls „clouddrive“](media/persisting-shell-storage/clouddrive-h.png)

## <a name="mount-a-new-clouddrive"></a>Einbinden eines neuen clouddrive-Verzeichnisses

### <a name="prerequisites-for-manual-mounting"></a>Voraussetzungen für die manuelle Bereitstellung
Sie können die Dateifreigabe aktualisieren, die Cloud Shell zugeordnet ist, indem Sie den Befehl `clouddrive mount` verwenden.

Wenn Sie eine vorhandene Dateifreigabe bereitstellen, müssen die Speicherkonten folgende Voraussetzungen erfüllen:
* Es muss sich um lokal redundanten Speicher oder georedundanten Speicher zur Unterstützung von Dateifreigaben handeln.
* Das Speicherkonto muss sich in Ihrer zugewiesenen Region befinden. Beim Onboarding wird die Region, der Sie zugewiesen sind, in der Ressourcengruppe mit dem Namen `cloud-shell-storage-<region>` aufgelistet.

### <a name="the-clouddrive-mount-command"></a>Der Clouddrive-Befehl zum Einbinden

> [!NOTE]
> Wenn Sie eine neue Dateifreigabe einbinden, wird für Ihr `$Home`-Verzeichnis ein neues Benutzerimage erstellt. Ihre vorheriges `$Home`-Image wird in Ihrer vorherigen Dateifreigabe gespeichert.

Führen Sie den Befehl `clouddrive mount` mit den folgenden Parametern aus:

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

Führen Sie `clouddrive mount -h` wie hier gezeigt aus, um weitere Details anzuzeigen:

![Ausführen des Befehls „clouddrive mount“](media/persisting-shell-storage/mount-h.png)

## <a name="unmount-clouddrive"></a>Aufheben der Einbindung von clouddrive
Sie können die Bereitstellung einer Dateifreigabe in Cloud Shell jederzeit aufheben. Cloud Shell erfordert jedoch eine eingebundene Dateifreigabe, daher werden Sie aufgefordert, in der nächsten Sitzung eine andere Dateifreigabe zu erstellen und einzubinden.

1. Führen Sie `clouddrive unmount`aus.
2. Lesen und bestätigen Sie die Eingabeaufforderungen.

Ihre Dateifreigabe ist weiterhin vorhanden, sofern Sie sie nicht manuell löschen. Cloud Shell sucht in nachfolgenden Sitzungen nicht mehr nach dieser Dateifreigabe. Führen Sie `clouddrive unmount -h` wie hier gezeigt aus, um weitere Details anzuzeigen:

![Ausführen des Befehls „clouddrive unmount“](media/persisting-shell-storage/unmount-h.png)

> [!WARNING]
> Beim Ausführen dieses Befehls werden keine Ressourcen gelöscht, aber beim manuellen Löschen einer Ressourcengruppe, eines Speicherkontos oder einer Dateifreigabe mit Zuordnung zu Cloud Shell werden Ihr Datenträgerimage im Verzeichnis `$Home` und alle Dateien in Ihrer Dateifreigabe gelöscht. Diese Aktion kann nicht rückgängig gemacht werden.

## <a name="list-clouddrive"></a>Auflisten von `clouddrive`
Führen Sie den `df`-Befehl aus, um zu ermitteln, welche Dateifreigabe als `clouddrive` bereitgestellt wird. 

Der Dateipfad zu „clouddrive“ zeigt den Namen Ihres Speicherkontos und die Dateifreigabe in der URL. Beispiel: `//storageaccountname.file.core.windows.net/filesharename`

```
justin@Azure:~$ df
Filesystem                                          1K-blocks   Used  Available Use% Mounted on
overlay                                             29711408 5577940   24117084  19% /
tmpfs                                                 986716       0     986716   0% /dev
tmpfs                                                 986716       0     986716   0% /sys/fs/cgroup
/dev/sda1                                           29711408 5577940   24117084  19% /etc/hosts
shm                                                    65536       0      65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName 5368709120    64 5368709056   1% /home/justin/clouddrive
justin@Azure:~$
```

[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Nächste Schritte
[Cloud Shell-Schnellstart](quickstart.md) <br>
[Erfahren Sie mehr über Azure File Storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage). <br>
[Weitere Informationen zu Speichertags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>
