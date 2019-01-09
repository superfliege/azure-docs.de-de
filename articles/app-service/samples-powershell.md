---
title: Azure PowerShell-Beispiele – App Service | Microsoft-Dokumentation
description: Azure PowerShell-Beispiele – App Service
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 69fd27785e5fc16a79fc23728b6d1e50a0a7b834
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632856"
---
# <a name="powershell-samples-for-azure-app-service"></a>PowerShell-Beispiele für Azure App Service

Die folgende Tabelle enthält Links zu PowerShell-Skripts, die mithilfe von Azure PowerShell erstellt wurden.

| | |
|-|-|
|**Erstellen einer App**||
| [Erstellen einer App mit Bereitstellung über GitHub](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App, die Code aus GitHub bezieht. |
| [Erstellen einer App mit Continuous Deployment über GitHub](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App, die dauerhaft Code aus GitHub bereitstellt. |
| [Erstellen einer App und Bereitstellen von Code mit FTP](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und lädt Dateien aus einem lokalen Verzeichnis per FTP hoch. |
| [Erstellen einer App und Bereitstellen von Code über ein lokales GitHub-Repository](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und konfiguriert einen Codepush aus einem lokalen Git-Repository. |
| [Erstellen einer App und Bereitstellen von Code in einer Stagingumgebung](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App mit einem Bereitstellungsslot für Änderungen am Stagingcode. |
|**Konfigurieren der App**||
| [Zuordnen einer benutzerdefinierten Domäne zu einer App](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und ordnet ihr einen benutzerdefinierten Domänennamen zu. |
| [Binden eines benutzerdefinierten SSL-Zertifikats an eine App](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und bindet das SSL-Zertifikat eines benutzerdefinierten Domänennamens an die App. |
|**Skalieren der App**||
| [Manuelles Skalieren einer App](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und skaliert sie über zwei Instanzen. |
| [Weltweites Skalieren einer App mit einer Hochverfügbarkeitsarchitektur](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt zwei App Service-Apps in zwei verschiedenen geografischen Regionen und macht sie mithilfe von Azure Traffic Manager über einen zentralen Endpunkt verfügbar. |
|**Verbinden der App mit Ressourcen**||
| [Verbinden einer App mit einer SQL-Datenbank](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und eine SQL-Datenbank und fügt dann die Datenbank-Verbindungszeichenfolge den App-Einstellungen hinzu. |
| [Verbinden einer App mit einem Speicherkonto](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Erstellt eine App Service-App und ein Speicherkonto und fügt dann die Speicherverbindungszeichenfolge den App-Einstellungen hinzu. |
|**Sichern und Wiederherstellen einer App**||
| [Sichern einer App](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und eine einmalige Sicherung dafür. |
| [Erstellen einer geplanten Sicherung für eine App](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App und eine geplante Sicherung dafür. |
| [Löschen einer Sicherung für eine App](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Löscht eine vorhandene Sicherung für eine App. |
| [Restore a web app from a backup](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Wiederherstellen einer Web-App aus einem Backup) | Stellt eine App aus einer zuvor ausgeführten Sicherung wieder her. |
| [Restore a web app from a backup in another subscription](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) (Wiederherstellen einer Web-App aus einer Sicherung in einem anderen Abonnement) | Stellt eine Web-App aus einer Sicherung in einem anderen Abonnement wieder her. |
|**Überwachen der App**||
| [Überwachen einer Web-App mit Webserverprotokollen](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine App Service-App, aktiviert die Protokollierung für die App und lädt die Protokolle auf Ihren lokalen Computer herunter. |
| | |
