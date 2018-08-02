---
title: Funktionsweise der Anwendungszustimmung | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Funktionsweise des Azure AD-Zustimmungsframeworks und dessen Verwendung beim Entwickeln von Anwendungen in Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 91378f4816db773aebd038bd9f176596a4f2c316
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366926"
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
