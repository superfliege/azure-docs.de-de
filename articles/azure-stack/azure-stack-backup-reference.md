---
title: Referenz für den Infrastructure Backup-Dienst für Azure Stack | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzmaterial für den Infrastructure Backup-Dienst für Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 80cca629ea49d8843580c23f74d1d2076aa623e3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246710"
---
# <a name="infrastructure-backup-service-reference"></a>Referenz für den Infrastructure Backup-Dienst

## <a name="azure-backup-infrastructure"></a>Azure-Sicherungsinfrastruktur

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack besteht aus vielen Diensten, die das Portal, Azure Resource Manager und die Infrastrukturverwaltung umfassen. Ziel der anwendungsähnlichen Verwaltung von Azure Stack ist die Reduzierung der Komplexität für Bediener der Lösung.

Infrastructure Backup ist so konzipiert, dass die komplexen Vorgänge zur Sicherung und Wiederherstellung von Daten intern in den Infrastrukturdiensten durchgeführt werden, sodass Bediener sich auf die Verwaltung der Lösung und die Einhaltung einer SLA für Benutzer konzentrieren können.

Um zu vermeiden, dass Sicherungen auf demselben System gespeichert werden, ist der Export der Sicherungsdaten in eine externe Freigabe erforderlich. Aufgrund der Erfordernis einer externen Freigabe kann der Administrator basierend auf vorhandenen Richtlinien des Unternehmens zur Sicherung und Notfallwiederherstellung den Speicherort der Daten flexibel festlegen. 

### <a name="infrastructure-backup-components"></a>Infrastructure Backup-Komponenten

Infrastructure Backup enthält die folgenden Komponenten:

 - **Infrastructure Backup Controller**  
 Infrastructure Backup Controller wird mit jeder Azure Stack-Cloud instanziiert und ist in jeder Azure Stack-Cloud enthalten.
 - **Backup Resource Provider**  
 Backup Resource Provider (Backup RP) besteht aus der Benutzeroberfläche und Anwendungsprogrammierschnittstellen (APIs), die grundlegende Sicherungsfunktionen für die Azure Stack-Infrastruktur bereitstellen.

#### <a name="infrastructure-backup-controller"></a>Infrastructure Backup Controller

Infrastructure Backup Controller ist ein Service Fabric-Dienst, der für eine Azure Stack-Cloud instanziiert wird. Sicherungsressourcen werden auf regionaler Ebene erstellt und erfassen regionsspezifische Daten von AD, Zertifizierungsstellen, Azure Resource Manager, CRP, SRP, NRP, KeyVault und RBAC. 

### <a name="backup-resource-provider"></a>Backup Resource Provider

Backup Resource Provider dient als Benutzeroberfläche im Azure Stack-Portal zur Basiskonfiguration und Anzeige von Sicherungsressourcen. Der Bediener kann über die Benutzeroberfläche die folgenden Vorgänge durchführen:

 - Erstmaliges Aktivieren der Sicherung durch Angabe des externen Speicherorts, der Anmeldeinformationen und des Verschlüsselungsschlüssels
 - Anzeigen der fertig erstellten Sicherungsressourcen und des Status der noch in Erstellung befindlichen Ressourcen
 - Ändern des Speicherorts, in dem Backup Controller Sicherungsdaten speichert
 - Ändern der Anmeldeinformationen, über die Backup Controller auf den externen Speicherort zugreift
 - Ändern des Verschlüsselungsschlüssels, mit dem Backup Controller Sicherungen verschlüsselt 


## <a name="backup-controller-requirements"></a>Anforderungen an Backup Controller

In diesem Abschnitt werden die wichtigen Anforderungen für Infrastructure Backup beschrieben. Sie sollten die Informationen sorgfältig überprüfen, bevor Sie die Sicherung für Ihre Azure Stack-Instanz aktivieren. Auch später sollten Sie während der Bereitstellung und beim nachfolgenden Betrieb bei Bedarf als Referenz darauf zurückgreifen.

Es handelt sich um folgende Anforderungen:

  - **Softwareanforderungen:** unterstützte Speicherorte und Angaben zur Größe 
  - **Netzwerkanforderungen:** Netzwerkanforderungen für unterschiedliche Speicherorte  

### <a name="software-requirements"></a>Softwareanforderungen

#### <a name="supported-storage-locations"></a>Unterstützte Speicherorte

| Speicherort                                                                 | Details                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| SMB-Dateifreigabe, gehostet auf einem Speichergerät innerhalb der vertrauenswürdigen Netzwerkumgebung | SMB-Freigabe im selben Rechenzentrum, in dem Azure Stack bereitgestellt wird, oder in einem anderen Rechenzentrum. Mehrere Azure Stack-Instanzen können dieselbe Dateifreigabe verwenden. |
| SMB-Dateifreigabe in Azure                                                          | Derzeit nicht unterstützt.                                                                                                                                 |
| Blob Storage in Azure                                                            | Derzeit nicht unterstützt.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Unterstützte SMB-Versionen

| SMB | Version |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Speicherortgröße 

Infrastructure Backup Controller sichert Daten bedarfsgesteuert. Es empfiehlt sich, die Sicherung mindestens zweimal täglich durchzuführen und Sicherungen höchstens sieben Tage lang aufzubewahren. 

**1811 und höher**
| Umgebungsgröße | Voraussichtliche Größe der Sicherung | Gesamtmenge des erforderlichen Speicherplatzes |
|-------------------|--------------------------|--------------------------------|
| 4 bis 16 Knoten        | 20 GB                    | 280 GB                        |
| ASDK              | 10 GB                    | 140 GB                        |

**Vor 1811**
| Umgebungsgröße | Voraussichtliche Größe der Sicherung | Gesamtmenge des erforderlichen Speicherplatzes |
|-------------------|--------------------------|--------------------------------|
| 4–16 Knoten, ASDK  | 10 GB                     | 140 GB                        |

### <a name="network-requirements"></a>Netzwerkanforderungen
| Speicherort                                                                 | Details                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SMB-Dateifreigabe, gehostet auf einem Speichergerät innerhalb der vertrauenswürdigen Netzwerkumgebung | Port 445 ist erforderlich, wenn sich die Azure Stack-Instanz in einer Umgebung mit Firewall befindet. Infrastructure Backup Controller initiiert eine Verbindung mit dem SMB-Dateiserver über Port 445. |
| Zur Verwendung des FQDN des Dateiservers muss der Name über den PEP aufgelöst werden können.             |                                                                                                                                                                                         |

> [!Note]  
> Es müssen keine eingehenden Ports geöffnet werden.


## <a name="infrastructure-backup-limits"></a>Beschränkungen von Infrastructure Backup

Berücksichtigen Sie folgende Einschränkungen beim Planen, Bereitstellen und Betreiben von Microsoft Azure Stack-Instanzen. Die Einschränkungen sind in der folgenden Tabelle beschrieben.

### <a name="infrastructure-backup-limits"></a>Beschränkungen von Infrastructure Backup
| Grenzwertbezeichner                                                 | Begrenzung        | Kommentare                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Sicherungstyp                                                      | Nur vollständig    | Infrastructure Backup Controller unterstützt nur vollständige Sicherungen. Inkrementelle Sicherungen werden nicht unterstützt.                                          |
| Geplante Sicherungen                                                | Geplant und manuell  | Backup Controller unterstützt geplante und bedarfsgesteuerte Sicherungen.                                                                                 |
| Maximale Anzahl gleichzeitiger Sicherungsaufträge                                   | 1            | Pro Backup Controller-Instanz wird nur ein aktiver Sicherungsauftrag unterstützt.                                                                  |
| Netzwerkswitchkonfiguration                                     | Nicht zutreffend | Der Administrator muss die Netzwerkswitchkonfiguration mithilfe von OEM-Tools sichern. Informationen finden Sie in der Dokumentation der einzelnen OEM-Anbieter zu Azure Stack. |
| Hardwarelebenszyklus-Host                                          | Nicht zutreffend | Der Administrator muss den Hardwarelebenszyklus-Host mithilfe der OEM-Tools sichern. Informationen finden Sie in der Dokumentation der einzelnen OEM-Anbieter zu Azure Stack.      |
| Maximale Anzahl der Dateifreigaben                                    | 1            | Zum Speichern von Sicherungsdaten kann nur eine Dateifreigabe verwendet werden.                                                                                        |
| Backup App Services-, Function-, SQL-, MySQL-Ressourcenanbieterdaten | Nicht zutreffend | Informationen finden Sie in der Anleitung zur Bereitstellung und Verwaltung von durch Microsoft erstellten Erweiterungs-RPs.                                                  |
| Sicherung von Drittanbieter-RPs                              | Nicht zutreffend | Informationen finden Sie in der Anleitung zur Bereitstellung und Verwaltung von durch Drittanbieter erstellten Erweiterungs-RPs.                                          |

## <a name="next-steps"></a>Nächste Schritte

 - Weitere Informationen zum Infrastructure Backup-Dienst finden Sie unter [Sicherung und Datenwiederherstellung für Azure Stack mit dem Infrastructure Backup-Dienst](azure-stack-backup-infrastructure-backup.md).
