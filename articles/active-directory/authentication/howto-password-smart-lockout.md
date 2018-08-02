---
title: Verhindern von Brute-Force-Angriffen mithilfe von Azure AD Smart Lockout
description: Azure AD Smart Lockout trägt dazu bei, Ihre Organisation vor Brute-Force-Angriffen zu schützen, bei denen versucht wird, Kennwörter zu erraten.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: b0fded9f5543d151091955c0b0d645bf9db16b7d
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158582"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory Smart Lockout

Smart Lockout verwendet intelligente Cloudfunktionen, um schädliche Akteure auszusperren, die versuchen, Benutzerkennwörter zu erraten oder mithilfe von Brute-Force-Methoden in Ihr System einzudringen. Diese intelligenten Funktionen können Anmeldevorgänge gültiger Benutzer erkennen und anders behandeln als Anmeldevorgänge von Angreifern oder anderen unbekannten Quellen. Smart Lockout sperrt die Angreifer aus, während Ihre Benutzer weiterhin auf ihre Konten zugreifen und produktiv arbeiten können.

Nach zehn Fehlversuchen sperrt Smart Lockout das Konto standardmäßig eine Minute lang für Anmeldeversuche. Das Konto wird nach jedem weiteren fehlgeschlagenen Anmeldeversuch zuerst für eine Minute und bei anschließenden Versuchen länger gesperrt.

Smart Lockout ist für alle Azure AD-Kunden ständig aktiv und bietet standardmäßig die richtige Mischung aus Sicherheit und Nutzbarkeit. Wenn Sie die Smart Lockout-Einstellungen mit spezifischen Werten für Ihre Organisation konfigurieren möchten, benötigen Sie mindestens Azure AD Basic-Lizenzen für Ihre Benutzer.

Smart Lockout ist in Hybridbereitstellungen integrierbar und kann mittels Kennworthashsynchronisierung oder Passthrough-Authentifizierung verhindern, dass lokale Active Directory-Konten von Angreifern gesperrt werden. Durch korrektes Festlegen von Smart Lockout-Richtlinien in Azure AD können Angriffe herausgefiltert werden, bevor sie die lokale Active Directory-Instanz erreichen.

Bei Verwendung der [Passthrough-Authentifizierung](../connect/active-directory-aadconnect-pass-through-authentication.md) muss Folgendes sichergestellt werden:

   * Der Azure AD-Sperrschwellenwert ist **kleiner** als der Schwellenwert für eine Active Directory-Kontosperrung. Legen Sie die Werte so fest, dass der Schwellenwert für eine Active Directory-Kontosperrung mindestens das Zwei- oder Dreifache des Azure AD-Sperrschwellenwerts beträgt. 
   * Die Azure AD-Sperrdauer (in **Sekunden**) ist **länger** als die Active Directory-Zurücksetzungsdauer des Kontosperrungszählers (in **Minuten**).

> [!IMPORTANT]
> Derzeit können die Cloudkonten der Benutzer nicht von einem Administrator entsperrt werden, wenn sie von Smart Lockout gesperrt wurden. Der Administrator muss warten, bis die Sperrdauer abgelaufen ist.

## <a name="verify-on-premises-account-lockout-policy"></a>Überprüfen der lokalen Kontosperrungsrichtlinie

Gehen Sie wie folgt vor, um Ihre lokale Active Directory-Kontosperrungsrichtlinie zu überprüfen:

1. Öffnen Sie das Gruppenrichtlinienverwaltungstool.
2. Bearbeiten Sie die Gruppenrichtlinie, die die Kontosperrungsrichtlinie Ihrer Organisation enthält (beispielsweise die **Standarddomänenrichtlinie**).
3. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Windows-Einstellungen** > **Sicherheitseinstellungen** > **Kontorichtlinien** > **Kontosperrungsrichtlinien**.
4. Überprüfen Sie Ihre Werte für **Kontensperrungsschwelle** und **Zurücksetzungsdauer des Kontosperrungszählers**.

![Ändern der lokalen Active Directory-Kontosperrungsrichtlinie mithilfe eines Gruppenrichtlinienobjekts](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Verwalten der Werte für Azure AD Smart Lockout

Abhängig von den Anforderungen Ihrer Organisation müssen die Werte für Smart Lockout ggf. angepasst werden. Wenn Sie die Smart Lockout-Einstellungen mit spezifischen Werten für Ihre Organisation konfigurieren möchten, benötigen Sie mindestens Azure AD Basic-Lizenzen für Ihre Benutzer.

Gehen Sie wie folgt vor, um die Smart Lockout-Werte zu überprüfen und ggf. für Ihre Organisation anzupassen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Azure Active Directory** > **Authentifizierungsmethoden**.
1. Legen Sie **Schwellenwert für Sperre** auf die Anzahl nicht erfolgreicher Anmeldeversuche fest, nach der das Konto erstmals gesperrt werden soll. (Standardwert: 10.)
1. Legen unter **Sperrdauer in Sekunden** fest, wie viele Sekunden die Sperre jeweils dauern soll. Der Standardwert ist 60 Sekunden (eine Minute).

> [!NOTE]
> Ist der erste Anmeldeversuch nach einer Sperre wieder nicht erfolgreich, wird das Konto erneut sperrt. Bei wiederholter Kontosperrung erhöht sich die Sperrdauer.

![Anpassen Azure AD Smart Lockout-Richtlinie über das Azure-Portal](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie Sie mithilfe von Azure AD ungültige Kennwörter aus Ihrer Organisation verbannen.](howto-password-ban-bad.md)

[Konfigurieren Sie die Self-Service-Kennwortzurücksetzung, damit Benutzer ihre Konten selbst entsperren können.](quickstart-sspr.md)