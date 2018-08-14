---
title: Festlegen der Zugriffssteuerung in Azure Cosmos DB | Microsoft Docs
description: Erfahren Sie, wie die Zugriffssteuerung in Azure Cosmos DB festgelegt wird.
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: rafats
ms.openlocfilehash: 0d4595bcf8a9f009dce928d3f3cbc442328ec39b
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40038112"
---
# <a name="access-control-in-azure-cosmos-db"></a>Zugriffssteuerung in Azure Cosmos DB

Um Ihrem Benutzerkonto Azure Cosmos DB-Kontoleserzugriff hinzuzufügen, lassen Sie den Besitzer eines Abonnements die folgenden Schritte im Azure-Portal ausführen.

1. Öffnen Sie das Azure-Portal, und wählen Sie Ihr Azure Cosmos DB-Konto aus.
2. Klicken Sie auf die Registerkarte **Zugriffssteuerung (IAM)** aus, und klicken Sie dann auf **Hinzufügen**.
3. Wählen Sie im Bereich **Berechtigungen hinzufügen** im Feld **Rolle** die Option **Cosmos DB-Rolle "Kontoleser"** aus.
4. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus.
5. Wählen Sie den Benutzer, die Gruppe oder die Anwendung aus, dem oder der Sie Zugriff gewähren möchten.  Sie können das Verzeichnis nach Anzeigename, E-Mail-Adresse oder Objektbezeichner durchsuchen.
    Der ausgewählten Benutzer, die Gruppe oder die Anwendung wird in der Liste der ausgewählten Elemente angezeigt.
6. Klicken Sie auf **Speichern**.

Die Entität kann jetzt Azure Cosmos DB-Ressourcen lesen.
