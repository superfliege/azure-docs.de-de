---
title: Verbesserungen an Cloud App Discovery in Azure Active Directory | Microsoft-Dokumentation
description: Bietet Informationen zum Suchen und Verwalten von Anwendungen mit Cloud App Discovery, zu den Vorteilen und zu der Funktionsweise.
services: active-directory
keywords: Cloud App Discovery, Verwalten von Anwendungen
documentationcenter: 
author: curtand
manager: femila
tags: ignite
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2017
ms.author: curtand
ms.reviewer: nigu
ms.openlocfilehash: 59af2a5de5936d15456058aaeacfc334b9b34d4c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="cloud-app-discovery-enhancements-in-azure-active-directory"></a>Verbesserungen an Cloud App Discovery mit Azure Active Directory 
Mehr als 80 % der Arbeitnehmer geben an, nicht genehmigte SaaS-Apps für Arbeitszwecke zu verwenden. Wir integrieren jetzt Cloud App Discovery in Azure AD mit Microsoft Cloud App Security-Daten und -Analysefunktionen, damit Sie nicht verwaltete Cloud-Apps leichter erkennen können. Diese neue Integration erfordert andere Setupschritte als zuvor.

## <a name="what-can-i-do-now-with-the-improvements-to-cloud-app-discovery-in-azure-ad"></a>Welche Vorteile habe ich durch die Verbesserungen an Cloud App Discovery in Azure AD?

* **Einen tieferen Einblick in die Cloud-App-Nutzung**: Cloud App Discovery in Azure AD ermittelt nun **mehr als 15.000 Apps** aus dem gesamten Netzwerkdatenverkehr der Organisation und von jedem Gerät. 
* **Keine Agents erforderlich**: Diese neue Version von Cloud App Discovery erfordert keine Agents auf den Benutzergeräten. Stattdessen wird die Ermittlung basierend auf Protokolldateien ausgeführt, die von Firewalls und Proxys importiert werden. Sie können Apps im gesamte Netzwerkdatenverkehr der Organisation ermitteln, unabhängig vom Gerät oder Betriebssystem.
* **Laufende Analyse und Warnungen**: Cloud App Discovery in Azure AD stellt nun detaillierte fortlaufende Risikoanalysen sowie Warnungen bereit, wenn eine neue App verwendet wird. Sie können jetzt weitere fundierte Kenntnisse der Cloud-App-Nutzung in Ihrer Organisation erlangen, z.B. Informationen zum eingehenden und ausgehenden Datenverkehr und die aktivsten Benutzer von ermittelten Apps.

Wenn Sie nicht Active Directory Premium haben, finden Sie weitere Informationen unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

Mit diesem Link können Sie die [neue Cloud App Discovery-Umgebung in Azure AD](https://portal.cloudappsecurity.com) kennenlernen.

## <a name="next-steps"></a>Nächste Schritte
Richten Sie mit den folgenden Links Cloud App Discovery in Azure AD ein.

* [Erste Schritte mit Cloud App Discovery](cloudappdiscovery-get-started.md)
* [Erstellen von Momentaufnahmenberichten](cloudappdiscovery-set-up-snapshots.md)
* [Konfigurieren des automatischen Uploads von Protokollen für kontinuierliche Berichte](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Verwenden eines benutzerdefinierten Protokollparsers](https://docs.microsoft.comcommit/cloud-app-security/custom-log-parser)