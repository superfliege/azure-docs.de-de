---
title: Konfigurieren von in Azure Active Directory eingebundenen Hybridgeräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte konfigurieren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 27068c1154a4cc5776bbcc74922ca31c4f28ced6
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399916"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Planen der Implementierung einer Azure Active Directory-Hybrideinbindung

Ähnlich wie ein Benutzer wird ein Gerät zu einer weiteren Identität, die Sie schützen sowie zum Schutz Ihrer Ressourcen jederzeit und überall verwenden können. Dafür können Sie die Identitäten Ihrer Geräte mit einer der folgenden Methoden in Azure AD bereitstellen:

- Azure AD-Einbindung
- Azure AD-Hybrideinbindung
- Azure AD-Registrierung

Durch das Bereitstellten Ihrer Geräte in Azure AD maximieren Sie die Produktivität Ihrer Benutzer durch einmaliges Anmelden (SSO) für Ihre gesamten Cloud- und lokalen Ressourcen. Gleichzeitig können Sie den Zugriff auf Ihre Cloud- und lokalen Ressourcen durch den [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) sichern.

Wenn Sie in einer lokalen Active Directory-Umgebung Ihre in die Domäne eingebundenen Geräte in Azure AD einbinden möchten, kann dies durch Konfigurieren von in Azure AD eingebundenen Hybridgeräten erfolgen. Dieser Artikel enthält eine Anleitung zum Implementieren einer Azure AD-Hybrideinbindung in Ihre Umgebung. 


## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel setzt voraus, dass Sie die [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md) gelesen haben.


## <a name="plan-your-implementation"></a>Planen Ihrer Implementierung

Um Ihre Azure AD-Hybridimplementierung zu planen, sollten Sie sich mit folgenden Themen vertraut machen:

|   |   |
|---|---|
|![Prüfen][1]|Überprüfung unterstützter Geräte|
|![Prüfen][1]|Überprüfung wichtiger Informationen|
|![Prüfen][1]|Auswahl Ihres Szenarios|


 


## <a name="review-supported-devices"></a>Überprüfung unterstützter Geräte 

Die Azure AD-Hybrideinbindung unterstützt zahlreiche Windows-Geräte. Da die Konfiguration für Geräte mit älteren Versionen von Windows zusätzliche oder unterschiedliche Schritte erfordert, werden die unterstützten Geräte in zwei Kategorien eingeteilt:

**Aktuelle Windows-Geräte**

- Windows 10
    
- Windows Server 2016


Für Geräte, auf denen das Windows-Desktopbetriebssystem ausgeführt wird, wird die Version Windows 10 Anniversary Update (Version 1607) oder höher unterstützt. Es wird empfohlen, ein Upgrade auf die aktuelle Version von Windows 10 durchzuführen.



 **Kompatible Windows-Geräte**

- Windows 8.1
 
- Windows 7

- Windows Server 2012 R2
 
- Windows Server 2012 
 
- Windows Server 2008 R2 


Als ersten Planungsschritt sollten Sie Ihre Umgebung überprüfen und ermitteln, ob Sie Unterstützung für kompatible Windows-Geräte benötigen.



## <a name="review-things-you-should-know"></a>Überprüfung wichtiger Informationen

Sie können keine Azure AD-Hybrideinbindung verwenden, wenn Ihre Umgebung aus einer einzelnen Gesamtstruktur besteht, die Identitätsdaten mit mehr als einem Azure AD-Mandanten synchronisiert.

Wenn Sie das Systemvorbereitungstool Sysprep verwenden, stellen Sie sicher, dass Sie Images von einer Installation von Windows erstellen, die nicht für eine Azure AD-Hybrideinbindung konfiguriert wurde.

Wenn Sie zusätzliche VMs mit einer Momentaufnahme des virtuellen Computers erstellen, stellen Sie sicher, dass Sie eine Momentaufnahme verwenden, die nicht für die Azure AD-Hybrideinbindung konfiguriert wurde.

Für Geräte, die für das Roaming von Benutzerprofilen oder das Roaming von Anmeldeinformationen konfiguriert wurden, wird die Registrierung kompatibler Windows-Geräte nicht unterstützt. Verwenden Sie Windows 10, wenn Sie das Roaming von Profilen oder Einstellungen nutzen.

Die Registrierung von Windows Server mit der Domänencontrollerrolle wird nicht unterstützt.

Wenn Ihre Organisation Internetzugriff über einen authentifizierten ausgehenden Proxy erfordert, müssen Sie sicherstellen, dass Ihre Windows 10-Computer erfolgreich beim ausgehenden Proxy authentifiziert werden können. Da Windows 10-Computer die Geräteregistrierung mithilfe von Computerkontext ausführen, muss die Authentifizierung bei ausgehenden Proxys mit dem Computerkontext konfiguriert werden.


Die Azure AD-Hybrideinbindung ist ein Prozess für die automatische Registrierung Ihrer lokalen in die Domäne eingebundenen Geräte bei Azure AD. Es kann auch vorkommen, dass Sie nicht alle Ihre Geräte automatisch registrieren möchten. In diesem Fall finden Sie weitere Informationen unter [Steuern der Azure AD-Hybrideinbindung für Ihre Geräte](hybrid-azuread-join-control.md).



## <a name="select-your-scenario"></a>Auswahl Ihres Szenarios

Sie können Azure AD-Hybrideinbindungen für die folgenden Szenarien konfigurieren:

- Verwaltete Domänen
- Verbunddomänen  



Wenn Ihre Umgebung verwaltete Domänen umfasst, unterstützt die Azure AD-Hybrideinbindung:

- Passthroughauthentifizierung mit nahtlosem einmaligen Anmelden 

- Kennworthashsynchronisierung mit nahtlosem einmaligen Anmelden 

Ab Version 1.1.819.0 bietet Azure AD Connect einen Assistenten für die Konfiguration der Azure AD-Hybrideinbindung. Mit dem Assistenten können Sie den Konfigurationsprozess erheblich vereinfachen. Weitere Informationen finden Sie unter

- [Konfigurieren der Azure Active Directory-Hybrideinbindung für Verbunddomänen](hybrid-azuread-join-federated-domains.md)

- [Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen](hybrid-azuread-join-managed-domains.md)


 Wenn das Installieren der erforderlichen Version von Azure AD Connect keine Option für Sie ist, informieren Sie sich über die [manuelle Konfiguration der Geräteregistrierung](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren der Azure Active Directory-Hybrideinbindung für Verbunddomänen](hybrid-azuread-join-federated-domains.md)
> [Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
