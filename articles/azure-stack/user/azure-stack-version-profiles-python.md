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
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: d17ba9ed4548a986d6846d934aee197609ec80ca
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806835"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Verwenden von API-Versionsprofilen mit Python in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

## <a name="python-and-api-version-profiles"></a>Python und API-Versionsprofile

Das Python SDK unterstützt API-Versionsprofile für unterschiedliche Cloudplattformen, z.B. Azure Stack und globales Azure. Sie können API-Profile beim Erstellen von Lösungen für eine Hybrid Cloud nutzen. Das Python SDK unterstützt die folgenden API-Profile:

1. **Neueste**  
    Das Profil ist auf die aktuellsten API-Versionen für alle Dienstanbieter auf der Azure Platform ausgerichtet.
2.  **2017-03-09-profile**  
    **2017-03-09-profile**  
    Das Profil ist auf die API-Versionen der von Azure Stack unterstützten Ressourcenanbieter ausgerichtet.

    Weitere Informationen zu API-Profilen und Azure Stack finden Sie unter [Verwalten von API-Versionsprofilen in Azure Stack](azure-stack-version-profiles.md).

## <a name="install-azure-python-sdk"></a>Installieren des Azure Python SDK

1.  Installieren Sie Git über die [offizielle Website](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2.  Eine Anleitung zum Installieren des Python SDK finden Sie unter [Azure für Python-Entwickler](https://docs.microsoft.com/python/azure/python-sdk-azure-install?view=azure-python).
3.  Erstellen Sie ein Abonnement, wenn keins verfügbar ist, und speichern Sie die Abonnement-ID zur späteren Verwendung. Eine Anleitung zum Erstellen eines Abonnements finden Sie unter [Erstellen von Abonnements für Angebote in Azure Stack](../azure-stack-subscribe-plan-provision-vm.md). 
4.  Erstellen Sie einen Dienstprinzipal, und speichern Sie die ID und das Geheimnis. Eine Anleitung zum Erstellen eines Dienstprinzipals für Azure Stack finden Sie unter [Bereitstellen des Anwendungszugriffs auf Azure Stack](../azure-stack-create-service-principals.md). 
5.  Stellen Sie sicher, dass Ihr Dienstprinzipal über die Rolle „Mitwirkender“ bzw. „Besitzer“ für Ihr Abonnement verfügt. Eine Anleitung zum Zuweisen von Rollen zum Dienstprinzipal finden Sie unter [Bereitstellen des Anwendungszugriffs auf Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Voraussetzungen

Um das Python Azure SDK mit Azure Stack zu verwenden, müssen Sie die folgenden Werte angeben und dann Werte mit Umgebungsvariablen festlegen. In der Anleitung unter der Tabelle für Ihr Betriebssystem ist angegeben, wie Sie die Umgebungsvariablen festlegen. 

| Wert | Umgebungsvariablen | BESCHREIBUNG |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| Mandanten-ID | AZURE_TENANT_ID | Der Wert Ihrer [Mandanten-ID](../azure-stack-identity-overview.md) für Azure Stack. |
| Client-ID | AZURE_CLIENT_ID | Die Anwendungs-ID des Dienstprinzipals, die beim Erstellen des Dienstprinzipals im vorherigen Abschnitt dieses Dokuments gespeichert wurde. |
| Abonnement-ID | AZURE_SUBSCRIPTION_ID | Mit der [Abonnement-ID](../azure-stack-plan-offer-quota-overview.md#subscriptions) greifen Sie in Azure Stack auf Angebote zu. |
| Geheimer Clientschlüssel | AZURE_CLIENT_SECRET | Die Geheimnisanwendung des Dienstprinzipals, die bei der Erstellung des Dienstprinzipals gespeichert wurde. |
| Resource Manager-Endpunkt | ARM_ENDPOINT | Siehe [Azure Stack-Resource Manager-Endpunkt](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |


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

5.  Erstellen Sie einen [Dienstprinzipal](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) zur Zusammenarbeit mit Azure Stack. Stellen Sie sicher, dass Ihr Dienstprinzipal über die Rolle [„Mitwirkender“ bzw. „Besitzer“](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) für Ihr Abonnement verfügt.

6.  Legen Sie die folgenden Variablen fest, und exportieren Sie diese Umgebungsvariablen in Ihre aktuelle Shell. 

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  In order to run this sample, Ubuntu 16.04-LTS and WindowsServer 2012-R2-Datacenter images must be present in Azure Stack market place. These can be either [downloaded from Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) or [added to Platform Image Repository](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image).

8. Run the sample.

    ```
    python unmanaged-disks\example.py
    ```

## Notes

You may be tempted to try to retrieve a VM's OS disk by using
`virtual_machine.storage_profile.os_disk`.
In some cases, this may do what you want,
but be aware that it gives you an `OSDisk` object.
In order to update the OS Disk's size, as `example.py` does,
you need not an `OSDisk` object but a `Disk` object.
`example.py` gets the `Disk` object with the following:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## Next steps

- [Azure Python Development Center](https://azure.microsoft.com/develop/python/)
- [Azure Virtual Machines documentation](https://azure.microsoft.com/services/virtual-machines/)
- [Learning Path for Virtual Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- If you don't have a Microsoft Azure subscription, you can get a FREE trial account [here](http://go.microsoft.com/fwlink/?LinkId=330212).