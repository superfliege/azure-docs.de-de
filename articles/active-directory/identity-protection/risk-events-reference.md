---
title: Referenz für Risikoereignisse in Azure Active Directory Identity Protection | Microsoft-Dokumentation
description: Referenz für Risikoereignisse in Azure Active Directory Identity Protection.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: d8daa1747323abe8115e2b1b06db906a48199426
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692646"
---
# <a name="azure-active-directory-identity-protection-risk-events-reference"></a>Referenz für Risikoereignisse in Azure Active Directory Identity Protection

Die allermeisten Sicherheitsverletzungen kommen vor, wenn Angreifer Zugriff auf eine Umgebung erhalten, indem sie die Identität eines Benutzers stehlen. Die Ermittlung von kompromittierten Identitäten ist keine einfache Aufgabe. Azure Active Directory verwendet adaptive Machine Learning-Algorithmen und -Heuristiken, um verdächtige Aktivitäten im Zusammenhang mit Ihren Benutzerkonten zu erkennen. Jede erkannte verdächtige Aktion wird in einem Datensatz gespeichert, der als Risikoereignis bezeichnet wird.


## <a name="anonymous-ip-address"></a>Anonyme IP-Adresse

**Erkennungstyp**: Echtzeit  
**Alter Name**: Anmeldungen von einer anonymen IP-Adresse


Diese Art von Risikoereignis gibt Anmeldungen über eine anonyme IP-Adresse (z. B. Tor-Browser, anonymisierte VPNs) an.
Diese IP-Adressen werden in der Regel von Akteuren verwendet, die ihre Anmeldungstelemetriedaten (IP-Adresse, Standort, Gerät usw.) für potenziell böswillige Absichten verbergen möchten.


## <a name="atypical-travel"></a>Ungewöhnlicher Ortswechsel

**Erkennungstyp**: Offline  
**Alter Name**: Unmöglicher Ortswechsel zu atypischen Orten


Mit dieser Art von Risikoereignis werden zwei Anmeldungen identifiziert, die von weit entfernten Orten durchgeführt wurden und bei denen mindestens einer der Orte aufgrund des bisherigen Verhaltens atypisch für den Benutzer ist. Neben zahlreichen anderen Faktoren berücksichtigt dieser Machine Learning-Algorithmus die Zeit zwischen den beiden Anmeldungen sowie die Zeit, die der Benutzer für einen Ortswechsel benötigen würde, wovon sich ableiten lässt, dass ein anderer Benutzer die gleichen Anmeldeinformationen verwendet.

Bei diesem Algorithmus werden offensichtliche falsch positive Ergebnisse ignoriert, die zu den unmöglichen Ortswechselbedingungen beitragen. Hierzu zählen etwa VPNs und regelmäßig von anderen Benutzern der Organisation verwendete Standorte. Das System verfügt über einen anfänglichen Lernzeitraum von 14 Tagen oder 10 Anmeldungen (je nachdem, welcher Punkt früher erreicht wird), in dem das Anmeldeverhalten des neuen Benutzers erlernt wird.


## <a name="leaked-credentials"></a>Kompromittierte Anmeldeinformationen

**Erkennungstyp**: Offline  
**Alter Name**: Benutzer mit kompromittierten Anmeldeinformationen


Diese Art von Risikoereignis gibt an, dass die gültigen Anmeldeinformationen des Benutzers offengelegt wurden.
Wenn Internetkriminelle an gültige Kennwörter von berechtigten Benutzern gelangen, geben sie diese Anmeldeinformationen häufig weiter. Dies erfolgt in der Regel durch eine Veröffentlichung im Darknet oder auf Paste Sites oder durch den Handel bzw. Verkauf der Anmeldeinformationen auf dem Schwarzmarkt. Der Microsoft-Dienst für offengelegte Anmeldeinformationen sammelt Kombinationen aus Benutzername und Kennwort durch die Überwachung von öffentlichen und Dark Web-Websites und die Zusammenarbeit mit:

- Forschern

- Strafverfolgungsbehörden

- Sicherheitsteams bei Microsoft

- Anderen vertrauenswürdigen Quellen

Wenn der Dienst Benutzeranmeldeinformationen aus dem Darknet, von Paste Sites oder den oben aufgeführten Quellen erwirbt, werden sie mit aktuell gültigen Anmeldeinformationen von Azure AD-Benutzern verglichen, um gültige Übereinstimmungen zu finden.


## <a name="malware-linked-ip-address"></a>Mit Schadsoftware verknüpfte IP-Adresse

**Erkennungstyp**: Offline  
**Alter Name**: Anmeldungen von infizierten Geräten


Bei dieser Art von Risikoereignis werden Anmeldungen von IP-Adressen identifiziert, die mit Schadsoftware infiziert sind und bekanntermaßen aktiv mit einem Botserver kommunizieren. Dies wird ermittelt, indem IP-Adressen des Benutzergeräts mit IP-Adressen korreliert werden, die in Kontakt mit einem Botserver gestanden haben, während der Botserver aktiv war.


## <a name="unfamiliar-sign-in-properties"></a>Ungewöhnliche Anmeldeeigenschaften

**Erkennungstyp**: Echtzeit  
**Alter Name**: Anmeldungen von unbekannten Standorten

Diese Art von Risikoereignis berücksichtigt die bisherigen Anmeldeeigenschaften (z. B. Gerät, Standort, Netzwerk), um Anmeldungen mit unbekannten Eigenschaften zu ermitteln. Das System speichert Eigenschaften der vorherigen Standorte, die von einem Benutzer verwendet wurden, und betrachtet diese als „bekannt“. Das Risikoereignis wird ausgelöst, wenn die Anmeldung mit Eigenschaften erfolgt, die in der Liste der bekannten Eigenschaften nicht enthalten sind. Das System benötigt einen anfänglichen Lernzeitraum von 30 Tagen, in dem Neuerkennungen nicht gekennzeichnet werden.
Diese Erkennung wird auch für die Standardauthentifizierung (bzw. ältere Protokolle) ausgeführt. Da diese Protokolle nicht über moderne Eigenschaften (z. B. Client-ID) verfügen, reichen die Telemetriedaten nicht aus, um falsch positive Ergebnisse zu reduzieren. Wir empfehlen unseren Kunden, auf eine moderne Authentifizierung umzusteigen.

