---
title: Includedatei
description: Includedatei
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ec8fa6c06dff0091627a800c895d45fd3b0e778e
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2018
ms.locfileid: "53381492"
---
# <a name="enabling-azure-ultra-ssds"></a>Aktivieren von Azure Ultra SSDs

Azure SSD Ultra bietet hohen Durchsatz, hohe IOPS und konsistenten Datenträgerspeicher mit niedrigen Wartezeiten für Azure IaaS-VMs. Dieses neue Angebot bietet Spitzenleistung auf den gleichen Verfügbarkeitsebenen wie unsere vorhandenen Datenträgerangebote. Zu den weiteren Vorteilen von SSD Ultra gehört die Möglichkeit, die Datenträgerleistung dynamisch in Abstimmung mit Ihren Workloads ändern zu können, ohne Ihre virtuellen Computer neu starten zu müssen. SSD Ultra eignet sich für datenintensive Workloads wie SAP HANA, führende Datenbanken und Workloads mit vielen Transaktionen.

Aktuell befinden sich Ultra SSDs in der Vorschauphase, und Sie müssen sich bei der Preview [registrieren](https://aka.ms/UltraSSDPreviewSignUp), um darauf zuzugreifen.

Nach der Genehmigung, führen Sie einen der folgenden Befehle aus, um zu bestimmen, in welcher Zone in „USA, Osten 2“ Ihr SSD Ultra bereitgestellt werden soll:

PowerShell: `Get-AzureRmComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query “[?name==’UltraSSD_LRS’]”`

Die Antwort wird dem unten gezeigten Formular ähneln, wobei X für die Zone steht, die für die Bereitstellung in „USA, Osten 2“ verwendet werden soll. X kann entweder 1, 2 oder 3 sein.

|ResourceType  |NAME  |Standort  |Zones  |Einschränkung  |Funktion  |Wert  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

Wenn der Befehl keine Antwort zurückgegeben hat, bedeutet das, dass Ihre Registrierung bei dem Feature entweder noch aussteht oder noch nicht genehmigt ist.

Nachdem Sie nun wissen, in welcher Zone bereitgestellt werden soll, führen Sie die Bereitstellungsschritte in diesem Artikel aus, um Ihre ersten virtuellen Computer mit Ultra SSD-Datenträgern bereitzustellen.

## <a name="deploying-an-ultra-ssd"></a>Bereitstellen einer Ultra SSD

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Im Rahmen dieser Preview werden nur VMs der Familien DsV3 und EsV3 unterstützt. Zusätzliche Details zu diesen VM-Größen finden Sie in der zweiten Tabelle in diesem [Blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).
Ferner finden Sie Informationen in dem Beispiel [Erstellen eines virtuellen Computers mit mehreren Ultra SSD-Datenträgern](https://aka.ms/UltraSSDTemplate), wie Sie einen virtuellen Computer mit mehreren Ultra SSD-Datenträgern erstellen.

Im Folgenden werden die Änderungen an den neuen/geänderten Resource Manager-Vorlagen beschrieben: **apiVersion** für `Microsoft.Compute/virtualMachines` und `Microsoft.Compute/Disks` muss als `2018-06-01` (oder höher) festgelegt sein.

Geben Sie die Datenträger-SKU „UltraSSD_LRS“, die Datenträgerkapazität, IOPS und Durchsatz in Mbit/s an, um einen Ultra SSD-Datenträger zu erstellen. Im Folgenden finden Sie ein Beispiel, das einen Datenträger mit 1.024 GiB (GiB = 2^30 Bytes), 80.000 IOPS und 1.200 Mbit/s (Mbit/s = 10^6 Bytes/s) erstellt:

```json
"properties": {  
    "creationData": {  
    "createOption": "Empty"  
},  
"diskSizeGB": 1024,  
"diskIOPSReadWrite": 80000,  
"diskMBpsReadWrite": 1200,  
}
```

Fügen Sie den Eigenschaften der VM eine zusätzliche Funktion hinzu, um anzuzeigen, dass die Ultra SSD aktiviert ist (die vollständige Resource Manager-Vorlage finden Sie im [Beispiel](https://aka.ms/UltraSSDTemplate)):

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "properties": {
                    "hardwareProfile": {},
                    "additionalCapabilities" : {
                                    "ultraSSDEnabled" : "true"
                    },
                    "osProfile": {},
                    "storageProfile": {},
                    "networkProfile": {}
    }
}
```

Nachdem der virtuelle Computer bereitgestellt wurde, können Sie die Datenträger für Daten partitionieren und formatieren sowie für Ihre Workloads konfigurieren.

## <a name="additional-ultra-ssd-scenarios"></a>Zusätzliche Ultra SSD-Szenarien

- Während der Erstellung des virtuellen Computers können Ultra SSDs auch implizit erstellt werden. Allerdings erhalten diese Datenträger einen Standardwert für IOPS (500) und den Durchsatz (8 MiB/s).
- Zusätzliche Ultra SSDs können Ultra SSD-kompatiblen VMs angefügt werden.
- SSD Ultra unterstützt die Anpassung der Datenträgerleistungsattribute (IOPS und Durchsatz) zur Laufzeit, ohne den Datenträger vom virtuellen Computer zu trennen. Nachdem ein Vorgang zur Größenänderung der Datenträgerleistung auf einem Datenträger gestartet wurde, kann es bis zu einer Stunde dauern, bis die Änderung tatsächlich wirksam wird.
- Das Erhöhen der Datenträgerkapazität erfordert das Aufheben der Zuordnung eines virtuellen Computers.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den neuen Datenträgertyp testen möchten und sich noch nicht für die Preview registriert haben, [fordern Sie den Zugriff über diese Umfrage an](https://aka.ms/UltraSSDPreviewSignUp).
