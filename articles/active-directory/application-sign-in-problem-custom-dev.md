---
title: Probleme bei der Anmeldung bei benutzerdefiniert entwickelten Anwendungen | Microsoft-Dokumentation
description: Allgemeine Fehler, die dazu führen können, dass Sie sich nicht bei einer mit Azure AD entwickelten Anwendung anmelden können
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
ms.reviewer: asteen
ms.openlocfilehash: 68ab777efee3af67cfcdddf1744a36aca0f09571
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366701"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Probleme bei der Anmeldung bei benutzerdefiniert entwickelten Anwendungen

Es gibt mehrere Fehler, die dazu führen können, dass Sie sich nicht einer App anmelden können. Der Hauptgrund für das Auftreten dieses Problems sind falsch konfigurierte Apps.

## <a name="errors-related-to--misconfigured-apps"></a>Fehler im Zusammenhang mit falsch konfigurierten Apps

* Stellen Sie sicher, dass die Konfigurationen im Portal mit der App übereinstimmen. Vergleichen Sie insbesondere Client-/Anwendungs-ID, Antwort-URLs, Clientgeheimnisse/-schlüssel und App-ID-URI.

* Vergleichen Sie die Ressource, auf die Sie zugreifen möchten, im Code mit den konfigurierten Berechtigungen auf der Registerkarte **Erforderliche Ressourcen**, um sicherzustellen, dass Sie nur Ressourcen anfordern, die Sie konfiguriert haben.

* Weitere Informationen zu ähnlichen Fehlern oder Problemen finden Sie unter [Azure AD bei Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory).

## <a name="next-steps"></a>Nächste Schritte

[Entwicklerhandbuch zu Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Genehmigen und Integrieren von Apps in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Genehmigen und Zuweisen von Berechtigungen für konvergierte Azure AD V2.0-Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD bei Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
