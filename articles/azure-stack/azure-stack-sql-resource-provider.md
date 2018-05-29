---
title: Verwenden von SQL-Datenbanken in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in wenigen Schritten SQL-Datenbanken als Dienst in Azure Stack und den SQL Server-Ressourcenanbieteradapter bereitstellen können.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: b1cc1fad6b0831bcf0bab5ba4f37b753c3cf33ca
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33206163"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Verwenden von SQL-Datenbanken in Microsoft Azure Stack
Verwenden Sie den SQL Server-Ressourcenanbieteradapter, um SQL-Datenbanken als Dienst in [Azure Stack](azure-stack-poc.md) verfügbar zu machen. Nachdem Sie den Ressourcenanbieter installiert und mit mindestens einer SQL Server-Instanz verbunden haben, können Sie und Ihre Benutzer damit Folgendes erstellen:
- Datenbanken für native Cloud-Apps.
- Websites, die auf SQL basieren.
- Workloads, die auf SQL basieren.
Sie müssen nicht jedes Mal einen virtuellen Computer bereitstellen, der SQL Server hostet.

Der Ressourcenanbieter unterstützt nicht alle Datenbankverwaltungsfunktionen von [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/). Pools für elastische Datenbanken und die automatische Skalierung der Datenbankleistung nach oben oder unten sind beispielsweise nicht verfügbar. Der Ressourcenanbieter unterstützt jedoch ähnliche Vorgänge für das Erstellen, Lesen, Aktualisieren und Löschen. Die API ist nicht mit SQL-Datenbank kompatibel.

## <a name="sql-resource-provider-adapter-architecture"></a>Architektur des SQL-Ressourcenanbieteradapters
Der Ressourcenanbieter besteht aus drei Komponenten:

- **Dem virtuellen Computer des SQL-Ressourcenanbieteradapters**, einem virtuellen Windows-Computer, der die Anbieterdienste ausführt.
- **Dem Ressourcenanbieter selbst**, der die Bereitstellungsanforderungen verarbeitet und Datenbankressourcen verfügbar macht
- **Servern, die SQL Server hosten**, die Kapazität für Datenbanken bereitstellen und als „Hostserver“ bezeichnet werden.

Sie müssen mindestens eine SQL Server-Instanz erstellen bzw. den Zugriff auf externe SQL Server-Instanzen bereitstellen.

> [!NOTE]
> Hostserver, die auf integrierten Azure Stack-Systemen installiert werden, müssen mit einem Mandantenabonnement erstellt werden. Sie können nicht mit dem Standardabonnement des Anbieters erstellt werden. Sie müssen im Mandantenportal oder einer PowerShell-Sitzung mit einer geeigneten Anmeldung erstellt werden. Alle Hostserver sind abrechenbare VMs, die entsprechende Lizenzen benötigen. Der Dienstadministrator kann Besitzer des Mandantenabonnements sein.


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des SQL Server-Ressourcenanbieters](azure-stack-sql-resource-provider-deploy.md)
