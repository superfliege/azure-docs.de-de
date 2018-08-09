---
title: Vergleich von Azure Data Lake Store mit Azure Storage Blob | Microsoft Docs
description: Vergleich von Azure Data Lake Store mit Azure Storage Blob
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 0b374e92a1e1d9828bc8c095e29e1dfdfd13275b
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492910"
---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergleich von Azure Data Lake Store und Azure Blob Storage
In der Tabelle in diesem Artikel werden die Unterschiede zwischen Azure Data Lake Store und Azure Blob Storage zusammengefasst und einige wichtige Aspekte der Big Data-Verarbeitung aufgezeigt. Azure Blob Storage ist ein universell einsetzbarer und skalierbarer Objektspeicher, der auf eine Vielzahl von Speicherszenarien ausgelegt ist. Azure Data Lake Store ist ein hochgradig skalierbares Repository, das für Big Data-Analyseworkloads optimiert ist.

|  | Azure Data Lake Store | Azure Blob Storage |
| --- | --- | --- |
| Zweck |Optimierter Speicher für Big Data-Analyseworkloads |Universell einsetzbarer Objektspeicher für eine Vielzahl von Speicherszenarien, wie z.B. Big Data-Analysen |
| Anwendungsfälle |Batch-, interaktive Streaming Analytics- und Machine Learning-Daten wie beispielsweise Protokolldateien, IoT-Daten, Clickstreams, große Datasets |Jede Art von Text- oder Binärdaten, beispielsweise Daten des Anwendungs-Back-Ends, Sicherungsdaten, Medienspeicher für Streaming und universelle Daten Zusätzlich vollständige Unterstützung für Analyseworkloads; Batch-, interaktive Streaming Analytics- und Machine Learning-Daten wie beispielsweise Protokolldateien, IoT-Daten, Clickstreams, große Datasets |
| Wichtige Begriffe |Das Data Lake Store-Konto enthält Ordner, die wiederum als Dateien gespeicherte Daten enthalten. |Das Speicherkonto enthält Container, die wiederum Daten in Form von Blobs enthalten. |
| Strukturdefinition |Hierarchisches Dateisystem |Objektspeicher mit flachem Namespace |
| API |REST-API über HTTPS |REST-API über HTTP/HTTPS |
| Serverseitige API |[WebHDFS-kompatible REST-API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Storage-REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop-Dateisystemclient |JA |JA |
| Datenvorgänge – Authentifizierung |Basierend auf [Azure Active Directory-Identitäten](../active-directory/develop/authentication-scenarios.md) |Basierend auf gemeinsamen Geheimnissen – [Kontozugriffsschlüssel](../storage/common/storage-create-storage-account.md#manage-your-storage-account) und [Shared Access Signature-Schlüssel](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Datenvorgänge – Authentifizierungsprotokoll |OAuth 2.0. Aufrufe müssen ein gültiges, über Azure Active Directory ausgestelltes JWT (JSON Web Token) enthalten. |Hashbasierter Nachrichtenauthentifizierungscode (Hashed Message Authentication Code, HMAC). Aufrufe müssen einen Base64-codierten SHA-256-Hash über einen Teil der HTTP-Anforderung enthalten. |
| Datenvorgänge – Autorisierung |POSIX-Zugriffssteuerungslisten (ACLs).  Auf Azure Active Directory-Identitäten basierende ACLs können auf Datei- und Ordnerebene festgelegt werden. |Für die Autorisierung auf Kontoebene – Verwenden Sie [Zugriffsschlüssel](../storage/common/storage-create-storage-account.md#manage-your-storage-account)<br>Für Konto-, Container- oder Blobautorisierung – Verwenden Sie [Shared Access Signature-Schlüssel](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Datenvorgänge – Überwachung |Verfügbar. Weitere Informationen finden Sie [hier](data-lake-store-diagnostic-logs.md) . |Verfügbar |
| Verschlüsselung für ruhende Daten |<ul><li>Transparent, serverseitig</li> <ul><li>Mit dienstverwalteten Schlüsseln</li><li>Mit kundenverwalteten Schlüsseln in Azure Key Vault</li></ul></ul> |<ul><li>Transparent, serverseitig</li> <ul><li>Mit dienstverwalteten Schlüsseln</li><li>Mit kundenverwalteten Schlüsseln in Azure Key Vault (Vorschau)</li></ul><li>Clientseitige Verschlüsselung</li></ul> |
| Verwaltungsvorgänge (z.B. Kontoerstellung) |[Rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (Role-Based Access Control, RBAC) von Azure zur Kontoverwaltung |[Rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (Role-Based Access Control, RBAC) von Azure zur Kontoverwaltung |
| Entwickler-SDKs |.NET, Java, Python, Node.js |.NET, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS |
| Leistung von Analyseworkloads |Optimierte Leistung für parallele Analyseworkloads. Hoher Durchsatz, hohe IOPS. |Optimierte Leistung für parallele Analyseworkloads. |
| Größenbeschränkungen |Keine Beschränkungen für Kontogrößen, Dateigrößen oder die Anzahl von Dateien. |Die geltenden Einschränkungen finden Sie [hier](../storage/common/storage-scalability-targets.md). Für größere Kontolimits wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/). |
| Georedundanz |Lokal redundant (mehrere Kopien von Daten in einer Azure-Region) |Lokal redundant (LRS), zonenredundant (ZRS) global redundant (GRS), global redundant mit Lesezugriff (RA-GRS). Weitere Informationen finden Sie [hier](../storage/common/storage-redundancy.md) . |
| Dienstzustand |Allgemein verfügbar |Allgemein verfügbar |
| Regionale Verfügbarkeit |Siehe [hier](https://azure.microsoft.com/regions/#services) |In allen Azure-Regionen verfügbar |
| Preis |Siehe [Preise](https://azure.microsoft.com/pricing/details/data-lake-store/) |Siehe [Preise](https://azure.microsoft.com/pricing/details/storage/) |


