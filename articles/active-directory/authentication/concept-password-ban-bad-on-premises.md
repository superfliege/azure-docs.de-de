---
title: Kennwortschutz für Azure AD – Vorschauversion
description: Sperren unsicherer Kennwörter im lokalen Active Directory mit der Vorschau des Azure AD-Kennwortschutzes
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
ms.openlocfilehash: f1beae186f6eb276b9aa302d3d51f0ba8688e591
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415747"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Vorschau: Erzwingen des Azure AD-Kennwortschutzes für Windows Server Active Directory

|     |
| --- |
| Azure AD-Kennwortschutz und die benutzerdefinierte Liste der gesperrten Kennwörter sind in der öffentlichen Vorschau befindliche Features von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Azure AD-Kennwortschutz ist ein neues, von Azure Active Directory (Azure AD) unterstütztes Feature in der öffentlichen Vorschau, um Kennwortrichtlinien in einer Organisation zu verbessern. Die lokale Bereitstellung von Azure AD-Kennwortschutz verwendet sowohl die Liste globaler als auch benutzerdefinierter gesperrter Kennwörter, die in Azure AD gespeichert sind, und führt die gleichen Überprüfungen lokal wie bei cloudbasierten Azure AD-Änderungen durch.

## <a name="design-principles"></a>Entwurfsprinzipien

Azure AD-Kennwortschutz für Active Directory wird unter Berücksichtigung der folgenden Prinzipien entworfen:

* Domänencontroller müssen nie direkt mit dem Internet kommunizieren.
* Auf Domänencontrollern sind keine neuen Netzwerkports geöffnet.
* Es sind keine Änderungen des Active Directory-Schemas erforderlich. Die Software verwendet die vorhandenen Active Directory-Container- und serviceConnectionPoint-Schemaobjekte.
* Es besteht keine minimale Anforderung an die Funktionsebene von Active Directory-Domäne oder Gesamtstruktur (DFL\FFL).
* Die Software erstellt weder Konten in den Active Directory-Domänen, die sie schützt, noch setzt sie sie voraus.
* Klartextkennwörter verlassen niemals den Domänencontroller (sei es während der Kennwortvalidierung oder zu einem anderen Zeitpunkt).
* Inkrementelle Bereitstellung wird mit dem Nachteil unterstützt, dass die Kennwortrichtlinie nur dort erzwungen wird, wo der Domänencontroller-Agent installiert ist.
* Es wird empfohlen, den DC-Agent auf allen Domänencontrollern zu installieren, um die Durchsetzung des Kennwortschutzes umfassend zu gewährleisten.

## <a name="architectural-diagram"></a>Architekturdiagramm

Es ist wichtig, den zugrunde liegenden Entwurf und die Funktionskonzepte zu verstehen, bevor Sie Azure AD-Kennwortschutz in einer lokalen Active Directory-Umgebung einsetzen. Das folgende Diagramm zeigt, wie die Komponenten des Azure AD-Kennwortschutzes zusammenarbeiten.

![Zusammenspiel der Komponenten des Azure AD-Kennwortschutzes](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Das Diagramm oben zeigt die drei grundlegenden Softwarekomponenten, die den Azure AD-Kennwortschutz bilden:

* Der Azure AD-Kennwortschutz-Proxydienst wird auf einem beliebigen in die Domäne eingebundenen Computer in der aktuellen Active Directory-Gesamtstruktur ausgeführt. Sein Hauptzweck ist es, Download-Anforderungen von Kennwortrichtlinien von Domänencontrollern an Azure AD weiterzuleiten und die Antwort von Azure AD an den Domänencontroller zurückzugeben.
* Die DC-Agent-Kennwortfilter-DLL für den Azure AD-Kennwortschutz empfängt Anforderungen zur Kennwortüberprüfung vom Betriebssystem und leitet sie an den Azure AD-Kennwortschutz-DC-Agent-Dienst weiter, der lokal auf dem Domänencontroller ausgeführt wird.
* Der DC-Agent-Dienst für den Azure AD-Kennwortschutz empfängt Anforderungen zur Kennwortvalidierung von der DC-Agent-Kennwortfilter-DLL, verarbeitet sie unter Verwendung der aktuellen (lokal verfügbaren) Kennwortrichtlinie und gibt das Ergebnis zurück (bestanden\nicht bestanden).

## <a name="theory-of-operations"></a>Betriebstheorie

Wie funktioniert also der Azure AD-Kennwortschutz angesichts des obigen Diagramms und der Entwurfsprinzipien tatsächlich?

Jedes Azure AD-Kennwortschutz-Proxydienst kündigt sich selbst bei Domänencontrollern in der Gesamtstruktur an, indem er ein serviceConnectionPoint-Objekt im Active Directory erstellt.

Jeder DC-Agent-Dienst für den Azure AD-Kennwortschutz erstellt zudem ein serviceConnectionPoint-Objekt in Active Directory. Dies wird jedoch in erster Linie für Berichte und Diagnosen verwendet.

Der DC-Agent-Dienst für den Azure AD-Kennwortschutz ist dafür verantwortlich, das Herunterladen einer neuen Kennwortrichtlinie von Azure AD einzuleiten. Der erste Schritt besteht darin, einen Azure AD-Kennwortschutz-Proxydienst zu finden, indem Sie die Gesamtstruktur nach Proxy-serviceConnectionPoint-Objekten durchsuchen. Sobald ein verfügbarer Proxy-Dienst gefunden wurde, wird eine Download-Anforderung für Kennwortrichtlinien vom DC-Agent-Dienst an den Proxy-Dienst gesendet, der sie wiederum an Azure AD sendet und dann die Antwort an den DC-Agent-Dienst zurückgibt. Nachdem der DC-Agent-Dienst eine neue Kennwortrichtlinie von Azure AD erhalten hat, speichert er die Richtlinie in einem speziellen Ordner im Stammverzeichnis seiner Sysvol-Freigabe seiner Domäne. Der DC-Agent-Dienst überwacht auch diesen Ordner, falls sich neuere Richtlinien von anderen DC-Agent-Dienste in der Domäne replizieren.

Der DC-Agent-Dienst für den Azure AD-Kennwortschutz fordert immer eine neue Richtlinie für den Start des Dienstes an. Nachdem der DC-Agentendienst gestartet wurde, überprüft er regelmäßig (einmal pro Stunde) das Alter der aktuellen lokal verfügbaren Richtlinie; wenn die aktuelle Richtlinie älter als eine Stunde ist, fordert der DC-Agent-Dienst wie oben beschrieben eine neue Richtlinie von Azure AD an, andernfalls verwendet der DC-Agent weiterhin die aktuelle Richtlinie.

Der DC-Agent-Dienst des Azure AD-Kennwortschutzes kommuniziert mit dem Azure AD-Kennwortschutz-Proxy-Diensts mittels RPC (Remote Procedure Call) über TCP. Der Dienst überwacht diese Aufrufe über einen dynamischen oder statischen RPC-Port (wie konfiguriert).

Der DC-Agent für den Azure AD-Kennwortschutz lauscht nie an einem, für das Netzwerk verfügbaren Port und der Proxy-Dienst versucht nie, den DC-Agent-Dienst aufzurufen.

Der Azure AD-Kennwortschutz-Proxy-Dienst ist zustandslos und speichert keine Richtlinien oder keinen anderen, von Azure heruntergeladen Status zwischen.

Der DC-Agent-Dienst für den Azure AD-Kennwortschutz wertet das Kennwort eines Benutzers nur mit der neuesten lokal verfügbare Kennwortrichtlinie aus. Wenn keine Kennwortrichtlinie auf dem lokalen DC verfügbar ist, wird das Kennwort automatisch akzeptiert und eine Ereignisprotokollmeldung protokolliert, um den Administrator zu warnen.

Der Azure AD-Kennwortschutz ist keine Echtzeit-Engine für die Richtlinienanwendung. Zwischen einer Änderung der Kennwortrichtlinien-Konfiguration in Azure AD und dem Zeitpunkt, zu dem sie alle Domänencontroller erreicht und dort ihre Durchsetzung erzwungen wird, kann eine Verzögerung auftreten.

## <a name="foresttenant-binding-for-azure-ad-password-protection"></a>Gesamtstruktur-/Mandantenbindung für den Azure AD-Kennwortschutz

Die Bereitstellung von Azure AD- Kennwortschutz in einer Active Directory-Gesamtstruktur erfordert die Registrierung der Active Directory-Gesamtstruktur und aller bereitgestellten Azure AD-Kennwortschutz-Proxy-Dienste mit Azure AD. Beide Registrierungen (Gesamtstruktur und Proxy) sind einem bestimmten Azure AD Mandanten zugeordnet, der implizit über die bei der Registrierung verwendeten Anmeldeinformationen identifiziert wird. Jedes Mal, wenn eine Kennwortrichtlinie für den Azure AD-Kennwortschutz heruntergeladen wird, ist sie immer spezifisch für diesen Mandanten (d.h., diese Richtlinie wird immer eine Kombination aus der globalen Microsoft-Kennwortliste für gesperrte Kennwörter und der benutzerdefinierten Kennwörterliste für jeden Mandanten sein). Es wird nicht unterstützt, verschiedene Domänen oder Proxys in einer Gesamtstruktur zu konfigurieren, um an verschiedene Azure AD-Mandanten gebunden zu sein.

## <a name="license-requirements"></a>Lizenzanforderungen

Die Vorteile der Liste global gesperrter Kennwörter gelten für alle Benutzer von Azure Active Directory (Azure AD).

Die Liste benutzerdefinierter gesperrter Kennwörter erfordert Azure AD Basic-Lizenzen.

Azure AD-Kennwortschutz für Windows Server Active Directory erfordert Azure AD Premium-Lizenzen.

Weitere Informationen zur Lizenzierung einschließlich der Kosten finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Download

Die zwei erforderlichen Agent-Installer sind für den Azure AD-Kennwortschutz erforderlich, die vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) heruntergeladen werden können.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises-deploy.md)
