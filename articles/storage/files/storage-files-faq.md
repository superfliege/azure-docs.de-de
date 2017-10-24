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
ms.date: 10/08/2017
ms.author: renash
ms.openlocfilehash: cbe3c3a8f223a4bcc05e19b7a723cf5aff413527
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-about-azure-files"></a>Häufig gestellte Fragen zu Azure Files

## <a name="general"></a>Allgemein
* **Was ist Azure Files?**  
    Azure Files bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll [Server Message Block (SMB)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) (auch als Common Internet File System oder CIFS bezeichnet) zugegriffen werden kann. Azure-Dateifreigaben können gleichzeitig durch die Cloud oder lokale Bereitstellungen von Windows, Linux und macOS eingebunden werden.

* **Warum ist Azure Files nützlich?**  
   Azure Files ermöglicht Ihnen, Dateifreigaben in der Cloud zu erstellen, ohne den Overhead eines physischen Servers oder Geräts verwalten zu müssen. Somit müssen Sie weniger Zeit für die Anwendung von Betriebssystemupdates und den Austausch defekter Festplatten aufwenden – denn all diese monotonen Arbeiten erledigen wir für Sie. Um mehr über die Szenarien zu erfahren, in denen Azure Files Abhilfe leisten kann, lesen Sie [Nützlichkeit von Azure Files](storage-files-introduction.md#why-azure-files-is-useful).

* **Auf welche Arten kann auf Dateien in Azure Files zugegriffen werden?**  
    Sie können die Dateifreigabe mithilfe des SMB 3.0-Protokolls auf Ihrem lokalen Computer einbinden oder mithilfe von Tools wie dem [Speicher-Explorer](http://storageexplorer.com/) auf Dateien in Ihrer Dateifreigabe zugreifen. In Ihrer Anwendung können Sie über Speicherclientbibliotheken, REST-APIs oder PowerShell auf Ihre Dateien in der Azure-Dateifreigabe zugreifen.

* **Was ist Azure File Sync?**  
    Mit Azure File Sync (Vorschau) können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Dies erfolgt durch Umwandeln der Windows-Server in einen Schnellcache der Azure-Dateifreigabe. Sie können alle unter Windows Server verfügbaren Protokolle für den lokalen Zugriff auf Ihre Daten (einschließlich SMB, NFS und FTPS) sowie beliebig viele Caches weltweit verwenden.

* **Warum sollte ich für meine Daten eine Azure-Dateifreigabe im Vergleich zu einem Azure Blob Storage verwenden?**  
    Azure Files und Azure Blob Storage bieten beide die Möglichkeit, große Datenmengen in der Cloud zu speichern, unterscheiden sich in ihrem Einsatzzweck jedoch etwas voneinander. Der Azure Blob Storage ist für umfangreiche native Cloudanwendungen geeignet, die unstrukturierte Daten speichern müssen. In Bezug auf die Maximierung von Leistung und Skalierbarkeit stellt der Azure Blob Storage eine einfachere Speicherabstraktion dar als ein echtes Dateisystem. Darüber hinaus kann auf den Azure Blob Storage nur über REST-basierte Clientbibliotheken (oder direkt über das REST-basierte Protokoll) zugegriffen werden.

    Azure Files hingegen ist speziell als Dateisystem mit sämtlichen Dateiabstrakten konzipiert, die Sie aus ihrer jahrelangen Arbeit mit lokalen Betriebssystemen kennen und bevorzugen. Wie der Azure Blob Storage bietet auch Azure Files eine REST-Schnittstelle und REST-basierte Clientbibliotheken, gewährt jedoch im Gegensatz zum Azure Blob Storage darüber hinaus SMB-Zugriff auf Azure-Dateifreigaben. So können Sie eine Azure-Dateifreigabe direkt auf Windows-, Linux-, macOS-, lokale oder Cloud-VMs bereitstellen, ohne Codes schreiben oder spezielle Treiber zum Dateisystem hinzufügen zu müssen. Außerdem können Azure-Dateifreigaben auf lokalen Dateiservern per Azure File Sync zwischengespeichert werden, um in der Nähe des Datennutzungsorts für schnellen Zugriff zu sorgen. 
   
    Eine ausführliche Erläuterung der Unterschiede zwischen Azure Files und dem Azure Blob Storage finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Weitere Informationen zum Azure Blob Storage finden Sie unter [Einführung in Blob Storage](../blobs/storage-blobs-introduction.md).

* **Warum sollte ich eine Azure-Dateifreigabe im Vergleich zu Azure-Datenträgern verwenden?**  
    Bei einem Azure-Datenträger handelt es sich einfach ausgedrückt um einen Datenträger. Ein eigenständiger Datenträger allein bietet keinen großen Nutzen. Um von einem Azure-Datenträger zu profitieren, müssen Sie diesen an einen in Azure ausgeführten virtuellen Computer anfügen. Azure-Datenträger können für verschiedenste Einsatzzwecke verwendet werden, für die ein Datenträger für einen lokalen Server eingesetzt wird: als Betriebssystemsystemdatenträger, als Auslagerungsbereich für ein Betriebssystem oder als dedizierter Speicher für eine Anwendung. Ein interessanter Einsatzzweck von Azure-Datenträgern ist die Erstellung eines Dateiservers in der Cloud, der genau in denselben Situationen verwendet wird wie bei einer Azure-Dateifreigabe. Die Bereitstellung eines Dateiservers in Azure-VMs ist eine hervorragende für hohe Leistung ausgelegte Methode, um Dateispeicher in Azure freizugeben, wenn Sie derzeit nicht von Azure Files unterstützte Bereitstellungsoptionen benötigen (z.B. NFS-Protokollunterstützung oder Storage Premium). 

    Andererseits ist der Betrieb eines Dateiservers mit Azure-Datenträgern als Back-End-Speicher in der Regel deutlich kostspieliger als die Verwendung einer Azure-Dateifreigabe. Hierfür gibt es mehrere Gründe: Zunächst einmal müssen Sie nicht nur für die Kosten des Datenträgerspeichers aufkommen, sondern auch für die Ausgaben zum Betrieb von einer oder mehreren Azure-VMs. Des Weiteren müssen Sie auch die VMs für den Betrieb des Dateiservers verwalten. Beispielsweise würde die Durchführung von Betriebssystemupgrades usw. Ihnen obliegen. Zu guter Letzt gilt auch: Wenn Daten lokal zwischengespeichert werden müssen, sind Sie für die Einrichtung und Verwaltung der Replikationstechnologien (z.B. DFS-Replikation) zuständig.

    Eine interessante Vorgehensweise, um das Beste aus beiden Welten – nämlich von Azure Files und einem Dateiserver, der auf Azure-VMs mit Azure-Datenträgern als Back-End-Speicher gehostet wird – zu erhalten, besteht darin, Azure File Sync auf Ihrem von einer Cloud-VM gehosteten Dateiserver zu installieren. Wenn sich die Azure-Dateifreigabe in derselben Region wie Ihr Dateiserver befindet, können Sie das Cloudtiering aktivieren und den Prozentsatz des freien Volumespeicherplatzes auf den Maximalwert (99 %) festlegen. Dies stellt nicht nur eine minimale Duplizierung von Daten sicher, sondern ermöglicht Ihnen gleichzeitig, beliebige Anwendungen auf Ihren Dateiservern zu verwenden (z.B. Anwendungen, die NFS-Protokollunterstützung erfordern).

    Informationen zu einer Möglichkeit, wie Sie einen leistungsstarken und hochverfügbaren Cluster in Azure einrichten, finden Sie unter [Deploying IaaS VM Guest Clusters in Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/) (Bereitstellen von IaaS-VM-Gastclustern in Microsoft Azure). Eine ausführliche Erläuterung der Unterschiede zwischen Azure Files und Azure-Datenträgern finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Weitere Informationen zu Azure-Datenträgern finden Sie unter [Azure Managed Disks – Übersicht](../../virtual-machines/windows/managed-disks-overview.md).

* **Wie kann ich mich mit Azure Files vertraut machen?**  
    Die ersten Schritte mit Azure Files sind einfach: [Erstellen Sie einfach eine Dateifreigabe](storage-how-to-create-file-share.md), und stellen Sie sie unter dem gewünschten Betriebssystem bereit: 

    - [Bereitstellen unter Windows](storage-how-to-use-files-windows.md)
    - [Bereitstellen unter Linux](storage-how-to-use-files-linux.md)
    - [Bereitstellen unter macOS](storage-how-to-use-files-mac.md)

    Eine ausführlichere Anleitung zum Bereitstellen einer Azure-Dateifreigabe für den Ersatz von Produktionsdateifreigaben in Ihrer Organisation finden Sie unter [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md).

* **Welche Speicherredundanzoptionen werden von Azure Files unterstützt?**  
    Azure Files unterstützt derzeit ausschließlich lokal redundanten Speicher (LRS) und georedundanten Speicher (GRS). Zukünftig ist geplant, Unterstützung für zonenredundanten Speicher (ZRS) und georedundanten Speicher mit Lesezugriff (Read-Access Geo Redundant Storage, RA-GRS) zu bieten. Zum gegenwärtigen Zeitpunkt stehen jedoch keine Zeitpläne zur Verfügung.

* **Ich möchte unbedingt, dass Feature *x* in Azure Files eingeführt wird. Ist dies möglich?**  
    Ja, das Azure Files-Team legt großen Wert darauf, Ihre Feedbacks zu unserem Dienst zu erfahren. Stimmen Sie in [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) über Funktionsanfragen ab! Wir hoffen, dass wir Sie mit vielen neuen Funktionen begeistern können.

## <a name="azure-file-sync"></a>Azure-Dateisynchronisierung
* **Welche Regionen werden derzeit für Azure File Sync (Vorschau) unterstützt?**  
    Azure File Sync ist derzeit in den Regionen „USA, Westen“, „Europa, Westen“, „Australien, Osten“ und „Asien, Südosten“ verfügbar. Im Rahmen unserer Bemühungen zur Bereitstellung einer allgemeinen Verfügbarkeit werden wir die Unterstützung auf weitere Regionen erweitern. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](storage-sync-files-planning.md#region-availability).

* **Kann ich in Domänen eingebundene Server und nicht in Domänen eingebundene Server in derselben Synchronisierungsgruppe verwenden?**  
    Ja, eine Synchronisierungsgruppe kann Serverendpunkte mit unterschiedlichen Active Directory-Mitgliedschaften umfassen, einschließlich nicht in Domänen eingebundener Endpunkte. Die Konfiguration funktioniert zwar technisch einwandfrei, es wird jedoch davon abgeraten, diese als normale Konfiguration zu verwenden, da für Dateien bzw. Ordner auf einem Server definierte ACLs möglicherweise nicht von anderen Servern in der Synchronisierungsgruppe erzwungen werden können. Um optimale Ergebnisse zu erhalten, wird empfohlen, Server in derselben Active Directory-Gesamtstruktur, Server in verschiedenen Active Directory-Gesamtstrukturen mit hergestellten Vertrauensbeziehungen oder nicht in Domänen eingebundene Server zu synchronisieren. Die oben genannten Möglichkeiten sollten jedoch nicht miteinander kombiniert werden.

* **Ich habe eine Datei direkt über SMB oder über das Portal in meiner Azure-Dateifreigabe erstellt. Wie lange dauert es, bis die Datei mit den Servern in der Synchronisierungsgruppe synchronisiert ist?** 
    Wenn Sie mit dem Azure-Portal, SMB oder REST Dateien in einer Azure-Dateifreigabe erstellen oder ändern, kann es bis zu 24 Stunden dauern, bis die Änderungen an den Servern in der Synchronisierungsgruppe synchronisiert sind.

* **Was geschieht, wenn dieselbe Datei ungefähr zum selben Zeitpunkt auf zwei Servern geändert wird?**  
    Azure File Sync verwendet eine simple Konfliktauflösungsstrategie: Beide Änderungen werden beibehalten. Die Datei, in die zuletzt geschrieben wurde, behält den ursprünglichen Dateinamen. Bei der älteren Datei werden der „Quellcomputer“ und die Konfliktnummer mit folgender Taxonomie an den Namen angefügt: 
   
    `<FileNameWithoutExtension>-<MachineName>[-#].<ext>`  

    Beispielsweise würde `CompanyReport.docx` beim ersten Konflikt in `CompanyReport-CentralServer.docx` geändert werden, wenn der vorherige Schreibvorgang in `CentralServer` stattfand. Beim zweiten Konflikt würde der Name `CompanyReport-CentralServer-1.docx` lauten.

* **Warum erhalte ich bei der Registrierung eines Servers zahlreiche Antworten, dass eine Website nicht vertrauenswürdig sei?**  
    Dieser Fehler tritt auf, weil die Richtlinie **Erhöhte Internet Explorer-Sicherheit** bei der Serverregistrierung aktiviert war. Weitere Informationen zum ordnungsgemäßen Deaktivieren der Richtlinie **Erhöhte Internet Explorer-Sicherheit** finden Sie unter [Vorbereiten von Windows-Servern für die Verwendung von Azure File Sync](storage-sync-files-deployment-guide.md#prepare-windows-servers-for-use-with-azure-file-sync) und [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

* **Wird georedundanter Speicher (GRS) von Azure File Sync unterstützt?**  
    Ja, Azure File Sync unterstützt sowohl lokal redundanten Speicher (LRS) als auch georedundanten Speicher (GRS). Bei einem GRS-Failover zwischen gekoppelten Regionen wird empfohlen, die neue Region lediglich als Datensicherung zu behandeln. Azure File Sync beginnt nicht automatisch die Synchronisierung mit der neuen primären Region. 

* **Warum stimmt die Eigenschaft *Größe auf Festplatte* einer Datei nach der Verwendung von Azure File Sync nicht mit der Eigenschaft *Größe* überein?**  
    Der Windows-Datei-Explorer verwendet zwei Eigenschaften zur Darstellung einer Dateigröße: **Größe** und **Größe auf Festplatte**. Diese Eigenschaften unterscheiden sich in ihrer Bedeutung nur geringfügig voneinander: Während **Größe** die Gesamtgröße der Datei darstellt, bezieht sich **Größe auf Festplatte** auf die Größe des tatsächlich auf der Festplatte gespeicherten Dateidatenstroms. Diese Abweichung kann auf verschiedene Ursachen zurückgeführt werden, z.B. eine Komprimierung, eine Datendeduplizierung oder in unserem Fall ein Cloudtiering mit Azure File Sync. Wenn eine Datei per Tiering auf eine Azure-Dateifreigabe ausgelagert wird, ist die Größe auf der Festplatte gleich Null, da der Dateidatenstrom nicht auf der Festplatte, sondern in Ihrer Azure-Dateifreigabe gespeichert wird. Eine Datei kann auch zum Teil per Tiering ausgelagert (bzw. teilweise zurückgerufen) werden, d.h., dieser Teil der Datei befindet sich auf der Festplatte. Dies kann der Fall sein, wenn Dateien teilweise von Anwendungen wie Multimediaplayern oder Komprimierungshilfsprogrammen gelesen werden. 

* **Woran erkenne ich, ob eine Datei per Tiering ausgelagert wurde?**  
    Es gibt mehrere Möglichkeiten, um zu überprüfen, ob eine Datei per Tiering auf Ihre Azure-Dateifreigabe ausgelagert wurde:
    
    1. **Überprüfen Sie die Dateiattribute in der Datei.** Klicken Sie hierzu mit der rechten Maustaste auf eine Datei, navigieren Sie zu **Details**, und scrollen Sie nach unten zur Eigenschaft **Attribute**. In einer per Tiering ausgelagerten Datei sind folgende Attribute festgelegt:     
        
        | Attributbuchstabe | Attribut | Definition |
        |:----------------:|-----------|------------|
        | A | Archivieren | Gibt an, dass die Datei durch Sicherungssoftware gesichert werden sollte. Dieses Attribut ist immer festgelegt, unabhängig davon, ob die Datei per Tiering ausgelagert oder vollständig auf dem Datenträger gespeichert wurde. |
        | P | Sparsedatei | Gibt an, dass es sich bei der Datei um eine Sparsedatei handelt – einen speziellen Dateityp, der von NTFS zur effizienten Verwendung bei einem nahezu leeren Datenträgerdatenstrom einer Datei bereitgestellt wird. Azure File Sync verwendet Sparsedateien entweder, weil eine Datei vollständig per Tiering ausgelagert wurde (d.h., der Dateidatenstrom wird nur in der Cloud gespeichert), oder weil eine Datei teilweise zurückgerufen wurde (d.h., ein Teil der Datei befindet sich bereits auf dem Datenträger). Wenn eine Datei vollständig auf die Festplatte zurückgerufen wird, konvertiert Azure File Sync diese Sparedatei in eine normale Datei. |
        | L | Analysepunkt | Gibt an, dass die Datei einen Analysepunkt enthält, der als spezieller Zeiger für die Verwendung durch einen Dateisystemfilter fungiert. Azure File Sync verwendet Anaysepunkte, um den Speicherort für den Azure File Sync-Dateisystemfilter (StorageSync.sys) zu definieren, an dem die Datei in der Cloud gespeichert ist. Dies ermöglicht einen nahtlosen Zugriff, ohne dass Ihr Endbenutzer wissen muss, ob Azure File Sync verwendet wird oder wie der Zugriff auf die Datei in Ihrer Azure-Dateifreigabe erfolgt. Wenn eine Datei vollständig zurückgerufen wird, entfernt Azure File Sync den Analysepunkt aus der Datei. |
        | O | Offline | Gibt an, dass ein Teil des Dateiinhalts oder der gesamte Dateiinhalt nicht auf dem Datenträger gespeichert ist. Wenn eine Datei vollständig zurückgerufen wird, entfernt Azure File Sync dieses Attribut. |

        ![Dialogfeld „Eigenschaften“ einer Datei mit ausgewählter Registerkarte „Details“](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Es können auch die Attribute für alle Dateien in einem Ordner angezeigt werden, indem das Feld **Attribute** zur Tabellenanzeige des Datei-Explorers hinzugefügt wird. Klicken Sie dazu mit der rechten Maustaste auf eine vorhandene Spalte (z.B. „Größe“). Wählen Sie anschließend **Mehr** und in der Dropdownliste **Attribute** aus.
        
    2. **Verwenden Sie `fsutil`, um nach Analysepunkten in einer Datei zu suchen.** Wie in der vorherigen Option erwähnt, ist für eine per Tiering ausgelagerte Datei immer ein Analysepunkt oder ein spezieller Zeiger für den Azure File Sync-Dateisystemfilter (StorageSync.sys) festgelegt. Mit dem Hilfsprogramm `fsutil` an einer Eingabeaufforderung mit erhöhten Rechten oder in einer PowerShell-Sitzung können Sie überprüfen, ob eine Datei einen Analysepunkt aufweist:
    
        ```PowerShell
        fsutil reparsepoint query <your-file-name>
        ```

        Falls die Datei einen Analysepunkt enthält, sollte **Analyseerkennungswert: 0x8000001e** angezeigt werden. Dieser Hexadezimalwert ist der Analysepunktwert, der Azure File Sync zuzuordnen ist. Die Ausgabe enthält auch die Analysedaten, die den Pfad zu Ihrer Datei in Ihrer Azure-Dateifreigabe darstellen.

        > [!Warning]  
        > Der Hilfsprogrammbefehl `fsutil reparsepoint` bietet außerdem die Möglichkeit, einen Analysepunkt zu löschen. Führen Sie diesen Befehl nur aus, wenn Sie vom Azure File Sync-Technikteam dazu aufgefordert werden, da es sonst zu einem Datenverlust kommen kann. 

* **Eine Datei, die ich verwenden möchte, wurde per Tiering verschoben. Wie kann ich sie zur lokalen Verwendung auf die Festplatte zurückrufen?**  
    Der einfachste Weg zum Zurückrufen einer Datei auf die Festplatte besteht darin, sie zu öffnen. Der Azure File Sync-Dateisystemfilter (StorageSync.sys) lädt die Datei nahtlos von Ihrer Azure-Dateifreigabe herunter, ohne dass Sie irgendetwas tun müssen. Bei Dateitypen, die teilweise gelesen werden können (z.B. Multimedia- oder ZIP-Dateien), wird beim Öffnen einer Datei nicht die gesamte Datei heruntergeladen.

    Das Zurückrufen einer Datei kann auch mithilfe von PowerShell erzwungen werden. Dies kann beispielsweise hilfreich sein, wenn Sie viele Dateien gleichzeitig (z.B. alle Dateien in einem Ordner) zurückrufen möchten. Öffnen Sie eine PowerShell-Sitzung auf dem Serverknoten, auf dem Azure File Sync installiert ist, und führen Sie die folgenden PowerShell-Befehle aus:
    
    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncFileRecall -Path <file-or-directory-to-be-recalled>
    ```

* **Wie kann ich das Tiering einer Datei oder eines Verzeichnisses erzwingen?**  
    Bei aktivierter Cloudtieringfunktion werden Dateien automatisch gemäß des letzten Zugriffs per Tiering verschoben und die Zeiten werden geändert, um den auf dem Cloudendpunkt angegebenen Prozentsatz des freien Volumespeicherplatzes freizugeben. In manchen Fällen sollten Sie das Tiering einer Datei jedoch manuell erzwingen. Dies kann z.B. nützlich sein, wenn Sie eine große Datei speichern, die Sie für längere Zeit nicht mehr verwenden werden, und den freien Speicherplatz auf Ihrem Volume nun für andere Dateien bzw. Ordner nutzen möchten. Sie können das Tiering mithilfe der folgenden PowerShell-Befehle erzwingen:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
    ```

* **Warum wird eine Datei oder ein Ordner von Azure File Sync ausgeschlossen?**
    Azure File Sync schließt standardmäßig die folgenden Dateien aus:
    
    - desktop.ini
    - thumbs.db
    - ehthumbs.db
    - ~$\*.\*
    - \*.laccdb
    - \*.tmp
    - 635D02A9D91C401B97884B82B3BCDAEA.\*

    Die folgenden Ordner werden ebenfalls standardmäßig ausgeschlossen:

    - \System Volume Information
    - \$RECYCLE.BIN
    - \SyncShareState

* **Ich möchte Azure File Sync mit Windows Server 2008 R2, mit Linux oder mit meinem NAS-Gerät verwenden. Wird dies von Azure File Sync unterstützt?**
    Gegenwärtig unterstützt Azure File Sync ausschließlich Windows Server 2016 und Windows Server 2012 R2. Zurzeit sind keine anderen Optionen geplant. Je nach Kundennachfrage sind wir jedoch bereit und interessiert daran, weitere Plattformen zu unterstützen. Teilen Sie uns über [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) mit, für welche Plattformen Sie Unterstützung von uns wünschen.

## <a name="security-authentication-and-access-control"></a>Sicherheit, Authentifizierung und Zugriffssteuerung
* **Unterstützt Azure Files die Active Directory-basierte Authentifizierung und Zugriffssteuerung?**  
    Azure Files unterstützt derzeit nicht die AD-basierte (Active Directory-basierte) Authentifizierung oder ACLs, doch wir arbeiten aktiv darauf hin. Momentan gibt es zwei Möglichkeiten, das Problem der fehlenden AD-basierten Authentifizierung und Zugriffssteuerung zu umgehen:

    - Mit SAS können Sie Token mit bestimmten Berechtigungen generieren, die für ein bestimmtes Zeitintervall gültig sind. So können Sie beispielsweise für eine bestimmte Datei ein Token mit Lesezugriff generieren, das nach zehn Minuten abläuft. Jeder Benutzer, der über dieses Token verfügt, hat für einen Zeitraum von zehn Minuten Lesezugriff auf die Datei. SAS-Schlüssel werden derzeit nur über die REST-API oder über Clientbibliotheken unterstützt. Sie müssen die Azure-Dateifreigabe über SMB mit den Speicherkontoschlüsseln bereitstellen.

    - Azure File Sync behält alle ACLs (AD-basiert oder lokal) auf allen Serverendpunkten bei, mit denen es synchronisiert wird, und repliziert diese. Da Windows Server bereits bei Active Directory authentifiziert werden kann, kann Azure File Sync eine ausgezeichnete Überbrückungsmaßnahme darstellen, bis volle Unterstützung für eine AD-basierte Authentifizierung und ACL-Unterstützung geboten wird.

* **Wie kann ich sicherstellen, dass meine ruhende Azure-Dateifreigabe verschlüsselt aufbewahrt wird?**  
    Sie müssen nichts tun, um die Verschlüsselung zu aktivieren. Die [serverseitige Verschlüsselung](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) ist nun standardmäßig für Azure Files in allen öffentlichen Regionen und nationalen Clouds aktiviert. 

* **Wie kann ich mithilfe eines Webbrowsers Zugriff auf eine bestimmte Datei gewähren?**  
    Mit SAS können Sie Token mit bestimmten Berechtigungen generieren, die für ein bestimmtes Zeitintervall gültig sind. Beispielsweise können Sie ein Token mit Lesezugriff auf eine bestimmte Datei für einen bestimmten Zeitraum generieren. Jeder, der über diese URL verfügt, kann bis zum Ablauf der Gültigkeit direkt über einen beliebigen Webbrowser auf die Datei zugreifen. SAS-Schlüssel können problemlos über eine Benutzeroberfläche wie Speicher-Explorer generiert werden.

* **Ist es möglich, Lese- oder Schreibberechtigungen für Ordner der Freigabe anzugeben?**  
    Sie verfügen nicht über dieses Maß an Kontrolle über Berechtigungen, wenn Sie die Dateifreigabe per SMB bereitstellen. Sie können dies aber erreichen, indem Sie über die REST-API oder Clientbibliotheken eine Shared Access Signature (SAS) erstellen.
    
* **Welche Datenkonformitätsrichtlinien werden für Azure Files unterstützt?**  
   Da Azure Files auf der gleichen Speicherarchitektur basiert wie andere Speicherdienste in Azure Storage, kommen auch die gleichen Datenkonformitätsrichtlinien zur Anwendung. Weitere Informationen zur Datenkonformität von Azure Storage finden Sie im herunterladbaren [Dokument zum Datenschutz in Microsoft Azure](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Lokaler Zugriff
* **Muss ich Azure ExpressRoute verwenden, wenn ich eine Verbindung mit Azure Files herstellen oder Azure File Sync lokal verwenden möchte?**  
    Nein, ExpressRoute ist für den Zugriff auf eine Azure-Dateifreigabe nicht erforderlich. Wenn Sie eine Azure-Dateifreigabe direkt lokal bereitstellen, muss lediglich Port 445 (TCP ausgehend) für den Internetzugriff geöffnet sein (dies ist der Port, über den SMB kommuniziert). Wenn Sie Azure File Sync verwenden, ist lediglich Port 443 (TCP ausgehend) für den HTTPS-Zugriff erforderlich (SMB ist nicht erforderlich). Bei Bedarf kann ExpressRoute jedoch auch mit beiden Zugriffsoptionen verwendet werden.

* **Wie kann ich eine Azure-Dateifreigabe auf meinem lokalen Computer bereitstellen?**  
    Sie können die Dateifreigabe über das SMB-Protokoll bereitstellen, solange Port 445 (TCP ausgehend) geöffnet ist und Ihr Client das SMB 3.0-Protokoll unterstützt (was beispielsweise unter Windows 10 oder Windows Server 2016 der Fall ist). Wenn Port 445 durch eine Richtlinie Ihrer Organisation oder Ihres ISP blockiert ist, können Sie über Azure File Sync auf Ihre Azure-Dateifreigabe zugreifen.

## <a name="backup"></a>Backup 
* **Wie sichere ich meine Azure-Dateifreigabe?**  
    Sie können regelmäßige Freigabemomentaufnahmen (Vorschau) zum Schutz vor einer versehentlichen Löschung verwenden. Sie können AzCopy, RoboCopy oder ein Sicherungstool eines Drittanbieters verwenden, das zum Sichern einer eingebundenen Dateifreigabe geeignet ist.

## <a name="share-snapshots"></a>Freigabemomentaufnahmen
### <a name="share-snapshots---general"></a>Freigabemomentaufnahmen – allgemein
* **Was ist eine Dateifreigabemomentaufnahme?**
    Mit Momentaufnahmen von Azure-Dateifreigaben können Sie schreibgeschützte Versionen Ihrer Dateifreigaben erstellen. Zudem können Sie eine ältere Version Ihres Inhalts für weitere Änderungen wieder in dieselbe Freigabe oder einen alternativen Speicherort in Azure oder lokal kopieren. Weitere Informationen zu Freigabemomentaufnahmen finden Sie unter [Freigabemomentaufnahme – Übersicht](storage-snapshots-files.md).

* **Wo werden meine Freigabemomentaufnahmen gespeichert?**
    Freigabemomentaufnahmen werden im gleichen Speicherkonto gespeichert wie die Dateifreigabe.

* **Sind Auswirkungen auf die Leistung zu erwarten?**
    Freigabemomentaufnahmen verursachen kein Leistungsoverhead.

* **Sind Freigabemomentaufnahmen anwendungskonsistent?**
    Nein. Nein, Freigabemomentaufnahmen sind nicht anwendungskonsistent. Benutzer müssen vor dem Erstellen von Freigabemomentaufnahmen die Schreibvorgänge von der Anwendung in die Freigabe leeren.

* **Gibt es Beschränkungen hinsichtlich der Anzahl von Freigabemomentaufnahmen?**
    Azure Files kann maximal 200 Freigabemomentaufnahmen beibehalten. Freigabemomentaufnahmen werden nicht zum Kontingent für die Dateifreigabe gezählt, so dass es keine Begrenzung pro Freigabe für den von allen Freigabemomentaufnahmen genutzten Gesamtspeicherplatz gibt. Die Beschränkungen für Speicherkonten gelten weiterhin. Nach 200 Freigabemomentaufnahmen müssen ältere Freigabemomentaufnahmen gelöscht werden, damit neue Freigabemomentaufnahmen erstellt werden können.

### <a name="create-share-snapshots"></a>Erstellen von Freigabemomentaufnahmen
* **Kann ich Freigabemomentaufnahmen von den einzelnen Dateien erstellen?**
    Freigabemomentaufnahmen werden auf Dateifreigabeebene erstellt. Sie können einzelne Dateien über die Dateifreigabemomentaufnahme wiederherstellen, jedoch keine Freigabemomentaufnahmen auf Dateiebene erstellen. Wenn Sie jedoch eine Freigabemomentaufnahme auf Freigabeebene erstellt haben und Freigabemomentaufnahmen, bei denen sich eine bestimmte Datei geändert hat, auflisten möchten, können Sie hierfür die Funktion „Vorherige Versionen“ in einer bereitgestellten Windows-Freigabe nutzen.

* **Kann ich Freigabemomentaufnahmen von verschlüsselten Dateifreigaben erstellen?**
    Sie können eine Freigabemomentaufnahme von Azure-Dateifreigaben erstellen, bei der die Verschlüsselung ruhender Daten aktiviert ist. Sie können Dateien von einer Freigabemomentaufnahme in einer verschlüsselten Dateifreigabe wiederherstellen. Wenn Ihre Freigabe verschlüsselt ist, wird Ihre Freigabemomentaufnahme ebenfalls verschlüsselt.

* **Sind meine Freigabemomentaufnahmen georedundant?**
    Freigabemomentaufnahmen weisen dieselbe Redundanz auf wie die Azure-Dateifreigabe, für die sie vorgesehen sind. Wenn Sie für Ihr Konto einen georedundanten Speicher (GRS) ausgewählt haben, wird Ihre Freigabemomentaufnahme ebenfalls redundant in der gekoppelten Region gespeichert.

### <a name="manage-share-snapshots"></a>Verwalten von Freigabemomentaufnahmen
* **Kann ich meine Freigabemomentaufnahmen über Linux durchsuchen?**
    Sie können mit der Azure CLI 2.0 unter Linux Freigabemomentaufnahmen erstellen, auflisten, durchsuchen und wiederherstellen.

* **Kann ich die Freigabemomentaufnahmen in ein anderes Speicherkonto kopieren?**
    Sie können Dateien von Freigabemomentaufnahmen an einen anderen Speicherort kopieren, nicht aber die Freigabemomentaufnahmen selbst.

### <a name="restore-data-from-share-snapshots"></a>Wiederherstellen von Daten aus Freigabemomentaufnahmen
* **Kann ich eine Freigabemomentaufnahme auf die Basisfreigabe höher stufen?**
    Sie können Daten von einer Freigabemomentaufnahme nur in ein beliebiges Ziel kopieren. Eine Freigabemomentaufnahme kann jedoch nicht auf die Basisfreigabe höher gestuft werden.

* **Kann ich Daten aus meiner Freigabemomentaufnahme in einem anderen Speicherkonto wiederherstellen?**
    Ja. Dateien aus einer Freigabemomentaufnahme können in den ursprünglichen oder in einen alternativen Speicherort kopiert werden, der dasselbe oder andere Speicherkonten in derselben oder in anderen Regionen enthält. Zudem können Dateien auch lokal oder in andere Clouds kopiert werden.    
  
### <a name="cleanup-share-snapshot"></a>Bereinigen einer Freigabemomentaufnahme
* **Kann ich meine Freigabe löschen, jedoch nicht Freigabemomentaufnahmen?**
    Wenn Sie über aktive Freigabemomentaufnahmen in Ihrer Freigabe verfügen, können Sie keine Freigaben löschen. Über eine API können Freigabemomentaufnahmen zusammen mit der Freigabe gelöscht werden, was auch über das Azure-Portal möglich ist.

* **Was geschieht mit meinen Freigabemomentaufnahmen, wenn ich mein Speicherkonto lösche?**
    Wenn Sie Ihr Speicherkonto löschen, werden die Freigabemomentaufnahmen ebenfalls gelöscht.

## <a name="billing-and-pricing"></a>Abrechnung und Preise
* **Zählt der Netzwerkdatenverkehr zwischen einer Azure-VM und einer Dateifreigabe als externe Bandbreite, die für das Abonnement berechnet wird?**  
    Wenn sich die Dateifreigabe und der virtuelle Computer in der gleichen Azure-Region befinden, ist der Datenverkehr zwischen den Komponenten kostenlos. Befinden sie sich dagegen in unterschiedlichen Regionen, wird der Datenverkehr zwischen den Komponenten als externe Bandbreite berechnet.

* **Wie viel kosten Freigabemomentaufnahmen?**
     In der Vorschauversion wird die Freigabemomentaufnahmekapazität nicht in Rechnung gestellt. Für ausgehenden Speicher und Transaktionen fallen nach wie vor die üblichen Kosten an. Gemäß der allgemeinen Verfügbarkeit werden sowohl die Kapazität als auch Transaktionen in der Freigabemomentaufnahme berechnet.
     
     Freigabemomentaufnahmen sind in der Regel inkrementell. Bei der Basisfreigabemomentaufnahme handelt es sich um die Freigabe selbst. Alle nachfolgenden Freigabemomentaufnahmen sind inkrementell und speichern lediglich die Differenz zur vorherigen Freigabemomentaufnahme. Ihnen wird nur der geänderte Inhalt in Rechnung gestellt. Wenn Sie über eine Freigabe mit 100 GB an Daten verfügen, jedoch nur 5 GB nach Ihrer letzten Freigabemomentaufnahme geändert wurden, verbraucht die Freigabemomentaufnahme nur zusätzlich 5 GB, und Ihnen werden lediglich 105 GB berechnet. Auf der Seite [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/) finden Sie weitere Informationen zur Transaktion und den Standardgebühren für ausgehenden Speicher.

## <a name="scale-and-performance"></a>Skalierung und Leistung
* **Welche Skalierungsgrenzwerte gelten für Azure Files?**  
    Informationen zu Skalierbarkeits- und Leistungszielen von Azure Files finden Sie unter [Azure Storage Scalability and Performance Targets (Skalierbarkeits- und Leistungsziele von Azure Storage)](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **Wie viele Clients können gleichzeitig auf dieselbe Datei zugreifen?**  
    Es gibt ein Kontingent von 2000 offenen Handles für eine einzelne Datei. Sobald Sie über 2000 offene Handles verfügen, wird eine Fehlermeldung ausgegeben, dass das Kontingent erreicht ist.

* **Dateien wurden in Azure Files nur sehr langsam entzippt. Wie soll ich vorgehen?**  
    Für die Übertragung zahlreicher Dateien an Azure Files empfehlen wir die Verwendung von AzCopy (Windows, Vorschauversion für Linux/Unix) oder Azure PowerShell, da diese Tools für die Netzwerkübertragung optimiert sind.

* **Nachdem ich meine Azure-Dateifreigabe unter Windows Server 2012 R2 oder Windows 8.1 bereitgestellt habe, hat sich mein Computer verlangsamt. Warum ist dies so?**  
    Das Problem beim Bereitstellen einer Azure-Dateifreigabe unter Windows Server 2012 R2 und Windows 8.1 war bekannt und wurde im kumulativen Update für Windows 8.1 und Windows Server 2012 R2 von April 2014 gepatcht. Stellen Sie sicher, dass dieses Patch auf alle Instanzen von Windows Server 2012 R2 und Windows 8.1 angewendet wurde, um von einer optimalen Leistung zu profitieren (selbstverständlich empfehlen wir, alle Windows-Patches über Windows Update zu installieren). Weitere Informationen finden Sie im zugehörigen KB-Artikel [Slow performance when you access Azure Files from Windows 8.1 or Server 2012 R2 (Niedrige Leistung beim Zugriff auf Azure Files von Windows 8.1 oder Server 2012 R2)](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Features und Interoperabilität mit anderen Diensten
* **Ist ein „Dateifreigabenzeuge“ für einen Failovercluster einer der Anwendungsfälle für Azure Files?**  
    Dies wird derzeit nicht für eine Azure-Dateifreigabe unterstützt. Weitere Informationen zum Einrichten dieser Funktion für den Azure Blob Storage finden Sie unter [Bereitstellen eines Cloudzeugen für einen Failovercluster](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).

* **Steht in der REST-API ein Umbenennungsvorgang zur Verfügung?**  
    Derzeit leider nicht.

* **Können Freigaben geschachtelt werden? Kann eine Freigabe also einer anderen Freigabe untergeordnet sein?**  
    Nein. Die Dateifreigabe ist der virtuelle Treiber, den Sie bereitstellen können. Geschachtelte Freigaben werden nicht unterstützt.

* **Verwenden von Azure Files mit IBM MQ**  
    IBM hat ein Dokument mit Anleitungen für IBM MQ-Kunden veröffentlicht, die Azure Files mit ihrem Dienst konfigurieren möchten. Weitere Informationen finden Sie unter [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)(Gewusst wie: Einrichten des IBM MQ-Warteschlangen-Managers für mehrere Instanzen mit dem Microsoft Azure-Dateidienst).

## <a name="see-also"></a>Weitere Informationen
* [Behandeln von Azure Files-Problemen unter Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Behandeln von Azure Files-Problemen unter Linux](storage-troubleshoot-linux-file-connection-problems.md)
* [Problembehandlung bei Azure File Sync (Vorschau)](storage-sync-files-troubleshoot.md)
