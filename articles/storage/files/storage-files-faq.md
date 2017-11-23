---
title: "Häufig gestellte Fragen zu Azure Files | Microsoft-Dokumentation"
description: "Hier erhalten Sie Antworten auf häufig gestellte Fragen zu Azure Files."
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
ms.date: 10/13/2017
ms.author: renash
ms.openlocfilehash: da8ccf35dcc873a5c31842c6eb7bdf72879854c2
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Häufig gestellte Fragen zu Azure Files
[Azure Files](storage-files-introduction.md) bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll [Server Message Block (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (auch als Common Internet File System oder CIFS bezeichnet) zugegriffen werden kann. Sie können Azure-Dateifreigaben gleichzeitig unter Cloud- und lokalen Bereitstellungen von Windows, Linux und macOS einbinden. Azure-Dateifreigaben können auch auf Windows Server-Computern zwischengespeichert werden, indem Azure File Sync (Vorschauversion) verwendet wird, um den schnellen Zugriff in der Nähe der Datennutzung zu ermöglichen.

In diesem Artikel werden häufig gestellte Fragen zu Azure Files-Features und -Funktionen beantwortet, z.B. die Nutzung von Azure File Sync mit Azure Files. Wenn Sie hier keine Antwort auf Ihre Frage finden, können Sie sich über die folgenden Kanäle an uns wenden (Eskalationsreihenfolge):

1. Im Abschnitt „Kommentare“ dieses Artikels.
2. [Azure Storage-Forum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=windowsazuredata)
3. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) 
4. Microsoft-Support. Wählen Sie zum Erstellen einer neuen Supportanfrage im Azure-Portal auf der Registerkarte **Hilfe** die Schaltfläche **Hilfe und Support** und anschließend die Option **Neue Supportanfrage**.

## <a name="general"></a>Allgemein
* <a id="why-files-useful"></a>**Auf welche Weise ist Azure Files nützlich?**  
   Sie können Azure Files zum Erstellen von Dateifreigaben in der Cloud verwenden, ohne für die Verwaltung eines physischen Servers oder Geräts verantwortlich zu sein. Wir übernehmen die monotone Arbeit für Sie, z.B. das Anwenden von Betriebssystemupdates und das Austauschen von fehlerhaften Datenträgern. Lesen Sie [Nützlichkeit von Azure Files](storage-files-introduction.md#why-azure-files-is-useful), um mehr über die Szenarien zu erfahren, in denen Azure Files hilfreich ist.

* <a id="file-access-options"></a>**Auf welche Arten kann auf Dateien in Azure Files zugegriffen werden?**  
    Sie können die Dateifreigabe mithilfe des SMB 3.0-Protokolls auf Ihrem lokalen Computer einbinden oder mithilfe von Tools wie dem [Speicher-Explorer](http://storageexplorer.com/) auf Dateien in Ihrer Dateifreigabe zugreifen. In Ihrer Anwendung können Sie über Speicherclientbibliotheken, REST-APIs, PowerShell oder die Azure CLI auf Ihre Dateien in der Azure-Dateifreigabe zugreifen.

* <a id="what-is-afs"></a>**Was ist Azure File Sync?**  
    Sie können Azure File Sync verwenden, um die Dateifreigaben Ihrer Organisation in Azure Files zu zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Mit Azure File Sync werden Ihre Windows Server-Computer zu einem schnellen Cache für Ihre Azure-Dateifreigabe. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen, z.B. SMB, Network File System (NFS) und File Transfer Protocol Service (FTPS). Sie können weltweit so viele Caches wie nötig nutzen.

* <a id="files-versus-blobs"></a>**Warum sollte ich für meine Daten eine Azure-Dateifreigabe anstelle von Azure Blob Storage verwenden?**  
    Azure Files und Azure Blob Storage bieten beide die Möglichkeit, große Datenmengen in der Cloud zu speichern, unterscheiden sich in ihrem Einsatzzweck aber leicht voneinander. 
    
    Der Azure Blob Storage ist für umfangreiche native Cloudanwendungen geeignet, die unstrukturierte Daten speichern müssen. In Bezug auf die Maximierung von Leistung und Skalierbarkeit stellt der Azure Blob Storage eine einfachere Speicherabstraktion dar als ein echtes Dateisystem. Sie können auf den Azure Blob Storage nur über REST-basierte Clientbibliotheken (oder direkt über das REST-basierte Protokoll) zugreifen.

    Azure Files ist eigentlich ein Dateisystem. Azure Files verfügt über alle Datei-Abstracts, die Sie bei der jahrelangen Arbeit mit lokalen Betriebssystemen kennen und schätzen gelernt haben. Wie Azure Blob Storage auch, verfügt Azure Files über eine REST-Schnittstelle und REST-basierte Clientbibliotheken. Im Gegensatz zu Azure Blob Storage ermöglicht Azure Files den SMB-Zugriff auf Azure-Dateifreigaben. Per SMB können Sie eine Azure-Dateifreigabe direkt unter Windows, Linux oder macOS (lokal oder auf Cloud-VMs) bereitstellen, ohne Codes schreiben oder dem Dateisystem spezielle Treiber hinzufügen zu müssen. Außerdem können Sie Azure-Dateifreigaben auf lokalen Dateiservern zwischenspeichern, indem Sie Azure File Sync nutzen, um den schnellen Zugriff in der Nähe der Datennutzung zu ermöglichen. 
   
    Eine ausführliche Beschreibung der Unterschiede zwischen Azure Files und dem Azure Blob Storage finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Weitere Informationen zum Azure Blob Storage finden Sie unter [Einführung in Blob Storage](../blobs/storage-blobs-introduction.md).

* <a id="files-versus-disks"></a>**Warum sollte ich eine Azure-Dateifreigabe anstelle von Azure-Datenträgern verwenden?**  
    Unter Azure-Datenträger ist ein Datenträger nicht mehr als ein einfacher Datenträger. Ein eigenständiger Datenträger allein ist nicht sehr nützlich. Sie erhöhen den Nutzen von Azure-Datenträgern, indem Sie einen Datenträger an einen virtuellen Computer anfügen, der in Azure ausgeführt wird. Azure-Datenträger können für alle Zwecke verwendet werden, die auch für einen Datenträger auf einem lokalen Server gelten. Sie können sie als Betriebssystemdatenträger, als Auslagerungsbereich für ein Betriebssystem oder als dedizierten Speicher für eine Anwendung einsetzen. Ein interessanter Einsatzzweck von Azure-Datenträgern ist die Erstellung eines Dateiservers in der Cloud, der in denselben Situationen wie bei einer Azure-Dateifreigabe verwendet wird. Die Bereitstellung eines Dateiservers auf Azure Virtual Machines ist ein auf hohe Leistung ausgelegtes Verfahren, um Dateispeicher in Azure freizugeben, wenn Sie Bereitstellungsoptionen benötigen (z.B. NFS-Protokollunterstützung oder Storage Premium), die derzeit nicht von Azure Files unterstützt werden. 

    Das Ausführen eines Dateiservers mit Azure-Datenträgern als Back-End-Speicher ist aus einigen Gründen normalerweise aber deutlich teurer als die Verwendung einer Azure-Dateifreigabe. Zunächst einmal müssen Sie nicht nur für die Kosten des Datenträgerspeichers aufkommen, sondern auch für die Ausgaben zum Betrieb von einer oder mehreren Azure-VMs. Zweitens müssen Sie auch die VMs verwalten, die zum Ausführen des Dateiservers verwendet werden. Beispielsweise sind Sie für Upgrades des Betriebssystems verantwortlich. Und falls Daten lokal zwischengespeichert werden müssen, sind Sie auch für die Einrichtung und Verwaltung der Replikationstechnologien (z.B. DFS-Replikation (Distributed File System Replication)) zuständig.

    Ein Ansatz, bei dem Sie jeweils die Vorteile von Azure Files sowie eines unter Azure Virtual Machines gehosteten Dateiservers (zusätzlich zur Verwendung von Azure-Datenträgern als Back-End-Speicher) nutzen können, ist die Installation von Azure File Sync auf einem Dateiserver, der auf einer Cloud-VM gehostet wird. Wenn sich die Azure-Dateifreigabe in derselben Region wie Ihr Dateiserver befindet, können Sie das Cloudtiering aktivieren und den Prozentsatz des freien Volumespeicherplatzes auf den Maximalwert (99%) festlegen. So wird die minimale Duplizierung von Daten sichergestellt. Außerdem können Sie für Ihre Dateiserver beliebige Anwendungen nutzen, z.B. Anwendungen, die eine Unterstützung des NFS-Protokolls benötigen.

    Informationen zu einer Möglichkeit, wie Sie einen leistungsstarken und hochverfügbaren Dateiserver in Azure einrichten, finden Sie unter [Deploying IaaS VM Guest Clusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/) (Bereitstellen von IaaS-VM-Gastclustern in Microsoft Azure). Eine ausführliche Beschreibung der Unterschiede zwischen Azure Files und Azure-Datenträgern finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Weitere Informationen zu Azure-Datenträgern finden Sie unter [Azure Managed Disks – Übersicht](../../virtual-machines/windows/managed-disks-overview.md).

* <a id="get-started"></a>**Wie kann ich mich mit Azure Files vertraut machen?**  
   Die ersten Schritte mit Azure Files sind leicht. [Erstellen Sie zuerst eine Dateifreigabe](storage-how-to-create-file-share.md), und binden Sie sie dann in Ihrem bevorzugten Betriebssystem ein: 

    * [Bereitstellen unter Windows](storage-how-to-use-files-windows.md)
    * [Bereitstellen unter Linux](storage-how-to-use-files-linux.md)
    * [Bereitstellen unter macOS](storage-how-to-use-files-mac.md)

   Eine ausführlichere Anleitung zum Bereitstellen einer Azure-Dateifreigabe als Ersatz für Produktionsdateifreigaben in Ihrer Organisation finden Sie unter [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md).

* <a id="redundancy-options"></a>**Welche Speicherredundanzoptionen werden von Azure Files unterstützt?**  
    Azure Files unterstützt derzeit nur Konten mit lokal redundantem Speicher (LRS) und georedundantem Speicher (GRS). Für die Zukunft ist Unterstützung für zonenredundanten Speicher (ZRS) und georedundanten Speicher mit Lesezugriff (Read-Access Geo Redundant Storage, RA-GRS) geplant. Zum gegenwärtigen Zeitpunkt sind hierfür aber noch keine Zeitpläne verfügbar.

* <a id="tier-options"></a>**Welche Speicherebenen werden in Azure Files unterstützt?**  
    Derzeit unterstützt Azure Files nur die Speicherebene „Standard“. Für die Unterstützung von Storage Premium und „kaltem“ Speicher können noch keine Starttermine genannt werden. 
    
    > [!NOTE]
    > Es ist nicht möglich, Azure-Dateifreigaben basierend auf reinen Blobspeicherkonten oder Storage Premium-Konten zu erstellen.

* <a id="give-us-feedback"></a>**Ich wünsche mir, dass Azure Files ein bestimmtes Feature hinzugefügt wird. Können Sie es hinzufügen?**  
    Das Azure Files-Team legt großen Wert darauf, Ihr Feedback zu unserem Dienst zu erhalten. Stimmen Sie in [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) über Funktionsanfragen ab! Wir hoffen, dass wir Sie mit vielen neuen Funktionen begeistern können.

## <a name="azure-file-sync"></a>Azure-Dateisynchronisierung
* <a id="afs-region-availability"></a>**Welche Regionen werden für Azure File Sync (Vorschau) unterstützt?**  
    Azure File Sync ist derzeit in den Regionen „USA, Westen“, „Europa, Westen“, „Australien, Osten“ und „Asien, Südosten“ verfügbar. Unterstützung für weitere Regionen wird hinzugefügt, während auf die allgemeine Verfügbarkeit hingearbeitet wird. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](storage-sync-files-planning.md#region-availability).

* <a id="cross-domain-sync"></a>**Kann ich in Domänen eingebundene Server und nicht in Domänen eingebundene Server in derselben Synchronisierungsgruppe verwenden?**  
    Ja. Eine Synchronisierungsgruppe kann Serverendpunkte mit unterschiedlichen Active Directory-Mitgliedschaften auch dann umfassen, wenn diese nicht in Domänen eingebunden sind. Diese Konfiguration funktioniert zwar technisch einwandfrei, aber es wird davon abgeraten, diese als normale Konfiguration zu verwenden. Der Grund ist, dass Zugriffssteuerungslisten (ACLs), die für Dateien und Ordner auf einem Server definiert sind, unter Umständen von anderen Servern in der Synchronisierungsgruppe nicht durchgesetzt werden können. Die besten Ergebnisse erzielen Sie, wenn Sie sich an die folgende Empfehlung halten: Führen Sie die Synchronisierung zwischen Servern durch, die in derselben Active Directory-Gesamtstruktur enthalten sind, zwischen Servern in unterschiedlichen Active Directory-Gesamtstrukturen, die aber über eingerichtete Vertrauensstellungen verfügen, oder zwischen Servern, die keiner Domäne angehören. Hierbei ist es nicht ratsam, diese Konfigurationen zu mischen.

* <a id="afs-change-detection"></a>**Ich habe eine Datei in meiner Azure-Dateifreigabe direkt per SMB oder über das Portal erstellt. Wie lange dauert es, bis die Datei für die Server der Synchronisierungsgruppe synchronisiert wurde?**  
    [!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

* <a id="afs-conflict-resolution"></a>**Was passiert, wenn dieselbe Datei ungefähr zur gleichen Zeit auf zwei Servern geändert wird?**  
    Für Azure File Sync wird eine einfache Strategie zur Konfliktlösung verwendet: Die Änderungen der Dateien, die gleichzeitig auf zwei Servern vorgenommen werden, werden jeweils beibehalten. Für die zuletzt vorgenommene Änderung wird der ursprüngliche Dateiname beibehalten. Bei der älteren Datei werden der „Quellcomputer“ und die Konfliktnummer an den Namen angefügt. Hierfür wird die folgende Taxonomie verwendet: 
   
    \<Dateiname_ohne_Erweiterung\>-\<Name_des_Computers\>\[-#\].\<Erweiterung\>  

    Beim ersten Konflikt für „CompanyReport.docx“ wird beispielsweise „CompanyReport-CentralServer.docx“ verwendet, wenn CentralServer der Ort ist, an dem der ältere Schreibvorgang durchgeführt wurde. Der zweite Konflikt hat dann den Namen „CompanyReport-CentralServer-1.docx“.

* <a id="afs-storage-redundancy"></a>**Wird georedundanter Speicher von Azure File Sync unterstützt?**  
    Ja. Azure Files unterstützt sowohl lokal redundanten Speicher (LRS) als auch georedundanten Speicher (GRS). Bei einem GRS-Failover zwischen gekoppelten Regionen wird empfohlen, die neue Region lediglich als Datensicherung zu behandeln. Azure File Sync beginnt nicht automatisch die Synchronisierung mit der neuen primären Region. 

* <a id="sizeondisk-versus-size"></a>**Warum stimmt die Eigenschaft *Größe auf Festplatte* einer Datei nach der Verwendung von Azure File Sync nicht mit der Eigenschaft *Größe* überein?**  
    Der Windows-Datei-Explorer verwendet zwei Eigenschaften zur Darstellung einer Dateigröße: **Größe** und **Größe auf Festplatte**. Diese Eigenschaften unterscheiden sich leicht in ihrer Bedeutung. **Größe** steht für die vollständige Größe der Datei. **Größe auf Datenträger** steht für die Größe des Dateidatenstroms, der auf dem Datenträger gespeichert ist. Die Werte dieser Eigenschaften können aus unterschiedlichen Gründen variieren, z.B. aufgrund einer Komprimierung, einer Datendeduplizierung oder eines Cloudtiering mit Azure File Sync. Wenn eine Datei per Tiering auf eine Azure-Dateifreigabe ausgelagert wird, ist die Größe auf dem Datenträger gleich null, da der Dateidatenstrom nicht auf dem Datenträger, sondern in Ihrer Azure-Dateifreigabe gespeichert wird. Für eine Datei kann das Tiering auch teilweise durchgeführt werden (bzw. teilweise zurückgerufen werden). Bei einer teilweise per Tiering ausgelagerten Datei befindet sich ein Teil der Datei auf dem Datenträger. Dies kann vorkommen, wenn Dateien von Anwendungen, z.B. Multimedia-Playern oder ZIP-Hilfsprogrammen, teilweise gelesen werden. 

* <a id="is-my-file-tiered"></a>**Woran erkenne ich, ob eine Datei per Tiering ausgelagert wurde?**  
    Es gibt mehrere Möglichkeiten, um zu überprüfen, ob eine Datei per Tiering auf Ihre Azure-Dateifreigabe ausgelagert wurde:
    
   *  **Überprüfen Sie die Dateiattribute in der Datei.**
     Klicken Sie hierzu mit der rechten Maustaste auf eine Datei, navigieren Sie zu **Details**, und scrollen Sie dann nach unten zur Eigenschaft **Attribute**. In einer per Tiering ausgelagerten Datei sind folgende Attribute festgelegt:     
        
        | Attributbuchstabe | Attribut | Definition |
        |:----------------:|-----------|------------|
        | A | Archivieren | Gibt an, dass die Datei durch Sicherungssoftware gesichert werden sollte. Dieses Attribut ist immer festgelegt, unabhängig davon, ob die Datei per Tiering ausgelagert oder vollständig auf dem Datenträger gespeichert wurde. |
        | P | Sparsedatei | Gibt an, dass die Datei eine „Sparsedatei“ (platzsparende Datei) ist. Eine platzsparende Datei ist ein spezieller Typ von Datei, der von NTFS zur effizienten Verwendung bereitgestellt wird, wenn die Datei im Datenträgerdatenstrom nahezu leer ist. Platzsparende Dateien werden von Azure File Sync genutzt, da eine Datei entweder vollständig per Tiering ausgelagert oder teilweise zurückgerufen wurde. Bei einer vollständig per Tiering ausgelagerten Datei wird der Dateidatenstrom in der Cloud gespeichert. Bei einer teilweise zurückgerufenen Datei befindet sich dieser Teil der Datei bereits auf dem Datenträger. Wenn eine Datei vollständig auf die Festplatte zurückgerufen wird, konvertiert Azure File Sync diese platzsparende Datei in eine normale Datei. |
        | L | Analysepunkt | Gibt an, dass die Datei über einen Analysepunkt verfügt. Ein Analysepunkt ist ein spezieller Zeiger für die Verwendung durch einen Dateisystemfilter. Azure File Sync verwendet Analysepunkte, um den Speicherort für den Azure File Sync-Dateisystemfilter (StorageSync.sys) zu definieren, an dem die Datei in der Cloud gespeichert ist. Dies erleichtert den nahtlosen Zugriff. Benutzer müssen nicht wissen, dass Azure File Sync verwendet wird oder wie der Zugriff auf die Datei in Ihrer Azure-Dateifreigabe möglich ist. Wenn eine Datei vollständig zurückgerufen wird, entfernt Azure File Sync den Analysepunkt aus der Datei. |
        | O | Offline | Gibt an, dass ein Teil des Dateiinhalts oder der gesamte Dateiinhalt nicht auf dem Datenträger gespeichert ist. Wenn eine Datei vollständig zurückgerufen wird, entfernt Azure File Sync dieses Attribut. |

        ![Dialogfeld „Eigenschaften“ einer Datei mit ausgewählter Registerkarte „Details“](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Sie können die Attribute für alle Dateien in einem Ordner anzeigen, indem das Feld **Attribute** der Tabellenanzeige des Datei-Explorers hinzugefügt wird. Klicken Sie hierzu mit der rechten Maustaste auf eine vorhandene Spalte (z.B. **Größe**). Wählen Sie anschließend **Mehr** und dann in der Dropdownliste **Attribute** aus.
        
   * **Verwenden Sie `fsutil`, um nach Analysepunkten in einer Datei zu suchen.**
       Wie für die vorherige Option beschrieben, verfügt eine per Tiering ausgelagerte Datei immer über einen festgelegten Analysepunkt. Ein Analysepunkt ist ein spezieller Zeiger für den Azure File Sync-Dateisystemfilter (StorageSync.sys). Führen Sie das Hilfsprogramm `fsutil` an einer Eingabeaufforderung mit erhöhten Rechten oder in einem PowerShell-Fenster aus, um zu überprüfen, ob eine Datei einen Analysepunkt hat:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Falls die Datei einen Analysepunkt enthält, sollte **Analysenkennungswert : 0x8000001e** angezeigt werden. Dieser Hexadezimalwert ist der Analysepunktwert, der Azure File Sync zuzuordnen ist. Die Ausgabe enthält außerdem die Analysedaten, die für den Pfad zu Ihrer Datei auf der Azure-Dateifreigabe stehen.

        > [!WARNING]  
        > Der Hilfsprogrammbefehl `fsutil reparsepoint` bietet außerdem die Möglichkeit, einen Analysepunkt zu löschen. Führen Sie diesen Befehl nur aus, wenn Sie vom Engineering-Team von Azure File Sync dazu aufgefordert werden. Bei der Ausführung dieses Befehls kann es zu Datenverlusten kommen. 

* <a id="afs-recall-file"></a>**Eine Datei, die ich verwenden möchte, wurde per Tiering ausgelagert. Wie kann ich die Datei auf den Datenträger zurückrufen, um sie lokal zu verwenden?**  
    Der einfachste Weg zum Zurückrufen einer Datei auf den Datenträger besteht darin, sie zu öffnen. Der Azure File Sync-Dateisystemfilter (StorageSync.sys) lädt die Datei nahtlos von Ihrer Azure-Dateifreigabe herunter, ohne dass Sie irgendetwas tun müssen. Bei Dateitypen, die teilweise gelesen werden können (z.B. Multimedia- oder ZIP-Dateien), wird beim Öffnen einer Datei nicht die gesamte Datei heruntergeladen.

    Sie können auch PowerShell nutzen, um für eine Datei das Zurückrufen zu erzwingen. Diese Option kann nützlich sein, wenn Sie mehrere Dateien auf einmal zurückrufen möchten, z.B. alle Dateien in einem Ordner. Öffnen Sie eine PowerShell-Sitzung auf dem Serverknoten, auf dem Azure File Sync installiert ist, und führen Sie anschließend die folgenden PowerShell-Befehle aus:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* <a id="afs-force-tiering"></a>**Wie kann ich das Tiering einer Datei oder eines Verzeichnisses erzwingen?**  
    Wenn das Cloudtiering-Feature aktiviert ist, wird beim Cloudtiering automatisch basierend auf dem letzten Zugriff und den Änderungszeiten gefiltert, um den auf dem Cloudendpunkt angegebenen freien Volumespeicherplatz zu erzielen. In einigen Fällen kann es auch erforderlich sein, das Tiering für eine Datei manuell zu erzwingen. Dies kann beispielsweise nützlich sein, wenn Sie eine große Datei speichern, die Sie für längere Zeit nicht mehr verwenden werden, und den freien Speicherplatz auf Ihrem Volume nun für andere Dateien und Ordner nutzen möchten. Sie können das Tiering mithilfe der folgenden PowerShell-Befehle erzwingen:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* <a id="afs-files-excluded"></a>**Welche Dateien oder Ordner werden automatisch von Azure File Sync ausgeschlossen?**  
    Azure File Sync schließt standardmäßig die folgenden Dateien aus:
    * desktop.ini
    * thumbs.db
    * ehthumbs.db
    * ~$\*.\*
    * \*.laccdb
    * \*.tmp
    * 635D02A9D91C401B97884B82B3BCDAEA.\*

    Die folgenden Ordner werden ebenfalls standardmäßig ausgeschlossen:

    * \System Volume Information
    * \$RECYCLE.BIN
    * \SyncShareState

* <a id="afs-os-support"></a>**Kann ich Azure File Sync entweder mit Windows Server 2008 R2, Linux oder meinem NAS-Gerät (Network Attached Storage) verwenden?**  
    Gegenwärtig unterstützt Azure File Sync ausschließlich Windows Server 2016 und Windows Server 2012 R2. Zurzeit sind keine anderen Optionen geplant. Je nach Kundennachfrage sind wir aber offen dafür, weitere Plattformen zu unterstützen. Teilen Sie uns über [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) mit, für welche Plattformen Sie Unterstützung von uns wünschen.

## <a name="security-authentication-and-access-control"></a>Sicherheit, Authentifizierung und Zugriffssteuerung
* <a id="ad-support"></a>**Unterstützt Azure Files die Active Directory-basierte Authentifizierung und Zugriffssteuerung?**  
    Azure Files bietet zwei Möglichkeiten zur Verwaltung der Zugriffssteuerung:

    - Sie können Shared Access Signatures (SAS) zum Generieren von Token verwenden, die über bestimmte Berechtigungen verfügen und für ein angegebenes Zeitintervall gültig sind. Sie können beispielsweise ein Token mit Lesezugriff auf eine bestimmte Datei generieren, das nach zehn Minuten abläuft. Jeder Benutzer, der während des Gültigkeitszeitraums über das Token verfügt, hat für einen Zeitraum von zehn Minuten Lesezugriff auf die Datei. Shared Access Signature-Schlüssel werden derzeit nur per REST-API oder in Clientbibliotheken unterstützt. Sie müssen die Azure-Dateifreigabe per SMB einbinden, indem Sie die Speicherkontoschlüssel verwenden.

    - Für Azure File Sync werden alle besitzerverwalteten ACLs bzw. DACLs (ob Active Directory-basiert oder lokal) für alle Serverendpunkte beibehalten und repliziert, für die die Synchronisierung durchgeführt wird. Da Windows Server bereits bei Active Directory authentifiziert werden kann, stellt Azure File Sync eine ausgezeichnete Überbrückungsmaßnahme dar, bis die volle Unterstützung für eine Active Directory-basierte Authentifizierung und ACL-Unterstützung bereitgestellt wird.

    Azure Files unterstützt Active Directory derzeit nicht direkt.

* <a id="encryption-at-rest"></a>**Wie kann ich sicherstellen, dass meine Azure-Dateifreigabe im ruhenden Zustand verschlüsselt wird?**  
    Azure Storage Service Encryption wird derzeit für alle Regionen standardmäßig aktiviert. Für diese Regionen müssen Sie nichts unternehmen, um die Verschlüsselung zu aktivieren. Informationen zu anderen Regionen finden Sie unter [Serverseitige Verschlüsselung](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="access-via-browser"></a>**Wie kann ich mithilfe eines Webbrowsers Zugriff auf eine bestimmte Datei gewähren?**  
    Sie können Shared Access Signatures zum Generieren von Token verwenden, die über bestimmte Berechtigungen verfügen und für ein angegebenes Zeitintervall gültig sind. Beispielsweise können Sie ein Token, das Lesezugriff auf eine bestimmte Datei ermöglicht, für einen festen Zeitraum generieren. Jeder Benutzer, der über diese URL verfügt, kann bis zum Ablauf der Tokengültigkeit direkt über einen beliebigen Webbrowser auf die Datei zugreifen. Sie können einen Shared Access Signature-Schlüssel über eine Benutzeroberfläche, z.B. Storage-Explorer, leicht generieren.

* <a id="file-level-permissions"></a>**Ist es möglich, Lese- oder Schreibberechtigungen für Ordner der Freigabe anzugeben?**  
    Wenn Sie die Dateifreigabe per SMB einbinden, haben Sie auf Ordnerebene keine Kontrolle über die Berechtigungen. Aber wenn Sie eine Shared Access Signature mit der REST-API oder Clientbibliotheken erstellen, können Sie für Ordner der Freigabe Berechtigungen für Lese- oder Schreibzugriff angeben.

* <a id="ip-restrictions"></a>**Können IP-Einschränkungen für eine Azure-Dateifreigabe implementiert werden?**  
    Ja. Der Zugriff auf Ihre Azure-Dateifreigabe kann auf Speicherkontoebene eingeschränkt werden. Weitere Informationen finden Sie unter [Konfigurieren von Firewalls und virtuellen Netzwerken in Azure Storage](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* <a id="data-compliance-policies"></a>**Welche Richtlinien zur Datenkonformität werden von Azure Files unterstützt?**  
   Azure Files wird zusätzlich zu der gleichen Speicherarchitektur ausgeführt, die auch in anderen Speicherdiensten in Azure Storage genutzt wird. Für Azure Files werden die gleichen Richtlinien zur Datenkonformität wie in anderen Azure-Speicherdiensten angewendet. Weitere Informationen zur Datenkonformität von Azure Storage finden Sie im herunterladbaren [Dokument zum Datenschutz in Microsoft Azure](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409) und im [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

## <a name="on-premises-access"></a>Lokaler Zugriff
* <a id="expressroute-not-required"></a>**Muss ich Azure ExpressRoute verwenden, wenn ich eine Verbindung mit Azure Files herstellen oder Azure File Sync lokal verwenden möchte?**  
    Nein. ExpressRoute ist für den Zugriff auf eine Azure-Dateifreigabe nicht erforderlich. Wenn Sie eine Azure-Dateifreigabe direkt lokal bereitstellen, muss lediglich Port 445 (TCP ausgehend) für den Internetzugriff geöffnet sein (dies ist der Port, über den SMB kommuniziert). Wenn Sie Azure File Sync verwenden, ist lediglich Port 443 (TCP ausgehend) für den HTTPS-Zugriff erforderlich (SMB ist nicht erforderlich). Sie *können* ExpressRoute aber mit beiden Zugriffsoptionen verwenden.

* <a id="mount-locally"></a>**Wie kann ich eine Azure-Dateifreigabe auf meinem lokalen Computer bereitstellen?**  
    Sie können die Dateifreigabe über das SMB-Protokoll bereitstellen, sofern Port 445 (TCP ausgehend) geöffnet ist und Ihr Client das SMB 3.0-Protokoll unterstützt (beispielsweise unter Windows 10 oder Windows Server 2016 der Fall). Wenn Port 445 durch eine Richtlinie Ihrer Organisation oder Ihres ISP blockiert ist, können Sie über Azure File Sync auf Ihre Azure-Dateifreigabe zugreifen.

## <a name="backup"></a>Backup 
* <a id="backup-share"></a>**Wie sichere ich meine Azure-Dateifreigabe?**  
    Sie können regelmäßige [Freigabemomentaufnahmen (Vorschau)](storage-how-to-use-files-snapshots.md) zum Schutz vor einer versehentlichen Löschung verwenden. Außerdem können Sie AzCopy, Robocopy oder ein Sicherungstool eines Drittanbieters nutzen, das zum Sichern einer eingebundenen Dateifreigabe geeignet ist. 

## <a name="share-snapshots"></a>Freigabemomentaufnahmen
### <a name="share-snapshots-general"></a>Freigabemomentaufnahmen: allgemein
* <a id="what-are-snaphots"></a>**Was sind Dateifreigaben-Momentaufnahmen?**  
    Sie können Momentaufnahmen von Azure-Dateifreigaben verwenden, um eine schreibgeschützte Version Ihrer Dateifreigaben zu erstellen. Außerdem können Sie Azure Files nutzen, um eine frühere Version Ihres Inhalts zurück auf dieselbe Freigabe, an einen anderen Speicherort in Azure oder zur weiteren Bearbeitung an einen lokalen Speicherort zu kopieren. Weitere Informationen zu Freigabemomentaufnahmen finden Sie unter [Freigabemomentaufnahme – Übersicht](storage-snapshots-files.md).

* <a id="where-are-snapshots-stored"></a>**Wo werden meine Freigabemomentaufnahmen gespeichert?**  
    Freigabemomentaufnahmen werden im gleichen Speicherkonto gespeichert wie die Dateifreigabe.

* <a id="snapshot-perf-impact"></a>**Sind bei der Nutzung von Freigabemomentaufnahmen Auswirkungen auf die Leistung zu erwarten?**  
    Freigabemomentaufnahmen verursachen kein Leistungsoverhead.

* <a id="snapshot-consistency"></a>**Sind Freigabemomentaufnahmen anwendungskonsistent?**  
    Nein. Freigabemomentaufnahmen sind nicht anwendungskonsistent. Benutzer müssen vor dem Erstellen von Freigabemomentaufnahmen die Schreibvorgänge von der Anwendung in die Freigabe leeren.

* <a id="snapshot-limits"></a>**Gibt es Beschränkungen hinsichtlich der Anzahl von Freigabemomentaufnahmen, die verwendet werden können?**  
    Ja. In Azure Files können maximal 200 Freigabemomentaufnahmen verwendet werden. Freigabemomentaufnahmen werden nicht zum Kontingent für die Dateifreigabe gezählt, sodass es für den von allen Freigabemomentaufnahmen genutzten Gesamtspeicherplatz keine Begrenzung pro Freigabe gibt. Die Beschränkungen für Speicherkonten gelten weiterhin. Wenn die Zahl von 200 Freigabemomentaufnahmen erreicht ist, müssen Sie ältere Momentaufnahmen löschen, um neue Freigabemomentaufnahmen erstellen zu können.

### <a name="create-share-snapshots"></a>Erstellen von Freigabemomentaufnahmen
* <a id="file-snaphsots"></a>**Kann ich Freigabemomentaufnahmen von den einzelnen Dateien erstellen?**  
    Freigabemomentaufnahmen werden auf der Dateifreigabeebene erstellt. Sie können einzelne Dateien über die Dateifreigabemomentaufnahme wiederherstellen, aber das Erstellen von Freigabemomentaufnahmen auf Dateiebene ist nicht möglich. Wenn Sie aber eine Freigabemomentaufnahme auf der Freigabeebene erstellt haben und Freigabemomentaufnahmen, bei denen sich eine bestimmte Datei geändert hat, auflisten möchten, können Sie hierfür die Option **Vorherige Versionen** auf einer bereitgestellten Windows-Freigabe nutzen. 
    
    Wenden Sie sich unter [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) an uns, falls Sie ein Feature für Dateimomentaufnahmen benötigen.

* <a id="encypted-snapshots"></a>**Kann ich Freigabemomentaufnahmen von verschlüsselten Dateifreigaben erstellen?**  
    Sie können eine Freigabemomentaufnahme von Azure-Dateifreigaben erstellen, bei der die Verschlüsselung ruhender Daten aktiviert ist. Sie können Dateien von einer Freigabemomentaufnahme in einer verschlüsselten Dateifreigabe wiederherstellen. Wenn Ihre Freigabe verschlüsselt ist, wird Ihre Freigabemomentaufnahme ebenfalls verschlüsselt.

* <a id="geo-redundant-snaphsots"></a>**Sind meine Freigabemomentaufnahmen georedundant?**  
    Freigabemomentaufnahmen weisen dieselbe Redundanz wie die Azure-Dateifreigabe auf, für die sie erstellt wurden. Wenn Sie für Ihr Konto einen georedundanten Speicher ausgewählt haben, wird Ihre Freigabemomentaufnahme ebenfalls redundant in der gekoppelten Region gespeichert.

### <a name="manage-share-snapshots"></a>Verwalten von Freigabemomentaufnahmen
* <a id="browse-snapshots-linux"></a>**Kann ich meine Freigabemomentaufnahmen unter Linux durchsuchen?**  
    Sie können mit Azure CLI 2.0 unter Linux Freigabemomentaufnahmen erstellen, auflisten, durchsuchen und wiederherstellen.

* <a id="copy-snapshots-to-other-storage-account"></a>**Kann ich die Freigabemomentaufnahmen in ein anderes Speicherkonto kopieren?**  
    Sie können Dateien von Freigabemomentaufnahmen an einen anderen Speicherort kopieren, aber nicht die Freigabemomentaufnahmen selbst.

### <a name="restore-data-from-share-snapshots"></a>Wiederherstellen von Daten aus Freigabemomentaufnahmen
* <a id="promote-share-snapshot"></a>**Kann ich eine Freigabemomentaufnahme auf die Basisfreigabe höher stufen?**  
    Sie können Daten aus einer Freigabemomentaufnahme an ein anderes Ziel kopieren. Eine Freigabemomentaufnahme kann nicht auf die Basisfreigabe höher gestuft werden.

* <a id="restore-snapshotted-file-to-other-share"></a>**Kann ich Daten aus meiner Freigabemomentaufnahme in einem anderen Speicherkonto wiederherstellen?**  
    Ja. Dateien einer Freigabemomentaufnahme können an den ursprünglichen Speicherort oder einen anderen Speicherort kopiert werden, der entweder dasselbe Speicherkonto oder ein anderes Speicherkonto in derselben oder in anderen Regionen enthält. Außerdem können Sie Dateien an einen lokalen Speicherort oder in eine beliebige andere Cloud kopieren.    
  
### <a name="clean-up-share-snapshots"></a>Bereinigen von Freigabemomentaufnahmen
* <a id="delete-share-keep-snapshots"></a>**Kann ich meine Freigabe löschen, Freigabemomentaufnahmen aber nicht?**  
    Wenn Sie auf Ihrer Freigabe über aktive Freigabemomentaufnahmen verfügen, können Sie die Freigabe nicht löschen. Sie können eine API verwenden, um Freigabemomentaufnahmen zusammen mit der Freigabe zu löschen. Außerdem können Sie die Freigabemomentaufnahmen und die Freigabe im Azure-Portal löschen.

* <a id="delete-share-with-snapshots"></a>**Was geschieht mit meinen Freigabemomentaufnahmen, wenn ich mein Speicherkonto lösche?**  
    Wenn Sie Ihr Speicherkonto löschen, werden die Freigabemomentaufnahmen ebenfalls gelöscht.

## <a name="billing-and-pricing"></a>Abrechnung und Preise
* <a id="vm-file-share-network-traffic"></a>**Zählt der Netzwerkdatenverkehr zwischen einer Azure-VM und einer Dateifreigabe als externe Bandbreite, die für das Abonnement berechnet wird?**  
    Wenn sich die Dateifreigabe und die VM in derselben Azure-Region befinden, fallen für den Datenverkehr zwischen der Dateifreigabe und der VM keine zusätzlichen Kosten an. Wenn sich die Dateifreigabe und die VM in verschiedenen Regionen befinden, wird der dazwischen ausgetauschte Datenverkehr als externe Bandbreite berechnet.

* <a id="share-snapshot-price"></a>**Wie viel kosten Freigabemomentaufnahmen?**  
     Während der Vorschauphase fallen keine Gebühren für die Kapazität von Freigabemomentaufnahmen an. Für regulären ausgehenden Speicher und Transaktionen fallen die üblichen Kosten an. Wenn die allgemeine Verfügbarkeit erreicht ist, werden für Abonnements sowohl die Kapazität als auch Transaktionen für Freigabemomentaufnahmen berechnet.
     
     Freigabemomentaufnahmen sind in der Regel inkrementell. Bei der Basisfreigabemomentaufnahme handelt es sich um die Freigabe selbst. Alle nachfolgenden Freigabemomentaufnahmen sind inkrementell und enthalten nur die Differenzdaten gegenüber der vorherigen Freigabemomentaufnahme. Ihnen wird nur der geänderte Inhalt in Rechnung gestellt. Wenn Sie über eine Freigabe mit 100 GiB an Daten verfügen, aber nur 5 GiB nach Ihrer letzten Freigabemomentaufnahme geändert wurden, belegt die Freigabemomentaufnahme nur zusätzliche 5 GiB, und Ihnen werden 105 GiB berechnet. Weitere Informationen zu Gebühren für Transaktionen und Standardausgangsgebühren finden Sie auf der Seite [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="scale-and-performance"></a>Skalierung und Leistung
* <a id="files-scale-limits"></a>**Welche Skalierungsgrenzwerte gelten für Azure Files?**  
    Informationen zu Skalierbarkeits- und Leistungszielen für Azure Files finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](storage-files-scale-targets.md).

* <a id="need-larger-share"></a>**Ich benötige eine größere Dateifreigabe, die für Azure Files derzeit noch nicht angeboten wird. Kann ich die Größe meiner Azure-Dateifreigabe erhöhen?**  
    Nein. Die maximale Größe einer Azure-Dateifreigabe beträgt 5 TiB. Zurzeit ist dies eine feste Grenze, die wir nicht überschreiten können. Wir arbeiten an einer Lösung, um die Freigabegröße auf 100 TiB zu erhöhen, können hierfür derzeit aber keinen Zeitplan nennen.

* <a id="open-handles-quota"></a>**Wie viele Clients können gleichzeitig auf dieselbe Datei zugreifen?**   
    Für eine einzelne Datei gilt ein Kontingent von 2.000 geöffneten Handles. Wenn Sie über 2.000 geöffnete Handles verfügen, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass das Kontingent erreicht ist.

* <a id="zip-slow-performance"></a>**Die Leistung verlangsamt sich, wenn ich in Azure Files Dateien entzippe. Wie soll ich vorgehen?**  
    Zum Übertragen einer großen Zahl von Dateien nach Azure Files empfehlen wir Ihnen die Verwendung von AzCopy (für Windows, in der Vorschauphase für Linux und UNIX) oder Azure PowerShell. Diese Tools wurden für die Netzwerkübertragung optimiert.

* <a id="slow-perf-windows-81-2012r2"></a>**Warum verschlechtert sich die Leistung, nachdem ich meine Azure-Dateifreigabe unter Windows Server 2012 R2 oder Windows 8.1 bereitgestellt habe?**  
    Es gibt ein bekanntes Problem, das beim Bereitstellen einer Azure-Dateifreigabe unter Windows Server 2012 R2 und Windows 8.1 auftritt. Für das Problem wurde im April 2014 ein kumulatives Update für Windows 8.1 und Windows Server 2012 R2 als Patch veröffentlicht. Stellen Sie zur Erzielung einer optimalen Leistung sicher, dass dieser Patch für alle Instanzen von Windows Server 2012 R2 und Windows 8.1 angewendet wurde. (Sie sollten Windows-Patches immer per Windows Update erhalten.) Weitere Informationen finden Sie im zugehörigen Microsoft Knowledge Base-Artikel [Niedrige Leistung beim Zugriff auf Azure Files von Windows 8.1 oder Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Features und Interoperabilität mit anderen Diensten
* <a id="cluster-witness"></a>**Kann ich meine Azure-Dateifreigabe als *Dateifreigabenzeugen* für meinen Windows Server-Failovercluster verwenden?**  
    Diese Konfiguration wird für eine Azure-Dateifreigabe derzeit nicht unterstützt. Weitere Informationen zum Einrichten dieser Funktion für den Azure Blob Storage finden Sie unter [Bereitstellen eines Cloudzeugen für einen Failovercluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* <a id="containers"></a>**Kann ich eine Azure-Dateifreigabe in einer Azure-Containerinstanz bereitstellen?**  
    Ja. Azure-Dateifreigaben sind eine gute Möglichkeit, um Informationen über die Lebensdauer einer Containerinstanz hinaus dauerhaft zu speichern. Weitere Informationen finden Sie unter [Einbinden einer Azure-Dateifreigabe in Azure Container Instances](../../container-instances/container-instances-mounting-azure-files-volume.md).

* <a id="rest-rename"></a>**Steht in der REST-API ein Umbenennungsvorgang zur Verfügung?**  
    Derzeit leider nicht.

* <a id="nested-shares"></a>**Kann ich geschachtelte Freigaben einrichten? Anders ausgedrückt: Kann eine Freigabe unter einer Freigabe angeordnet werden?**  
    Nein. Die Dateifreigabe *ist* der virtuelle Treiber, den Sie bereitstellen können. Geschachtelte Freigaben werden nicht unterstützt.

* <a id="ibm-mq"></a>**Wie kann ich Azure Files mit IBM MQ nutzen?**  
    IBM hat ein Dokument mit einer Beschreibung veröffentlicht, wie IBM MQ-Kunden Azure Files mit dem IBM-Dienst konfigurieren können. Weitere Informationen finden Sie unter [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)(Gewusst wie: Einrichten eines IBM MQ-Warteschlangen-Managers für mehrere Instanzen mit dem Microsoft Azure Files-Dienst).

## <a name="see-also"></a>Weitere Informationen
* [Behandeln von Azure Files-Problemen unter Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Behandeln von Azure Files-Problemen unter Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Problembehandlung bei Azure File Sync (Vorschau)](storage-sync-files-troubleshoot.md)
