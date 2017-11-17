---
title: "Problembehandlung für Microsoft Azure Stack | Microsoft-Dokumentation"
description: "Problembehandlung für Azure Stack"
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2017
ms.author: helaw
ms.openlocfilehash: 0a8e871a3a44cb14503832d2f3a096712f8112a7
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2017
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Problembehandlung für Microsoft Azure Stack

*Gilt für: Azure Stack Development Kit*

Dieses Dokument enthält allgemeine Informationen zur Problembehandlung für Azure Stack. 

Da das Technical Development Kit für Azure Stack als Auswertungsumgebung angeboten wird, ist kein offizieller Support vom Microsoft-Kundensupport verfügbar.  Suchen Sie im [MSDN-Forum für Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) nach weiterer Unterstützung und Informationen, falls Sie feststellen, dass ein Problem nicht dokumentiert ist.  

Die in diesem Abschnitt beschriebenen Empfehlungen zur Behandlung von Problemen basieren auf mehreren Quellen, und es kann vorkommen, dass sich Ihr spezifisches Problem nicht mit den Empfehlungen beheben lässt. Codebeispiele werden entsprechend dem aktuellen Entwicklungsstand bereitgestellt, und die erwarteten Ergebnisse können nicht garantiert werden. An diesem Abschnitt werden im Zuge der Implementierung von Produktverbesserungen regelmäßig Änderungen und Aktualisierungen vorgenommen.

## <a name="deployment"></a>Bereitstellung
### <a name="deployment-failure"></a>Fehler bei der Bereitstellung
Wenn während der Installation ein Fehler auftritt, können Sie die Option „rerun“ des Bereitstellungsskripts verwenden, um die Bereitstellung ab dem Schritt, in dem der Fehler aufgetreten ist, neu zu starten.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Am Ende der Bereitstellung ist die PowerShell-Sitzung noch geöffnet, und es wird keine Ausgabe angezeigt.
Dies ist wahrscheinlich nur auf das Standardverhalten des PowerShell-Befehlsfensters zurückzuführen, wenn dieses ausgewählt wurde. Die Bereitstellung des Development Kits war eigentlich erfolgreich, das Skript wurde jedoch bei der Auswahl des Fensters angehalten. Sie können überprüfen, ob dies der Fall ist, indem Sie in der Titelleiste des Befehlsfensters nach dem Wort „select“ suchen.  Drücken Sie die ESC-Taste, um die Auswahl aufzuheben. Danach sollte die Abschlussmeldung angezeigt werden.

## <a name="virtual-machines"></a>Virtuelle Computer
### <a name="default-image-and-gallery-item"></a>Standardimage und Katalogelement
Sie müssen vor der Bereitstellung virtueller Computer in Azure Stack zuerst ein Windows Server-Image und ein Katalogelement hinzufügen.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Nach einem Neustart des Azure Stack-Hosts werden einige virtuelle Computer möglicherweise nicht automatisch gestartet.
Nach dem Neustart des Hosts ist Azure Stack möglicherweise nicht sofort verfügbar.  Dies ist darauf zurückzuführen, dass [Infrastruktur-VMs](azure-stack-architecture.md#virtual-machine-roles) von Azure Stack und die RPs etwas Zeit zum Überprüfen den Konsistenz benötigen. Sie werden jedoch letztlich automatisch gestartet.

Es kann auch vorkommen, dass Mandanten-VMs nach einem Neustart des Azure Stack Development Kit-Hosts nicht automatisch gestartet werden.  Dies ist ein bekanntes Problem, und es sind nur einige manuelle Schritte erforderlich, um diese VMs wieder online zu schalten:

1.  Starten Sie auf dem Azure Stack Development Kit-Host im Startmenü den **Failovercluster-Manager**.
2.  Wählen Sie den Cluster **S-Cluster.azurestack.local** aus.
3.  Wählen Sie **Rollen** aus.
4.  Mandanten-VMs werden mit dem Zustand *gespeichert* angezeigt.  Sobald alle Infrastruktur-VMs ausgeführt werden, klicken Sie mit der rechten Maustaste auf die Mandanten-VMs, und wählen Sie **Start**, um die Ausführung des virtuellen Computers fortzusetzen.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ich habe einige virtuelle Computer gelöscht, die VHD-Dateien werden mir auf dem Datenträger aber weiterhin angezeigt. Ist dieses Verhalten zu erwarten?
Ja, dieses Verhalten ist zu erwarten. Es wurde aus den folgenden Gründen so entwickelt:

* Wenn Sie einen virtuellen Computer löschen, werden die VHDs nicht gelöscht. Datenträger sind separate Ressourcen in der Ressourcengruppe.
* Wenn ein Speicherkonto gelöscht wird, wird der Löschvorgang über Azure Resource Manager (Portal, PowerShell) sofort angezeigt, die darin enthaltenen Datenträger bleiben jedoch möglicherweise im Speicher erhalten, bis die Garbage Collection ausgeführt wird.

Wenn „verwaiste“ VHDs angezeigt werden, müssen Sie wissen, ob diese zu einem Ordner für ein gelöschtes Speicherkonto gehören. Wenn das Speicherkonto nicht gelöscht wurde, ist es normal, dass die VHDs noch angezeigt werden.

Weitere Informationen zum Konfigurieren des Schwellenwerts für die Aufbewahrung und zur bedarfsgesteuerten Anforderung der Freigabe von Speicherplatz finden Sie unter [Manage Storage Accounts](azure-stack-manage-storage-accounts.md) (Verwalten von Speicherkonten).

## <a name="storage"></a>Speicher
### <a name="storage-reclamation"></a>Freigabe von Speicherplatz
Es dauert unter Umständen bis zu 14 Stunden, bis freigegebene Kapazität im Portal angezeigt wird. Die Freigabe von Speicherplatz hängt von verschiedenen Faktoren ab, einschließlich der prozentualen Auslastung von internen Containerdateien im Blockblobspeicher. Je nach gelöschter Datenmenge besteht daher keine Garantie für die Menge des Speicherplatzes, der freigegeben werden kann, wenn der Garbage Collector ausgeführt wird.

## <a name="windows-azure-pack-connector"></a>Windows Azure Pack-Connector
* Wenn Sie das Kennwort des azurestackadmin-Kontos nach der Bereitstellung des Azure Stack Development Kits ändern, können Sie den Modus mit mehreren Clouds nicht mehr konfigurieren. Daher können Sie keine Verbindung mit der Windows Azure Pack-Zielumgebung herstellen.
* Nach dem Einrichten des Modus mit mehreren Clouds:
    * Benutzer können das Dashboard nur anzeigen, nachdem sie die Portaleinstellungen zurückgesetzt haben. (Klicken Sie im Benutzerportal Sie auf das Symbol für die Portaleinstellungen (Zahnradsymbol rechts oben). Klicken Sie unter **Standardeinstellungen wiederherstellen** auf **Übernehmen**.)
    * Die Dashboardtitel werden unter Umständen nicht angezeigt. Wenn dieses Problem auftritt, müssen Sie sie manuell wieder hinzufügen.
    * Einige Kacheln werden unter Umständen nicht korrekt angezeigt, wenn Sie diese zuerst zum Dashboard hinzuzufügen. Aktualisieren Sie den Browser, um dieses Problem zu beheben.



