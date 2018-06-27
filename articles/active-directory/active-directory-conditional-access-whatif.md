---
title: Was ist das Was-wäre-wenn-Tool für den bedingten Azure Active Directory-Zugriff? – Vorschauversion | Microsoft-Dokumentation
description: Erfahren Sie, welche Auswirkungen die Richtlinien für bedingten Zugriff auf Ihre Umgebung haben.
services: active-directory
keywords: bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 661ada8de8821d489732e1f36dc2406eaa0ee4a7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231946"
---
# <a name="what-is-the-what-if-tool-in-azure-active-directory-conditional-access---preview"></a>Was ist das Was-wäre-wenn-Tool für den bedingten Azure Active Directory-Zugriff? – Vorschauversion

Der [bedingte Zugriff](active-directory-conditional-access-azure-portal.md) ist eine Funktion von Azure Active Directory (Azure AD), mit der Sie den Zugriff autorisierter Benutzer auf Ihre Cloud-Apps steuern können. Wie wissen Sie, was Sie von den Richtlinien für bedingten Zugriff in Ihrer Umgebung erwarten können? Um diese Frage zu beantworten, können Sie das **Was-wäre-wenn-Tool für den bedingten Zugriff** verwenden.

In diesem Artikel wird erläutert, wie Sie dieses Tool zum Testen Ihrer Richtlinien für den bedingten Zugriff verwenden können.

## <a name="what-it-is"></a>Funktionsbeschreibung

Mit dem **Was-wäre-wenn-Tool für den bedingten Zugriff** erhalten Sie Einblicke in die Auswirkungen Ihrer Richtlinien für den bedingten Zugriff auf Ihre Umgebung. Anstelle von Tests Ihrer Richtlinien mithilfe mehrerer manueller Anmeldevorgänge ermöglicht dieses Tool die Auswertung simulierter Benutzeranmeldungen. Die Simulation schätzt die Auswirkungen dieser Anmeldungen auf Ihre Richtlinien ab und generiert einen Simulationsbericht. Der Bericht listet nicht nur die angewendeten Richtlinien für den bedingten Zugriff auf, sondern auch [klassische Richtlinien](active-directory-conditional-access-migration.md#classic-policies) (sofern vorhanden).    

Das Was-wäre-wenn-Tool stellt auch eine Möglichkeit dar, die Richtlinien zu ermitteln, die für einen bestimmten Benutzer gelten. Sie können diese Informationen z.B. nutzen, um ein Problem zu beheben.  

## <a name="how-it-works"></a>So funktioniert's

Sie müssen im **Was-wäre-wenn-Tool für den bedingten Zugriff** zunächst die Einstellungen des Anmeldeszenarios konfigurieren, das Sie simulieren möchten. Dies umfasst Folgendes:

- Den Benutzer, den Sie testen möchten 

- Die Cloud-Apps, auf die der Benutzer zuzugreifen versucht

- Die Bedingungen, unter denen der Zugriff auf die konfigurierten Cloud-Apps erfolgt
     
Im nächsten Schritt können Sie eine Simulation initiieren, die Ihre Einstellungen auswertet. Bei der Auswertung werden nur die aktivierten Richtlinien berücksichtigt.


Wenn die Auswertung abgeschlossen ist, generiert das Tool einen Bericht über die betroffenen Richtlinien.


## <a name="running-the-tool"></a>Ausführen des Tools

Sie finden das **Was-wäre-wenn-Tool** im Azure-Portal auf der Seite **[Bedingter Zugriff – Richtlinien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)**.

Zum Starten des Tools klicken Sie auf der Symbolleiste über der Liste der Richtlinien auf **Was-wäre-wenn**.

![Was-wäre-wenn](./media/active-directory-conditional-access-whatif/01.png)

Bevor Sie eine Auswertung durchführen können, müssen Sie die Einstellungen konfigurieren.

## <a name="settings"></a>Einstellungen

Dieser Abschnitt enthält Informationen zu den Einstellungen für die Simulation.

![Was-wäre-wenn](./media/active-directory-conditional-access-whatif/02.png)


### <a name="user"></a>Benutzer

Sie können nur einen Benutzer auswählen. Dies ist das einzige erforderliche Feld.

### <a name="cloud-apps"></a>Cloud-Apps

Der Standardwert für diese Einstellung ist **Alle Cloud-Apps**. Bei der Standardeinstellung wird eine Auswertung aller verfügbaren Richtlinien in Ihrer Umgebung durchgeführt. Sie können den Bereich auf Richtlinien begrenzen, die Auswirkungen auf bestimmte Cloud-Apps haben.


### <a name="ip-address"></a>IP-Adresse

Die IP-Adresse ist eine einzelne IPv4-Adresse zum Imitieren der [Standortbedingung](active-directory-conditional-access-locations.md). Die Adresse stellt die Adresse des Geräts mit Internetzugang dar, die der Benutzer für die Anmeldung verwendet. Sie können die IP-Adresse eines Geräts überprüfen, indem Sie beispielsweise zu [WhatIsMyIPAddress.com](https://whatismyipaddress.com) navigieren.    

### <a name="device-platforms"></a>Geräteplattformen

Diese Einstellung imitiert die [Geräteplattformbedingung](active-directory-conditional-access-conditions.md#device-platforms) und stellt die Entsprechung von **Alle Plattformen (auch nicht unterstützte)** dar. 
### <a name="client-apps"></a>Client-Apps

Diese Einstellung imitiert die [Client-Apps-Bedingung](active-directory-conditional-access-conditions.md#client-apps).
Standardmäßig bewirkt diese Einstellung eine Auswertung aller Richtlinien, bei denen **Browser** und/oder **Mobile Apps und Desktopclients** aktiviert ist. Damit werden außerdem Richtlinien erkannt, die **Exchange ActiveSync (EAS)** erzwingen. Sie können diese Einstellung eingrenzen, indem Sie Folgendes auswählen:

- **Browser**, um alle Richtlinien auszuwerten, für die mindestens **Browser** ausgewählt wurde. 

- **Mobile Apps und Desktopclients**, um alle Richtlinien auszuwerten, für die mindestens **Mobile Apps und Desktopclients** ausgewählt wurde. 


### <a name="sign-in-risk"></a>Anmelderisiko

Diese Einstellung imitiert die [Anmelderisikobedingung](active-directory-conditional-access-conditions.md#sign-in-risk).   


## <a name="evaluation"></a>Auswertung 

Sie starten eine Auswertung, indem Sie auf **Was-wäre-wenn** klicken. Das Auswertungsergebnis wird Ihnen in Form eines Berichts präsentiert, der Folgendes enthält: 

![Was-wäre-wenn](./media/active-directory-conditional-access-whatif/03.png)

- Einen Indikator, ob klassische Richtlinien in Ihrer Umgebung vorhanden sind
- Richtlinien, die für den Benutzer gelten
- Richtlinien, die für den Benutzer nicht gelten


Wenn [klassische Richtlinien](active-directory-conditional-access-migration.md#classic-policies) für die ausgewählten Cloud-Apps vorhanden ist, wird ein Indikator angezeigt. Durch Klicken auf den Indikator werden Sie zur Seite „Klassische Richtlinien“ umgeleitet. Auf der Seite „Klassische Richtlinien“ können Sie eine klassische Richtlinie migrieren oder einfach deaktivieren. Schließen Sie diese Seite, um zu den Auswertungsergebnissen zurückzukehren.

In der Liste der Richtlinien, die für den ausgewählten Benutzer gelten, finden Sie auch eine Liste der [Steuerelemente zur Rechteerteilung](active-directory-conditional-access-controls.md#grant-controls) und [Sitzungssteuerelemente](active-directory-conditional-access-controls.md#session-controls), die der Benutzer erfüllen muss.

In der Liste der Richtlinien, die nicht für den Benutzer gelten, finden Sie auch die Gründe dafür, warum diese Richtlinien nicht gelten. Für jede der aufgeführten Richtlinien entspricht der Grund der ersten Bedingung, die nicht erfüllt wurde. Falls eine Richtlinie nicht angewendet wird, könnte sie deaktiviert worden sein, da sie nicht weiter auswertet wird.   



## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](active-directory-conditional-access-app-based-mfa.md).

- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, lesen Sie unter [Best Practices für den bedingten Zugriff in Azure Active Directory](active-directory-conditional-access-best-practices.md) nach. 

- Wenn Sie klassische Richtlinien migrieren möchten, lesen Sie unter [Migrieren klassischer Richtlinien in das Azure-Portal](active-directory-conditional-access-migration.md) nach.  
