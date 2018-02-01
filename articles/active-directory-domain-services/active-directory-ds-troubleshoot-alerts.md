---
title: 'Azure Active Directory Domain Services: Problembehandlung von Warnungen| Microsoft-Dokumentation'
description: "Problembehandlung von Warnungen für die Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: ergreenl
ms.openlocfilehash: b2e0edf3588f3b1db5f4b6641019be1ded9cb50e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services – Problembehandlung von Warnungen
Dieser Artikel enthält Leitfäden für die Problembehandlung aller Warnungen, denen Sie in Ihrer verwalteten Domäne begegnen können.


Wählen Sie die Schritte zur Problembehandlung aus, die der ID der aufgetretenen Warnung oder Meldung entspricht.

| **Warnungs-ID** | **Warnmeldung** | **Lösung** |
| --- | --- | :--- |
| AADDS001 | *Für die verwaltete Domäne ist Secure LDAP über Internet aktiviert. Der Zugriff auf Port 636 ist jedoch nicht mithilfe einer Netzwerksicherheitsgruppe gesperrt. Dies kann Brute-Force-Kennwortangriffe auf Benutzerkonten in der verwalteten Domäne möglich machen.* | [Falsche sichere LDAP-Konfiguration](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Das Ihrer verwalteten Domäne zugeordnete Azure AD-Verzeichnis wurde möglicherweise gelöscht. Die verwaltete Domäne befindet sich nicht mehr in einer unterstützten Konfiguration. Microsoft kann Ihre verwaltete Domäne nicht überwachen, verwalten, patchen und synchronisieren.* | [Fehlendes Verzeichnis](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services kann in einem Azure AD B2C-Verzeichnis nicht aktiviert werden.* | [Azure AD B2C wird in diesem Verzeichnis ausgeführt](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Ein für den ordnungsgemäßen Betrieb von Azure AD Domain Services erforderlicher Dienstprinzipal wurde aus Ihrem Azure AD-Verzeichnis gelöscht. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.* | [Fehlender Dienstprinzipal](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Der IP-Adressbereich für das virtuelle Netzwerk, in dem Sie Azure AD Domain Services aktiviert haben, liegt in einem öffentlichen IP-Bereich. Azure AD Domain Services müssen in einem virtuellen Netzwerk mit einem privaten IP-Adressbereich aktiviert werden. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.* | [Adresse befindet sich in einem öffentlichen IP-Adressbereich](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft kann nicht auf die Domänencontroller für diese verwaltete Domäne zugreifen. Dies kann geschehen, wenn eine für Ihr virtuelles Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) den Zugriff auf ihre verwaltete Domäne blockiert. Eine weitere mögliche Ursache besteht in einer benutzerdefinierten Route, die eingehenden Datenverkehr aus dem Internet blockiert.* | [Netzwerkfehler](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: Fehlendes Verzeichnis
**Warnmeldung:**

*Das Ihrer verwalteten Domäne zugeordnete Azure AD-Verzeichnis wurde möglicherweise gelöscht. Die verwaltete Domäne befindet sich nicht mehr in einer unterstützten Konfiguration. Microsoft kann Ihre verwaltete Domäne nicht überwachen, verwalten, patchen und synchronisieren.*

**Problembehandlung**:

Dieser Fehler wird normalerweise durch das nicht ordnungsgemäße Verschieben Ihres Azure-Abonnements in ein neues Azure AD-Verzeichnis und das anschließende Löschen des alten Azure AD-Verzeichnisses verursacht, das immer noch Azure AD Domain Services zugeordnet ist.

Dieser Fehler ist nicht behebbar. Um das Problem zu lösen, müssen Sie [Ihre vorhandene verwaltete Domäne löschen](active-directory-ds-disable-aadds.md) und sie in Ihrem neuen Verzeichnis neu erstellen. Wenn beim Löschen Probleme auftreten, bitten Sie das Produktteam für Azure Active Directory Domain Services um [Unterstützung](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C wird in diesem Verzeichnis ausgeführt
**Warnmeldung:**

*Azure AD Domain Services kann in einem Azure AD B2C-Verzeichnis nicht aktiviert werden.*

**Problembehandlung**:

>[!NOTE]
>Um Azure AD Domain Services weiterhin zu verwenden, müssen Sie Ihre Instanz von Azure AD Domain Services in einem anderen als einem Azure AD B2C-Verzeichnis neu erstellen.

Führen Sie die folgenden Schritte aus, um Ihren Dienst wiederherzustellen:

1. [Löschen Sie Ihre verwaltete Domäne](active-directory-ds-disable-aadds.md) aus Ihrem vorhandenen Azure AD-Verzeichnis.
2. Erstellen Sie ein neues Verzeichnis, das kein Azure AD B2C-Verzeichnis ist.
3. Folgen Sie den Anweisungen im Leitfaden [Erste Schritte](active-directory-ds-getting-started.md), um eine verwaltete Domäne neu zu erstellen.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adresse befindet sich in einem öffentlichen IP-Adressbereich

**Warnmeldung:**

*Der IP-Adressbereich für das virtuelle Netzwerk, in dem Sie Azure AD Domain Services aktiviert haben, liegt in einem öffentlichen IP-Bereich. Azure AD Domain Services müssen in einem virtuellen Netzwerk mit einem privaten IP-Adressbereich aktiviert werden. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.*

**Problembehandlung**:

> [!NOTE]
> Zum Beheben dieses Problems müssen Sie Ihre vorhandene verwaltete Domäne löschen und sie in einem virtuellen Netzwerk mit einem privaten IP-Adressbereich neu erstellen. Dieser Vorgang geht mit einer Dienstunterbrechung einher.

Lesen Sie den Abschnitt **privater IP-v4-Adressbereich** in [diesem Artikel](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces), bevor Sie beginnen.

1. [Löschen Sie Ihre verwaltete Domäne](active-directory-ds-disable-aadds.md) aus Ihrem Verzeichnis.
2. Korrigieren Sie den IP-Adressbereich für das Subnetz
  1. Navigieren Sie zur [Seite „Virtuelle Netzwerke“ im Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Wählen Sie das virtuelle Netzwerk aus, das Sie für Azure AD Domain Services verwenden möchten.
  3. Klicken Sie unter „Einstellungen“ auf **Adressbereich**.
  4. Aktualisieren Sie den Adressbereich, indem Sie auf den vorhandenen Adressbereich klicken und ihn bearbeiten oder einen zusätzlichen Adressbereich hinzufügen. Achten Sie dabei darauf, dass sich der neue Adressbereich in einem privaten IP-Bereich befindet. Speichern Sie die Änderungen.
  5. Klicken Sie in linken Navigationsbereich auf **Subnetze**.
  6. Klicken Sie in der Tabelle auf das Subnetz, das Sie bearbeiten möchten.
  7. Aktualisieren Sie den Adressbereich, und speichern Sie Ihre Änderungen.
3. Befolgen Sie die Anweisungen im Handbuch [Erste Schritte mit Azure Active Directory Domain Services](active-directory-ds-getting-started.md), um Ihre verwaltete Domäne neu zu erstellen. Achten Sie darauf, ein virtuelles Netzwerk mit einem privaten IP-Adressbereich auszuwählen.
4. Um den Beitritt Ihrer virtuellen Computer zu Ihrer neuen Domäne zu realisieren, befolgen Sie die Anweisungen in [diesem Handbuch](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Überprüfen Sie in zwei Stunden die Integrität Ihrer Domäne, um sicherzustellen, dass Sie die Schritte ordnungsgemäß abgeschlossen haben.


## <a name="contact-us"></a>Kontakt
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).
