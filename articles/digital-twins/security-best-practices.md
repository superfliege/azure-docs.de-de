---
title: Grundlegendes zu den bewährten Methoden für Azure Digital Twins-Sicherheit | Microsoft-Dokumentation
description: Bewährte Methoden für Azure Digital Twins-Sicherheit
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: 6ca01523744dbce15f8fdb3bbe2d5a9b44510f3f
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959476"
---
# <a name="security-best-practices"></a>Bewährte Methoden für die Sicherheit

Azure Digital Twins-Sicherheit ermöglicht präzisen Zugriff auf bestimmte Ressourcen und Aktionen in Ihrem IoT-Graphen. Dies erfolgt mithilfe einer detaillierten Rollen- und Rechteverwaltung namens „Rollenbasierte Zugriffssteuerung“.

Azure Digital Twins verwendet außerdem weitere Sicherheitsfunktionen, die für Azure IoT verfügbar sind, z.B. Azure Active Directory (Azure AD). Aus diesem Grund beinhaltet ein Konfigurieren Ihrer Azure Digital Twins-App das Verwenden vieler derselben [Sicherheitsmaßnahmen für Azure IoT](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/), die derzeit empfohlen werden.

In diesem Artikel sind die wichtigsten bewährten Methoden zusammengefasst, die Sie befolgen sollten.

> [!IMPORTANT]
> Überprüfen Sie zusätzliche Sicherheitsressourcen, um die maximale Sicherheit für Ihren IoT-Raum zu gewährleisten. Achten Sie darauf, Ihre Gerätehersteller zu berücksichtigen.

## <a name="iot-security-best-practices"></a>Bewährte Methoden für die IoT-Sicherheit

Einige der wichtigsten Methoden zum sicheren Schutz Ihrer IoT-Geräte sind:

> [!div class="checklist"]
> * Schützen Sie jedes Gerät, das mit Ihrem IoT-Raum verbunden ist, auf eine manipulationssichere Weise.
> * Begrenzen Sie die Rolle jedes Geräts, jedes Sensors und jeder Person in Ihrem IoT-Raum. Bei einer Kompromittierung werden die Auswirkungen minimiert.
> * Verwenden Sie ggf. Filterung der Geräte-IP-Adressen und Einschränkungen der Ports.
> * Begrenzen Sie E/A- und Gerätebandbreite, um die Leistung zu verbessern. Begrenzung der Übertragungsrate kann die Sicherheit verbessern, indem Denial-of-Service-Angriffe verhindert werden.
> * Halten Sie die Gerätefirmware auf dem neuesten Stand.

Einige der wichtigsten Methoden zum sicheren Schutz eines IoT-Raums sind:

> [!div class="checklist"]
> * Verschlüsseln Sie gespeicherte oder persistente Daten.
> * Erfordern Sie, dass Kennwörter oder Schlüssel in regelmäßigen Abständen geändert oder aktualisiert werden.
> * Schränken Sie den Zugriff und die Berechtigungen sorgfältig nach Rollen ein. Weitere Informationen finden Sie im Abschnitt „Rollenbasierte Zugriffssteuerung – bewährte Methoden“.
> * Verwenden Sie leistungsstarke Verschlüsselung. Verlangen Sie lange Kennwörter, und verwenden Sie sichere Protokolle sowie die zweistufige Authentifizierung.

Überwachen Sie IoT-Ressourcen, und achten Sie auf Ausreißer, Bedrohungen und Ressourcenparameter, die außerhalb des üblichen Betriebsbereichs liegen. Verwalten Sie die Überwachung mit Azure Analytics.

> [!NOTE]
> Weitere Informationen zum Verarbeiten und Überwachen von Ereignissen finden Sie unter [Weiterleiten von Ereignissen und Nachrichten mit Azure Digital Twins](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Azure Active Directory – bewährte Methoden

Azure Digital Twins verwendet Azure AD, um Benutzer zu authentifizieren und Anwendungen zu schützen. Azure AD unterstützt die Authentifizierung für eine Vielzahl moderner Architekturen. Diese basieren alle auf branchenüblichen Protokollen wie OAuth 2.0 oder OpenID Connect. Einige wichtige Methoden zum Schützen Ihres IoT-Raums für Azure AD sind:

> [!div class="checklist"]
> * Speichern Sie Geheimnisse von Azure AD-Apps an einem sicheren Ort wie [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Verwenden Sie zum Authentifizieren anstelle des App-Geheimnisses ein Zertifikat, das von einer vertrauenswürdigen [Zertifizierungsstelle](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) ausgestellt wurde.
> * Begrenzen Sie den OAuth 2.0-Zugriffsbereich für ein Token.
> * Überprüfen Sie, wie lange ein Token gültig ist und ob ein Token gültig bleibt.
> * Legen Sie entsprechende Zeitspannen fest, während denen Token gültig sind.
> * Aktualisieren Sie abgelaufene Token.

## <a name="role-based-access-control-best-practices"></a>Rollenbasierte Zugriffssteuerung – bewährte Methoden

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu bewährten Methoden für Azure IoT finden Sie unter [Bewährte Methoden für die IoT-Sicherheit](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Weitere Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung](./security-role-based-access-control.md).

Wenn Sie mehr über das Authentifizieren erfahren möchten, lesen Sie [Authentifizieren mit APIs](./security-authenticating-apis.md).
