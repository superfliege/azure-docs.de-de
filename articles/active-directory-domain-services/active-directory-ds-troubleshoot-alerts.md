---
title: 'Azure Active Directory Domain Services: Problembehandlung von Warnungen| Microsoft-Dokumentation'
description: Problembehandlung von Warnungen für die Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 5a9f1bfee1df41d25309e84fe9958ff19a368943
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services – Problembehandlung von Warnungen
Dieser Artikel enthält Leitfäden für die Problembehandlung aller Warnungen, denen Sie in Ihrer verwalteten Domäne begegnen können.


Verwenden Sie die Problembehandlungsschritte für die ID oder Meldung in der Warnung.

| **Warnungs-ID** | **Warnmeldung** | **Lösung** |
| --- | --- | :--- |
| AADDS001 | *Für die verwaltete Domäne ist Secure LDAP über Internet aktiviert. Der Zugriff auf Port 636 ist jedoch nicht über eine Netzwerksicherheitsgruppe gesperrt. Dies kann Brute-Force-Kennwortangriffe auf Benutzerkonten in der verwalteten Domäne möglich machen.* | [Falsche sichere LDAP-Konfiguration](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Das Ihrer verwalteten Domäne zugeordnete Azure AD-Verzeichnis wurde möglicherweise gelöscht. Die verwaltete Domäne befindet sich nicht mehr in einer unterstützten Konfiguration. Microsoft kann Ihre verwaltete Domäne nicht überwachen, verwalten, patchen und synchronisieren.* | [Fehlendes Verzeichnis](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services kann in einem Azure AD B2C-Verzeichnis nicht aktiviert werden.* | [Azure AD B2C wird in diesem Verzeichnis ausgeführt](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Ein für den ordnungsgemäßen Betrieb von Azure AD Domain Services erforderlicher Dienstprinzipal wurde aus Ihrem Azure AD-Verzeichnis gelöscht. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.* | [Fehlender Dienstprinzipal](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Der IP-Adressbereich für das virtuelle Netzwerk, in dem Sie Azure AD Domain Services aktiviert haben, liegt in einem öffentlichen IP-Bereich. Azure AD Domain Services müssen in einem virtuellen Netzwerk mit einem privaten IP-Adressbereich aktiviert werden. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.* | [Adresse befindet sich in einem öffentlichen IP-Adressbereich](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft kann nicht auf die Domänencontroller für diese verwaltete Domäne zugreifen. Dies kann geschehen, wenn eine für Ihr virtuelles Netzwerk konfigurierte Netzwerksicherheitsgruppe (NSG) den Zugriff auf ihre verwaltete Domäne blockiert. Eine weitere mögliche Ursache besteht in einer benutzerdefinierten Route, die eingehenden Datenverkehr aus dem Internet blockiert.* | [Netzwerkfehler](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *Der Dienstprinzipal mit der Anwendungs-ID „d87dcbc6-a371-462e-88e3-28ad15ec4e64“ wurde gelöscht und dann neu erstellt. Die Wiederherstellung hinterlässt inkonsistente Berechtigungen für Azure AD Domain Services-Ressourcen, die für Ihre verwaltete Domäne benötigt werden. Die Synchronisierung von Kennwörtern in Ihrer verwalteten Domäne kann beeinträchtigt werden.* | [Die Kennwortsynchronisierungsanwendung ist veraltet.](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS500 | *Die verwaltete Domäne wurde zuletzt am [Datum] mit Azure AD synchronisiert. Benutzer können sich möglicherweise nicht bei der verwalteten Domäne anmelden, oder Gruppenmitgliedschaften sind möglicherweise nicht mit Azure AD synchronisiert.* | [Die Synchronisierung wurde eine Weile nicht durchgeführt.](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Die verwaltete Domäne wurde zuletzt am [Datum] gesichert.* | [Eine Sicherung wurde eine Weile nicht durchgeführt.](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Das sichere LDAP-Zertifikat für die verwaltete Domäne läuft am XX ab.* | [Ablauf des sicheren LDAP-Zertifikats](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Die verwaltete Domäne wird angehalten, da das mit der Domäne verknüpfte Azure-Abonnement nicht aktiv ist.* | [Anhalten aufgrund eines deaktivierten Abonnements](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Die verwaltete Domäne wird aufgrund einer ungültigen Konfiguration angehalten. Der Dienst konnte die Domänencontroller für Ihre verwaltete Domäne für einen längeren Zeitraum nicht verwalten, kein Patch für sie ausführen oder sie nicht aktualisieren.* | [Anhalten aufgrund einer ungültigen Konfiguration](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: Fehlendes Verzeichnis
**Warnmeldung:**

*Das Ihrer verwalteten Domäne zugeordnete Azure AD-Verzeichnis wurde möglicherweise gelöscht. Die verwaltete Domäne befindet sich nicht mehr in einer unterstützten Konfiguration. Microsoft kann Ihre verwaltete Domäne nicht überwachen, verwalten, patchen und synchronisieren.*

**Lösung:**

Dieser Fehler wird normalerweise durch das nicht ordnungsgemäße Verschieben Ihres Azure-Abonnements in ein neues Azure AD-Verzeichnis und das anschließende Löschen des alten Azure AD-Verzeichnisses verursacht, das immer noch Azure AD Domain Services zugeordnet ist.

Dieser Fehler ist nicht behebbar. Um das Problem zu lösen, müssen Sie [Ihre vorhandene verwaltete Domäne löschen](active-directory-ds-disable-aadds.md) und sie in Ihrem neuen Verzeichnis neu erstellen. Wenn beim Löschen Probleme auftreten, bitten Sie das Produktteam für Azure Active Directory Domain Services um [Unterstützung](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C wird in diesem Verzeichnis ausgeführt
**Warnmeldung:**

*Azure AD Domain Services kann in einem Azure AD B2C-Verzeichnis nicht aktiviert werden.*

**Lösung:**

>[!NOTE]
>Um Azure AD Domain Services weiterhin zu verwenden, müssen Sie Ihre Instanz von Azure AD Domain Services in einem anderen als einem Azure AD B2C-Verzeichnis neu erstellen.

Führen Sie die folgenden Schritte aus, um Ihren Dienst wiederherzustellen:

1. [Löschen Sie Ihre verwaltete Domäne](active-directory-ds-disable-aadds.md) aus Ihrem vorhandenen Azure AD-Verzeichnis.
2. Erstellen Sie ein neues Verzeichnis, das kein Azure AD B2C-Verzeichnis ist.
3. Folgen Sie den Anweisungen im Leitfaden [Erste Schritte](active-directory-ds-getting-started.md), um eine verwaltete Domäne neu zu erstellen.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adresse befindet sich in einem öffentlichen IP-Adressbereich

**Warnmeldung:**

*Der IP-Adressbereich für das virtuelle Netzwerk, in dem Sie Azure AD Domain Services aktiviert haben, liegt in einem öffentlichen IP-Bereich. Azure AD Domain Services müssen in einem virtuellen Netzwerk mit einem privaten IP-Adressbereich aktiviert werden. Diese Konfiguration wirkt sich darauf aus, wie Microsoft Ihre verwaltete Domäne überwachen, verwalten, patchen und synchronisieren kann.*

**Lösung:**

> [!NOTE]
> Zum Beheben dieses Problems müssen Sie Ihre vorhandene verwaltete Domäne löschen und sie in einem virtuellen Netzwerk mit einem privaten IP-Adressbereich neu erstellen. Dieser Vorgang geht mit einer Dienstunterbrechung einher.

Lesen Sie den Abschnitt **privater IP-v4-Adressbereich** in [diesem Artikel](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces), bevor Sie beginnen.

Innerhalb des virtuellen Netzwerks können Computer Anforderungen für Azure-Ressourcen ausführen, die sich im gleichen IP-Adressbereich wie die für das Subnetz konfigurierten befinden. Da das virtuelle Netzwerk jedoch für diesen Bereich konfiguriert ist, werden diese Anforderungen innerhalb des virtuellen Netzwerks weitergeleitet und erreichen nicht die vorgesehenen Webressourcen. Diese Konfiguration kann zu unvorhersehbaren Fehlern mit Azure AD Domain Services führen.

**Wenn Sie der Besitzer des in Ihrem virtuellen Netzwerk konfigurierten IP-Adressbereichs sind, kann diese Warnung ignoriert werden. Allerdings kann Azure AD Domain Services die [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] mit dieser Konfiguration nicht einhalten, da sie zu unvorhersehbaren Fehlern führen kann.**


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
8. Um sicherzustellen, dass die Warnung aufgelöst wurde, überprüfen Sie den Status Ihrer Domäne nach zwei Stunden noch einmal.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Synchronisierung wurde nicht in kurzer Zeit abgeschlossen

**Warnmeldung:**

*Die verwaltete Domäne wurde zuletzt am [Datum] mit Azure AD synchronisiert. Benutzer können sich möglicherweise nicht bei der verwalteten Domäne anmelden, oder Gruppenmitgliedschaften sind möglicherweise nicht mit Azure AD synchronisiert.*

**Lösung:**

[Überprüfen Sie die Integrität Ihrer Domäne](active-directory-ds-check-health.md) auf Warnungen, die auf Probleme in Ihrer Konfiguration der verwalteten Domäne hinweisen könnten. In einigen Fällen können Probleme mit der Konfiguration die Fähigkeit von Microsoft zum Synchronisieren Ihrer verwalteten Domäne blockieren. Wenn Sie Warnungen auflösen können, warten Sie zwei Stunden, und überprüfen Sie dann, ob die Synchronisierung abgeschlossen ist.


## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Eine Sicherung wurde eine Weile nicht durchgeführt.

**Warnmeldung:**

*Die verwaltete Domäne wurde zuletzt am [Datum] gesichert.*

**Lösung:**

[Überprüfen Sie die Integrität Ihrer Domäne](active-directory-ds-check-health.md) auf Warnungen, die auf Probleme in Ihrer Konfiguration der verwalteten Domäne hinweisen könnten. In einigen Fällen können Probleme mit der Konfiguration die Fähigkeit von Microsoft zum Synchronisieren Ihrer verwalteten Domäne blockieren. Wenn Sie Warnungen auflösen können, warten Sie zwei Stunden, und überprüfen Sie dann, ob die Synchronisierung abgeschlossen ist.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Anhalten aufgrund eines deaktivierten Abonnements

**Warnmeldung:**

*Die verwaltete Domäne wird angehalten, da das mit der Domäne verknüpfte Azure-Abonnement nicht aktiv ist.*

**Lösung:**

Zum Wiederherstellen Ihres Diensts [erneuern Sie Ihr Azure-Abonnement](https://docs.microsoft.com/en-us/azure/billing/billing-subscription-become-disable), das Ihrer verwalteten Domäne zugeordnet ist.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Anhalten aufgrund einer ungültigen Konfiguration

**Warnmeldung:**

*Die verwaltete Domäne wird aufgrund einer ungültigen Konfiguration angehalten. Der Dienst konnte die Domänencontroller für Ihre verwaltete Domäne für einen längeren Zeitraum nicht verwalten, kein Patch für sie ausführen oder sie nicht aktualisieren.*

**Lösung:**

[Überprüfen Sie die Integrität Ihrer Domäne](active-directory-ds-check-health.md) auf Warnungen, die auf Probleme in Ihrer Konfiguration der verwalteten Domäne hinweisen könnten. Wenn Sie einige von diesen Warnungen auflösen können, tun Sie dies. Wenden Sie sich danach an den Support, um Ihr Abonnement erneut zu aktivieren.

## <a name="contact-us"></a>Kontakt
Wenden Sie sich an das Produktteam der Azure Active Directory Domain Services, um [Feedback zu geben oder Support zu erhalten](active-directory-ds-contact-us.md).
