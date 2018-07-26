---
title: Kennwortschutz für Azure AD – Vorschauversion
description: Sperren unsicherer Kennwörter im lokalen Active Directory mit der Vorschau des Azure AD-Kennwortschutzes
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 5d4062ff2c21deeb56eb4ae6e8386452608f85fa
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158803"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Vorschau: Erzwingen des Azure AD-Kennwortschutzes für Windows Server Active Directory

|     |
| --- |
| Azure AD-Kennwortschutz und die benutzerdefinierte Liste der gesperrten Kennwörter sind in der öffentlichen Vorschau befindliche Features von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Azure AD-Kennwortschutz ist ein neues, von Azure Active Directory (Azure AD) unterstütztes Feature in der öffentlichen Vorschau, um Kennwortrichtlinien in einer Organisation zu verbessern. Die lokale Bereitstellung von Azure AD-Kennwortschutz verwendet sowohl die Liste globaler als auch benutzerdefinierter gesperrter Kennwörter, die in Azure AD gespeichert sind, und führt die gleichen Überprüfungen lokal wie bei cloudbasierten Azure AD-Änderungen durch.

Es gibt drei Softwarekomponenten, die den Azure AD-Kennwortschutz bilden:

* Der Azure AD-Kennwortschutz-Proxydienst wird auf einem beliebigen in die Domäne eingebundenen Computer in der aktuellen Active Directory-Gesamtstruktur ausgeführt. Er leitet Anforderungen von Domänencontrollern an Azure AD weiter und gibt die Antwort von Azure AD an den Domänencontroller zurück.
* Der Azure AD-Kennwortschutz-DC-Agent-Dienst empfängt Anforderungen zur Kennwortüberprüfung von der DC-Agent-Kennwortfilter-DLL, verarbeitet sie mit der aktuellen lokal verfügbaren Kennwortrichtlinie, und gibt das Ergebnis zurück (bestanden\nicht bestanden). Dieser Dienst ist für das regelmäßige (stündliche) Aufrufen des Azure AD-Kennwortschutz-Proxydiensts zum Abrufen neuer Versionen der Kennwortrichtlinie verantwortlich. Die Kommunikation für Aufrufe, die beim Azure AD-Kennwortschutz-Proxydienst ein- und davon ausgehen, wird per Remoteprozeduraufruf (Remote Procedure Call, RPC) über TCP abgewickelt. Beim Abruf werden neue Richtlinien in einem SYSVOL-Ordner gespeichert, in dem sie an andere Domänencontroller replizieren können. Der DC-Agent-Dienst überwacht auch den SYSVOL-Ordner auf Änderungen – für den Fall, dass andere Domänencontroller neue Kennwortrichtlinien dorthin geschrieben haben. Wenn bereits eine passende letzte Richtlinie verfügbar ist, wird die Überprüfung des Azure AD-Kennwortschutz-Proxydiensts übersprungen.
* Die DC-Agent-Kennwortfilter-DLL empfängt Anforderungen zur Kennwortüberprüfung vom Betriebssystem und leitet sie an den Azure AD-Kennwortschutz-DC-Agent-Dienst weiter, der lokal auf dem Domänencontroller ausgeführt wird.

![Zusammenspiel der Komponenten des Azure AD-Kennwortschutzes](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

## <a name="requirements"></a>Requirements (Anforderungen)

* Alle Computer, auf denen Azure AD-Kennwortschutzkomponenten einschließlich Domänencontrollern installiert sind, müssen Windows Server 2012 oder höher ausführen.
* Netzwerkkonnektivität muss zwischen mindestens einem Domänencontroller in jeder Domäne und mindestens einem Server bestehen, der den Azure AD-Kennwortschutz-Proxydienst hostet.
* Alle Active Directory-Domänen, die die DC-Agent-Dienst-Software ausführen, müssen DFSR für die SYSVOL-Replikation verwenden.
* Ein globales Administratorkonto zum Registrieren des Azure AD-Kennwortschutz-Proxydienstes bei Azure AD.
* Ein Konto mit Administratorrechten der Active Directory-Domäne in der Gesamtstruktur-Stammdomäne.

### <a name="license-requirements"></a>Lizenzanforderungen

Die Vorteile der Liste global gesperrter Kennwörter gelten für alle Benutzer von Azure Active Directory (Azure AD).

Die Liste benutzerdefinierter gesperrter Kennwörter erfordert Azure AD Basic-Lizenzen.

Azure AD-Kennwortschutz für Windows Server Active Directory erfordert Azure AD Premium-Lizenzen. 

Weitere Informationen zur Lizenzierung einschließlich der Kosten finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Download

Es gibt zwei erforderliche Installer für den Azure AD-Kennwortschutz, die vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) heruntergeladen werden können.

## <a name="answers-to-common-questions"></a>Antworten auf häufig gestellte Fragen

* Es ist keine Internetkonnektivität der Domänencontroller erforderlich. Nur die Computer, auf denen der Azure AD-Kennwortschutz-Proxydienst ausgeführt wird, benötigen Internetkonnektivität.
* Auf Domänencontrollern sind keine Netzwerkports geöffnet.
* Es sind keine Änderungen des Active Directory-Schemas erforderlich.
   * Die Software verwendet die vorhandenen Active Directory-Container- und serviceConnectionPoint-Schemaobjekte.
* Es besteht keine minimale Anforderung an die Funktionsebene von Active Directory-Domäne oder Gesamtstruktur (DFL\FFL).
* Die Software erstellt weder Konten in den Active Directory-Domänen, die sie schützt, noch setzt sie sie voraus.
* Inkrementelle Bereitstellung wird mit dem Nachteil unterstützt, dass die Kennwortrichtlinie nur dort erzwungen wird, wo der Domänencontroller-Agent installiert ist.
* Azure AD-Kennwortschutz ist kein Echtzeit-Richtlinienanwendungs-Modul. Zwischen einer Änderung der Kennwortrichtlinien-Konfiguration und dem Zeitpunkt, zu dem sie alle Domänencontroller erreicht und dort ihre Durchsetzung erzwungen wird, kann eine Verzögerung auftreten.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des Kennwortschutzes für Azure AD](howto-password-ban-bad-on-premises.md)