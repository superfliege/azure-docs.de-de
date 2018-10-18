---
title: Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Konfigurieren Sie die benötigte Verbindungsinfrastruktur, um SAP HANA in Azure (große Instanzen) zu verwenden.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c61dffc6fd9d0c65f5e925daebdf2a02cfb5d6ba
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161355"
---
# <a name="sap-hana-large-instances-deployment"></a>Bereitstellung von SAP HANA (große Instanzen) 

Nachdem der Kauf von SAP HANA in Azure (große Instanzen) zwischen Ihnen und dem Microsoft Enterprise-Kundenbetreuungsteam abgewickelt ist, benötigt Microsoft die folgenden Informationen, um HANA (große Instanz)-Einheiten bereitzustellen:

- Kundenname
- Geschäftliche Kontaktinformationen (samt E-Mail-Adresse und Telefonnummer)
- Technische Kontaktinformationen (samt E-Mail-Adresse und Telefonnummer)
- Technische Kontaktinformationen zum Netzwerk (samt E-Mail-Adresse und Telefonnummer)
- Region der Azure-Bereitstellung („USA, Westen“, „USA, Osten“, „Australien, Osten“, „Australien, Südosten“, „Europa, Westen“ und „Europa, Norden“ ab Juli 2017)
- SKU für SAP HANA in Azure (große Instanzen) (Konfiguration)
- Wie bereits im Dokument mit der Übersicht und Architektur für HANA (große Instanzen) ausgeführt, für jede Azure-Region, in der eine Bereitstellung erfolgt:
    - Ein /29-IP-Adressbereich für ER-P2P-Verbindungen, über die Azure-VNets mit HANA (große Instanzen) verbunden werden
    - Ein /24-CIDR-Block, der für den HANA (große Instanzen)-Server-IP-Pool verwendet wird
- Die IP-Adressbereichswerte, die im „VNet-Adressraum“-Attribut jedes Azure-VNets verwendet werden, für das Verbindungen mit HANA (große Instanzen) hergestellt werden
- Daten für jedes HANA (große Instanzen)-System:
  - Der gewünschte Hostname – idealerweise mit vollqualifiziertem Domänennamen
  - Die gewünschte IP-Adresse für die Einheit von HANA (große Instanzen) aus dem Adressbereich des Server-IP-Pools. Beachten Sie, dass die ersten 30 IP-Adressen im Adressbereich des Server-IP-Pools für die interne Verwendung in HANA (große Instanzen) reserviert sind.
  - SAP HANA-SID-Name für die SAP HANA-Instanz (zum Erstellen der benötigten SAP HANA-bezogenen Datenträgervolumes erforderlich). Die HANA-SID wird zum Erstellen der Berechtigungen für „sidadm“ auf den NFS-Volumes benötigt, die an die Einheit von HANA (große Instanz) angefügt werden. Außerdem wird sie als eine der Namenskomponenten der Datenträgervolumes verwendet, die bereitgestellt werden. Wenn Sie auf der Einheit mehr als eine HANA-Instanz ausführen möchten, müssen Sie mehrere HANA-SIDs auflisten. Für jede Instanz wird jeweils ein separater Volumesatz zugewiesen.
  - Die Gruppen-ID (groupid), die der Benutzer „sidadm“ in Linux hat, wird zum Erstellen der erforderlichen SAP HANA-bezogenen Datenträgervolumes benötigt. Bei einer SAP HANA-Installation wird die Gruppe „sapsys“ in der Regel mit der Gruppen-ID „1001“ erstellt. Der Benutzer „sidadm“ gehört zu dieser Gruppe.
  - Die Benutzer-ID (userid), die der Benutzer „sidadm“ in Linux hat, wird zum Erstellen der erforderlichen SAP HANA-bezogenen Datenträgervolumes benötigt. Wenn Sie auf der Einheit mehrere HANA-Instanzen ausführen, müssen Sie alle Benutzer vom Typ „<sid>adm“ auflisten. 
- Die ID des Azure-Abonnements, mit dem SAP HANA in Azure HANA (große Instanzen) direkt verbunden wird. Diese Abonnement-ID verweist auf das Azure-Abonnement, für das die HANA-Einheiten (große Instanzen) berechnet werden.

Nachdem Sie die Informationen bereitgestellt haben, stellt Microsoft SAP HANA in Azure (große Instanzen) bereit, und Microsoft gibt die Informationen zurück, die erforderlich sind, damit Sie Ihre Azure-VNets mit HANA (große Instanzen) verbinden und auf die HANA (große Instanz)-Einheiten zugreifen können.

Machen Sie sich vor dem Lesen dieses Artikels mit [allgemeinen Begriffen zu HANA (große Instanzen)](hana-know-terms.md) und [SKUs für HANA (große Instanzen)](hana-available-skus.md) vertraut.

Über die nachstehende Abfolge können Sie nach der Bereitstellung durch Microsoft eine Verbindung mit HANA (große Instanzen) herstellen:

1. [Verbinden von virtuellen Azure-Computern mit HANA (große Instanzen)](hana-connect-azure-vm-large-instances.md)
2. [Herstellen einer ExpressRoute-Verbindung zwischen einem VNET und HANA (große Instanzen)](hana-connect-vnet-express-route.md)
3. [Zusätzliche Netzwerkanforderungen (optional)](hana-additional-network-requirements.md)

**Nächste Schritte**

- Lesen Sie [Verbinden von virtuellen Azure-Computern mit HANA (große Instanzen)](hana-connect-azure-vm-large-instances.md).
- Lesen Sie [Herstellen einer ExpressRoute-Verbindung zwischen einem VNET und HANA (große Instanzen)](hana-connect-vnet-express-route.md).