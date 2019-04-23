---
title: Allgemeine SAS-URL-Probleme und Fehlerbehebungen für den Azure Marketplace | Microsoft-Dokumentation
description: Liste allgemeiner Probleme in Verbindung mit der Verwendung von SAS-URIs und möglicher Lösungen.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/27/2018
ms.author: pbutlerm
ms.openlocfilehash: abb29cd0d31288ba7bfab7024cf7657ab6b9a3d3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58879215"
---
# <a name="common-sas-url-issues-and-fixes"></a>Allgemeine SAS-URL-Probleme und Fehlerbehebungen

Die folgende Tabelle enthält einige allgemeine Probleme im Zusammenhang mit der Verwendung von Shared Access Signatures (dienen zur Identifizierung und gemeinsamen Verwendung der hochgeladenen VHDs für Ihre Lösung) sowie Lösungsvorschläge.

| **Problem** | **Fehlermeldung** | **Behebung** | 
| --------- | ------------------- | ------- | 
| &emsp;  *Fehler beim Kopieren von Images* |  |  |
| „?“ wurde in der SAS-URL nicht gefunden. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aktualisieren Sie die SAS-URL mithilfe empfohlener Tools. |
| Die Parameter „st“ und „se“ sind in der SAS-URL nicht enthalten. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aktualisieren Sie die SAS-URL mit geeigneten Werten für **Startdatum** und **Enddatum**. | 
| „sp=rl“ ist in der SAS-URL nicht enthalten. | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | Aktualisieren Sie die SAS-URL mit den Berechtigungen `Read` und `List`. | 
| Der VHD-Name in der SAS-URL enthält Leerzeichen. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Entfernen Sie die Leerzeichen aus der SAS-URL. |
| SAS-URL-Autorisierungsfehler | `Failure: Copying Images. Not able to download blob due to authorization error` | Überprüfen und korrigieren Sie das SAS-URI-Format. Generieren Sie ihn bei Bedarf neu. |
| Die Parameter „st“ und „se“ der SAS-URL verfügen nicht über eine vollständige Angabe für Datum und Uhrzeit. | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | Für ein vollständiges Datums- und Uhrzeitformat (Beispiel: `11-02-2017T00:00:00Z`) sind SAS-URL-Parameter für **Startdatum** und **Enddatum** (Teilzeichenfolgen `st` und `se`) erforderlich. Verkürzte Versionen sind nicht zulässig. (Von bestimmten Befehlen der Azure-Befehlszeilenschnittstelle werden unter Umständen standardmäßig verkürzte Werte generiert.) | 
|  |  |  |

Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
