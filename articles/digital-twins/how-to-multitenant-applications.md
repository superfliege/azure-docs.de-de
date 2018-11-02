---
title: Aktivieren von mehrinstanzenfähigen Anwendungen für Azure Digital Twins | Microsoft-Dokumentation
description: Grundlegendes zum Registrieren von Azure Active Directory-Mandanten Ihrer Kunden für Azure Digital Twins
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: e465ab95b670268f8ef31472259783fce8f24b36
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323887"
---
# <a name="how-to-enable-multitenant-applications-with-azure-digital-twins"></a>Aktivieren von mehrinstanzenfähigen Anwendungen für Azure Digital Twins

Es ist wahrscheinlich, dass Entwickler, die die Azure Digital Twins-Plattform nutzen, mehrinstanzenfähige Anwendungen erstellen möchten. Eine *mehrinstanzenfähige Anwendung* ist eine einzelne bereitgestellte Instanz, in der mehrere Kunden unterstützt werden, wobei jeder Kunde seine eigenen unabhängigen Daten und Berechtigungen hat.

In diesem Dokument ist beschrieben, wie eine mehrinstanzenfähigen Azure Digital Twins-App erstellt wird, in der mehrere AD-Mandanten (Azure Active Directory) und Kunden unterstützt werden.

## <a name="scenario-summary"></a>Szenariozusammenfassung

Nehmen Sie für dieses Szenario an, es gibt den Entwickler D und den Kunden C:

- Entwickler D hat ein Azure-Abonnement mit einem Azure AD-Mandanten.
- Entwickler D hat eine Digital Twins-Instanz in seinem Azure-Abonnement bereitgestellt.
- Benutzer im Azure AD-Mandanten des Entwicklers D können Token für den Digital Twins-Dienst abrufen, weil Azure AD einen Dienstprinzipal im Azure AD-Mandanten des Entwicklers D erstellt hat.
- Entwickler D erstellt jetzt eine mobile App, die direkt in die Verwaltungs-API von Digital Twins integriert ist.
- Danach gestattet Entwickler D dem Kunden C die Nutzung der mobilen App.
- Kunde C muss nun autorisiert werden, um die Verwaltungs-API von Digital Twins in der Anwendung von Entwickler D verwenden zu können.

  > [!IMPORTANT]
  > - Wenn sich Kunde C bei der App von Entwickler D anmeldet, kann die App keine Token für die Benutzer von Kunde C abrufen, um mit der Verwaltungs-API zu kommunizieren.
  > - Azure AD löst dann einen Fehler aus, der angibt, dass Digital Twins im Verzeichnis von Kunde C nicht erkannt wird.

## <a name="solution"></a>Lösung

Um dieses Szenario zu lösen, müssen die folgenden Aktionen ausgeführt werden, um einen Digital Twins-Dienstprinzipal im Azure AD-Mandanten des Kunden C zu erstellen:

- Wenn Kunde C noch kein Azure-Abonnement für den Azure AD-Mandanten hat:

  - Der Azure AD-Mandantenadministrator des Kunden C muss ein [Azure-Abonnement des Typs „Nutzungsbasierte Bezahlung“](https://azure.microsoft.com/offers/ms-azr-0003p/) erwerben.
  - Der Azure AD-Mandantenadministrator des Kunden C muss dann [den Mandanten mit dem neuen Abonnement verknüpfen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Im [Azure-Portal](https://portal.azure.com) führt der Azure AD-Mandantenadministrator des Kunden C dann folgende Schritte aus:
  1. Er öffnet **Abonnements**.
  1. Er wählt das Abonnement aus, das den Azure AD-Mandanten umfasst, der in der App des Entwicklers D verwendet werden soll.
  1. Er wählt **Ressourcenanbieter** aus.
  1. Er sucht nach **Microsoft.IoTSpaces**.
  1. Klicken Sie auf **Registrieren**.
  
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von benutzerdefinierten Funktionen mit Azure Digital Twins finden Sie unter [Verwenden von benutzerdefinierten Funktionen in Azure Digital Twins](how-to-user-defined-functions.md).

Informationen dazu, wie rollenbasierte Zugriffssteuerung verwendet werden kann, um die Anwendung zusätzlich mit Rollenzuweisungen zu schützen, finden Sie unter [Erstellen und Verwalten von Rollenzuweisungen](security-create-manage-role-assignments.md).
