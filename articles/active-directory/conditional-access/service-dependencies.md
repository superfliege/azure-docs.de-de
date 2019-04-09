---
title: Was sind Dienstabhängigkeiten beim bedingten Azure Active Directory-Zugriff? | Microsoft-Dokumentation
description: Es wird beschrieben, wie Bedingungen beim bedingten Zugriff in Azure Active Directory zum Auslösen einer Richtlinie verwendet werden.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: f727fc7133ebc9ee124e63253e8a266862b0d908
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522029"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Was sind Dienstabhängigkeiten beim bedingten Azure Active Directory-Zugriff? 


Mit Richtlinien für bedingten Zugriff können Sie Zugriffsanforderungen für Websites und Dienste angeben. Ihre Zugriffsanforderungen können beispielsweise die Multi-Factor Authentication (MFA) oder [verwaltete Geräte](require-managed-devices.md) umfassen. 


Wenn Sie direkt auf eine Site oder einen Dienst zugreifen, kann die Auswirkung einer zugehörigen Richtlinie normalerweise leicht bewertet werden. Falls Sie beispielsweise über eine Richtlinie verfügen, für die eine MFA-Konfiguration für SharePoint Online benötigt wird, wird MFA für jede Anmeldung am SharePoint-Webportal erzwungen. Es ist aber nicht immer einfach, die Auswirkung einer Richtlinie zu bewerten, weil Cloud-Apps mit Abhängigkeiten von anderen Cloud-Apps vorhanden sind. Beispielsweise wird SharePoint Online von Microsoft Teams genutzt. Wenn Sie in Ihrem aktuellen Szenario also auf Microsoft Teams zugreifen, unterliegen Sie auch der SharePoint-MFA-Richtlinie.   


## <a name="policy-enforcement"></a>Durchsetzung von Richtlinien 

Wenn Sie eine Dienstabhängigkeit konfiguriert haben, kann die Richtlinie per früh oder spät gebundener Erzwingung angewendet werden. 

**Früh gebundene Richtlinienerzwingung** bedeutet, dass ein Benutzer die abhängige Dienstrichtlinie erfüllen muss, bevor er auf die aufrufende App zugreift. Ein Benutzer muss beispielsweise die SharePoint-Richtlinie erfüllen, bevor er sich an MS Teams anmeldet. 

Die **spät gebundene Richtlinienerzwingung** erfolgt, nachdem sich der Benutzer an der aufrufenden App angemeldet hat. Die Erzwingung wird zurückgestellt, wenn App-Anforderungen (Token für den Downstreamdienst) aufgerufen werden. Beispiele hierfür sind der Zugriff auf Planner durch MS Teams und auf SharePoint durch Office.com. 

In der Abbildung unten sind MS Teams-Dienstabhängigkeiten dargestellt. Durchgehende Pfeile weisen auf eine früh gebundene Erzwingung hin, während der gestrichelte Pfeil für Planner für eine spät gebundene Erzwingung steht. 



![MS Teams-Dienstabhängigkeiten](./media/service-dependencies/01.png)



  

Die bewährte Methode besteht darin, für alle zugehörigen Apps und Dienste nach Möglichkeit immer gemeinsame Richtlinien festzulegen. Ein einheitlicher Sicherheitsstatus sorgt für die bestmögliche Benutzererfahrung. Wenn Sie beispielsweise eine gemeinsame Richtlinie für Exchange Online, SharePoint Online, MS Teams und Skype for Business festlegen, wird die Zahl von unerwarteten Aufforderungen reduziert, die ggf. angezeigt werden, weil unterschiedliche Richtlinien auf Downstreamdienste angewendet werden. 

In der Tabelle unten sind die zusätzlichen Dienstabhängigkeiten aufgeführt, die von den Client-Apps erfüllt werden müssen.  

| Client-Apps         | Downstreamdienst                          | Erzwingung |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure-Verwaltung (Portal und API) | Früh gebunden |
| Microsoft Classroom | Exchange                                    | Früh gebunden |
|                     | SharePoint                                  | Früh gebunden  |
| Microsoft Teams     | Exchange                                    | Früh gebunden |
|                     | MS Planner                                  | Spät gebunden  |
|                     | SharePoint                                  | Früh gebunden |
|                     | Skype for Business Online                   | Früh gebunden |
| Office-Portal       | Exchange                                    | Spät gebunden  |
|                     | SharePoint                                  | Spät gebunden  |
| Outlook-Gruppen      | Exchange                                    | Früh gebunden |
|                     | SharePoint                                  | Früh gebunden |
| PowerApps           | Microsoft Azure-Verwaltung (Portal und API) | Früh gebunden |
|                     | Microsoft Azure Active Directory              | Früh gebunden |
| Project             | Dynamics CRM                                | Früh gebunden |
| Skype for Business  | Exchange                                    | Früh gebunden |
| Visual Studio       | Microsoft Azure-Verwaltung (Portal und API) | Früh gebunden |



## <a name="next-steps"></a>Nächste Schritte

Unter [Anleitung: Planen der Bereitstellung von bedingtem Zugriff in Azure Active Directory](plan-conditional-access.md) erfahren Sie, wie Sie bedingten Zugriff in Ihrer Umgebung implementieren.
