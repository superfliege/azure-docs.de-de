---
title: Kennwortschutz für Azure AD
description: Sperren unsicherer Kennwörter im lokalen Active Directory mit dem Kennwortschutz für Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1b3660d256e4beda948f723035aa75ca8a9ed2e
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58284867"
---
# <a name="enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Erzwingen des Azure AD-Kennwortschutzes für Windows Server Active Directory

Der Kennwortschutz für Azure AD ist ein Feature, das Kennwortrichtlinien in einer Organisation verbessert. Der Kennwortschutz für lokale Bereitstellungen verwendet sowohl die Liste der globalen als auch benutzerdefinierten gesperrten Kennwörter, die in Azure AD gespeichert sind. Er führt lokal die gleichen Überprüfungen durch wie Azure AD wie bei cloudbasierten Änderungen.

## <a name="design-principles"></a>Entwurfsprinzipien

Der Kennwortschutz für Azure AD wurde unter Berücksichtigung der folgenden Prinzipien entwickelt:

* Domänencontroller müssen nie direkt mit dem Internet kommunizieren.
* Auf Domänencontrollern sind keine neuen Netzwerkports geöffnet.
* Es sind keine Änderungen des Active Directory-Schemas erforderlich. Die Software verwendet die vorhandenen Active Directory-Schemaobjekte vom Typ **container** und **serviceConnectionPoint**.
* Für die Funktionsebene von Active Directory-Domäne oder Gesamtstruktur (DFL\FFL) gelten keine Mindestanforderungen.
* Die Software erstellt oder erfordert keine Konten in den von ihr geschützten Active Directory-Domänen.
* Klartextkennwörter verlassen den Domänencontroller weder bei der Kennwortüberprüfung noch zu anderen Zeitpunkten.
* Die inkrementelle Bereitstellung wird unterstützt. Die Kennwortrichtlinie wird jedoch nur dort erzwungen, wo der Domänencontrolleragent (DC-Agent) installiert ist.
* Installieren Sie den DC-Agent auf allen Domänencontrollern, um die Durchsetzung des universellen Kennwortschutzes zu gewährleisten.

## <a name="architectural-diagram"></a>Architekturdiagramm

Bevor Sie den Kennwortschutz von Azure AD in einer lokalen Active Directory-Umgebung bereitstellen, sollten Sie sich mit den zugrunde liegenden Entwurfs- und Funktionskonzepten vertraut machen. Im folgenden Diagramm sehen Sie, wie die Komponenten des Kennwortschutzes zusammenarbeiten:

![Zusammenspiel der Komponenten des Azure AD-Kennwortschutzes](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

* Der Azure AD-Kennwortschutz-Proxydienst wird auf einem beliebigen in die Domäne eingebundenen Computer in der aktuellen Active Directory-Gesamtstruktur ausgeführt. Er dient primär dazu, Downloadanforderungen für Kennwortrichtlinien von Domänencontrollern an Azure AD weiterzuleiten. Anschließend gibt er die Antwort von Azure AD an den Domänencontroller zurück.
* Die Kennwortfilter-DLL des DC-Agents empfängt Anforderungen zur Kennwortüberprüfung vom Betriebssystem. Sie leitet diese an den DC-Agent-Dienst weiter, der lokal auf dem Domänencontroller ausgeführt wird.
* Der DC-Agent-Dienst des Kennwortschutzes empfängt Anforderungen zur Kennwortüberprüfung von der Kennwortfilter-DLL des DC-Agents. Er verarbeitet sie mit der aktuellen (lokal verfügbaren) Kennwortrichtlinie und gibt das Ergebnis zurück: *pass* (erfolgreich) oder *fail* (fehlerhaft).

## <a name="how-password-protection-works"></a>Funktionsweise des Kennwortschutzes

Jede Proxydienstinstanz des Kennwortschutzes für Azure AD kündigt sich selbst bei Domänencontrollern in der Gesamtstruktur an, indem sie ein Objekt vom Typ **serviceConnectionPoint** in Active Directory erstellt.

Jeder DC-Agent-Dienst für den Kennwortschutz erstellt zudem ein Objekt vom Typ **serviceConnectionPoint** in Active Directory. Dieses Objekt wird jedoch in erster Linie für Berichte und Diagnosen verwendet.

Der DC-Agent-Dienst ist dafür verantwortlich, das Herunterladen einer neuen Kennwortrichtlinie von Azure AD einzuleiten. Zuerst sucht er einen Proxydienst für den Azure AD-Kennwortschutz. Dazu fragt er Proxyobjekte vom Typ **serviceConnectionPoint** in der Gesamtstruktur ab. Sobald er einen verfügbaren Proxydienst gefunden hat, sendet der DC-Agent eine Downloadanforderung für Kennwortrichtlinien an diesen Proxydienst. Der Proxydienst sendet die Anforderung wiederum an Azure AD. Der Proxydienst gibt dann die Antwort an den DC-Agent-Dienst zurück.

Nachdem der DC-Agent-Dienst eine neue Kennwortrichtlinie von Azure AD erhalten hat, speichert er die Richtlinie in einem speziellen Ordner im Stammverzeichnis der *sysvol*-Ordnerfreigabe seiner Domäne. Der DC-Agent-Dienst überwacht auch diesen Ordner, falls neuere Richtlinien von anderen DC-Agent-Diensten in der Domäne repliziert werden.

Der DC-Agent-Dienst fordert beim Starten des Diensts immer eine neue Richtlinie an. Nachdem der DC-Agent-Dienst gestartet wurde, überprüft er stündlich das Alter der aktuellen lokal verfügbaren Richtlinie. Wenn die Richtlinie älter als eine Stunde ist, fordert der DC-Agent wie oben beschrieben eine neue Richtlinie von Azure AD an. Wenn die aktuelle Richtlinie nicht älter als eine Stunde ist, verwendet der DC-Agent weiterhin diese Richtlinie.

Wenn eine Kennwortrichtlinie für den Azure AD-Kennwortschutz heruntergeladen wird, gilt diese Richtlinie spezifisch für einen Mandanten. Anders gesagt: Kennwortrichtlinien sind stets eine Kombination der globalen Liste gesperrter Kennwörter von Microsoft und der benutzerdefinierten Liste gesperrter Kennwörter des Mandanten.

Der DC-Agent kommuniziert mit dem Proxydienst via RPC über TCP. Der Proxydienst lauscht an einem dynamischen oder statischen RPC-Port auf diese Aufrufe (je nach Konfiguration).

Der DC-Agent lauscht nie an einem im Netzwerk verfügbaren Port.

Der Proxydienst ruft nie den DC-Agent-Dienst auf.

Der Proxydienst ist zustandslos. Richtlinien oder andere aus Azure heruntergeladene Zustände werden nicht zwischengespeichert.

Der DC-Agent-Dienst verwendet immer die neueste lokal verfügbare Kennwortrichtlinie, um Benutzerkennwörter auszuwerten. Wenn keine Kennwortrichtlinie auf dem lokalen Domänencontroller verfügbar ist, wird das Kennwort automatisch akzeptiert. In diesem Fall wird eine Ereignismeldung protokolliert, um den Administrator zu warnen.

Der Azure AD-Kennwortschutz ist kein Echtzeitmodul für die Richtlinienanwendung. Zwischen einer Änderung der Kennwortrichtlinienkonfiguration in Azure AD und dem Zeitpunkt, zu dem sie alle Domänencontroller erreicht und dort durchgesetzt wird, kann eine Verzögerung auftreten.

## <a name="foresttenant-binding-for-password-protection"></a>Gesamtstruktur-/Mandantenbindung für den Kennwortschutz

Wenn Sie den Kennwortschutz für Azure AD in einer Active Directory-Gesamtstruktur bereitstellen möchten, müssen Sie diese Gesamtstruktur bei Azure AD registrieren. Die bereitgestellten Proxydienste müssen ebenfalls bei Azure AD registriert werden. Diese Registrierungen der Gesamtstrukturen und Proxydienste sind einem bestimmten Azure AD-Mandanten zugeordnet, der implizit über die bei der Registrierung verwendeten Anmeldeinformationen identifiziert wird.

Die Active Directory-Gesamtstruktur und alle bereitgestellten Proxydienste innerhalb einer Gesamtstruktur müssen beim gleichen Mandanten registriert werden. Active Directory-Gesamtstrukturen oder andere Proxydienste in dieser Gesamtstruktur können nicht bei unterschiedlichen Azure AD-Mandanten registriert werden. Wenn die Bereitstellung falsch konfiguriert wurde, können Sie beispielsweise keine Kennwortrichtlinien herunterladen.

## <a name="license-requirements"></a>Lizenzanforderungen

Die Vorteile der Liste global gesperrter Kennwörter gelten für alle Benutzer von Azure AD.

Die Liste benutzerdefinierter gesperrter Kennwörter erfordert Azure AD Basic-Lizenzen.

Azure AD-Kennwortschutz für Windows Server Active Directory erfordert Azure AD Premium-Lizenzen.

Weitere Informationen zur Lizenzierung finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Download

Die zwei erforderlichen Agent-Installer für den Azure AD-Kennwortschutz können Sie im [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) herunterladen.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises-deploy.md)
