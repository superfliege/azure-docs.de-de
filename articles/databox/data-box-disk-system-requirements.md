---
title: Systemanforderungen für Microsoft Azure Data Box-Datenträger | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Software- und Netzwerkanforderungen für Ihre Azure Data Box-Datenträger.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 12/27/2018
ms.author: alkohli
ms.openlocfilehash: 5debc14a6a20c42b62b9a7b2c524e36e94302221
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792865"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Systemanforderungen für Microsoft Azure Data Box-Datenträger (Vorschauversion)

In diesem Artikel werden wichtige Systemanforderungen für Ihre Microsoft Azure Data Box-Datenträgerlösung und die Clients beschrieben, die mit dem Data Box-Datenträger verbunden sind. Sie sollten die Informationen sorgfältig lesen, bevor Sie Ihren Data Box-Datenträger bereitstellen. Auch später sollten Sie während der Bereitstellung und beim nachfolgenden Betrieb bei Bedarf als Referenz darauf zurückgreifen.

> [!IMPORTANT]
> Der Data Box-Datenträger befindet sich in der Vorschauversion. Lesen Sie [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen. 

Die Systemanforderungen enthalten die unterstützten Plattformen für Clients, die eine Verbindung mit Datenträgern, unterstützten Speicherkonten und Speichertypen herstellen.


## <a name="supported-operating-systems-for-clients"></a>Unterstützte Betriebssysteme für Clients

Im Folgenden finden Sie eine Liste der unterstützten Betriebssysteme für Datenträgerentsperrungen und Datenkopiervorgänge über die Clients, die mit dem Data Box-Datenträger verbunden sind.

| **Betriebssystem** | **Getestete Version** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Andere erforderliche Software für Windows-Clients

Für den Windows-Client sollte darüber hinaus noch Folgendes installiert sein.

| **Software**| **Version** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Andere erforderliche Software für Linux-Clients

Für den Linux-Client installiert das Data Box-Datenträgertoolset die folgende erforderliche Software:

- dislocker
- OpenSSL

## <a name="supported-storage-accounts"></a>Unterstützte Speicherkonten

Im Folgenden finden Sie eine Liste der unterstützten Speichertypen für den Data Box-Datenträger.

| **Speicherkonto** | **Hinweise** |
| --- | --- |
| Klassisch | Standard |
| Allgemeiner Zweck  |Standard: sowohl V1 als auch V2 werden unterstützt. Es werden sowohl „heiße“ als auch „kalte“ Ebenen unterstützt. |

>[!NOTE]
> Azure Data Lake Storage Gen 2-Konten werden nicht unterstützt.


## <a name="supported-storage-types"></a>Unterstützte Speichertypen

Im Folgenden finden Sie eine Liste der unterstützten Speichertypen für den Data Box-Datenträger.

| **Dateiformat** | **Hinweise** |
| --- | --- |
| Azure-Blockblob | |
| Azure-Seitenblob  | |


## <a name="next-step"></a>Nächster Schritt

* [Bereitstellen des Azure Data Box-Datenträgers](data-box-disk-deploy-ordered.md)

