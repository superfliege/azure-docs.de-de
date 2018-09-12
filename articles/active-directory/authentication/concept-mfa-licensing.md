---
title: Azure MFA-Versionen und Verbrauchspläne | Microsoft-Dokumentation
description: Informationen über den Client für die Multi-Factor Authentication sowie die verschiedenen verfügbaren Methoden und Versionen
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: e5915157c1ff2c1f58779e5c3b1d784241464b32
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669045"
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Beziehen von Azure Multi-Factor Authentication

Zum Schutz Ihrer Konten sollte die zweistufige Überprüfung in Ihrem Unternehmen Standard sein. Dieses Feature ist insbesondere für Konten wichtig, die privilegierten Zugriff auf Ressourcen haben. Aus diesem Grund bietet Microsoft grundlegende Features zur zweistufigen Überprüfung für Administratoren von Office 365 und Azure Active Directory (Azure AD) ohne Zusatzkosten. Wenn Sie für die Features für Ihre Administratoren ein Upgrade durchführen oder die zweistufige Überprüfung auf den Rest Ihrer Benutzer ausweiten möchten, können Sie Azure Multi-Factor Authentication auf verschiedenen Wegen erwerben.

> [!IMPORTANT]
> Dieser Artikel bietet einen Überblick über die verschiedenen Möglichkeiten zum Erwerb der Azure Multi-Factor Authentication. Ausführliche Informationen zur Preisgestaltung und Abrechnung finden Sie auf der Seite [Multi-Factor Authentication Preise ](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Verfügbare Versionen von Azure Multi-Factor Authentication

Die folgende Tabelle beschreibt die Unterschiede zwischen den drei Versionen der Multi-Factor Authentication:

| Version | BESCHREIBUNG |
| --- | --- |
| Multi-Factor Authentication für Office 365 |Diese Version arbeitet ausschließlich mit Office 365-Anwendungen und wird über das Office 365-Portal verwaltet. Administratoren können [ihre Office 365-Ressourcen mit der zweistufigen Überprüfung sichern](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Diese Version ist Teil eines Office 365-Abonnements. |
| Multi-Factor Authentication für Azure AD-Administratoren | Benutzer, denen die Rolle „Globaler Administrator“ für Azure AD-Mandanten zugewiesen ist, können die zweistufige Überprüfung ohne zusätzliche Kosten aktivieren.|
| Azure Multi-Factor Authentication | Azure Multi-Factor Authentication wird häufig auch als „Vollversion“ bezeichnet und bietet von allen Versionen den größten Funktionsumfang. Über das [Azure-Portal](https://portal.azure.com) sind zusätzliche Konfigurationsoptionen verfügbar. Außerdem stehen erweiterte Funktionen für die Berichterstellung zur Verfügung, und es werden verschiedene lokale Anwendungen und Cloudanwendungen unterstützt. Azure Multi-Factor Authentication ist ein Feature von [Azure Active Directory Premium](https://www.microsoft.com/cloud-platform/azure-active-directory-features) und kann in der Cloud oder lokal bereitgestellt werden. |

> [!NOTE]
> Ab dem 1. September 2018 können Neukunden Azure Multi-Factor Authentication nicht mehr als eigenständiges Angebot erwerben. Multi-Factor Authentication ist in Azure AD Premium-Lizenzen weiterhin ein verfügbares Feature.

## <a name="feature-comparison-of-versions"></a>Funktionsvergleich der Versionen

In der folgenden Tabelle werden die Features aufgeführt, die in den verschiedenen Versionen von Azure Multi-Factor Authentication verfügbar sind:

> [!NOTE]
> In dieser Vergleichstabelle werden die Features erläutert, die in den einzelnen Versionen von Multi-Factor Authentication enthalten sind. Wenn Sie über die Vollversion des Multi-Factor Authentication-Diensts verfügen, sind einige Features möglicherweise nicht verfügbar (je nachdem, ob Sie [MFA in der Cloud oder lokal](concept-mfa-whichversion.md) verwenden).

| Feature | Multi-Factor Authentication für Office 365 | Multi-Factor Authentication für Azure AD-Administratoren | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Schutz von Azure AD-Administratorkonten mit MFA |● |● (Gilt nur für globale Azure AD-Administratorkonten) |● |
| Mobile App als zweiter Faktor |● |● |● |
| Telefonanruf als zweiter Faktor |● |● |● |
| SMS als zweiter Faktor |● |● |● |
| App-Kennwörter für Clients, die MFA nicht unterstützen |● |● |● |
| Administrative Kontrolle über Überprüfungsmethoden |● |● |● |
| Schutz von Nicht-Administrator-Konten mit MFA |● (Nur für Office 365-Anwendungen) | |● |
| PIN-Modus | | |● |
| Betrugswarnung | | |● |
| MFA-Berichte | | |● |
| Einmalumgehung | | |● |
| Benutzerdefinierte Begrüßungen für Telefonanrufe | | |● |
| Benutzerdefinierte Anrufer-ID für Telefonanrufe | | |● |
| Vertrauenswürdige IP-Adressen | | |● |
| Speichern der MFA für vertrauenswürdige Geräte |● |● |● |
| MFA-SDK | | |● (veraltet) | 
| MFA für lokale Anwendungen | | |● |

## <a name="how-to-turn-on-azure-multi-factor-authentication-for-azure-ad-administrators"></a>Aktivieren von Azure Multi-Factor Authentication für Azure AD-Administratoren

Benutzer, denen die Rolle „Globaler Administrator“ für Azure AD-Mandanten zugewiesen ist, können die zweistufige Überprüfung für ihre globalen Azure AD-Administratorkonten ohne zusätzliche Kosten aktivieren. Wenn Sie ein Microsoft-Konto verwenden, können Sie sich für die Multi-Factor Authentication registrieren, indem Sie die Anleitung im Microsoft-Konto-Support-Artikel [Informationen zur Überprüfung in zwei Schritten](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) verwenden. Wenn Sie kein Microsoft-Konto verwenden, aktivieren Sie die Multi-Factor Authentication für globale Administratoren mithilfe der Anleitung im Artikel [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer oder eine Gruppe](howto-mfa-userstates.md).

## <a name="how-to-get-azure-multi-factor-authentication"></a>Beziehen von Azure Multi-Factor Authentication

Wenn Sie alle Funktionen von Azure Multi-Factor Authentication nutzen möchten, stehen mehrere Optionen zur Verfügung:

### <a name="option-1---licenses-that-include-mfa"></a>Option 1 – Lizenzen, die MFA umfassen

Erwerben Sie Lizenzen mit Azure Multi-Factor Authentication, wie Azure Active Directory Premium oder ein Lizenzbundle mit Azure AD Premium, und weisen Sie diese Ihren Benutzern in Azure Active Directory zu.

Erstellen Sie bei dieser Option nur dann einen Azure Multi-Factor Authentication-Anbieter, wenn Sie für einige Benutzer, die über keine Lizenzen verfügen, die zweistufige Überprüfung bereitstellen müssen. Andernfalls erfolgt die Abrechnung unter Umständen zweimal.

### <a name="option-2---mfa-consumption-based-model"></a>Option 2 – Verbrauchsbasiertes MFA-Modell

> [!NOTE]
> Ab dem 1. September 2018 können keine neuen Authentifizierungsanbieter mehr erstellt werden. Vorhandene Authentifizierungsanbieter können ggf. weiterhin verwendet und aktualisiert werden. Multi-Factor Authentication ist in Azure AD Premium-Lizenzen weiterhin ein verfügbares Feature.

Erstellen Sie einen Azure Multi-Factor Authentication-Anbieter in einem Azure-Abonnement. Azure MFA-Anbieter sind Azure-Ressourcen, die wie alle anderen Azure-Ressourcen über Ihr Enterprise Agreement, Ihre Azure-Zahlungsverpflichtung oder Ihre Kreditkarte abgerechnet werden. Diese Anbieter können nur in vollständigen Azure-Abonnements erstellt werden und nicht in beschränkten Azure-Abonnements, für die ein Ausgabenlimit von 0 US-Dollar gilt. Beschränkte Abonnements werden beim Aktivieren von Lizenzen erstellt, wie etwa in Option 1.

Bei Verwendung eines Azure Multi-Factor Authentication-Anbieters stehen zwei Nutzungsmodelle zur Verfügung, die über Ihr Azure-Abonnement abgerechnet werden:

1. **Pro aktiviertem Benutzer:** Diese Option ist für Unternehmen geeignet, die die zweistufige Überprüfung für eine feste Anzahl von Mitarbeitern aktivieren möchten, die die Authentifizierung regelmäßig benötigen. Die Abrechnung pro Benutzer basiert auf der Anzahl der Benutzer in Ihrem Azure AD-Mandanten und auf Ihrem Azure MFA-Server, für die MFA aktiviert ist. Wenn MFA für Benutzer sowohl in Azure AD als auch auf dem Azure MFA-Server aktiviert ist und die Domänensynchronisierung (Azure AD Connect) ebenfalls aktiviert ist, berechnen wir die größere Benutzergruppe. Wenn die Domänensynchronisierung nicht aktiviert ist, berechnen wir die Summe aller Benutzer, für die MFA in Azure AD und auf dem Azure MFA-Server aktiviert ist. Die Abrechnung erfolgt anteilsmäßig und wird täglich an das Commerce-System berichtet.

  > [!NOTE]
  > Abrechnungsbeispiel 1: Für MFA sind heute 5.000 Benutzer aktiviert. Das MFA-System teilt diese Zahl durch 31 und meldet 161,29 Benutzer für diesen Tag. Morgen aktivieren Sie 15 weitere Benutzer, sodass das MFA-System für diesen Tag 161,77 Benutzer meldet. Am Ende des Abrechnungszyklus beträgt die Gesamtzahl der Benutzer, die im Rahmen Ihres Azure-Abonnements abgerechnet werden, 5.000.
  >
  > Abrechnungsbeispiel 2: Sie haben sowohl Benutzer mit als auch ohne Lizenz. Für die Differenz verwenden Sie einen Azure MFA-Anbieter für eine Abrechnung pro Benutzer. In Ihrem Mandanten befinden sich 4.500 Enterprise Mobility + Security-Lizenzen, für MFA sind jedoch 5.000 Benutzer aktiviert. Für Ihr Azure-Abonnement werden 500 Benutzer abgerechnet, die täglich anteilsmäßig als 16,13 Benutzer gemeldet werden.

2. **Pro Authentifizierung** – Diese Option ist für Unternehmen geeignet, die die zweistufige Überprüfung für eine große Benutzergruppe aktivieren möchten, welche die Authentifizierung unregelmäßig benötigt. Die Abrechnung basiert auf der Anzahl der Anforderungen für die zweistufige Überprüfung, unabhängig davon, ob diese Überprüfungen erfolgreich waren oder abgelehnt wurden. Diese Abrechnung erscheint auf Ihrer Azure-Nutzungsaufstellung in Paketen mit 10 Authentifizierungen und wird täglich gemeldet.

  > [!NOTE]
  > Abrechnungsbeispiel 3: Der Azure MFA-Dienst hat heute 3.105 Anforderungen für die zweistufige Überprüfung empfangen. Ihrem Azure-Abonnement werden heute 310,5 Authentifizierungspakete berechnet.

Beachten Sie, dass Sie Lizenzen haben können, die verbrauchsbasierte Konfiguration aber weiterhin berechnet wird. Wenn Sie einen Azure MFA-Anbieter für die Abrechnung pro Authentifizierung einrichten, wird jede Anforderung für die zweistufige Überprüfung abgerechnet, auch für Benutzer, die über Lizenzen verfügen. Wenn Sie einen Azure MFA-Anbieter für die Abrechnung pro Benutzer einrichten, der nicht mit Ihrem Azure AD-Mandanten verknüpft ist, erfolgt die Abrechnung pro Benutzer, auch wenn Ihre Benutzer in Azure AD über Lizenzen verfügen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Preisinformationen finden Sie unter [Preise für Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Wählen Sie, ob Azure MFA [in der Cloud oder lokal](concept-mfa-whichversion.md) bereitgestellt werden soll.
