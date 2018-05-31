---
title: Verwenden von API-Versionsprofilen mit Python in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie mehr zur Verwendung von API-Versionsprofilen mit Python in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: mabrigg
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: a4fe62ba8c0732745326831b977e8975e1210436
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2018
ms.locfileid: "32310298"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Verwenden von API-Versionsprofilen mit Python in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

## <a name="python-samples-for-azure-stack"></a>Python-Beispiele für Azure Stack 

Mit den folgenden Codebeispielen können Sie allgemeine Verwaltungsaufgaben für virtuelle Computer in Ihrer Azure Stack-Instanz ausführen.

Die Codebeispiele zeigen Ihnen Folgendes:

- Erstellen von virtuellen Computern:
    - Erstellen einer virtuellen Linux-Maschine
    - Erstellen eines virtuellen Windows-Computers
- Aktualisieren eines virtuellen Computers:
    - Erweitern eines Laufwerks
    - Markieren eines virtuellen Computers
    - Anfügen von Datenträgern für Daten
    - Trennen von Datenträgern
- Betreiben eines virtuellen Computers:
    - Starten eines virtuellen Computers
    - Dient zum Beenden eines virtuellen Computers.
    - Neustarten eines virtuellen Computers
- Auflisten von virtuellen Computern
- Löschen eines virtuellen Computers

Um den Code zum Ausführen dieser Vorgänge zu überprüfen, ziehen Sie die **run_example()**-Funktion im Python-Skript **Hybrid/unmanaged-disks/example.py** im GitHub-Repository [virtual-machines-python-manage](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88) zu Rate.

Jeder Vorgang ist deutlich mit einem Kommentar und einer Druckfunktion gekennzeichnet.
Die Beispiele befinden sich nicht unbedingt in der Reihenfolge, die in der obigen Liste angezeigt wird.


## <a name="run-the-python-sample"></a>Ausführen des Python-Beispiels

1.  [Installieren Sie Python](https://www.python.org/downloads/), falls Sie es noch nicht getan haben.

    Dieses Beispiel (und das SDK) ist mit Python 2.7, 3.4, 3.5 und 3.6 kompatibel.

2.  Allgemein wird für die Python-Entwicklung die Verwendung einer virtuellen Umgebung empfohlen. 
    Weitere Informationen finden Sie unter https://docs.python.org/3/tutorial/venv.html.
    
    Installieren und initialisieren Sie die virtuelle Umgebung mit dem „venv“-Modul auf Python 3 (Sie müssen [virtualenv](https://pypi.python.org/pypi/virtualenv) für Python 2.7 installieren):

    ````bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ````

3.  Klonen Sie das Repository.

    ````bash
    git clone https://github.com/Azure-Samples/virtual-machines-python-manage.git
    ````

4.  Installieren Sie die Abhängigkeiten mit „pip“.

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  Erstellen Sie einen [Dienstprinzipal](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals) zur Zusammenarbeit mit Azure Stack. Stellen Sie sicher, dass Ihr Dienstprinzipal über die Rolle [„Mitwirkender“ bzw. „Besitzer“](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) für Ihr Abonnement verfügt.

6.  Legen Sie die folgenden Variablen fest, und exportieren Sie diese Umgebungsvariablen in Ihre aktuelle Shell. 

`Note: provide an explanation of where these variables come from?`

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Beachten Sie, dass zur Ausführung dieses Beispiels Ubuntu 16.04-LTS- und Windows Server 2012-R2-Datencenter-Images im Azure Stack-Marktplatz vorhanden sein müssen. Diese können entweder [aus Azure heruntergeladen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-download-azure-marketplace-item) oder [dem Plattform Image Repository hinzugefügt](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-vm-image) werden.


8. Führen Sie das Beispiel aus.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Notizen

Möglicherweise möchten Sie versuchen, den Betriebssystem-Datenträger eines virtuellen Computers mit `virtual_machine.storage_profile.os_disk` abzurufen.
In einigen Fällen können Sie damit Ihr Ziel erreichen, aber beachten Sie, dass Sie so ein `OSDisk`-Objekt erhalten.
Zum Aktualisieren der Größe des Betriebssystemdatenträgers, wie von `example.py` ausgeführt, benötigen Sie kein `OSDisk`-Objekt, sondern ein `Disk`-Objekt.
`example.py` ruft das `Disk`-Objekt mit Folgendem ab:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Python Developer Center](https://azure.microsoft.com/develop/python/)
- [Dokumentation zu virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/)
- [Lernpfad für Virtual Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Wenn Sie über kein Microsoft Azure-Abonnement verfügen, können Sie [hier](http://go.microsoft.com/fwlink/?LinkId=330212) ein KOSTENLOSES Testkonto erstellen.
