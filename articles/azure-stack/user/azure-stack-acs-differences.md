---
title: 'Azure Stack-Speicher: Unterschiede und Überlegungen'
description: Lernen Sie die Unterschiede zwischen dem Azure Stack-Speicher und Azure Storage kennen, und erhalten Sie weitere Informationen zur Bereitstellung von Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.reviwer: xiaofmao
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/21/2017
ms.author: jeffgilb
ms.openlocfilehash: 165a899dbad0893b3a2bddcfc68c9b5d737e9d3d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2018
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Azure Stack-Speicher: Unterschiede und Überlegungen

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack-Speicher ist eine Reihe von Speicherclouddiensten in Microsoft Azure Stack. Der Azure Stack-Speicher bietet Blob-, Tabellen-, Warteschlangen- und Kontoverwaltungsfunktionen mit einer mit Azure konsistenten Semantik.

Dieser Artikel beschreibt die bekannten Unterschiede des Azure Stack-Speichers im Vergleich zu Azure Storage. Außerdem enthält er noch weitere Überlegungen, die Sie bei der Bereitstellung von Azure Stack beachten sollten. Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Thema [Key considerations](azure-stack-considerations.md) (Wichtige Aspekte).

## <a name="cheat-sheet-storage-differences"></a>Spickzettel: Speicherunterschiede

| Feature | Azure (global) | Azure Stack |
| --- | --- | --- |
|File Storage|Cloudbasierte SMB-Dateifreigaben unterstützt|Noch nicht unterstützt
|Azure Storage Service Encryption für ruhende Daten|256-Bit-AES-Verschlüsselung|BitLocker-128-Bit-AES-Verschlüsselung
|Speicherkontotyp|Allgemeine Speicherkonten und Azure Blob Storage-Konten|Nur allgemein.
|Replikationsoptionen|Lokal redundanter Speicher, georedundanter Speicher, schreibgeschützter georedundanter Speicher und zonenredunanter Speicher|Lokal redundanter Speicher.
|Storage Premium|Vollständig unterstützt|Kann bereitgestellt werden, aber ohne Leistungsgrenzwerte oder Garantien.
|Verwaltete Datenträger|Unterstützung für Premium und Standard|Noch nicht unterstützt.
|Blobname|1.024 Zeichen (2.048 Bytes)|880 Zeichen (1.760 Bytes)
|Maximale Blockblobgröße|4,75 TB (100 MB X 50.000 Blöcke)|4,75 TB (100 MB x 50.000 Blöcke) für das Update 1802 oder eine neuere Version. 50.000 x 4 MB (ca. 195 GB) für vorherige Versionen.
|Seitenblob-Momentaufnahmenkopie|Die Sicherung nicht verwalteter Azure-VM-Datenträger, die an einen ausgeführten virtuellen Computer angefügt sind, wird unterstützt.|Noch nicht unterstützt.
|Inkrementelle Momentaufnahmenkopie des Seitenblobs|Unterstützung für Premium- und Standard-Azure-Seitenblobs|Noch nicht unterstützt.
|Speicherebenen für Blobspeicher|Speicherebenen „Heiß“ (Hot), „Kalt“ (Cool) und „Archiv“.|Noch nicht unterstützt.
Vorläufiges Löschen für Blobspeicher|Vorschau|Noch nicht unterstützt.
|Maximale Seitenblobgröße|8 TB|1 TB
|Seitenblob – Seitengröße|512 Bytes|4 KB
|Größe für Tabellenpartitionsschlüssel und Zeilenschlüssel|1.024 Zeichen (2.048 Bytes)|400 Zeichen (800 Bytes)
|Momentaufnahme eines Blobs|Die maximale Anzahl von Momentaufnahmen eines Blobs ist nicht beschränkt.|Die maximale Anzahl von Momentaufnahmen eines Blobs beträgt 1.000.|

### <a name="metrics"></a>Metriken
Es gibt auch einige Unterschiede zu Speichermetriken:
* Die Transaktionsdaten in Speichermetriken unterscheiden nicht zwischen interner oder externer Netzwerkbandbreite.
* Die Transaktionsdaten in Speichermetriken beinhalten keinen Zugriff auf die bereitgestellten Datenträger über virtuelle Computer.

## <a name="api-version"></a>API-Version
Die folgenden Versionen werden für Azure Stack-Speicher unterstützt:

Azure Storage-Dienst-APIs:

Update 1802 oder neuer:
 - [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
 - [2015-07-08 ](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Vorherige Versionen:
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)


Verwaltungs-APIs für Azure Storage-Dienste:

 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

## <a name="sdk-versions"></a>SDK-Versionen

Die folgenden Clientbibliotheken werden für Azure Stack-Speicher unterstützt:

| Clientbibliothek | Von Azure Stack unterstützte Version | Link                                                                                                                                                                                                                                                                                                                                     | Endpunktspezifikation       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | Von 6.2.0 bis 8.7.0          | NuGet-Paket:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | app.config-Datei              |
| Java           | Von 4.1.0 bis 6.1.0           | Maven-Paket:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Verbindungszeichenfolgen-Setup      |
| Node.js        | Von 1.1.0 bis 2.7.0           | NPM-Link:<br>https://www.npmjs.com/package/azure-storage<br>(Beispiel: Führen Sie „npm install azure-storage@2.7.0“ aus.)<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Dienstinstanzdeklaration |
| C++            | Von 2.4.0 bis 3.1.0           | NuGet-Paket:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Verbindungszeichenfolgen-Setup      |
| PHP            | Von 0.15.0 bis 1.0.0          | GitHub-Release:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Installation über Composer (Details siehe unten)                                                                                                                                                                                                                  | Verbindungszeichenfolgen-Setup      |
| Python         | Von 0.30.0 bis 1.0.0          | GitHub-Release:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Dienstinstanzdeklaration |
| Ruby           | Von 0.12.1 bis 1.0.1          | RubyGems-Paket:<br>Allgemein:<br>https://rubygems.org/gems/azure-storage-common/<br>Blob: https://rubygems.org/gems/azure-storage-blob/<br>Warteschlange: https://rubygems.org/gems/azure-storage-queue/<br>Tabelle: https://rubygems.org/gems/azure-storage-table/<br> <br>GitHub-Release:<br>https://github.com/Azure/azure-storage-ruby/releases | Verbindungszeichenfolgen-Setup      |

## <a name="next-steps"></a>Nächste Schritte

* [Get started with Azure Stack Storage development tools](azure-stack-storage-dev.md) (Erste Schritte mit den Azure Stack-Speicherentwicklungstools)
* [Introduction to Azure Stack Storage](azure-stack-storage-overview.md) (Einführung zu Azure Stack-Speicher)

