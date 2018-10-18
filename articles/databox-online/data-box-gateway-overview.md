---
title: Microsoft Azure Data Box Gateway – Übersicht | Microsoft-Dokumentation
description: In diesem Artikel wird Azure Data Box Gateway beschrieben, eine Speicherlösung mit einem virtuellen Gerät, die zum Übertragen von Daten in Azure dient.
services: databox
author: alkohli
ms.service: databox
ms.topic: overview
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 4f1ab6d955c81ce6f7b141eef42341f43bb379f6
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165316"
---
# <a name="what-is-azure-data-box-gateway-preview"></a>Was ist Azure Data Box Gateway (Vorschauversion)? 

Azure Data Box Gateway ist eine Speicherlösung, die zum nahtlosen Übertragen von Daten in Azure dient. Dieser Artikel bietet Ihnen einen Überblick über die Azure Data Box Gateway-Lösung, ihre Vorteile, ihre wichtigsten Funktionen und die Szenarien, in denen Sie dieses Gerät bereitstellen können. 

Data Box Gateway ist ein auf einem virtuellen Computer basierendes virtuelles Gerät, das in Ihrer virtualisierten Umgebung oder im Hypervisor bereitgestellt wird. Das virtuelle Gerät befindet sich an Ihrem Standort, und Sie beschreiben es mit den Protokollen NFS und SMB mit Daten. Das Gerät überträgt dann Ihre Daten in ein Azure-Blockblob, -Seitenblob oder Azure Files. 

> [!IMPORTANT]
> Data Box Gateway ist in der Vorschauphase. Lesen Sie [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen.

## <a name="use-cases"></a>Anwendungsfälle

Data Box Gateway kann für die Übertragung von Daten in die Cloud genutzt werden, z.B. zur Archivierung in der Cloud, Notfallwiederherstellung oder bei Bedarf zur Verarbeitung Ihrer Daten in der Cloud. Es folgen die verschiedenen Szenarien, in denen Data Box Gateway für die Datenübertragung verwendet werden kann.

- **Archivierung in der Cloud**: Kopieren Sie mit Data Box Gateway Hunderte von TBs an Daten auf sichere und effiziente Weise in Azure Storage. Zur Archivierung können die Daten einmalig oder fortlaufend erfasst werden.

- **Datenaggregation**: Aggregieren Sie Daten aus mehreren Quellen zur Datenverarbeitung und -analyse an zentraler Stelle in Azure Storage.  

- **Integration in lokale Workloads**: Führen Sie eine Integration in lokale Workloads wie Sicherung und Wiederherstellung durch, die Cloudspeicher nutzen und lokalen Zugriff auf häufig verwendete Dateien benötigen. 

## <a name="benefits"></a>Vorteile

Data Box Gateway hat die folgenden Vorteile:

- **Einfache Datenübertragung**: Macht das Verschieben von Daten in und aus Azure Storage so einfach wie das Arbeiten mit einer lokalen Netzwerkfreigabe.  
- **Hohe Leistung**: Erleichtert mit leistungsstarken Übertragungsfunktionen den Transport von Netzwerkdaten aus und in Azure. 
- **Schneller Zugriff**: Speichert die zuletzt verwendeten Dateien für einen schnellen Zugriff auf lokale Dateien im Cache zwischen.  
- **Eingeschränkte Bandbreitenauslastung**: Daten können in Azure geschrieben werden, auch wenn das Netzwerk gedrosselt ist, um die Nutzung während der Hauptgeschäftszeiten einzuschränken.  

## <a name="key-capabilities"></a>Wichtige Funktionen

Data Box Gateway bietet die folgenden Funktionen:

|Funktion |BESCHREIBUNG  |
|---------|---------|
|Geschwindigkeit     | Vollständig automatisierte und stark optimierte Datenübertragung und Bandbreite.|
|Unterstützte Protokolle     | Für die Datenerfassung werden die Standardprotokolle SMB und NFS unterstützt. <br> Weitere Informationen zu unterstützten Versionen finden Sie unter [Systemanforderungen für Data Box Gateway](data-box-gateway-system-requirements.md).|
|Datenzugriff     | Direkter Datenzugriff aus Azure Storage-Blobs und Azure Files über Cloud-APIs zur zusätzlichen Datenverarbeitung in der Cloud.|
|Schneller Zugriff     | Lokaler Cache auf dem Gerät für schnellen Zugriff auf zuletzt verwendete Dateien.|
|Hochladen von Daten im Offlinemodus     | Der Modus „Getrennt“ unterstützt Szenarien für das Hochladen von Daten im Offlinemodus.|
|Datenaktualisierung     | Möglichkeit, lokale Dateien mit den neuesten Informationen aus der Cloud zu aktualisieren.|
|Verschlüsselung    | BitLocker-Unterstützung zur lokalen Verschlüsselung von Daten und zur sicheren Datenübertragung in die Cloud über *HTTPS*.       |
|Resilienz     | Integrierte Netzwerkresilienz.        |


## <a name="specifications"></a>Spezifikationen

Das virtuelle Data Box Gateway-Gerät weist die folgenden Merkmale auf:

| Spezifikationen                                          | BESCHREIBUNG              |
|---------------------------------------------------------|--------------------------|
| Virtuelle Prozessoren (Kerne)   | Mindestens 4 |            
| Arbeitsspeicher  | Mindestens 8 GB|
| Verfügbarkeit|Einzelner Knoten|
| Datenträger| Betriebssystem-Datenträger: 250 GB <br> Datenträger für Daten: Mindestens 2 TB, für schlanke Speicherzuweisung geeignet, muss durch SSD-Datenträger unterstützt werden|
| Netzwerkschnittstellen|Mindestens eine virtuelle Netzwerkschnittstelle|
| Native Dateifreigabeprotokolle|SMB und NFS  |
| Sicherheit| Authentifizierung, um den Zugriff auf Gerät und Daten zu entsperren <br> Verschlüsselung der Daten während der Übertragung mithilfe von AES-256-BitLocker-Verschlüsselung|
| Verwaltung| Lokale Webbenutzeroberfläche: Ersteinrichtung, Diagnose und Energieverwaltung für das Gerät <br> Azure-Portal: Für die sonstige Verwaltung von Data Box Gateway-Geräten       |


## <a name="components"></a>Komponenten

Die Data Box Gateway-Lösung umfasst die Data Box Gateway-Ressource, ein virtuelles Data Box Gateway-Gerät und eine lokale Webbenutzeroberfläche.

* **Virtuelles Data Box Gateway-Gerät**: Ein auf einem virtuellen Computer basierendes virtuelles Gerät, das in Ihrer virtualisierten Umgebung oder im Hypervisor bereitgestellt wird und die Übertragung von Daten in Azure ermöglicht. 
    
* **Data Box Gateway-Ressource:** Eine Ressource im Azure-Portal, mit der Sie ein Data Box Gateway-Gerät auf einer zentralen Webbenutzeroberfläche verwalten können, auf die an verschiedenen geografischen Standorten zugegriffen werden kann. Verwenden Sie die Data Box Gateway-Ressource, um Ressourcen zu erstellen und zu verwalten, Geräte und Warnungen anzuzeigen und zu verwalten sowie Freigaben zu verwalten.  

    <!--![The Data Box Gateway service in Azure portal](media/data-box-overview/data-box-Gateway-service1.png)-->

    <!--For more information, go to [Use the Data Box Gateway service to administer your Data Box Gateway device](data-box-gateway-portal-ui-admin.md).-->

* **Lokale Webbenutzeroberfläche für Data Box**: Auf der lokalen Webbenutzeroberfläche können Sie Diagnosen ausführen, das Data Box Gateway-Gerät herunterfahren und neu starten, Kopierprotokolle anzeigen und den Microsoft-Support kontaktieren, um eine Serviceanfrage zu stellen.

    <!--![The Data Box Gateway local web UI](media/data-box-gateway-overview/data-box-gateway-local-web-ui.png)-->

    <!-- For information about using the web-based UI, go to [Use the web-based UI to administer your Data Box](data-box-gateway-portal-ui-admin.md).-->


## <a name="region-availability"></a>Regionale Verfügbarkeit

Das virtuelle Data Box Gateway-Gerät, die Azure-Ressource und das Zielspeicherkonto, an das Sie Daten übertragen, müssen sich nicht alle in der gleichen Region befinden.

- **Verfügbarkeit der Ressource**: Bei dieser Version ist die Data Box Gateway-Ressource in den folgenden Regionen verfügbar:
    - **USA**: USA, Westen 2 und USA, Osten
    - **Europa**: Europa, Westen
    - **Asien-Pazifik**: Asien-Pazifik, Südosten

- **Zielspeicherkonten**: Die Speicherkonten, in denen die Daten gespeichert werden, sind in allen Azure-Regionen verfügbar. 

    Die Regionen, in denen die Speicherkonten Data Box-Daten speichern, sollten sich in der Nähe des Standorts des Geräts befinden, um eine optimale Leistung zu erzielen. Ein weit entfernt vom Gerät befindliches Speicherkonto führt zu langen Wartezeiten und einer langsameren Leistung. 


## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Data Box Gateway-Systemanforderungen](data-box-gateway-system-requirements.md) an.
- Machen Sie sich mit den [Data Box Gateway-Einschränkungen](data-box-gateway-limits.md) vertraut.
- Stellen Sie [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) im Azure-Portal bereit.




