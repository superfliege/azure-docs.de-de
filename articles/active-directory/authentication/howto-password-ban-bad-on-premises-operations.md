---
title: Azure AD-Kennwortschutz (Vorschauversion) – Vorgänge und Berichterstellung
description: Azure AD-Kennwortschutz (Vorschauversion) – Vorgänge und Berichterstellung nach der Bereitstellung
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 8d7002a014fc6cfab1888a6bc97c0f864de1d99d
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080870"
---
# <a name="preview-azure-ad-password-protection-operational-procedures"></a>Vorschau: Verfahren des Azure AD-Kennwortschutzes

|     |
| --- |
| Azure AD-Kennwortschutz ist eine öffentliche Vorschaufunktion für Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Nachdem Sie die Installation des [Azure AD-Kennwortschutzes](howto-password-ban-bad-on-premises-deploy.md) vor Ort abgeschlossen haben, müssen einige Elemente im Azure-Portal konfiguriert werden.

## <a name="configure-the-custom-banned-password-list"></a>Konfigurieren der Liste benutzerdefinierter gesperrter Kennwörter

Befolgen Sie die Anweisungen im Artikel [Konfigurieren der Liste benutzerdefinierter gesperrter Kennwörter](howto-password-ban-bad-configure.md), um die Liste benutzerdefinierter Kennwörter für Ihre Organisation anzupassen.

## <a name="enable-password-protection"></a>Aktivieren des Kennwortschutzes

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Azure Active Directory**, **Authentifizierungsmethoden** und **Kennwortschutz (Vorschau)**.
1. Legen Sie **Kennwortschutz für Windows Server Active Directory aktivieren** auf **Ja** fest.
1. Wie im [Bereitstellungshandbuch](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy) bereits erwähnt, wird empfohlen, den **Modus** zunächst auf **Überwachung** festzulegen.
   * Wenn Sie mit dem Feature vertraut sind, können Sie zum **Modus** **Erzwingen** wechseln.
1. Klicken Sie unten auf der Seite auf **Speichern**.

![Aktivieren der Komponenten des Azure AD-Kennwortschutzes im Azure-Portal](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Überwachungsmodus

Der Überwachungsmodus ist als Möglichkeit gedacht, die Software in einem „Was-wäre-wenn“-Modus auszuführen. Jeder DC-Agentendienst wertet ein eingehendes Kennwort entsprechend der aktuell aktiven Richtlinie aus. Wenn die aktuelle Richtlinie im Überwachungsmodus konfiguriert ist, führen „unzulässige“ Kennwörter zu Ereignismeldungen, werden aber akzeptiert. Dies ist der einzige Unterschied zwischen dem Überwachungs-- und dem Erzwingungsmodus; alle anderen Vorgänge laufen gleich ab.

> [!NOTE]
> Microsoft empfiehlt, dass die anfängliche Bereitstellung und das Testen immer im Überwachungsmodus beginnen. Ereignisse im Ereignisprotokoll sollten dann überwacht werden, um vorauszusehen, ob bestehende Betriebsvorgänge gestört werden würden, sobald der Erzwingungsmodus aktiviert ist.

## <a name="enforce-mode"></a>Erzwingungsmodus

Der Erzwingungsmodus dient als endgültigen Konfiguration. Genauso wie beim oben aufgeführten Überwachungsmodus wertet jeder DC-Agentendienst eingehende Kennwörter entsprechend der aktuell aktiven Richtlinie aus. Ist allerdings der Erzwingungsmodus aktiviert, wird ein Kennwort, das entsprechend der Richtlinie als unsicher angesehen wird, abgelehnt.

Wenn ein Kennwort im Erzwingungsmodus vom DC-Agenten des Azure AD Kennwortschutzes abgelehnt wird, ist die sichtbare Auswirkung für den Endbenutzer identisch mit dem, was er sehen würde, wenn sein Kennwort von der herkömmlichen lokalen Kennwortkomplexitätserzwingung abgelehnt werden würde. Zum Beispiel könnte ein Benutzer die folgende herkömmliche Fehlermeldung im Windows-Bildschirm zum Anmelden bzw. Ändern des Kennworts sehen:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Diese Meldung ist nur ein Beispiel von mehreren möglichen Ergebnissen. Die spezifische Fehlermeldung kann je nach Software oder Szenario, das versucht, ein unsicheres Kennwort festzulegen, variieren.

Betroffene Endbenutzer müssen sich möglicherweise bei ihren IT-Mitarbeitern über die neuen Anforderungen informieren, um sichere Kennwörter wählen zu können.

## <a name="enable-mode"></a>Modus aktivieren

Diese Einstellung sollte normalerweise im standardmäßig aktivierten Zustand (Ja) bleiben. Wenn Sie diese Einstellung auf deaktiviert (Nein) festlegen, führt dies dazu, dass alle bereitgestellten DC-Agents für den Azure AD-Kennwortschutz in einen Ruhemodus wechseln, in dem alle Kennwörter vorbehaltlos akzeptiert werden und keinerlei Validierungsaktivitäten ausgeführt werden (z. B. werden nicht einmal Überwachungsereignisse ausgelöst).

## <a name="usage-reporting"></a>Nutzungsberichte

Das `Get-AzureADPasswordProtectionSummaryReport`-Cmdlet kann verwendet werden, um eine zusammenfassende Darstellung der Aktivität zu erstellen. Eine Beispielausgabe dieses Cmdlets lautet folgendermaßen:

```PowerShell
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

Der Umfang der Berichterstellung des Cmdlets kann mit einem der -Forest, -Domain- oder -DomainController-Parameter beeinflusst werden. Keine Angabe eines Parameters bedeutet –Forest.

> [!NOTE]
> Dieses Cmdlet funktioniert, indem es eine PowerShell-Sitzung mit jedem Domänencontroller öffnet. Damit dies gelingt, muss die Unterstützung für PowerShell-Remotesitzungen auf jedem Domänencontroller aktiviert sein, und der Client muss über ausreichende Berechtigungen verfügen. Weitere Informationen zu Anforderungen für PowerShell-Remotesitzungen führen Sie in einem PowerShell-Fenster „Get-Help about_Remote_Troubleshooting“ aus.

> [!NOTE]
> Dieses Cmdlet funktioniert durch Remoteabfrage des Admin-Ereignisprotokolls jedes DC-Agentendienstes. Wenn die Ereignisprotokolle eine große Anzahl von Ereignissen enthalten, kann es lange dauern, bis das Cmdlet abgeschlossen ist. Darüber hinaus können umfangreiche Netzwerkabfragen großer Datenmengen die Leistung des Domänencontrollers beeinträchtigen. Daher sollte dieses Cmdlet in Produktionsumgebungen sorgfältig verwendet werden.

## <a name="dc-agent-discovery"></a>DC-Agent-Ermittlung

Mit dem `Get-AzureADPasswordProtectionDCAgent`-Cmdlet können grundlegende Informationen über die verschiedenen DC-Agents angezeigt werden, die in einer Domäne oder Gesamtstruktur ausgeführt werden. Diese Informationen werden aus den serviceConnectionPoint-Objekten abgerufen, die von den ausgeführten DC-Agent-Diensten registriert werden. Eine Beispielausgabe dieses Cmdlets lautet folgendermaßen:

```PowerShell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Die verschiedenen Eigenschaften werden von jedem DC-Agent-Dienst ungefähr stündlich aktualisiert. Die Daten unterliegen nach wie vor der Replikationswartezeit von Active Directory.

Der Bereich der Cmdlet-Abfrage wird möglicherweise durch die Verwendung entweder des Parameters „–Forest“ oder „–Domain“ beeinflusst.

## <a name="next-steps"></a>Nächste Schritte

[Problembehandlung und Überwachung beim Azure AD-Kennwortschutz](howto-password-ban-bad-on-premises-troubleshoot.md)
