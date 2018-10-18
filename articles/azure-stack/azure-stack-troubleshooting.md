---
title: Problembehandlung für Microsoft Azure Stack | Microsoft-Dokumentation
description: Problembehandlung für Azure Stack
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: c463599190c5bfaac47a70dbca7b8a67dc830f3a
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363482"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Problembehandlung für Microsoft Azure Stack

Dieses Dokument enthält allgemeine Informationen zur Problembehandlung für Azure Stack. 

> [!NOTE]
> Da das Technical Development Kit für Azure Stack (ASDK) als Auswertungsumgebung angeboten wird, ist kein offizieller Support vom Microsoft-Kundensupport verfügbar. Suchen Sie bei Problemen im [MSDN-Forum für Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) nach weiterer Unterstützung und Informationen.  

Die in diesem Abschnitt beschriebenen Empfehlungen zur Behandlung von Problemen basieren auf mehreren Quellen, und es kann vorkommen, dass sich Ihr spezifisches Problem nicht mit den Empfehlungen beheben lässt. Codebeispiele werden entsprechend dem aktuellen Entwicklungsstand bereitgestellt, und die erwarteten Ergebnisse können nicht garantiert werden. An diesem Abschnitt werden im Zuge der Implementierung von Produktverbesserungen regelmäßig Änderungen und Aktualisierungen vorgenommen.

## <a name="deployment"></a>Bereitstellung
### <a name="deployment-failure"></a>Fehler bei der Bereitstellung
Wenn während der Installation ein Fehler auftritt, können Sie mithilfe der Option „-rerun“ des Bereitstellungsskripts die Bereitstellung ab dem Schritt neu starten, in dem der Fehler aufgetreten ist.  

### <a name="at-the-end-of-asdk-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Am Ende der ASDK-Bereitstellung ist die PowerShell-Sitzung noch geöffnet, und es wird keine Ausgabe angezeigt.
Dies ist wahrscheinlich nur auf das Standardverhalten des PowerShell-Befehlsfensters zurückzuführen, wenn dieses ausgewählt wurde. Die Bereitstellung des Development Kits war erfolgreich, das Skript wurde jedoch bei der Auswahl des Fensters angehalten. Sie können überprüfen, ob das Setup abgeschlossen ist, indem Sie in der Titelleiste des Befehlsfensters nach dem Wort „select“ suchen.  Drücken Sie die ESC-Taste, um die Auswahl aufzuheben. Danach sollte die Abschlussmeldung angezeigt werden.

## <a name="virtual-machines"></a>Virtuelle Computer
### <a name="default-image-and-gallery-item"></a>Standardimage und Katalogelement
Vor der Bereitstellung virtueller Computer in Azure Stack müssen Sie zuerst ein Windows Server-Image und ein Katalogelement hinzufügen.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Nach einem Neustart des Azure Stack-Hosts werden einige virtuelle Computer möglicherweise nicht automatisch gestartet.
Nach dem Neustart des Hosts ist Azure Stack möglicherweise nicht sofort verfügbar.  Dies ist darauf zurückzuführen, dass [Infrastruktur-VMs](..\azure-stack\asdk\asdk-architecture.md#virtual-machine-roles) von Azure Stack und Ressourcenanbieter etwas Zeit zum Überprüfen der Konsistenz benötigen. Sie werden jedoch letztlich automatisch gestartet.

Es kann auch vorkommen, dass Mandanten-VMs nach einem Neustart des Azure Stack Development Kit-Hosts nicht automatisch gestartet werden. Dies ist ein bekanntes Problem, und es sind nur einige manuelle Schritte erforderlich, um diese VMs wieder online zu schalten:

1.  Starten Sie auf dem Azure Stack Development Kit-Host im Startmenü den **Failovercluster-Manager**.
2.  Wählen Sie den Cluster **S-Cluster.azurestack.local** aus.
3.  Wählen Sie **Rollen** aus.
4.  Mandanten-VMs werden mit dem Zustand *gespeichert* angezeigt. Sobald alle Infrastruktur-VMs ausgeführt werden, klicken Sie mit der rechten Maustaste auf die Mandanten-VMs, und wählen Sie **Start**, um die Ausführung des virtuellen Computers fortzusetzen.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ich habe einige virtuelle Computer gelöscht, die VHD-Dateien werden mir auf dem Datenträger aber weiterhin angezeigt. Ist dieses Verhalten zu erwarten?
Ja, dieses Verhalten wird erwartet. Es wurde aus den folgenden Gründen so entwickelt:

* Wenn Sie einen virtuellen Computer löschen, werden die VHDs nicht gelöscht. Datenträger sind separate Ressourcen in der Ressourcengruppe.
* Wenn ein Speicherkonto gelöscht wird, wird der Löschvorgang über Azure Resource Manager sofort angezeigt, die darin enthaltenen Datenträger bleiben jedoch möglicherweise im Speicher erhalten, bis die Garbage Collection ausgeführt wird.

Wenn „verwaiste“ VHDs angezeigt werden, müssen Sie wissen, ob diese zu einem Ordner für ein gelöschtes Speicherkonto gehören. Wenn das Speicherkonto nicht gelöscht wurde, ist es normal, dass die VHDs noch angezeigt werden.

Weitere Informationen zum Konfigurieren des Schwellenwerts für die Aufbewahrung und zur bedarfsgesteuerten Anforderung der Freigabe von Speicherplatz finden Sie unter [Manage Storage Accounts](azure-stack-manage-storage-accounts.md) (Verwalten von Speicherkonten).

## <a name="storage"></a>Speicher
### <a name="storage-reclamation"></a>Freigabe von Speicherplatz
Es kann bis zu 14 Stunden dauern, bis freigegebene Kapazität im Portal angezeigt wird. Die Freigabe von Speicherplatz hängt von verschiedenen Faktoren ab, einschließlich der prozentualen Auslastung von internen Containerdateien im Blockblobspeicher. Je nach gelöschter Datenmenge besteht daher keine Garantie für die Menge des Speicherplatzes, der freigegeben werden kann, wenn der Garbage Collector ausgeführt wird.

