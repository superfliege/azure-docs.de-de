---
title: Aktivieren von mehrinstanzenfähigen Anwendungen mit Azure Digital Twins | Microsoft-Dokumentation
description: Grundlegendes zum Registrieren von Azure Active Directory-Mandanten Ihrer Kunden für Azure Digital Twins
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: a2d9ece119003c341f49ee03d735d5636b179a32
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259886"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Aktivieren von mehrinstanzenfähigen Anwendungen mit Azure Digital Twins

Entwickler, die Azure Digital Twins nutzen, möchten im Allgemeinen mehrinstanzenfähige Anwendungen erstellen. Eine *mehrinstanzenfähige Anwendung* ist eine einzelne bereitgestellte Instanz, die mehrere Kunden unterstützt. Jeder Kunde verfügt über eigene unabhängige Daten und Berechtigungen.

In diesem Dokument wird beschrieben, wie Sie eine mehrinstanzenfähigen Azure Digital Twins-App erstellen, in der mehrere Azure AD-Mandanten (Azure Active Directory) und Kunden unterstützt werden.

## <a name="scenario-summary"></a>Szenariozusammenfassung

Nehmen Sie für dieses Szenario an, es gibt den Entwickler D und den Kunden C:

- Entwickler D hat ein Azure-Abonnement mit einem Azure AD-Mandanten.
- Entwickler D hat eine Digital Twins-Instanz in seinem Azure-Abonnement bereitgestellt.
- Benutzer im Azure AD-Mandanten von Entwickler D können Token für den Azure Digital Twins-Dienst abrufen, da Azure AD einen Dienstprinzipal im Azure AD-Mandanten von Entwickler D erstellt hat.
- Entwickler D erstellt jetzt eine mobile App, die direkt in die Verwaltungs-API von Azure Digital Twins integriert ist.
- Entwickler D gestattet dem Kunden C die Nutzung der mobilen App.
- Kunde C muss autorisiert werden, um die Verwaltungs-API von Azure Digital Twins in der Anwendung von Entwickler D verwenden zu können.

  > [!IMPORTANT]
  > - Wenn sich Kunde C bei der App von Entwickler D anmeldet, kann die App keine Token für die Benutzer von Kunde C abrufen, um mit der Verwaltungs-API zu kommunizieren.
  > - Azure AD löst dann einen Fehler aus, der angibt, dass Azure Digital Twins im Verzeichnis von Kunde C nicht erkannt wird.

## <a name="solution"></a>Lösung

Um das vorgenannte Szenario zu lösen, müssen die folgenden Aktionen ausgeführt werden, um einen Azure Digital Twins-Dienstprinzipal im Azure AD-Mandanten von Kunde C zu erstellen:

- Wenn Kunde C noch kein Azure-Abonnement für den Azure AD-Mandanten hat:

  - Der Azure AD-Mandantenadministrator von Kunde C muss ein [Azure-Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) erwerben.
  - Der Azure AD-Mandantenadministrator von Kunde C muss dann [den Mandanten mit dem neuen Abonnement verknüpfen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Im [Azure-Portal](https://portal.azure.com) führt der Azure AD-Mandantenadministrator von Kunde C folgende Schritte aus:

  1. Er öffnet **Abonnements**.
  1. Er wählt das Abonnement aus, das den Azure AD-Mandanten umfasst, der in der App des Entwicklers D verwendet werden soll.
  1. Er wählt **Ressourcenanbieter** aus.
  1. Er sucht nach **Microsoft.IoTSpaces**.
  1. Wählen Sie **Registrieren**.
  
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von benutzerdefinierten Funktionen mit Azure Digital Twins finden Sie unter [Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](how-to-user-defined-functions.md).

Informationen dazu, wie die rollenbasierte Zugriffssteuerung verwendet werden kann, um Anwendung zusätzlich mit Rollenzuweisungen zu schützen, finden Sie unter [Rollenbasierte Zugriffssteuerung mit Azure Digital Twins](security-create-manage-role-assignments.md).
