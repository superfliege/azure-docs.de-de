---
title: Verwalten des Zugriffs auf Azure-Ressourcen mit Privileged Identity Management (PIM)
description: Erfahren Sie mehr über den Zugriff auf Azure-Ressourcen mithilfe der rollenbasierten Zugriffssteuerung in PIM.
services: active-directory
documentationcenter: ''
author: skwan
manager: mtillman
editor: bryanla
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 1e74579ef2f0e18f23a40dfc573177938b9b726f
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management"></a>Verwalten des Zugriffs auf Azure-Ressourcen mit Privileged Identity Management

Um privilegierte Konten vor bösartigen Cyberangriffen zu schützen, können Sie mit Azure Active Directory Privileged Identity Management (PIM) die Dauer der Aktivierung von Berechtigungen verkürzen und sich mittels Berichten und Warnungen einen besseren Überblick über deren Verwendung verschaffen. Hierfür werden Benutzern von PIM mit Beschränkungen auferlegt, indem ihnen Berechtigungen nur gemäß des Just-in-Time-Prinzips (JIT) zugeteilt oder nur für eine kurze Dauer zugewiesen und danach automatisch widerrufen werden. 

Sie können PIM nun mit der rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) verwenden, um den Zugriff auf Azure-Ressourcen zu verwalten, zu steuern und zu überwachen. PIM kann die Mitgliedschaft von integrierten und benutzerdefinierten Rollen verwalten, um Sie bei Folgendem zu unterstützen: 

- Bedarfsgesteuertes Aktivieren des Just-in-Time-Zugriffs auf Azure-Ressourcen
- Automatisches Ablaufen des Ressourcenzugriffs für zugewiesene Benutzer und Gruppen
- Zuweisen des temporären Zugriffs auf Azure-Ressourcen für kurze Aufgaben oder Bereitschaftszeitpläne
- Empfangen von Benachrichtigungen, wenn neuen Benutzern oder Gruppen Ressourcenzugriff zugewiesen wird und sie geeignete Zuweisungen aktivieren

Weitere Informationen finden Sie in der [Übersicht über die rollenbasierte Zugriffssteuerung in Azure PIM](privileged-identity-management/azure-pim-resource-rbac.md).