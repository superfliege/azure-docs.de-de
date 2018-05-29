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
ms.openlocfilehash: c2686a2d5241af46e70263d1827028aa7e9b2138
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2018
ms.locfileid: "33206162"
---
# <a name="remove-the-sql-resource-provider"></a>Entfernen des SQL-Ressourcenanbieters

Zum Entfernen des SQL-Ressourcenanbieters müssen unbedingt zuerst alle Abhängigkeiten entfernt werden:

1. Stellen Sie sicher, dass Sie das ursprüngliche Bereitstellungspaket verwenden, das Sie für diese Version des SQL-Ressourcenanbieteradapters heruntergeladen haben.

2. Alle Benutzerdatenbanken müssen aus dem Ressourcenanbieter gelöscht werden. (Die Daten werden dabei nicht gelöscht.) Diese Aufgabe sollte von den Benutzern selbst ausgeführt werden.

3. Der Administrator muss die Hostserver aus dem SQL-Ressourcenanbieteradapter löschen.

4. Der Administrator muss alle Pläne löschen, die auf den SQL-Ressourcenanbieteradapter verweisen.

5. Der Administrator muss alle SKUs und Kontingente löschen, die dem SQL-Ressourcenanbieteradapter zugewiesen sind.

6. Führen Sie das Bereitstellungsskript mit den folgenden Elementen erneut aus:
    - Parameter „-Uninstall“
    - Azure Resource Manager-Endpunkte
    - DirectoryTenantID
    - Anmeldeinformationen für das Dienstadministratorkonto

