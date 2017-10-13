---
title: Verwalten des Zugriffs auf Azure-Ressourcen mit Privileged Identity Management (PIM)
description: "Erfahren Sie mehr über den Zugriff auf Azure-Ressourcen mithilfe der rollenbasierten Zugriffssteuerung in PIM."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: bryanla
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: skwan
ms.openlocfilehash: 4245d0dd9798046674f9ae3b197cec6b9b5d2545
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management-preview"></a>Verwalten des Zugriffs auf Azure-Ressourcen mit Privileged Identity Management (Vorschau)

Um privilegierte Konten vor bösartigen Cyberangriffen zu schützen, können Sie mit Azure Active Directory Privileged Identity Management (PIM) die Dauer der Aktivierung von Berechtigungen verkürzen und sich mittels Berichten und Warnungen einen besseren Überblick über deren Verwendung verschaffen. Hierfür werden Benutzern von PIM mit Beschränkungen auferlegt, indem ihnen Berechtigungen nur gemäß des Just-in-Time-Prinzips (JIT) zugeteilt oder nur für eine kurze Dauer zugewiesen und danach automatisch widerrufen werden. 

Sie können PIM nun mit der rollenbasierten Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) verwenden, um den Zugriff auf Azure-Ressourcen zu verwalten, zu steuern und zu überwachen. PIM kann die Mitgliedschaft von integrierten und benutzerdefinierten Rollen verwalten, um Sie bei Folgendem zu unterstützen: 

- Bedarfsgesteuertes Aktivieren des Just-in-Time-Zugriffs auf Azure-Ressourcen
- Automatisches Ablaufen des Ressourcenzugriffs für zugewiesene Benutzer und Gruppen
- Zuweisen des temporären Zugriffs auf Azure-Ressourcen für kurze Aufgaben oder Bereitschaftszeitpläne
- Empfangen von Benachrichtigungen, wenn neuen Benutzern oder Gruppen Ressourcenzugriff zugewiesen wird und sie geeignete Zuweisungen aktivieren

Weitere Informationen finden Sie in der [Übersicht über die rollenbasierte Zugriffssteuerung in Azure PIM](privileged-identity-management/azure-pim-resource-rbac.md).