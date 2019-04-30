---
title: Planen der Implementierung einer Azure Active Directory-Hybrideinbindung in Azure Active Directory (Azure AD) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure Active Directory eingebundene Hybridgeräte konfigurieren.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8827a51a23b2ea274d8096a154e630c9cecbba7c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59489517"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Anleitung: Planen der Implementierung einer Azure Active Directory-Hybrideinbindung

Ähnlich wie ein Benutzer wird ein Gerät zu einer weiteren Identität, die Sie schützen sowie zum Schutz Ihrer Ressourcen jederzeit und überall verwenden können. Dafür können Sie die Identitäten Ihrer Geräte mit einer der folgenden Methoden in Azure AD bereitstellen:

- Azure AD-Einbindung
- Azure AD-Hybrideinbindung
- Azure AD-Registrierung

Durch das Bereitstellten Ihrer Geräte in Azure AD maximieren Sie die Produktivität Ihrer Benutzer durch einmaliges Anmelden (SSO) für Ihre gesamten Cloud- und lokalen Ressourcen. Gleichzeitig können Sie den Zugriff auf Ihre Cloud- und lokalen Ressourcen durch den [bedingten Zugriff](../active-directory-conditional-access-azure-portal.md) sichern.

Wenn Sie in einer lokalen Active Directory-Umgebung Ihre in die Domäne eingebundenen Geräte in Azure AD einbinden möchten, kann dies durch Konfigurieren von in Azure AD eingebundenen Hybridgeräten erfolgen. Dieser Artikel enthält eine Anleitung zum Implementieren einer Azure AD-Hybrideinbindung in Ihre Umgebung. 

## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel setzt voraus, dass Sie die [Einführung in die Geräteverwaltung in Azure Active Directory](../device-management-introduction.md) gelesen haben.

> [!NOTE]
> Die mindestens erforderliche Domänen- und Gesamtstruktur-Funktionsebene für Hybrid-Azure AD-Einbindung in Windows 10 ist Windows Server 2008 R2. Bei niedrigeren Versionen erhält der Benutzer wegen LSA-Problemen möglicherweise kein primäres Aktualisierungstoken während der Windows-Anmeldung.

## <a name="plan-your-implementation"></a>Planen Ihrer Implementierung

Um Ihre Azure AD-Hybridimplementierung zu planen, sollten Sie sich mit folgenden Themen vertraut machen:

|   |   |
| --- | --- |
| ![Prüfen][1] | Überprüfung unterstützter Geräte |
| ![Prüfen][1] | Überprüfung wichtiger Informationen |
| ![Prüfen][1] | Steuern von Azure AD Hybrid Join für Ihre Geräte |
| ![Prüfen][1] | Auswahl Ihres Szenarios |

## <a name="review-supported-devices"></a>Überprüfung unterstützter Geräte

Die Azure AD-Hybrideinbindung unterstützt zahlreiche Windows-Geräte. Da die Konfiguration für Geräte mit älteren Versionen von Windows zusätzliche oder unterschiedliche Schritte erfordert, werden die unterstützten Geräte in zwei Kategorien eingeteilt:

### <a name="windows-current-devices"></a>Aktuelle Windows-Geräte

- Windows 10
- Windows Server 2016
- Windows Server 2019

Für Geräte, auf denen das Windows-Desktopbetriebssystem ausgeführt wird, wird die Version Windows 10 Anniversary Update (Version 1607) oder höher unterstützt. Es wird empfohlen, ein Upgrade auf die aktuelle Version von Windows 10 durchzuführen.

### <a name="windows-down-level-devices"></a>Kompatible Windows-Geräte

- Windows 8.1
- Windows 7
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

Als ersten Planungsschritt sollten Sie Ihre Umgebung überprüfen und ermitteln, ob Sie Unterstützung für kompatible Windows-Geräte benötigen.

## <a name="review-things-you-should-know"></a>Überprüfung wichtiger Informationen

Sie können keine Azure AD-Hybrideinbindung verwenden, wenn Ihre Umgebung aus einer einzelnen Gesamtstruktur besteht, die Identitätsdaten mit mehr als einem Azure AD-Mandanten synchronisiert.

Wenn Sie das Systemvorbereitungstool (Sysprep) verwenden, stellen Sie sicher, dass Images, die aus einer Installation von Windows 10 1803 oder früher erstellt wurden, nicht für Hybrid-Azure AD-Einbindung konfiguriert wurden.

Wenn Sie zusätzliche VMs mit einer Momentaufnahme des virtuellen Computers erstellen, stellen Sie sicher, dass Sie eine Momentaufnahme verwenden, die nicht für die Azure AD-Hybrideinbindung konfiguriert wurde.

Azure AD Hybrid Join für kompatible Windows-Geräte:

- **Wird** in nicht zu einem Verbund gehörenden Umgebungen durch das [nahtlose einmalige Anmelden von Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) unterstützt. 
- Wird bei Verwendung der Azure AD-Passthrough-Authentifizierung ohne nahtloses einmaliges Anmelden **nicht** unterstützt.
- Wird **nicht** unterstützt, wenn das Roaming von Anmeldeinformationen oder Benutzerprofilen aktiviert ist oder wenn eine virtuelle Desktopinfrastruktur (VDI) verwendet wird.

Die Registrierung von Windows Server mit der Domänencontrollerrolle wird nicht unterstützt.

Wenn Ihre Organisation Internetzugriff über einen authentifizierten ausgehenden Proxy erfordert, müssen Sie sicherstellen, dass Ihre Windows 10-Computer erfolgreich beim ausgehenden Proxy authentifiziert werden können. Da Windows 10-Computer die Geräteregistrierung mithilfe von Computerkontext ausführen, muss die Authentifizierung bei ausgehenden Proxys mit dem Computerkontext konfiguriert werden.

Die Azure AD-Hybrideinbindung ist ein Prozess für die automatische Registrierung Ihrer lokalen in die Domäne eingebundenen Geräte bei Azure AD. Es kann auch vorkommen, dass Sie nicht alle Ihre Geräte automatisch registrieren möchten. In diesem Fall finden Sie weitere Informationen unter [Steuern der Azure AD-Hybrideinbindung für Ihre Geräte](hybrid-azuread-join-control.md).

Wenn Ihre in die Windows 10-Domäne eingebundenen Geräte bereits für Ihren Mandanten bei [Azure AD registriert](https://docs.microsoft.com/azure/active-directory/devices/overview#azure-ad-registered-devices) sind, wird dringend empfohlen, diesen Status vor dem Aktivieren von Azure AD Hybrid Join zu entfernen. In Windows 10 Release 1809 wurden die folgenden Änderungen vorgenommen, um diesen Doppelstatus zu vermeiden:

- Eine etwaige Registrierung bei Azure AD würde nach der Azure AD-Hybrideinbindung des Geräts automatisch entfernt werden.
- Sie können verhindern, dass Ihr in die Domäne eingebundenes Gerät bei Azure AD registriert wird, indem Sie diesen Registrierungsschlüssel hinzufügen: HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin"=dword:00000001.
- Diese Änderung ist jetzt für Windows 10, Release 1803 mit KB4489894 verfügbar.

FIPS-kompatible TPMs werden für Hybrid-Azure AD-Einbindung nicht unterstützt. Wenn Ihre Geräte über FIPS-kompatible TPMs verfügen, müssen Sie sie vor dem Fortsetzen der Hybrid-Azure AD-Einbindung deaktivieren. Microsoft stellt keine Tools zum Deaktivieren des FIPS-Modus für TPMs bereit, da dieser vom TPM-Hersteller abhängig ist. Wenden Sie an Ihren Hardware-OEM, um Unterstützung zu erhalten.

## <a name="review-how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Steuern von Azure AD Hybrid Join für Ihre Geräte

Die Azure AD-Hybrideinbindung ist ein Prozess für die automatische Registrierung Ihrer lokalen in die Domäne eingebundenen Geräte bei Azure AD. In einigen Fällen sollten Sie nicht alle Ihre Geräte automatisch registrieren. Dies gilt beispielsweise während des anfänglichen Rollouts, um sicherzustellen, dass alles wie erwartet funktioniert.

Weitere Informationen finden Sie unter [Steuern von Azure AD Hybrid Join für Ihre Geräte](hybrid-azuread-join-control.md).

## <a name="select-your-scenario"></a>Auswahl Ihres Szenarios

Sie können Azure AD-Hybrideinbindungen für die folgenden Szenarien konfigurieren:

- Verwaltete Domänen
- Verbunddomänen  

Wenn Ihre Umgebung verwaltete Domänen umfasst, unterstützt die Azure AD-Hybrideinbindung:

- Passthrough-Authentifizierung (PTA)
- Kennworthashsynchronisierung (Password Hash Sync, PHS)

Ab Version 1.1.819.0 bietet Azure AD Connect einen Assistenten für die Konfiguration der Azure AD-Hybrideinbindung. Mit dem Assistenten können Sie den Konfigurationsprozess erheblich vereinfachen. Weitere Informationen finden Sie unter

- [Konfigurieren der Azure Active Directory-Hybrideinbindung für Verbunddomänen](hybrid-azuread-join-federated-domains.md)
- [Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen](hybrid-azuread-join-managed-domains.md)

 Wenn das Installieren der erforderlichen Version von Azure AD Connect keine Option für Sie ist, informieren Sie sich über die [manuelle Konfiguration der Geräteregistrierung](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual). 

## <a name="on-premises-ad-upn-support-in-hybrid-azure-ad-join"></a>Unterstützung lokaler AD-Benutzerprinzipalnamen (UPNs) in Azure AD Hybrid Join

In einigen Fällen können Ihre lokalen AD-UPNs von den Azure AD-UPNs abweichen. In diesen Fällen bietet Azure AD Hybrid Join unter Windows 10 auf Grundlage der [Authentifizierungsmethode](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), dem Domänentyp und der Windows 10-Version eingeschränkte Unterstützung für lokale AD-UPNs. Es gibt zwei Arten lokaler AD-UPNs, die in Ihrer Umgebung vorhanden sein können:

- Routingfähige Benutzerprinzipalnamen (UPNs): Ein routingfähiger UPN verfügt über eine gültige überprüfte Domäne, die bei einer Domänenregistrierungsstelle registriert ist. Ist beispielsweise „contoso.com“ die primäre Domäne in Azure AD, ist „contoso.org“ die primäre Domäne im lokalen Active Directory, die Contoso gehört und [in Azure AD überprüft wird](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain).
- Nicht routingfähige Benutzerprinzipalnamen (UPNs): Ein nicht routingfähiger UPN verfügt nicht über eine überprüfte Domäne. Sie ist nur in einem privaten Netzwerk Ihrer Organisation gültig. Ist beispielsweise „contoso.com“ die primäre Domäne in Azure AD, ist „contoso.local“ die primäre Domäne im lokalen Active Directory, aber keine überprüfbare Domäne im Internet und wird nur innerhalb des Contoso-Netzwerks verwendet.

Die folgende Tabelle enthält Details zur Unterstützung dieser lokalen AD UPNs in Azure AD Hybrid Join unter Windows 10

| Art der lokalen AD UPNs | Domänentyp | Windows 10-Version | BESCHREIBUNG |
| ----- | ----- | ----- | ----- |
| Routingfähig | Im Verbund | Version 1703 | Allgemein verfügbar |
| Routingfähig | Verwaltet | Version 1709 | Derzeit in der privaten Vorschau. Azure AD SSPR wird nicht unterstützt. |
| Nicht routingfähig | Im Verbund | Version 1803 | Allgemein verfügbar |
| Nicht routingfähig | Verwaltet | Nicht unterstützt | |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren der Azure Active Directory-Hybrideinbindung für Verbunddomänen](hybrid-azuread-join-federated-domains.md)
> [Konfigurieren der Azure Active Directory-Hybrideinbindung für verwaltete Domänen](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
