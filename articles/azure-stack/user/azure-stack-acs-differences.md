---
title: 'Azure Stack-Speicher: Unterschiede und Überlegungen | Microsoft-Dokumentation'
description: Lernen Sie die Unterschiede zwischen dem Azure Stack-Speicher und Azure-Speicher kennen, und erhalten Sie weitere Informationen zur Bereitstellung von Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/03/2018
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.openlocfilehash: 1393dd32aea8cb6d348092ea1fc56752f659beab
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/21/2018
ms.locfileid: "53717873"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack-Speicher: Unterschiede und Überlegungen

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Bei Azure Stack-Speicher handelt es sich um eine Reihe von Speicherclouddiensten in Microsoft Azure Stack. Der Azure Stack-Speicher bietet Blob-, Tabellen-, Warteschlangen- und Kontoverwaltungsfunktionen mit einer mit Azure konsistenten Semantik.

Dieser Artikel fasst die bekannten Unterschiede des Azure Stack-Speichers im Vergleich zu Azure Storage-Diensten zusammen. Außerdem werden dort die Aspekte erläutert, die beim Bereitstellen von Azure Stack berücksichtigt werden sollten. Informationen zu allgemeinen Unterschieden zwischen globalen Azure-Umgebungen und Azure Stack finden Sie im Artikel [Wichtige Aspekte](azure-stack-considerations.md).

## <a name="cheat-sheet-storage-differences"></a>Cheat Sheet: Speicherunterschiede

| Feature | Azure (global) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloudbasierte SMB-Dateifreigaben unterstützt|Noch nicht unterstützt
|Azure Storage Service Encryption für ruhende Daten|256-Bit-AES-Verschlüsselung. Unterstützung von Verschlüsselung mit vom Kunden verwalteten Schlüsseln in Key Vault|BitLocker-128-Bit-AES-Verschlüsselung. Verschlüsselung mit vom Kunden verwalteten Schlüsseln wird nicht unterstützt.
|Speicherkontotyp|Konten vom Typ „Allgemein v1 (GPv1)“ und „Allgemein v2 (GPv2)“ sowie Blobspeicherkonten|Nur Konten vom Typ „Allgemein v1 (GPv1)“
|Replikationsoptionen|Lokal redundanter Speicher, georedundanter Speicher, schreibgeschützter georedundanter Speicher und zonenredundanter Speicher|Lokal redundanter Speicher.
|Storage Premium|Vollständig unterstützt|Kann bereitgestellt werden, aber ohne Leistungsgrenzwerte oder Garantien.
|Verwaltete Datenträger|Unterstützung für Premium und Standard|Unterstützt bei Verwendung von Version 1808 oder höher.
|Blobname|1.024 Zeichen (2.048 Bytes)|880 Zeichen (1.760 Bytes)
|Maximale Blockblobgröße|4,75 TB (100 MB X 50.000 Blöcke)|4,75 TB (100 MB x 50.000 Blöcke) für das Update 1802 oder eine neuere Version. 50.000 x 4 MB (ca. 195 GB) für vorherige Versionen.
|Seitenblob-Momentaufnahmenkopie|Die Sicherung nicht verwalteter Azure-VM-Datenträger, die an einen ausgeführten virtuellen Computer angefügt sind, wird unterstützt.|Noch nicht unterstützt.
|Inkrementelle Momentaufnahmenkopie des Seitenblobs|Unterstützung für Premium- und Standard-Azure-Seitenblobs|Noch nicht unterstützt.
|Speicherebenen für Blobspeicher|Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“.|Noch nicht unterstützt.
|Vorläufiges Löschen für Blobspeicher|Allgemein verfügbar|Noch nicht unterstützt.
|Maximale Seitenblobgröße|8 TB|1 TB
|Seitenblob – Seitengröße|512 Bytes|4 KB
|Größe für Tabellenpartitionsschlüssel und Zeilenschlüssel|1.024 Zeichen (2.048 Bytes)|400 Zeichen (800 Bytes)
|Momentaufnahme eines Blobs|Die maximale Anzahl von Momentaufnahmen eines Blobs ist nicht beschränkt.|Die maximale Anzahl von Momentaufnahmen eines Blobs beträgt 1.000.
|Azure AD-Authentifizierung für Speicher|In der Vorschau|Noch nicht unterstützt.
|Unveränderliche Blobs|Allgemein verfügbar|Noch nicht unterstützt.
|Firewallregeln und Regeln für virtuelle Netzwerke für Speicher|Allgemein verfügbar|Noch nicht unterstützt.|

Es gibt auch Unterschiede zu Speichermetriken:

* Die Transaktionsdaten in Speichermetriken unterscheiden nicht zwischen interner oder externer Netzwerkbandbreite.
* Die Transaktionsdaten in Speichermetriken beinhalten keinen Zugriff auf die bereitgestellten Datenträger über virtuelle Computer.

## <a name="api-version"></a>API-Version

Die folgenden Versionen werden für Azure Stack-Speicher unterstützt:

Azure Storage-Dienst-APIs:

Update 1811 oder neuere Versionen:

 - [2017-11-09](https://docs.microsoft.com/rest/api/storageservices/version-2017-11-09)
 - [2017-07-29](https://docs.microsoft.com/rest/api/storageservices/version-2017-07-29)
 - [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
 - [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Update 1802 bis Update 1809:

- [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Vorherige Versionen:

- [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Verwaltungs-APIs für Azure Storage-Dienste:

- [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
- [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

Vorherige Versionen:

 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 
## <a name="sdk-versions"></a>SDK-Versionen

Azure Stack-Speicher unterstützt die folgenden Clientbibliotheken:

| Clientbibliothek | Von Azure Stack unterstützte Version | Link                                                                                                                                                                                                                                                                                                                                     | Endpunktspezifikation       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | Von 6.2.0 bis 8.7.0          | NuGet-Paket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | app.config-Datei              |
| Java           | Von 4.1.0 bis 6.1.0           | Maven-Paket:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Verbindungszeichenfolgen-Setup      |
| Node.js        | Von 1.1.0 bis 2.7.0           | NPM-Link:<br>https://www.npmjs.com/package/azure-storage<br>(Beispiel: Führen Sie „npm install azure-storage@2.7.0“ aus.)<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Dienstinstanzdeklaration |
| C++            | Von 2.4.0 bis 3.1.0           | NuGet-Paket:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Verbindungszeichenfolgen-Setup      |
| PHP            | Von 0.15.0 bis 1.0.0          | GitHub-Release:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Installation über Composer (Details siehe unten)                                                                                                                                                                                                                  | Verbindungszeichenfolgen-Setup      |
| Python         | Von 0.30.0 bis 1.0.0          | GitHub-Release:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Dienstinstanzdeklaration |
| Ruby           | Von 0.12.1 bis 1.0.1          | RubyGems-Paket:<br>Allgemein:<br>https://rubygems.org/gems/azure-storage-common/<br>Blob: https://rubygems.org/gems/azure-storage-blob/<br>Queue: https://rubygems.org/gems/azure-storage-queue/<br>Table: https://rubygems.org/gems/azure-storage-table/<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-ruby/releases | Verbindungszeichenfolgen-Setup      |

## <a name="next-steps"></a>Nächste Schritte

* [Get started with Azure Stack Storage development tools](azure-stack-storage-dev.md) (Erste Schritte mit den Azure Stack-Speicherentwicklungstools)
* [Introduction to Azure Stack Storage](azure-stack-storage-overview.md) (Einführung zu Azure Stack-Speicher)
