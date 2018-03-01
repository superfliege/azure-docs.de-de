---
title: "Azure Storage Service Encryption für ruhende Daten | Microsoft Docs"
description: "Verwenden Sie Azure Storage Service Encryption, um Azure Blob Storage auf der Dienstseite zu verschlüsseln, wenn die Daten gespeichert werden, und beim Abrufen der Daten zu entschlüsseln."
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: tamram
ms.openlocfilehash: d04752c92218d17b4e90bb03a2ed2ac5a0a11b93
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2018
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service Encryption für ruhende Daten
Azure Storage Service Encryption (SSE) für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Mit diesem Feature verschlüsselt Azure Storage Ihre Daten automatisch, bevor sie im Speicher abgelegt werden, und entschlüsselt sie vor dem Abrufen. Verschlüsselung, Entschlüsselung und Schlüsselverwaltung sind für Benutzer vollständig transparent.

Die folgenden Abschnitte enthalten eine ausführliche Anleitung zur Verwendung der Features von Storage Service Encryption sowie die unterstützten Szenarien und Benutzererfahrungen.

## <a name="overview"></a>Übersicht
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Daten können während der Übertragung zwischen einer Anwendung und Azure mit [clientseitiger Verschlüsselung](../storage-client-side-encryption.md), HTTPS oder SMB 3.0 geschützt werden. Storage Service Encryption (SSE) bietet eine Verschlüsselung ruhender Daten, die Verarbeitung der Ver- und Entschlüsselung sowie die Schlüsselverwaltung auf vollständig transparente Weise. Sämtliche Daten werden mittels 256-Bit- [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.

SSE verschlüsselt die Daten, wenn sie in Azure Storage geschrieben werden, und kann für Azure Blob, File, Table und Queue Storage verwendet werden. Das Feature funktioniert wie folgt:

* Standard Storage: Allgemeine Speicherkonten für Blobs, File, Table und Queue Storage sowie Blob Storage-Konten
* Storage Premium 
* Standardmäßig für klassische und ARM-Speicherkonten aktiviert 
* Alle Redundanzebenen (LRS, ZRS, GRS, RA-GRS)
* Azure Resource Manager-Speicherkonten (nicht jedoch klassische Konten) 
* Alle Regionen.

Weitere Informationen erhalten Sie in den FAQ.

Melden Sie sich zum Anzeigen der Einstellungen der Speicherdienstverschlüsselung für ein Speicherkonto beim [Azure-Portal](https://portal.azure.com) an, und wählen Sie ein Speicherkonto aus. Suchen Sie auf dem Blatt „Einstellungen“ nach dem Abschnitt mit dem Blobdienst, und klicken Sie auf „Verschlüsselung“.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image1.png)
<br/>*Abbildung 1: Aktivieren von SSE für Blob-Dienst (Schritt 1)*

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image3.png)
<br/>*Abbildung 2: Aktivieren von SSE für File Service (Schritt 1)*

Nach Klicken auf die Einstellung „Verschlüsselung“ können Sie Storage Service Encryption aktivieren oder deaktivieren.

![Portal-Screenshot mit Verschlüsselungseigenschaften](./media/storage-service-encryption/image2.png)
<br/>*Abbildung 3: Aktivieren von SSE für Blob und File Service (Schritt 2)*

## <a name="encryption-scenarios"></a>Verschlüsselungsszenarien
Storage Service Encryption wird auf einer Speicherkontoebene aktiviert. Sie ist standardmäßig für alle Dienste aktiviert. Folgende Kundenszenarien werden unterstützt:

* Verschlüsselung von Blob, File, Table und Queue Storage für klassische und Resource Manager-Konten.

Für SSE gelten die folgenden Einschränkungen:

* Vorhandene Daten – SSE verschlüsselt nur nach Aktivierung der Verschlüsselung neu erstellte Daten. Wenn Sie z.B. ein neues Resource Manager-Speicherkonto erstellen, jedoch keine Verschlüsselung aktivieren, und dann Blobs oder archivierte VHDs in das Speicherkonto hochladen und SSE aktivieren, werden diese Blobs nicht verschlüsselt, sofern sie nicht neu geschrieben oder kopiert werden.
* Unterstützung für Marketplace – Aktivieren der Verschlüsselung von virtuellen Computern, die über den Marketplace mit dem [Azure-Portal](https://portal.azure.com), mit PowerShell und mit der Azure-Befehlszeilenschnittstelle erstellt werden. Das VHD-Basisimage bleibt unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

## <a name="getting-started"></a>Erste Schritte
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Schritt 1: [Erstellen eines neuen Speicherkontos](../storage-create-storage-account.md)
### <a name="step-2-verify-encryption"></a>Schritt 2: Überprüfen der Verschlüsselung
Sie können die Verschlüsselung im [Azure-Portal](https://portal.azure.com) überprüfen.

> [!NOTE]
> Wenn Sie SSE programmgesteuert für ein Speicherkonto überprüfen möchten, können Sie die [REST-API des Azure Storage-Ressourcenanbieters](https://msdn.microsoft.com/library/azure/mt163683.aspx), die [Clientbibliothek des Speicherressourcenanbieters für .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](/powershell/azureps-cmdlets-docs) oder die [Azure-Befehlszeilenschnittstelle](../storage-azure-cli.md) verwenden.
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>Schritt 3: Kopieren von Daten in das Speicherkonto
Sobald SSE für ein Speicherkonto aktiviert ist, werden alle in dieses Speicherkonto geschriebenen Daten verschlüsselt. Alle Daten, die sich bereits in diesem Speicherkonto befinden, werden erst verschlüsselt, nachdem sie erneut geschrieben wurden. Sie können Daten in einen anderen Container kopieren, um zu überprüfen, ob die vorherigen Daten verschlüsselt werden. Sie können dazu eines der folgenden Tools verwenden. Das Verhalten bei File, Table und Queue Storage ist identisch.

#### <a name="using-azcopy"></a>Verwenden von AzCopy
AzCopy ist ein Windows Befehlszeilenprogramm, das zum Kopieren von Daten zu und von Microsoft Azure Blob, File und Table Storage entwickelt wurde, wobei durch einfache Befehle optimale Leistung erzielt wird. Mit diesem Tool können Sie Ihre Blobs oder Dateien aus einem Speicherkonto in ein anderes kopieren, für das SSE aktiviert ist. 

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md).

#### <a name="using-smb"></a>Verwendung von SMB
Azure Files bietet Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls. Sie können eine Dateifreigabe auf einem Client lokal oder in Azure bereitstellen. Nach der Bereitstellung können Tools wie Robocopy zum Kopieren von Dateien auf Azure-Dateifreigaben verwendet werden. Weitere Informationen finden Sie unter [Bereitstellen der Azure-Dateifreigabe unter Windows](../files/storage-how-to-use-files-windows.md) und [Bereitstellen der Azure-Dateifreigabe unter Linux](../files/storage-how-to-use-files-linux.md).


#### <a name="using-the-storage-client-libraries"></a>Verwenden von Speicherclientbibliotheken
Sie können mithilfe unseres umfangreichen Angebots an Speicherclientbibliotheken, einschließlich .NET, C++, Java, Android, Node.js, PHP, Python und Ruby, Blob- oder Dateidaten in und aus Blobspeicher oder zwischen Speicherkonten kopieren.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../blobs/storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Verwenden eines Storage-Explorers
Mit einem Storage-Explorer können Sie Speicherkonten erstellen, Daten hoch- und herunterladen, Inhalte von Blobs anzeigen und durch Verzeichnisse navigieren. Sie können eines dieser Konten zum Hochladen von Blobs in Ihr Speicherkonto mit aktivierter Verschlüsselung verwenden. Mit einigen Storage-Explorern können Sie auch Daten aus Ihrem vorhandenen Blobspeicher in einen anderen Container in Ihrem Speicherkonto oder ein neues Speicherkonto kopieren, für das SSE aktiviert ist.

Weitere Informationen finden Sie unter [Microsoft Azure Storage-Explorer](../storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>Schritt 4: Abfragen des Status der verschlüsselten Daten
Eine aktualisierte Version der Speicherclientbibliotheken wurde bereitgestellt, mit denen Sie den Status eines Objekts ermitteln können, um zu bestimmen, ob es verschlüsselt ist oder nicht. Dies ist derzeit nur für Blob Storage verfügbar. Support für File Storage ist allerdings geplant. 

Rufen Sie in der Zwischenzeit [Kontoeigenschaften abrufen](https://msdn.microsoft.com/library/azure/mt163553.aspx) auf, um zu überprüfen, ob die Verschlüsselung für das Speicherkonto aktiviert wurde, oder zeigen Sie die Eigenschaften des Speicherkontos im Azure-Portal an.

## <a name="encryption-and-decryption-workflow"></a>Verschlüsselungs- und Entschlüsselungsworkflow
Hier ist eine kurze Beschreibung des Verschlüsselungs-/Entschlüsselungsworkflows:

* Die Verschlüsselung wird für ein Speicherkonto aktiviert.
* Wenn der Kunde neue Daten (PUT Blob, PUT Block, PUT Page, PUT File usw.) in Blob Storage oder in File Storage schreibt, wird jeder Schreibvorgang mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.
* Wenn der Kunde auf Daten zugreifen muss (GET Blob usw.), werden die Daten vor der Rückgabe an den Benutzer automatisch entschlüsselt.
* Wenn die Verschlüsselung deaktiviert ist, werden neue Schreibvorgänge nicht mehr verschlüsselt, und vorhandene verschlüsselte Daten bleiben verschlüsselt, bis sie vom Benutzer neu geschrieben werden. Bei aktivierter Verschlüsselung werden Schreibvorgänge in Blob Storage oder in File Storage verschlüsselt. Der Zustand der Daten ändert sich nicht, wenn der Benutzer zwischen Aktivieren und Deaktivieren der Verschlüsselung für das Speicherkonto umschaltet.
* Alle Verschlüsselungsschlüssel werden von Microsoft gespeichert, verschlüsselt und verwaltet.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Häufig gestellte Fragen zu Storage Service Encryption für ruhende Daten
**F: Ich habe ein klassisches Speicherkonto. Kann ich dafür SSE aktivieren?**

A: SSE ist standardmäßig für alle Speicherkonten (klassische und Resource Manager-Speicherkonten) aktiviert.

**F: Wie kann ich Daten in meinem klassischen Speicherkonto verschlüsseln?**

A: Da die Verschlüsselung standardmäßig aktiviert ist, werden Ihre neuen Daten automatisch vom Speicherdienst verschlüsselt. 

Sie können auch ein neues Resource Manager-Speicherkonto erstellen und all Ihre Daten mit [AzCopy](storage-use-azcopy.md) aus Ihrem vorhandenen klassischen Speicherkonto in Ihr neu erstelltes Resource Manager-Speicherkonto kopieren. 

Sie können auch Ihr klassisches Speicherkonto zu einem Resource Manager-Speicherkonto migrieren. Dieser Vorgang erfolgt unmittelbar und ändert den Typ des Kontos, hat aber keine Auswirkungen auf Ihre vorhandenen Daten. Neu geschriebene Daten werden erst nach dem Aktivieren der Verschlüsselung verschlüsselt. Weitere Informationen finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Beachten Sie, dass dies nur für Blob- und File-Dienste unterstützt wird.

**F: Ich habe ein vorhandenes Resource Manager-Speicherkonto. Kann ich dafür SSE aktivieren?**

A: SSE ist standardmäßig auf allen vorhandenen Resource Manager-Speicherkonten aktiviert. Dies wird für Blobs, Tables, File und Queue Storage unterstützt. 

**F: Kann ich die aktuellen Daten in einem vorhandenen Resource Manager-Speicherkonto verschlüsseln?**

A: SSE ist standardmäßig für alle Speicherkonten (klassische und Resource Manager-Speicherkonten) aktiviert. Daten, die bereits vorhanden waren, werden jedoch nicht verschlüsselt. Um vorhandene Daten zu verschlüsseln, können Sie sie unter einem anderen Namen oder in einen anderen Container kopieren und anschließend die nicht verschlüsselten Versionen entfernen. 

**F: Ich verwende Storage Premium; kann ich SSE verwenden?**

A: Ja, SSE wird von Standard Storage und Premium Storage unterstützt.  Storage Premium wird für den Dateidienst nicht unterstützt.

**F: Wenn ich ein neues Speicherkonto erstelle, SSE aktiviere und dann eine neue VM erstelle, die dieses Speicherkonto verwendet, bedeutet dies, dass mein virtueller Computer verschlüsselt ist?**

A: Ja. Alle erstellten Datenträger, die das neue Speicherkonto verwenden, werden verschlüsselt, sofern sie nach der SSE-Aktivierung erstellt wurden. Wurde die VM mit Azure Market Place erstellt, bleibt das VHD-Basisimage unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

**F: Kann ich neue Speicherkonten mithilfe von Azure PowerShell und Azure-CLI erstellen, wenn SSE aktiviert ist?**

A: SSE wird beim Erstellen von Speicherkonten (klassische und Resource Manager-Speicherkonten) standardmäßig aktiviert. Sie können Kontoeigenschaften mit Azure PowerShell und der Azure-Befehlszeilenschnittstelle überprüfen.

**F: Wie hoch sind die Mehrkosten für Azure Storage, wenn SSE aktiviert ist?**

A: Es fallen keine zusätzlichen Kosten an.

**F: Wer verwaltet die Verschlüsselungsschlüssel?**

A: Die Schlüssel werden von Microsoft verwaltet.

**F: Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?**

A: Wir arbeiten darauf hin, dass Kunden ihre eigenen Verschlüsselungsschlüssel verwenden können.

**F: Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: Derzeit nicht; die Schlüssel werden vollständig von Microsoft verwaltet.

**F: Ist SSE standardmäßig aktiviert, wenn ich ein neues Speicherkonto erstelle?**

A: Ja, SSE mit von Microsoft verwalteten Schlüsseln ist standardmäßig für alle Speicherkonten (Azure Resource Manager und klassische Speicherkonten) aktiviert. Zudem ist es für alle Dienste (Blob, Table, Queue und File Storage) aktiviert.

**F: Wo liegt der Unterschied zu Azure Disk Encryption?**

A: Dieses Feature wird zum Verschlüsseln von Daten in Azure Blob Storage verwendet. Azure Disk Encryption wird zum Verschlüsseln von Betriebssystemdatenträgern und sonstigen Datenträgern in virtuellen IaaS-Computern verwendet. Weitere Informationen finden Sie in unserem [Azure Storage-Sicherheitsleitfaden](../storage-security-guide.md).

**F: Was geschieht, wenn SSE aktiviert ist und ich Azure Disk Encryption auf den Datenträgern aktiviere?**

A: Dies funktioniert nahtlos. Ihre Daten werden durch beide Methoden verschlüsselt.

**F: Mein Speicherkonto ist so eingerichtet, dass es georedundant repliziert wird. Werden meine redundanten Kopien ebenfalls verschlüsselt, wenn SSE aktiviert ist?**

A: Ja, alle Kopien des Speicherkontos werden verschlüsselt, und alle Redundanzoptionen – lokal redundanter Speicher (LRS), zonenredundanter Speicher (ZRS), georedundanter Speicher (GRS) und lesezugriff-georedundanter Speicher (RA-GRS, Read-Access Geo Redundant) werden unterstützt.

**F: Ich kann die Verschlüsselung für mein Speicherkonto nicht deaktivieren.**

A: Die Verschlüsselung ist standardmäßig aktiviert, und es gibt keine Möglichkeit, die Verschlüsselung für Ihr Speicherkonto zu deaktivieren. 

**F: Ist SSE nur in bestimmten Regionen möglich?**

A: SSE ist in allen Regionen für alle Dienste verfügbar. 

**F: Wohin wende ich mich, wenn Probleme auftreten oder ich Feedback geben möchte?**

A: Wenden Sie sich in allen Angelegenheiten, die Storage Service Encryption betreffen, an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](../storage-security-guide.md).
