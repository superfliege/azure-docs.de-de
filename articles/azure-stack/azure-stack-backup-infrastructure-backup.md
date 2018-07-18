---
title: Sicherung und Datenwiederherstellung für Azure Stack mit dem Dienst für die Infrastruktursicherung | Microsoft-Dokumentation
description: Sie können Konfigurations- und Dienstdaten mithilfe des Diensts für die Infrastruktursicherung sichern und wiederherstellen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9B51A3FB-EEFC-4CD8-84A8-38C52CFAD2E4
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/20/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: 12138ac5a173f66d8b6b0041de9f31f4ac326485
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34822954"
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Sicherung und Datenwiederherstellung für Azure Stack mit dem Dienst für die Infrastruktursicherung

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Sie können Konfigurations- und Dienstdaten mithilfe des Diensts für die Infrastruktursicherung sichern und wiederherstellen. Jede Azure Stack-Installation enthält eine Instanz des Diensts. Sie können die vom Dienst erstellten Sicherungen für die erneute Bereitstellung der Azure Stack-Cloud verwenden, um Identitäts-, Sicherheits- und Azure Resource Manager-Daten wiederherzustellen.

Sie können die Sicherung aktivieren, wenn Sie bereit sind, Ihre Cloud in der Produktion einzusetzen. Aktivieren Sie die Sicherung nicht, wenn Sie planen, für einen längeren Zeitraum Tests und Überprüfungen durchzuführen.

Stellen Sie vor der Aktivierung des Sicherungsdiensts sicher, dass die [Voraussetzungen erfüllt sind](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> Der Dienst für die Infrastruktursicherung deckt keine Benutzerdaten und Anwendungen ab. In den folgenden Artikeln finden Sie Anweisungen zum Sichern und Wiederherstellen der Ressourcenanbieter [App Services](https://aka.ms/azure-stack-app-service), [SQL](https://aka.ms/azure-stack-ms-sql) und [MySQL](https://aka.ms/azure-stack-mysql) und zugehöriger Benutzerdaten.

## <a name="the-infrastructure-backup-service"></a>Dienst für die Infrastruktursicherung

Der Dienst umfasst die folgenden Features.

| Feature                                            | BESCHREIBUNG                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sicherungsinfrastrukturdienste                     | Koordiniert die Sicherung für eine Teilmenge der Infrastrukturdienste in Azure Stack. Im Notfall können die Daten im Rahmen einer erneuten Bereitstellung wiederhergestellt werden. |
| Komprimierung und Verschlüsslung von exportierten Sicherungsdaten | Die Sicherungsdaten werden vom System komprimiert und verschlüsselt, bevor sie an den externen Speicherort exportiert werden, der vom Administrator angegeben wurde.                |
| Überwachung von Sicherungsaufträgen                              | Das System gibt Benachrichtigungen zu Fehlern bei Sicherungsaufträgen aus und zeigt Bereinigungsschritte an.                                                                                                |
| Benutzeroberfläche für die Sicherungsverwaltung                       | Der Sicherungsressourcenanbieter unterstützt die Aktivierung der Sicherung.                                                                                                                         |
| Cloudwiederherstellung                                     | Bei einem schwerwiegenden Datenverlust können Sicherungen dazu verwendet werden, grundlegende Azure Stack-Informationen im Rahmen der Bereitstellung wiederherzustellen.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Überprüfen der Anforderungen für den Dienst zur Infrastruktursicherung

- **Speicherort**  
  Sie benötigen eine Dateifreigabe, die von Azure Stack aus zugänglich ist und sieben Sicherungen enthalten kann. Jede Sicherung ist etwa 10 GB groß. Ihre Freigabe sollte Sicherungen mit einer Größe von 140 GB speichern können. Weitere Informationen zur Auswahl eines Speicherorts für den Azure Stack-Dienst für die Infrastruktursicherung finden Sie unter [Anforderungen für den Sicherungscontroller](azure-stack-backup-reference.md#backup-controller-requirements).
- **Anmeldeinformationen**  
  Sie benötigen ein Domänenbenutzerkonto und Anmeldeinformationen. Sie können beispielsweise die Anmeldeinformationen des Azure Stack-Administrators verwenden.
- **Verschlüsselungsschlüssel**  
  Die Sicherungsdateien werden mit diesem Schlüssel verschlüsselt. Stellen Sie sicher, dass Sie diesen Schlüssel an einem sicheren Speicherort speichern. Nachdem Sie diesen Schlüssel zum ersten Mal festgelegt haben oder künftig ändern, kann er über diese Schnittstelle nicht mehr angezeigt werden. Weitere Informationen zum Generieren eines vorinstallierten Schlüssels finden Sie in den Skripts unter [Aktivieren der Sicherung für Azure Stack mit PowerShell](azure-stack-backup-enable-backup-powershell.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Aktivieren der Sicherung für Azure Stack über das Verwaltungsportal](azure-stack-backup-enable-backup-console.md).
- Erfahren Sie mehr über das [Aktivieren der Sicherung für Azure Stack mit PowerShell](azure-stack-backup-enable-backup-powershell.md).
- Erfahren Sie mehr über das [Sichern von Azure Stack](azure-stack-backup-back-up-azure-stack.md ).
- Erfahren Sie mehr über das [Wiederherstellen nach schwerwiegendem Datenverlust](azure-stack-backup-recover-data.md).
