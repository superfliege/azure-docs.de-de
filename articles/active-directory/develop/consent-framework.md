---
title: Funktionsweise der Anwendungszustimmung | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Funktionsweise des Azure AD-Zustimmungsframeworks und dessen Verwendung beim Entwickeln von Anwendungen in Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 5d8d526e67a69959687fb79ac6406d8313a417a8
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723770"
---
# <a name="how-application-consent-works"></a>Funktionsweise der Anwendungszustimmung

In diesem Artikel erfahren Sie, wie das Azure AD-Zustimmungsframework funktioniert, das Sie für eine effizientere Entwicklung von Anwendungen verwenden können.

## <a name="recommended-documents"></a>Empfohlene Dokumente

- Eine allgemeine Übersicht über die [Steuerung des Anwendungszugriffs auf Ressourcen durch einen Ressourcenbesitzer mittels Zustimmung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#consent)
- Eine ausführliche Übersicht über die [Zustimmungsimplementierung mit dem Azure AD-Zustimmungsframework](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework)
- Ausführlichere Informationen zur [Verwendung des Zustimmungsframeworks durch mehrinstanzenfähige Anwendungen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) zur Implementierung von Benutzer- und Administratorzustimmung mit Unterstützung für erweiterte Anwendungsmuster mit mehreren Ebenen
- Ausführlichere Informationen zur [OAuth 2.0-Protokollunterstützung für Zustimmung während des Flows zum Gewähren des Autorisierungscodes](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)

## <a name="next-steps"></a>Nächste Schritte
[Azure AD bei Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
