---
title: Featurebasierter Vergleich der Azure API Management-Tarife | Microsoft-Dokumentation
description: In diesem Artikel werden die API Management-Tarife anhand der angebotenen Features verglichen.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: apimpm
ms.openlocfilehash: eae36aa6e60e3da03c59952a1d9e035e6a773d2d
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156696"
---
# <a name="feature-based-comparison-of-the-azure-api-management-tiers"></a>Featurebasierter Vergleich der Azure API Management-Tarife

Jeder API Management-[Tarif](https://aka.ms/apimpricing) bietet einen bestimmten Satz von Features und [Kapazität pro Einheit](api-management-capacity.md). In der folgenden Tabelle werden die wichtigsten Features, die in den einzelnen Tarifen verfügbar sind, zusammengefasst. Einige Features funktionieren je nach Tarif möglicherweise anders oder weisen unterschiedliche Funktionen auf. In solchen Fällen werden die Unterschiede in den Dokumentationsartikel zu diesen einzelnen Features hervorgehoben.

| Feature                                                                                      | Consumption<sup>VORSCHAU</sup> | Developer      | Basic          | Standard       | Premium        |
| -------------------------------------------------------------------------------------------- | ----------------------------- | -------------- | -------------- | -------------- | -------------- |
| Azure AD-Integration<sup>1</sup>                                                             | Nein                             | Ja            | Nein             | Ja            | JA            |
| Unterstützung von virtuellen Netzwerken (VNETs)                                                               | Nein                             | Ja            | Nein              | Nein              | JA            |
| Bereitstellung in mehreren Regionen                                                                      | Nein                             | Nein              | Nein              | Nein              | JA            |
| Mehrere benutzerdefinierte Domänennamen                                                                 | Nein                             | Nein              | Nein              | Nein              | JA            |
| Entwicklerportal<sup>2</sup>                                                                 | Nein                             | Ja            | Ja            | Ja            | JA            |
| Integrierter Cache                                                                               | Nein                             | Ja            | Ja            | Ja            | JA            |
| Integrierte Analysen                                                                           | Nein                             | Ja            | Ja            | Ja            | JA            |
| [SSL-Einstellungen](api-management-howto-manage-protocols-ciphers.md)                             | Nein                             | Ja            | Ja            | Ja            | JA            |
| [Externer Cache](https://aka.ms/apimbyoc)                                                    | JA                           | Nein<sup>3</sup> | Nein<sup>3</sup> | Nein<sup>3</sup> | Nein<sup>3</sup> |
| [Clientzertifikatsauthentifizierung](api-management-howto-mutual-certificates-for-clients.md) | Nein<sup>4</sup>                | JA            | Ja            | Ja            | JA            |
| [Sichern und Wiederherstellen](api-management-howto-disaster-recovery-backup-restore.md)               | Nein                             | Ja            | Ja            | Ja            | JA            |
| [Verwaltung über Git](api-management-configuration-repository-git.md)                        | Nein                             | Ja            | Ja            | Ja            | JA            |
| Direkte Verwaltungs-API                                                                        | Nein                             | Ja            | Ja            | Ja            | JA            |
| Azure Monitor-Protokolle und -Metriken                                                               | Nein<sup>5</sup>                | JA            | Ja            | Ja            | JA            |

<sup>1</sup> Ermöglicht die Verwendung von Azure AD (und Azure AD B2C) als Identitätsanbieter für die Benutzeranmeldung im Entwicklerportal.<br/>
<sup>2</sup> Einschließlich zugehöriger Funktionen, z.B. Benutzer, Gruppen, Probleme, Anwendungs- und E-Mail-Vorlagen und Benachrichtigungen.<br/>
<sup>3</sup> Eine Unterstützung von externem Cache für diesen Tarif wird bald verfügbar sein.<br/>
<sup>4</sup> Clientzertifikatauthentifizierung wird dem Tarif „Consumption“ vor der allgemeinen Verfügbarkeit hinzugefügt.<br/>
<sup>5</sup> Vollständige Unterstützung von Azure Monitor wird dem Tarif „Consumption“ hinzugefügt.
