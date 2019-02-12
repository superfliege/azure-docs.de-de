---
title: Migrieren von virtuellen Azure IaaS-Computern zu einer anderen Azure-Region mithilfe des Azure Site Recovery-Diensts | Microsoft-Dokumentation
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure IaaS-Computer von einer Azure-Region zu einer anderen zu migrieren.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5d844692b6199d93fa835da1021c9753311e17de
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824615"
---
# <a name="move-azure-vms-to-another-region"></a>Verschieben virtueller Azure-Computer in eine andere Region

Aufgrund des stetig wachsenden Kundenstamms von Azure und der dadurch steigenden Nachfrage wird das Angebot durch neue Regionen ergänzt. Auch kommen jeden Monat neuere Funktionen für verschiedene Dienste hinzu. Daher kann es bisweilen sinnvoll sein, Ihre virtuellen Computer in eine andere Region oder in Verfügbarkeitszonen zu verschieben, um die Verfügbarkeit zu erhöhen.

In diesem Dokument werden die verschiedenen Szenarien erläutert, in denen die Verschiebung Ihrer virtuellen Computer sinnvoll sein kann, und Sie erfahren, wie die Architektur am Ziel konfiguriert werden muss, um eine höhere Verfügbarkeit zu erreichen. 
> [!div class="checklist"]
> * [Gründe für die Verschiebung virtueller Azure-Computer](#why-would-you-move-azure-vms)
> * [Vorgehensweise zum Verschieben virtueller Azure-Computer](#how-to-move-azure-vms)
> * [Typische Architekturen](#typical-architectures-for-a-multi-tier-deployment)
> * [Unverändertes Verschieben virtueller Computer in eine Zielregion](#move-azure-vms-to-another-region)
> * [Verschieben virtueller Computer zur Erhöhung der Verfügbarkeit](#move-vms-to-increase-availability)


## <a name="why-would-you-move-azure-vms"></a>Gründe für die Verschiebung virtueller Azure-Computer

Kunden verschieben virtuelle Computer aus folgenden Gründen:

- Wenn Sie bereits virtuelle Computer in einer Region bereitgestellt haben und eine neue Region hinzugefügt wurde, die sich näher an den Endbenutzern Ihrer Anwendung oder Ihres Diensts befindet, **verschieben Sie Ihre virtuellen Computer unverändert in die neue Region**, um die Wartezeit zu verringern. Die gleiche Vorgehensweise können Sie verwenden, wenn Sie Abonnements konsolidieren möchten oder Governance-/Organisationsregeln eine Verschiebung erforderlich machen. 
- Wenn Ihr virtueller Computer als einzelne VM-Instanz oder als Teil einer Verfügbarkeitsgruppe bereitgestellt wurde und Sie die Verfügbarkeits-SLAs erhöhen möchten, können Sie **Ihre virtuellen Computer in eine Verfügbarkeitsgruppe verschieben**. 

## <a name="how-to-move-azure-vms"></a>Vorgehensweise zum Verschieben virtueller Azure-Computer
Das Verschieben virtueller Computer umfasst folgende Schritte:

1. Überprüfen der Voraussetzungen 
2. Vorbereiten der Quell-VMs 
3. Vorbereiten der Zielregion 
4. Kopieren der Daten in die Zielregion: Verwenden Sie die Azure Site Recovery-Replikationstechnologie, um die Daten der Quell-VM in die Zielregion zu kopieren.
5. Testen der Konfiguration: Testen Sie nach Abschluss der Replikation die Konfiguration mithilfe eines Testfailovers auf ein Netzwerk, das nicht in die Produktion eingebunden ist.
6. Durchführen der Verschiebung 
7. Verwerfen der Ressourcen in der Quellregion 


> [!IMPORTANT]
> Azure Site Recovery unterstützt aktuell das Verschieben virtueller Computer zwischen Regionen, aber keine Verschiebung innerhalb einer Region. 

> [!NOTE]
> Ausführlichere Informationen zu diesen Schritten finden Sie in der Dokumentation für die jeweiligen Szenarien aus diesem Artikel.

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Typische Architekturen für eine Bereitstellung mit mehreren Ebenen
Im folgenden Abschnitt werden die gängigsten Bereitstellungsarchitekturen erläutert, die von Kunden für eine Anwendung mit mehreren Ebenen in Azure verwendet werden. Als Beispiel dient hier eine Anwendung mit drei Ebenen und einer öffentlichen IP-Adresse. Die einzelnen Ebenen (Web, Anwendung und Datenbank) enthalten jeweils zwei virtuelle Computer und sind über einen Load Balancer miteinander verbunden. Die Datenbankebene verfügt dank SQL Always On-Replikation zwischen den virtuellen Computern über Hochverfügbarkeit.

1.  **Einzelinstanz-VMs mit Bereitstellung über verschiedene Ebenen:** Jeder virtuelle Computer auf einer Ebene ist als Einzelinstanz-VM konfiguriert und über Load Balancer mit den anderen Ebenen verbunden. Dies ist die einfachste Kundenkonfiguration.

       ![single-VMs](media/move-vm-overview/regular-deployment.PNG)

2. **VMs auf den einzelnen Ebenen mit Bereitstellung über eine Verfügbarkeitsgruppe:** Jeder virtuelle Computer auf einer Ebene ist in einer Verfügbarkeitsgruppe konfiguriert. [Verfügbarkeitsgruppen](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwareknoten in einem Cluster verteilt werden. Dadurch wird sichergestellt, dass sich Hardware- oder Softwarefehler in Azure nur auf einen Teil Ihrer VMs auswirken und die Lösung insgesamt verfügbar und betriebsbereit bleibt. 
   
      ![avset](media/move-vm-overview/AVset.PNG)

3. **VMs auf den einzelnen Ebenen mit Bereitstellung über eine Verfügbarkeitszone:** Jeder virtuelle Computer auf einer Ebene ist über [Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) hinweg konfiguriert. Eine Verfügbarkeitszone in einer Azure-Region ist eine Kombination aus einer Fehlerdomäne und einer Updatedomäne. Wenn Sie z.B. drei oder mehr virtuelle Computer über drei Zonen verteilt in einer Azure-Region erstellen, werden Ihre virtuellen Computer effektiv auf drei Fehlerdomänen und drei Updatedomänen verteilt. Die Azure-Plattform erkennt diese Updatedomänen übergreifende Verteilung, um sicherzustellen, dass virtuelle Computer in unterschiedlichen Zonen nicht gleichzeitig aktualisiert werden.

      ![zone-deploymnt](media/move-vm-overview/zone.PNG)



 ## <a name="move-vms-as-is-to-a-target-region"></a>Unverändertes Verschieben virtueller Computer in eine Zielregion

Dieser Abschnitt zeigt anhand der oben erwähnten [Architekturen](#typical-architectures-for-a-multi-tier-deployment), wie die Bereitstellungen nach der unveränderten Verschiebung in die Zielregion jeweils aussehen.


1. **Einzelinstanz-VMs mit Bereitstellung über verschiedene Ebenen:** 

     ![single-zone.PNG](media/move-vm-overview/single-zone.PNG)

2. **VMs auf den einzelnen Ebenen mit Bereitstellung über eine Verfügbarkeitsgruppe:**

     ![crossregionAset.PNG](media/move-vm-overview/crossregionAset.PNG)


3. **VMs auf den einzelnen Ebenen mit Bereitstellung über eine Verfügbarkeitszone:**
      

     ![AzoneCross.PNG](media/move-vm-overview/AzoneCross.PNG)

## <a name="move-vms-to-increase-availability"></a>Verschieben virtueller Computer zur Erhöhung der Verfügbarkeit

1. **Einzelinstanz-VMs mit Bereitstellung über verschiedene Ebenen:** 

     ![single-zone.PNG](media/move-vm-overview/single-zone.PNG)

2. **VMs auf den einzelnen Ebenen mit Bereitstellung über eine Verfügbarkeitsgruppe:** Sie können Ihre virtuellen Computer in einer Verfügbarkeitsgruppe in separaten Verfügbarkeitszonen platzieren, wenn Sie die Replikation für Ihren virtuellen Computer über Azure Site Recovery aktivieren. Die SLA für die Verfügbarkeit hat nach Abschluss der Verschiebung einen Wert von 99,9 Prozent.

      ![aset-Azone.PNG](media/move-vm-overview/aset-Azone.PNG)


## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurden allgemeine Aspekte der Verschiebung virtueller Computer behandelt. Eine schrittweise Anleitung finden Sie hier:


> [!div class="nextstepaction"]
> * [Verschieben virtueller Azure-Computer in eine andere Region](azure-to-azure-tutorial-migrate.md)

> * [Move Azure VMs into Availability Zones](move-azure-VMs-AVset-Azone.md) (Verschieben virtueller Azure-Computer in Verfügbarkeitszonen)

