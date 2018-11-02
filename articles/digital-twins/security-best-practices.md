---
title: Grundlegendes zu den bewährten Methoden für Azure Digital Twins-Sicherheit | Microsoft-Dokumentation
description: Bewährte Methoden für Azure Digital Twins-Sicherheit
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: adgera
ms.openlocfilehash: 28eb8b5dc0f75b5e031070803d35c8a1ceb1f000
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364339"
---
# <a name="security-best-practices"></a>Bewährte Methoden für die Sicherheit

Azure Digital Twins-Sicherheit ermöglicht präzisen Zugriff auf bestimmte Ressourcen und Aktionen in Ihrem IoT-Graphen. Dies geschieht durch eine granulare Rollen- und Berechtigungsverwaltung namens „Rollenbasierte Zugriffssteuerung“.

Azure Digital Twins nutzt außerdem weitere Sicherheitsfunktionen, die für Azure IoT verfügbar sind, z. B. Azure Active Directory. Aus diesem Grund beinhaltet ein Konfigurieren Ihrer Azure Digital Twins-App das Verwenden vieler derselben [Sicherheitsmaßnahmen für Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/), die derzeit empfohlen werden.

In diesem Artikel sind die wichtigsten bewährten Methoden zusammengefasst, die Sie befolgen sollten.

> [!IMPORTANT]
> Überprüfen Sie zusätzliche Sicherheitsressourcen (einschließlich der Lieferanten Ihrer Geräte), um maximale Sicherheit für Ihren IoT-Raum sicherzustellen.

## <a name="iot-security-best-practices"></a>Bewährte Methoden für die IoT-Sicherheit

Einige der wichtigsten Methoden zum sicheren Schutz Ihrer IoT-Geräte sind:

> [!div class="checklist"]
> * Schützen Sie jedes Gerät, das mit Ihrem IoT-Raum verbunden ist, auf eine manipulationssichere Weise.
> * Begrenzen Sie die Rolle jedes Geräts, jedes Sensors und jeder Person in Ihrem IoT-Raum. Bei einer Kompromittierung sind die Auswirkungen minimiert.
> * Verwenden Sie ggf. Filterung der Geräte-IP-Adressen.
> * Begrenzen Sie E/A- und Gerätebandbreite, um die Leistung zu verbessern. Begrenzung der Übertragungsrate kann die Sicherheit verbessern, indem Denial-of-Service-Angriffe verhindert werden.

Einige der wichtigsten Methoden zum sicheren Schutz eines IoT-Raums sind:

> [!div class="checklist"]
> * Verschlüsseln Sie gespeicherte oder persistente Daten.
> * Erfordern Sie, dass Kennwörter oder Schlüssel in regelmäßigen Abständen geändert oder aktualisiert werden.
> * Schränken Sie den Zugriff und die Berechtigungen sorgfältig nach Rollen ein (siehe „Rollenbasierte Zugriffssteuerung – bewährte Methoden“ weiter unten).
> * Verwenden Sie leistungsstarke Verschlüsselung. Das bedeutet, es werden lange Kennwörter gefordert und sichere Protokolle sowie zweistufige Authentifizierung verwendet.

Die Überwachung von IoT-Ressourcen, um auf Ausreißer, Bedrohungen oder Ressourcenparameter zu prüfen, die außerhalb des üblichen Betriebsbereichs liegen, wird über Azure Analytics verwaltet.

> [!NOTE]
> Weitere Informationen zur Ereignisverarbeitung und -überwachung finden Sie im Artikel auf [Weiterleiten von Ereignissen und Nachrichten](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Azure Active Directory – bewährte Methoden

Azure Digital Twins verwendet Azure Active Directory, um Benutzer zu authentifizieren und Anwendungen zu schützen. Azure Active Directory unterstützt die Authentifizierung für eine Vielzahl moderner Architekturen, die alle auf den branchenüblichen Standardprotokollen basieren, z. B. OAuth 2.0 oder OpenID Connect. Einige wichtige Methoden zum Schützen Ihres IoT-Raums für Azure Active Directory sind:

> [!div class="checklist"]
> * Speichern Sie geheime Schlüssel von Azure Active Directory-Apps an einem sicheren Ort, z. B. [Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Verwenden Sie zum Authentifizieren anstelle der geheimen App-Schlüssel ein Zertifikat, das von einer vertrauenswürdigen [Zertifizierungsstelle](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) ausgegeben wurde.
> * Begrenzen Sie den OAuth 2.0-Zugriffsbereich für ein Token.
> * Überprüfen Sie, wie lange ein Token gültig ist und ob ein Token gültig bleibt.
> * Legen Sie entsprechende Zeitspannen fest, während denen Token gültig sind.
> * Aktualisieren Sie abgelaufene Token.

## <a name="role-based-access-control-best-practices"></a>Rollenbasierte Zugriffssteuerung – bewährte Methoden

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu bewährten Methoden für Azure IoT finden Sie unter [Bewährte Methoden für die IoT-Sicherheit](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung](./security-role-based-access-control.md).

Informationen über Authentifizierung finden Sie unter [Authentifizieren mit APIs](./security-authenticating-apis.md).
