---
title: Systemanforderungen für Microsoft Azure Data Box | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Software- und Netzwerkanforderungen für Ihre Azure Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 12/27/2018
ms.author: alkohli
ms.openlocfilehash: af7bcf2a83259b9d883a824b05312316f9f1f4f8
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794004"
---
# <a name="azure-data-box-system-requirements"></a>Systemanforderungen für Azure Data Box

In diesem Artikel werden wichtige Systemanforderungen für Ihre Microsoft Azure Data Box und die Clients beschrieben, die sich mit der Data Box verbinden. Sie sollten die Informationen sorgfältig lesen, bevor Sie Ihre Data Box bereitstellen. Später können Sie während der Bereitstellung und beim nachfolgenden Betrieb bei Bedarf als Referenz darauf zurückgreifen.

Systemanforderungen:

* **Softwareanforderungen für Hosts, die sich mit der Data Box verbinden**: Beschreibt die unterstützten Plattformen, Browser für die lokale Webbenutzeroberfläche, SMB-Clients und alle zusätzlichen Anforderungen an Hosts, die sich mit der Data Box verbinden können.
* **Netzwerkanforderungen für die Data Box**: Liefert Informationen zu den Netzwerkanforderungen für einen optimalen Betrieb der Data Box.


## <a name="software-requirements"></a>Softwareanforderungen

Die Softwareanforderungen umfassen Informationen zu den unterstützten Betriebssystemen, unterstützten Browsern für die lokale Webbenutzeroberfläche und SMB-Clients.

### <a name="supported-operating-systems-for-clients"></a>Unterstützte Betriebssysteme für Clients

Im Folgenden finden Sie eine Liste der unterstützten Betriebssysteme für Datenkopiervorgänge über die Clients, die mit dem Data Box-Gerät verbunden sind.

| **Betriebssystem** | **Versionen** | 
| --- | --- | 
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 | 
| Windows |7, 8, 10 | 
|Linux    |         |

### <a name="supported-file-systems-for-linux-clients"></a>Unterstützte Dateisysteme für Linux-Clients

| **Protokolle** | **Versionen** | 
| --- | --- | 
| SMB |2.x und höher |
| NFS | Alle Versionen bis einschließlich 4.1|

### <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

Im Folgenden finden Sie eine Liste der unterstützten Speichertypen für das Data Box-Gerät.

| **Speicherkonto** | **Hinweise** |
| --- | --- |
| Klassisch | Standard |
| Allgemeiner Zweck  |Standard: sowohl V1 als auch V2 werden unterstützt. |
| Blob |Es werden sowohl „heiße“ als auch „kalte“ Blobs unterstützt. |

>[!NOTE]
> Azure Data Lake Storage Gen 2-Konten werden nicht unterstützt.


### <a name="supported-storage-types"></a>Unterstützte Speichertypen

Im Folgenden finden Sie eine Liste der unterstützten Speichertypen für das Data Box-Gerät.

| **Dateiformat** | **Hinweise** |
| --- | --- |
| Azure-Blockblob | |
| Azure-Seitenblob  | Die Daten müssen einem ganzzahligen Vielfachen von 512 Bytes entsprechen.|
| Azure Files | |


### <a name="supported-web-browsers"></a>Unterstützte Webbrowser

Es folgt eine Liste der für die lokale Webbenutzeroberfläche unterstützten Webbrowser.

| **Browser** | **Versionen** | **Weitere Anforderungen/Hinweise** |
| --- | --- | --- |
| Google Chrome |Aktuelle Version |Getestet mit Chrome|
| Microsoft Edge |Aktuelle Version | |
| Firefox | Aktuelle Version | Getestet mit Firefox|
| Internet Explorer |Aktuelle Version |Wenn Sie sich nicht anmelden können, überprüfen Sie, ob Cookies und JavaScript aktiviert sind. Um den Benutzeroberflächenzugriff zu aktivieren, fügen Sie die Geräte-IP zu **Datenschutzmaßnahmen** hinzu, damit das Gerät auf Cookies zugreifen kann. |


## <a name="networking-requirements"></a>Netzwerkanforderungen

Ihr Datencenter verfügt über ein Hochgeschwindigkeitsnetzwerk. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, kann zum Kopieren der Daten eine 1-GbE-Datenverbindung verwendet werden, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt.

## <a name="next-step"></a>Nächster Schritt

* [Bereitstellen Ihrer Azure Data Box](data-box-deploy-ordered.md)

