---
title: Verwalten von Verwaltungseinheiten in Azure Active Directory (Vorschau)
description: Verwenden von Verwaltungseinheiten zum präziseren Delegieren von Berechtigungen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.component: users-groups-roles
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 373983413ea83e5924b52fbbcbc666aa1a188d80
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Verwaltung von Verwaltungseinheiten in Azure AD: Öffentliche Vorschau
Dieser Artikel beschreibt Verwaltungseinheiten – einen neuen Azure Active Directory-Container mit Ressourcen, die zum Delegieren administrativer Berechtigungen und Anwenden von Richtlinien für bestimmte Benutzergruppen verwendet werden können. In Azure Active Directory können Administratoren mit Verwaltungseinheiten Berechtigungen zu regionalen Administratoren delegieren oder eine Richtlinie auf präziser Ebene festlegen.

Dies ist hilfreich in Organisationen mit unabhängigen Bereichen, z.B. eine große Universität, die aus vielen autonomen Fakultäten besteht (eine Wirtschaftsfakultät, eine Ingenieursfakultät, usw.), die unabhängig voneinander sind. Solche Bereiche haben ihre eigenen IT-Administratoren, die den Zugriff kontrollieren, Benutzer verwalten und Richtlinien festlegen, die speziell für die jeweilige Abteilung gelten. Zentrale Administratoren wollen in der Lage sein, den Administratoren der einzelnen Bereiche Berechtigungen für die Benutzer in ihren Bereichen zu erteilen. Genauer gesagt kann ausgehend von diesem Beispiel ein zentraler Administrator z. B. eine Verwaltungseinheit für eine bestimmten Fakultät (Wirtschaftsfakultät) erstellen und sie nur mit den Benutzern der Wirtschaftsfakultät befüllen. Dann kann ein zentraler Administrator den IT-Mitarbeitern der Wirtschaftsfakultät eine bereichsbezogene Rolle hinzufügen. Oder anders ausgedrückt, er kann den IT-Mitarbeitern der Wirtschaftsfakultät Berechtigungen nur für die Verwaltungseinheit „Wirtschaftsfakultät“ gewähren.

> [!IMPORTANT]
> Für die Verwendung von Verwaltungseinheiten muss der Administrator, der für eine solche Einheit zuständig ist, über eine Azure Active Directory Premium-Lizenz verfügen. Alle anderen Benutzer in der Verwaltungseinheit müssen über Azure Active Directory Basic-Lizenzen verfügen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](active-directory-get-started-premium.md).
>


Aus Sicht des zentralen Administrators ist eine Verwaltungseinheit ein Verzeichnisobjekt, das erstellt und mit Ressourcen aufgefüllt werden kann. **In dieser Vorschauversion können diese Ressourcen nur Benutzer sein.** Sobald erstellt und aufgefüllt, kann die Verwaltungseinheit zum Einschränken der erteilten Berechtigung nur für Ressourcen in der Verwaltungseinheit verwendet werden.

## <a name="managing-administrative-units"></a>Verwalten von Verwaltungseinheiten
In dieser Vorschauversion können Sie Verwaltungseinheiten mit dem Azure Active Directory-Modul für Windows PowerShell-Cmdlets erstellen und verwalten. Weitere Informationen hierzu finden Sie unter [Verwenden von Verwaltungseinheiten](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0).

Weitere Informationen zu Softwareanforderungen und der Installation des Azure AD-Moduls sowie Informationen zu den Azure AD-Modul-Cmdlets zur Verwaltung von Verwaltungseinheiten, einschließlich Syntax, Beschreibung der Parameter und Beispielen, finden Sie unter [Working with Administrative Units](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0) (Verwenden von Verwaltungseinheiten).

## <a name="next-steps"></a>Nächste Schritte
[Azure Active Directory-Editionen](active-directory-editions.md)
