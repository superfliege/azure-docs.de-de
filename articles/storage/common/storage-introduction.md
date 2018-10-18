---
title: Einführung in Azure Storage – Cloudspeicher in Azure | Microsoft-Dokumentation
description: Azure Storage ist die Cloudspeicherlösung von Microsoft. Azure Storage stellt Speicher für Datenobjekte bereit, die hoch verfügbar, sicher, langlebig, stark skalierbar und redundant sind.
services: storage
author: tamram
ms.service: storage
ms.topic: get-started-article
ms.date: 07/11/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: f03872a125e4e4d2f1e0b458a31d67a07802f2c8
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387446"
---
# <a name="introduction-to-azure-storage"></a>Einführung in Azure Storage

Azure Storage ist die Cloudspeicherlösung von Microsoft für moderne Datenspeicherszenarien. Für Azure Storage werden ein stark skalierbarer Objektspeicher für Datenobjekte, ein Dateisystemdienst für die Cloud, ein Messagingspeicher für zuverlässiges Messaging und ein NoSQL-Speicher bereitgestellt. Für Azure Storage gilt Folgendes:

- **Robust und hoch verfügbar:** Mit Redundanz wird sichergestellt, dass Ihre Daten sicher sind, falls es zu vorübergehenden Hardwareausfällen kommt. Sie können sich auch für das Replizieren von Daten über Rechenzentren oder geografische Regionen hinweg entscheiden, um eine weitere Schutzebene vor lokalen Notfällen oder Naturkatastrophen zu schaffen. Daten, die auf diese Weise repliziert werden, sind bei einem unerwarteten Ausfall weiterhin hoch verfügbar. 
- **Sicher:** Alle Daten, die in Azure Storage geschrieben werden, werden vom Dienst verschlüsselt. Bei Azure Storage können Sie genau steuern, wer Zugriff auf Ihre Daten hat.
- **Skalierbar:** Azure Storage ist auf hohe Skalierbarkeit ausgelegt, um die Datenspeicherungs- und Leistungsanforderungen heutiger Anwendungen zu erfüllen. 
- **Verwaltet:** Microsoft Azure übernimmt für Sie die Wartung und Behandlung aller kritischen Probleme.
- **Zugänglich:** Auf Daten in Azure Storage kann weltweit per HTTP oder HTTPS zugegriffen werden. Microsoft stellt SDKs für Azure Storage in verschiedenen Sprachen – .NET, Java, Node.js, Python, PHP, Ruby, Go und anderen – sowie eine ausgereifte REST-API bereit. Azure Storage unterstützt die Skripterstellung in Azure PowerShell oder der Azure CLI. Darüber hinaus werden im Azure-Portal und über Azure Storage-Explorer einfache visuelle Lösungen für die Arbeit mit Ihren Daten bereitgestellt.  

## <a name="azure-storage-services"></a>Azure Storage-Dienste

Azure Storage umfasst die folgenden Datendienste: 

- [Azure-Blobs](../blobs/storage-blobs-introduction.md): Ein überaus skalierbarer Objektspeicher für Text- und Binärdaten.
- [Azure Files](../files/storage-files-introduction.md): Verwaltete Dateifreigaben für Bereitstellungen lokal oder in der Cloud.
- [Azure-Warteschlangen](../queues/storage-queues-introduction.md): Ein Messagingspeicher für zuverlässiges Messaging zwischen Anwendungskomponenten. 
- [Azure-Tabellen](../tables/table-storage-overview.md): Ein NoSQL-Speicher für die schemalose Speicherung von strukturierten Daten.

Auf jeden Dienst wird über ein Speicherkonto zugegriffen. Informationen zu den ersten Schritten finden Sie unter [Speicherkonto erstellen](storage-quickstart-create-account.md).

## <a name="blob-storage"></a>Blob Storage

Azure Blob Storage ist die Objektspeicherlösung von Microsoft für die Cloud. Blobspeicher ist für die Speicherung großer Mengen von unstrukturierten Daten, z.B. Text oder Binärdaten, optimiert. 

Blobspeicher ist für folgende Zwecke ideal geeignet:

* Speichern von Bildern oder Dokumenten direkt für einen Browser
* Speichern von Dateien für verteilten Zugriff
* Video- und Audio-Streaming
* Speichern von Daten für Sicherung und Wiederherstellung, Notfallwiederherstellung und Archivierung
* Speichern von Daten für Analysen durch einen lokalen oder von Azure gehosteten Dienst

Über HTTP oder HTTPS kann von allen Orten weltweit auf Objekte zugegriffen werden, die sich in Blobspeicher befinden. Benutzer oder Clientanwendungen können über URLs, die [Azure Storage-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), die [Azure CLI](https://docs.microsoft.com/cli/azure/storage) oder eine Azure Storage-Clientbibliothek auf Blobs zugreifen. Die Speicherclientbibliotheken sind für mehrere Sprachen verfügbar, z.B. [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](http://azure.github.io/azure-storage-php/) und [Ruby](http://azure.github.io/azure-storage-ruby).

Weitere Informationen zu Blobspeicher finden Sie unter [Einführung in Blob Storage](../blobs/storage-blobs-introduction.md).

## <a name="azure-files"></a>Azure Files
[Azure Files](../files/storage-files-introduction.md) ermöglicht die Einrichtung hochverfügbarer Netzwerkdateifreigaben, auf die über das standardmäßige SMB-Protokoll (Server Message Block) zugegriffen werden kann. Dadurch können mehrere virtuelle Computer gemeinsam die gleichen Dateien mit Lese- und Schreibzugriff nutzen. Die Dateien können auch mithilfe der REST-Schnittstelle oder mithilfe der Speicherclientbibliotheken gelesen werden.

Azure Files unterscheidet sich in einem Punkt von Dateien auf einer Dateifreigabe eines Unternehmens: Über eine URL, die auf die gewünschte Datei verweist und ein SAS-Token (Shared Access Signature) enthält, kann von jedem Ort der Welt auf die Datei zugegriffen werden. Sie können SAS-Token generieren, um für einen bestimmten Zeitraum spezifischen Zugriff auf eine private Ressource zu ermöglichen.

Dateifreigaben können in zahlreichen Szenarien verwendet werden:

* Viele lokale Anwendungen verwenden Dateifreigaben. Dieses Feature erleichtert die Migration dieser Anwendungen mit gemeinsamen Daten zu Azure. Wenn Sie die Dateifreigabe unter dem gleichen Laufwerksbuchstaben einbinden, den auch die lokale Anwendung verwendet, müsste der Teil Ihrer Anwendung, der auf die Dateifreigabe zugreift, mit minimalen (oder sogar ganz ohne) Änderungen funktionieren.

* Konfigurationsdateien können auf einer Dateifreigabe gespeichert und von mehreren virtuellen Computern genutzt werden. Tools und Hilfsprogramme, die von mehreren Entwicklern in einer Gruppe verwendet werden, können auf einer Dateifreigabe gespeichert werden, um sicherzustellen, dass sie von allen gefunden werden und dass alle die gleiche Version verwenden.

* Diagnoseprotokolle, Metriken und Absturzabbilder sind nur drei Beispiele für Daten, die zur späteren Verarbeitung oder Analyse auf eine Dateifreigabe geschrieben werden können.

Active Directory-basierte Authentifizierung und Zugriffssteuerungslisten (Access Control Lists, ACLs) werden derzeit noch nicht unterstützt, wir arbeiten aber daran. Zur Authentifizierung für den Zugriff auf die Dateifreigabe werden die Speicherkonto-Anmeldeinformationen verwendet. Das bedeutet, dass jeder Benutzer, bei dem die Freigabe eingebunden ist, uneingeschränkten Lese-/Schreibzugriff auf die Freigabe hat.

Weitere Informationen zu Azure Files finden Sie unter [Einführung in Azure Files](../files/storage-files-introduction.md).

## <a name="queue-storage"></a>Queue Storage

Der Azure Queue-Dienst wird zum Speichern und Abrufen von Nachrichten verwendet. Warteschlangennachrichten können eine Größe von bis zu 64 KB haben, und eine Warteschlange kann Millionen von Nachrichten enthalten. Warteschlangen dienen im Allgemeinen zum Speichern von Listen mit Nachrichten, die asynchron verarbeitet werden sollen.

Beispiel: Ihre Kunden sollen Bilder hochladen können, und Sie möchten Miniaturbilder für jedes Bild erstellen. Sie können Kunden warten lassen, bis die Miniaturbilder beim Hochladen der Bilder erstellt werden. Alternativ können Sie eine Warteschlange verwenden. Wenn der Kunde den Upload abgeschlossen hat, schreiben Sie eine Nachricht an die Warteschlange. Legen Sie dann eine Azure-Funktion fest, die die Nachricht aus der Warteschlange abruft und die Miniaturbilder erstellt. Alle Schritte dieses Verarbeitungsvorgangs können separat skaliert werden, sodass Sie ihn für Ihre Zwecke optimieren können.

Weitere Informationen zu Azure-Warteschlangen finden Sie unter [Einführung in Warteschlangen](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Table Storage

Azure Table Storage ist jetzt Teil von Azure Cosmos DB. Diese Azure Table Storage-Dokumentation finden Sie unter [Übersicht über Azure Table Storage](../tables/table-storage-overview.md). Zusätzlich zum vorhandenen Azure Table Storage-Dienst ist das neue Angebot „Azure Cosmos DB-Tabellen-API“ mit durchsatzoptimierten Tabellen, globaler Verteilung und automatischen sekundären Indizes verfügbar. Unter [Einführung in die Tabellen-API von Azure Cosmos DB](https://aka.ms/premiumtables) erfahren Sie mehr und können das neue Premiumangebot testen.

Weitere Informationen zu Table Storage finden Sie unter [Übersicht über Azure Table Storage](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Datenträgerspeicher

Azure Storage umfasst auch die Funktionen für verwaltete und nicht verwaltete Datenträger, die von virtuellen Computern verwendet werden. Weitere Informationen zu diesen Features finden Sie in der [Microsoft Azure-Dokumentation](https://docs.microsoft.com/azure/#pivot=products&panel=Compute).

## <a name="types-of-storage-accounts"></a>Speicherkontentypen

Die folgende Tabelle enthält die verschiedenen Arten von Speicherkonten sowie die Objekte, die jeweils mit den Konten verwendet werden können.

|**Speicherkontotyp**|**Standard (allgemein)**|**Premium (allgemein)**|**Blobspeicher, Zugriffsebenen „Heiß“ und „Kalt“**|
|-----|-----|-----|-----|
|**Unterstützte Dienste**| Blob-, File-, Queue- und Tabellendienste | Blob-Dienst | Blob-Dienst|
|**Unterstützte Blobtypen**|Blockblobs, Seitenblobs und Anfügeblobs | Seitenblobs | Blockblobs und Anfügeblobs|

### <a name="general-purpose-storage-accounts"></a>Allgemeine Speicherkonten

Es gibt zwei Arten von allgemeine Speicherkonten.

#### <a name="standard-storage"></a>Standardspeicher

Die am häufigsten genutzten Speicherkonten sind Standardspeicherkonten, die für alle Datentypen verwendet werden können. Standardspeicherkonten verwenden Datenträger zum Speichern von Daten.

#### <a name="premium-storage"></a>Storage Premium

Storage Premium bietet Hochleistungsspeicher für Seitenblobs, die in erster Linie für VHD-Dateien verwendet werden. Storage Premium-Konten nutzen SSD für die Datenspeicherung. Microsoft empfiehlt die Verwendung von Storage Premium für alle virtuellen Computer.

### <a name="blob-storage-accounts"></a>Blob Storage-Konten

Beim Blob Storage-Konto handelt es sich um ein spezielles Speicherkonto für die Speicherung von Blockblobs und Anfügeblobs. Seitenblobs können in diesen Konten nicht gespeichert werden, daher ist das Speichern von VHD-Dateien nicht möglich. Mit diesen Konten können Sie die Zugriffsebene „Heiß“ oder „Kalt“ festlegen. Die Ebene kann jederzeit geändert werden.

Die Zugriffsebene „Heiß“ wird für Dateien verwendet, auf die häufig zugegriffen wird: Die Speicherkosten sind höher, die Kosten für den Zugriff auf die Blobs jedoch viel niedriger. Für Blobs, die in der Zugriffsebene „Kalt“ gespeichert sind, fallen höhere Kosten für den Zugriff auf die Blobs an. Die Kosten für die Speicherung sind jedoch viel niedriger.

## <a name="accessing-your-blobs-files-and-queues"></a>Zugreifen auf Blobs, Dateien und Warteschlangen

Jedes Speicherkonto verfügt über zwei Authentifizierungsschlüssel, die beide für alle Vorgänge verwendet werden können. Da es zwei Schlüssel gibt, können Sie gelegentlich ein Rollover für die Schlüssel durchführen, um die Sicherheit zu erhöhen. Es ist sehr wichtig, diese Schlüssel sicher aufzubewahren, da sie zusammen mit dem Kontonamen den unbegrenzten Zugriff auf alle Daten im Speicherkonto ermöglichen.

In diesem Abschnitt werden zwei Optionen zum Schützen der Speicherkonten und der dazugehörigen Daten erläutert. Ausführliche Informationen zum Schützen der Speicherkonten und Daten finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

### <a name="securing-access-to-storage-accounts-using-azure-ad"></a>Schützen des Zugriffs auf Speicherkonten mithilfe von Azure AD

Eine Möglichkeit zum Schützen der Speicherdaten besteht darin, den Zugriff auf die Speicherkontoschlüssel zu steuern. Mit der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) von Resource Manager können Sie Benutzern, Gruppen oder Anwendungen Rollen zuweisen. Diese Rollen sind an einen bestimmten Satz von Aktionen gebunden, die zulässig bzw. unzulässig sind. Wenn Sie den Zugriff auf ein Speicherkonto mithilfe von RBAC gewähren, betrifft dies nur die Verwaltungsvorgänge für dieses Speicherkonto (beispielsweise die Änderung der Zugriffsebene). Mit RBAC können Sie keinen Zugriff auf Datenobjekte wie einen bestimmten Container oder eine Dateifreigabe gewähren. Sie können jedoch RBAC verwenden, um Zugriff auf die Speicherkontoschlüssel zu erteilen, mit deren Hilfe anschließend die Datenobjekte gelesen werden können.

### <a name="securing-access-using-shared-access-signatures"></a>Schützen des Zugriffs mit Shared Access Signatures

Sie können Datenobjekte mithilfe von Shared Access Signatures und gespeicherten Zugriffsrichtlinien schützen. Eine Shared Access Signature (SAS) ist eine Zeichenfolge mit einem Sicherheitstoken, die an den URI für ein Objekt angefügt werden kann. Mit diesem URI können Sie den Zugriff auf bestimmte Speicherobjekte delegieren und Einschränkungen festlegen, beispielsweise Berechtigungen und den Datums- und Uhrzeitbereich für den Zugriff. Dieses Feature bietet umfangreiche Funktionen. Ausführliche Informationen finden Sie unter [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Verwenden von Shared Access Signatures (SAS)).

### <a name="public-access-to-blobs"></a>Öffentlicher Zugriff auf Blobs

Mit dem Blob-Dienst können Sie öffentlichen Zugriff auf einen Container und seine Blobs oder auf ein bestimmtes Blob erteilen. Wenn Sie einen Container oder ein Blob als öffentliche Ressource konfigurieren, kann jeder Benutzer anonym und ohne Authentifizierung drauf zugreifen. Diese Vorgehensweise käme beispielsweise bei einer Website mit Bildern, Videos oder Dokumenten aus Blob Storage zum Einsatz. Weitere Informationen finden Sie unter [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md) (Verwalten des anonymen Lesezugriffs auf Container und Blobs).

## <a name="encryption"></a>Verschlüsselung

Für die Storage-Dienste stehen zwei grundlegende Verschlüsselungsarten zur Verfügung. Ausführlichere Informationen zu Sicherheit und Verschlüsselung finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten

Azure Storage Service Encryption (SSE) für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Mit diesem Feature verschlüsselt Azure Storage Ihre Daten automatisch, bevor sie im Speicher abgelegt werden, und entschlüsselt sie vor dem Abrufen. Verschlüsselung, Entschlüsselung und Schlüsselverwaltung sind für Benutzer vollständig transparent.


SSE verschlüsselt die Daten automatisch in allen Leistungsebenen (Standard oder Premium), allen Bereitstellungsmodellen (Azure Resource Manager und Classic) sowie allen Azure Storage-Diensten (Blob, Queue, Table und File). SSE hat keine Auswirkungen auf die Leistung von Azure Storage.

Weitere Informationen zur SSE-Verschlüsselung für ruhende Daten finden Sie unter [Azure Storage Service Encryption für ruhende Daten](storage-service-encryption.md).

### <a name="client-side-encryption"></a>Clientseitige Verschlüsselung

Die Speicherclientbibliotheken enthalten Methoden, die Sie zum programmgesteuerten Verschlüsseln von Daten aufrufen können, bevor Sie sie vom Client an Azure senden. Sie werden verschlüsselt gespeichert, d.h. sie sind auch im Ruhezustand verschlüsselt. Beim Einlesen der Daten entschlüsseln Sie die Informationen nach ihrem Erhalt.

Weitere Informationen zur clientseitigen Verschlüsselung finden Sie unter [Clientseitige Verschlüsselung mit .NET für Microsoft Azure Storage](storage-client-side-encryption.md).

## <a name="replication"></a>Replikation

Um die Dauerhaftigkeit Ihrer Daten zu gewährleisten, repliziert Azure Storage mehrere Kopien Ihrer Daten. Beim Einrichten Ihres Speicherkontos wählen Sie einen Replikationstyp aus. In den meisten Fällen kann diese Einstellung nach der Erstellung des Speicherkontos angepasst werden. 

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Informationen zur Notfallwiederherstellung finden Sie unter [What to do if an Azure Storage outage occurs](storage-disaster-recovery-guidance.md) (Vorgehensweise beim Ausfall von Azure Storage).

## <a name="transferring-data-to-and-from-azure-storage"></a>Übertragen von Daten in und aus Azure Storage

Über das Befehlszeilenprogramm AzCopy können Sie Blob- und Dateidaten innerhalb Ihres Speicherkontos oder zwischen Speicherkonten kopieren. Weitere Anweisungen finden Sie in den folgenden Artikeln:

* [Transfer data with the AzCopy on Windows](storage-use-azcopy.md) (Übertragen von Daten mit AzCopy unter Windows)
* [Transfer data with AzCopy on Linux](storage-use-azcopy-linux.md) (Übertragen von Daten mit AzCopy unter Linux)

AzCopy baut auf der [Azure Storage Data Movement Library](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)auf, die derzeit als Vorschau verfügbar ist.

Mit dem Azure Import/Export-Dienst können große Mengen Blobdaten in das Speicherkonto importiert bzw. aus dem Speicherkonto exportiert werden. Sie bereiten mehrere Festplatten vor und senden Sie an ein Azure-Rechenzentrum. Dort werden die Daten von den bzw. auf die Festplatten übertragen und die Festplatten anschließend an sie zurückgesendet. Weitere Informationen zu diesem Dienst finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](../storage-import-export-service.md).

Wenn Sie schnell, kostengünstig und zuverlässig große Mengen an Blobdaten in Ihr Speicherkonto importieren möchten, können Sie auch Azure Data Box Disk verwenden. Microsoft sendet über einen regionalen Transportdienstleister bis zu fünf verschlüsselte SSDs (Solid State Drives) mit einer Kapazität von 40 TB an Ihr Datencenter. Sie können die Datenträger schnell konfigurieren, Daten über eine USB-Verbindung auf die Datenträger kopieren und sie anschließend wieder an Azure zurücksenden. Im Azure-Datencenter werden Ihre Daten dann automatisch in die Cloud hochgeladen. Weitere Informationen zu dieser Lösung finden Sie in der [Übersicht über Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview).

## <a name="pricing"></a>Preise

Ausführliche Informationen zu den Preisen für Azure Storage finden Sie auf der Seite mit den [Azure Storage-Preisen](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Storage-APIs, -Bibliotheken und -Tools
Auf Azure Storage-Ressourcen kann über jede Sprache zugegriffen werden, die für HTTP/HTTPS-Anforderungen geeignet ist. Zusätzlich bietet Azure Storage Programmierbibliotheken für einige beliebte Sprachen. Diese Bibliotheken vereinfachen viele Aspekte der Arbeit mit Azure Storage und behandeln bestimmte Details wie synchrone und asynchrone Aufrufe, die Zusammenfassung von Vorgängen, die Ausnahmeverwaltung, automatische Wiederholungen, das Betriebsverhalten und Ähnliches. Bibliotheken sind aktuell für die folgenden Sprachen und Plattformen erhältlich. Weitere sind in Planung.

### <a name="azure-storage-data-api-and-library-references"></a>Verweise auf die Azure Storage-Daten-API und Bibliotheken
* [REST-API für Speicherdienste](https://docs.microsoft.com/rest/api/storageservices/)
* [Speicherclientbibliothek für .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
* [Speicherclientbibliothek für Java/Android](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Speicherclientbibliothek für Node.js](https://docs.microsoft.com/javascript/api/azure-storage)
* [Speicherclientbibliothek für Python](https://github.com/Azure/azure-storage-python)
* [Speicherclientbibliothek für PHP](https://github.com/Azure/azure-storage-php)
* [Speicherclientbibliothek für Ruby](https://github.com/Azure/azure-storage-ruby)
* [Speicherclientbibliothek für C++](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>Verweise auf die Azure Storage-Verwaltungs-API und Bibliotheken
* [Speicherressourcenanbieter – REST-API](https://docs.microsoft.com/rest/api/storagerp/)
* [Speicherressourcenanbieter – Clientbibliothek für .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
* [Storage-Dienstverwaltung – REST-API (klassisch)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>Verweise auf die Azure Storage-Datenverschiebungs-API und Bibliotheken
* [Import/Export-Storage-Dienst – REST-API](https://docs.microsoft.com/rest/api/storageimportexport/)
* [Storage-Datenverschiebung – Clientbibliothek für .NET](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.datamovement)

### <a name="tools-and-utilities"></a>Tools und Hilfsprogramme
* [Azure PowerShell-Cmdlets für Storage](https://docs.microsoft.com/powershell/module/azure.storage)
* [Azure CLI-Cmdlets für Storage](https://docs.microsoft.com/cli/azure/storage)
* [Befehlszeilenprogramm AzCopy](http://aka.ms/downloadazcopy)
* Bei [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.
* [Azure Storage-Clienttools](../storage-explorers.md)
* [Azure-Entwicklertools](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Herstellung der Betriebsbereitschaft für Azure Storage finden Sie unter [Erstellen eines Speicherkontos](storage-quickstart-create-account.md).
