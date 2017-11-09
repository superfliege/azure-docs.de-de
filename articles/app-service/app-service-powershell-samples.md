---
title: "Azure PowerShell-Beispiele – App Service | Microsoft-Dokumentation"
description: "Azure PowerShell-Beispiele – App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b587f299e7b11effd05cbbc4b15976fee3f83ffd
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2017
---
# <a name="azure-powershell-samples"></a>Azure PowerShell-Beispiele

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe von Azure PowerShell erstellt wurden.

| | |
|-|-|
|**Erstellen einer App**||
| [Erstellen einer Web-App mit Bereitstellung über GitHub](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine Azure-Web-App, die Code aus GitHub bezieht. |
| [Erstellen einer Web-App dauerhafter Bereitstellung über GitHub](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine Azure-Web-App, die dauerhaft Code aus GitHub bereitstellt. |
| [Erstellen einer Web-App und Bereitstellen von Code mit FTP](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine Azure-Web-App und lädt Dateien aus einem lokalen Verzeichnis mithilfe von FTP hoch. |
| [Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine Azure-Web-App und konfiguriert einen Codepush aus einem lokalen Git-Repository. |
| [Erstellen einer Web-App und Bereitstellen von Code in einer Stagingumgebung](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine Azure-Web-App mit einem Bereitstellungsslot für Änderungen am Stagingcode. |
|**Konfigurieren der App**||
| [Zuordnen einer benutzerdefinierten Domäne zu einer Web-App](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine Azure-Web-App und ordnet ihr einen benutzerdefinierten Domänennamen zu. |
| [Binden eines benutzerdefinierten SSL-Zertifikats an eine Web-App](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine Azure-Web-App und bindet das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. |
|**Skalieren der App**||
| [Manuelles Skalieren einer Web-App](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine Azure-Web-App und skaliert sie über zwei Instanzen. |
| [Weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt zwei Azure-Web-Apps in zwei verschiedenen geografischen Regionen und macht sie mithilfe von Azure Traffic Manager über einen einzigen Endpunkt verfügbar. |
|**Verbinden der App mit Ressourcen**||
| [Verbinden einer Web-App mit einer SQL-­Datenbank](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine Azure-Web-App und eine SQL-Datenbank und fügt dann die Datenbank-Verbindungszeichenfolge zu den App-Einstellungen hinzu. |
| [Verbinden einer Web-App mit einem Speicherkonto](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine Azure-Web-App und ein Speicherkonto und fügt dann die Speicherverbindungszeichenfolge zu den App-Einstellungen hinzu. |
|**Sichern und Wiederherstellen einer App**||
| [Back up a web app](./scripts/app-service-powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Sichern einer Web-App) | Erstellt eine Azure-Web-App und eine einmalige Sicherung dafür. |
| [Create a scheduled backup for a web app](./scripts/app-service-powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Erstellen einer geplanten Sicherung für eine Web-App) | Erstellt eine Azure-Web-App und eine geplante Sicherung dafür. |
| [Delete a backup for a web app](./scripts/app-service-powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Löschen einer Sicherung für eine Web-App) | Löscht eine vorhandene Sicherung für eine Web-App. |
|**Überwachen der App**||
| [Überwachen einer Web-App mit Webserverprotokollen](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine Azure-Web-App, aktiviert die Protokollierung für die App und lädt die Protokolle auf Ihren lokalen Computer herunter. |
| | |
