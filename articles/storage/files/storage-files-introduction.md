---
title: "Einführung in Azure Files | Microsoft-Dokumentation"
description: "Übersicht über Azure Files – ein Dienst zum Erstellen und Verwenden von Netzwerkdateifreigaben in der Cloud mit dem SMP-Protokoll nach Branchenstandard."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: renash
ms.openlocfilehash: 81de67334f66a61582bb2c5d5a2303cf3a1a086c
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-files"></a>Einführung in Azure Files
Azure Files bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll [Server Message Block (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (auch als Common Internet File System oder CIFS bezeichnet) zugegriffen werden kann. Azure-Dateifreigaben können gleichzeitig durch die Cloud oder lokale Bereitstellungen von Windows, Linux und macOS eingebunden werden. Außerdem können Azure-Dateifreigaben auf Windows Server-Instanzen per Azure-Dateisynchronisierung (Vorschauversion) zwischengespeichert werden, um in der Nähe des Datennutzungsorts für schnellen Zugriff zu sorgen.

## <a name="videos"></a>Videos
| Einführung in Azure File Sync (2 Min.) | Azure Files mit Sync (Ignite 2017) (85 Min.)  |
|-|-|
| [![Standbild aus dem Einführungsvideo für Azure File Sync. Klicken Sie darauf, um die Wiedergabe zu starten.](./media/storage-files-introduction/azure-file-sync-video-snapshot.png)](https://www.youtube.com/watch?v=Zm2w8-TRn-o) | [![Standbild der Präsentation zu Azure Files mit Sync. Klicken Sie darauf, um die Wiedergabe zu starten.](./media/storage-files-introduction/azure-files-ignite-2017-video.png)](https://www.youtube.com/watch?v=r26jWDGF_rg) |

## <a name="why-azure-files-is-useful"></a>Nützlichkeit von Azure Files
Verwendungsmöglichkeiten für Azure-Dateifreigaben:

* **Ersetzen oder Erweitern von lokalen Dateiservern**:  
    Azure Files kann herkömmliche lokale Dateiserver oder NAS-Geräte vollständig ersetzen bzw. erweitern. Gängige Betriebssysteme wie Windows, macOS und Linux auf der ganzen Welt können Azure-Dateifreigaben direkt bereitstellen. Azure-Dateifreigaben können auch per Azure-Dateisynchronisierung auf Windows Server-Instanzen repliziert werden (entweder lokal oder in der Cloud), um für die Daten am Ort ihrer Verwendung eine leistungsstarke und verteilte Zwischenspeicherung zu erzielen.

* **Lift &amp; Shift-Anwendungen:**  
    Azure Files vereinfacht Lift & Shift-Vorgänge in die Cloud für Anwendungen, bei denen erwartet wird, dass auf einer Dateifreigabe Dateianwendungs- oder Benutzerdaten gespeichert werden. Azure Files ermöglicht nicht nur das klassische Lift & Shift-Szenario, bei dem sowohl die Anwendung als auch die dazugehörigen Daten nach Azure verschoben werden, sondern auch das Lift & Shift-Hybridszenario, bei dem die Anwendungsdaten nach Azure Files verschoben werden und die Anwendung weiter lokal ausgeführt wird. 

* **Einfachere Cloudentwicklung:**  
    Azure Files kann auch auf verschiedene Arten genutzt werden, um neue Cloudentwicklungsprojekte zu vereinfachen. Beispiel:
    * **Gemeinsame Anwendungseinstellungen:**  
        Bei verteilten Anwendungen befinden sich die Konfigurationsdateien häufig an einem zentralen Ort, sodass viele Anwendungsinstanzen darauf zugreifen können. Anwendungsinstanzen können ihre Konfiguration über die Datei-REST-API laden, und Menschen können je nach Bedarf darauf zugreifen, indem sie die SMB-Freigabe lokal bereitstellen.

    * **Diagnosefreigabe:**  
        Eine Azure-Dateifreigabe ist für Cloudanwendungen ein guter Ort zum Ablegen von Schreibprotokollen, Metriken und Absturzabbildern. Protokolle können von den Anwendungsinstanzen über die Datei-REST-API geschrieben werden, und Entwickler können darauf zugreifen, indem sie die Dateifreigabe auf ihrem lokalen Computer bereitstellen. Dies führt zu einer hohen Flexibilität, da Entwickler sich mit der Cloudentwicklung beschäftigen können, ohne dafür ihre vertrauten und bevorzugten Tools aufgeben zu müssen.

    * **Entwickeln/Testen/Debuggen:**  
        Für die Arbeit an virtuellen Computern in der Cloud benötigen Entwickler und Administratoren häufig verschiedene Tools oder Hilfsprogramme. Das Kopieren dieser Hilfsprogramme und Tools auf die einzelnen VMs kann sehr zeitaufwändig sein. Indem eine Azure-Dateifreigabe lokal auf den VMs bereitgestellt wird, können Entwickler und Administratoren schnell auf ihre Tools und Hilfsprogramme zugreifen, ohne sie kopieren zu müssen.

## <a name="key-benefits"></a>Hauptvorteile
* **Gemeinsamer Zugriff:** Azure-Dateifreigaben unterstützen das branchenübliche SMB-Standardprotokoll. Dadurch können Sie Ihre lokalen Dateifreigaben problemlos durch Azure-Dateifreigaben ersetzen, ohne sich Gedanken über die Anwendungskompatibilität machen zu müssen. Durch die Möglichkeit, ein Dateisystem über mehrere Computer, Anwendungen und Instanzen hinweg zu nutzen, bietet Azure Files einen deutlichen Vorteil für Anwendungen, die gemeinsam nutzbar sein müssen. 
* **Vollständige Verwaltung:** Sie können Azure-Dateifreigaben erstellen, ohne sich um die Hardware oder um das Betriebssystem zu kümmern. Sie müssen also weder das Serverbetriebssystem mit kritischen Sicherheitsupgrades patchen noch fehlerhafte Festplatten austauschen.
* **Skripts und Tools:** Mit PowerShell-Cmdlets und der Azure-Befehlszeilenschnittstelle können Sie Azure-Dateifreigaben im Rahmen der Verwaltung von Azure-Anwendungen erstellen, einbinden und verwalten. Azure-Dateifreigaben können über das Azure-Portal und mithilfe des Azure Storage-Explorers erstellt und verwaltet werden. 
* **Resilienz:** Azure Files ist von Grund auf als hochverfügbare Lösung konzipiert. Anders als bei lokalen Dateifreigaben müssen Sie sich bei Azure Files nicht mehr mit lokalen Stromausfällen oder Netzwerkproblemen auseinandersetzen. 
* **Vertraute Programmierbarkeit:** In Azure ausgeführte Anwendungen können über [Dateisystem-E/A-APIs](https://msdn.microsoft.com/library/system.io.file.aspx) auf Daten in der Freigabe zugreifen. Entwickler können daher ihren vorhandenen Code und bereits erlernte Fertigkeiten für die Migration vorhandener Anwendungen verwenden. Neben System-E/A-APIs können Sie auch [Azure Storage-Clientbibliotheken](https://msdn.microsoft.com/library/azure/dn261237.aspx) oder die [Azure Storage-REST-API](/rest/api/storageservices/file-service-rest-api) verwenden.

## <a name="next-steps"></a>Nächste Schritte
* [Create a File Share in Azure File storage](storage-how-to-create-file-share.md) (Erstellen einer Dateifreigabe in Azure File Storage)
* [Mount an Azure File share and access the share in Windows](storage-how-to-use-files-windows.md) (Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows)
* [Verwenden des Azure-Dateispeichers unter Linux](storage-how-to-use-files-linux.md)
* [Mount Azure File share over SMB with macOS](storage-how-to-use-files-mac.md) (Einbinden einer Azure-Dateifreigabe über SMB unter macOS)